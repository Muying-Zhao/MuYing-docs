# 前言

>

# 一、rollup组件库环境搭建

## 1、在桌面输入cmd

> 打开命令行工具，创建`muying-rollupvue`项目

* 创建`muying-rollupvue`文件夹

```
mkdir muying-rollupvue
```

* 进入muying-rollupvue文件

```
cd muying-rollupvue/
```

* 初始化项目,新建packjson.json

```
npm init -y
```

* 用vscode打开该项目，查看packjson.json

属性|作用|
:--:|:--:|
name|项目的名称|
version|项目的版本号|
description|项目的简短描述|
main|项目的入口文件，当其他人或工具需要引用这个项目时，会从这个文件开始。|
scripts|包含了项目的一些脚本命令，可以在命令行中运行|
keywords|用于描述项目的核心功能、特性或所属领域，以便在npm等包管理平台上更容易被搜索到自己的项目|
author|项目的作者信息，一般会留作者名称和邮箱|
license|项目的许可证|
devDependencies|列出在开发过程中需要的依赖，但这些依赖在项目的最终运行环境中通常不是必需的。通常包括构建工具、测试框架、代码格式化工具|
dependencies|列出了项目运行所必需的依赖包。这些依赖是项目在生产环境中正常运行所必须安装的。

```json
{
  "name": "muying-rollupvue",
  "version": "1.0.0",
  "description": "muying components rollupvue",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "muying <2479377049@qq.com>",
  "license": "ISC"
}
```



## 2、安装rollup插件

* 打开终端，安装rollup插件

> 插件可以使用`./node_modules/.bin/rollpu`进行查看

```
npm i -D rollup
```

* 修改scripts脚本命令

```
  "scripts": {
    "dev":"rollup"
  },
```

* 运行rollup项目

```
npm run dev
```

## 3、搭建rollup环境，打包rollup

* 新建`/src/index.js `文件

```
console.log('hello workd')
export default {}
```

* 新建开发者环境`rollup.config.dev.js`

> 配置 Rollup 以支持源代码映射（source maps）、实时重新加载可能未压缩的、更易于调试的代码。

```javascript
const path = require('path')
const inputPath = path.resolve(__dirname, './src/index.js')
const outputPath = path.resolve(__dirname, './dist/muying-rollupvue.js')
module.exports = {
    input: inputPath,
    output: [
        {
            file: outputPath,
            format: 'umd',
            name: 'roolupVue'
        }
    ]
}

```

* 同时需要在`packjson.json`修改脚本命令

> "dev": "rollup -wc rollup.config.dev.js",其中之所以使用`-wc` 而不提倡使用`-c`的区别在于额外的`w`监听标志。在 Rollup 中，-w 或 --watch 是一个特殊的标志，它告诉 Rollup 以监听模式运行，即当源文件发生变化时自动重新构建项目。

```
"scripts": {
  "dev": "rollup -wc rollup.config.dev.js"
}
```

* 运行脚本命令

> 运行成功将会显示如下图

```
npm run dev
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240426/1714133497414727083.png)


* 在项目中预览打包效果

> 新建`public/index.html`,引入`muying-rollupvue.js`,然后打开`index.html`直接预览效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>muying-rollupvue</title>
    <script src="../dist/muying-rollupvue.js"></script>
</head>
<body>
</body>
</html>
```

# 二、模块化标准和多模块打包

## 1、format的三种协议

模块|作用|
:--:|:--:|
umd|`umd`模块可以在浏览器和 Node.js 中使用，表示输出一个umd的模块（一个jsfunction函数）。|
cjs|`cjs`(commonjs)是`Node.js` 默认的模块系统。`cjs`模块可以被 Node.js 直接 require。(但不支持浏览器上使用)|
es|`es`是一种es6模块化标准（浏览器不能支持,需要在<script type="module"></script>引入浏览器才能支持）|

* 输出`umd`模块

