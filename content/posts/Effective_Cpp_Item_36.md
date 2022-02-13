---
title: "Effective_Cpp_Item_36"
date: 2022-02-13T15:54:32+08:00
draft: false
toc: true
images:
tags: 
  - effective_cpp
  - cpp
  - ood
  - inheritance
---

> Item 36: Never redefine an inherited non-virtual function.

non-virtual member functions are not very good to hide

let's say you have class D and class B. D inherts from B. they both have a member function mf().

```cpp
#include <iostream>

class B {
public:
    void mf() { std::cout << "B::mf" << std::endl; } 
};

class D : public B {
public:
    void mf() { std::cout << "D::mf" << std::endl; } 
};

```

then calling

```cpp
int main() {
    D x;
    B* pB = &x;
    pB->mf();  // B::mf
    D* pD = &x;
    pD->mf();  // D::mf 
}
```

the two function calls cause different results.

but if you change this function into `virtual`

```cpp

class B {
public:
    virtual void mf() { std::cout << "B::mf" << std::endl; } 
};


class D : public B {
public:
    virtual void mf() { std::cout << "D::mf" << std::endl; } 
};
```

then the results will be the same _D::mf_

---
> 1. Never redefine an inherited non-virtual function.  
> 2. Remember to set the destructor as virtual if the class will be inherted.