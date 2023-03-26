---
layout: post
title:  "A quick review of RegEx"
date:  2022-09-16T14:42:00+0800
tags: ["review_label"]
categories: ["Programming"]
keywords: regex
author: Yichong Zhou
hidden: false
---

> RegEx specifies a text search pattern that is used to perform string-searching and validation. 
This post mainly focuses on the core concepts and critical details in RegEx that help us better understand it. 
Most of the materials in this post are collected from internet. 



<!--more-->


{% include toc.html %}


## Basics

Regular expressions are used in search engines, in word processors and text editors, in text processing utilities such as sed and AWK, and in lexical analysis in a compiler. Most general-purpose programming languages support regex capabilities either natively or via libraries, including Python, C/C++, Java, JavaScript etc. 
Implementations of regex functionality is often called a **regex engine**, and a number of libraries are available for reuse. In the late 2010s, several companies started to offer hardware, FPGA, GPU implementations of PCRE compatible regex engines that are faster compared to CPU implementations.


**Lazy matching**

In Python and some other implementations (e.g. Java), the three common **quantifiers** (`*`, `+` and `?`) are greedy by default because they match as many characters as possible. The regex `".+"` (including the double-quotes) applied to the string

`"Ganymede," he continued, "is the largest moon in the Solar System."`

matches the entire line (because the entire line begins and ends with a double-quote) instead of matching only the first part, `"Ganymede,"`. The aforementioned quantifiers may, however, be made lazy or minimal or reluctant, matching as few characters as possible, by appending a question mark: `".+?"` matches only `"Ganymede,"`.



**Possessive matching**

In Java, quantifiers may be made possessive by appending a plus sign, which disables backing off (in a backtracking engine), even if doing so would allow the overall match to succeed: While the regex `".*"` applied to the string

`"Ganymede," he continued, "is the largest moon in the Solar System."`

matches the entire line, the regex `".*+"` does not match at all, because `.*+` consumes the entire input, including the final `"`. Thus, possessive quantifiers are most useful with negated character classes, e.g. `"[^"]*+"`, which matches `"Ganymede,"` when applied to the same string.

Another common extension serving the same function is **atomic grouping**, which **disables backtracking** for a parenthesized group. The typical syntax is `(?>group)`. For example, while `^(wi|w)i$` matches both `wi` and `wii`, `^(?>wi|w)i$` only matches `wii` because the engine is **forbidden from backtracking and so cannot try setting** the group to `"w"` after matching `"wi"`. 

Possessive quantifiers are easier to implement than greedy and lazy quantifiers, and are typically more efficient at runtime. 



## Features


#### Single Characters

| Use           | To match any character     | 
| ------------- |:-------------------------- |
| `[a–z]` `[abc]`   | **In** the *a-z* range, *a*,*b*,*c* set |
| `[^a–z]` `[^abc]`  | **Not in** the *a-z* range, *a*,*b*,*c* set  |
| `.`           | Any except `\n` (new line) |
| `\char`       | Escaped special character  |
{: class="info" style=""}

 
#### Character Classes

| Use           | To match character        | 
| ------------- |-------------------------- |
| `\w` `\W`     | Word character, Non-word character  |
| `\d` `\D`     | Decimal digit, Not a decimal digit |
| `\s` `\S`     | White-space character, Non-white-space character  |
| `\p{ctgry}` `\P{ctgry}`  | In that Unicode category or block, Not in |
{: class="info" style=""}


#### Groups

| Use                      | To define                      | 
| -----------------------  |------------------------------- |
| `(?:exp)`                | Noncapturing group             |
| `(?=exp)`                | Zero-width positive lookahead  |
| `(?!exp)`                | Zero-width negative lookahead  |
| `(?<=exp)`               | Zero-width positive lookbehind |
| `(?<!exp)`               | Zero-width negative lookbehind |
| `(?>exp)`                | Non-backtracking (greedy)      |
| `(exp)`                  | Indexed group                  |
| `(?<name>exp)`           | Named group                    |
| `(?<name1-name2>exp)`    | Balancing group                |
{: class="info" style=""}

#### Group Backreferences


| Use           | To match        | 
| ------------- |---------------- |
| `\n`          | Indexed group   |
| `\k<name>`    | Named group     |
{: class="info" style=""}
     
 

#### Quantifiers

| Greedy        | Lazy            | Matches           | 
| ------------- |---------------- | ----------------- |
| `*`           | `*?`            | 0 or more times   |
| `+`           | `+?`            | 1 or more times   |
| `?`           | `??`            | 0 or 1 time       |
| `{n}`         | `{n}?`          | Exactly n times   |
| `{n,}`        | `{n,}?`         | At least n times  |
| `{n,m}`       | `{n,m}?`        | From n to m times |
{: class="info" style=""}









  

 









## References & Resources  {#references}


*\[1\]. [Regular expression - wikipedia][1]*
{: id="ref-1-regular-expression" class="ref-item" }

*\[2\]. [regexr.com][2]*
{: id="ref-2-regexr-com" class="ref-item" }

*\[3\]. [regex101.com][3]*
{: id="ref-3-regex101-com" class="ref-item" }


[1]: <https://en.wikipedia.org/wiki/Regular_expression/>  "Regular expression - wikipedia"

[2]: <https://regexr.com/>  "regexr.com"

[3]: <https://regex101.com/>  "regex101.com"