>  `umd`这是一个旨在兼容所有模块加载器（AMD, CommonJS, 以及全局变量）的模块定义方式。UMD 模块可以在浏览器和 Node.js 中使用，表示输出一个umd的模块（一个jsfunction函数）。

```javascript
const path = require('path')
const inputPath = path.resolve(__dirname, './src/index.js')
const outputUmdPath = path.resolve(__dirname, './dist/muying-rollupvue.umd.js')
module.exports = {
    input: inputPath,
    output: [
        {
            file: outputUmdPath,
            format: 'umd',
            name: 'roolupVue'
        }
    ]
}
```

> 在`public/index.html`文件中引入`umd`打包模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>muying-rollupvue</title>
    <script src="../dist/muying-rollupvue.umd.js"></script>
</head>
<body>
</body>
</html>
```

* 输出`cjs`模块

> `cjs`全称commonjs是`Node.js` 默认的模块系统。CJS 模块可以被 Node.js 直接 require。(但不支持浏览器上使用)

```javascript
const path = require('path')

const inputPath = path.resolve(__dirname, './src/index.js')
const outputCjsPath = path.resolve(__dirname, './dist/muying-rollupvue.cjs.js')
module.exports = {
    input: inputPath,
    output: [
        {
            file: outputCjsPath,
            format: 'cjs',
            name: 'roolupVue'
        }
    ]
}
```

* 输出`es`模块

>es是一种es6模块化标准（浏览器不能支持,需要在<script type="module"></script>引入浏览器才能支持）
，在使用时需要给在引入打包模块的`public/index.html`文件中的`<script>`标签加上`type="module"`

```javascript
const path = require('path')
const inputPath = path.resolve(__dirname, './src/index.js')
const outputEsPath = path.resolve(__dirname, './dist/muying-rollupvue.es.js')
module.exports = {
    input: inputPath,
    output: [
        {
            file: outputEsPath,
            format: 'es',
            name: 'roolupVue'
        }
    ]
}
```

> 在`public/index.html`文件中引入`es`打包模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>muying-rollupvue</title>
    <script src="../dist/muying-rollupvue.es.js" type="module"></script>
</head>
<body>
</body>
</html>
```



* format也支持同时输出多个模块

```javascript
const path = require('path')
const inputPath = path.resolve(__dirname, './src/index.js')
const outputUmdPath = path.resolve(__dirname, './dist/muying-rollupvue.umd.js')
const outputCjsPath = path.resolve(__dirname, './dist/muying-rollupvue.cjs.js')
const outputEsPath = path.resolve(__dirname, './dist/muying-rollupvue.es.js')
module.exports = {
    input: inputPath,
    output: [
        {
            file: outputUmdPath,
            format: 'umd',
            name: 'roolupVue'
        },
        {
            file: outputCjsPath,
            format: 'cjs'
        },
        {
            file: outputEsPath,
            format: 'es',
        }
    ]
}
```

## 2、配置`resolve`插件（为了支持在其他框架上使用）


* 使用`resolve`插件

> resolve插件帮助Rollup查找并解析node_modules中的模块

> 安装`resolve`插件
```
npm i -D @rollup/plugin-node-resolve
```

> 在rollup.config.dev.js中引入`resolve`插件

```javascript
const resolve = require('@rollup/plugin-node-resolve')
module.exports = {
    plugins: [
        resolve()
    ]
}
```

* demo案例，引入使用`sam`包，但是`sam-test-data` 包的 ES 模块构建并没有提供一个默认的导出，所以不能使用浏览器查看，可以使用`CommonJS`导入,使用`require`语法,并通过 Node.js 的 node 命令来运行你的脚本。

> 修改`src/index.js`内容

```javascript
const data = require('sam-test-data')
console.log(data.random(),data.a,data.b,'data')
module.exports={}
```

> 执行`node src/index.js`命令运行查看

* 使用`babel`插件，将es6转化，通过`babel-node src/index.js`命令检查语法

> 使用`babel-node src/index.js`命令来检查babel需要安装的包

```
npm i -D @babel/node
npm i -D @babel/core
```

