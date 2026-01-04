---
title: "C++ Basics (1): Types, Structs and Objects"
date: 2026-01-04
description: ""
tags:
  - cpp
image: ""
imageAlt: ""
imageOG: false
hideCoverImage: false
hideTOC: false
targetKeyword: ""
draft: false
---
## Types and Structs

*Keywords*.
- Struct
- Type alias
- Auto type inference

### Struct

+ build data together into a new, single object, thus
+ enable user to return multiple values simultaneously.

```cpp
struct StanfordID {
    string name;
    string sunet;
    int idNumber;
};

StanfordID id;
id.name = "Jacob";
id.sunet = "jtrb";
id.idNumber = 6504417;

// -------------------
// Can now be returned 
StanfordID issueNewID() {
    StanfordID id {"Jacob", "ad", 23};
    return id;
}

StanfordID new_id = issueNewID();
```

### `std::pair`

`std::pair` is a general purpose struct with two fields.

```cpp
std::pair<int, double> example {2, 3.14};
int first_num = example.first; // .first
double second_num =example.second; // .second

std::cout << example.first << std::endl;
```

### `std::tuple`

`std::tuple` is a general purpose struct with multiple fields.

```cpp
std::tuple<int, std::string, double> {2, "ad", 80.8};
```


### Type alias and type inference

_Type alias_. `using` creates cd types alias and `auto` infers the type of a variable.

```cpp
using Roots = std::pair<double, double>; // alias
using Solution = std::pair<bool, Roots>;
```

_Automatic Type Inference_. `auto` infers the type of a variable

```cpp
std::map<std::string, std::vector<std::pair<int, std::unordered_map<char, double>>>> complexType;

auto it complexType.begin(); // auto = std::map<std::string, std::vector<std::pair<int, std::unordered_map<char, double>>>>::iterator
```



## Initialization and Reference

### Initilization

Methods of initialization.

+ *Direct initialization*: `()`
	```cpp
	int numOne(12.0); // Direct initialization : doesn't care if 12.0 is a int or not
	```
+ *Uniform initialization* (recommended!) : Care about types: `{}`
	```cpp
	int numTwo{12.0}; // Uniform initialization : DOES care, raise error
	```

### Structured binding

*Structured Binding*.
- A useful way to initialize some variables from data structures with fixed sizes at compile time
- Ability to access multiple values returned by a function
- _But_! Can only use on objects where the _size_ is _known at compile time_.

![](init_structured_binding-1.png)
Example (Structured Binding).

This is identical to use `std::get<i>`:

```cpp
auto classInfo = getClassInfo();
std::string className = std::get<0>(classInfo);
std::string buildingName = std::get<1>(classInfo);
std::string language = std::get<2>(classInfo);
```


### References

A *reference* is an _alias_ to an already-existing thing.

```cpp
int num = 5;
int& ref = num; // Reference
```

Example (Changing the value of the reference). The code `ref = 10;` will also change the value of `num`, the "referenced" value.

![](init_ref_example-1.png)
### Pass by reference

_Passing in a variable by reference_: Take in the _actual_ piece of memory, don't make a copy!

```cpp
// This function takes the actual piece of `nums` instead of modifing the COPY of `nums`
void shift(std::vector<std::pair<int, int>> &nums) {
    for (auto &num : nums) { // Common Bug
        std::swap(num.first, num.second);
    }
}

int main() {
    std::vector<std::pair<int, int>> nums {{2, 3}, {4, 5}};
    shift(nums);
    std::cout << nums[0].first << " " << nums[0].second << std::endl;
}
```

Common bug. With `auto`, structured bindings copy the element; add `&` to bind by reference.

```cpp
// `auto` structured bindings copy;
// `auto&` binds by reference.
for (auto& [num1, num2] : nums) { ... }
```

### L-values and R-values and reference


*L-value*
- has stable _identifiable location_ in memory, and 
- you can use it to modify the value stored at the location.

*R-value* is something that's _"manufactured on the spot"_. It is temporary.

Example (R-value).
- The return value of a function (which is not a reference): `f()`
- Literal values `42`

Only L-values can be passed by reference.

### Const and reference

Can't declare a non-const reference to a const variable

```cpp
std::vector<int> vec{1, 2, 3};
const std::vector<int> const_vec{1, 2, 3};
const std::vector<int> &const_ref{vec};

// incorrect
std::vector<int> &invalid_ref{const_vec};
```
