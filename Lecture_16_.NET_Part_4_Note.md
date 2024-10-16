# .NET Part 3

## Description

- 本篇笔记是根据Lily老师 Lecture 16 .NET Part4 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGJ9a1PzeA/tIAXsG9WfChl9qCe6YJkcA/view?utm_content=DAGJ9a1PzeA&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table Of Content

## 1. Web API 相关技术

### 1.1. 泛型（Generic）

我们在编程程序时，经常会遇到功能非常相似的模块，只是它们处理的数据不一样。但我们没有办法，只能分别写多个方法来处理不同的数据类型。这个时候，那么问题来了，有没有一种办法，用同一个方法来处理传入不同种类型参数的办法呢？泛型的出现就是专门来解决这个问题的。

### 1.1.1. 泛型的引入

泛型（Generic）允许我们使用一个方法来处理不同的数据类型。这种方法通过泛型参数 `<T>` 的方式定义，避免了重复代码，并提高了代码的灵活性。

### 示例代码

非泛型方法：
```csharp
    public void PrintInt(int number) {
        Console.WriteLine(number);
    }

    public void PrintString(string message) {
        Console.WriteLine(message);
    }
```

泛型方法：
```csharp
public void Print<T>(T value) {
    Console.WriteLine(value);
}
```

使用示例：
```csharp
Print(123);        // 打印整数
Print("Hello!");   // 打印字符串
```
通过泛型方法 Print<T>, 我们可以传入任意类型的参数，从而实现代码的重用。

这种方式使得代码更加简洁、易于维护，同时避免了代码重复。

### 1.1.2. C# 泛型语法

### 泛型的类型
- **泛型类**
- **泛型方法**
- **泛型接口**
- **泛型约束**
- **泛型委托**（委托将单独学习）

### 常见的泛型约束类型
- `where T : struct`：约束 T 必须是值类型。
- `where T : class`：约束 T 必须是引用类型。
- `where T : new()`：约束 T 必须有无参构造函数。
- `where T : BaseClass`：约束 T 必须继承自某个类。
- `where T : interface`：约束 T 必须实现某个接口。
- **多个约束**：可以结合多个约束，要求类型参数同时满足多个条件。

### 1.2. .NET 内置的泛型集合

### List<T>
- **作用**：List<T> 是一个动态数组（列表），用于存储同一类型的对象。
- **特点**：与数组不同，List<T> 的大小是动态的，可以根据需要自动增长。

### Queue<T>
- **作用**：Queue<T> 是一个先进先出（FIFO）的数据结构。
- **特点**：元素按照加入的顺序排列，第一个加入的元素最先被移除。

### Dictionary<TKey, TValue>
- **作用**：Dictionary<TKey, TValue> 是一个键值对集合，允许根据键快速查找值。
- **特点**：每个键必须唯一，不能重复。

### IEnumerable<T>
- **作用**：IEnumerable<T> 是大部分集合类型（如 List<T>、Array 等）的基接口。
- **特点**：提供遍历集合的能力。

### 1.3. 依赖注入 (Dependency Injection, DI) & 控制反转 (Inversion of Control, IoC)

.NET Core 的依赖注入是一种帮助程序管理对象之间依赖关系的机制，它让代码更灵活、更好维护、更容易测试。

### 核心思想
- **不要让类自己创建所需的对象**，而是通过外部将这些对象提供给它。
- 控制反转 (IoC) 表达了将对象的创建和依赖的绑定交由框架或外部组件管理的思想。

### 1.4. 依赖注入在 Web API 中的应用

在 Web API 应用中，`UserService` 是负责管理和操作用户数据的业务逻辑服务。它使得代码结构更清晰，实现了控制器 (Controller) 和业务逻辑的职责分离，有助于代码复用。

### ASP.NET Core Web API 中的依赖注入步骤
1. **注册服务**：在 `Program.cs` 文件中注册 `UserService`。
2. **注入服务**：在控制器中通过依赖注入使用 `UserService`。
3. **调用服务**：通过 HTTP 请求调用 API，执行 `UserService` 中的相关业务逻辑。

### 1.5. 依赖注入的生命周期

