# .NET Part 6

## Description

- 本篇笔记是根据Lily老师 Lecture 18 .NET Part6 的课堂内容整理的随堂笔记。
- 参考资料：https://www.canva.com/design/DAGThX47NqA/g-ieYxQvCzLOjfFvw5SmFw/view?utm_content=DAGThX47NqA&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table Of Content

- [1. 什么是跨域问题](#1-什么是跨域问题)
  - [同源策略（Same-Origin Policy）](#同源策略same-origin-policy)
  - [跨域的解决方案 —— CORS](#跨域的解决方案--cors)
- [2. Web API Authentication](#2-web-api-authentication)
  - [常用的 API 身份验证方式](#常用的-api-身份验证方式)
- [3. JWT 工作原理](#3-jwt-工作原理)
  - [JWT 工作流程](#jwt-工作流程)
  - [JWT 数据结构](#jwt-数据结构)
- [4. 扩展方法](#4-扩展方法)
  - [扩展方法示例](#扩展方法示例)
- [5. Swagger](#5-swagger)
  - [安装和配置步骤](#安装和配置步骤)
  - [访问 Swagger UI](#访问-swagger-ui)
- [6. NLog](#6-nlog)
  - [NLog 的安装与配置](#nlog-的安装与配置)
  - [NLog 的日志级别](#nlog-的日志级别)
- [7. RBAC (Role-Based Access Control，基于角色的访问控制)](#7-rbac-role-based-access-control基于角色的访问控制)
  - [数据库设计](#数据库设计)
    - [1. `permissions` 表](#1-permissions-表)
    - [2. `roles` 表](#2-roles-表)
    - [3. `userrole` 表](#3-userrole-表)
    - [4. `rolepermission` 表](#4-rolepermission-表)

## 1. 什么是跨域问题

跨域（Cross-Origin）问题指的是当浏览器在一个域下加载的网页发出请求，想要访问另一个不同域名、协议或端口的资源时，浏览器会进行安全限制。

### 同源策略（Same-Origin Policy）

同源策略是一种浏览器的安全机制，用来防止不同来源的网站之间相互干扰。所谓“同源”，指的是协议、域名和端口号都必须相同。只有满足“同源”的网站，才允许彼此间的资源共享。

示例:

- `https://example.com:8080/page` 和 `https://example.com/page` 被认为是不同源，因为端口号不同。
- 网页 `https://www.example.com` 尝试访问 API `https://api.anotherdomain.com/data`，由于不同的域名，会触发跨域限制。

### 跨域的解决方案 —— CORS

跨域资源共享（CORS, Cross-Origin Resource Sharing）是一种机制，允许服务器明确告诉浏览器“允许来自特定域的跨域请求”。

通过 CORS，服务器可以在响应头中设置允许的来源，从而实现资源共享。例如，可以通过在服务器配置中添加允许的源来避免浏览器的跨域阻止。

## 2. # Web API Authentication

API 身份验证是在应用程序与 API 进行对话时，用于确认对方身份的过程。它的作用类似于进入保密场所前所需的身份认证，目的是确保只有授权的用户才能访问 API，从而保护敏感的“资源”和“数据”免遭滥用或破坏。

### 常用的 API 身份验证方式

- **Token-based Authentication**（基于令牌的身份验证，如 JWT）

Token-based Authentication 是一种常见的身份验证方式，利用令牌（Token）进行身份验证和授权。例如，JWT（JSON Web Token）是一种广泛应用的 Token-based 身份验证技术，确保每次请求都附带有效的身份凭证。

## 3. JWT 工作原理

JWT（JSON Web Token）是一种基于令牌的身份验证方法，通常用于 Web API 的身份验证。以下是 JWT 的工作流程和数据结构。

## JWT 工作流程

1. **用户登录**：
   - 客户端发送 `POST` 请求到 `/login`，携带用户名和密码。
   - 服务器验证用户名和密码是否正确，并生成 JWT。

2. **返回 Token**：
   - 服务器将生成的 Token 返回给客户端，例如：`{ token: '123abcd...' }`。

3. **客户端存储 Token**：
   - 客户端将 Token 存储在 `localStorage` 或 `cookie` 中，以便后续请求使用。

4. **使用 Token 访问受保护资源**：
   - 客户端在访问受保护的 API 时，将 Token 放入请求头中，例如：
     ```http
     Authorization: Bearer 123abcd
     ```

5. **服务器验证 Token**：
   - 服务器在每个请求中验证 JWT 的有效性，确保请求来自已认证用户。

## JWT 数据结构

JWT 由三部分组成：Header、Payload 和 Signature，各部分用“.”分隔。示例：
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### Header
包含算法类型和Token类型：

```json
    {
    "alg": "HS256",
    "typ": "JWT"
    }
```

### Payload
包含用户数据和声明信息，例如：

- iss：签发人
- exp：过期时间
- sub：主题
- aud：受众
- nbf：生效时间
- iat：签发时间
- jti：JWT ID

###  Signature
用于验证 Token 的真实性。生成方式如下：

```plaintext
    HMACSHA256(
    base64UrlEncode(header) + "." + base64UrlEncode(payload),
    secret)
```

## JWT 在 Web API 中的应用

### 1. 配置 JWT：

- JWT 通常会有一些关键配置参数，比如密钥和 Token 的有效期，可以在 appsettings.json 中进行配置。
在 Program.cs 中配置 JWT 验证：

### 2. 定义 JWTConfig 类
- 在 Program.cs 中读取配置并添加 JWT 服务
定义 AddJWT 扩展方法

### 3. 生成 JWT Token：
- 当用户登录时，服务器验证用户名和密码，并生成一个 JWT Token 返回给客户端。

### 4. 客户端存储和使用 JWT：
- 客户端（如 Web 前端、移动应用）收到 JWT 之后，通常将其存储在浏览器的 LocalStorage、SessionStorage，或在移动应用的安全存储中。
- 请求时使用 JWT：当客户端访问需要授权的 API 时，会将 JWT 放到请求头的 Authorization 部分，格式为：
```php
    Bearer <token>
```

### 5. 验证 JWT Token：

- 服务器在每个请求中验证 JWT Token 是否有效。在配置的 AddJwtBearer 中，服务器会自动解析请求头中的 JWT，并验证其有效性。

## 4. 扩展方法

扩展方法使你能够向现有类型“添加”方法，而无需创建新的派生类型、重新编译或以其他方式修改原始类型。扩展方法是一种静态方法，但可以像扩展类型上的实例方法一样进行调用。

### 扩展方法示例

以下示例展示了如何为 `string` 类型创建一个扩展方法 `Reverse`，用来反转字符串内容。

```csharp
    public static class StringExtensions
    {
        public static string Reverse(this string str)
        {
            char[] charArray = str.ToCharArray();
            Array.Reverse(charArray);
            return new string(charArray);
        }
    }
```

在代码中调用扩展方法 Reverse：
```csharp
    string myString = "hello";
    string reversedString = myString.Reverse(); // 输出 "olleh"
```
通过这种方式，我们可以直接在 string 类型的实例上调用 Reverse 方法，而无需修改原始的 string 类型。

## 5. Swagger

Swagger 是一种用于生成 API 文档和测试界面的工具，可以帮助开发者直观地了解和测试 API 接口。以下是使用 Swagger 的基本步骤。

### 1. 安装必要的 NuGet 包

在项目中通过 NuGet 包管理器安装 `Swashbuckle.AspNetCore` 包。

### 2. 在 Program.cs 中配置 Swagger

在 `Program.cs` 文件中配置 Swagger，以启用 Swagger 服务。

### 3. 使用 Swagger 中间件

在 `Program.cs` 中启用 Swagger 和 Swagger UI 中间件，以提供交互式的 API 文档界面。

### 4. 为 API Controller 添加注释

可以在控制器和操作方法上使用 XML 注释，Swagger 将自动提取并显示这些信息。要启用此功能，需要在项目中启用 XML 注释。

### 启用 XML 注释的步骤

1. 右键点击项目 > Properties > Build，勾选 **XML documentation file**。
2. 打开 `Program.cs`，并将 XML 注释文件路径添加到 Swagger 配置中：

```csharp
    var xmlFileName = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
    options.IncludeXmlComments(Path.Combine(AppContext.BaseDirectory, xmlFileName), true);
```
### 5. 访问 Swagger UI
启动项目后，访问 Swagger UI 界面（通常是 http://localhost:<port>/swagger）即可查看并测试 API。

## 6. NLog

NLog 是一个用于 .NET 应用程序的日志库，它提供了强大的功能来记录应用程序中的日志信息。

### NLog 的安装

在 .NET 项目中，可以通过 NuGet 安装 NLog，例如 `NLog.Web.AspNetCore`。

### NLog 的基本配置

NLog 的配置可以通过 XML、JSON 或者代码配置完成。常用的配置文件包括 `nlog.config`（XML 格式）和 `appsettings.json`（JSON 格式）。

### NLog 的日志级别

NLog 提供了多种日志级别，用于不同的场景需求：

- **Trace**：最详细的日志信息，通常用于调试。
- **Debug**：调试时使用，记录调试信息。
- **Info**：用于记录常规的操作和信息。
- **Warn**：表示可能存在的问题，但不影响程序运行。
- **Error**：表示发生了错误，程序可能无法继续正常运行。
- **Fatal**：表示发生了严重的错误，程序将无法继续运行。

通过这些日志级别，开发者可以根据不同的严重性来记录日志，便于后续的分析和调试。

## 7. RBAC (Role-Based Access Control，基于角色的访问控制)

RBAC 是一种用于权限管理的模型，它通过角色来控制用户对系统资源的访问权限。在 RBAC 模型中，权限是基于用户的角色而非单个用户设置的，简化了权限管理的复杂性。

### 数据库设计

在我们的项目中，RBAC 的数据库设计主要包括以下几张表：

### 1. `permissions` 表

- 用于定义系统中的具体权限。
- 常用字段：
  - `Id`：权限 ID
  - `Title`：权限名称（例如 "add course"）
  - `PermissionCode`：权限代码（例如 "pm001"）

### 2. `roles` 表

- 用于定义系统中的角色。
- 常用字段：
  - `Id`：角色 ID
  - `RoleName`：角色名称（例如 "admin", "user"）

### 3. `userrole` 表

- 用于关联用户和角色，表示一个用户可以拥有多个角色。
- 常用字段：
  - `Id`：关联 ID
  - `UserId`：用户 ID
  - `RoleId`：角色 ID

### 4. `rolepermission` 表

- 用于关联角色和权限，表示一个角色可以拥有多个权限。
- 常用字段：
  - `Id`：关联 ID
  - `PermissionId`：权限 ID
  - `RoleId`：角色 ID

通过这种设计，一个用户可以被分配一个或多个角色，而每个角色可以对应多个权限，从而实现基于角色的访问控制。
