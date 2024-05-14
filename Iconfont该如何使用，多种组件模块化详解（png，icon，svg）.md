# 前言


> Iconfont是一个功能强大、资源丰富的在线图标库，通过掌握其使用方法和多种组件的模块化应用技巧，我们可以更好地利用图标进行设计和管理，提升作品的质量和用户体验。接下来，本文将详细介绍Iconfont的使用方法以及各种图标格式的模块化应用技巧


- iconfont官网：<a href="https://www.iconfont.cn/" target="_blank">iconfont-阿里巴巴矢量图标库</a>    

# 一、下载png格式 

* 前端网页开发科通过img标签引入图片

```html
<img src="./avatar.png" alt="" /> 
```
* 微信小程序等用image标签引入图片

```html
<image src="./avatar.png" /> 
```

# 二、复制Svg代码 ，直接使用  

```html
<svg t="1713362279012" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="993" width="200" height="200"><path d="M766.492701 468.808534c-44.584739 68.326113-124.725808 106.780451-218.35376 119.598563 4.458474 25.636225 0 63.979101 22.292369 102.433439 44.584739 81.144226 80.252531 170.871013 93.516491 234.961576 75.794057 29.871775 231.729183-42.689888 276.20246-136.652226 49.043213-102.5449 0-205.089801-80.141069-213.560902s-124.725808 34.107326-115.80886 81.144226 53.390225 46.925438 102.433438 17.053663 93.516491 34.218787 40.126266 59.855012-133.754218 46.925438-178.338958-4.347012c-49.043213-51.27245-44.584739-234.850114 151.476652-217.796451S1047.042174 780.232938 984.735 861.711549C922.316365 955.673887 846.63377 1024 624.490307 1024H267.812392c-89.169479 0-26.750844-179.342114 53.390226-328.812452 22.29237-42.80135 17.833896-76.908675 22.292369-102.544901-97.974965-13.041036-178.338957-51.27245-218.465222-119.598563H67.181065a27.419615 27.419615 0 0 1-22.292369-25.636225 20.620442 20.620442 0 0 1 22.292369-21.400675h35.667792a121.270491 121.270491 0 0 1-8.916948-42.689888H22.596326a21.400675 21.400675 0 1 1 0-42.689888h66.877109a224.372701 224.372701 0 0 1 44.584739-140.999238V28.868619c0-21.400675 17.833896-34.218787 40.126266-25.636225L374.481381 89.169479A427.902035 427.902035 0 0 1 446.15135 84.376619 354.894525 354.894525 0 0 1 513.028458 89.169479L713.659785 3.789703c22.29237-8.582562 40.126265 0 40.126266 25.636225v170.202242a243.989986 243.989986 0 0 1 44.584739 140.999238h66.877109c17.833896 0 26.750844 8.582562 26.750844 21.400675a20.620442 20.620442 0 0 1-22.29237 21.289213h-71.335583c-4.458474 17.165125-4.458474 29.871775-8.916948 42.689888H825.121634a21.400675 21.400675 0 1 1 0 42.80135z" fill="#658FFF" p-id="994"></path></svg>
```

# 三、如何引用svg图标组件

## 1、创建我的项目（datav-libs-dev）

> 搜索心仪的图标，添加至data-libs-dev项目中

## 2、icomoon.io在线图标编辑器

> icomoon.io是一个在线的图标字体生成器与编辑器网站，它允许用户创建和编辑自己的图标字体库，以及将图标转换为可嵌入网页的字体格式。通过icomoon，用户可以选择和定制图标，然后生成一个字体文件，该文件可以直接在网页项目中使用。
该网站提供了大量的图标供用户选择，包括各种常见的形状、符号和图像。用户还可以上传自己的SVG图标进行编辑和转换。icomoon同时支持多种字体格式，如woff、woff2、ttf等，以适应不同的项目需求。
此外，icomoon还提供了图标搜索功能，方便用户快速找到所需的图标。用户还可以创建自己的图标集，以便在多个项目之间共享和使用。


