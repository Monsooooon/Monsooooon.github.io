---
title: "Effective_Modern_Cpp_Item_21"
date: 2022-02-11T09:38:45+08:00
draft: false
toc: true
images:
tags: 
  - cpp
  - smart_pointer
  - effetive_modern_cpp
---

>Item 21: Prefer std::make_unique and
std::make_shared to direct use of new.

three make functions
- std::make_unique
- std::make_shared
- std::allocate_shared

## how to use?
```cpp
// deprecated: std::unique_ptr<Widget> upw(new Widget);
auto upw(std::make_unique<Widget>());

// deprecated: std::shared_ptr<Widget> spw(new Widget);
auto spw(std::make_shared<Widget>());
```

## why use?  

### exception safety
consider the following code:
```cpp
processWidget(std::shared_ptr<Widget>(new Widget), computePriority());
```

possible execution order (depends on compiler)
1. new Widget
2. computerPriority --> cause exception!!
3. build shared_ptr

if the second step throws an exception, the new-ed Widget will __NEVER__ be deleted.


correct way with `std::make_shared`: 
```cpp
processWidget(std::make_shared<Widget>(), computePriority());
```

> The best way to do that is to make sure that when you use
new directly, you immediately pass the result to a smart pointer constructor in a
statement that does nothing else.

or split this statement into two parts:

```cpp
std::shared_ptr<Widget> spw(new Widget);
processWidget(std::move(spw), computePriority());
```

### memory allocation efficiency

both the object and the smart_ptr need mem allocations.  
we want to do it in one single shot.

```cpp
// two mem allocs for two data parts
std::unique_ptr<Widget> upw(new Widget);

// one single mem alloc for two data parts
auto upw(std::make_unique<Widget>());
```
