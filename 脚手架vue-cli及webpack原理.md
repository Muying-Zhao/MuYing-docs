# 前言

> Vue CLI是Vue.js项目的官方脚手架，基于Node.js与Webpack构建。安装Vue CLI前需确保Node.js已安装，随后通过npm全局安装。Vue CLI能迅速创建和管理Vue.js项目，提升开发效率。而Webpack则负责资源打包，通过配置文件管理依赖、插件等，优化代码并分割资源，提高项目性能。Vue CLI与Webpack的完美结合，让开发者更专注于业务逻辑，轻松构建高效、稳定的Vue.js应用。

# 一、Vue CLI脚手架

## 1、Vue单文件组件

> Vue 单文件组件（又名*.vue文件，Single File Components缩写SFC）是 Vue.js 框架中的一种组件编写方式，它允许我们将一个组件的模板`template`、逻辑`script`和样式`style`封装在单个文件中。这种方式使得组件的代码更加清晰、易于管理和维护。

## 2、使用Vue CLI脚手架

> Vue单文件组件使用的组件模版，逻辑和样式编写方式在浏览器中是不被认识的。所以我们需要使用Vue CLI提供的一套预定义的配置和插件系统，使得 Vue 组件能够被识别和正确构建。其中Vue CLI已经算是一个功能完备的 Vue.js 项目脚手架，它提供了一套标准化的项目结构和工具链，用于快速构建 Vue.js 项目。它的主要目标是帮助开发者更高效地创建、开发和维护 Vue.js 应用程序。

* 需要首先在本地安装node.js

* 安装vue cli脚手架

> 输入cmd打开命令行工具，输入命令

`npm install -g @vue/cli@5.0.6`

* 桌面创建vue项目

`vue create vue-study`

* Vue CLI 选项

> 选择默认vue3项目

`Default([Vue 3] babel, eslint)`

![6cb05ba85991fda5c4038ad1aa0f4f2.png](https://bbs-img.huaweicloud.com/blogs/img/20240424/1713891412370891268.png)

* 创建package.json

> 打开项目文件夹，输入cmd命令或者将项目放入vscod中，打开终端，先创建`package.json`文件

`npm init -y`

* 运行项目

`npm run serve`

# 二、解析Vue CLI脚手架原理和webpack使用

## 1、使用webpack

> Webpack 是一个模块打包工具，它可以将多个模块（如 JavaScript、CSS、图片等）打包成一个或多个 bundle，供浏览器使用。在 Vue CLI 或其他现代前端脚手架中，Webpack 通常被用作构建工具，负责处理项目中的资源，并优化输出。

* 创建空文件夹，然后安装`package.json`

```
npm init -y
```

* 安装webpack

```
npm install --save-dev-webpack
npm install --save-dev-webpack-cli
或者执行
npm i -D webpack webpack-cli
```

* 创建`webpack.config.js`

> 配置文件webpack.config.js,通过安装的`webpack-cli`来调用，这个文件是Webpack的核心配置文件，它告诉Webpack如何处理项目中的模块、插件以及其他配置选项。

```
module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist' // 输出的路径path
    },
    // 开发环境
    mode: 'development'
}
```

* 在`package.json`中写入脚本命令`"build": "webpack"`调用配置文件

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```

* 创建文件夹src(开发所在区域)

> 并在其里面创建`index.js`和`main.js`文件。src 文件夹通常是用于存放项目的源代码。

* index.js(项目的入口点)

```
let str='hello world'
export default str;
```

* main.js(主模块)

```
import str from "./index.js";
console.log(str)
```

* 执行`npm run build`命令生成打包dist文件

```
npm run build
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240424/1713957847808454700.png)

* 预览`src`输出的内容

> 在dist文件夹下新建index.html,输入`!+回车`写入基本的HTML文档结构，引入`main.js`打包模块，然后运行`index.html`文件，就可以看到控制台输出的内容了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./main.js"></script>
</head>
<body>
</body>
</html>
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240424/1713958506381301673.png)

