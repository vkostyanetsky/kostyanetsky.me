This week, I developed a REST service to set up our service manager (this is a configuration for managing a 1cFresh instance). Deploying the development environment is a regular task for us, and every time the manager's database had to be tuned by hand: tweaking the storefront, changing application addresses, overwriting scheduled tasks, and so on.

The implementation was simple. Come up with a JSON structure, write a parser, find a code in the configuration, make it work by external call, and make sure you don't break anything. Routine work, in general, but I love to do such things from time to time: I mean, to look around and try to figure out which of the daily tasks is annoying enough.

This one is a good example. To be frank, setting up the Service Manager wasn't a problem (launching the app and fiddling with the settings), but it was a thing to pay attention & spend time to. What do we have now:

1. There is a JSON file with all the settings;
2. There is a REST service for its processing;
3. There is a script that will put the first into the second;
4. There is a pipeline that will do it all by itself.

In short, a boot was rubbing a leg, and now it isn't. Yahoo!