> 项目中新建`.babelrc`文件，查找模版`babel.config.json`写法，并写入

[babel官网文档](https://babel.dev/docs/usage)

```json
{
    "presets": [
        [
            "@babel/preset-env"
        ]
    ]
}
```

> 安装`babel/preset-env`

```
npm i -D @babel/preset-env
```

> 这时`src/index.js`可以写es6语法，不过不能直接引入需要使用`* as`，或者使用解构方法来获取数据

```javascript
// * as
import * as data from "sam-test-data";
console.log(data.random(), data.a, data.b, 'data')
export default random
```

```javascript
// 解构方法
import {random,a,b} from "sam-test-data";
console.log(random(), a, b, 'data')
export default random
```

* `babel-node`调试方法

> 执行`node src/index.js`命令运行查看，或者输入`babel-node`命令转换到`babel`命令下再执行`require('./src/index.js')`

```
node src/index.js
或者执行
babel-node
require('./src/index.js')
```
> 离开`babel`命令

```
.exit
```

* 在 `rollup.config.dev.js`的plugins中如果不使用`resolve`时

> 重新执行命令`npm run dev`,在组件间使用`babel-node dist/muying-rollupvue.es.js`(umd,cjs都要可以)是可以查看的，但放在桌面上不行,连接不到外部依赖。所以`resolve`模块就是为了解决这方面的问题，将源码和第三方模块进行混合打包

```
plugins: [
        resolve() //resolve插件帮助Rollup查找并解析node_modules中的模块
]
```

## 3、如何使用`tree-shaking`

> Tree shaking 的主要原理是依赖于 ES6 模块系统中的静态结构特性，通过消除 JavaScript 上下文中的未引用代码（即，你的项目中未使用的模块或导出）来优化你的最终打包文件大小的技术。这个术语通常与 ES6 模块和诸如 Webpack、Rollup 这样的现代模块打包器一起使用。

* 新建`src/plugin.js`

```javascript
const a=1;
const b=2;
function random(params) {
    console.log(`random`)
}
export {
    random,a,b
}
```

* `src/index.js`修改

```javascript
import {random,a,b} from "./plugin";
console.log(random(), a, b, 'data')
export default random
```

* 执行`babel-node`调试命令

```
babel-node src/index.js
```

* 当只使用了random（a,b未使用时）执行`npm run dev`命令查看打包内容

> 会看到dist文件夹下将不必要的a，b内容也进行了打包，而我们只需要random内容

```
npm run dev
```

* 解决此类办法使用`tree shaking`

> 修改`plugin.js`的内容,采用export 直接导出内容

```javascript
export const a=1;
export const b=2;
export function random(params) {
    console.log(`random`)
}
```

> 修改`plugin.js`的内容

```javascript
import { random} from "./plugin";
console.log(random(),'random')
```

## 4、`external`属性

> 有些场景下，虽然我们使用了resolve插件，但我们仍然某些库保持外部引用状态，这时我们就需要使用external属性，告诉rollup.]s哪些是外部的类库,修改rollup.js的配置文件。因为一般引用的框文件太大，打包回变慢。所以依旧采用外部引入（如vue，react）。如在`rollup.config.dev.js`中加上`external`属性,将打包的库文件依旧以外部状态引入。

```
external:[
        'sam-test-data'
]
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714200776189541899.png)

## 5、`commonjs`插件

* 使用`commonjs`

> rollup.js默认不支持CommonJS模块,需要将CommonJS模块转换为ES6，这样Rollup就可以处理它们。如下面的经典案例，新建`src/cjs.js`文件

```javascript
const a=1
module.exports=a
```

> `src/index.js`中引入`cjs.js`内容

```javascript
import data from "./cjs";
console.log(data)
export default data
```

> 输入babel-node调试命令,发现可以运行

```
 babel-node ./src/index.js
