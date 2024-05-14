# 前言

> 本章将学习Vue3的高级特性，通俗易懂的方式来理解这些高级知识点，对面试、复杂应用开发及全面了解Vue3框架都有很大的帮助。
- ref属性在元素和组件上的分别使用
- 利用nextTick监听DOM更新后的情况
- 自定义指令与自定义全局属性及应用场景
- 复用组件功能之Mixin混入
- 插件的概念及插件的实现
- Element Plus框架的安装与使用
- transition动画与过渡的实现
- 动态组件与keep-alive组件缓存
- 异步组件与Suspense一起使用
- 跨组件间通信方案 Provide_Inject
- Teleport实现传送门功能
- 虚拟DOM与render函数及Diff算法

# 一、ref属性

> Vue是基于MVVM设计模式进行实现的，视图与数据不直接进行通信，但是Vue并没有完全遵循这一原则，而是允许开发者直接进行原生DOM操作。在Vue中可通过ref属性来完成这一行为，通过给标签添加ref属性，然后再通过vm.$refs来获取DOM，代码如下

```vue
<template>
  <div>
    <h2>ref属性</h2>
    <button @click="handleClick">点击</button>
    <div ref="elem">aaaaaaaaa</div>
    <div ref="elem2">bbbbbbbbb</div>
  </div>
</template>

<script>
  export default {
    methods: {
      handleClick(){
        // ref属性添加到元素身上，可以获取到当前元素的原生DOM
        console.log( this.$refs.elem );
        console.log( this.$refs.elem2 );
      }
    }
  }
</script>
```

> 除了可以把ref属性添加给DOM元素外，还可以把ref属性添加给组件，这样可以获取到组件的实例对象，可以间接的实现组件之间的通信，代码如下：

```vue
<template>
  <div>
    <h2>ref属性</h2>
    <my-head ref="elem3"></my-head>
  </div>
</template>

<script>
  import MyHead from '@/2_头部组件.vue'
  export default {
    methods: {
      handleClick(){
        // ref属性添加到组件身上，可以获取到当前组件的vm对象(实例对象)
        console.log( this.$refs.elem3 );
        console.log( this.$refs.elem3.message );
        this.$refs.elem3.handleMessage('根组件的数据');
        //$refs 也可以实现间接的父子通信
      }
    }
  }
</script>
```


```vue
<template>
  <div>
    hello myhead
  </div>
</template>

<script>
  export default {
    data(){
      return {
        message: '头部组件的消息'
      }
    },
    methods: {
      handleMessage(data){
        console.log(data);
      }
    }
  }
</script>
```

# 二、利用nextTick监听DOM更新后的情况

> 它的主要作用是将回调推迟到下一个 DOM 更新周期之后执行。在更改了一些数据以等待 DOM 更新后立即使用它。
默认情况下，数据的更新会产生一个很小的异步延迟，所以直接再数据改变后取获取DOM是得不到DOM更新后的结果，而得到的是DOM更新前的结果。

```vue
<template>
  <div>
    <h2>hello nextTick</h2>
    <div ref="elem">{{ message }}</div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        message: 'hello world'
      }
    },
    mounted(){
      setTimeout(()=>{
        this.message = 'hi vue';
        console.log( this.$refs.elem.innerHTML );  // 'hello world'
      }, 2000)
    }
  }
</script>
```

> 如何才能得到DOM更新后的结果呢，可以有两种方案，第一种就是利用生命周期updated这个钩子函数，第二种就是利用我们讲的nextTick方法，支持两种风格即回调和promise。

```vue
<template>
  <div>
    <h2>hello nextTick</h2>
    <div ref="elem">{{ message }}</div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        message: 'hello world'
      }
    },
    mounted(){
      setTimeout(()=>{
        this.message = 'hi vue';
        /* this.$nextTick(()=>{
          console.log( this.$refs.elem.innerHTML );   // 'hi vue'
        }) */
        this.$nextTick().then(()=>{
          console.log( this.$refs.elem.innerHTML );  // 'hi vue'
        })
      }, 2000)
    },
    updated(){
       console.log( this.$refs.elem.innerHTML );  // 'hi vue'
    }
  }
</script>
```

# 三、自定义指令与自定义全局属性及应用场景

> 除了核心功能默认内置的指令 (例如 v-model 和 v-show)，Vue 也允许注册自定义指令，来实现一些封装功能。

## 1、自定义指令的实现

> 首先我们先来实现一个简单的`v-color`指令，用于给元素添加背景色，代码如下


