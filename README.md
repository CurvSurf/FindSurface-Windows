# FindSurface for Windows

**Curv*Surf* FindSurface™**

## Overview

This library is the implementation of the **FindSurface** library for **Windows (C/C++).**

The C++ header file (`.hpp`) provided with the library contains an example wrapper of FindSurface to help you to use FindSurface in an OOP style. You may write your own wrapper in order to use more advanced features of the latest version of C++, or to use FindSurface in lower versions of C++, if you need. The current header is written for C++14.



## Samples

These are the samples to help you get started to make your application with the library (more samples are to be added in the future):

- [BasicDemo (C/C++)](https://github.com/CurvSurf/FindSurface-BasicDemo-Windows-Linux)



## F.A.Qs

### Q. How do I import this library to my project?

Refer to [this official document](https://docs.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp?view=msvc-160).



### Q. My MSVC compiler complains about "C++ 14+ is required" when using the C++ header.

Since the wrapper is written for C++14, the following macro guard has been inserted at the beginning of the header file, to indicate the version requirement:

````c++
#if !defined(__cplusplus) || __cplusplus < 201402L
#error "C++ 14+ is required"
#endif
````

However, MSVC compiler does not redefine the macro according to the version of C++ for compatibility reasons.

The workaround is to set the following **compiler option** to your project or **just delete the guard**:

````C++
/Zc:__cplusplus
````

Refer to [this document](https://docs.microsoft.com/en-us/cpp/build/reference/zc-cplusplus?view=msvc-160) for more details.



## ---

(c) Copyright 2021 CurvSurf, Inc. All rights reserved.

This library's ownership is solely on CurvSurf, Inc. and anyone can use it for non-commercial purposes. Contact to support@curvsurf.com for commercial use of the library.

