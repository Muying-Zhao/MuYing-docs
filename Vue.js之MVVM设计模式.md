# 前言

>看到招聘信息网站上有对MVVM框架经验的需求，刚好曾有过这方面的笔记，在复习的同时总结核心知识点分析给大家。
MVVM是可以实现View和Model的完全分离，通过ViewModel这个桥梁进行交互，然后ViewModel通过双向数据绑定把View层和Model层连接起来，而View层和Model层之间的通信则完全由ViewModel负责。

# 一、MVC设计模式与MVVM设计模式，vue.js

## 1、什么是MVC设计模式

> MVC是一种最早出现的软件架构模式，它将应用程序的输入、处理和输出明确地划分为三个部分，使得业务逻辑、数据和界面显示可以独立地进行开发、测试和维护。其中MVC指的是Model（模型）、View（视图）、Controller（控制器）

## 2、为什么还需要MVVM设计模式

> 在MVC中，Controller负责处理用户输入，并更新Model和View。View被动地接受Controller传来的数据，并显示给用户。而在MVVM中，ViewModel是连接Model和View的桥梁，它实现了View和Model的双向绑定,当Model中的数据变化时，View会自动更新；同样，View中的数据变化也会反映到Model中。所以它比MVC更容易实现复杂的用户界面和数据交互。

# 二、使用vue框架

> vue框架的好处可以不用自己完成复杂的DOM操作了，而由框架帮我们去完成，同时可以使用组件化开发,子组件与父组件之间可以通过触发事件进行通信，从而改变数据，可以使我们很好的复用代码。

## 1、使用html，js，css在线运行代码

> 使用链接在线访问[vue.global.js](https://unpkg.com/browse/vue@3.2.36/dist/)或者安装下载vue.global.js在本地项目中
如果想要切换版本

* 链接：[切换版本](https://unpkg.com/browse/vue@3.2.36/dist/)

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
        {{message}}
        <input type="text" v-model="message">
    </div>
    <script>
        let app=Vue.createApp({
            data(){
                return{
                    message: 'hello world'
                }
            }
        })

        let vm=app.mount('#app')
    </script>
</body>
</html>
```

## 2、选项式API
```javascript
let vm=createApp({
       //选项
       methods:{},
       computed:{},
       watch:{},
       data(){},
       mounted(){},
}).mount('#app');
```

## 3、声明式渲染

> 声明式渲染是Vue.js等前端框架的核心概念之一。它意味着开发者只需描述数据应该如何渲染，而不需要手动操作DOM来更新视图。其中messgae可以是js表达式，如字符串，数组，方法

```html
<body>
    <div id="app">
        {{message}}
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            }
        }).mount('#app')

        setTimeout(()=>{
            vm.message='world hello'
        },1000)
    </script>
</body>
```

* proxy内置对象

> vm.message利用的原理是Es6的Proxy对象对底层进行监控
Proxy 用于创建一个对象的代理，从而实现对目标对象某些操作的拦截和自定义。比如：
拦截和自定义操作：使用 Proxy 来拦截对目标对象的各种操作，并在这些操作发生时执行自定义代码。
验证和转换：利用 Proxy 来确保目标对象的属性总是符合某些条件，或者在读取或设置属性时执行某些转换。
日志和调试：通过 Proxy，记录对目标对象的所有操作

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
        {{message}}
    </div>
    <script>
        let data={
            message:'hello world'
        };
        data=new Proxy(data,{
            // 发生改变时触发set
            set(target,key,value){
                console.log(target, key, value,'set')
                app.innerHTML = value
            },
             // 默认执行get
            get(target){
                console.log(target.message,'get')
                return target.message
            }
        })
        app.innerHTML= data.message

        setTimeout(() => {
            data.message = 'world hello'
        }, 1000)
    </script>
</body>

</html>
```

## 4、指令系统与事件方法及传参处理

* vue指令
> 常用指令的有`v-if`,`v-else`,`v-else-is`,`v-for`,`v-show`,`v-on`,`v-bind`,`v-model`,`v-html`等,
`v-bind:title`相当于`:title`,`v-on:click`相当于`@click`

* 事件方法

> methods选项向组件实例添加方法，Vue自动为methods 绑定this，以便于它始终指向组件实例

* 传参处理

> Vue帮我们处理好了如何进行事件传参处理，提供了内部的$event语法来获取event对象

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
        <div :title="message" :class="className" @click="handleClick($event,123)">
            {{message}}
        </div>
    </div>
    <script>
        let vm=Vue.createApp({
            data(){
                return {
                    message: '点击一下有惊喜',
                    className: 'box'
                }
            },
            methods: {
                handleClick(event,num){
                    this.message='hello world';
                    console.log(event,num,'event,num')
                }
            }
        }).mount('#app')
    </script>