```

> 输入npm run serve打包，发现模块找不到打包错误

```
npm run dev
```

> 这时解决方法是使用`commonjs`插件

```
npm i -D @rollup/plugin-commonjs
```

> 在`rollup.config.dev.js`进行引入commonjs插件

```javascript
const commonjs = require('@rollup/plugin-commonjs')
module.exports = {
    plugins: [
        commonjs() //将CommonJS模块转换为ES6，这样Rollup就可以处理它们
    ]
}
```

* `commonjs`中的`tree-shaking`

> 修改`src/cjs.js`文件

```javascript
exports.a=1
exports.b=2
```

> 修改`src/cjs.js`文件

```javascript
import {a,b} from "./cjs";
console.log(a)
export default a
```

## 6、`babel`插件

* 修改`src/index.js`内容然后运行`npm run dev`

> 会发现打包模块并未进行转义，容易在低版本中报错

```javascript
const a=()=>{
    return 3;
}
export default a
```

* 使用`babel`插件

```
npm i -D @rollup/plugin-babel
```

> 在`rollup.config.dev.js`进行引入babel插件,使其可以在低版本中运行

```javascript
const babel = require('@rollup/plugin-babel')
module.exports = {
    plugins: [
        babel({
            exclude: 'node_modules/**'// 哪些模块不进行babel编译
        })
    ]
}
```

## 7、`json`插件

* 引入packjson.json

> 修改`src/index.js`

```javascript
import pkg from "../package.json";
console.log(pkg,'pkg')
```

> 使用`babel-node`命令调试,并可以进行运行并查看到`packjson.json`内容

```
babel-node src/index.js
```

> 使用`npm run dev`命令运行输出，会发现报错（json文件是不支持模块打包的）

```
npm run dev
```

* 使用`json`插件

> 安装json插件

```
npm i -D @rollup/plugin-json
```

> 在`rollup.config.dev.js`进行引入json插件,使其可以在低版本中运行

```javascript
const json = require('@rollup/plugin-json')
module.exports = {
    plugins: [
        json()
    ]
}
```

## 8、`terser`插件

> 把打包文件进行最小化压缩

* 使用`terser`插件

> 安装json插件

```
npm i -D @rollup/plugin-terser
```

> 由于在开发模式下并不需要进行压缩，所以将`rollup.config.dev.js`拷贝一份到`rollup.config.prod.js`中，再在prod中使用terser插件

```javascript
const terser = require('@rollup/plugin-terser')
module.exports = {
    plugins: [
        terser()
    ]
}
```

> 在`packjson.json`中修改scripts调试命令,build是进行打包工具，dev是开发者监听模式

```json
"scripts": {
    "dev": "rollup -wc rollup.config.dev.js",
    "build": "rollup -c rollup.config.dev.js",
    "build:prod": "rollup -c rollup.config.prod.js"
  },
```

> 执行打包和监听模式命令
```
npm run build
npm run build:prod
npm run dev
```

> 当执行`npm run build:prod`时会发生报错，报错要求我们通过解构方法获取

![029082bbacc225ad146a43ef722d20e.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714204513122723339.png)

```
const {terser} = require('@rollup/plugin-terser')
```

> 发现好像不是该问题，找不到具体解决办法，采用了低版本的`rollup`和`terser`

```
npm i -D rollup-plugin-terser@^7.0.2
npm i -D rollup@^2.35.1
```

> 如果安装指定版本不行，可以采取直接复制版本到`devDependencies`,然后删除`node_modules`,重新安装`npm -i`

```json
"devDependencies": {
    "@babel/core": "^7.24.4",
    "@babel/node": "^7.23.9",
    "@babel/preset-env": "^7.24.4",
    "@rollup/plugin-babel": "^6.0.4",
    "@rollup/plugin-commonjs": "^25.0.7",
    "@rollup/plugin-json": "^6.1.0",
    "@rollup/plugin-node-resolve": "^15.2.3",
    "rollup-plugin-terser": "^7.0.2",
    "rollup": "^2.35.1"
}
```

> 然后修改`rollup.config.prod.js`

```
const {terser} = require('rollup-plugin-terser')
```

> 然后执行压缩打包

```
npm run build:prod
```

* 项目html源码链接：[muying-html](https://github.com/Muying-Zhao/muying-rollupvue)

# 三、将rollup组件打包成vue组件库

## 1、`vue`插件

* 安装`vue`插件

```
npm i -D rollup i -D rollup-plugin-vue
npm i -D @vue/compiler-sfc
npm i -D rollup-plugin-postcss
npm i -D sass
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714222552008667117.png)

