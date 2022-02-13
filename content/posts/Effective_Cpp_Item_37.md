---
title: "Effective_Cpp_Item_37"
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

> Item 37: Never redefine a function’s inherited default parameter value.

virtual functions are dynamically bound, but default
 parameter values are statically bound.

you should avoid adding default param to virtual functions

let's say you have two class B & D. D is derived from B.
```cpp
class B {
  virtual void mf(int i = 1) {
    cout << i << endl;
  }
};

class D : public B {
  virtual void mf(int i = 2) {
    cout << i << endl;
  }
};

D x;
x.mf(); // "1" not "2"
```

__NOTICE__: virtual function's default param will not be dynamically binded!