```vue
<template>
  <div>
    <h2>自定义指令</h2>
    <div @click="handleClick" v-color="color">aaaaaaa</div>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        color: 'red'
      }
    },
    //创建局部的自定义指令
    directives: {
      // el, binding中el指的事dom，binding指的是拿到的内容（red）
      /* color: {
        mounted(el, binding){
          el.style.background = binding.value
        },
        updated(el, binding){
          el.style.background = binding.value
        }
      } */
      // 箭头函数可以被当做回调函数
      color: (el, binding) => {
        el.style.background = binding.value
      }
    }
  }
</script>
```

> 这里的回调函数是指令中mounted生命周期和updated生命周期的简写方式。
下面我们来完成一个实际可以应用的指令，按钮权限指令，一般情况下这种指令不会`局部`使用，而是`全局`使用，所以可以通过vue来实现一个全局的按钮权限指令，代码如下：

```vue

// src/main.js中

const app=createApp(App)
app.directive('auth', (el, binding) => {
    let auths = ['edit', 'delete'];
    let ret = auths.includes(binding.value);
    if(!ret){
    el.style.display = 'none';
    }
});
app.mounnt('#app')

// demo.vue
<template>
<button v-auth="'edit'">编辑</button>
</template>
```

## 2、自定义全局属性

> 封装一个全局函数，
添加一个可以在应用的任何组件实例中访问的全局 property，这样在引入一些第三方模块的时候，就不用每一次进行import操作，而是直接通过this对象去访问全局属性即可，下面举一个例子，实现一个http的全局属性。

* 新建`src/http.js`

```javascript
function get(params) {
    console.log('get request')
}
export {
    get
}
```

* 在`main.js`中引入全局指令

```javascript
import { createApp } from 'vue'
import Home from './Home.vue'
import * as http from "./http.js";
const app = createApp(Home)
app.config.globalProperties.$http=http;
app.mount('#app')
```

* 在`demo.vue`中使用全局属性

```vue
<template>
    <div class="wrapper">
       <div @click="handleClick">编辑</div>
    </div>
</template>
<script>
export default {
    name:'Home',
    data(){
       return {}
    },
    methods:{
      handleClick(){
        this.$http.get()
      }
    }
}
</script>
<style scoped lang="scss">
</style>
```

# 四、复用组件功能之Mixin混入

## 1、Mixin混入

> 本小节我们来了解一下mixin混入，它是选项式API的一种复用代码的形式，可以非常灵活的复用功能。

```js
// mymixin.js
const myMixin = {
  data(){
    return {
      message: '复用的数据'
    }
  },
  computed: {
    message2(){
      return '复用的数据2'
    }
  }
};
export {
  myMixin
}
// mymixin.vue
<template>
  <div>
    <h2>mixin混入</h2>
    <div>{{ message }}</div>
    <div>{{ message2 }}</div>
  </div>
</template>
<script>
  import { myMixin } from '@/mymixin.js'
  export default {
    mixins: [myMixin]
  }
</script>
```

## 2、全局混入
> 这样就可以直接在.vue中使用这些混入的功能。当然这种方式是局部混入写法，也可以进行全局混入的写法，代码如下：

```javascript
// main.js
import { myMixin } from '@/mymixin.js'
app.mixin(myMixin)
```

> mixin存在的问题也是有的，那就是不够灵活，不支持传递参数，这样无法做到差异化的处理，所以目前比较推荐的复用操作还是选择使用组合式API中的use函数来完成复用的逻辑处理。后面章节我们会学习到这种组合式API的应用。

# 五、插件的概念及插件的实现

> 插件是自包含的代码，通常向 Vue 添加全局级功能。例如：全局方法、全局组件、全局指令、全局mixin等等。基于Vue的第三方模块都是需要通过插件的方式在Vue中进行生效的，比如：Element Plus、Vue Router、Vuex等等。

```js
// myplugin.js
import * as http from '@/http.js'
export default {
  install(app, options){
    console.log(options);
    app.config.globalProperties.$http = http;
    app.directive('auth', (el, binding) => {
      let auths = ['edit', 'delete'];
      let ret = auths.includes(binding.value);
      if(!ret){
        el.style.display = 'none';
      }
    });
    app.component('my-head', {
      template: `<div>hello myhead</div>`
    })
  }
}
// main.js 让插件生效
import myplugin from './myplugin.js'
app.use(myplugin, { info: '配置信息' })
```
> vue.config.js,
注意：使用 runtimeCompiler: true 会增加构建后的包大小，因为 Vue 需要包含额外的模板编译器代码。因此，除非你真的需要它，否则在生产环境中最好避免使用它。