* 使用`vue`插件

> 如在`rollup.config.dev.js`中引入插件，注意`commonjs()`要放在`vue()`之后，不然会发生报错，`rollup.config.prod.js`同样也需要加入

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714222694646726449.png)

```javascript
const resolve = require('@rollup/plugin-node-resolve')
const commonjs = require('@rollup/plugin-commonjs')
const babel = require('@rollup/plugin-babel')
const json = require('@rollup/plugin-json')
const vue = require('rollup-plugin-vue')
const postcss = require('rollup-plugin-postcss')

const path = require('path')

const inputPath = path.resolve(__dirname, './src/index.js')
const outputUmdPath = path.resolve(__dirname, './dist/muying-rollupvue.js')
const outputCjsPath = path.resolve(__dirname, './dist/muying-rollupvue.cjs.js')
const outputEsPath = path.resolve(__dirname, './dist/muying-rollupvue.es.js')

module.exports = {
    input: inputPath,
    output: [
        {
            file: outputUmdPath,
            format: 'umd',
            name: 'roolupVue'
        },
        {
            file: outputCjsPath,
            format: 'cjs'
        },
        {
            file: outputEsPath,
            format: 'es',
        }
    ],
    plugins: [
        vue(),
        resolve(), //resolve插件帮助Rollup查找并解析node_modules中的模块
        commonjs(),
        babel({
            exclude: 'node_modules/**'// 哪些模块不进行babel编译
        }),
        json(),
        postcss({
            plugins:[]
        })
    ],
    external:[
        'vue'
    ]
}
```

* 新建`src/TestDemo.vue`文件

```html
<template>
    <div class="wrapper">hello world</div>
</template>
<script>
export default {
    name:'TestDemo'
}
</script>
<style scoped lang="scss">
.wrapper{
   width: 100px;
   height: 100px; 
   background: orange;
}
</style>
```

* 修改`src/index.js`内容

```javascript
import TestDemo from "./TestDemo.vue";
export default function (Vue) {
    Vue.component(TestDemo.name, TestDemo)
}
```

* 执行打包命令

```
npm run build
npm run build:prod
```

## 2、解决警告warn问题

* global警告
> 这个警告通常意味着你的代码或某个依赖试图访问一个全局变量，但 Rollup 无法确定这个全局变量的名称。这可能会导致运行时错误，因为 Rollup 无法在构建过程中包含这个全局变量。解决办法是在输出模块安装全局`globals:{ 'vue':'vue'}`

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714222966654844618.png)

```javascript
 output: [
        {
            globals:{
                'vue':'vue'
            }
        },
]
```

## 3、本地演示使用vue

* `public/index.js`,引入打包的文件和使用vue.js在线版本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>muying-rollupvue</title>
    <!-- vue的开发环境 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@3.2.4/dist/vue.global.prod.js"></script>
    <script src="../dist/muying-rollupvue.umd.js"></script>
</head>
<body>
    <div id="app">{{message}}</div>
    <script>
        Vue.createApp({
            setup() {
                var message = 'hello datav!'
                return {
                    message
                }
            }
        }).mount('#app')
    </script>
</body>
</html>
```

* 使用`TestDemo`组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>muying-rollupvue</title>
    <!-- vue的开发环境 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@3.2.4/dist/vue.global.prod.js"></script>
    <script src="../dist/muying-rollupvue.umd.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
        <test-demo></test-demo>
    </div>
    <script>
        Vue.createApp({
            setup() {
                var message = 'hello world'
                return {
                    message
                }
            }
        }).use(window.roolupVue).mount('#app')
    </script>
</body>
</html>
```