## 2、使用Webpack的HtmlWebpackPlugin插件

* 安装`HtmlWebpackPlugin`插件

> HtmlWebpackPlugin插件主要作用是根据Webpack构建配置，自动生成一个或多个HTML文件，并将打包生成的JavaScript和CSS文件自动引入到这些HTML文件中。在以下链接中找到`HtmlWebpackPlugin`，点击想要使用的插件，会提示安装和引入过程

[webpack官网HtmlWebpackPlugin](https://www.webpackjs.com/plugins/html-webpack-plugin)

```
npm install --save-dev html-webpack-plugin
```

* 在`webpack.config.js`文件中引入`HtmlWebpackPlugin`插件

> `template`的作用主要是指定一个 HTML 文件作为模板，webpack 和 html-webpack-plugin 会使用这个模板来生成最终的 HTML 文件。

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist', // 输出的路径path
        clean:true //清理打包的dist中不必要的文件
    },
    // 引入webpack插件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'webpack study'
        })
    ],
    // 开发环境
    mode: 'development'
}
```

* 在项目中创建`public`文件夹，在其内部创建`index.html`

> 在`index.html`里写入如下代码，并删除dist文件重新执行`npm run build`命令,这是创建的dist文件夹下会自动存在`index.html`并引入了`main.js`模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
</body>
</html>
```

## 3、vue.config.js与webpack.config.js的联系

* vue.config.js

> vue.config.js 是 Vue CLI 项目的配置文件，用于自定义 Vue CLI 项目的 webpack 配置和插件选项。

* webpack.config.js

> webpack.config.js 是 webpack 的主要配置文件，用于定义 webpack 构建的配置对象，包含 loader、插件等详细的配置。

## 4、webpack启动服务器运行环境

> 使用webpack-dev-server来搭建一个本地开发服务器。webpack-dev-server的主要功能包括自动监听工程文件的变动，并在文件变动时自动打包并刷新浏览器，从而提供实时重载的开发体验。

*  安装webpack启动服务器

```
npm i -D webpack-dev-server
```

* 在`package.json`中写入脚本命令`"serve": "webpack-dev-server"`调用配置文件

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "serve": "webpack-dev-server"
  },
```

* 启动webwebpack启动服务器

> 在启动webwebpack启动服务器后，修改文件内容是会刷新浏览器的,但这个操作与打包是不会改变成dist文件。在运行webwebpack启动服务器是不需要dist文件的，项目完成后才需要`npm run build`打包成dist文件夹

```
npm run serve
```

* 打开本地开发服务器的URL地址

```
http://localhost:8081/
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240424/1713964016086499699.png)

## 5、dist与src的映射:

