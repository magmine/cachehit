---
layout: post
title:  "Cross platform code in C++ and the pre-defined macros"
date:   2020-11-16 16:10:28 +0100
categories: Computer-science
desc: ""
---

We usually hear the word *Cross platform*, which means that the same code can be compiled in multiple platforms without changing anything, this can be acheived by using VM langauges like Java but how does C++ achieve Cross-platform code?


Cross platform code does not come from the air, but it is a layer of abstraction that some geeks added by duplicating the code for each platform and putting them in some libraries or frameworks we use nowadays.

To write a cross platform library from scratch in C or C++, that works both in windows and linux we will need to write something like:

```
#ifdef _WIN32
// do Windows-specific stuff
#elif __linux__
// do Linux-specific stuff
#endif
```
But the problem is where are those macros defined ?

The answer in short is in the compiler, msvc compiler will define _WIN32 and _WIN64 while a gcc compiler will define __linux__. Thus each compiler will compile only the code specific to it's platform.

#### More reading

- `man cpp`
cpp **C PreProcessor**  is a macro processor that is used automatically by the C compiler transform your program by applying the macros before compilation.

- msvc predefined macros:
https://docs.microsoft.com/en-us/cpp/preprocessor/predefined-macros?view=msvc-160

- gcc predefined macros:
https://gcc.gnu.org/onlinedocs/cpp/Predefined-Macros.html

Hope you learned something new!