依赖注入的生命周期管理指的是在依赖注入中，服务对象的创建时间和销毁时间如何被管理。在 ASP.NET Core 中，依赖注入容器提供了三种常见的生命周期管理方式：

1. **瞬时 (Transient)**：每次请求服务时都会创建一个新的实例。
2. **作用域 (Scoped)**：在每个请求中创建一个实例，在请求结束时销毁。
3. **单例 (Singleton)**：整个应用程序中只创建一个实例，并在应用程序的生命周期内共享。

### 1.6. Scoped 作用域示例

在 ASP.NET Core 中，依赖注入的作用域生命周期 (Scoped) 意味着在每个请求中创建一个服务实例，且该实例在整个请求周期内保持一致。以下示例展示了如何在控制器中使用 `UserService`，并通过构造函数注入。

```csharp
    public class UserController : ControllerBase
    {
        private readonly UserService _userService;

        public UserController(UserService userService)
        {
            _userService = userService;
        }

        [HttpGet("{userName}")]
        public IActionResult GetUser(string userName)
        {
            var user = _userService.GetUserByName(userName);
            return Ok(user);
        }
    }
```
- 构造函数注入：UserController 接收 UserService 对象并将其赋值给私有只读字段 _userService。
- 方法调用：GetUser 方法使用 _userService 调用业务逻辑层方法 GetUserByName，获取用户信息并返回响应。

这种结构在整个 HTTP 请求生命周期内复用相同的 UserService 实例，确保请求的一致性。

### 1.7. Transient 瞬时

在瞬时 (Transient) 生命周期中，每次请求服务时都会创建一个新的实例。这意味着：
- 每次请求，不论是在同一 HTTP 请求期间还是在不同的 HTTP 请求之间，瞬时服务都会重新实例化。
- 适用于不需要共享状态的轻量级服务。

瞬时生命周期适合短期任务或无状态的服务实例，避免了实例之间的数据共享。

### 1.8. Singleton 单例

单例模式 (Singleton) 表示服务在整个应用程序的生命周期中只创建一个实例，并且所有请求和服务都共享该实例。
- **特性**：无论有多少个 HTTP 请求或服务需要使用该服务，都将使用同一个对象。
- **应用场景**：在 ASP.NET Core 中，将服务注册为 Singleton 时，服务实例会在应用程序启动时创建，且只会创建一次，之后在整个应用程序的生命周期内都被复用。

Singleton 适用于需要在应用全局范围内共享的服务，例如配置管理器或日志记录器。

## 2. .NET 项目中的Web API 相关应用

### 2.1. 数据库 - 关系型数据库

数据库是存储、管理和组织数据的系统，允许用户以结构化的方式访问、操作和维护数据。

### 数据库类型
- **关系型数据库 (RDBMS)**：数据以表格形式存储，表与表之间通过键关联。常见的 RDBMS 包括 MySQL、SQL Server、Oracle 等，主要通过 SQL 语言操作。

### 基本概念
- **表 (Table)**：数据库的基本单位，包含行（记录）和列（字段）。每个表代表特定的数据实体，列定义属性，行则是每条数据的具体值。
- **主键 (Primary Key)**：表中唯一标识一条记录的字段，不能有重复值，且不能为 NULL。
- **外键 (Foreign Key)**：用于建立表之间的关系。外键是另一个表的主键，确保数据库的完整性。

### SQL (结构化查询语言)
用于访问和管理关系型数据库的语言，常用操作包括：
- **SELECT**：查询数据
- **INSERT**：插入数据
- **UPDATE**：更新数据
- **DELETE**：删除数据

### 2.2. MySQL 服务端与客户端


### 2.2.1. MySQL 服务端 (MySQL Server)
MySQL 服务端是负责管理和维护数据库的核心组件。它的主要任务包括：
- **数据存储**：将数据存储在数据库中，并确保数据的安全性和完整性。
- **数据管理**：处理数据的插入、查询、更新和删除等操作。
- **访问控制**：管理用户权限，确保只有授权用户才能访问或操作数据库。
- **连接管理**：处理多个客户端的连接请求，支持高并发。
- **事务处理**：支持事务操作，确保数据的一致性和可靠性。

服务端需要在服务器上安装，启动后可以监听指定的端口（默认是 3306），等待客户端的连接。

