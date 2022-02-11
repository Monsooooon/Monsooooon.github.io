---
title: "Effective_Modern_Cpp_Item_23"
date: 2022-02-11T09:38:45+08:00
draft: false
toc: true
images:
tags: 
  - cpp
  - effetive_modern_cpp
---

> Item 23: Understand std::move and std::forward.

our focus: 
- std::move
- std::forward

they are functions.  

## std::move
std::move casts its arguement to an rvalue. it's better to call it 'rvalue cast'.  

__it does the cast. it doesn't move.__

in some cases, the std::move-ed object is still copied, not moved

```cpp
class Annotation {
public:
  Annotation(const std::string text): 
  value(std::move(text)) { // try move text (call string's move ctor)
    //...
  }
private:
  std::string value;
}

class string {
public:
  string(const string& rhs);  // copy ctpr
  string(string&& rhs);       // move ctor
  // ...
}
```

`std::move` the `const std::string`, produce an `rvalue const string` type object. __the constness remains__

therefore, the string's move ctor cannot use it. but the copy ctor can, because __an lvalue-reference-to-const is permitted to bind to a const rvalue.__

## std::forward

used together when
1. template function
2. T&& as paramter
3. need to pass param to other functions

std::forward will convert the `T&&` into rvalue or lvalue, depending on the original initialization varible's type.

common usage: 

```cpp
#include <iostream>

class Widget {};

void process(const Widget& w) {
    std::cout << "inside lvalue process" << std::endl;
}

void process(Widget&& w) {
    std::cout << "inside rvalue process" << std::endl;
}

template<typename T>
void logAndProcess(T&& param) {
    std::cout << "log" << std::endl;
    process(std::forward<T>(param));
}

int main() {
    Widget w;
    logAndProcess(w);   // log inside lvalue process
    logAndProcess(std::move(w));  // log inside rvalue process
}
```
