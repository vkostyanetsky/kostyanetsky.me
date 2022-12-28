The other day, I was working with cookies in one of my scripts. While searching for the optimal solution, I came across an absolutely charming (100% working, by the way) [tip](https://stackoverflow.com/questions/13030095/how-to-save-requests-python-cookies-to-a-file) from StackOverflow:

> You can get a CookieJar object from the session with session.cookies, and use pickle to store it to a file.

So, literally: keep your cookies in a jar, and to store them, pickle them. The jar with pickled cookies, by the way, can be put on the shelf later.

How can you not love python after that, huh?

P.S. While writing this post, I got curious – why pickle and not serialization? So, briefly: [this is the way](https://stackoverflow.com/questions/27324986/pickles-why-are-they-called-that/27325007#27325007).