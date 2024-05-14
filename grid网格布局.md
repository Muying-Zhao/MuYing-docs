# 前言

> 今天给大家带来的干货内容是如何使用grid网格布局，小编实打实的总结宝贵经验，希望能祝大家学有所成，为将来职业发展多填一项技能。有什么问题反馈可以在评论区下方留言，希望大家可以点赞支持，你的每一个点赞都是我继续创作下去的动力。

# 一、为什么使用网格布局

* 独特的单元格"行"和"列"布局

> 是W3C提出的一个二维布局系统。它与其他布局方式有所不同，因为它不仅可以指定容器内部多个项目的位置，还能创建更加复杂和灵活的页面布局。Grid布局可以将容器划分为“行”和“列”，形成单元格，并允许开发者指定项目所在的单元格。

* 强大的功能性和灵活性

> Grid布局可以适应不同屏幕大小和设备类型，实现响应式设计，确保页面在各种设备上都能呈现出满意的效果。同时还提供了自定义对齐方式的功能，包括水平对齐、垂直对齐和对角线对齐，使页面布局更加精确和美观。

* 大厂公司的青睐

> 如阿里巴巴、腾讯、华为、字节等公司大厂通常会结合自身的业务需求和设计理念，利用Grid布局打造出独特且吸引人的页面效果。

# 二、grid容器

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713548363467332279.png)

## 1、grid-template-rows

## 2、grid-template-columns
> grid-template-columns,grid-template-rows 是 CSS Grid 布局的一个属性，用于定义网格容器中的列的结构。该属性允许你指定网格中的列数、列宽,列高以及列与列之间的间隙。

* 固定尺寸:可以使用像素值（px）、百分比（%）、em、rem 等单位来指定列的固定宽度。

```html
<style>
        .main{
            width:300px;
            height:300px;
            background:skyblue;
            display: grid;
            grid-template-columns: 50px 50px 50px;
            grid-template-rows: 50px 50px 50px;
        }
        .main div{
            background:pink;
        }
</style>

<div class="main">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
</div>
```

* 灵活尺寸:使用 fr 单位可以创建灵活的列宽,fr表示可用空间的一等份

```css
.main {
  display: grid;
  grid-template-columns: 150px 1fr 1fr;
  grid-template-rows: 0.3fr 0.3fr;
}
```

* 自动尺寸：使用auto自动调节宽度

```css
.main {
  display: grid;
  grid-template-columns: 50px 20% auto;
  grid-template-rows: 50px 50px;
}
```

*  repeat() 函数：允许重复地定义列的模式

```css
.main {
  display: grid;
  grid-template-columns: repeat(3, 100px);
}
```

* 自动填充:使用 auto-fill 关键字，浏览器会尽量在容器中填充尽可能多的列，同时保持每列的最小宽度。

* 最小和最大内容大小:使用 min-content 和 max-content 关键字，可以根据内容的最小和最大大小来设置列的大小。

> grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); 的作用是根据容器的宽度自动填充尽可能多的列，同时保证每列的最小宽度为 100px。如果容器宽度足够大，浏览器会填充尽可能多的列，并且每列会尽量等宽（1fr 表示可用空间的一等份）。如果容器变窄，列数会减少，但每列的最小宽度仍然保持为 100px。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>响应式的网格布局</title>
    <style>
        .container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            /* 添加网格间隙 */
            grid-gap: 10px;
            background-color: skyblue;
        }

        .item {
            background-color: pink;
            padding: 20px;
            /* 使padding 不会增加元素的总宽度 */
            box-sizing: border-box;
            text-align: center;
        }
    </style>
</head>

<body>

    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
        <div class="item">7</div>
        <div class="item">8</div>
        <div class="item">9</div>
        <div class="item">10</div>
        <div class="item">11</div>
        <div class="item">12</div>
    </div>

</body>

