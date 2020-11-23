---
layout: post
title:  "Cross platform code and macros"
date:   2020-11-16 16:10:28 +0100
categories: Computer-science
desc: ""
---

We usually hear the word *Cross platform* which means that the code can be compiled and executed in multiple platforms, this can be acheived by using a cross platform langauge like Java.

Cross platform code does not come from the air, but it is a layer of abstraction that some geeks added by duplicating the code for each platform and putting them in some libraries we use nowadays.

To write a cross platform library from scratch in C or C++, that works both in windows and linux we will need to write something like:

```
#ifdef _WIN32
// do Windows-specific stuff
#elif __linux__
// do Linux-specific stuff
#endif
```
But the problem is where are those macros defined ?
The answer in short is in the compiler, MinGW compiler will define _WIN32 while a gcc compiler will define __linux__. Thus each compiler will compile only the code specific to it's platform.

Hope you learned something new!