在 Vue.js 中，组件的模板通常有两种编译方式：
预编译：这是默认的方式。当你使用 Vue CLI 构建项目时，模板会在构建过程中被编译成 JavaScript 的渲染函数。这意味着在浏览器运行时，模板已经被转换成了函数，并可以直接执行，无需再次编译。这种方式的好处是性能较高，因为模板只需要编译一次。
运行时编译：当设置 runtimeCompiler: true 时，Vue 会在浏览器运行时动态地编译模板。这意味着，如果你的组件中有动态模板内容（例如，通过字符串或 innerHTML 插入的模板），那么这些模板会在浏览器端被编译。然而，这种方式的性能通常较低，因为每次组件被创建或更新时，模板都需要重新编译。

```
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  //需要在vue.config.js中配置runtimeCompiler
  runtimeCompiler:true
})
```

> 可以看到，让插件生效的语法为`app.use`，这样就可以跟Vue结合到一起，所以插件就可以独立出去，成为第三方模块。

# 六、Element Plus插件
# Element Plus框架的安装与使用

前面小节中介绍了自定义插件的实现，那么本小节来看下一比较出名的第三方插件Element Plus如何安装与使用。

## Element Plus框架

Element Plus是一套基于PC端的组件库，可以直接应用到很多管理系统的后台开发中，使用前需要先下载安装，除了下载组件库以外，最好也下载组件库对应的icon图标模块，如下：

```shell
npm install element-plus @element-plus/icons-vue
```

接下来把element plus完整引入到Vue中，包装全局组件，全局样式。

```js
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

app.use(ElementPlus)
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
	app.component(key, component)
}
```

## 基本使用方式

element plus中提供了非常多的常见组件，例如：按钮、评分、表单控件等等。

```vue
<template>
  <div>
    <h2>element plus</h2>
    <el-button @click="handleClick" type="primary" icon="Clock">Primary</el-button>
    <el-button @click="handleClick2" type="success">Success</el-button>
    <el-rate v-model="value1" />
    <el-icon><Clock /></el-icon>
  </div>
</template>
<script>
  import { ElMessage, ElNotification } from 'element-plus';
  export default {
    data(){
      return {
        value1: 3
      }
    },
    mounted(){
      setTimeout(()=>{
        this.value1 = 5;
      }, 2000)
    },
    methods: {
      handleClick(){
        ElMessage.success('提示成功的消息');
      },
      handleClick2(){
        ElNotification({
          title: '邮件',
          message: '您今日的消费记录'
        });
      }
    }
  }
</script>
```

除了常见的组件外，element plus中也提供了一些逻辑组件，这些逻辑组件是可以直接在JavaScript中进行使用，例如：ElMessage，ElNotification等方法。


# 七、transition

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240429/1714374675430219606.png)

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240429/1714374785024957785.png)


## 动画与过渡的实现

在Vue中推荐使用CSS3来完成动画效果。当在插入、更新或从 DOM 中移除项时，Vue 提供了多种应用转换效果的方法。

## transition动画

Vue中通过两个内置的组件来实现动画与过渡效果，分别是：`<transition>`和`<transition-group>`，代码如下：

```vue
<template>
  <div>
    <h2>hello transition</h2>
    <button @click=" isShow = !isShow ">点击</button>
    <transition name="slide" mode="out-in">
      <div v-if="isShow" class="box"></div>
      <div v-else class="box2"></div>
    </transition>
  </div>
</template>
<script>
  export default {
    data(){
      return {
        isShow: true
      }
    }
  }
</script>
<style scoped>
.box{
  width: 200px;
  height: 200px;
  background: skyblue;
}
.box2{
  width: 200px;
  height: 200px;
  background: pink;
}
.slide-enter-from{
  opacity: 0;
  transform: translateX(200px);
}
.slide-enter-to{
  opacity: 1;
  transform: translateX(0);
}
.slide-enter-active{
  transition: 1s;
}

.slide-leave-from{
  opacity: 1;
  transform: translateX(0);
}
.slide-leave-to{
  opacity: 0;
  transform: translateX(200px);
}
.slide-leave-active{
  transition: 1s;
}
</style>
```

其中`<transition>`组件通过`name`属性去关联CSS中的选择器，CSS中的选择器主要有6种，分别：