> 在`webpack.config.js`中写入`devtool: 'inline-source-map'`，这是一个source maps映射文件，当你在浏览器的开发者工具中打开这些打包文件时，开发者工具会读取内嵌的source map信息，并显示原始的src目录下的源代码，而不是打包后的代码。

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist', // 输出的路径path
        clean:true //清理打包的dist中不必要的文件
    },
    devtool: 'inline-source-map',
    // 引入webpack插件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'webpack study'
        })
    ],
    // 开发环境
    mode: 'development'
}
```

# 三、webpack处理样式模块和图片模块

> Webpack支持使用各种 loader 来预处理不同类型的文件，包括样式模块和图片模块。这些 loader 可以将不同类型的文件转换为 JavaScript 模块，以便 Webpack 能够将其打包到最终的 bundle 中。

[loader介绍](https://www.webpackjs.com/loaders/#root)

## 1、css-loader使用

> Webpack 支持使用 loader 对文件进行预处理。你可以构建包括 JavaScript 在内的任何静态资源。并且可以使用 Node.js 轻松编写自己的 loader。

* 安装css-loader

> css-loader 加载 CSS 文件并解析 import 的 CSS 文件，最终返回 CSS 代码

```
npm i -D style-loader css-loader
```

* 在webpack.config.js中配置模版`module`css处理规则

> use: ['style-loader', 'css-loader']：这是一个数组，指定了处理这些文件时要使用的 loader。Webpack 会按照数组中的逆序（从右到左）来应用这些 loader。其中这个`css-loade`会读取 CSS 文件内容，并将其转换为 CommonJS 模块。这样，Webpack 就可以处理 CSS 文件，就像处理 JavaScript 文件一样。而这个`style-loader`会将 CSS 插入到 HTML 文档的`style`标签中。它依赖于 css-loader，因为它从 css-loader 接收 CSS 内容，并将其注入到页面中。

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist', // 输出的路径path
        clean:true //清理打包的dist中不必要的文件
    },
    devtool: 'inline-source-map',
    module: {
        rules: [
            {
                test: /\.css$/i, //匹配所有以 .css 结尾的文件
                use: ['style-loader','css-loader']
            }
        ]
    },
    // 引入webpack插件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'webpack study'
        })
    ],
    // 开发环境
    mode: 'development'
}
```

* 在src文件夹下新建一个assets文件夹

> assets 文件夹通常用于存放项目中不直接通过源代码引用的静态资源文件。在文件夹中新建`.css`文件，如`common.css`

```html
body{
    background: pink;
}
```

* 在src中的`main.js`文件中引入`common.css`文件

```html
import str from "./index.js";
import './assets/common.css'
console.log(str)
```

* 执行`npm run serve`启动webwebpack启动服务器

> 打开本地开发服务器的URL地址，将会看到css文件内容成功被引入进来，页面背景将会显示为粉色。

```
npm run serve
```

## 2、webpack处理图片模块

* 安装`file-loader` 和 `url-loader`

> file-loader和url-loader可以将图片文件转换为模块

```
npm i -D file-loader url-loader
```

* 在webpack.config.js中配置模版`module`图片处理规则

```javascript
module: {
        rules: [
            {
                test: /\.css$/i, //匹配所有以 .css 结尾的文件
                use: ['style-loader','css-loader']
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/i,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: 8192, // 小于 8kb 的图片转为 base64 格式  
                            name: '[name].[hash:7].[ext]', // 输出的文件名格式  
                            outputPath: 'images', // 输出到 dist/images 目录下  
                        },
                    },
                ],
            }
        ],
    },
```

* 在`public`文件夹下使用`img`标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <img src="" alt="">
</body>
</html>
```

* 在assets文件夹下存放一张图片（我存放的是一张`logo.jpeg`）

* 在src下的`main.js`文件中导入的图片资源

```html
import str from "./index.js";
import './assets/common.css'
import logo from './assets/logo.jpeg'
let img=document.querySelector('img')
img.src=logo
console.log(str)
```

* 执行`npm run serve`启动webwebpack启动服务器

> 打开本地开发服务器的URL地址，将会图片和css文件内容成功被引入。

## 3、新的webpack方式处理图片模块

> 随着webpack的升级，特别是从webpack 5.0开始，处理图片资源的方式变得更加简洁和高效。webpack 5.0内置了对图片资源处理的支持，这使得开发者无需再额外安装如file-loader或url-loader等加载器来处理图片。

* 配置`asset/resource`模块

> 将`webpack.config.js`中配置模版替换成asset/resource模块，通过配置asset/resource模块类型来处理图片资源。webpack会自动将图片文件输出到输出目录，并在代码中生成对应的URL。

```javascript
module: {
        rules: [
            {
                test: /\.css$/i, //匹配所有以 .css 结尾的文件
                use: ['style-loader','css-loader']
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/i,
                type: 'asset/resource'
            }
        ],
    },
