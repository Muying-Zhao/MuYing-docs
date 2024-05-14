# 前言

> 本章将学习Vue3组合式API（即：Composition API），组合式API是Vue3组织代码的一种新型写法，有别于选项式的API。学会使用这种风格进行编程，并应用在项目之中。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240429/1714379682639739438.png)

setup方法与script_setup及ref响应式
事件方法_计算属性_reactive_toRefs
生命周期_watch_watchEffect
跨组件通信方案provide_inject
复用组件功能之use函数
利用defineProps与defineEmits进行组件通信
利用组合式API开发复杂的搜索功能
利用组合式API开发搜索提示列表
利用组合式API开发搜索结果列表
利用组合式API开发搜索历史列表

# 一、setup方法与script_setup及ref响应式

## setup方法与script_setup

在Vue3.1版本的时候，提供了setup方法；而在Vue3.2版本的时候，提供了script_setup属性。那么setup属性比setup方法对于操作组合式API来说会更加的简易。

```vue

<template>
  <div>
    <h2>setup方法</h2>
    {{ count }}
  </div>
</template>

// Vue3.1
<script>
export default {
  setup () {
    let count = 0;
    return {
      count
    }
  }
}
</script>

// Vue3.2
<script setup>
let count = 0;
</script>
```

setup方法是需要把数据进行return后，才可以在template标签中进行使用，而setup属性方式定义好后就可以直接在template标签中进行使用。

## ref响应式

下面来学习一下，如何在组合式API中来完成数据的响应式操作，通过的就是ref()方法，需要从vue模块中引入这个方法后才可以使用。

```vue
<script setup>
import { ref } from 'vue';
let count = ref(0);   // count -> { value: 0 }
//count += 1;   //✖
count.value += 1;   // ✔
</scirpt>
```

count数据的修改，需要通过count.value的方式，因为vue底层对响应式数据的监控必须是对象的形式，所以我们的count返回的并不是一个基本类型，而是一个对象类型，所以需要通过count.value进行后续的操作，那么这种使用方式可能会添加我们的心智负担，还好可以通过Volar插件可以自动完成.value的生成，大大提升了使用方式。

那么现在count就具备了响应式变化，从而完成视图的更新操作。

那么ref()方法还可以关联原生DOM，通过标签的ref属性结合到一起，也可以关联到组件上。

```vue
<template>
  <div>
    <h2 ref="elem">setup属性方式</h2>
  </div>
</template>
<script setup>
import { ref } from 'vue';
let elem = ref();
setTimeout(()=>{
  console.log( elem.value );   //拿到对应的原生DOM元素
}, 1000)
</script>
```

# 二、事件方法_计算属性 reactive_toRefs

## 事件方法与计算属性

下面看一下在组合式API中是如何实现事件方法和计算属性的。

```vue
<template>
  <div>
    <button @click="handleChange">点击</button>
    {{ count }}, {{ doubleCount }}
  </div>
</template>
<script setup>
import { computed, ref } from 'vue';
let count = ref(0);
let doubleCount = computed(()=> count.value * 2)
let handleChange = () => {
  count.value += 1;
};
</script>
```

事件方法直接就定义成一个函数，计算属性需要引入computed方法，使用起来是非常简单的。

## reactive与toRefs

reactive()方法是组合式API中另一种定义响应式数据的实现方式，它是对象的响应式副本。

```vue
<template>
  <div>
    <h2>reactive</h2>
    {{ state.count }}
  </div>
</template>

<script setup>
import { reactiv} from 'vue';
let state = reactive({
  count: 0,
  message: 'hi vue'
})
state.count += 1;
</script>
```

* torefs：响应式对象转换为普通对象
reactive()方法返回的本身就是一个state对象，那么在修改的时候就不需要.value操作了，直接可以通过state.count的方式进行数据的改变，从而影响到视图的变化。

ref()和reactive()这两种方式都是可以使用的，一般ref()方法针对基本类型的响应式处理，而reactive()针对对象类型的响应式处理，当然还可以通过toRefs()方法把reactive的代码转换成ref形式。

```vue
<template>
  <div>
    <h2>reactive</h2>
    {{ state.count }}, {{ count }}
  </div>
</template>

<script setup>
import { reactive, toRefs } from 'vue';

let state = reactive({
  count: 0
})
let { count } = toRefs(state);   //  let count = ref(0)
setTimeout(() => {
  //state.count += 1;
  count.value += 1;
}, 1000)

</script>
```

# 三、生命周期_watch_watchEffect

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240429/1714380805180352788.png)

## 生命周期钩子函数

在学习选项式API的时候，我们学习了生命周期钩子函数，那么在组合式API中生命周期又是如何使用的呢？下面我们从图中先看一下对比的情况吧。

<div align=center>
    <img src="./img/05-03-生命周期对比.png" />
    <div>生命周期对比</div>
</div>
那么具体的区别如下：

- 组合式中是没有beforeCreate和created这两个生命周期，因为本身在组合式中默认就在created当中，直接定义完响应式数据后就可以直接拿到响应式数据，所以不需要再有beforeCreate和created这两个钩子
- 组合式的钩子前面会有一个on，类似于事件的特性，那就是可以多次重复调用

```vue
<script>
import { onMounted, ref } from 'vue';
let count = ref(0);
onMounted(()=>{
  console.log( count.value );
});
onMounted(()=>{
  console.log( count.value );
});
onMounted(()=>{
  console.log( count.value );
});
</script>
```