### 2.2.2. MySQL 客户端 (MySQL Client)
MySQL 客户端是用户与 MySQL 服务端交互的工具。它允许用户通过命令行或图形界面（如 MySQL Workbench）访问和操作数据库。客户端的主要功能包括：
- **连接到服务器**：通过 IP 地址和端口连接到 MySQL 服务端。
- **执行 SQL 语句**：发送 SQL 语句给服务端，服务端执行后返回结果。
- **管理数据库**：创建、删除数据库和表，管理数据结构。
- **数据操作**：执行查询、插入、更新和删除数据等操作。
- **用户管理**：创建和管理数据库用户及其权限。

客户端可以安装在用户的电脑上，也可以通过网络远程连接到 MySQL 服务端。

### 2.2.3. 常见 MySQL 客户端工具
- **MySQL Command Line Client**：官方提供的命令行工具，用于直接输入 SQL 语句。
- **MySQL Workbench**：图形化工具，提供更加直观的数据库管理界面。
- **phpMyAdmin**：基于 Web 的管理工具，通常用于管理 Web 项目的数据库。
- **DBeaver、HeidiSQL** 等第三方工具：提供跨平台支持和丰富的功能。

### 示例：连接到 MySQL 服务端
使用命令行客户端连接到 MySQL 服务端的示例：
```bash
    mysql -h localhost -u root -p
```

### 2.3. NuGet - .NET 包管理工具

### 2.3.1. 什么是 NuGet？
NuGet 是 .NET 平台的包管理工具，主要用于管理和分发软件包（Packages）。它使得开发者能够轻松地将第三方库、框架或工具集成到项目中，同时也可以创建并发布自己的包供其他开发者使用。NuGet 是 .NET 开发生态系统中标准的包管理工具。

### 2.3.2. NuGet 的主要功能
- **包查找与安装**：开发者可以通过 NuGet 从在线库（NuGet Gallery）中查找和安装所需的包。
- **自动管理依赖项**：在安装包时，NuGet 会自动下载并安装该包所依赖的其他包。
- **包更新**：NuGet 支持更新已安装的包到最新版本，确保项目使用最新的功能和安全补丁。
- **包卸载**：当包不再需要时，可以通过 NuGet 移除包及其相关依赖项。
- **本地缓存与离线安装**：NuGet 可以在本地缓存包，以支持离线开发环境。

### 2.3.3. NuGet 的核心概念
- **包 (Package)**：包含库、工具或其他内容的压缩文件。通常包括 DLL 文件、依赖项信息、描述和其他元数据。
- **包源 (Package Source)**：NuGet 下载包的源，通常是 NuGet 官方的在线库，也可以是企业内部的私有包源。
- **`NuGet.config`**：配置文件，用于指定包源、缓存位置和其他设置。
- **`.nuspec` 文件**：包含包的元数据，如名称、版本、作者等信息，用于创建和发布 NuGet 包。

### 2.3.4. 常用命令
NuGet 提供了命令行工具 `nuget.exe` 和 Visual Studio 内置的 NuGet 包管理器。以下是一些常用的 NuGet 命令：
- **安装包**：
```bash
    nuget install <package_name>
```

### 2.3.5. 使用 NuGet 管理包的流程
- 查找包：在 NuGet Gallery 网站或 Visual Studio 中搜索所需的包。
- 安装包：在项目中添加包引用，NuGet 会自动下载和配置包的依赖项。
- 更新包：根据需要更新包，以使用最新版本。
- 移除包：如果不再需要某个包，可以选择卸载它。
### 在 Visual Studio 中使用 NuGet
在 Visual Studio 中，NuGet 包管理器提供了图形界面，可以通过以下步骤使用：

- 右键点击项目，选择“管理 NuGet 包”。
- 在“浏览”选项卡中搜索所需的包并选择安装。
- 查看“已安装”选项卡，以更新或移除现有包。
### 总结
- NuGet 是 .NET 的包管理工具，支持查找、安装、更新和发布包。
- 它简化了依赖项管理，帮助开发者更高效地集成和管理外部库。
NuGet 集成于 Visual Studio，也可以通过命令行工具 nuget.exe 使用。
- NuGet 是 .NET 开发中不可或缺的工具，极大地提高了代码复用和开发效率。