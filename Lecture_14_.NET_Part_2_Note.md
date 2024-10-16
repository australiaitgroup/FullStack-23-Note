# .NET Part 2

## Description

- 本篇笔记是根据Peng Dai老师 Lecture 14 .NET Part2 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGJ9S-wrPY/pY2GhOixAwRrXOejBzpbSg/view?utm_content=DAGJ9S-wrPY&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table of Contents

- [1. 什么是面向对象](#1-什么是面向对象)
  - [面向对象的定义](#面向对象的定义)
  - [基本概念](#基本概念)
  - [面向对象的优点](#面向对象的优点)
  - [示例解说](#示例解说)
  - [系统程序设计的各个阶段](#系统程序设计的各个阶段)
  - [面向对象的优势](#面向对象的优势)
- [2. 面向对象的三大特性](#2-面向对象的三大特性)
  - [2.1. 封装（Encapsulation）](#21-封装encapsulation)
  - [2.2. 继承（Inheritance）](#22-继承inheritance)
  - [关于继承的其他注意事项](#关于继承的其他注意事项)
  - [2.3. 多态（Polymorphism）](#23-多态polymorphism)
- [3. 构造函数（Constructor）](#3-构造函数constructor)
  - [默认构造函数](#默认构造函数)
  - [自定义构造函数](#自定义构造函数)
  - [构造函数重载 : this()](#构造函数重载--this)
  - [子类/基类构造函数的关系和调用顺序](#子类基类构造函数的关系和调用顺序)
- [4. 访问控制（Access Modifiers）](#4-访问控制access-modifiers)
  - [4.1. `public`](#41-public)
  - [4.2. `private`](#42-private)
  - [4.3. `protected`](#43-protected)
  - [4.4. `internal`](#44-internal)
  - [4.5. `protected internal`](#45-protected-internal)
  - [4.6. `private protected`](#46-private-protected)
  - [封装与访问控制](#封装与访问控制)
- [5. Static 类](#5-static-类)
  - [静态类的特性](#静态类的特性)
  - [静态类的用途](#静态类的用途)
    - [共享数据和方法](#共享数据和方法)
    - [自定义扩展方法](#自定义扩展方法)
  - [总结](#总结)
- [6. Sealed 方法和类](#6-sealed-方法和类)
  - [6.1. 封闭方法（Sealed Method）](#61-封闭方法sealed-method)
  - [6.2. 封闭类（Sealed Class）](#62-封闭类sealed-class)
  - [总结](#总结-1)

## 1. 什么是面向对象

### 面向对象的定义

### 基本概念

- 面向对象编程（OOP）是一种程序设计思想，核心思想是“一切皆对象”。
- 面向对象将对象作为程序的基本单元，每个对象包含数据和操作数据的函数。
- 面向对象设计强调通过先设计组件，再组装整体系统。

### 面向对象的优点

- 程序设计的基本单元是对象，更符合人类的思维方式。
- 比较于面向过程的编程，面向对象更容易重用、理解、维护和扩展。

### 示例解说

- 通过将大象装入冰箱的例子来解释：
  - 将大象类定义为包含`eHeight`（大象的高度）属性。
  - 冰箱类定义为包含`rHeight`（冰箱的高度）属性，以及`打开冰箱()`和`关闭冰箱()`等行为。
  - 操作类包含将大象放入冰箱的操作行为。

### 系统程序设计的各个阶段

1. **系统分析阶段 OOA**：抽象同类对象的共性定义为类。
2. **系统设计阶段 OOD**：设计出符合业务需求的类和接口。
3. **实现和编程阶段 OOP**：将分析和设计阶段的类翻译成代码。

### 面向对象的优势

- 代码的重用性高，易于理解，维护性强，扩展性好。

## 2. 面向对象的三大特性

### 2.1. 封装（Encapsulation）

- 将相关信息聚集在一起，隐藏实现细节。

- 封装通过将代码和操作的数据绑定在一起，使外部无法直接干扰和滥用。

- **优点**：隐藏复杂性，使代码更具安全性和可维护性。

### 示例代码
```csharp
public class Encapsulation {
    private int x;
    
    public Encapsulation(int iX) {
        this.x = iX;
    }
    
    public void MySquare() {
        int calc = x * x;
        Console.WriteLine(calc);
    }
}
```

### 2.2. 继承（Inheritance）

- 表示类之间的父子关系，通过继承实现代码复用和扩展。

- IS-A关系：子类继承父类的特性和方法。

- 继承的限制：

  - C#和.NET只支持单继承。

  - 类的构造函数不能被继承。

  - 访问控制符（private/protected/public）依然有效。

  - 子类无法访问父类的private成员。

### 示例代码

```csharp
public class Parent {
    public void Method1() {
        // Some code here
    }
}

public class Child : Parent {
    public void Method2() {
        // Some code here
    }
}
```

### 关于继承的其他注意事项

- 单继承：C#和.NET仅支持单继承，但可通过接口实现多重继承效果。
- 传递继承：例如D继承C，C继承B，B继承A。
- 访问控制：private成员无法被子类访问，需注意父类中abstract和virtual方法的使用。
- 基类的公共基类：object是所有类的基类，sealed类不能被继承。

### 2.3. 多态（Polymorphism）

- 相同方法在不同场景中的不同实现。
- 方法重载（Overload）：编译期多态，通过定义多个同名方法但参数不同实现。
- 方法重写（Override）：运行期多态，子类在继承父类方法的基础上提供特定实现。

### 示例代码

```csharp
public abstract class Employee {
    public virtual void LeaderName() {
        Console.WriteLine("Leader");
    }
}

public class HrDepart : Employee {
    public override void LeaderName() {
        Console.WriteLine("Mr. Jones");
    }
}

public class ItDepart : Employee {
    public override void LeaderName() {
        Console.WriteLine("Mr. Tom");
    }
}
```

### 运行时多态

- 隐藏基类方法：使用new关键字替换基类的成员。
- 重写基类方法：防止派生类重写虚拟成员。
- 停止虚拟继承：可以通过sealed关键字防止进一步的重写。

### 示例代码

```csharp
public class A {
    public virtual void DoWork() {
        // Some work
    }
}

public class B : A {
    public override void DoWork() {
        // Some different work
    }
}

public class C : B {
    public sealed override void DoWork() {
        // Final implementation, cannot be overridden
    }
}
```
## 3. 构造函数（Constructor）

用来实例化对象，使类生成的对象具备初始状态。
  
### 默认构造函数

- 编译器自动生成一个无参构造函数，适用于没有自定义构造函数的类。
- 如果类中没有定义任何构造函数，编译器会默认提供一个隐式无参构造函数。
  
### 自定义构造函数

- 当需要指定参数或初始化逻辑时，可以手动定义构造函数。
- 自定义构造函数可以覆盖默认构造函数，此时编译器不会再自动生成无参构造函数。
  
### 构造函数重载 : this()

- 一个类可以有多个构造函数，通过重载实现不同参数组合的构造。
- 使用`this()`关键字可以在一个构造函数内调用另一个构造函数，从而减少代码重复。
  
### 示例

```csharp
public class Person {
    public string Name;
    public int Age;
    public string Address;
    
    public Person() {
        // 默认构造函数
    }
    
    public Person(string name, int age) : this(name, age, "Unknown") {
        // 调用带三个参数的构造函数
    }
    
    public Person(string name, int age, string address) {
        this.Name = name;
        this.Age = age;
        this.Address = address;
    }
}
```

### 子类/基类构造函数的关系和调用顺序

- 当子类继承基类时，子类的构造函数会首先调用基类的构造函数，以确保基类部分正确初始化。
- 子类构造函数可以显式调用基类构造函数，使用base()关键字传递参数。
- 如果基类有参数化构造函数，子类的构造函数必须显式调用基类构造函数并传递参数，否则编译会报错。

### 示例代码

```csharp
public class Person {
    public string Name;
    public int Age;
    
    public Person(string name, int age) {
        this.Name = name;
        this.Age = age;
    }
}

public class Student : Person {
    public double Salary;
    
    public Student(string name, int age, double salary) : base(name, age) {
        this.Salary = salary;
    }
}
```
## 4. 访问控制（Access Modifiers）

访问控制修饰符用于控制类成员的可见性，为每个类和类成员提供安全级别。它们是实现封装的重要手段，可以保护用户的敏感数据不被外部直接访问。

### C#中的访问控制修饰符

### 4.1. `public`

- 代码可以被所有类访问，没有访问限制。
- 适用于需要在项目各处访问的类和成员。

### 4.2. `private`

- 代码只能在同一个类中访问。
- 默认的访问修饰符，用于保护类的内部实现，不被外部类或对象直接访问。
  
### 4.3. `protected`

- 代码只能在同一个类或该类的派生类中访问。
- 适用于希望子类能够访问，但不希望其他非相关类访问的成员。

### 4.4. `internal`

- 代码仅在同一程序集（Assembly）内可以访问，不可跨程序集访问。
- 通常用于项目内部访问，防止外部程序集访问特定类和成员。

### 4.5. `protected internal`

- 代码只能在同一程序集内访问，或在派生类中访问。
- 结合了`protected`和`internal`的特性，用于允许程序集内和继承链中的访问。

### 4.6. `private protected`

- 代码只能在同一个类和派生类中访问，但访问者必须在同一程序集内。
- 限制比`protected`更严格，不允许跨程序集的派生类访问。

### 示例

```csharp
public class Example {
    public int PublicValue;          
    // 所有类都可以访问
    private int PrivateValue;        
    // 仅限当前类访问
    protected int ProtectedValue;    
    // 当前类和派生类可以访问
    internal int InternalValue;      
    // 当前程序集内可以访问
    protected internal int ProtIntValue; 
    // 派生类和当前程序集内可以访问
    private protected int PrivProtValue; 
    // 同程序集的派生类可访问
}
```

### 封装与访问控制

- 通过使用private修饰符，类成员可以被隐藏，避免被外部类或对象直接访问。
- 结合访问修饰符，可以根据需求对类成员进行权限管理，提高代码安全性与可维护性。

## 5. Static 类

静态类用于包含静态属性和方法，通常用于共享数据和提供通用的功能方法。静态类无法被实例化或继承。

### 静态类的特性

- 只能包含静态成员（属性和方法），标识为`static`。
- 无法实例化，不能创建对象。
- 不能被继承，因为静态类不能作为其他类的基类。

### 静态类的用途

1. **共享数据和方法**

   - 静态类适合用于定义共享的数据和方法，提升代码的重用性。
   - 例如：定义通用的`Helper`类或者提供数学运算功能的`Math`类。

   ### 示例

   ```csharp
   public static class Helper {
       public static void Log(string message) {
           Console.WriteLine(message);
       }
   }
   
   public static class MathHelper {
       public static int Add(int a, int b) {
           return a + b;
       }
   }
   ```

2. **自定义扩展方法**

   - 静态类可以包含扩展方法，为现有类型添加新方法，而无需修改原始类型。

   - 扩展方法使用this关键字指向扩展的类型，第一个参数必须是要扩展的类型。

   ### 示例

   ```csharp
    public static class EnumerableExtensions {
        public static int CountWords(this string str) {
            return str.Split(new char[] { ' ' }, StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }
   ```
### 总结

- 静态类通过静态成员和扩展方法，提供了功能性的代码模块，适合用于定义工具类和扩展方法。

- 由于静态类不能实例化或继承，因此所有成员必须为静态类型，且只能通过类名直接访问。

## 6. Sealed 方法和类

### 6.1. 封闭方法（Sealed Method）
- `sealed`关键字可以用于方法，以防止子类对该方法进行重写。
- 如果类继承自其他基类，可以使用`sealed`与`override`一起使用，来对基类的重写方法进行封闭。
  
### 示例
```csharp
public class BaseClass {
    public virtual void PrintName() {
        Console.WriteLine("BaseClass PrintName");
    }
}

public class DerivedClass : BaseClass {
    public override sealed void PrintName() {
        Console.WriteLine("DerivedClass PrintName");
    }
}

public class GrandDerivedClass : DerivedClass {
    // 无法重写 PrintName 方法，因为它在 DerivedClass 中被封闭
    public void Print() {
        base.PrintName(); // 仍可调用基类的实现
    }
}
```

### 6.2. 封闭类（Sealed Class）
- 使用sealed关键字修饰类可以防止该类被其他类继承。
- 封闭类用于确定类的继承层次结构，确保该类不能作为基类。

### 示例
```csharp
public sealed class FinalClass {
    public void Show() {
        Console.WriteLine("FinalClass Show");
    }
}

// 尝试继承 FinalClass 会导致编译错误
// public class DerivedFromFinal : FinalClass {
//     // 编译错误：无法从密封类 FinalClass 派生
// }
```

### 总结
- 封闭方法可以防止子类重写，提供方法行为的最终定义。
- 封闭类可以防止进一步的继承，适用于工具类或核心业务类。