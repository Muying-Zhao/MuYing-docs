# 前言

> Node.js是一个基于Chrome V8 JavaScript引擎的开源、跨平台JavaScript运行时环境，主要用于开发服务器端应用程序。它的特点是非阻塞I/O模型，使其在处理高并发请求时表现出色。

# 一、Node JS到底是什么

## 1、Node JS是什么
1. Node.js不是一种独立的编程语言
2. Node.js不是JavaScript框架
3. Node.js是一个JavaScript运行环境

## 2、Node.js 与JavaScript是什么关系?

1. Node.js与JavaScript的关系

* 层面: Node.js使用的编程语言是JavaScript。这意味着在Node.js中编写的代码语法与浏览器中的JavaScript代码语法基本相同。
* 运行环境: 浏览器中的JavaScript运行在浏览器提供的环境中，而Node.js中的JavaScript运行在Node.js提供的运行时环境中。浏览器和Node.js都基于V8引擎，但Node.js提供了额外的API，使其更适合服务器端开发。

2. Node.js扩展了JavaScript的能力

> Node.js不仅仅是JavaScript的运行时环境，它还提供了一些独特的特性，使JavaScript在服务器端更加强大：

* 非阻塞I/O: Node.js采用事件驱动和非阻塞I/O模型，适合处理高并发请求。
* 模块系统: Node.js使用CommonJS模块系统，允许开发者将代码分割成独立的模块。
* 内置API: Node.js提供了一系列内置API，用于文件系统操作、网络通信、流处理、子进程管理等。

# 二、Node JS本地环境搭建

## 1、安装Node.js

> 从Node.js官网下载并安装Node.js，安装完成后可以使用以下命令验证安装是否成功

[Node JS官网](https://nodejs.org/zh-cn)

```shell
node -v
npm -v
```

# 三、文件操作与模块化的概念

## 1、文件操作

1. 文件读取

> 创建read.js文件

```js
var fs = require("fs");
fs.readFile("./text.txt", "utf8", function (err, data) {
  console.log(err);
  console.log(data);
});
```

> 创建text.txt文件

```
hello world
```

> 通过`node read.js`命令进行读取

```shell
node read.js
```
![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240621/1718940762149724998.png)

2. 文件写入

> 创建`write.js`文件
```js
var fs = require("fs");
fs.writeFile("./text.txt", "world hello", function (err) {
  console.log(err);
});
```

> 通过`node write.js`命令进行读取

```shell
node write.js
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240621/1718941040217920840.png)

> text.txt文件内容被写入

hello world-> world hello

3. 文件追加

> 创建file.js文件

```js
var fs = require("fs");
fs.readFile("./text.txt", "utf8", function (err, data) {
  if (!err) {
    var newData = data + "welcome";
    fs.writeFile("./text.txt", newData, function (err) {
      if (!err) {
        console.log("追加内容成功");
      }
    });
  }
});
```

> 通过`node file.js`命令进行读取

```shell
node file.js
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240621/1718941397084684781.png)

> text.txt文件内容被写入

hello world-> hello worldwelcome

## 2、模块化编程

> 模块化编程是一种软件设计和开发的方法，通过将程序分解成独立、互相协作的模块来提高代码的可维护性、可读性和可重用性。每个模块通常实现特定的功能，并且可以独立开发、测试和部署。模块化编程广泛应用于现代软件开发中，包括前端和后端开发。

1. 模块的定义

> 模块是一个封装了特定功能或一组相关功能的代码单元。模块可以是一个文件、一组文件或一个包。

* 接口：模块向外界提供的功能（方法、属性等），即模块的公开API。
* 实现：模块内部实现细节，对外界不可见，只能通过接口访问。

2. 模块化编程的优点

* 高内聚低耦合：模块内的代码高内聚，而模块之间的耦合度低，提高了代码的可维护性。
* 可重用性：模块可以在不同的项目中重复使用，提高开发效率。
* 易于调试和测试：独立的模块可以单独调试和测试，简化了开发过程。
* 团队协作：不同的开发人员可以并行开发不同的模块，提升团队协作效率。

3. JavaScript中的模块化

* CommonJS模块系统（Node.js）

> Node.js使用CommonJS模块系统，通过require引入模块和module.exports导出模块。

```js
// math.js
exports.add = function(a, b) {
  return a + b;
};
// app.js
const math = require('./math');
console.log(math.add(2, 3));  // 5
```
* ES6模块系统（浏览器和Node.js）

> ES6引入了模块系统（ESM），使用import和export关键字。


```js
// im.mjs
import { val } from "./ex.mjs";
console.log(val);
```

```js
// ex.mjs
var val = "ex data";
export { val };
```

```shell
node im.mjs
```

> 如果直接只用js编译需要再packjson.json中加入{"type":"module"}

```json
"type":"module"
```

* AMD

> AMD是一种用于浏览器环境的模块化规范，旨在解决JavaScript模块的异步加载问题。它通过异步方式加载模块，从而避免阻塞浏览器的运行。RequireJS是一个实现AMD规范的库。

> 定义模块
```js
// 定义一个名为'math'的模块
define('math', [], function() {
  return {
    add: function(a, b) {
      return a + b;
    },
    subtract: function(a, b) {
      return a - b;
    }
  };
});
```
> 加载模块
```js
require(['math'], function(math) {
  console.log(math.add(1, 2));  // 3
});
```

* UMD

> UMD是一种模块定义模式，旨在兼容多种模块系统，如CommonJS、AMD和全局变量。这使得模块可以在不同的环境中（如浏览器和Node.js）无缝运行。

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD. Register as an anonymous module.
    define([], factory);
  } else if (typeof module === 'object' && module.exports) {
    // Node. Does not work with strict CommonJS, but
    // only CommonJS-like environments that support module.exports,
    // like Node.
    module.exports = factory();
  } else {
    // Browser globals (root is window)
    root.myModule = factory();
  }
}(typeof self !== 'undefined' ? self : this, function () {
  // 模块代码
  return {
    add: function(a, b) {
      return a + b;
    },
    subtract: function(a, b) {
      return a - b;
    }
  };
}));
```