- `v-enter-from`：进入动画的起始状态。在元素插入之前添加，在元素插入完成后的下一帧移除。
- `v-enter-active`：进入动画的生效状态。应用于整个进入动画阶段。在元素被插入之前添加，在过渡或动画完成之后移除。这个 class 可以被用来定义进入动画的持续时间、延迟与速度曲线类型。
- `v-enter-to`：进入动画的结束状态。在元素插入完成后的下一帧被添加 (也就是 `v-enter-from` 被移除的同时)，在过渡或动画完成之后移除。
- `v-leave-from`：离开动画的起始状态。在离开过渡效果被触发时立即添加，在一帧后被移除。
- `v-leave-active`：离开动画的生效状态。应用于整个离开动画阶段。在离开过渡效果被触发时立即添加，在过渡或动画完成之后移除。这个 class 可以被用来定义离开动画的持续时间、延迟与速度曲线类型。
- `v-leave-to`：离开动画的结束状态。在一个离开动画被触发后的下一帧被添加 (也就是 `v-leave-from` 被移除的同时)，在过渡或动画完成之后移除。 

<div align=center>
    <img src="./img/04-01-动画与过渡.png" />
    <div>动画与过渡</div>
</div>

默认情况下，进入和离开在两个元素身上是同时执行的，如果想改变其顺序，需要用到`mode`属性，其中`out-in`表示先离开再进入，而`in-out`表示先进入再离开。


# 八、动态组件，tab切换

# 动态组件与keep-alive组件缓存

## 动态组件

动态组件可以实现在同一个容器内动态渲染不同的组件，依一个内置组件`<component>`的`is`属性的值，来决定使用哪个组件进行渲染。

```vue
<template>
  <div>
    <h2>动态组件</h2>
    <button @click=" nowCom = 'my-com1' ">组件1</button>
    <button @click=" nowCom = 'my-com2' ">组件2</button>
    <button @click=" nowCom = 'my-com3' ">组件3</button>
 	<component :is="nowCom"></component>
  </div>
</template>

<script>
import MyCom1 from '@/13_MyCom1.vue'
import MyCom2 from '@/14_MyCom2.vue'
import MyCom3 from '@/15_MyCom3.vue'
  export default {
    data(){
      return {
        nowCom: 'my-com1'
      }
    },
    components: {
      'my-com1': MyCom1,
      'my-com2': MyCom2,
      'my-com3': MyCom3
    }
  }
</script>
```

## keep-alive组件

当我们点击的时候，就会进行组件的切换。在每次切换的过程中都会重新执行组件的渲染，这样组件操作的行为就会还原，而我们如何能够保证组件不变呢？可以利用`<keep-alive>`对组件进行缓存，这样不管如何切换，都会保持为初始的组件渲染，这样可以很好的保留之前组件的行为。

组件的切换也可以配合`<transition>`完成动画的切换。

```vue
<template>
  <div>
    <h2>动态组件</h2>
    <button @click=" nowCom = 'my-com1' ">组件1</button>
    <button @click=" nowCom = 'my-com2' ">组件2</button>
    <button @click=" nowCom = 'my-com3' ">组件3</button>
    <transition name="slide" mode="out-in">
      <keep-alive>
        <component :is="nowCom"></component>
      </keep-alive>
    </transition>
  </div>
</template>
```

# 九、异步组件与Suspense一起使用

## 异步组件

在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。

在上一个小节的动态组件的基础上，进行异步组件的演示。首先可以打开chrome浏览器的network网络，可以观察到在动态组件切换的时候，network网络中没有进行任何请求的加载，这证明了在初始的时候，相关的动态组件就已经加载好了。

所以对于大型项目来说，如果能实现按需载入的话，那么势必会对性能有所提升，在Vue中主要就是利用defineAsyncComponent来实现异步组件的。

```vue
<script>
import { defineAsyncComponent } from 'vue'
export default {
    data(){
        return {
            nowCom: 'my-com1'
        }
    },
    components: {
        'my-com1': defineAsyncComponent(() => import('@/MyCom1.vue')),
        'my-com2': defineAsyncComponent(() => import('@/MyCom2.vue')),
        'my-com3': defineAsyncComponent(() => import('@/MyCom3.vue'))
    }
}
</script>
```

## Suspense组件

由于异步组件是点击切换的时候才去加载的，所以可能会造成等待的时间，那么这个时候可以配合一个loading效果，在Vue中提供了一个叫做`<Suspense>`的组件用来完成loading的处理。