* IcoMoon.io官网：[IcoMoon.io官网链接](https://icomoon.io/ "SVG Icon Libraries and Custom Icon Font Organizer      ❍ IcoMoon")

# 3、使用下载的svg图标

> 如何将下载的图标放入iconfont项目中，在iconfont中创建我的项目并打开，如果选择上传svg图标，点击右侧上传图标至项目，将svg下载的图标拖拽至框内上传即可


 ![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713437061997391007.png)




# 4、引用svg图标组件

* 点击symbol，生成在线链接

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713438465066597425.png)


* 打开需要使用的项目，如我使用的是vue3框架，则需要将链接放入全局里，public文件夹下的index.html里

```javascript
<script src="//at.alicdn.com/t/font_3377923_iv89pllgqsa.js"></script>
```

* 然后创建svg组件

```html
<template>
    <div class="icon-wrapper" :style="{ ...style }">
        <svg class="icon" >
            <use :href="iconName"></use>
        </svg>
    </div>
</template>

<script>
export default {
    name: 'Icon',
    props: {
        name: String,
        prefix: {
            type: String,
            default: 'icon-'
        },
        style: Object
    },
    setup(ctx) {
        const iconName = `#${ctx.prefix}${ctx.name}`
        return { iconName }
    }
}
</script>

<style lang="scss" scoped>
.icon {
    width: 100%;
    height: 100%;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
}
.icon-wrapper{
    display: inline-block;
}
</style>
```

* 父组件调用svg组件

```html
<icon prefix="icon-" class="set" name="dongmanxiao" ></icon>
```
![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713438554815615306.png "效果图展示")


# 四、引用icon图标组件(上)

## 1、下载至本地

> 首先将需要使用的图标放入我的项目，然后选择下载至本地

## 2、压缩包放入并解压

> 桌面创建iconfont文件夹，将压缩包放入并解压，此时iconfont.css文件正式我们需要的

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713438704841415120.png)


## 3、微信小程序引入iconfont

> 如果是在微信小程序中引入iconfont.css，此时后面会发生报错，原因是不支持直接只用@font-face，需要转成base64编码

## 4、base64编码

> base64编译转码免费网站：[transfonter.org官网链接](https://transfonter.org/ "transfonter.org")
>选择打开Base64 encode

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713438963857112272.png)


## 5、点击Add fonts上传

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713438864056375998.png)


## 6、上传的下载文件中的woff文件

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713439155021951600.png)


## 7、点击conver转变

## 8、点击download下载

## 9、解压找到stylesheet.css文件

## 10、打开最初下载iconfont文件iconfont.css

> 将stylesheet.css里的@font-face替换掉iconfont.css里的@font-face
这样iconfont就转码base64编译成功啦

# 五、引用icon图标组件(下)

## 1、打开微信开发者工具 

> 创建一个微信小程序项目，在项目文件夹下创建 iconfont/index.wxss

## 2、index.wxss输入下载的iconfont.css内容

```css
@font-face {
    font-family: 'iconfont';
    src: url('data:font/woff2;charset=utf-8;base64,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') format('woff2'),
        url('data:font/woff;charset=utf-8;base64,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') format('woff');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}


.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-zhizhen:before {
  content: "\e60a";
}

.icon-logo1:before {
  content: "\e621";
}

.icon-logo:before {
  content: "\e60c";
}

.icon-dongmanxiao:before {
  content: "\e6ac";
}

.icon-shezhi1:before {
  content: "\e601";
}
```

## 3、在components组件文件夹下创建icon组件

* icon.js

```javascript
// components/icon/icon.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
    name:String,
    color:{
      type:String,
      value:"#6faf92"
    },
    size:{
      type:String,
      value:"34"
    }
  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```

* icon.wxml

```html
<view class="iconfont icon-{{name}}" style="font-size: {{size}}rpx; color:{{color}};"></view>
```

* icon.wxss

```css
@import "../../iconfont/index.wxss"
```

## 4、在app.json中创建全局使用

```json
"usingComponents":{
"i-icon": "/components/icon/icon"
}
```

## 5、引用icon组件

> 可用过size控制icon大小，color控制颜色，name控制图标

```html
<i-icon name="dongmanxiao" size="20" color="#fff" />
```

![](https://bbs-img.huaweicloud.com/blogs/img/20240418/1713439248662396597.png)
