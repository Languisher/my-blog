---
title: "C++ Basics (6): Functions and Lambdas"
date: 2026-01-04
description: ""
tags: []
image: ""
imageAlt: ""
imageOG: false
hideCoverImage: false
hideTOC: false
targetKeyword: ""
draft: true
---
Represent functions as variables.

## History: Predicates

**Predicate**: boolean-valued function

Example.
```c++
bool isVowel(char c) {  
    c = toupper(c);  
    return c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U';  
}  
```

Idea: Passing functions to _generalize_ an algorithm with _user-defined behavior_

```cpp
template <typename It, typename Pred>  
It find_pred(It first, It last, Pred pred) {  
    for (auto it = first; it != last; ++it) {  
        if (pred(*it)) return it;  
    }  
    return last;  
}

std::string corlys = "Lord of the Tides";
auto it = find_pred(corlys.begin(), corlys.end(), isVowel);  
```

In this example, `Pred` is a function pointer which is `bool (*)(char)`, and can be any callable object that takes an element and returns bool.