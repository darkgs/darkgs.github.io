---
layout: post
title:  "Interview Questions for C++"
date:   2019-10-16 16:29:00
author: Bonhun Koo
categories: [Coding Interview]
tags:    coding
cover:  "/assets/instacode.jpeg"
---

## Inline function
Inline function is designed to reduce the overheads from function call.
Body of function should be small to achieve efficiency.

#### Overheads from function call
* Store register values related with arguments and return value of calling function.
* Set register values for arguments and return value of callee function.
* Control instruction points of callee and calling function.

#### Disadvantages
* More executable size
* More compile time
* Reduces instruction cache hit rate (compared to Macro)

#### Comparison with Macro
Macro copies piece of code in the preprocessing phase.
In the case of inline function, compiler decides whether performing inline or not.

Please find the detailed description of the inline function from [here][inline_function].

## Call by Value VS by Reference
While calling a function,
* Call by Value: copying variables (using copy constructor for object of class)
* Call by Reference: pass the memory reference

## Volatile
Volatile prevents variables from optimizing by compiler.
It is useful when a variable can be modified by external cause.
* Memory-mapped I/O (MMIO)
* Interupt service routine
* Multi-threaded environment

[inline_function]: https://www.geeksforgeeks.org/inline-functions-cpp