> 发生了如下报错，vue.global.prod.js:1 TypeError: vue.openBlock is not a function，
解决办法是打开`muying-rollupvue.umd.js`文件找到`factory(global.vue)`将其改为`factory(global.Vue)`

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240427/1714227812772404707.png)

# 四、npm link映射+eslint初始化

## 1、npm link映射

* 修改`packjson.json`中的`"main": "index.js"`

> 将输出umd模块中`'./dist/muying-rollupvue.umd.js`写成`'./dist/muying-rollupvue.js`

```
"main": "dist/muying-rollupvue.js",
"files": [
    "dist",
    "src"
  ],
"keywords": [
    "vue",
    "muying",
    "rollupvue"
],
```

* 映射链接

```
npm link
```

* 由于未在npm上线，所以需要再使用该组件时添加依赖

> 在新的vue项目中添加依赖

```json
"dependencies": {
    "muying-rollupvue":"1.0.0"
  },
```

* 安装映射链接

> 会在`node_modules`中出现`muying-rollupvue`打包的库

```
npm link muying-rollupvue
```

* 在新项目中引入打包模块

> .use() 是一个全局 API，用于安装 Vue 插件

```javascript
import datas from "muying-rollupvue";
createApp(Home)
.use(datas)
.mount('#app')
```

* 执行`npm run serve`命令发现报错，原是`muying-rollupvue`需要进行`eslint`初始化

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240428/1714236024708645777.png)

* 在`muying-rollupvue`项目中安装`eslint`依赖

> 安装eslint，事先执行版本，删除`node_modules，`重新安装

```json
"devDependencies": {
    "eslint": "^7.32.0",
    "eslint-plugin-vue": "^9.25.0",
  }
```

> 执行eslint命令

```
./node_modules/.bin/eslint --init
```

> 选择第一个`To check syntax only`仅仅检查语法，空格勾选，回车确认 

> 选择模块第一个` JavaScript modules (import/export)`

> 选择框架 `Vue.js`,

> 是否使用TypeScript语法，`no`

> 在什么环境下运行代码，选择`Browser`浏览器

> format输出模块`JavaScript`

> 它询问您是否希望现在安装这些依赖项，选择`Yes`

> 然后会自动生成`.eslintrc.js`文件,在`.eslintrc.js`文件中进行配置和修改

```javascript
/*
ESLint 是一个开源的 JavaScript 代码检查工具，
用于识别和报告代码模式，这些模式可能是错误、可能的问题，
或者只是代码风格的约定。
*/
module.exports = {
    /* env: 这个对象定义了代码运行的环境。 */
    "env": {
        "browser": true,//浏览器（browser）
        "es2020": true
    },
    // extends: 这个属性指定了继承的基础配置
    "extends": "plugin:vue/essential",
    //parserOptions: 这个对象定义了 ESLint 解析器的选项
    "parserOptions": {
        "ecmaVersion": 12,//ecmaVersion 设置为 12，意味着解析器将支持 ECMAScript 2021（即 ES12）的特性
        "sourceType": "module"//sourceType 设置为 module，表明代码将使用 ES6 模块语法。
    },
    /* plugins: 这个数组列出了 ESLint 使用的插件 */
    "plugins": [
        "vue"
    ],
    "rules": {
    }
};
```

* 在`packjson.json`中`scripts`中加上`eslint`检查，

> 这时可以解决之前打包模块的vue大小写问题，当eslint检查不通过的时候，是不允许build

```json
"scripts": {
    "dev": "rollup -wc rollup.config.dev.js",
    "build": "eslint ./src && rollup -c rollup.config.dev.js",
    "build:prod": "eslint ./src && rollup -c rollup.config.prod.js",
    "lint": "eslint ./src"
},
```

> 通过 `npm run lint`进行eslint检查

```
npm run lint
```