</body>
</html>
```

## 5、计算属性

> `split(' ').reverse().join(' ')`使用空格将字符串变成数组，然后数组顺序颠倒，最后转回字符串类型如。

* 计算属性`computed`

> 调用方法时，若写在methods里需要再方法后加上(),如果是写在computed里，直接写上方法名即可调用

```
//methods
handleClick()
//computed
handleClick
```

* 计算属性与方法相比

> 具备缓存的能力，而方法不具备缓存，在依赖的数据未改变时，即使调用了两次相同方法，计算属性只执行一次，而在methods里会执行两次。且计算属性只读取不能修改，要想修改，得修改最终依赖的东西。

* 计算属性原理

> 计算属性虽然是对象方法，但是以属性的方式进行使用，并且默认是只读的

## 6、侦听器watch

* 原理

> 侦听器是在监听的属性发生改变的时候才会触发，初始时是不会触发的

```
wathc:{
   message(newVal,oldVal){
     console.log(newVal,oldVal);
    }
}
```
* 侦听器与计算属性区别

>计算属性适合:多个值去影响一个值的应用;而侦听器适合:一个值去影响多个值的应用,
同时侦听器支持异步的程序，而计算属性不支持异步的程序。
如计算一个加法，由两个变量得出加法的和，这是就需要用到计算属性，而侦听器一般是在异步加载请求后，对后端返回的api进行处理。

## 7、条件渲染v-if

> 条件渲染:使用v-if指令条件性地渲染一块内容。这块内容只会在指令的表达式返回真值的时候被渲染。
同时被认为是假值fasle的有（false,0,空字符串，null,undefined和NaN ),
注意：空数组（`[]`）在布尔上下文中会被转换为true，因为数组对象总是真值，但是可以使用arr.length来判断

## 8、 列表渲染v-for

* v-for用法

> v-for指令基于一个数组来渲染一个列表。v-for指令需要使用item in items形式的特殊语法，其中items是源数据数组，而item 则是被迭代的数组元素的别名。

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
        <!-- key是用来跟踪每一个遍历项的身份 -->
        <div v-for="item,index in list" :key="index">
            {{item}}
            {{index}}
        </div>
        <div v-for="value,key,index in info">
            {{value}}
            {{key}}
            {{index}}
        </div>
        <div v-for="item in test">
            {{item}}
        </div>
        <!-- num类型和字符串也可以遍历，数字从1开始到num，字符串逐步返回单个字符类型 -->
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    list:['a','b','c'],
                    info:{username:'小明',age:20},
                    test:'hello world'
                }
            }
        }).mount('#app')
    </script>
</body>

</html>
```

* 注意v-for不建议与v-if同时使用，可以使用计算属性来实现

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
        <div v-for="item,index in updateList" :key="index">
            {{item.str}}
            {{index}}
        </div>
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    list:[
                        {id:1, str:'a'},
                        {id:2, str:'b'},
                        {id:3, str:'c'},
                    ],
                }
            },
            computed:{
                updateList(){
                    return this.list.filter((v)=>v.id%2 ===1)
                }
            }
        }).mount('#app')
    </script>
</body>

</html>
```

* 在vue中，通常v-for，v-if通常是在template上使用，

## 9、calss和style的三种使用方式

### 1)、class的三种使用方式

* 对象语法

>通过绑定一个对象到`:class`，你可以动态地切换class。

```html
<div :class="isActive ? 'active-class' : 'inactive-class'"></div>
<script>
    export default {
        data() {
            return {
                isActive: true
            };
        }
    };
</script>
```

* 数组语法

> 将一个数组绑定到:class，以应用一个类列表。数组中的每个元素都会是一个类名。
在这个class绑定的例子中，isActive为true时，active类会被添加，而base-class则始终被添加
这个三元表达式只会判断`isActive ? 'active' : ''`,

```html
<template>
  <div :class="[isActive ? 'active' : '', 'base-class']"></div>
</template>
<script>
export default {
  data() {
    return {
      isActive: true
    };
  }
};
</script>
```

* 在组件上使用

> 当在Vue组件上使用class时，可以直接在组件标签上添加静态的class，同时使用:class来动态绑定。

```html
<template>
  <my-component :class="isActive ? 'active-class' : 'inactive-class'"></my-component>
</template>
```

### 2)、style的三种使用方式

> 与calss用法相似

* 对象语法

```html
<template>
  <div :style="{ color: isActive ? 'aqua' : 'pink' }">Hello world</div>
</template>
<script>
export default {
  data() {
    return {
      isActive: true
    };
  }
};
</script>
```

* 结合数组和对象语法

```html
<template>
  <div :class="[isActive ? 'active' : '', 'base-class']"></div>
</template>
<script>
export default {
  data() {
    return {
      isActive: true
    };
  }
};
</script>
```

* 在组件上使用

```html
<template>
  <my-component :style="{ color: isActive ? 'aqua' : 'pink' }"></my-component>
</template>
```

### 3)、数组语法的demo

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
        .box1{
            background: aqua;
        }
        .box2{
            color: rebeccapurple;
            background: orange;
        }
    </style>
</head>

<body>
    <div id="app">
        <div :class="myClass">123</div>
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    myClass:['box1','box2']
                }
            }
        }).mount('#app')

        setTimeout(()=>{
            //删除数组的最后一项
            vm.myClass.pop();
        },2000)
    </script>
</body>

</html>
```

