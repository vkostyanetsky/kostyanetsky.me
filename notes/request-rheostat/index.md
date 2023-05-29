I forgot to tell you that a couple of months ago I [posted](https://github.com/vkostyanetsky/RequestRheostat) on GitHub an example of code that limits the number of requests per second that can be sent to some third-party resource from a 1C:Enterprise infobase.

The problem is solved through a constant that stores the date for the current second and the number of requests that have already been sent. Clients who run into a limitation are waiting. The queue is not guaranteed, but is more or less respected.

It can be useful for managing the load on an external system. For example, the cloud-based Bitrix24 requires not to send requests to it more than twice per second, and if you exceed it, it bans you.