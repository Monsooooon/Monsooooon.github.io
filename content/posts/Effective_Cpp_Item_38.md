---
title: "Effective_Cpp_Item_38"
date: 2022-02-13T16:34:32+08:00
draft: false
toc: true
images:
tags: 
  - effective_cpp
  - cpp
  - ood
  - inheritance
---

> Item 38: Model “has-a” or “is-implemented-in-terms- of” through composition.

Composition means either “has-a” or “is-implemented-in-terms-of.”

Let's say that you want to implement a `Set` class that need less space than the standard `std::set`. You want to use `std::list`

you probably use 
```cpp
template<typename T>
class MySet: std::list<T> {
  ...
};
```

but that is __QUITE WRONG!!!__

by using public inheritance, you are saying that a `MySet` object `is-a` `std::list` type.

BUT `std::list` may contain duplicate, but a `MySet` doesn't.

Therefore, MySet `is-implemented-in-terms-of` std::list. 

In that case, you should use Composition to express this relationship

```cpp
template<class T>
class MySet: {
public: 
  void insert(const T& item);
  void remove(const T& item);
  //...
private:
  std::list<T> rep; // representation for set data
};
```