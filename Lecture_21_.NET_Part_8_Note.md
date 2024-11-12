# .NET Part 8

## Description

- 本篇笔记是根据Peng Dai老师 Lecture 21 .NET Part8 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGVO6w5IJQ/lvrznudNo6xufpQ7sM0GVg/view?utm_content=DAGVO6w5IJQ&utm_campaign=designshare&utm_medium=embeds&utm_source=link

## Table Of Content

- [1. What is OOP and Why need it](#1-what-is-oop-and-why-need-it)
  - [1.1. What is OOP](#11-what-is-oop)
    - [Class Structure Example](#class-structure-example)
  - [1.2. Advantages of OOP](#12-advantages-of-oop)
- [2. Class and Object](#2-class-and-object)
  - [Example](#example)
  - [Coding Exercise](#coding-exercise)
- [3. Access Modifier](#3-access-modifier)
  - [Coding Exercise](#coding-exercise-1)
- [4. Constructors](#4-constructors)
  - [Key Points](#key-points)
  - [Coding Exercise](#coding-exercise-2)
- [5. # Class Member](#5-class-member)
  - [Fields - 字段](#fields---字段)
  - [Property – readable/writable or read-only](#property--readablewritable-or-read-only)
- [6. Inheritance (Derived and Base Class)](#6-inheritance-derived-and-base-class)
  - [Coding Exercise](#coding-exercise-3)
  - [Additional Concepts](#additional-concepts)
- [7. Interface](#7-interface)
  - [How to Choose Between Abstract Class and Interface](#how-to-choose-between-abstract-class-and-interface)
  - [两种多态 - Dynamic vs Static](#两种多态---dynamic-vs-static)
  - [Compile Time - Static](#compile-time---static)
  - [Runtime - Dynamic](#runtime---dynamic)
  - [How to Implement Dynamic Polymorphism](#how-to-implement-dynamic-polymorphism)
  - [Polymorphism via Interfaces](#polymorphism-via-interfaces)
  - [Virtual Method vs Abstract Method](#virtual-method-vs-abstract-method)
- [8. Sealed 方法和类](#8-sealed-方法和类)
  - [封闭方法](#封闭方法)
  - [封闭类](#封闭类)
- [9. IoC vs DIP vs DI](#9-ioc-vs-dip-vs-di)
  - [依赖反转原则的核心思想](#依赖反转原则的核心思想)
  - [相关概念](#相关概念)
  - [原则、模式和框架的关系](#原则模式和框架的关系)
- [10. Types of Dependency Injection in C#](#10-types-of-dependency-injection-in-c)
  - [DI Packages in C#](#di-packages-in-c)
    - [1. Microsoft.Extensions.DependencyInjection](#1-microsoftextensionsdependencyinjection)
    - [2. Other DI Packages and Their Pros and Cons](#2-other-di-packages-and-their-pros-and-cons)


## 1. What is OOP and Why need it

### 1.1. What is OOP

**Programming Design Concept** – The basic unit of program operation is an object. Compared to procedural programming, it aligns more closely with human thinking.

An example to illustrate this: Putting an elephant into a refrigerator.

#### Class Structure Example:
- **Elephant Class**
  - Attribute: `eHeight` (Height of the elephant)

- **Refrigerator Class**
  - Attribute: `rHeight` (Height of the refrigerator)
  - Behavior: `OpenRefrigerator()`, `CloseRefrigerator()`

- **Operation Class**
  - Behavior: `PutElephantInRefrigerator()`

### 1.2. Advantages of OOP
- OOP is faster and easier to execute.
- OOP provides a clear structure for programs.
- OOP helps keep C# code DRY ("Don't Repeat Yourself"), making the code easier to maintain, modify, and debug.
- OOP enables the creation of fully reusable applications with less code and shorter development time.

## 2. Class and Object

- A **class** is a template for objects.
- An **object** is an instance of a class.

### Example:
- **Class**: Fruit
  - **Objects**: Apple, Banana, Mango

- **Class**: Car
  - **Objects**: Volvo, Audi

### Coding Exercise
- Create classes for `Teacher`, `Student`, and `Course`.
- Create objects for:
  - Teacher: Mr. Jack Lee
  - Student: Peter Hugh
  - Course: C# OOP

> **Tips**: Follow class naming conventions.

## 3. Access Modifier

| Modifier   | Description |
|------------|-------------|
| `public`   | The code is accessible for all classes. |
| `private`  | The code is only accessible within the same class. |
| `protected`| The code is accessible within the same class, or in a class that is inherited from that class. You will learn more about inheritance in a later chapter. |
| `internal` | The code is only accessible within its own assembly, but not from another assembly. You will learn more about this in a later chapter. |

### Coding Exercise
Define:
- One private field – `salary`
- One protected method – `PrintName`
- One public method – `Teaching` for the `Teacher` class

## 4. Constructors

- A **constructor** is a special method that is used to initialize objects.
- It can be used to set initial values for fields.
- The constructor must **match the class name**.
- No return type (not even `void`).

### Key Points:
1. The system automatically generates a parameterless constructor.
2. Multiple constructors can be defined.
3. Constructors can call other constructors.
4. Static constructors are also allowed.
5. In real projects, pay attention to the order of field assignments, constructor execution, and static constructor execution.

### Coding Exercise
- Create constructors

## 5. # Class Member

### Fields - 字段
- 用来保存 class 方法访问的数据。
- 一般不推荐 `public`。
- 可以为只读 `readonly`。

### Property – readable/writable or read-only
- 具有访问器（accessor）：`get` / `set`
- 大多数定义为 `public`
- 可以自定义初始值（避免使用系统自定义的默认值）
- 注意区分几种定义的形式：

```csharp
    public string? FirstName { get; private set; }
    public string? FirstName { get; } // 没有 set，只能通过构造函数赋值，之后不能变
    public string? FirstName { get; init; } // 没有 set，但定义了 init，只能通过初始化赋值，之后不能变
```

## 6. Inheritance (Derived and Base Class)

- **Derived Class (child)** - The class that inherits from another class.
- **Base Class (parent)** - The class being inherited from.

### Coding Exercise
Define:
- `Tutor` class
- `Admin` class
  - Both with the same properties and methods as the `Teacher` class

Create a base class `Staff` and inherit from `Staff` for all `Teacher`, `Tutor`, and `Admin` classes.

### Additional Concepts
- **Hide Method in Base Class** - Not recommended.
- **Abstract and Virtual Methods** - Abstract methods must be implemented by derived classes, while virtual methods can be overridden.
- **Abstract Base Classes** - Classes that cannot be instantiated directly and are designed to be inherited.

## 7. Interface

An **interface** is a completely "abstract class" that can only contain abstract methods and properties (with empty bodies).

- It is a good practice to start the name of an interface with the letter "I".
- By default, members of an interface are abstract and public.
- Interfaces can contain properties and methods, but not fields.
- You do not need to use the `override` keyword when implementing an interface.

### How to Choose Between Abstract Class and Interface
- Use an interface when you want to define capabilities (`has a`).
- Use an abstract class when you want to define a general type (`is a`).

### 两种多态 - Dynamic vs Static

### Compile Time - Static
- 多态的具体实现是在程序编译时确定的。
- 通过方法重载（overloading）实现，方法名相同但参数不同。
  - 仅仅返回值不同是不允许的。
- 示例：举例

### Runtime - Dynamic
- 多态的具体实现（例如方法）在程序编译时不确定，只有在程序运行时才明确。
- 通过方法的重写（例如 `override` 基类方法）或者实现接口方法来实现，方法名和参数都相同。

### 两种多态 - Dynamic vs Static

### Compile Time - Static
- 在程序编译时确定多态的具体实现。
- 通过方法重载（overloading）实现，即方法名相同但参数不同。
  - **仅仅返回值不同是不允许的**。
- 示例：举例

### Runtime - Dynamic
- 在程序编译时具体实现（如方法）不确定，只有在程序运行时才明确。
- 通过方法的重写（如 `override` 基类方法）或实现接口方法实现，即方法名和参数都相同。

### How to Implement Dynamic Polymorphism

- **Base classes** may define and implement `virtual` methods, and derived classes can `override` them, providing their own definitions and implementations.

- At run-time, when client code calls the method, the CLR (Common Language Runtime) looks up the run-time type of the object and invokes that override of the virtual method.

### Polymorphism via Interfaces
- Implement polymorphism using an interface instead of an abstract or concrete base class.

### Virtual Method vs Abstract Method

- **Virtual method and abstract method**
  - **Virtual method** in base class – 可以有通用实现
  - **Overriding** virtual methods in derived class
  - Access base class method from derived class

  - **Abstract method** – 无实现
  - 有 Abstract 方法的类必须为 abstract 类 – 不能实例化

## 8. Sealed 方法和类

### 封闭方法
- 不允许子类对方法进行重写。
- 如果类继承于其他基类，使用 `sealed + override`。

### 封闭类
- 不允许作为基类被继承。

```csharp
public class DerivedClass : SealedMethodInBaseClass
{
    public override sealed void PrintName()
    {
        base.PrintName();
    }
}

public class GrandDerivedClass : DerivedClass
{
    // GrandDerivedClass 不可类重写 PrintName 方法
    // 只能调用基类的方法
    public void Print()
    {
        base.PrintName();
    }
}
```

## 9. IoC vs DIP vs DI

- **IoC (Inversion of Control)**: 控制反转原则
- **DIP (Dependency Inversion Principle)**: 依赖反转原则
- **DI (Dependency Injection)**: 依赖注入

### 依赖反转原则的核心思想
- 高层模块或类不应依赖于低层模块，而是都应依赖于抽象。
- 抽象不应依赖于细节，细节应依赖于抽象。

### 相关概念
- **.NET** 支持依赖注入 (DI) 作为一种软件设计模式，是实现控制反转 (IoC) 的一种技术。

### 原则、模式和框架的关系
1. **原则 (Principle)**
   - Inversion of Control (IoC)
   - Dependency Inversion Principle (DIP)
2. **模式 (Pattern)**
   - Dependency Injection (DI)
3. **框架 (Framework)**
   - IoC Container

## 10. Types of Dependency Injection in C#

1. **Constructor Injection**:
   - Dependencies are injected through the class constructor.

2. **Property Injection**:
   - Dependencies are injected through public properties.

3. **Method Injection**:
   - Dependencies are injected through methods.

---

# DI Packages in C#

### 1. Microsoft.Extensions.DependencyInjection
- 提供一个常用于 .NET Core 应用程序的轻量级、灵活的 DI 容器。

  - **Service Collection in DI**:
    - `ServiceCollection` 是 ASP.NET Core 中的一个容器，用于保存应用程序中使用的服务。它是配置阶段的一部分，用于注册服务。

### 2. Other DI Packages and Their Pros and Cons

- **Autofac**:
  - **Pros**: 强大、表达性强，支持多种生命周期范围。
  - **Cons**: 与 Microsoft.Extensions.DependencyInjection 相比需要更多配置代码。

- **Ninject**:
  - **Pros**: 轻量级，易于使用。
  - **Cons**: 与更全面的 DI 容器相比功能较少。

- **Unity**:
  - **Pros**: 与其他 Microsoft 技术集成良好。
  - **Cons**: 灵活性不如其他一些 DI 容器。




