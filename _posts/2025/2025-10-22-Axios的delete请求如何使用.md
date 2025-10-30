---
layout: post
title: "2025-10-22-Axios 的 delete 请求如何使用"
author: "gyeon"
date: "2025-10-22_14:48:30"
header-img: "img/post-bg-js-version.jpg"
header-mask: 0.4
tags:
  - 前端
  - axios
---

# Axios 的 delete 请求如何使用？传参写法有哪几种？

本文介绍了 Axios 的 DELETE 请求的基本概念和使用方法，并讲解在 URL 中传递参数、使用 params 参数传递查询参数。

## 简介

Axios 是一个流行的 HTTP 请求库，它能够在浏览器和 Node.js 环境中发送异步请求，使用 Axios 这样的 JavaScript 库来处理 HTTP 请求已经变得非常普遍。本文将重点介绍 Axios 的 DELETE 请求的使用方式，并探讨不同的传参写法。


## 基本的 DELETE 请求概念
DELETE 请求用于向服务器发送删除资源的请求。它是 RESTful API 中的一个重要方法，用于删除指定的资源。


在 Axios 中，发送 DELETE 请求需要指定目标 URL，并可选地传递一些参数，例如请求头、请求体等。DELETE 请求不同于 GET 请求，它可以包含请求体，因此在某些情况下，你可能需要在 DELETE 请求中传递数据。

## DELETE 请求传参写法

在 Axios 中，DELETE 请求的传参写法主要有以下几种方式：

### 1. 在 URL 中传递参数

最简单的方式是将参数直接拼接在 URL 上，这通常用于传递少量的数据，例如资源的 ID。


示例代码：

```javascript
const axios = require('axios');

const resourceId = 123;
axios.delete(`https://api.example.com/resource/${resourceId}`)
  .then(response => {
    console.log('Resource deleted successfully:', response.data);
  })
  .catch(error => {
    console.error('Error deleting resource:', error);
  });
```

### 2. 使用 params 参数传递参数

如果你希望将参数作为查询参数传递，可以使用 Axios 的 params 参数。


示例代码：

```javascript
const axios = require('axios');

const params = { id: 123 };
axios.delete('https://api.example.com/resource', { params })
  .then(response => {
    console.log('Resource deleted successfully:', response.data);
  })
  .catch(error => {
    console.error('Error deleting resource:', error);
  });
```

### 3. 使用 data 参数传递请求体数据

对于一些需要在 DELETE 请求中传递复杂数据的情况，可以使用 Axios 的 data 参数。


示例代码：

```javascript
const axios = require('axios');

const requestData = { id: 123, reason: 'No longer needed' };
axios.delete('https://api.example.com/resource', { data: requestData })
  .then(response => {
    console.log('Resource deleted successfully:', response.data);
  })
  .catch(error => {
    console.error('Error deleting resource:', error);
  });
```

## 实践案例：使用 Axios 发送 DELETE 请求

让我们来实践一个简单的案例，在这个案例中，我们将使用 Axios 发送 DELETE 请求来删除一个用户。

### 1.安装 json-server

首先，你需要在项目目录下使用 npm 或 yarn 安装 json-server。

```shell
npm install -g json-server
```

然后，在项目目录下创建一个 JSON 文件，用于模拟你的数据。假设你要模拟的数据是用户数据，可以创建一个名为 `users.json` 的文件，并在其中定义用户数据。
users.json 文件内容示例：

```javascript
{
  "users": [
    { "id": 1, "name": "John Doe", "email": "john@example.com" },
    { "id": 2, "name": "Jane Smith", "email": "jane@example.com" }
  ]
}
```

最后，在终端中运行以下命令，以启动 json-server 并指定模拟数据文件：

```shell
json-server --watch users.json --port 3000
```

这将启动一个模拟服务器，并监听端口 3000，使用 `users.json` 文件中的数据作为模拟的资源，如图所示：

```shell
PS D:\技术文档\前端系列\axios\axios-delete>json-server --watch users.json --port 3000

\{^_^}/ hi!

Loading users.json
Done

Resources
http://localhost:3000/users

Home
http://localhost:3008
Type s + enter at any time to create a snapshot of the database
Watching...
```

### 2.发送 delete 请求

上面的 json-server 提供的路由可以为：

* DELETE http://localhost:3000/users/:userId

首先，在 IDE 编辑器中创建一个新的 JavaScript 文件（例如，`deleteUser.js`），然后粘贴以下代码，并用 `node deleteUser.js` 命令在控制台运行。

```javascript
const axios = require('axios');

const userId = 1; // 要删除的用户 ID
axios.delete('http://localhost:3000/users/' + userId)
  .then(response => {
    console.log('User deleted successfully:', response.data);
  })
  .catch(error => {
    console.error('Error deleting user:', error);
  });
```

> 注：如果报错，请确保是否安装了 `axios`，安装命令为 `npm install axios`

在上面的代码中，我们使用了第一种方式，在 URL 中直接传递参数（用户 ID），以便删除对应的用户。


如果一切顺利，你应该能够在控制台中看到删除成功的信息，并且 `users.json` 中 id 为 1 的对象已被删除。

## 提示与注意事项

* 在使用 DELETE 请求时，务必确认请求的目标和资源，以防止误删数据或对重要资源造成影响。
* 如果你需要发送复杂的请求体数据，使用 data 参数而不是 params 参数。
* 建议在发送 DELETE 请求时处理成功和失败的情况，以便及时处理潜在的错误。


## 总结
本文介绍了 Axios 的 DELETE 请求的基本概念和使用方法，并讲解三种不同的传参写法，包括直接在 URL 中传递参数、使用 `params` 参数传递查询参数，以及使用 `data` 参数传递请求体数据。我们还通过一个实践案例演示了如何在 IDE 编辑器中使用 Axios 发送 DELETE 请求来删除用户。


Axios 是一个功能强大且易于使用的 HTTP 客户端，它可以帮助我们轻松处理各种 HTTP 请求任务，包括 GET、POST、PUT、DELETE 等。通过合理使用 DELETE 请求，我们可以更好地构建高效的前端应用程序，并与后端服务器进行数据交互。