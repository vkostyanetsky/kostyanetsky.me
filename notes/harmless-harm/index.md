The other day my colleague and I were debugging an issue, nothing fancy, just another "why is this query behaving weird?" moment.

Simplified, the idea was: we read a table from the database and dump the result into a temp table. If a certain condition is met, we still want the temp table to be created, but it must be empty (regardless of whether the source table has rows or not).

The query looked roughly like this:

<pre>
SELECT
    Table.Field1 AS Field1
FROM
    Table AS Table
WHERE 
    &Parameter
</pre>

If we needed to copy rows from the source table into the temp table, we passed TRUE into the parameter; if we wanted the temp table to be empty, we passed FALSE.

Looks simple at first glance, but this trick is a silent performance foot-gun if the table being read is large.

The reason is how DB engines work with parameterized queries. Both MS SQL and PostgreSQL build the execution plan based on the query text, and in this example the parameter value does **not** affect the decision of whether the table should be read or not.

As a result, when this query runs, both engines will pedantically scan the whole table (or its index), even if the parameter is FALSE. In that case each row is just discarded, so the logic is technically correct, but we're wasting resources on a pointless scan and polluting the buffer cache, slowing down the system overall and diligently contributing to global warming :)

The fix is simple: inline TRUE/FALSE in the query body as a constant instead of using a parameter. Or use TOP so the query text is even simpler:

<pre>
SELECT TOP 0
    Table.Field1 AS Field1
FROM
    Table AS Table
</pre>

At the SQL level this turns into something like "SELECT TOP 0 ... FROM Table" for MS SQL and "SELECT ... FROM Table LIMIT 0" for PostgreSQL. The final plan will still contain a read operator, but the executor won't actually request any rows, so there's no real data scan (yay).

P.S. If you don't care about having proper column types in the temp table, you can even do this:

<pre>
SELECT TOP 0
    UNDEFINED AS Field1
</pre>

The performance gain, however, is so tiny that it's probably not worth over-optimizing for this. There are better hills to die on.