> 然后执行`npm run build`和`npm run build:prod`进行打包，发现`muying-rollupvue.js`中的`global.vue`未变，依旧需要手动更改,解决办法是在`rollup.config.dev.js`和`rollup.config.prod.js`中，将协议模块中global改变成大写`Vue`。可以给三方模块都添上

```
globals: {
     'vue': 'Vue'
}
```

> 重新映射link

```
npm link
```

> 在新的项目vue中进行link

```
npm link muying-rollupvue
```

> 在新的vue项目中就可以直接使用`test-demo`组件

# 五、按需加载

## 1、`muying-rollupvue`

* 新建`src/components/TestDemo1`,在其文件夹下新建`index.js和TestDemo1`

* `TestDemo1/index.js`

```javascript
import TestDemo1 from "./TestDemo1.vue";
export default function (Vue) {
    Vue.component(TestDemo1.name, TestDemo1)
}
```

* `TestDemo1/TestDemo1.vue`

```html
<template>
    <div class="wrapper">
        <div class="box1">{{ message }}</div>
    </div>
</template>
<script>
export default {
    name: 'TestDemo1',
    setup() {
        const message = 'hello world'
        return {
            message
        }
    }
}
</script>
<style scoped lang="scss">
.wrapper {
    width: 100px;
    height: 100px;
    background: orange;
    .box1{
        background: aquamarine;
    }
}
</style>
```

* 其他组件类似(TestDemo2)，然后在`src/index.js`下导出

```javascript
import TestDemo1 from "./components/TestDemo1/index";
import TestDemo2 from "./components/TestDemo2/index";
export default function (Vue) {
    Vue.use(TestDemo1)
    Vue.use(TestDemo2)
}
```

## 2、在新建的vue项目中link组件，

* 在`src/main.js`中引入(全部加载`muying-rollupvue`组件内容)

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import datas from 'muying-rollupvue'

createApp(App)
    .use(datas)
    .mount('#app')
```

## 3、按需加载组件内容,在`main.js`中

```Javascript
import { createApp } from 'vue'
import App from './App.vue'
// 按需加载找到指定需要的组件
import TestDemo1 from 'muying-rollupvue/src/components/TestDemo1/index'
createApp(App)
    .use(TestDemo1)
    .mount('#app')
```

# 六、vue2组件库兼容方案

## 1、新建vue2项目

* `package.json`中写入库的版本

```json
"dependencies": {
    "muying-rollupvue":"^1.0.0"
  },
```

* link组件

```
npm link muying-rollupvue
```

* 在`src/main.js`中使用组件

```javascript
import Vue from 'vue'
import App from './App.vue'
import datav from "muying-rollupvue";

Vue.config.productionTip = false
Vue.use(datav)

new Vue({
  render: h => h(App),
}).$mount('#app')
```

* 在App.vue中使用`<test-demo1>`组件

> 使用vue3组件在vue2中会报错，不兼容

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240428/1714275305944681776.png)

```
<test-demo1></test-demo1>
```

## 2、降低`muying-rollupvue`版本到vue2

* 在`package.json`中降低vue版本和去掉compiler-sfc,

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240428/1714275677624833089.png)

```
"rollup-plugin-vue": "^5.0.0",
```

* 重新安装依赖

```
npm i
```

* 安装新的vue插件(@^2.6.11)

```
npm i -D vue-template-compiler
```

* 将`src/components`里的内容全部改成vue2语法

* 重新打包、npm link，重新引用组件就可以运行了

* vue2组件可以被vue3项目使用，但vue2组件中使用不了vue3语法

> vue2的过滤器语法在vue3项目中是不能使用的

```html
<template>
    <div class="wrapper">
        {{ message | add }}
    </div>
</template>

<script>
  export default {
    name:'TestDemo3',
    filters: {
        add(v){
            return v+'!'
        }
    },
    data(){
        return {
            message:'hello world'
        }
    }
  }
</script>
<style>
.wrapper {
    width: 100px;
    height: 100px;
    background: orange;
}
</style>
```
