# 前言

>结合上一章VMMV设计模式，今天的目的事全面了解Vue3组件的相关概念，掌握组件之间的通信以及如何封装一个可复用的组件
了解什么是单文件组件SFC，以及脚手架的使用和底层实现机制，对工程化有一定的认知

# 一、什么是组件

## 1、组件概念

> 组件是独立于其他组件,可以在不同的项目或应用中复用。从而可以减少开发成本和提升效率，同时也方便维护和管理。

## 2、组件的基本使用方式

### 1)、组件的命名方式及规范

> 在Vue.js中，组件通常使用`XxxYyy`大驼峰风格命名，或者使用小写字母加短横线`xxx-yyy`风格命名，调用组件时`<xxx-yyy>`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./vue.global.js"></script>
</head>
<body>
    <div id="app">
        <test-demo></test-demo>
    </div>
    <script>
        let app=Vue.createApp({
            data(){
                return {}
            }
        })
        app.component('TestDemo',{
            template:`
            <div>hello world</div>
            <div>{{message}}</div>
            `,
            data() {
                return {
                    message: 'world hello'
                }
            }
        })
        let vm=app.mount(`#app`);
    </script>
</body>
</html>
```

### 2)、根组件与普通组件

> 根组件中的template使用形式，普通组件的优先级要比根组件的低

```html
<body>
    <div id="app">
        <div>我是根组件一</div>
        <test-demo></test-demo>
    </div>
    <script>
        let RootApp = {
            data() {
                return {}
            },
            template: `
               <div>我是根组件二</div>
            `,
        }
        let TestDemo={
            template: `
            <div>我是组件三</div>
            `,
        }
        let app=Vue.createApp(RootApp)
        app.component('TestDemo',TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

### 3)、全局组件与局部组件

> 全局组件指的事可以在整个Vue应用的任何位置使通过`Vue.component('tagName', options)`进行定义，而局部组件只能在定义它们的父组件中使用，通过Vue实例的components选项进行定义
注意：Vue中在使用局部组件时，组件

* 使用全局组件

```html
<body>
    <div id="app">
        <test-global></test-global>
        <test-part></test-part>
    </div>
    <script>
        let TestPart = {
            template: `
            <div>hello world</div>
            `,
        }
        let TestGlobal = {
            template: `
            <div>world hello</div>
            `
        }
        let RootApp = {
            data() {
                return {}
            },
        }
        let app = Vue.createApp(RootApp)
        // 全局组件
        app.component('TestGlobal', TestGlobal)
        app.component('TestPart', TestPart)
        let vm = app.mount(`#app`)
    </script>
</body>
```

* 使用局部组件

> 使用局部组件时，在谁调用该组件下才能渲染加载出来，同时需要先在局部被引用之前完成。

```html
<body>
    <div id="app">
        <test-global></test-global>
        <!-- <test-part></test-part> -->
    </div>
    <script>
        let TestPart = {
            template: `
            <div>hello world</div>
            `,
        }
        let TestGlobal = {
            template: `
            <div>world hello</div>
            <test-part></test-part>
            `,
            // 局部组件
            components: {
                TestPart
            }
        }
        let RootApp = {
            data() {
                return {}
            },
            // 局部组件
            components: {
                TestGlobal
            }
        }
        let app = Vue.createApp(RootApp)
        // 全局组件
        // app.component('TestGlobal', TestGlobal)
        // app.component('TestPart', TestPart)
        let vm = app.mount(`#app`)
    </script>
</body>
```

### 4)、插槽（slot）

* 普通插槽`slot`使用

> 在子组件中使用`<slot></slot>`插槽时，父组件可以将其内容渲染到子组件的默认插槽中,
父组件中`<template>world hello</template>`就是在子组件中插入的内容

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./vue.global.js"></script>
</head>

<body>
    <div id="app">
        <test-demo>
            <template>world hello</template>
        </test-demo>
    </div>
    <script>
        let TestDemo = {
            template: `
            <div>
              <h1>hello world</h1>
              <slot></slot>
            </div>
            `,
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>

</html>
```

* 具名插槽

> 如果在子组件中使用多个插槽，是需要给插槽加上具名，父组件通过使用 `<template>` 和 `v-slot` 指令来定义具名插槽精准的插入到应有的位置。如以下修改的body内容，其中`v-slot:`可以该写成`#`

```html
<body>
    <div id="app">
        <test-demo>
            <template  v-slot:title>
                <div>world hello</div>
            </template>
          <template #item>
            hi vue
          </template>
        </test-demo>
    </div>
    <script>
        let TestDemo = {
            template: `
            <div>
              <slot name="item"></slot>
              <h1>hello world</h1>
              <slot name="title"></slot>
            </div>
            `,
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

* 作用域插槽

> 允许父组件访问子组件内部的数据。子组件通过插槽传递数据给父组件，父组件则可以在其模板中定义如何渲染这些数据。作用域插槽在创建可重用和灵活的组件时非常有用。如下列代码中子组件通过插槽传递`list`数据给父组件，父组件将其渲染到插槽中进行for循环渲染数据

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./vue.global.js"></script>
</head>

<body>
    <div id="app">
        <test-demo>
            <template #list="{list}">
                <ul>
                    <li v-for="(item,index) in list" :key="index">{{item}}</li>
                </ul>
            </template>
        </test-demo>
    </div>
    <script>
        let TestDemo = {
            data(){
                return {
                    list :['苹果','香蕉','橘子']
                }
            },
            template: `
            <div>
                <slot name="list" :list="list"></slot>
            </div>
            `,
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>

</html>
```

# 二、组件之间通信联系

> 组件之间通信主要是为了让组件满足不同的需求，可以是数据的共享与同步和组件封装与复用。

## 1、组件之间是如何进行互相通信的

> 父组件通过`props`进行传参，子组件通过自定义`events`

## 2、父组件通过props进行传参

> 当使用子组件标签时，可以在标签上添加属性，然后通过这些属性就是传递给子组件的props。子组件通过声明props选项来接收这些传递进来的数据。

```html
<body>
    <div id="app">
        <test-demo title="hello world" :count="count"></test-demo>
    </div>
    <script>
        let TestDemo = {
            template: `
            <div>{{title}}</div>
            <div>{{count}}</div>
            `,
            props:['title','count']
        }
        let RootApp = {
            data() {
                return {
                    count:1
                }
            },
            mounted(){
                setTimeout(()=>{
                    this.count=2
                },1000)
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

## 3、给prop设置默认值

> 当在Vue组件中定义props时，可以为每个prop设置默认值,如果父组件没有为子组件提供某个prop的值，那么子组件将使用默认值，从而确保组件的健壮性和可维护性。具体如下，当没有:count="count"进行传值时，会显示默认值为0.

```html
<body>
    <div id="app">
        <test-demo></test-demo>
    </div>
    <script>
        let TestDemo = {
            template: `
            <div>{{count}}</div>
            `,
            props:{
                count:{
                    type:Number,
                    default:0
                }
            }
        }
        let RootApp = {
            data() {
                return {
                    count:1
                }
            },
            mounted(){
                setTimeout(()=>{
                    this.count=2
                },1000)
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```


## 4、子组件通过自定义emit进行通信

> 子组件内部可以通过this.$emit('eventName', payload)来触发一个事件，并传递一个可选的负载。父组件然后在子组件的标签上监听这个事件，并定义相应的事件处理函数。

```html
<body>
    <div id="app">
        <test-demo @tab-switch="handleSwitch"></test-demo>
    </div>
    <script>
        let TestDemo = {
            emits:['tab-switch'],
            template: `
            <div>{{message}}</div>
            `,
            data() {
                return {
                    message:'hello world'
                }
            },
            mounted(){
                this.message= 'world hello'
                setTimeout(()=>{
                    this.$emit('tab-switch', this.message)
                })
            }
        }
        let RootApp = {
            data() {
                return {}
            },
            methods:{
                handleSwitch(event){
                    console.log(event,'event')
                }
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

## 5、不分组件的双向流动

> 组件之间的数据是单向流动的，子组件不能直接修改传递过来的值,(如父组件传递了一个count属性，子组件是不能直接修改count，需要通过自定义emit传值回去在父组件上进行修改），但是有时候表单元素组件也需要数据的双向流动，可利用v-model来实现。如下：

```html
<body>
    <div id="app">
        <test-demo v-model="count"></test-demo>
        {{count}}
    </div>
    <script>
        let TestDemo = {
            props: {
                // 通过 v-model 传递给组件的值
                'modelValue':{
                    type:Number
                }
            },
            //组件内部的某个值发生变化时，它会自动发出这个事件来更新父组件中的值。
            emits:['update:modelValue'],
            // 当 input 的值发生变化时，触发一个 update:modelValue 事件，并将新的值（转换为数字）传递给父组件。
            template:`
            <input type="text" :value="modelValue" 
            @input=" $emit('update:modelValue', Number($event.target.value)) "
            >
            `
        }
        let RootApp = {
            data() {
                return {
                    count: 0
                }
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

## 6、当父传子参没有props时

> 默认不通过props接收的话，属性会直接挂载到组件容器上。当父组件向子组件传入参数时，子组件没有用props接受，会默认当做普通属性。相当于原封不动的传递到子组件上。同时事件也是如此，会直接挂载到组件容器上

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./vue.global.js"></script>
    <style>
        .box {
            background: aqua;
        }
    </style>
</head>

<body>
    <div id="app">
        <test-demo title="world hello" class="box"></test-demo>
    </div>
    <script>
        let TestDemo = {
            // props:['title'], 
            template:`
            <div>123</div>
            `
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>

</html>
```

## 7、通过inheritAttrs选项阻止这种行为

> 在组件中加入`inheritAttrs:false`可以阻止props，事件没有时默认传递行为

```html
<body>
    <div id="app">
        <test-demo title="world hello" class="box"></test-demo>
    </div>
    <script>
        let TestDemo = {
            // props:['title'], 
            template:`
            <div>123</div>
            `,
            inheritAttrs:false
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

## 8、$attrs 内置语法

> 可通过$attrs 内置语法，给指定元素传递属性和事件。可以在子组件中通过打印`this.$attrs`获取传递下来的信息

```
<body>
    <div id="app">
        <test-demo title="world hello" class="box"></test-demo>
    </div>
    <script>
        let TestDemo = {
            // props:['title'],
            template:`
            <div>
              <h1 v-bind="$attrs" >hello world</h1>
              <h2 >world hello</h2>
            </div>
            `,
            inheritAttrs:false
        }
        let RootApp = {
            data() {
                return {}
            }
        }
        let app = Vue.createApp(RootApp)
        app.component('TestDemo', TestDemo)
        let vm = app.mount(`#app`)
    </script>
</body>
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240423/1713879584585910948.png)
