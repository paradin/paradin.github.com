---
layout: post
title: "[转载]Why does C++ compilation take so long?"
date: 2013-04-07 14:10
comments: true
categories: c++
---

Several reasons:

		1. Header files
		2. Linking
		3. Templates
		4. Optimization
		5. Machine code
<!--more-->

1. Header files
- 

Every single compilation unit requires hundreds or even thousands of headers to be **1: loaded**, and **2: compiled**. Every one of them typically has to be **recompiled** for every compilation unit, because the preprocessor ensure that the result of compiling a header might vary between every compilation unit. (A macro may be defined in one compilation unit which changes the content of the header).

This is probably the main reason, as it requires huge amounts of code to be compiled for every compilation unit, and additionally, every header has to be compiled multiple times (once for every compilation unit that includes it)

2. Linking
- 

Once compiled, all the object files have to be linked together. This is basically a monolithic process that **can't very well be parallelized**, and has to process your entire project.

Parsing: The syntax is extremely complicated to parse, depends heavily on context, and is very hard to disambiguate. This takes a lot of time

3. Templates
- 

In C#, List<T> is the only type that is compiled, no matter how many instantiations of List you have in your program. In C++, vector<int> is a completely separate type from vector<float>, and each one will has to be compiled separately.

Add to this that templates make up a full turing-complete "sub-language" that the compiler has to interpret, and this can become ridiculously complicated. Even relatively simple template metaprogramming code can define recursive templates that create dozens and dozens of template instantiations. Templates may also result in extremely complex types, with ridiculously long names, adding a lot of extra work to the linker. (It has to compare a lot of symbol names, and if these names can grow into many thousand characters, that can become fairly expensive).

And of course, they exacerbate the problems with header files, because templates generally have to be defined in headers, which means far more code has to be parsed and compiled for every compilation unit. In plain C code, a header typically only contains **forward declarations**, but very little actual code. In C++, it is not uncommon for almost all the code to reside in header files.


4. Optimization
- 

C++ allows some very dramatic optimizations. C# or Java don't allow classes to be completely eliminated (they have to be there for reflection purposes), but even a simple C++ template metaprogram can easily generate dozens or hundreds of classes, all of which are inlined and eliminated again in the optimization phase.

Moreover, a C++ program must be fully optimized by the compiler. A C# program can rely on the JIT compiler to perform additional optimizations at load-time, C++ doesn't get any such "second chances". What the compiler generates is as optimized as it's going to get.


5. Machine code
- 

C++ is compiled to machine code which may be somewhat more complicated than the bytecode Java or .NET use (especially in the case of x86).
(This is mentioned out of completeness only because it was mentioned in comments and such. In practice, this step is unlikely to take more than a tiny fraction of the total compilation time.)

Most of these factors are shared by C code, which actually compiles fairly efficiently. The parsing step is a lot more complicated in C++, and can take up significantly more time, but the main offender is probably templates. They're useful, and make C++ a far more powerful language, but they also take their toll in terms of compilation speed.