</html>
```

## 3、grid-template-areas

> grid-template-areas 是 CSS Grid 布局模块中的一个属性，它允许通过定义一个可视化的网格模板来指定网格容器中的行和列的区域（areas）。通过使用字符串来命名网格中的不同区域，你可以更直观地创建复杂的网格布局。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713579553605535936.png)

* 实现该效果图1

```html
<style>
        .main{
            width:300px;
            height:300px;
            background:skyblue;
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr 1fr;
            grid-template-areas:
            "a1 a1 a2"
            "a1 a1 a2"
            "a3 a3 a3";
        }
        .main div{
            background:pink;
            border:1px black solid;
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .main div:nth-of-type(1){
            grid-area: a1;
        }
        .main div:nth-of-type(2){
            grid-area: a2;
        }
        .main div:nth-of-type(3){
            grid-area: a3;
        }
</style>
<div class="main">
        <div>1</div>
        <div>2</div>
        <div>3</div>
</div>
```

* 实现该效果图2

```html
<style>
        .main{
            width:300px;
            height:300px;
            background:skyblue;
            display: grid;
            grid-template:
            "a1 a1 a2" 1fr
            "a1 a1 a2" 1fr
            "a3 a3 a3" 1fr
            / 1fr 1fr 1fr;
        }
        .main div{
            background:pink;
            border:1px black solid;
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .main div:nth-of-type(1){
            grid-area: a1;
        }
        .main div:nth-of-type(2){
            grid-area: a2;
        }
        .main div:nth-of-type(3){
            grid-area: a3;
        }
</style>
<div class="main">
        <div>1</div>
        <div>2</div>
        <div>3</div>
</div>
```

## 4、gap

> gap属性用于设置网格或弹性容器内的行与行之间（row-gap）以及列与列之间（column-gap）的间隙。在较新的CSS规范中，gap属性是一个简写属性，用于同时设置row-gap和column-gap。

```css
/* row-gap: 20px;
column-gap: 30px; */
gap: 20px 30px;
```

## 5、grid-gap

> 使用新的gap属性来同时设置行间隙和列间隙,与gap属性相同，但是这两个属性在较新的CSS规范中已经被row-gap和column-gap替代，为了保持代码的现代性和兼容性，通常推荐使用row-gap、column-gap和gap属性。

```css
/* 使用新的gap属性来同时设置行间隙和列间隙 */
grid-gap: 20px 30px;
/* 或者分别设置行间隙和列间隙
/*
row-gap: 20px;
column-gap: 30px;
*/
```

## 6、place-items

> place-items是align-items和justify-items的简写属性，用于同时设置网格或弹性容器内项目的对齐方式。

## 7、place-content

> place-content是align-content和justify-content的简写属性，用于同时设置网格或弹性容器内内容的对齐方式。

## 8、grid-auto-flow

> 是 CSS Grid 布局中的一个属性，用于控制网格中自动放置项的流动方向以及是否应该尝试填充网格中的空白区域（即密集填充）。

* row：自动放置的项会按照行方向进行流动。这是默认值。

* column：自动放置的项会按照列方向进行流动。

```css
/* grid-template-columns: 100px 100px 100px;
grid-template-rows: 100px; */
/* 默认：row 就是行产生隐式网格 */
/* grid-auto-flow: row; */
/* 可以调节产生隐式网格的高度 */
/* grid-auto-rows: 100px; */
grid-template-columns: 100px;
grid-template-rows: 100px 100px 100px;
/* column 就是列产生隐式网格 */
grid-auto-flow: column;
/* 可以调节产生隐式网格的宽度 */
grid-auto-columns: 100px;
```

* 尝试填充网格中的空白区域，而不是按照行或列的顺序进行填充。

```css
grid-auto-flow: row dense;  /* dense 紧密的 */
grid-auto-rows: 100px;
```

## 9、grid-template-columns,grid-template-rows

> 属性用于定义网格的列和行的结构。这些属性允许你指定网格线的位置以及网格单元格的大小。[col1], [col2], [col3], 和 [col4] 是命名的网格线，它们可以用来更精确地定位网格项，下列示例相当于画了一个九宫格。

```css
grid-template-columns:[col1] 1fr [col2] 1fr [col3] 1fr [col4];
grid-template-rows:[row1] 1fr [row2] 1fr [row3] 1fr [row4];

/* 网格容器将有三列*/
/* grid-template-columns: 100px 100px 100px; */
```

# 三、grid容器子项

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713593533286579645.png)

## 1、grid-column-start

* 如果父项使用了 grid-auto-flow: row dense;grid-column-start：2，使子项第一个排在第二的起始位置，然后依次往右开始排列
```css
.main2 div:nth-of-type(1){
       grid-column-start: 2;
}
```
* 若父项没有使用dense，则可以控制起始位置。开始顺序排列
```css
.main div:nth-of-type(1){
       background:pink;
       grid-column-start: 2;
       grid-column-end: 3;
       /* 默认值：auto */
       /* grid-row-start: 1;
       grid-row-end: 2; */
}
```

## 2、grid-area

> 用于指定一个网格元素在网格容器中的位置以及跨越的行数和列数。grid-row-start, grid-column-start, grid-row-end 以及grid-column-end属性的缩写，以及额外支持grid-template-areas设置的网格名称

```css
/*grid-area: 2/1/span 2/span 2;*/

/* grid-column: 2 / 3;
grid-row: 2 / 4; */
grid-area: 2 / 2 / 3 / 3;
```

## 3、place-self

> 跟place-item用法相同，只不过是操作指定的子项.用于同时设置align-items和justify-items的值。这两个属性分别控制元素在块级（垂直）方向和内联（水平）方向的对齐方式。

# 四、grid实战运用

## 1、响应式布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .main{
            background:skyblue;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            grid-template-rows: 100px;
            grid-auto-rows: 100px;
            grid-gap:20px 20px;
        }
        .main div{
            background:pink;
            border:1px #666 solid;
        }
    </style>
</head>
<body>
    <div class="main">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
        <div>7</div>
        <div>8</div>
        <div>9</div>
    </div>
</body>
</html>
```

![未标题-2.gif](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713598877503476035.gif)

## 2、自定义网格大小和顺序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .main{
            width:300px;
            height:300px;
            background:skyblue;
            display: grid;
            grid-template-columns: repeat(3,1fr);
            grid-template-rows: repeat(3,1fr);
            gap:5px;

        }
        .main div{
            background:pink;
        }
        .main div:nth-of-type(5){
            grid-area: 2/1/span 2/span 2;
        }
        .main div:nth-of-type(2){
            grid-area: 1/1/span 1/span 1;
        }
    </style>
</head>
<body>
    <div class="main">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
    </div>
</body>
</html>
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713597014725539241.png)


