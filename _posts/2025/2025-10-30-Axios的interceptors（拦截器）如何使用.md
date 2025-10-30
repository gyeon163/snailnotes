---
layout: post
title: "2025-10-30-Axios中如何设置请求头"
author: "gyeon"
date: "2025-10-30_10:48:11"
header-img: "img/post-bg-js-version.jpg"
header-mask: 0.4
tags:
  - 前端
  - axios
---

# Axios 的 interceptors（拦截器）如何使用？一文讲述它的用法

本文中介绍了请求拦截器和响应拦截器的基本概念，并通过一个实践案例演示了如何使用 Axios 拦截器来处理基本路由与请求。

## 简介

Axios 提供了一种称为“拦截器（interceptors）”的功能，使我们能够在请求或响应被发送或处理之前对它们进行全局处理。拦截器为我们提供了一种简洁而强大的方式来转换请求和响应、进行错误处理、添加认证信息等操作。在本文中，我们将深入探讨如何使用 Axios 的拦截器，并提供一个实际案例来演示其用法。

## Axios 拦截器的基本概念

在 Axios 中，拦截器是一个由两个部分组成的对象：请求拦截器（request interceptors）和响应拦截器（response interceptors）。这两种拦截器允许我们在请求发出之前和收到响应后对其进行预处理。

**请求拦截器（request interceptors）**用于在请求被发送之前修改请求配置，或者在发送请求前进行一些操作，例如添加认证信息、设置请求头等。

**响应拦截器（response interceptors）**用于在接收到响应数据之后进行处理，可以对响应数据进行转换、错误处理等操作。

Axios 拦截器是链式结构的，这意味着您可以同时使用多个拦截器，并且它们按照添加顺序依次执行。

下面是 Axios 中拦截器的基本用法：

```javascript
// 添加请求拦截器
axios.interceptors.request.use(
  function (config) {
    // 在发送请求之前做些什么
    return config;
  },
  function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  }
);

// 添加响应拦截器
axios.interceptors.response.use(
  function (response) {
    // 对响应数据做些什么
    return response;
  },
  function (error) {
    // 对响应错误做些什么
    return Promise.reject(error);
  }
);
```

## 实践案例：使用 Axios 拦截器请求处理

让我们通过一个实际案例来演示 Axios 拦截器的用法。在这个案例中，我们将使用 Axios 发起一个 GET 请求，并在请求拦截器中添加一个基本的认证头，并在响应拦截器中处理返回的数据。

### 请求路径

为了便于测试，所以案例使用 Apifox 提供的开放 API 来测试，测试的 API 路径为：https://echo.apifox.com/get?q1=v1


### 案例代码

首先，确保您已经在项目中安装了 Axios：

```shell
npm install axios
```

现在，我们来编写实践案例代码：

```javascript
// 导入 Axios 和你的 IDE 编辑器中的其他必要模块
const axios = require('axios');

// 添加请求拦截器
axios.interceptors.request.use(
  function (config) {
    // 在发送请求之前添加认证头
    config.headers['Authorization'] = 'Bearer your_access_token';
    return config;
  },
  function (error) {
    return Promise.reject(error);
  }
);

// 添加响应拦截器
axios.interceptors.response.use(
  function (response) {
    // 对响应数据进行处理
    return response.data;
  },
  function (error) {
    // 对响应错误进行处理
    return Promise.reject(error);
  }
);

// 发起 GET 请求
axios.get('https://echo.apifox.com/get?q1=v1')
  .then((data) => {
    // 处理返回的数据
    console.log('请求成功，数据为：', data);
  })
  .catch((error) => {
    // 处理错误
    console.error('请求失败，错误为：', error);
  });
```

在这个案例中，我们在请求拦截器中添加了一个名为 "Authorization" 的认证头，并在响应拦截器中处理了返回的数据。


## 提示与注意事项

1. 当添加多个拦截器时，确保它们的顺序是正确的，因为它们会按照添的顺序依次执行。
2. 在拦截器中，务必返回修改后的 config 对象或响应数据，否则可能会导致请求或响应失败。
3. 谨慎使用拦截器，不要滥用，否则可能会导致代码难以维护和理解。

## 总结

Axios 的拦截器是一个强大的功能，使得我们能够在请求和响应阶段对数据进行全局处理。我们在本文中介绍了请求拦截器和响应拦截器的基本概念，并通过一个实践案例演示了如何使用 Axios 拦截器来处理基本路由与请求。拦截器为我们提供了更灵活、高效的方式来管理 HTTP 请求和响应，帮助我们在前端开发中更好地处理数据交互。