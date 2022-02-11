---
title: "Effective_Cpp_Item_32"
date: 2022-02-10T13:43:32+08:00
draft: false
toc: true
images:
tags: 
  - effetive_cpp
  - cpp
  - ood
  - interface design
---

> Item 32: Make sure public inheritance models “is-a.”

> the single most important rule in object-oriented programming with C++ is this: public inheritance means “is-a.”

For example, a student is a person

```cpp
class Person { /* ... */ };
class Student : public Person { /* ... */ };
```

A Student can be used anywhere a Person can be used, but not vice versa.

But __NOTICE__, sometimes the is-a relationship is misleading, especially when it comes to operations

For example, a Penguin is a Bird.
```cpp
class Bird {
public:
    virtual void fly(); // birds can fly
    ...
};

class Penguin: public Bird { // penguins are birds
    ...
};
```

Actually, not EVERY types of Bird can fly. There are flying birds and non-flying bird.

Therefore, when talking about flying, we can define a FlyingBird class.

```cpp
class FlyingBird: public Bird {
public:
    virtual void fly();
    ...
};

class Penguin: public Bird { // penguins are non-flying birds
    ...
};
```

However, if the system does not need to distinguish between flying or non-flying bird, the previos design is perfectly valid.  

Another example of problematic OOD modeling:

> A Square is a Rectangle.

We can have member variable `width` and `height` for a Rectangle class. These two varible can be set independently. But for a Square object, its `width` and `height` should always be equal.

---

Things to remember:  
Public inheritance means “is-a.” Everything that applies to base classes must also apply to derived classes, because every derived class object is a base class object.

