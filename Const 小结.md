<!--
 * @Author: JinBridge
 * @Date: 2022-03-14 15:28:55
 * @LastEditors: JinBridge
 * @LastEditTime: 2022-03-14 22:05:09
 * Copyright (c) 2022 by JinBridge, All Rights Reserved. 
-->
# Const 小结

## 普通的 `const`

最普通的 `const` ,用于声明某一个变量为一个常量。比如
```cpp
const int a = 10;
```
值得注意的是，声明为 `const` 的常量必须给定初始值。像下面这样的会在编译时出错：
```cpp
const int a; // 错误，没有给定初始值
```

## 指针的 `const`

指针的 `const` 有两种，一种是 `const` 指针， 一种是指向 `const` 的指针。

### `const` 指针

下面是一个 `const` 指针：

```cpp
int* const p = &a;
```

它的 `const` 意义在于 __这个指针自己是一个常量，不能被修改指向其他量__ 。例如下面的就是非法的：

```cpp
int* const p = &a;
p = &b; // 错误，不能指向其他量
```

但是，可以通过这个指针修改指向的量的数值：

```cpp
int* const p = &a;
*p = 3;
```

### 指向 `const` 的指针

与 `const` 指针不同，指向 `const` 的指针 __可以更改指向的对象，但是不能更改对象的值。__

```cpp
const int* p = &a;
p = &b; // 正确
*p = 3; // 错误，不能修改对象的值
```

这里需要注意的是，这个指针并不一定必须指向 `const` 量。上面代码中，即使 `a` 与 `b` 都是变量也可以正常运行。简而言之，就是 __它自认为自己指向的是一个常量__ 。

## 普通函数的 `const`

### 参数列表的 `const`

可以将 `const` 作为函数的形参，如：

```cpp
void foo(const int a);
```

此时 `foo` 可以读取 `a` 但不能修改 `a`.

值得注意的是，__在编译过程中 `const` 会被忽略掉__，也就是说下面的重载是非法的：

```cpp
void foo(const int a);
void foo(int a); // 错误，编译器会将这两个 foo 认为成一样的 foo(int)

void foo(int* a);
void foo(int* const a); // 错误，编译器会将这两个 foo 认为是一样的 foo(int*)
```

但是注意，下面的指向 `const` 的指针与 `const` 引用的重载是合法的：

```cpp
void foo(int& a);
void foo(const int& a);

void foo(int* a);
void foo(const int* a);
```

所以，如果函数没有修改参数的必要，尽量加上 `const` ，不仅可以避免误修改，同时也可以传入 `const` 类型。

### 返回类型的 `const`

值得一提的是返回 `const` 常常与返回引用同时使用。比如下面在类中使用。

## 类里的 `const`

### `const` 成员函数

```cpp
class A {
    ...
    void foo() const { ... }
};
```

这就是一个 `const` 成员函数，他的作用在于 __修改隐式 `this` 指针的类型__ 从而使得 `foo` 只能读取值而不能修改这个对象的值。

注意，下面的语句会报错：

```cpp
class A {
    ...
    void foo() { ... }
    ...
};
...
const A obj;
obj.foo(); // 错误：没有 const 版本的成员函数
...
```

因为 `obj` 是常量，而 `foo` 的 `this` 是变量。

### 返回 `const` 的成员函数

```cpp
class A {
   public:
    int& foo() { return val; }
    ...
   private:
    int val;
};
```

这段代码看上去没什么问题，但是有漏洞。比如下面这个语句就会正常执行：

```cpp
foo() = -1;
```

这就导致程序本来 `private` 的成员可以被外界更改。因此应当改为

```cpp
const int& foo() { return val; }
```