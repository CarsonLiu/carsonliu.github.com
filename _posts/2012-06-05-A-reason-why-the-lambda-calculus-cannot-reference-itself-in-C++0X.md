---
layout: post
title: A reason why the lambda calculus can't reference itself in C++0X
---

{{ page.title }}
================

<p class="meta">5 Jun 2012 - Sydney</p>

A discussion about the possibility of adding a self-reference keyword for lambda expression in C++0X standard is held [here](https://groups.google.com/forum/?fromgroups#!topic/comp.lang.c++.moderated/8bNWnIqsBmA "comp.lang.c++.moderated"). I'd like to share my idea in this article about why there isn't such a useful keyword.



Presume there is a self-reference keyword, <font color="blue">selfref</font>, which could be used in the compound-statement of a lambda expression to refer to that lambda calculus per se. Given this hypothetical keyword, the lambda calculus <font color="green">even</font> could be defined, not pretty wisely, as:
    auto even = [](unsigned x) {
        return x < 2? 0 == x: selfref(x - 2);
    }



So far so good, but the real problem commences when a developer made a simple mistake,omitting the base case.
    auto even_without_base_case = [](unsigned x) {
        return selfref(x - 2);
    }
Interestingly, and somewhat luckily, this mistake would be complained in compiling time, rather than running time. Well-formed though the source code is, the compiler couldn't deduce a type of the expression <font color="green">even_without_base_case</font>. That's it, the big problem in C++0X standard with a hypothesis <font color="blue">selfref</font> key word and that's why the convenient keyword is not provided.

Comparing this problem with Haskell programming language might be of interest. The counterpart of <font color="green">even</font> in Haskell is:
    even' = \x -> if x < 2 then 0 == x else even' (x - 2)
    even_without_basecase' = even_without_base_case'.flip (-) 2
Both of them could be compiled because the Haskell supports generic types, which isn't supported by C++. The type of <font color="green">even_without_base_case'</font> is <font color="red">Integer->t</font> and such type is invalid for C++ any object.

And we can dig further down. If a developer writes code as:
    auto even_infinite_type = [](unsigned x) {
        return selfref;
    }
The counterpart in Haskell is:
    even_infinite_type' = \x -> even_infinite_type'
Neither C++ nor Haskell compiler can compile this function because its type is infinite.

In conclusion, the self-reference keyword of lambda calculus could not be added to C++0X standard directly since it might strike the type system of C++.


