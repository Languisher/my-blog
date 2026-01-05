---
title: "C++: RAII, Smart Pointers"
date: 2026-01-11
description: Stanford CS106L Notes — Spring 2025
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
## RAII

There are many resources that you need to *release* after *acquiring*.

| Resource      | Acquire   | Release |
|---------------|-----------|---------|
| Heap memory   | `new`     | `delete` |
| Files         | `open`    | `close`  |
| Locks         | `try_lock`| `unlock` |
| Sockets       | `socket`  | `close`  |

To avoid memory leakage, **Resource Acquisition is Initialization (RAII)** is developed.
- All resources used by a class should be acquired in the _constructor_
- All resources that are used by a class should be release in the _destructor_

Bad examples. I/O managed in code. If the code throws an exception between the `open()` and the `close()`, the input never get released.

```cpp
void printFile() {
	ifstream input;
	input.open('hamlet.txt');
	
	string line;
	while(getLine(input, line)) { /* ... */ }
	
	input.close();
}
```

## Smart Pointers

RAII-compliant pointers
- `std::unique_ptr`. Uniquely owns its resource, can't be copied
- `std::shared_ptr`, can make copies, destructed when the underlying memory goes out of scope
- `std::weak_ptr`, a class of pointers designed to mitigate circular dependencies

`std::shared_ptr<T>` implements shared ownership using reference counting: the object is deleted automatically when the last owning shared_ptr goes away.

`std::weak_ptr<T>` . It is used to break cyclic ownership by expressing observation instead of ownership. It allows an object to access another object if it is still alive, without extending its lifetime or contributing to reference counting.

Example.

```cpp
struct A {
    std::shared_ptr<B> bp; // A owns B
};

struct B {
    std::shared_ptr<A> ap; // B owns A, oh no!
    std::weak_ptr<A> ap; // B observes A (break the cycle)
};

auto a = std::make_shared<A>();
auto b = std::make_shared<B>();
a->bp = b;
b->ap = a;
```

### Initialization

Use `std::make_unique<T>` and `std::make_shared`.

Weak pointers must be derived from a shared pointer.

```cpp
Point p = Point(4, 3); // old fashioned

// std::unique_ptr<T> uniquePtr = std::make_unique<T>();
std::unique_ptr<Point> uniquePtr = std::make_unique<Point>(4, 3);

// std::shared_ptr<T> uniquePtr = std::make_shared<T>();
std::shared_ptr<Point> sharedPtr = std::make_shared<Point>(4, 3);

std::weak_ptr<Point> weakPtr = sharedPtr;
```

Not recommended to use `unique_ptr` or `shared_ptr` to point to an already existed object.