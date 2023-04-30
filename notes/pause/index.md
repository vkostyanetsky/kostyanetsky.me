> An important detail: the CallPause method is not available in a client-server call; when a client calls a server method in which CallPause is called, the exception "Cannot call the CallPause method in a client-server call" will be thrown.
>
> [CallPause Method](https://wonderland.v8.1c.ru/blog/metod-vyzvatpauzu/) (RU)

What a strange restriction, to be frank. On the one hand, an experienced developer will not make an intentional pause in a client-server call anyway; on the other hand, whoever wants to make it makes it anyway (by checking the time in the cycle, for example). Does security by obscurity worth the efforts?
  
At best, some junior will catch this exception, think “welp, it looks like I'm doing something wrong” and move in the right direction. However, putting the restriction on the platform for the sake of this case is like firing a cannon at sparrows. Let's implement a number limit as well — like, no more than 1000 pauses per session, otherwise users will suddenly think that the program is too slow :-)