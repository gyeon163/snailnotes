---
layout: post
title: "2025-10-30-Axios 中怎么上传文件（Upload File）？上传方法有哪几种？"
author: "gyeon"
date: "2025-10-30_14:15:45"
header-img: "img/post-bg-js-version.jpg"
header-mask: 0.4
tags:
  - 前端
  - axios
---

# Axios 中怎么上传文件（Upload File）？上传方法有哪几种？

本文介绍 Axios 的文件上传方法，包括使用 FormData 对象、URL参数、Base64编码的方式。

## 简介

Axios 提供了一些方便的方法来上传文件,包括使用 FormData、设置 Content-Type 为 multipart/form-data 等。本文将介绍 Axios 的文件上传概念，列举 Axios 中可用的上传方法，并通过一个实践案例来演示如何在 IDE 编辑器中运行上传文件的操作。

## 文件上传（Upload File）概念
文件上传是将本地计算机中的文件发送到服务器的过程。在前端开发中，通常使用`<input type="file">`元素来实现文件选择功能。用户通过点击这个元素，选择需要上传的文件。然后，前端代码将选中的文件传递给后端服务器。


在实际上传过程中，涉及到使用 `multipart/form-data` 请求格式，这样可以将文件和其他表单数据一起发送到服务器。

## Axios 中的文件上传（Upload File）方法

Axios 提供了多种上传文件（Upload File）的方法，适用于不同的上传场景。以下是其中几种常用的方法：

### 1. 使用 FormData 对象

FormData是一个用于创建表单数据的 API，可用于发送包含文件和其他表单数据的`multipart/form-data`请求。这是处理文件上传的常用方法。通过`FormData`对象，可以将文件数据添加到表单中，然后使用 Axios 的post或put方法发送请求。

```javascript
const axios = require('axios');

const fileInput = document.querySelector('#fileInput');
const file = fileInput.files[0];

const formData = new FormData();
formData.append('file', file);

axios.post('/upload', formData, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
}).then(response => {
  console.log('上传成功', response.data);
}).catch(error => {
  console.error('上传失败', error);
});
```


### 2. 使用 URL 参数

除了使用`FormData`，你还可以通过在 URL 参数中指定文件名的方式上传文件。这种方法适用于后端期望文件名直接出现在 URL 中的情况。

```javascript
const axios = require('axios');

const fileInput = document.querySelector('#fileInput');
const file = fileInput.files[0];

axios.post('/upload', file, {
  params: {
    fileName: file.name
  }
}).then(response => {
  console.log('上传成功', response.data);
}).catch(error => {
  console.error('上传失败', error);
});
```

### 3. 使用 Base64 编码

这种方法将文件转换成 Base64 编码的字符串，然后通过普通的 JSON 格式发送给服务器。这种方式适用于较小的文件，因为 Base64 编码会增加数据大小。

```javascript
const axios = require('axios');

const fileInput = document.querySelector('#fileInput');
const file = fileInput.files[0];

const reader = new FileReader();

reader.onload = function(event) {
  const base64Data = event.target.result.split(',')[1];

  axios.post('/upload', {
    file: base64Data
  }).then(response => {
    console.log('上传成功', response.data);
  }).catch(error => {
    console.error('上传失败', error);
  });
};

reader.readAsDataURL(file);
```

### 4.发送文件 Blob 对象

可以通过 CreateObjectURL 把文件对象转成 Blob URL，然后作为 Axios 请求的数据发送。

```javascript
const file = document.getElementById('file').files[0];

const blobUrl = URL.createObjectURL(file);

axios.post('/upload', blobUrl, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }  
});
```

## 实践案例

为了演示文件上传过程，我们将使用 Node.js 作为后端服务器。假设后端提供了`/upload`接口来处理文件上传请求。

创建前端 HTML 页面（文件名可命名为`index.html`）：

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

2.创建前端JavaScript文件 `main.js`：

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

3.创建 Node.js 后端服务器（文件可命名为 `server.js`）：

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

当然，要成功运行服务，还需要安装如下的扩展：

```shell
npm install express

npm install multer

npm install cors
```

运行前端和后端代码后，打开浏览器访问 HTML 页面，选择文件后点击“上传文件”按钮，控制台会输出上传成功的信息，并在后端服务器目录的`uploads/`文件夹中保存上传的文件。


## 提示与注意事项

* 确保后端服务器能够处理文件上传请求，通常需要使用`multer`等中间件来解析文件。
* 在使用`FormData`上传文件时，务必设置正确的`Content-Type`头信息。
* 使用Base64编码上传文件时，要注意文件大小可能会大幅增加，适用于较小的文件。
* 设置headers的Content-Type为`multipart/form-data`时，需要匹配服务端的解析方式。
* onUploadProgress 可以监听上传进度，方便实现上传进度条等功能。
* 服务端需要对应支持文件上传的接口。
* 根据文件大小，可能需要设置 timeout 或响应 timeout。

## 总结

本文介绍 Axios 的文件上传方法，包括使用 FormData 对象、URL参数、Base64编码的方式，并通过一个实践案例演示了如何在前端中使用Axios上传文件，并在 Node.js 后端进行处理。