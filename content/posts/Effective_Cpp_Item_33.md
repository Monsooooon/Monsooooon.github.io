---
title: "Effective_Cpp_Item_33"
date: 2022-02-10T14:09:32+08:00
draft: false
toc: true
images:
tags: 
  - effective_cpp
  - cpp
  - ood
  - interface design
---

> Item 33: Avoid hiding inherited names.

In Cpp, names be be hidden.  
for example:
```cpp
double x;

void func() {
    int x;
    cin >> x; // read the int x, not the double x;
}
```

the name searching rule is bottom-up, from local to larget scope, and finally to global scope.

the same rule applies to member functions in inheritance.

```cpp
class Base {
private:
    int x;
public:
    virtual void mf1() = 0;
    virtual void mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(double);
    ...
};

class Derived: public Base {
public:
    virtual void mf1();
    void mf3();
    void mf4();
    ...
};
```

the `mf1` and `mf3` names in Derived class hide the one in Base class

therefore, 

```cpp
Derived d;
d.mf1();
d.mf1(1); // complier error, because Derived::mf1 hides Base::mf1(int);

d.mf3();
d.mf3(1.0); // complier error, because Derived::m3 hides Base::mf3(double);
```

Name hiding can happens even the function arguments & return types are different.

To address this problem, you should use `using`
```cpp
class Base {
private:
    int x;
public:
    virtual void mf1() = 0;
    virtual void mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(double);
    ...
};
class Derived: public Base {
public:
    using Base::mf1; // make all things in Base named mf1 and mf3
    using Base::mf3; // visible (and public) in Derived’s scope
    virtual void mf1();
    void mf3();
    void mf4();
    ...
};
```

now the above code works

In summary:
> If you inherit from a base class with __overloaded functions__ and you want to redefine or override only some of them, you need
to include a using declaration for each name you’d otherwise be hiding.
If you don’t, some of the names you’d like to inherit will be hidden.

> a using declaration makes all inherited functions with a given
name visible in the derived class.