```

* 执行`npm run serve`启动webwebpack启动服务器

> 打开本地开发服务器的URL地址，将会图片和css文件内容成功被引入。

## 4、sass-loader（简称Scss）使用

* 安装Scss插件

```
npm i -D sass
npm i -D sass-loader
npm i -D vue-style-loader
```

* `webpack.config.js`中配置`sass-loader`规则

> 分别在module中配置SCSS或SASS规则，其中SCSS/SASS文件不是处理资源文件，它们会在Webpack的模块处理管道中自动运行。所以不需要再引入插件、

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { VueLoaderPlugin }=require('vue-loader')

module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist', // 输出的路径path
        clean:true //清理打包的dist中不必要的文件
    },
    devtool: 'inline-source-map',
    module: {
        rules: [
            {
                test: /\.css$/i, //匹配所有以 .css 结尾的文件
                use: ['style-loader','css-loader']
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/i,
                type: 'asset/resource'
            },
            {
                test: /\.vue$/i,
                use: ['vue-loader']
            },
            // 如果需要处理SCSS或SASS，可以添加如下规则  
            {
                test: /\.scss$/,
                use: [
                    'vue-style-loader',
                    'css-loader',
                    'sass-loader'
                ]
            },
        ],
    },
    // 引入webpack插件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'webpack study'
        }),
        new VueLoaderPlugin()
    ],
    // 开发环境
    mode: 'development'
}
```

## 5、在Home.vue中使用scss语法


* 使用scss语法一：嵌套

> SCSS允许你使用嵌套来简化CSS代码的书写，这样可以更加清晰地表达元素之间的层次关系。

```html
<template>
    <div class="wrapper">
      <div class="box">box</div>
    </div>
</template>
<script>
export default {
    name:'Home',
    data(){
       return {}
    }
}
</script>
<style scoped lang="scss">
.wrapper{
    background: orange;
    .box{
        background: pink;
    }
}
</style>
```

* 使用scss语法二：纯CSS的嵌套等效写法

```html
<template>
    <div class="wrapper">
      <div class="wrapper__box1">hello world</div>
      <div class="wrapper__box2">world hello</div>
      <div class="wrapper__box2 wrapper__box2-active">welcome vue</div>
    </div>
</template>
<script>
export default {
    name:'Home',
    data(){
       return {}
    }
}
</script>
<style scoped lang="scss">
.wrapper{
    background: orange;
    &__box1{
        background: green;
    }
    &__box2{
        background: red;
        &-active{
            background: aqua;
        }
    }
}
</style>
```

* 使用scss语法三：变量

> 在SCSS中，你可以使用变量来存储常用的CSS属性值，并在后续的代码中调用这些变量。变量名以美元符号$开头，后面跟变量名，变量名可以包含字母、数字、下划线、横线（连接符），并且变量名与值之间用冒号:分隔。变量必须先定义后使用。

```html
<template>
    <div class="wrapper">
      <div class="wrapper__box">hello world</div>
    </div>
</template>
export default {
    name:'Home',
    data(){
       return {}
    }
}
</script>
<style scoped lang="scss">
$primary-color: #007bff;
.wrapper{
    background: orange;
    &__box{
        background: $primary-color;
    }
}
</style>
```

* 使用scss语法四：混合

> 混合允许你定义一组CSS属性，并在后续的代码中调用这个混合

```html
<template>
    <div class="wrapper">
      <div class="wrapper__box">hello world</div>
    </div>
</template>
<script>
export default {
    name:'Home',
    data(){
       return {}
    }
}
</script>
<style scoped lang="scss">
@mixin error-style {
  background: #007bff;
}
.wrapper{
    background: orange;
    &__box{
        @include error-style;
    }
}
</style>
```

# 四、webpack处理单文件组件及loader转换

## 1、Vue CLI创建的项目

* 安装`Vue.js`框架

```
npm install --save-dev vue
```

* 安装Vue 3单文件组件(SFC)的编译器

