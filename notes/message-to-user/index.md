﻿1C:Enterprise has a convenient and accustomed method of showing information for a user — it's a UserMessage object. It outputs messages at the bottom of a form and suddenly it's a potential problem. The thing is, most users don't look at it. It's like: well, I see no critical errors for now so there is nothing to worry about.

Therefore, in case an error happens or you just have an essential message, you should show it through a dialogue window — by the ShowQueryBox() method or a ShowMessageBox() method, for example. Otherwise, a user may not notice a problem and continue to work in spite of some action might not be executed or might be executed in a wrong way. The issue will come out later and you will be rightfully blamed for it.

In addition, using of a UserMessage object should be prohibited in case of small service forms. Yes, in fact, it's hard to overlook a text if a window is small, but that's a different matter: the messages below literally [devour](en.png) form's workspace and it becomes hard to work with.