```vue
<template>
	<suspense>
        <component :is="nowCom"></component>
        <template #fallback>
            <div>loading...</div>
		</template>
	</suspense>
</template>
```

# 十、跨组件间通信方案 Provide_Inject

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240429/1714377344370768556.png)


## 跨组件通信方案

正常情况下，我们的组件通信是需要一级一级的进行传递，通过父子通信的形式，那么如果有多层嵌套的情况下，从最外层把数据传递给最内层的组件就非常的不方便，需要一级一级的传递下来，那么如何才能方便的做到跨组件通信呢？

可以采用Provide 和 inject 依赖注入的方式来完成需求，代码如下：

<div align=center>
    <img src="./img/04-02-依赖注入.png" />
    <div>依赖注入</div>
</div>



```vue
// provide.vue
<script>
export default {
    provide(){
        return {
            message: 'hello provide',
            count: this.count,
            getInfo(data){
                console.log(data);
            }
        }
    }
}
</script>

// inject.vue
<template>
<div>
    hello inject, {{ message }}, {{ count }}
 </div>
</template>

<script>
export default {
    inject: ['message', 'getInfo', 'count'],
    mounted(){
        this.getInfo('hello inject');
    }
}
</script>
```

## Provide与Inject注意点

- 保证数据是单向流动的，从一个方向进行数据的修改
- 如果要传递响应式数据，需要把provide改造成工厂模式发送数据

# 十一、Teleport实现传送门功能

## Teleport组件

Teleport可以实现传送门功能，也就是说逻辑属于当前组件中，而结构需要在组件外进行渲染，例如：按钮模态框组件。

```vue
// 模态框.vue
<template>
  <div>
    <button @click=" isShow = true ">点击</button>
    <teleport to="body">
      <div v-if="isShow">模态框</div>
    </teleport>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        isShow: false
      }
    }
  }
</script>
// 调用模态框.vue
<template>
  <div>
    <h2>传送门</h2>
    <my-modal></my-modal>
  </div>
</template>

<script>
import MyModal from '@/模态框.vue'
  export default {
    components: {
      'my-modal': MyModal
    }
  }
</script>
```

## 逻辑组件

但是往往我们需要的并不是普通组件的调用方式，而是逻辑组件的调用方式，那么如何实现逻辑组件呢？代码如下：

```js
//  定义逻辑组件，modal.js

import { createApp } from 'vue';
import ModalVue from '@/模态框.vue';

function modal(){
  let div = document.createElement('div');
  createApp(ModalVue).mount(div);
  document.body.append(div);
}

export default modal;
```

```vue
// 调用逻辑组件
<template>
  <div>
    <h2>传送门</h2>
    <button @click="handleClick">点击</button>
  </div>
</template>

<script>
import modal from '@/modal.js'
export default {
  methods: {
    handleClick(){
      modal();
    }
  }
}
</script>
```

# 十二、虚拟DOM与render函数及Diff算法

## 虚拟DOM

Vue框架帮我们完成了大量的DOM操作，那么在底层Vue并没有直接操作真实的DOM，因为真实的DOM直接去操作是非常好性能的，所以最好在JS环境下进行操作，然后在一次性进行真实DOM的操作。

```js
const vnode = {
  type: 'div',
  props: {
    id: 'hello'
  },
  children: [
    /* 更多 vnode */
  ]
}
```

那么在Vue中是如何把`<template>`模板中的字符串编译成虚拟DOM的呢？需要用到内置的render函数，这个函数可以把字符串转换成虚拟DOM。

<div align=center>
    <img src="./img/04-03-虚拟DOM.png" />
    <div>虚拟DOM</div>
</div>

## Diff算法

当更新的时候，一个依赖发生变化后，副作用会重新运行，这时候会创建一个更新后的虚拟 DOM 树。运行时渲染器遍历这棵新树，将它与旧树进行比较，然后将必要的更新应用到真实 DOM 上去。

而两个虚拟DOM进行对比的时候，需要加入一些算法提高对比的速度，这个就是Diff算法。

<div align=center>
    <img src="./img/04-04-Diff算法.png" />
    <div>Diff算法</div>
</div>

在脚手架下我们推荐使用<template>来完成结构的编写，那么也可以直接通过render函数进行虚拟DOM的创建，代码如下：

```vue
<!-- <template>
  <div>
    <h2>render</h2>
  </div>
</template> -->
<script>
  import { h } from 'vue';
  export default {
    render(){
      return h('div', h('h2', 'render2'))
    }  
  }
</script>

<style scoped>

</style>
```
