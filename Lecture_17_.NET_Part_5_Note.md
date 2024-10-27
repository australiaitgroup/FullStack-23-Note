# .NET Part 5

## Description

- 本篇笔记是根据Lily老师 Lecture 17 .NET Part5 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGThdOxrN4/RFPlVZJ7s9cPhAg32W99sg/view?utm_content=DAGThdOxrN4&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table Of Content
- [1. ADO.NET](#1-adonet)
  - [1.1. ADO.NET 的主要功能](#11-adonet-的主要功能)
  - [1.2. ADO.NET 读取数据的三种方式](#12-adonet-读取数据的三种方式)
- [2. Using 语句](#2-using-语句)
  - [传统写法（不使用 using）](#传统写法不使用-using)
  - [使用 using 语句](#使用-using-语句)
  - [什么时候使用 using](#什么时候使用-using)
- [3. 参SQL 注入](#3-参sql-注入)
  - [不安全的 SQL 查询示例](#不安全的-sql-查询示例)
  - [参数化查询防止 SQL 注入](#参数化查询防止-sql-注入)
  - [保护措施](#保护措施)
- [4. 什么是 ORM (Object-Relational Mapping)](#4-什么是-orm-object-relational-mapping)
  - [示例](#示例)
- [5. Why Entity Framework?](#5-why-entity-framework)
  - [适用场景](#适用场景)
  - [不适用场景](#不适用场景)
- [6. LINQ (Language Integrated Query)](#6-linq-language-integrated-query)
  - [查询表达式语法 (类似 SQL 风格)](#查询表达式语法-类似-sql-风格)
  - [查询表达式的关键字](#查询表达式的关键字)
  - [SQL 等效查询](#sql-等效查询)
  - [方法链式调用 (Lambda 表达式)](#方法链式调用-lambda-表达式)
  - [Lambda 表达式结构](#lambda-表达式结构)
  - [示例](#示例-1)
- [7. EF 中定义和管理数据库和数据模型的三种方式](#7-ef-中定义和管理数据库和数据模型的三种方式)
- [8. Web API 应用 EF - MySQL](#8-web-api-应用-ef---mysql)
  - [1. 从 NuGet 安装 EF Core 相关包](#1-从-nuget-安装-ef-core-相关包)
  - [2. 在 .NET 项目中配置与数据库的连接](#2-在-net-项目中配置与数据库的连接)
  - [3. 创建与数据库对应的实体类](#3-创建与数据库对应的实体类)
  - [4. 创建一个继承自 DbContext 的类](#4-创建一个继承自-dbcontext-的类)
  - [5. 在 program.cs 里读取配置](#5-在-programcs-里读取配置)
  - [6. 数据库迁移 (Migration)](#6-数据库迁移-migration)
  - [7. 增删改查 (CRUD) 操作](#7-增删改查-crud-操作)
- [9. 在 program.cs 里读取配置](#9-在-programcs-里读取配置)
  - [1. 使用 Configuration.GetValue<T> 方法读取简单配置项](#1-使用-configurationgetvaluet-方法读取简单配置项)
  - [2. 使用 Configuration.GetSection 读取复杂配置](#2-使用-configurationgetsection-读取复杂配置)
  - [3. 使用 Configuration.Bind 将整个配置节绑定到对象](#3-使用-configurationbind-将整个配置节绑定到对象)
  - [4. 使用 IOptions<T> 依赖注入方式读取配置](#4-使用-ioptionst-依赖注入方式读取配置)
- [10. 异步编程](#10-异步编程)
  - [异步编程的优势](#异步编程的优势)
    - [1. 非阻塞](#1-非阻塞)
    - [2. 提高性能](#2-提高性能)
    - [3. 改善用户体验](#3-改善用户体验)


## 1. ADO.NET

ADO.NET 是 .NET 框架中的一套用于与数据库进行交互的技术和类库。它允许开发者在应用程序中执行数据库操作，如查询、插入、更新和删除数据。简单来说，ADO.NET 是 .NET 应用程序和数据库之间的 “桥梁”。

### 1.1. ADO.NET 的主要功能 

1. **建立连接**：首先使用 `Connection` 对象打开与数据库的连接。
2. **执行命令**：使用 `Command` 对象向数据库发送 SQL 语句（如 `SELECT`、`INSERT` 等）或存储过程。
3. **读取数据**：如果需要获取查询结果，可以通过 `DataReader` 一行一行地读取数据，或者用 `DataAdapter` 填充 `DataSet` 来一次性获取多行数据。
4. **关闭连接**：操作完成后，关闭连接以释放资源。

### 1.2. ADO.NET 读取数据的三种方式

1. **第一种方式（逐行读取）**：最适合处理大数据量，避免内存占用过高，但需要保持数据库连接较长时间。
2. **第二种方式（`DataTable.Load()`）**：将数据一次性加载到内存中，代码更简洁，但对大数据量可能不适合。
3. **第三种方式（`MySqlDataAdapter`）**：最为简化的方式，适合不需要频繁手动管理连接的小到中等数据量的场景，但对于大数据量也可能存在内存开销问题。

### 选择建议：

- 如果数据量较大，并且对性能要求较高，选择**第一种方式**（逐行读取）。
- 如果数据量中等且代码需要更简洁，选择**第三种方式**（`MySqlDataAdapter`）。

## 2. Using 语句

通过 `using` 语句管理数据库连接和命令，确保资源及时释放，避免内存和连接泄露。

### 传统写法（不使用 `using`）

```csharp
    MySqlConnection mySqlConnection = new MySqlConnection(connection);
    try
    {
        mySqlConnection.Open();
        // 执行一些数据库操作
    }
    finally
    {
        // 无论操作是否成功，都需要手动关闭连接
        mySqlConnection.Close();
    }
```

### 使用 `using`语句

```csharp
using (MySqlConnection mySqlConnection = new MySqlConnection(connection))
{
    mySqlConnection.Open();
    // 执行一些数据库操作
}
// `using` 会自动关闭数据库连接，无论操作成功还是失败
```

### 什么时候使用 using
using 语句主要用于管理实现了 IDisposable 接口的对象，以下是常见的使用场景：

数据库连接（如 MySqlConnection、SqlConnection）
文件操作（如 FileStream、StreamReader、StreamWriter）
网络连接（如 HttpClient）
图像处理对象（如 Bitmap）

## 3. 参SQL 注入

SQL 注入（SQL Injection）是一种网络安全攻击方式，攻击者通过在 SQL 查询中插入或“注入”恶意的 SQL 代码，来操控后台数据库系统，从而达到非授权访问、数据泄露、数据篡改或删除等目的。SQL 注入通常发生在不安全的应用程序中，这些应用程序将用户输入直接嵌入到 SQL 查询中而没有进行适当的过滤和处理。

### 不安全的 SQL 查询示例

以下是一个不安全的 SQL 查询示例，它直接将用户输入插入到查询字符串中：

```csharp
    string userInput = "123 OR 1=1";
    string query = $"SELECT * FROM Users WHERE Id = {userInput}";
```
当用户输入 "123 OR 1=1" 时，生成的 SQL 查询如下：

```csharp
    SELECT * FROM Users WHERE Id = 123 OR 1=1
```
这条查询会返回所有用户，因为 1=1 总是为真，从而导致数据泄露。

### 参数化查询防止 SQL 注入

为了防止 SQL 注入，应该使用参数化查询。参数化查询会将用户输入作为参数，而不是直接拼接到 SQL 字符串中。如下示例：
```csharp
    string query = "SELECT * FROM Users WHERE Id = @Id";
    using (SqlCommand cmd = new SqlCommand(query, connection))
    {
        cmd.Parameters.AddWithValue("@Id", userInput);
        SqlDataReader reader = cmd.ExecuteReader();
    }
```

这样，即使用户输入了恶意代码如 1 OR 1=1，也不会影响查询的安全性，因为输入被当作参数处理。

### 保护措施

使用参数化查询是防止 SQL 注入的最佳实践之一。任何涉及用户输入的 SQL 查询都应通过参数化来进行，从而确保数据安全。

## 4. 什么是 ORM (Object-Relational Mapping)

### 什么是 ORM？

ORM 是一种将数据库表映射到编程语言中的对象的技术。它允许开发者以对象的方式操作数据库，而不需要编写复杂的 SQL 查询。ORM 框架通过将数据库的行和列映射为对象的属性和方法，简化了数据库操作。

### 示例

例如，数据库中的表 `Users`：

| ID  | Name  | Email          |
| --- | ----- | ---------------|
| 1   | Alice | alice@mail.com |
| 2   | Bob   | bob@mail.com   |

传统 SQL 查询：

```sql
    SELECT * FROM Users;
```

```csharp
    var users = dbContext.Users.ToList();
```

通过 ORM，开发者可以将表中的每一行数据作为对象来操作，进一步提高了开发效率，并减少了直接编写 SQL 代码的需求。

## 5. Why Entity Framework?

在 .NET 开发中，Entity Framework（EF）是最常用的 ORM 框架之一，具有以下优势：

1. **微软官方开发和支持**
   - Entity Framework 是由微软官方开发并持续更新的 ORM 框架，能够与 .NET 平台无缝集成。
   - 得益于微软的长期支持和维护，EF 在稳定性和安全性方面表现优秀。

2. **强大的 LINQ 查询支持**
   - Entity Framework 完全支持 LINQ（Language Integrated Query），使开发者可以直接在 C# 中使用类似 SQL 的语法进行数据查询。
   - LINQ 查询具有编译时检查功能，能够有效减少错误，并且可以与复杂的对象关系映射（ORM）轻松集成。

3. **自动管理数据库连接**
   - EF 能够自动管理数据库连接的开启和关闭，减少了开发者在数据库连接管理方面的工作量。
   - 它提供了延迟加载（Lazy Loading）和预加载（Eager Loading）等机制，帮助开发者在性能和资源管理之间取得平衡。

4. **社区和生态系统支持广泛**
   - Entity Framework 拥有活跃的社区和广泛的生态系统支持，开发者可以方便地找到资源、教程和开源插件。
   - 丰富的文档和教程让新手也能快速上手，同时高级开发者也可以利用社区贡献的扩展工具和最佳实践来优化开发流程。

### 适用场景

- **快速开发**：适合中小型项目和需要快速开发的应用，EF 能够通过代码优先（Code First）和数据库优先（Database First）模式，快速生成模型和数据库表。
- **多表关联查询**：EF 的 ORM 能力特别适合处理多表关联查询的场景，通过对象模型表达复杂的表关系。
- **数据驱动应用**：适用于需要频繁进行数据增删改查的应用，例如 CRM、ERP 系统等。

### 不适用场景

- **极高性能要求的系统**：对于需要极高查询性能的场景（如高并发的金融交易系统），EF 的自动管理特性可能带来额外的性能开销。
- **复杂存储过程或特定 SQL 调优需求**：如果需要大量复杂的存储过程或自定义 SQL 优化，可能更适合使用传统的 ADO.NET 或者直接操作 SQL。

## 6. LINQ (Language Integrated Query)

LINQ（语言集成查询）是一种 .NET 提供的查询语法，允许在 C# 等编程语言中使用类似 SQL 的方式查询数据。

### 查询表达式语法 (类似 SQL 风格)

使用 LINQ 查询表达式可以书写类似 SQL 的查询，便于阅读和理解。

```csharp
    var users = from u in dbContext.Users
                where u.Age > 30
                select u;
```
### 查询表达式的关键字：

from：指定数据源（类似于 SQL 的 FROM）。
where：筛选条件（类似于 SQL 的 WHERE）。
select：选择返回的数据（类似于 SQL 的 SELECT）。
order by：对结果排序（类似于 SQL 的 ORDER BY）。
group by：按字段分组（类似于 SQL 的 GROUP BY）。

### SQL 等效查询：

```sql

    SELECT * FROM Users WHERE Age > 30;

```
### 方法链式调用 (Lambda 表达式)

Lambda 表达式提供了另一种调用 LINQ 查询的方式，适用于简洁的链式调用。

```csharp
    var users = dbContext.Users.Where(u => u.Age > 30).ToList();
```

### Lambda 表达式结构

Lambda 表达式的基本结构为：

```csharp
    (parameters) => expression
```

- parameters：输入参数（类似于函数的参数）。
- =>：称为 "lambda 操作符"，表示 “goes to”。
- expression：可以是一个表达式或一段代码块。

### 示例：

- 单参数：X => X * X
- 多参数：(x, y) => x + y
- 多行语句需要大括号 {} 并显式使用 return：

```csharp
(x, y) => { int result = x + y; return result; }
```

## 7. EF 中定义和管理数据库和数据模型的三种方式

Entity Framework 提供了三种方式来定义和管理数据库及其数据模型：

1. 代码优先 (Code-First)：通过编写 C# 类生成数据库表。
2. 数据库优先 (Database-First)：从现有数据库生成模型代码。
3. 模型优先 (Model-First)：使用图形设计工具设计模型，再生成数据库和代码。

## Web API 应用 EF - MySQL

在 .NET Web API 项目中使用 Entity Framework (EF) Core 连接 MySQL 数据库的步骤如下：

### 1. 从 NuGet 安装 EF Core 相关包

在项目中，通过 NuGet 包管理器安装以下包：

- **Microsoft.EntityFrameworkCore**：EF Core 的核心库。
- **Microsoft.EntityFrameworkCore.Tools**：用于数据库迁移和模型生成的工具包。
- **MySql.Data**：MySQL 数据库的驱动程序。
- **Pomelo.EntityFrameworkCore.MySql**：EF Core 的 MySQL 扩展包，支持 MySQL 的数据操作。

### 2. 在 .NET 项目中配置与数据库的连接

在 `appsettings.json` 文件中，配置与 MySQL 数据库的连接字符串：

```json
    {
    "Logging": {
        "LogLevel": {
        "Default": "Information",
        "Microsoft.AspNetCore": "Warning"
        }
    },
    "key1": "release",
    "DBConnection": "server=localhost;port=3306;database=myfirstdb;user=root;password=12345678"
    }
```

- server：数据库服务器地址（例如 localhost）。
- port：MySQL 服务端口号（默认 3306）。
- database：数据库名称（例如 myfirstdb）。
- user：数据库用户名。
- password：数据库密码。

配置完成后，EF Core 将能够通过 DBConnection 连接到 MySQL 数据库。

### 3. 创建与数据库对应的实体类

- 为每个数据库表创建对应的实体类（Model），这些类的属性与数据库表的列相对应。
- 实体类通常放在 `Models` 文件夹中，并使用 `[Key]` 等属性注解来定义主键。

### 4. 创建一个继承自 DbContext 的类

- 创建一个继承自 `DbContext` 的类（例如 `AppDbContext`），用于管理实体类与数据库表之间的映射关系。
- 在 `DbContext` 类中定义 `DbSet<TEntity>` 属性来表示数据库表，例如：

```csharp
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<User> Users { get; set; }
    }
```

### 5. 在 program.cs 里读取配置

在 program.cs 文件中配置 DbContext，读取 appsettings.json 中的数据库连接字符串，并注入 DbContext。
```csharp
    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddDbContext<AppDbContext>(options =>
        options.UseMySql(builder.Configuration.GetConnectionString("DBConnection"), 
        ServerVersion.AutoDetect(builder.Configuration.GetConnectionString("DBConnection"))));
```

### 6. 数据库迁移 (Migration)

使用迁移功能来同步模型和数据库结构。

第一步：添加初始迁移

```bash
dotnet ef migrations add InitialMigration
```

第二步：更新数据库

```bash
dotnet ef database update
```

这会在数据库中创建表结构，与模型中的实体类匹配。

### 7. 增删改查 (CRUD) 操作

在控制器中使用 DbContext 实现 CRUD 操作。

以上步骤完成了使用 EF Core 操作 MySQL 数据库的基本流程，包括实体类创建、DbContext 配置、数据库迁移和 CRUD 操作。

## 8. 在 `program.cs` 里读取配置

在 .NET 项目中，可以通过 `Configuration` 对象在 `program.cs` 中读取应用的配置项。以下是常见的几种读取方式：

### 1. 使用 `Configuration.GetValue<T>` 方法读取简单配置项

- 可以直接使用 `GetValue<T>` 方法读取基本配置项。

```csharp
var mySetting = builder.Configuration.GetValue<string>("MySetting");
```

### 2.  使用 Configuration.GetSection 读取复杂配置

使用 GetSection 方法读取复杂配置节，并从中获取多个子项的值。

```csharp
var jwtSection = builder.Configuration.GetSection("JwtConfig");
var issuer = jwtSection.GetValue<string>("Issuer");
var audience = jwtSection.GetValue<string>("Audience");
```

### 3. 使用 Configuration.Bind 将整个配置节绑定到对象

可以使用 Bind 方法将配置节与自定义对象绑定，便于管理整个配置结构。

```csharp
var myConfig = new MyConfig();
builder.Configuration.Bind("MyConfigSection", myConfig);
```

### 4. 使用 IOptions<T> 依赖注入方式读取配置
通过 IOptions<T> 接口将配置项注入到服务中，便于在应用中使用依赖注入访问配置。

```csharp
    public class MyService
    {
        private readonly MyConfig _config;

        public MyService(IOptions<MyConfig> config)
        {
            _config = config.Value;
        }
    }
```

这种方式有助于在应用各个部分中访问配置数据，并保持代码的松耦合。

## 9. 异步编程

异步编程是一种编程范式，它允许程序在等待耗时操作（如 I/O 操作、网络请求、数据库查询等）完成时，不阻塞主线程，从而提高程序的性能和响应速度。

## 异步编程的优势

### 1. 非阻塞

异步方法在等待外部资源（如数据库、文件、网络请求等）的响应时，不会阻塞当前线程，从而允许其他任务继续执行。这对提升应用程序的响应速度和效率非常重要，尤其是在 I/O 密集型操作中。

### 2. 提高性能

异步编程在处理高并发的场景时能极大地提高性能。通过使用异步操作，程序可以在等待数据库响应的同时处理其他请求或任务，避免 CPU 空转。

### 3. 改善用户体验

在 Web 应用程序中，异步编程可以避免长时间等待操作（如数据库查询、文件上传等）导致的界面卡顿，从而提升用户体验。

