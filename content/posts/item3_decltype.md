---
title: "Item3_decltype"
date: 2022-02-10T12:09:32+08:00
draft: false
toc: true
images:
tags: 
  - effetive_modern_cpp
  - cpp
---

## 使用背景

假设我们定义一个函数，接收一个容器对象Container，一个索引对象Index，先进行用户认证，然后返回容器里一个对象的引用。需要通过 `decltype`，才能知道容器的类型参数。

## 具体使用
### 对容器的元素类型进行推导
在cpp11中我们使用`trailling type deduction`的方式, 如下

```cpp
// cpp 11
template<typename Container, typename Index>
auto wrapFunc(Container& c, Index i) -> decltype(c[i]) {
    authenticateUser();
    return c[i];
}
```
在cpp14中，可以通过`decltype(auto)`的方式来实现同样的功能。  

如果只使用auto而不使用decltype, 则返回类型是by value， 而不是by reference。故需要使用decltype。

```cpp
// cpp 14
template<typename Container, typename Index>
decltype(auto) wrapFunc(Container& c, Index i) {
    authenticateUser();
    return c[i];
}
```

同样，对于变量也可以用 `decltype(auto)` 来保证所推导的类型包含修饰符。

```cpp
Widget w;
const Widget& cw = w;
auto myWidget1 = cw;            // type: Widget
decltype(auto) myWidget2 = cw;  // type: const Widget&
```

### 对 Universal Reference 进行类型推导
前面的例子中，函数wrapFunc接收的容器类型是 Container&, 无法绑定临时对象（rvalue）。为了支持这一点， 可以替换成 `Container&&`.
因此可以替换为：
```cpp
// cpp 14
template<typename Container, typename Index>
decltype(auto) authAndAccess(Container&& c, Index i) {
    authenticateUser();
    return std::forward<Container>(c)[i];
}

// cpp 11
template<typename Container, typename Index>
decltype(auto) authAndAccess(Container&& c, Index i)
-> decltype(std::forward<Container>(c)[i]) {
    authenticateUser();
    return std::forward<Container>(c)[i];
}
```

## 其他需要注意的点
> For lvalue expressions of type T other than names, decltype always reports a
type of T&.

decltype对于所有非name的lvalue，推导的类型都是引用类型，对name的lvalue，推导类型是值类型。

因此要小心不要意外返回了对local变量的引用
```cpp
decltype(auto) f1()
{
    int x = 0;
    ...
    return x; // decltype(x) is int, so f1 returns int
}

// 这里很危险！
decltype(auto) f2()
{
    int x = 0;
    ...
    return (x); // decltype((x)) is int&, so f2 returns int&
}

```