## watch与watchEffect

这里先说一下watchEffect的用法，为了根据响应式状态自动应用和重新应用副作用，我们可以使用 watchEffect 函数。它立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

watchEffect常见特性：

- 一开始会初始触发一次，然后所依赖的数据发生改变的时候，才会再次触发
- 触发的时机是数据响应后，DOM更新前，通过flush: 'post' 修改成DOM更新后进行触发
- 返回结果是一个stop方法，可以停止watchEffect的监听
- 提供一个形参，形参主要就是用于清除上一次的行为

```vue
<template>
  <div>
    <h2>watchEffect</h2>
    <div>{{ count }}</div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue';
let count = ref(0);
// const stop = watchEffect(()=>{
//   console.log(count.value);
// }, {
//   flush: 'post'
// })

// setTimeout(()=>{
//   stop();
// }, 1000)

// setTimeout(()=>{
//   count.value += 1;
// }, 2000)

watchEffect((cb)=>{
  console.log(count.value);
  cb(()=>{
    //更新前触发和卸载前触发，目的：清除上一次的行为(停止上一次的ajax，清除上一次的定时器)
    console.log('before update');
  })
})

setTimeout(()=>{
  count.value += 1;
}, 2000)
</script>
```

再来看一下watch侦听器的使用方式，如下：

```vue
<script setup>
import { ref, watch } from 'vue';
let count = ref(0);
watch(count, (newVal, oldVal) => {
  console.log(newVal, oldVal);
})
setTimeout(()=>{
  count.value = 1;
}, 2000)
</script>
```

> 为了根据响应式状态自动应用和重新应用副作用，我们可以使用watchEffect函数。它立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数

那么watch与watchEffect的区别是什么呢？

- 懒执行副作用
- 更具体地说明什么状态应该触发侦听器重新运行
- 访问侦听状态变化前后的值

# 四、跨组件通信方案provide_inject

## 依赖注入

在Vue中把跨组件通信方案provide_inject也叫做依赖注入的方式，前面我们在选项式中也学习了它的基本概念，下面看一下在组合式API中改如何使用。

```vue
// provide.vue
<template>
  <div>
    <my-inject></my-inject>
  </div>
</template>
<script setup>
import MyInject from '@/inject.vue'
import { provide, ref, readonly } from 'vue'
//传递响应式数据
let count = ref(0);
let changeCount = () => {
  count.value = 1;
}
provide('count', readonly(count))
provide('changeCount', changeCount)

setTimeout(()=>{
  count.value = 1;
}, 2000)
</script>

//inject.vue
<template>
  <div>
    <div>{{ count }}</div>
  </div>
</template>

<script setup>
import { inject } from 'vue'
let count = inject('count')
let changeCount = inject('changeCount')
setTimeout(()=>{
  changeCount();
}, 2000);

</script>
```

依赖注入使用的时候，需要注意的点：

- 不要在inject中修改响应式数据，可利用回调函数修改
- 为了防止可设置成 readonly



# 五、复用组件功能之use函数

为了更好的组合代码，可以创建统一规范的use函数，从而抽象可复用的代码逻辑。利用use函数可以达到跟mixin混入一样的需求，并且比mixin更加强大。

```js
// useCounter.js
import { computed, ref } from 'vue';
function useCounter(num){
  let count = ref(num);
  let doubleCount = computed(()=> count.value * 2 );
  return {
    count,
    doubleCount
  }
}

export default useCounter;
```

```vue
<template>
  <div>
    <h2>use函数</h2>
    <div>{{ count }}, {{ doubleCount }}</div>
  </div>
</template>
<script setup>
import useCounter from '@/compotables/useCounter.js'
let { count, doubleCount } = useCounter(123);
setTimeout(()=>{
  count.value += 1;
}, 2000);
</script>
```

通过useCounter函数的调用，就可以得到内部return出来的对象，这样就可以在.vue文件中进行功能的使用，从而实现功能的复用逻辑。

# 六、利用defineProps与defineEmits进行组件通信

在组合式API中，是通过defineProps与defineEmits来完成组件之间的通信。

## defineProps

defineProps是用来完成父子通信的，基本使用跟选项式中的props非常的像，代码如下：

```vue
// parent.vue
<template>
  <div>
    <h2>父子通信</h2>
    <my-child :count="0" message="hello world"></my-child>
  </div>
</template>
<script setup>
import MyChild from '@/child.vue'
</script>

// child.vue
<template>
  <div>
    <h2>hi child, {{ count }}, {{ message }}</h2>
  </div>
</template>
<script setup>
import { defineProps } from 'vue'
const state = defineProps({   // defineProps -> 底层 -> reactive响应式处理的
  count: {
    type: Number
  },
  message: {
    type: String
  }
});
console.log( state.count, state.message );
</script>
```

## defineEmits

defineEmits是用来完成子父通信的，基本使用跟选项式中的emits非常的像，代码如下：

```vue
// parent.vue
<template>
  <div>
    <h2>子父通信</h2>
    <my-child @custom-click="handleClick"></my-child>
  </div>
</template>
<script setup>
import MyChild from '@/child.vue'
let handleClick = (data) => {
  console.log(data);
}
</script>

// child.vue
<script setup>
import { defineEmits } from 'vue'
const emit = defineEmits(['custom-click']);
setTimeout(()=>{
  emit('custom-click', '子组件的数据');
}, 2000)
</script>
```

# 七，搜索框demo
