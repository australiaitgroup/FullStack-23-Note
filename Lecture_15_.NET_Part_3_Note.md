# .NET Part 3

## Description

- 本篇笔记是根据Lily老师 Lecture 15 .NET Part3 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGJ9ZkYE3A/g-Gmd68WqSl0U3HmxWP1hg/view?utm_content=DAGJ9ZkYE3A&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table of Contents
- [1. API的概念 与 Web API](#1-api的概念-与-web-api)
  - [1.1. 什么是 API？ API 有什么作用？](#11-什么是-api-api-有什么作用)
  - [1.2. Web API 是什么？ Web API 与 API 有什么关联？](#12-web-api-是什么-web-api-与-api-有什么关联)
  - [1.3. HTTP 请求类型](#13-http-请求类型)
  - [1.4. HTTP 状态码分类](#14-http-状态码分类)
- [2. .NET 项目中的Web API](#2-net-项目中的web-api)
  - [2.1. Web API 项目结构](#21-web-api-项目结构)
  - [2.2. 管道 中间件](#22-管道-中间件)
  - [2.3. 中间件的处理顺序](#23-中间件的处理顺序)
  - [2.4. MVC 架构模式](#24-mvc-架构模式)
  - [2.5. 模型 & 控制器](#25-模型--控制器)
  - [2.6. 控制器（Controller） 和 Action（动作方法）](#26-控制器controller-和-action动作方法)
  - [2.7. Web API 常用返回类型——JsonResult](#27-web-api-常用返回类型jsonresult)
  - [2.8. 路由（Routing） & 属性路由（Attribute Routing）](#28-路由routing--属性路由attribute-routing)
  - [2.9. 路由传值](#29-路由传值)
  - [2.10. Postman](#210-postman)
  - [2.11. REST API](#211-rest-api)
  - [2.12. 模型验证（Model Validation）](#212-模型验证model-validation)
  - [2.13. 统一返回](#213-统一返回)
  - [2.14. 过滤器 - Filters](#214-过滤器---filters)
    - [过滤器的执行顺序](#过滤器的执行顺序)
  - [Aspect-Oriented Programming, AOP](#aspect-oriented-programming-aop)

## 1. API的概念 与 Web API

### 1.1. 什么是 API？ API 有什么作用？
...

### 1.2. Web API 是什么？ Web API 与 API


## 1. API的概念 与 Web API

### 1.1. 什么是 API？ API 有什么作用？

API 全名为 Application Programming Interface，而API 中文则是「应用程序介面」，是一种提供不同软件系统间互动的工具，定义了不同软件间的互动规范。 API允许不同的应用序、服务或系统之间能够共享信息与功能，以约定好的 API 接口实现互联互通。

简单来说，API提供了一组定义好的规则与协议，通过信息的传递，让开发者可以轻松整合与使用不同的服务，从而提高应用程序的功能与效能。

![what is api](/assets/images/api.png)

### 1.2. Web API 是什么？ Web API 与 API 有什么关联？

Web API 是 API 的一种类型，主要针对浏览器的API，它通过HTTP 协议在不同系统间进行数据交换，在 .NET中，ASP.NET提供了强大的工具来帮助开发者快速构建 Web API 服务。

### Web api 工作原理

![web api](/assets/images/webapi.png)

### 1.3. HTTP 请求类型

HTTP 请求类型（或称为 HTTP 方法）是指客户端在请求服务器时，想要执行的操作。常见的请求类型包括：

### GET
- **功能**：从服务器获取资源。通常用于读取数据，不会修改服务器的数据。
- **例子**：获取一个用户的信息：`GET /users/123`
- **说明**：GET 请求不会带有请求体，只是从服务器获取数据。

### POST
- **功能**：向服务器发送数据，通常用于创建新的资源。
- **例子**：创建一个新用户：`POST /users`
- **说明**：POST 请求会带有请求体，包含需要提交的数据，如用户信息等。

### PUT
- **功能**：更新服务器上的资源，通常是用新的数据替换现有资源。
- **例子**：更新用户信息：`PUT /users/123`
- **说明**：PUT 请求一般要求提交完整的资源数据，即使只修改一部分，也需要提交整个资源。

### DELETE
- **功能**：从服务器删除资源。
- **例子**：删除一个用户：`DELETE /users/123`
- **说明**：DELETE 请求通常不带有请求体，直接删除指定的资源。

### 1.4. HTTP 状态码分类

### 2xx 表明请求成功
- **200 OK**：一切都好，一切都很成功。
- **201 已创建**：与 200 类似，但衡量成功的标准是创建了新资源。

### 4xx 客户端错误
状态码表示客户端有错误。该错误通常会在响应中显示。
- **400 请求错误**：客户端请求有问题。可能格式不正确、无效或太大，或服务器无法理解请求。
- **401 未授权**：客户端在需要时没有识别或验证自己。
- **403 阻止访问**：客户端已知但没有访问权限。
- **404 未找到**：未找到请求的资源。

### 5xx 服务器端错误
- **500 内部服务器错误**：服务器遇到某种问题，并且没有更好或更具体的错误代码。

## 2. .NET 项目中的Web API

### 2.1. Web API 项目结构

### Controllers 文件夹
- 用于存放 Web API 的控制器（Controller）类，定义应用程序的路由和处理请求的方法。

### Program.cs 文件
- 项目的启动点，ASP.NET Core 从这里开始运行应用程序。配置应用的服务和请求管道。
- 代码示例：

    ```csharp
    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddControllers();
    var app = builder.Build();
    ```

### appsettings.json 和 appsettings.Development.json

- 应用程序的配置文件，用于存储各种配置信息，例如数据库连接字符串、日志记录设置等。

### Dependencies（依赖项）

- 项目所需的外部库和框架的引用。
- 主要依赖项示例：
  - Microsoft.AspNetCore.App
  - Microsoft.NETCore.App

### 2.2. 管道 中间件

管道和中间件是 ASP.NET Core 中处理 HTTP 请求的关键概念。它们的作用是帮助 Web 应用按步骤处理客户端发来的请求，并决定如何返回响应。

- **管道（Pipeline）**：管道定义了请求从服务器到达应用后，如何被一步步处理，以及如何生成最终的响应。
- **中间件（Middleware）**：管道中的每个处理单元，负责完成某种特定功能，例如身份验证、日志记录或错误处理。

### 2.3. 中间件的处理顺序

![middleware](/assets/images/middleware.png)

### 2.4. MVC 架构模式

MVC 是三个单词的首字母缩写，分别代表 Model（模型）、View（视图）和 Controller（控制器）。MVC 模式用于将应用程序的不同部分分离，以提高代码的可维护性和扩展性。

### 1. Model（模型）
- **功能**：负责处理与数据相关的所有逻辑，如定义数据结构、持久化数据和业务逻辑处理。
- **作用**：与数据库交互、进行数据验证、将数据通知给 Controller 和 View。

### 2. View（视图）
- **功能**：显示数据的用户界面部分，通常是从 Model 中获取数据并以模板或页面的形式展现。
- **作用**：接收 Controller 的数据，并将其格式化呈现给用户，提供数据的视觉表现。

### 3. Controller（控制器）
- **功能**：作为 Model 和 View 之间的桥梁，接收用户的请求，并根据请求执行相应的业务逻辑。
- **作用**：从 Model 中获取数据，将数据传递给 View，并返回结果给用户。

### MVC 工作流程
1. 用户通过 View 发送请求。
2. Controller 处理用户请求，并从 Model 中获取数据。
3. Controller 将获取的数据传递给 View。
4. View 将数据格式化，并将结果返回给用户。

这种模式使得应用程序的逻辑层、数据层和表现层分离，使得每一层都可以独立修改而不影响其他层。

### 2.5. 模型 & 控制器

### 模型（Model）
- 数据库中的表结构映射到程序中的类对象，Model 类表示了数据库中的数据结构。
- 例如，数据库中的 `Users` 表定义如下：

  ```sql
    CREATE TABLE Users (
        Id INT PRIMARY KEY,
        Name NVARCHAR(50),
        Email NVARCHAR(100)
    );
  ```
- 对应的 C# Model 类可以定义为：

    ``` csharp
    public class User {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Email { get; set; }
    }
    ```
### 2.6. 控制器（Controller） 和 Action（动作方法）
控制器：是应用程序中专门用来处理请求的类。它是用户与系统的中介，接收用户的请求，处理数据并返回结果。

动作方法：是控制器类中的一个方法，每个方法代表一个特定的请求操作。

### 示例
    ```csharp
    public class UsersController : Controller {
        public IActionResult Index() {
            // 处理并返回用户列表
        }
        
        public IActionResult Details(int id) {
            // 根据 id 获取用户详情并返回
        }
    }
    ```

控制器和动作方法通过路由来处理特定的 URL 请求，连接前端请求与后端数据逻辑。

### 2.7. Web API 常用返回类型——JsonResult

`JsonResult` 是 ASP.NET Core 中用于返回 JSON 格式数据的一种返回类型。

### JsonResult 的作用
- **功能**：将服务器端的对象或数据结构序列化为 JSON 格式，并将其作为响应返回给客户端。
- **使用场景**：主要用于 Web API 开发中，特别是在需要返回数据给 JavaScript 或前端框架（如 React、Angular）的场景中。

### 示例代码
```csharp
public IActionResult GetUserData() {
    var user = new User {
        Id = 1,
        Name = "John Doe",
        Email = "john.doe@example.com"
    };
    return Json(user);
}
```
在这个示例中，JsonResult 返回 User 对象的 JSON 序列化数据，使前端可以直接解析和使用该数据。

### 2.8. 路由（Routing） & 属性路由（Attribute Routing）

### 路由（Routing）
- 路由是将 HTTP 请求映射到控制器的动作方法的机制。通过路由，服务器可以根据客户端请求的 URL 来调用相应的控制器方法，并返回正确的响应。
- 路由的定义决定了 URL 如何被解析并对应到特定的控制器和方法上。

### 属性路由（Attribute Routing）
- 属性路由是直接在控制器和动作方法上定义路由规则的方式。通过在控制器或方法上使用 `[Route]` 属性，可以精确地控制 URL 与动作方法的映射。
  
### 示例代码
```csharp
[Route("api/[controller]")]
public class UsersController : Controller {
    [HttpGet("{id}")]
    public IActionResult GetUser(int id) {
        // 获取用户信息
    }
}
```
### 2.9. 路由传值

路由传值是指通过 URL 的路径、查询字符串或请求体向 Web API 传递参数，以便服务器根据这些参数处理请求并返回相应的结果。路由传值通常用于在 URL 中嵌入动态数据，比如资源的 ID、类型或其他查询参数。

### ASP.NET Core 中路由传值的主要方式

1. 路径参数（Route Parameters）：通过 URL 的路径传递参数。

- 示例：GET /users/123，这里的 123 是路径参数，用于指定用户 ID。

2. 查询字符串参数（Query String Parameters）：通过 URL 中的查询字符串传递参数。

- 示例：GET /users?name=John，name=John 是查询字符串参数。

3. 请求体参数（Request Body Parameters）：通过 POST、PUT 请求的请求体传递复杂的对象。

- 示例：在 POST /users 请求体中包含用户信息 JSON 数据。
这些路由传值方式提供了灵活的 API 设计方案，允许在请求中传递不同类型的数据。

### 2.10. Postman

### 什么是 Postman？
Postman 是一款流行的 API 开发工具，用于测试和管理 API。它提供了简洁的界面，让开发者能够方便地发送 HTTP 请求（如 GET、POST、PUT、DELETE 等），并查看响应结果。Postman 支持自动化测试、协作、和文档生成，使其成为开发、调试和维护 API 的得力助手。

### 为什么要使用 Postman？

### 1. 简化 API 测试
- 通过 Postman，开发者可以轻松发送各种类型的 HTTP 请求，检查 API 的响应，并快速排查问题。
- 支持在请求中添加参数、headers、body 等详细信息，精确模拟实际的 API 调用场景。

### 2. 提高效率
- Postman 提供了可视化的用户界面，减少了使用命令行或代码编写来测试 API 的繁琐步骤。
- 通过使用 Postman 的 Collection 功能，可以将多个请求组织到一起，快速运行一组 API 测试用例。

### 3. 支持自动化测试
- Postman 支持通过 JavaScript 编写测试脚本，轻松实现 API 测试的自动化。 
- 可以在请求之后添加断言，检查响应是否符合预期，以确保 API 的可靠性和一致性。

### 4. 团队协作与共享
- Postman 的工作区和协作功能使团队能够共享请求、测试用例、环境变量等，保持团队成员的测试一致性。
- 还支持生成 API 文档，便于团队和客户查看 API 的使用方法和规格。

### 5. 支持环境变量
- 通过在 Postman 中配置不同的环境（如开发、测试、生产环境），可以快速切换 API 请求的 base URL、认证令牌等变量，提升开发效率。
  
### 6. 丰富的集成
- Postman 可以与 CI/CD 工具集成，支持将 API 测试作为持续集成的一部分。
- 提供与 GitHub、Jenkins、Slack 等工具的集成，便于在开发流程中使用 API 测试。

### 使用场景
- **开发阶段**：模拟 API 请求，验证 API 的设计和实现是否正确。
- **测试阶段**：运行自动化测试用例，确保 API 按预期工作。
- **运维阶段**：监控和诊断 API 性能，发现问题并优化。

Postman 不仅是开发者用来测试 API 的工具，也是整个开发团队维护 API 文档和自动化测试的重要平台。无论是在开发、测试还是发布过程中，Postman 都能帮助提升 API 管理的效率和质量。

### 2.11. REST API

### 什么是 REST API？
REST API 是一种特定类型的 Web API，遵循 REST（Representational State Transfer）架构风格。它基于 HTTP 协议设计，通过使用 HTTP 方法（如 GET、POST、PUT、DELETE 等）操作资源（如用户、订单等），并以统一的接口设计实现对资源的管理。

### CRUD 操作与 HTTP 请求类型的对应关系：

1. **Create（创建）** 对应 **POST**
   - POST 请求用于在服务器上创建一个新的资源。
   - 示例：`POST /users` 用于创建一个新的用户。

2. **Read（读取）** 对应 **GET**
   - GET 请求用于从服务器获取资源。
   - 示例：`GET /users/1` 用于获取 ID 为 1 的用户信息。

3. **Update（更新）** 对应 **PUT**（或 **PATCH**）
   - PUT 请求用于更新服务器上的资源，通常用于整体更新资源的所有字段。
   - PATCH 用于部分更新资源。
   - 示例：`PUT /users/1` 用于更新 ID 为 1 的用户信息。

4. **Delete（删除）** 对应 **DELETE**
   - DELETE 请求用于从服务器删除资源。
   - 示例：`DELETE /users/1` 用于删除 ID 为 1 的用户。

### 2.12. 模型验证（Model Validation）

在 ASP.NET Core Web API 中，模型验证是一种用于确保客户端发送的数据符合预期规则的机制。模型验证通过数据注解（Data Annotations）实现，框架会自动检查模型中的属性是否符合这些注解定义的规则，并在验证失败时返回相应的错误响应。这对保证数据的完整性和一致性非常重要。

### 常用的数据注解

| 注解                       | 功能                                       |
|----------------------------|--------------------------------------------|
| `[Required]`               | 属性不能为空。                             |
| `[StringLength(max)]`      | 字符串的长度不能超过 `max`，可以指定 `MinimumLength` 属性来设置最小长度。 |
| `[Range(min, max)]`        | 限制数值或日期的范围。                     |
| `[EmailAddress]`           | 验证电子邮件地址的格式。                   |
| `[RegularExpression]`      | 根据提供的正则表达式验证属性的值。         |
| `[Compare("OtherProperty")]` | 确保当前属性的值与 `OtherProperty` 属性的值相等，通常用于确认密码。 |
| `[Phone]`                  | 验证电话号码格式。                         |
| `[Url]`                    | 验证 URL 格式。                            |

### 2.13. 统一返回

统一返回结果在 Web API 设计中非常重要，特别是在前后端分离架构中，它可以提高 API 的一致性、可维护性和可扩展性。`CommonResult` 类提供了一种统一的结构来封装 API 的响应数据，让每个 API 都遵循相同的格式。

### 统一返回的优势

### 简化前端开发

- **简化响应解析**：对于前端开发者来说，统一返回格式意味着不需要针对不同的 API 编写复杂的解析逻辑。所有的 API 响应都有固定的格式，这简化了数据处理和错误处理逻辑的编写，减少了潜在的 bug。
- **统一处理逻辑**：前端可以通过统一的方式处理成功或失败的情况。例如，前端可以根据 `IsSuccess` 属性统一决定是显示成功信息还是错误提示。

### 2.14. 过滤器 - Filters

过滤器允许在请求处理的不同阶段插入自定义逻辑。通过过滤器，可以在控制器或操作方法执行之前或之后执行某些操作，而无需在每个方法中编写重复的代码。

### 过滤器的主要类型

ASP.NET Core 提供了几种不同类型的过滤器，每种过滤器针对不同的操作阶段，可以根据需求选择使用：

### 授权过滤器 (Authorization Filters)
- **作用**：在请求管道中最早执行，用于决定当前请求是否被授权。
- **使用场景**：常用于处理认证和权限检查，比如要求用户登录后才能访问某些资源。
- **执行时机**：在控制器或方法执行之前。

### 资源过滤器 (Resource Filters)
- **作用**：控制请求是否继续处理，通常用于资源加载或缓存等操作。
- **使用场景**：可用于实现缓存机制，或在请求处理的最早阶段处理自定义的资源管理逻辑。
- **执行时机**：在授权过滤器之后，模型绑定和操作方法执行之前。

### 操作过滤器 (Action Filters)
- **作用**：在控制器的操作方法 (Action) 执行之前和之后运行自定义逻辑。
- **使用场景**：可用于验证请求数据、修改请求参数、操作返回结果等。
- **执行时机**：在控制器或操作方法执行之前和之后。

### 异常过滤器 (Exception Filters)
- **作用**：用于处理控制器或操作方法中的异常，可以对异常进行统一处理或记录日志。
- **使用场景**：捕获异常并将其转换为适当的错误响应。
- **执行时机**：当控制器或操作方法中发生未处理的异常时执行。

### 结果过滤器 (Result Filters)
- **作用**：在操作方法执行完毕，并在返回结果被处理之前和之后执行逻辑。
- **使用场景**：用于在返回结果之前修改响应数据，比如将返回结果格式化为特定的格式（如 JSON）。
- **执行时机**：在操作结果生成后，结果返回到客户端之前。

#### 过滤器的执行顺序

在 ASP.NET Core 中的过滤器是按一定顺序执行的，按照以下顺序进行：

1. **授权过滤器 (Authorization Filters)**：首先执行，用于确定请求是否被授权。
2. **资源过滤器 (Resource Filters)**：在授权之后执行，控制请求是否继续处理。
3. **操作过滤器 (Action Filters)**：在资源过滤后执行，对操作方法的执行过程进行控制。
4. **异常过滤器 (Exception Filters)**：在操作方法发生异常时执行，统一处理异常。
5. **结果过滤器 (Result Filters)**：最后执行，用于在返回结果前后进行处理。


---

### Aspect-Oriented Programming, AOP

通过 AOP（面向切面编程），可以在特定的逻辑点进行增强（例如，记录日志、处理异常），而不会影响核心业务逻辑。

