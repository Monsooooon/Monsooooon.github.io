---
title: "Effective_Cpp_Item_34"
date: 2022-02-10T14:48:32+08:00
draft: false
toc: true
images:
tags: 
  - effective_cpp
  - cpp
  - ood
  - inheritance
---

> Item 34: Differentiate between inheritance of interface and inheritance of implementation.

there are 3 types of member functions
- pure virtual functions
- simple(impure) virtual functions
- non-virtual functions

They differs in the way whether derived classes should inherit the interface or implementation:
- Pure virtual functions specify inheritance of interface only.
- Simple (impure) virtual functions specify inheritance of interface plus inheritance of a default implementation.
- Non-virtual functions specify inheritance of interface plus inheritance of a mandatory implementation.

When designing a Base class, think clearly about its member functions and how derived classes will use them.
