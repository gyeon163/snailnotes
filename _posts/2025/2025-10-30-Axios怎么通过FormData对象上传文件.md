---
layout: post
title: "2025-10-30-Axios 怎么通过 FormData 对象上传文件？"
author: "gyeon"
date: "2025-10-30_11:04:25"
header-img: "img/post-bg-js-version.jpg"
header-mask: 0.4
tags:
  - 前端
  - axios
---

# Axios 怎么通过 FormData 对象上传文件？

本文介绍 Axios 通过 FormData 上传文件主要的两种方法：直接追加文件到 FormData 中和设置 Content-Type 为 multipart/form-data。

## 简介

通过 FormData 对象结合 Axios，我们可以方便地实现文件上传功能。本文将详细介绍如何使用 Axios 和 FormData 对象上传文件，并提供多种方法和实践案例。

## FormData 对象介绍

FormData 是一个用于在客户端创建表单数据的接口。它可以通过 JavaScript 中的 `new FormData()` 构造函数创建。FormData 可以将表单字段的键值对以键值对的方式添加，同时也支持添加文件。

在文件上传的场景中，我们可以使用 FormData 对象来收集表单数据，包括文件和其他文本字段，然后将其发送到后端服务器。

FormData 对象用于将数据编译成键值对，以便将其提交到服务器，它主要用于通过 XHR 传输文件。

使用 FormData 的主要优点:

* 可以异步上传文件
* 可以上传二进制文件
* 提交表单时自动处理表单数据编码
* 可以上传文件流(Blob 对象和 File 对象)

## Axios 通过 FormData 上传文件

Axios 可以通过 FormData 对象上传文件，主要有两种方法:

### 1. 直接在 FormData 中追加文件

直接将文件对象作为 value，追加到 FormData 中，axios 会自动对文件进行编码。

```javascript
const formData = new FormData();

formData.append('file', fileInput.files[0]); // fileInput 为 <input type="file" />

axios.post('/upload', formData) 
```

### 2. 设置 request header 的 Content-Type

Axios 默认发送 JSON 数据，设置 headers 将 Content-Type 设置为 multipart/form-data 后，就会处理为 FormData 对象提交。

```javascript
const formData = new FormData();

formData.append('file', fileInput.files[0]);

axios.post('/upload', formData, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
})
```

## 实践案例

为演示文件上传过程，本文将使用 Node.js 构建后端服务器。后端会提供 `/upload` 接口来处理文件上传请求。


1.首先，创建前端 HTML 页面，本文以 `index.html` 为例:

```html
<!DOCTYPE html>
<html>
<head>
  <title>文件上传示例</title>
</head>
<body>
  <input type="file" id="fileInput">
  <button onclick="uploadFile()">上传文件</button>

  <script src="axios.min.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

2.然后，创建前端 JavaScript 文件 `main.js` ,用于实现文件上传的交互逻辑:

```javascript
function uploadFile() {
  const fileInput = document.querySelector('#fileInput');
  const file = fileInput.files[0];

  // 使用FormData对象上传文件
  const formData = new FormData();
  formData.append('file', file);

  axios.post('http://localhost:5500/upload', formData, {
    headers: {
      'Content-Type': 'multipart/form-data'
    }
  }).then(response => {
    console.log('上传成功', response.data);
  }).catch(error => {
    console.error('上传失败', error);
  });
}
```

3.最后，创建 Node.js 后端服务器脚本（可命名为 `server.js`，用于接收并处理文件上传请求:

```javascript
const express = require("express");
const multer = require("multer");
const cors = require("cors"); // 引入cors中间件
const app = express();
const upload = multer({ dest: "uploads/" });

// 使用cors中间件来设置CORS
app.use(cors());

app.post("/upload", upload.single("file"), (req, res) => {
  const file = req.file;
  console.log("已接收到文件", file);
  // 在此处进行文件保存或其他处理
  res.send({
    code: 200,
    data: "文件上传成功",
  });
});

app.listen(5500, () => {
  console.log("服务器已启动，监听端口 5500");
});
```

当然，为使后端服务器正常运行，还需要安装以下 Node.js 扩展模块:

* **express** - 构建 Web 应用框架
* **multer** - 用于处理 multipart/form-data 类型的表单数据，从而实现文件上传功能
* **cors** - 用于实现跨域资源共享，解决前后端跨域问题安装命令如下:

```javascript
npm install express

npm install multer

npm install cors
```

运行前端和后端代码后，打开浏览器访问 HTML 页面，选择文件后点击**“上传文件”**按钮，控制台会输出上传成功的信息，并在后端服务器目录的 `uploads/` 文件夹中保存上传的文件。


## 提示与注意事项

* FormData 对象中追加的是文件对象，而不是文件路径
* 大文件上传可能需要设置超时时间
* 服务端需要用 multer 等工具解析 FormData
* 设置正确的 Content-Type，服务端才能正确处理文件

## 总结

Axios 通过 FormData 上传文件主要的两种方法：直接追加文件到 `FormData` 中和设置 `Content-Type` 为 `multipart/form-data`。需要注意的地方是 FormData 中是文件对象，并且服务端需要能解析 FormData 对象。