> 安装Vue 3单文件组件(SFC)的编译器，将.vue文件编译成JavaScript和CSS代码的功能。

```
npm install --save-dev @vue/compiler-sfc
```

* 安装vue-loader

> 处理Vue单文件组件,将多种静态资源（如JavaScript、CSS、图片等）转换为浏览器可以使用的格式。

```
npm install --save-dev vue-loader
```

* 在`webpack.config.js`配置中引入`vue-loader`并进行预处理

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { VueLoaderPlugin }=require('vue-loader')

module.exports ={
    // entry打包的入口
    entry: {
        main: './src/main.js'
    },
    //output输出文件,__dirname是找当前文件根目录
    output:{
        path: __dirname + '/dist', // 输出的路径path
        clean:true //清理打包的dist中不必要的文件
    },
    devtool: 'inline-source-map',
    module: {
        rules: [
            {
                test: /\.css$/i, //匹配所有以 .css 结尾的文件
                use: ['style-loader','css-loader']
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/i,
                type: 'asset/resource'
            },
            {
                test: /\.vue$/i,
                use: ['vue-loader']
            }
        ],
    },
    // 引入webpack插件
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'webpack study'
        }),
        new VueLoaderPlugin()
    ],
    // 开发环境
    mode: 'development'
} 
```

* 在`src`文件夹下创建`Home.vue`

```html
<template>
    <div class="wrapper" @click="handleClick">{{ message }}</div>
</template>
<script>
export default {
    name:'Home',
    data(){
       return {
        message: 'hello world'
       }
    },
    methods:{
        handleClick(){
            this.message='world hello'
        }
    }
}
</script>
<style scoped>
.wrapper{
    background: orange;
}
</style>
```

* 修改`public/index.html`文件

> 在`public/index.html`文件中写入一个div标签，并为其指定一个唯一的id，例如app

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

* 修改`src/main.js`,引入单文件`Home.vue`

```javascript
import { createApp } from 'vue'
import Home from './Home.vue'
createApp(Home).mount('#app')
```

* 执行`npm run serve`启动webwebpack启动服务器

> 打开本地开发服务器的URL地址，将会看到Vue单文件组件成功被引入

# 五、安装vue和Chrome插件

## 1、Chrome安装Vue插件vue-devtools

> Vue.js devtools是一个Chrome浏览器插件，主要用于调试和检查Vue.js应用。它提供了一个清晰、可视化的组件树和状态树，使开发人员能够更好地了解应用程序的状态和结构。通过这个功能，开发人员可以更好地管理应用程序的状态和数据，并轻松了解整个应用程序的工作流程。

* 访问Chrome应用商店

[Chrome应用商店](chrome://extensions/)

> 直接在Chrome应用商店中搜索“Vue.js devtools”，这是由Vue.js官方发布的调试工具

* 安装插件

> 在搜索结果中找到Vue.js devtools，然后点击“添加至Chrome”按钮进行安装。

* 启用插件

> 安装完成后，打开您的Vue项目，打开Chrome浏览器的开发者工具（通常按F12键）会显示一个新的`Vue`标签，这表明vue-devtools已经成功安装并启用了。

* 使用插件

> 点击某个组件，还可以看到该组件对应的真实DOM，传入的props、data等详细信息。


![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240425/1714032598418831906.png)

## 2、安装VSCode里的vue常用插件

* Vue Language Features (Volar)

> 是一个专为Vue、Vitepress和petite-vue构建的语言支持扩展的插件，这个插件实现了原生TypeScript语言服务级别的性能。同时还能使开发者能够在VS Code编辑器中更流畅、更高效地编写Vue代码，通过智能代码提示、错误检查等功能，极大地提升了开发体验。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240425/1714033104502818409.png)

* Vue VSCode Snippets

> 通过预定义一系列的快捷码，帮助开发者在编写Vue代码时，能够更快速、更高效地生成常见的模板代码结构。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240425/1714033135640432517.png)