## 3、layout布局

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            margin: 0;
        }

        .main {
            background: skyblue;
            display: grid;
            grid-template-columns: 100px 1fr 100px;
            grid-template-rows: 100px 1fr 100px;
            gap: 10px;
            height: 100vh;
        }

        .main div {
            background: pink;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .main div:first-of-type {
            grid-area: 1/1/span 1/span 3;
        }

        .main div:last-of-type {
            grid-area: 3/1/span 1/span 3;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="">header</div>
        <div class="">left</div>
        <div class="">center</div>
        <div class="">right</div>
        <div class="">footer</div>
    </div>
</body>

</html>
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713597154754974156.png)

## 4、12列的网格系统

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .row{
            background:skyblue;
            display: grid;
            grid-template-columns: repeat(12, 1fr);
            grid-template-rows: 50px;
            grid-auto-rows: 50px;
        }
        .row div{
            background:pink;
            border:1px black solid;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .row .col-1{
            grid-area: auto/auto/auto/span 1;
        }
        .row .col-2{
            grid-area: auto/auto/auto/span 2;
        }
        .row .col-3{
            grid-area: auto/auto/auto/span 3;
        }
        .row .col-4{
            grid-area: auto/auto/auto/span 4;
        }
        .row .col-5{
            grid-area: auto/auto/auto/span 5;
        }
        .row .col-6{
            grid-area: auto/auto/auto/span 6;
        }
        .row .col-7{
            grid-area: auto/auto/auto/span 7;
        }
        .row .col-8{
            grid-area: auto/auto/auto/span 8;
        }
        .row .col-9{
            grid-area: auto/auto/auto/span 9;
        }
        .row .col-10{
            grid-area: auto/auto/auto/span 10;
        }
        .row .col-11{
            grid-area: auto/auto/auto/span 11;
        }
        .row .col-12{
            grid-area: auto/auto/auto/span 12;
        }
    </style>
</head>
<body>
    <div class="row">
        <div class="col-5">5</div>
        <div class="col-7">7</div>
        <div class="col-4">4</div>
        <div class="col-8">8</div>
        <div class="col-3">3</div>
        <div class="col-9">9</div>
        <div class="col-2">2</div>
        <div class="col-10">10</div>
        <div class="col-1">1</div>
        <div class="col-11">11</div>
        <div class="col-12">12</div>
        <div class="col-6">6</div>
        <div class="col-6">6</div>
    </div>
</body>
</html>
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240420/1713601872487324904.png)
