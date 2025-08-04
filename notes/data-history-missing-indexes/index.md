A colleague noticed that on one of our Fresh instances, deleting data areas had become painfully slow. Dug into the metrics:

<pre>
DELETE FROM T1
FROM _DataHistoryMetadata T1
WHERE 
    T1._MetadataId = ?
    AND T1._IsActual = 0x00 
    AND NOT (
        T1._MetadataVersionNumber IN (
            SELECT T2._MetadataVersionNumber AS MetadataVersionNumber_
            FROM _DataHistoryVersions T2
            WHERE T2._HistoryDataId IN (
                SELECT DataHistoryLatestVersions1.DataHistoryLatestVersions._HistoryDataId AS HistoryDataId_
                FROM DataHistoryLatestVersions1.DataHistoryLatestVersions T3
                WHERE DataHistoryLatestVersions1.DataHistoryLatestVersions._MetadataId = ?
            )
        )
    )
</pre>

Each of these queries was reading around 20GB. What's happening is mostly clear: the platform is trying to delete an area's data history, but the janky DB query causes a full scan over the whole history table across all areas instead of using an index. Someone on Dmitrovskaya got lazy again.

[![Why are you surpised?](why.png)](https://x.com/EffinBirds/status/1945545263407301033)

So we were losing between 30 seconds and a half an hour per operation. Wanted it faster. Fix:

<pre>
CREATE NONCLUSTERED INDEX IX_DataHistoryLatestVersions1_MetadataId
  ON dbo._DataHistoryLatestVersions1 (_MetadataId)
  INCLUDE (_HistoryDataId)
  WITH (DROP_EXISTING = OFF, ONLINE = OFF);
 
CREATE NONCLUSTERED INDEX IX_DataHistoryVersions_MetadataVersionNumber
  ON dbo._DataHistoryVersions (_MetadataVersionNumber)
  WITH (DROP_EXISTING = OFF, ONLINE = OFF);
</pre>

Deletion cost predictably dropped ~99%.

If you're going to repeat this on your side:

1. Technically this violates the license agreement, you've been warned and all that.
2. There's a risk the platform will trip over the new indexes during future restructurings (especially on the "new" schema). Better to have a ready script to drop the index, and then (after restructuring) put it back.