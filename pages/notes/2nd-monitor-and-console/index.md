I have two monitors. I use the second one during meetings in Zoom or when playing together with friends (in both cases it's convenient to bring the people's cameras there). There are more examples when it's useful, but often the device is simply idle. In order not to be distracted, I decided to turn it off.

What is the ambush here: it is inconvenient to enable or disable the second monitor via OS (a few clicks, and you need to scroll, and all the time you get confused where to go — in “Screen settings” or “Personalization”). I would like one command, and the command on a hotkey. And, ideally, from the script to rule all this.

I did not find the script, but I found a ready-made utility — MultiMonitorTool. It's free. Works under the Windows 10 without problems. The commands below turn on / off the 2nd monitor:

	MultiMonitorTool.exe / disable 2
	MultiMonitorTool.exe / enable 2
	MultiMonitorTool.exe / switch 2

For some reason, when you turn on a monitor via enable or switch, it's positioned incorrectly sometimes (for example, before it was turned off, it was on the right, and after turning it on, it was on the left). This is fixable. First, let's save the configuration at the moment when both monitors are on:

	MultiMonitorTool.exe / SaveConfig Monitors.cfg

And then, when you need to turn on the monitor, load the saved config:

	MultiMonitorTool.exe / LoadConfig Monitors.cfg

The utility can do a lot more (for example, one of the commands flips application windows between monitors). Follow the link above for the description.