## 10、表单处理与双向绑定

* 实现双向绑定一般做法

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
        <input type="text" :value="message" @input="message=$event.target.value">
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            }
        }).mount('#app')

        setTimeout(() => {
            vm.message='world hello'
        }, 2000)
    </script>
</body>

</html>
```

* v-model也能实现双向绑定

> 在Vue中是通过v-model指令来操作表单的，可以非常灵活的实现响应式数据的处理
v-model本质上不过是语法糖。可通过value属性+input或change事件来实现同样的效果

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
        <input type="text" v-model="message">
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            }
        }).mount('#app')

        setTimeout(() => {
            vm.message='world hello'
        }, 2000)
    </script>
</body>

</html>
```

* checkbox,radio表单类型

> 表单为checkbox,多选框，radio单选框时，v-modal绑定相同属性

* select下拉选项

```html
<body>
    <div id="app">
        <select v-model="city">
            <option value="成都">成都</option>
            <option value="重庆">重庆</option>
            <option value="北京">北京</option>
        </select>
        {{city}}
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    city:'北京'
                }
            }
        }).mount('#app')
    </script>
</body>
```

## 11、生命周期构子函数及原理分析

> 生命周期钩子
> >初始化实例触发顺序钩子
`beforeCreate`此时还不能访问到数据和 DOM，响应式数据页拿不到
`created`在实例创建完成后被立即调用。响应式数据页可以拿到但是此时还是不能访问到数据和 DOM，
`beforeMount`在挂载开始之前被调用：相关的 render 函数首次被调用。此时，虚拟 DOM 已经创建完成，但还没有挂载到真实的 DOM 上
`mounted`此时都能拿到响应式数据和DOM

> >更新数据时触发
`beforeUpdate` 在更新数据的时候会触发的生命周期,不能拿到更新后的内容
`updated`在更新数据后 会触发的生命周期，能拿到更新后的内容

> >不常用
`activated`keep-alive 组件激活时调用。
`deactivated`keep-alive 组件停用时调用。

> >卸载实例时触发
`beforeUnmount`组件实例卸载之前调用。在这一步，实例仍然完全可用。
`unmounted`组件实例卸载后调用,所有的事件监听器都会被移除，所有的子实例也会被销毁。

> >不常用
`errorCaptured`当捕获一个来自子孙组件的错误时被调用。
此钩子会接收三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上冒泡。
`renderTracked`在响应式依赖被追踪时调用。
`renderTriggered`在响应式依赖发生改变并导致组件重新渲染时被调用。

## 12、扩展JavaScript里的fetch函数使用

> 用于在浏览器中进行网络请求，它提供了一种简单、合理的方式来跨网络异步获取资源。fetch 返回的是一个 Promise 对象，通常用.then() 或async/await 来处理它的响应，默认不发送或接收 cookies。
fetch()发送请求里可以请求的 URL、请求方法和请求JSON数据

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
        <ul>
            <li v-for="item in filterList" :key="item.id">
                {{item.name}}
            </li>
        </ul>
    </div>
    <script>
        let vm = Vue.createApp({
            data() {
                return {
                    list: []
                }
            },
            created() {
                fetch('./test.json')
                     // 解析响应体为 JSON
                    .then((res) => res.json())
                    .then((res) => {
                        this.list = res
                    })
            },
            computed:{
                filterList(){
                    return this.list
                }
            }
        }).mount('#app')
    </script>
</body>

</html>
```

```json
[
  {
    "id": 1,
    "name": "小明",
    "gender": "女",
    "age": 20
  },
  {
    "id": 2,
    "name": "小强",
    "gender": "男",
    "age": 18
  },
  {
    "id": 3,
    "name": "大白",
    "gender": "女",
    "age": 25
  },
  {
    "id": 4,
    "name": "大红",
    "gender": "男",
    "age": 22
  }
]
```

### 13、javascript中解构对象使用

```javascript
<script>
     const obj = {
      name: 'yibo',
      age: 25,
      gender: 'female'
    };
    const { gender, ...newObj } = obj;
    console.log(obj,'obj'); // [ 'name', 'age', 'gender' ]
    console.log(newObj,'newObj'); // [ 'name', 'age']
    console.log(gender,'gender')
    </script>
```

### 14、javascript中filter过滤

> filter() 方法遍历数组的每个元素，并返回一个新数组，其中只包含那些使得提供的函数返回 true 的元素。
经常用于过滤筛选不需要的值

```javascript
this.list.filter((v)=>v.id%2 !==2)
```

### 15、v-for中处理特殊项

```html
<div v-for="(item, index) in items" :key="index">
  <div v-if="item.special" :class="{ 'special-item': true }">
    <!-- 特殊项的处理方式 -->
    {{ item.name }} (with special feature)
  </div>
  <div v-else>
    <!-- 普通项的处理方式 -->
    {{ item.name }}
  </div>
</div>

<style>
  .special-item {
    color: red;
    font-weight: bold;
  }
</style>
```
