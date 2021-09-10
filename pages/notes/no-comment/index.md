I did not notice when I gave up the habit of pedantically commenting on every method with which I have to work. This practice has some clever name for sure, but I don't care to be honest. I mean the style when a description is added to every meaningful block of code, like this:

    &AtServer
    Procedure BobDoSomething()

        // what the code below does, blah blah blah
        (lot of code)

        // what the code below does, blah blah blah
        (lot of another code)

        // what the code below does, blah blah blah
        (lot of code, once again)

    EndProcedure // BobDoSomething()

The meaning here is simple: when you dig in some dense legacy and run back and forth between chaotically scattered, big procedures - it is rather difficult to keep the logic of each of them in your head. The next cup of coffee will wash everything away. Therefore, such notes greatly speed up orientation on the ground, and the more time passes between approaches to the code, the more noticeable the effect.

However, at some point it became clear that this know-how is just a crutch supporting the frankly crappy code design. If you have a long procedure or function - take a breath, sit down and cut the fatty one into smaller methods. You will save both time and nerves, and you will figure out the code faster, and you will delight SonarCube.