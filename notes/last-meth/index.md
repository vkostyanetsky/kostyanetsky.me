I'm digging into the code of an external component for 1C platform, published by its developers as an example. The good thing is: well, it can be compiled and if you tweak it a little — it does the job.

As for other things, there are a lot of bruh moments. For example, the project can't be opened in modern Visual Studio (you need to specify CMake manually). The code is quite sloppy; there is no documentation, comments, or formatting. Long story short: I believe, it can be difficult for a developer without solid experience in C++ to get the hang of this.

Was a bit amused by the naming in the code below:

    long CAddInNative::FindMethod(const WCHAR_T* wsMethodName)
    { 
        long plMethodNum = -1;
        wchar_t* name = 0;

        ::convFromShortWchar(&name, wsMethodName);

        plMethodNum = findName(g_MethodNames, name, eMethLast);

        if (plMethodNum == -1)
            plMethodNum = findName(g_MethodNamesRu, name, eMethLast);

        delete[] name;

        return plMethodNum;
    }

I see here the inexplicable love for abbreviations. What made the author name the variable “eMethLast”, not “eMethodLast”? They already created "wsMethodName" and "plMethodNum", after all.

Perhaps this is an Easter egg with a reference to Breaking Bad. Then I like it for sure :)