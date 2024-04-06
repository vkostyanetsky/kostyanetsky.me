Among our projects, we have one where two systems are communicating with each other: ERP and CRM. Data exchange is done well: a push'n'pull server has been set up, subscriptions to events have been registered, a REST API has been implemented, and so on. There are many other fascinating technical details, but I'm not talking about that now.

The exchange has various logic chains inside. For instance, if a new company appears in CRM, it sends the data to ERP. The other day, a problem appeared: a company was not sent from the CRM, no matter how many times you tried to write it. So we went to investigate, suspecting the worst: CRM is written in PHP (nothing personal; it’s just not our technical stack), and there’s a lot of different legacy stuff there. It's easier to shoot yourself in the foot than to blow your nose.

However, it didn't take much digging. We opened the company’s page in CRM and saw that he had the “Do Not Export To ERP” checkbox, which, in fact, blocked the sending. A manager made an obvious mistake.

Should we uncheck the box and close the ticket?

![Well yes, but actually no](actually.jpg)

This will solve the problem with that particular company, but not the reason it appeared. It is actually in the interface, specifically in the name of the option: “do not” is used, which is advisable to avoid due to the fact that it is more difficult for users to read the wording correctly. By the way, this also applies to a simple “do”.

It is often difficult for programmers to understand why this is so: we are used to instantly calculating Boolean expressions in our heads, and variations like “not (not true)” are commonplace for us. But people with a different background can get confused. Just a little, but sometimes this is enough for them to perceive “do not export” as “do export” in the heat of the day, click the option, and move on.

To sum up, the solution is to rename the checkbox. “Disable Export” or “Stop Export” are both fine, for example. “Prohibit Export” also comes to mind, but it’s more about interpersonal relationships, and in general, a ban on doing something does not mean that it won’t be done :)