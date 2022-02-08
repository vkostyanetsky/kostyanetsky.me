I'll tell you about a funny and a little embarrassing case that I took apart in January. The essence in a nutshell: a huge auto-test based on Vanessa, which is intended to check the VAT calculation, falls somewhere near the end.

I start investigating. First, I look at the screenshots in Allure: OK, the reason is obvious – in one of the documents, the conditional appearance for the field with the VAT amount doesn't work. The test expected it to be unavailable if the VAT rate is zero, but it turned out to be available somehow.

It's a mess, it needs to be fixed! I look at the condition in the code: well, it locks the field if the VAT rate is in the list of “zero” rates (list of VAT rates whose rate is equal to zero). Everything appears to be simple and logical. What the hell could possibly go wrong here?

https://twitter.com/EffinBirds/status/1489980393675702281

Well, I try to reproduce the bug manually. And there, all of a sudden, everything is nice: the conditional appearance works as it should. Floating bug, or what? I run the auto-test again, at the right moment I jump in with the debugger and find some outright garbage: in the list of “zero” rates, besides themselves, there are a bunch of empty links!

Frankly speaking, I was scratching the back of my head. The document receives this list from the common module with [this code](https://gist.github.com/vkostyanetsky/5ec036ee148606aad9caefbc9305bfb0). An empty ref from here, even theoretically, cannot be obtained. Moreover, the module has the “Reuse Return Values” option ​​enabled, and the function is actually executes once somewhere at the beginning of the test, before all complex data manipulations. So, the test cannot affect it in any way, in theory.

Is it a dead end? Well, experienced colleagues have probably already guessed everything, but I had to dance around the bug  and even check the [standard](https://its.1c.ru/db/v8std/content/724/hdoc), until got the following: **return values ​​cache in 1C can be changed**. I mean, not by calling RefreshReusableValues​​(), but by changing reusable values directly.

How? Well, if you get some values ​​from a cached common module, and they are not of a primitive type (string, number, etc.), you will not get the value itself, but a pointer to it somewhere in memory. You write this pointer to a variable and try to change it – you change the cache.

It's that simple, yes. This is what happened in my case: the form of another document called the method that generates the list of “zero” rates. Having received a list of values, it added an empty ref to it and used it in its logic. Thus, each time this form opens, the list cache contains more and more empty refs, which eventually broke the document at the other end of the configuration.

https://twitter.com/EffinBirds/status/1488165946342662144

In a good way, the platform should throw exceptions when developer trying to change the cache, but until this happens, you have to take care yourself. For example, when developing cached modules, return immutable data types from them (FixedStructure instead of Structure, FixedArray instead of Array, and so on). True, this is not a 100% protection: firstly, fixed types are not applicable everywhere, and secondly, even in the latest versions of the SSL, this is far from being done everywhere. Do you know many configurations not based on SSL?

Sonar also know nothing about the problem, as well as less popular software. No silver bullet, in short – check your code, look at the code of your colleagues and try not to forget about another elegant way to bang ourselves in the foot.