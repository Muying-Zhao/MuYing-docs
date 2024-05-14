# 一、React 18简介

[React18 中文网](https://react.nodejs.cn/)

## 1、React是什么

> React是一个用于构建用户界面的JavaScript库，由Facebook开发，以其高效性能和简洁的API而备受开发者青睐。

## 2、React 18的新特性

* 并发模式

> React 18采用了并发模式，允许React在多个优先级任务之间进行切换，从而提高了应用的响应速度和用户体验。在之前的版本中，React是采用同步渲染模式，即一次只能渲染一个组件，如果某个组件渲染时间过长，会导致整个应用的响应性下降。而并发模式则可以更好地处理复杂的交互和动画效果。

* 新的渲染引擎

> React 18采用了新的渲染引擎，具有更好的性能和可扩展性，能更好地处理大型和复杂的组件树。这使得渲染大型应用程序时更加流畅，减少了卡顿现象。

* 服务器组件

> 服务器组件允许将组件的渲染任务从客户端移动到服务器端。这可以提高应用程序的性能和加载速度，并减轻客户端的工作负载。通过服务器端渲染（SSR），React 18还引入了新的“SSR”技术，可以在服务器端将组件树分割成小块，然后逐块发送给客户端，从而进一步提高了首次页面加载速度。

* 自动批处理

> React 18引入了自动化批处理机制，它可以在一次更新中收集和处理多个状态变更，减少了不必要的重新渲染。这样可以提高应用程序的性能和响应性能。在React 18中，自动批处理是React的内置功能，无需开发者进行显式操作。

* 更好的错误处理

> React 18提供了新的错误边界机制，可以捕获并处理组件树中的错误，从而避免整个应用崩溃。此外，React 18还引入了Suspense组件，可以在组件加载过程中显示占位符或加载指示器，提高了用户体验。

* 改变根节点的挂载方式

> React 18使用新的API createRoot 来创建根节点，而不是之前的 render 方法。使用 createRoot 后，React应用会更快地响应用户操作，提高用户体验。不过，旧的API仍然兼容，只有在使用 createRoot 之后才会有React 18的新特性。

* 不再支持IE浏览器

> React 18已经放弃了对IE浏览器的支持。这意味着开发者在开发新项目时，可以更加专注于现代浏览器的功能和性能优化。

# 二、虚拟DOM


## 1、虚拟DOM的优势。

> Vue和React框架都会自动控制DOM的更新，而直接操作真实DOM是非常耗性能的，虚拟DOM通过批量更新和最小化DOM操作，减少了对真实DOM的直接操作次数，从而提高了性能。

## 2、使用React18模块

> React模块负责生成虚拟DOM和提供组件系统，而react-dom（或react-dom/client）模块则负责将虚拟DOM转换为真实DOM，并处理与浏览器的交互。这两个库一起工作，使开发人员能够构建出高效且可维护的Web应用程序。

### 1)、react.development.js / react模块

* 生成虚拟DOM

> React使用虚拟DOM作为真实DOM的抽象表示。当组件的状态或属性发生变化时，React会创建一个新的虚拟DOM树，并与旧的虚拟DOM树进行比较，以确定哪些部分需要更新。

* 组件系统

> React提供了构建用户界面的组件系统。这些组件可以是函数组件或类组件，它们封装了UI的特定部分及其行为。

* JSX转换

> 虽然JSX本身不是JavaScript的有效语法，但React提供了一个Babel插件来将JSX转换为React.createElement调用，这使得编写组件变得更加直观和简洁。

* 事件系统

> React实现了自己的事件系统，用于处理DOM事件，并提供了跨浏览器的一致性。

* 生命周期方法和钩子（Hooks）

> React提供了生命周期方法和Hooks API，使开发人员能够管理组件的状态和生命周期。

### 2)、react-dom.development.js和react-dom/client模块

* Diff算法

> 当React的虚拟DOM树发生变化时，react-dom使用Diff算法来高效地找出哪些DOM节点需要更新、添加或删除。这个算法通过比较新旧虚拟DOM树来最小化必要的DOM操作。

* 处理真实DOM

> react-dom负责与浏览器的真实DOM进行交互。它负责将React组件渲染为DOM元素，并在必要时更新或删除它们。

* 服务端渲染（SSR）

> 在服务端渲染React组件为HTML字符串，然后将这些字符串发送到客户端。


## 3、构建虚拟DOM

* React 和 ReactDOM 的 CDN 链接

> 使用React 和 ReactDOM 的 CDN 在线链接或下载到本地项目中

```
// react对象,生成虚拟DOM
https://unpkg.com/react@18.3/umd/react.development.js
// reactdom对象,Diff算法和处理真实DOM
https://unpkg.com/react-dom@18.3/umd/react-dom.development.js
```

* 新建`index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js" crossorigin></script>
</head>
<body>
    <div id="app"></div>
    <script>
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        // React.createElement() ->创建虚拟dom
        let element =React.createElement('h1',{title:'hello world'},'hello world');
        root.render(element)
    </script>
</body>
</html>
```

# 三、JSX

## 1、什么是JSX

> JSX 是 JavaScript XML 的缩写，它是一种语法扩展，允许我们在 JavaScript 代码中书写类似于 XML 的标记。React 使用 JSX 来描述 UI 组件的结构和外观。JSX 使得我们可以使用类似 HTML 的语法来定义组件树，并且可以与 JavaScript 逻辑混合使用。

## 2、在React中使用JSX。

> 要在 React 中使用 JSX，你需要先确保你的构建环境（如 Babel 或 Create React App）已经配置了对 JSX 的转换。一旦配置完成，你就可以在 .js 或 .jsx 文件中使用 JSX 了。

[Babel在线转换器](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=DwEwlgbgfAFgpgGwQewAQHdkCcEmAenGiA&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.24.5&externalPlugins=&assumptions=%7B%7D)

* 可选:尝试通过 JSX 使用 React

[在线使用jsx链接](https://unpkg.com/babel-standalone@6/babel.min.js)

> 添加type="text/babel"属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        // 虚拟dom
        let element =(<h1 title="hello world">hello world</h1>)
        root.render(element)
    </script>
</body>
</html>
```

## 3、JSX详细使用方式

* 标签要小写
* 单标签要闭合
* class属性与for属性
* 多单次属性需驼峰，data-*不需要
* 唯一根节点
* {}模版语法
* 添加注释  {/* ... */}
* 属性渲染变量 vue中`:className="myClass"`,react中`className={myClass}`，`onclick={handleClick}`
* style渲染对象


```
let myStyle={
  color:'red'
};
style={myStyle}
```
* {}渲染JSX，{}将其变成js语法了，再放入标签依旧可以被js渲染


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        let element =(
            <div className="wrapper">
               <span>hello world</span>
               <label htmlFor="elemInput">用户名：</label>
               <input id="elemInput" type="text"/>
            </div>
            )
        root.render(element)
    </script>
</body>
</html>
```

# 三、类组件基本使用及组件通信

## 1、组件化开发的概念和优势。

> 组件化开发是一种软件开发方法，它将一个大型的应用程序或系统拆分成多个独立、可复用的组件。这些组件可以单独开发、测试和维护，从而提高了代码的可读性、可维护性和可扩展性。

* 提高代码质量

> 由于每个组件都是独立的，因此它们之间的耦合度较低，这有助于降低代码的复杂度和维护成本，提高代码的可读性和可维护性。

* 提高开发效率

> 组件化开发允许团队并行开发不同的组件，从而缩短了开发周期。此外，由于组件的可复用性，可以减少重复的开发工作，进一步提高开发效率。

* 方便团队协作

> 由于每个组件都是独立的，不同的团队可以分别开发不同的组件，从而提高了协作效率。此外，组件化开发还支持模块化的测试，使得测试工作更加便捷。

* 提高软件的可扩展性

> 由于每个组件都是独立的，因此可以方便地添加、删除或替换组件，这使得软件更加灵活，能够适应不同的需求变化。

## 2、函数组件和类组件的创建和使用。

* 函数组件

> 函数组件是一个简单的JavaScript函数，它接收props（属性）作为参数并返回React元素。函数组件没有自己的状态，也无法使用生命周期方法。它们通常用于表示纯UI组件，即那些只根据输入props渲染输出的组件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        function Testdemo() {
            return (
                <div>hello world</div>
            )
        }
        let element =(
            <div className="wrapper">
               <Testdemo/>
            </div>
            )
        root.render(element)
    </script>
</body>
</html>
```

* 类组件

> 类组件是使用ES6类语法定义的React组件。类组件有自己的状态和生命周期方法，并且可以处理更复杂的状态和交互逻辑。类组件通常用于表示有状态的UI组件，即那些需要根据内部状态或用户交互来更新UI的组件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Testdemo extends React.Component{
            render (){
                return (
                    <div>hello world</div>
                )
            }
        }
        let element =(
            <div className="wrapper">
               <Testdemo/>
            </div>
            )
        root.render(element)
    </script>
</body>
</html>
```

## 3、Props在组件中的使用。

### 1)、Props

> Props是组件的属性，它们从父组件传递到子组件。Props是只读的，子组件不能修改传递给它的props。

* props在函数组件中传参,如果只传属性默认值为true

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        function Testdemo(props) {
                return (
                    <div>hello world,my name is {props.name},and my age is {props.age}</div>
                )
        }
        let element =(
            <div className="wrapper">
               <Testdemo name="xiaomu" age={20} />
            </div>
            )
        root.render(element)
    </script>
</body>
```

* props在类组件中传参

> 通过this.props直接获取参数

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Testdemo extends React.Component{
            render (){
                return (
                    <div>hello world,my name is {this.props.name},and my age is {this.props.age}</div>
                )
            }
        }
        let element =(
            <div className="wrapper">
               <Testdemo name="xiaomu" age={20} />
            </div>
            )
        root.render(element)
    </script>
</body>
```

### 2)、通过回调函数方式向父组件传值

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Childdemo extends React.Component {
                render() {
                    this.props.callBack('propsData')
                    return (
                        <div>子组件，{this.props.name}</div>
                    )
                }
            }
        class Testdemo extends React.Component{
            callBack(data){
                console.log(data,'data')
            }
            render (){
                return (
                    <div>
                      父组件，{this.props.name}
                      <Childdemo name={this.props.name} callBack={this.callBack} />
                    </div>
                )
            }
        }
        let element =(
            <div className="wrapper">
               <Testdemo name="xiaomu"/>
            </div>
            )
        root.render(element)
    </script>
</body>
```

### 3)、props在构造函数中使用

> 通常情况下，我们不会在构造函数中处理props，构造函数，但可以用于初始化组件的状态和绑定事件处理器

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Testdemo extends React.Component{
            constructor(props){
                super(props)
                console.log(this.props.name,'props')
            }
            render (){
                return (
                    <div>hello world,my name is {this.props.name},and my age is {this.props.age}</div>
                )
            }
        }
        let element =(
            <div className="wrapper">
               <Testdemo name="xiaomu" age={20} />
            </div>
            )
        root.render(element)
    </script>
</body>
```

### 4)、props传递属性过多时可以通过解构赋值实现

> 解构可以通过`{...userinfo}`

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Testdemo extends React.Component{
            render (){
                let {name,sex,age}= this.props
                return (
                    <div>{name} is a {sex} who is already {age} years old.</div>
                )
            }
        }
        let userinfo={
            name:"xiaomu",
            sex:"boy",
            age:20
        }
        let element =(
            <div className="wrapper">
               <Testdemo {...userinfo} />
            </div>
            )
        root.render(element)
    </script>
</body>
```

### 5)、props传递属性默认值设置`defaultProps`,默认属性`propTypes`

> 使用propTypes需要使用在线链接或者访问下载到本地

[prop-types在线链接](https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app=document.querySelector('#app')
        let root=ReactDOM.createRoot(app);
        class Testdemo extends React.Component{
            static defaultProps ={
                age:0
            }
            static propTypes={
                age: PropTypes.number
            }
            render (){
                let {name,sex, age }= this.props
                return (
                    <div>{name} is a {sex} who is already {age} years old.</div>
                )
            }
        }
        let userinfo={
            name:"xiaomu",
            sex:"boy",
            age:20
        }
        let element =(
            <div className="wrapper">
               <Testdemo {...userinfo} />
            </div>
            )
        root.render(element)
    </script>
</body>
</html>
```

## 4、类组件中的事件处理。

> 在React的类组件中，事件处理通常涉及到在组件的类中定义事件处理函数，并将这些函数作为事件监听器的回调绑定到DOM元素上。

* 使用箭头函数

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            handleClick = (event) => {
                alert('Button clicked!');
                console.log(event,'event')
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 解决使用handleClick方法时的this指向问题

> 在构造函数中调用`.bind(this)`是React类组件中常见的模式，用于确保事件处理函数中的this指向正确的组件实例。但是，使用箭头函数作为类的属性通常更为简洁和直观。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            constructor(props) {
                super(props);
                this.handleClick = this.handleClick.bind(this);
            }
            handleClick(event) {
                alert('Button clicked!');
                console.log(this, 'this')
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 事件传参，推荐使用高阶函数的写法

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            constructor(props) {
                super(props);
                this.handleClick = this.handleClick.bind(this);
            }
            handleClick(num) {
                return (event)=>{
                    alert('Button clicked!');
                    console.log(this, 'this') // Testdemo
                    console.log(num, 'num') // 123
                }
            }
            render() {
                return (
                    <button onClick={this.handleClick(111)}>
                        点击
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 5、类组件响应式数据实现与原理

> 在React的类组件中，响应式数据的实现主要依赖于组件的state和props。当state或props发生变化时，React会重新渲染组件，从而实现了数据的响应式更新。而通过state设置的响应式数据，它是组件内私有的，受控于当前组件。

### 1)、状态（State）

> 在类组件中，状态是一个特殊的对象，用于保存和更新组件内部的数据。这些数据可能会在组件的生命周期内发生变化，从而触发组件的重新渲染。

* 初始化

> 在组件的构造函数（constructor）中，通过调用super(props)并定义this.state来初始化状态。

* 更新

> 通过调用this.setState()方法来更新状态。这个方法接受一个对象或函数作为参数，用于描述状态的变化。React会合并这些变化并重新渲染组件。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                count:0
            }
            handleClick=()=> {
                this.setState({
                    count: this.state.count+1
                })
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 2)、setState的自动批处理

- 多次重复调用`this.setState({})`也只执行一次，它允许组件在其生命周期内动态地改变其输出,有助于减少在状态更改时发生的重新渲染次数。

> 打印结果为1

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                count:0
            }
            handleClick=()=> {
                this.setState({
                    count: this.state.count+1
                })
                this.setState({
                    count: this.state.count + 1
                })
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

- 在React18之前也有批处理的，但是在Promise、setTimeout、原生事件中是不起作用的

- ReactDOM.flushSync可以取消批处理操作

> 打印结果为2

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                count:0
            }
            handleClick=()=> {
                ReactDOM.flushSync(() => {
                    this.setState({
                        count: this.state.count + 1
                    })
                })
                ReactDOM.flushSync(() => {
                    this.setState({
                        count: this.state.count + 1
                    })
                })
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 3)、setState同时也是一个异步方法

> 在调用setState之后，React可能不会立即更新组件的state，并且可能会在未来的某个时间点批量更新多个setState调用。下列demo点击过后count为1，打印依旧为0，

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                count:0
            }
            handleClick=()=> {
                console.log(this.state.count)
                this.setState({
                    count: this.state.count+1
                })
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

> 由于setState是异步的，因此在setState之后立即读取this.state可能无法获取到最新的state值。为了确保在state更新完成后执行某些操作，可以使用setState的回调函数。这个回调函数会在state更新完成并且组件重新渲染后被调用，因此可以确保在回调函数中使用的state是最新的。在类组件中，setState接受一个回调函数作为第二个参数，这个回调函数会在状态更新并且重新渲染完成后被调用。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                count:0
            }
            handleClick=()=> {
                console.log(this.state.count,'回调之前的值,0')
                this.setState({
                    count: this.state.count+1
                },()=>{
                    console.log(this.state.count, '回调之后的值,1')
                })
            }
            render() {
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

> 同样，在函数组件中，可以使用useEffect Hook来监听state的变化，并在state更新后执行相应的操作。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        // 函数组件
        function Testdemo() {
            // 使用useState Hook来初始化状态
            const [count, setCount] = React.useState(0);
            // 事件处理函数
            const handleClick = () => {
                console.log(count, '回调之前的值');
                setCount(count + 1);
                setCount(count => {
                    console.log(count, '回调之后的值');
                });
            };
            return (
                <button onClick={handleClick}>
                    点击{count}
                </button>
            );
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 6、PureComponent 和 shouldComponentUpdate

> 由于组件会默认重新渲染，所以为了减少没必要的渲染，React给开发者提供PureComponent 和 shouldComponentUpdate 处理组件重新渲染的两个不同方法

### 1)、组件会默认重新渲染

> 当count不变时，点击按钮组件依然会默认重新渲染并打印`render`

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                count: 0
            }
            handleClick = () => {
                this.setState({
                    count: 0
                })
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 2）、PureComponent

> `React.PureComponent` 是 React.Component 的一个子类，当父组件重新渲染时，PureComponent 将会进行`浅比较`来确定其 props 和 state 是否发生了更改。如果 props 和 state 保持不变（即它们的引用没有变化，并且对于对象或数组，它们的内部值也没有通过引用改变），那么 PureComponent 将不会触发重新渲染，从而避免了不必要的DOM操作和组件树的遍历。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.PureComponent {
            state = {
                count: 0
            }
            handleClick = () => {
                this.setState({
                    count: 0
                })
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 3)、shouldComponentUpdate

> shouldComponentUpdate() 是一个在 React.Component 中可以覆盖的生命周期方法。它允许你自定义组件是否应该重新渲染。默认情况下，它返回 true，意味着每次父组件重新渲染时，子组件都会重新渲染。但是，你可以覆盖此方法并返回 false 来阻止重新渲染。所以`React.PureComponent`它其实实现了`shouldComponentUpdate()`方法以进行浅比较（shallow comparison）。nextProps即将传递给组件的props（属性）,nextState即将传递给组件的state（状态）的值

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                count: 0
            }
            handleClick = () => {
                this.setState({
                    count: 1
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if(this.state.count=== nextState.count){
                    return false
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.count}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 7、`浅比较`、`深拷贝`、`深比较`

### 1)、`浅比较`

> `PureComponent`是进行`浅比较`来确定其 props 和 state 是否发生了更改。如果你传递了一个数组或对象作为 prop，并且这个数组或对象的内容发生了变化但引用没有变化（即你修改了数组或对象的内部属性，但没有创建新的数组或对象），那么 PureComponent 的浅比较就会失效，因为它不会深入比较这些嵌套属性。下列demo中obj1将不会发生改变。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1:{
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2= this.state.obj1
                obj2.b.c=3
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if(this.state.obj1 === nextState.obj1){
                    return false
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 2)、通过扩展运算或解构方法来解决浅比较问题

* `...`

> 在对象上使用展开语法时，它主要用于对象字面量的属性拷贝，但有一个重要的限制：它只拷贝对象自身的可枚举属性，不包括继承的属性或不可枚举的属性。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1: {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2 = { ...this.state.obj1};
                obj2.b.c=3
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate = (nextProps, nextState) => {
                if (this.state.obj1 == nextState.obj1) {
                    return false
                }
                else {
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

> 对于数组，展开语法可以非常直观和简单地使用，因为它允许你将一个数组的元素展开到另一个数组

```js
const array1 = [1, 2, 3];
const array2=[...array1]; // false
const array3 = [...array1,4,5]; // [1, 2, 3, 4, 5]
```

* 解构对象

> 解构赋值也可以用于嵌套对象

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1: {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                let { a, b:{c} } = this.state.obj1; 
                c=3
                let obj2={a,b:{c}}
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate = (nextProps, nextState) => {
                if (this.state.obj1 == nextState.obj1) {
                    return false
                }
                else {
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 3)、通过深拷贝来解决浅比较的问题

* JSON.parse(JSON.stringify()

> `JSON.stringify()`这个方法将一个 JavaScript 对象或值转换为一个 JSON 格式的字符串。在转换过程中，它会遍历对象的所有可枚举属性，并将它们转换为字符串表示。如果属性是对象或数组，则会递归地进行此过程。`JSON.parse()`这个方法将一个 JSON 格式的字符串转换回一个 JavaScript 对象或值。所以可以通过`JSON.parse(JSON.stringify())`用于将一个 JavaScript 对象或值“深拷贝”为另一个新的对象或值。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1:{
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2= JSON.parse(JSON.stringify(this.state.obj1))
                obj2.b.c=3
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if(this.state.obj1 === nextState.obj1){
                    return false
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 使用第三方库（如lodash的_.cloneDeep）

> 有许多第三方库可以帮助进行深比较，lodash提供了一个_.cloneDeep方法用于深拷贝对象。

[lodash库](https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>

<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1:{
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2 = _.cloneDeep(this.state.obj1);
                obj2.b.c=3
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if(this.state.obj1 === nextState.obj1){
                    return false
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
</html>
```


* `immutable.js`处理不可变数据结构,

> Immutable.js主要用于提供不可变的数据结构，但它也间接地提供了深拷贝的功能，因为每次修改都会返回一个新的对象。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/immutable@4.0.0/dist/immutable.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1:{
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                // this.state.obj1 是一个普通的对象，需要先转换为 Immutable.Map
                let immutableObj1 = Immutable.fromJS(this.state.obj1);
                // 使用 Immutable.Map 的 setIn 方法来更新值
                let updatedImmutableObj1 = immutableObj1.setIn(['b', 'c'], 3);
                // 将 Immutable.Map 转换回普通对象
                let obj2 = updatedImmutableObj1.toJS();
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if(this.state.obj1 === nextState.obj1){
                    return false
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
</html>
```

### 4）、通过深比较来解决浅比较问题

* `JSON.stringify()`

> JSON.stringify() 方法会将一个 JavaScript 对象或值转换为一个 JSON 格式的字符串。在转换过程中，它会递归地遍历对象的所有可枚举属性，并将它们转换为字符串表示。对于数组，它会遍历每个元素并进行相同的转换。当两个对象或数组的结构相同（即它们的属性/元素数量、顺序和值都相同）时，JSON.stringify() 会生成相同的字符串。因此，通过比较这两个字符串，可以判断出这两个对象或数组是否“结构相同”。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1:{
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2={
                    a: 1,
                    b: {
                        c: 3
                    }
                }
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate=(nextProps,nextState)=>{
                if (JSON.stringify(this.state.obj1) !== JSON.stringify(nextState.obj1)) {
                    return true;
                }
                else{
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 使用第三方库（如lodash的_.isEqual）

> lodash的_.isEqual方法提供了一个方便的深比较实现。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>

<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1: {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                let obj2= {
                    a: 1,
                    b: {
                        c: 3
                    }
                }
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate = (nextProps, nextState) => {
                if (_.isEqual(this.state.obj1, nextState.obj1)) {
                    return false
                }
                else {
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>

</html>
```

* Immutable.js

> Immutable.js它内部还可以实现深比较（与深拷贝类似），当其提供的方法来更新数据时，它会返回一个新的数据结构，并且可以通过比较引用来判断数据是否发生变化。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/immutable@4.0.0/dist/immutable.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>

<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                obj1: {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const map1 = Immutable.Map(this.state.obj1);
                const map2 = map1.setIn(['b', 'c'], 3);
                let obj2 = map2.toJS();
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate = (nextProps, nextState) => {
                if (this.state.obj1 === nextState.obj1) {
                    return false
                }
                else {
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>

</html>
```

### 5）、JSON.parse(JSON.stringify()

> `JSON.parse(JSON.stringify()`在大多数情况下确实可以能对象的深拷贝，但有一些重要的限制和注意事项,所以建议使用专门的深拷贝库（如 `lodash` 的 `_.cloneDeep()`）或手动实现深拷贝函数。

* 序列化函数或循环引用。

> JSON.stringify() 不会序列化函数或循环引用。如果对象中包含函数或存在循环引用，那么这部分数据在序列化和反序列化过程中会丢失。

* 特殊值

> NaN、Infinity 和 -Infinity 等特殊值在 JSON.stringify() 中会被特殊处理，例如 NaN 会被转换为 null。

* Date 对象

> Date 对象在 JSON.stringify() 时会被转换为 ISO 格式的字符串，而在 JSON.parse() 后需要手动转换回 Date 对象。

* RegExp 对象

> 正则表达式（RegExp）对象不会被 JSON.stringify() 序列化，因此会丢失。

* 性能

> 对于大型对象或数组，这种方法可能会非常慢，因为它需要遍历对象的整个结构，并转换为字符串，然后再解析回对象。

* 对象原型链

> JSON.stringify() 不会保留对象的原型链。如果你依赖于对象的原型链中的属性或方法，那么这种方法可能不适用。

### 6)、手动实现深比较函数（解释深比较原理不推荐使用）

> 可以在shouldComponentUpdate生命周期方法中手动实现深比较的逻辑。这通常涉及到递归地遍历对象和数组，以检查它们的每个属性是否都相等。但是，这种方法可能会增加额外的计算负担，并可能降低性能。对于大型或复杂的数据结构，深比较可能会变得相当昂贵，因此应该谨慎使用。在可能的情况下，尽量使用不可变数据结构或更细粒度的状态管理来避免不必要的渲染。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prop-types/15.6.0/prop-types.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>

<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);

        function deepCompare(a, b) {
            // 如果两个值引用同一对象，则认为它们相等
            if (a === b) return true;

            // 如果其中一个值为null或undefined且它们不相等，则认为它们不相等
            if (a == null || b == null) return false;

            // 如果类型不同，则认为它们不相等
            if (a.constructor !== b.constructor) return false;

            // 如果它们都是日期对象，则只比较它们的时间值
            if (a.constructor === Date) {
                return a.getTime() === b.getTime();
            }

            // 如果它们都是正则表达式对象，则转换为字符串后比较
            if (a.constructor === RegExp) {
                return a.toString() === b.toString();
            }

            // 如果其中一个是NaN、Infinity或-Infinity，则使用isNaN进行比较
            if (isNaN(a) && isNaN(b)) return true;

            // 如果它们的类型不是对象，则直接比较它们是否相等
            if (typeof a !== 'object' || typeof b !== 'object') return a === b;

            // 递归比较对象的属性
            let i;
            for (i in a) {
                if (!(i in b) || !deepCompare(a[i], b[i])) {
                    return false;
                }
            }

            // 还需要检查b中是否有a不存在的属性
            for (i in b) {
                if (!(i in a)) {
                    return false;
                }
            }

            return true;
        }

        class Testdemo extends React.Component {
            state = {
                obj1: {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
            }
            handleClick = () => {
                const obj2 = {
                    a: 1,
                    b: {
                        c: 2
                    }
                }
                this.setState({
                    obj1: obj2
                })
            }
            shouldComponentUpdate = (nextProps, nextState) => {
                if (deepCompare(this.state.obj1 == nextState.obj1)) {
                    return false
                }
                else {
                    return true
                }
            }
            render() {
                console.log('render')
                return (
                    <button onClick={this.handleClick}>
                        点击{this.state.obj1.b.c}
                    </button>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>

</html>
```

# 四、Refs操作DOM及操作类组件

## 1）、`React.createRef()`

> 在类组件中，通过React.createRef()方法或直接在类组件的构造函数中通过this.myRef = React.createRef()来创建ref。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            myRef = React.createRef()
            handleClick = () => {
                console.log(this.myRef.current)
            }
            render() {
                return (
                    <div>
                        <button onClick={this.handleClick}>
                            点击
                        </button>
                        <div ref={this.myRef}>获取dom</div>
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 2、`callbackRef`

> 除了React.createRef()，React还支持使用`callbackRef`回调refs。这种方式在组件渲染或卸载时提供一个回调函数，允许你在任何时候访问DOM节点。这对于处理DOM节点可能尚未挂载或已经卸载的情况特别有用。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
        .box {
            background: orange;
        }
    </style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            callbackRef=(el)=>{
                console.log(el,'el')
                this.myRef=el;
            }
            handleClick = () => {
                this.myRef.className='box'
                console.log(this.myRef, 'this.myRef')
            }
            render() {
                return (
                    <div>
                        <button onClick={this.handleClick}>
                            点击
                        </button>
                        <div ref={this.callbackRef}>获取dom</div>
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
</html>
```

## 3、Refs操作类组件,得到组件实例对象

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Childdemo extends React.Component {
            name = "xiaomu"
            render() {
                return (
                    <div >子组件,{this.name}</div>
                )
            }
        }
        class Testdemo extends React.Component {
            myRef=React.createRef()
            handleClick=()=>{
                console.log(this.myRef.current,'myRef')
                console.log(this.myRef.current.name,'myRef')
            }
            render() {
                return (
                    <div>
                        <button onClick={this.handleClick}>点击</button>
                        <Childdemo ref={this.myRef}/>
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 4、Refs在类组件中进行转发

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Childdemo extends React.Component {
            name='xiaomu'
            render() {
                return (
                    <div ref={this.props.myRef}>子组件,{this.name}</div>
                )
            }
        }
        class Testdemo extends React.Component {
            myRef=React.createRef()
            handleClick=()=>{
                console.log(this.myRef.current,'myRef')
            }
            render() {
                return (
                    <div>
                        <button onClick={this.handleClick}>点击</button>
                        <Childdemo myRef={this.myRef}/>
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

# 五、表单与数据双向绑定

## 1、onChange和value

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                message:'hello world'
            }
            handleClick=(event)=>{
                console.log(event.target.value,'message')
                let message= event.target.value
                this.setState({
                    message
                })
            }
            render() {
                return (
                    <div>
                        <input
                            type="text" value={this.state.message}
                            onChange={this.handleClick}
                        />
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 2、defaultValue和changeInput

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                message:'hello world'
            }
            handleClick=(event)=>{
                console.log(event.target.value,'message')
                let message= event.target.value
                this.setState({
                    message
                })
            }
            render() {
                return (
                    <div>
                        <input 
                            type="text" defaultValue={this.state.message} 
                            onInput={this.handleClick}
                        />
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 3、form表单通过`onSubmit`提交数据

> `event.preventDefault()` 到 handleSubmit 函数中，以防止表单的默认提交行为（这通常会刷新页面）。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state={
                message:'hello world'
            }
            handleClick=(event)=>{
                let message= event.target.value
                this.setState({
                    message
                })
            }
            handleSubmit=(event)=>{
                 // 阻止表单的默认提交行为
                event.preventDefault();
                console.log(this.state.message, 'handleSubmit');
            }
            render() {
                return (
                    <form onSubmit={this.handleSubmit}>
                        <input
                            type="text" value={this.state.message}
                            onChange={this.handleClick}
                        />
                        <button type="submit">提交</button>
                    </form>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 4、form表单上传文件

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            state = {
                file: null
            }
            handleFileChange = (event) => {
                // 获取用户选择的文件
                let file = event.target.files[0]
                this.setState({
                    file
                })
            }
            uploadFile = (formData) => {
                // 使用fetch API发送文件到服务器,假设/api/upload为上传的一个接口  
                fetch('/api/upload', {
                    method: 'POST',
                    body: formData,
                })
                    .then(response => {
                        if (!response.ok) {
                            throw new Error('文件上传失败');
                        }
                        // 如果服务器返回JSON，可以解析它 
                        return response.json()
                    })
                    .then(data => {
                        // 处理上传成功后的响应数据  
                        console.log('上传成功', data);
                    })
                    .catch(error => {
                        // 处理上传失败的情况  
                        console.error('上传失败', error);
                    })
            }
            handleSubmit = (event) => {
                // 阻止表单的默认提交行为
                event.preventDefault();
                const formData = new FormData();
                formData.append('file', this.state.file);
                console.log(this.state.file, '文件已选择，准备上传');
                // 调用uploadFile函数上传文件  
                //   this.uploadFile(formData); 
            }
            render() {
                return (
                    <form onSubmit={this.handleSubmit}>
                        <input
                            type="file"
                            onChange={this.handleFileChange}
                        />
                        <button type="submit">提交</button>
                    </form>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

# 六、生命周期钩子函数

[生命周期钩子函数](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240508/1715101536756958247.png)

## 1、生命周期钩子函数
|钩子函数|触发时机|常见/不常见|功能|
|:--:|:--:|:--:|:--:|
|constructor|组件创建时|常见|初始化组件的状态（state）和绑定事件处理函数（如果有需要）|
|getDerivedStateFromProps|组件创建和更新时|常见（在React 16.3+中引入）|在组件创建或更新时，根据props更新state。这是一个静态方法，不能使用this。|
|render|每次组件重新渲染时|常见|返回React元素以描述组件的UI。|
|componentDidMount|组件挂载到DOM后|常见|组件首次渲染和挂载到DOM后执行。常用于发起网络请求、添加DOM事件监听器等。|
|shouldComponentUpdate|组件重新渲染前|常见|返回布尔值，用于控制组件是否应该重新渲染。默认返回true，但可以根据需要优化性能。|
|getSnapshotBeforeUpdate|更新DOM前|不常见（在React 16.3+中引入）|在DOM更新前获取一些信息（例如，滚动位置）。这个生命周期与componentDidUpdate一起使用，用于在DOM更新前后捕获一些值。|
|componentDidUpdate|DOM更新后|常见|组件更新并重新渲染到DOM后执行。常用于执行依赖于DOM的操作。|
|componentWillMount|组件挂载到DOM前（已废弃）|不常见|在React 16.3+中已废弃，不建议使用。|
|componentWillReceiveProps|组件接收到新的props时（已废弃）|不常见|在React 16.3+中已废弃，建议使用getDerivedStateFromProps替代。|
|componentWillUpdate|组件更新前（已废弃）|不常见|在React 16.3+中已废弃，不建议使用。|
|componentWillUnmount|组件卸载前|常见|组件即将从DOM中移除时执行。常用于清理操作，如取消网络请求、移除DOM事件监听器等。|


```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/react@18.3/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18.3/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
    <style>
        .wrapper {
            background: pink;
        }
    </style>
</head>

<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            constructor(props) {
                super(props);
                this.state = { count: 0 };
                console.log('constructor');
            }

            componentDidMount() {
                console.log('componentDidMount');
                // 可以在这里发起网络请求、添加DOM事件监听器等
            }

            shouldComponentUpdate(nextProps, nextState) {
                console.log('shouldComponentUpdate');
                // 根据需要返回true或false来控制组件是否重新渲染
                return true;
            }

            componentDidUpdate(prevProps, prevState) {
                console.log('componentDidUpdate');
                // 可以在这里执行依赖于DOM的操作
            }

            componentWillUnmount() {
                console.log('componentWillUnmount');
                // 在这里进行清理操作，如取消网络请求、移除DOM事件监听器等
            }

            handleClick = () => {
                this.setState({ count: this.state.count + 1 });
            };

            render() {
                console.log('render');
                return (
                    <div>
                        <p>Count: {this.state.count}</p>
                        <button onClick={this.handleClick}>点击</button>
                    </div>
                );
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo />
            </div>
        )
        root.render(element)
    </script>
</body>

</html>
```

# 七、组件中插入自定义内容的方式

> 使用 props 传递子组件（children）或者使用高阶组件（Higher-Order Components, HOCs）和渲染 props（render props）

## 1、类组件中使用 children prop 来模拟插槽

* 自定义组件中的内容

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            render() {
                return (
                    <div>
                        {this.props.children}
                    </div>
                );
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo >自定义组件中的内容</Testdemo>
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 直接采用父子通信的方式来传递组件中的内容

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class Testdemo extends React.Component {
            render() {
                return (
                    <div title={this.props.title}>
                        {this.props.header}
                    </div>
                );
            }
        }
        let element = (
            <div className="wrapper">
                <Testdemo title="组件中插入自定义内容" header={<h1>标题</h1>} />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 2、复用组件功能之Render props模式

> Render props模式是一种在React组件之间共享代码的简单技术，它可以将特定行为或功能封装成一个组件，并允许其他组件通过render prop来使用这个功能。在这种模式中，外部调用者向组件传入一个带返回值的函数`render prop`，组件的render方法直接调用这个函数，并可能将自己的state或props作为参数传递给这个函数。然后，这个函数的返回值将作为组件render方法的返回值，从而实现了组件的复用和定制化渲染。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        class ChildDemo extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                    x: 0,
                    y: 0
                };
            }
            componentDidMount = () => {
                document.addEventListener('mousemove', this.move)
            }
            componentWillUnmount = () => {
                document.removeEventListener('mousemove', this.move)
            }
            move = (event) => {
                this.setState({
                    x: event.pageX,
                    y: event.pageY
                })
            }
            render() {
                return (
                    <React.Fragment>
                        {this.props.render(this.state.x, this.state.y)}
                    </React.Fragment>
                );
            }
        }
        class TestDemo extends React.Component {
            render() {
                return (
                    <ChildDemo render={(x, y) =>
                            <div>
                               <div>x:{x}</div>
                               <div>y:{y}</div>
                            </div>
                    }
                    />
                );
            }
        }
        let element = (
            <div className="wrapper">
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 3、复用组件功能之HOC模式

> HOC接收一个组件并返回一个新的组件。这个新组件可以访问被包装组件的props，并可以通过额外的props或自身的state来渲染它。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        function withComponent(ChildDemo) {
            return class extends React.Component {
                state={
                    x:0,
                    y:0
                }
                componentDidMount(event) {
                    document.addEventListener('mousemove',this.move)
                }
                componentDidWillUnmount(event) {
                    document.removeEventListener('mousemove',this.move)
                }
                move=(event)=>{
                    this.setState({
                        x:event.pageX,
                        y:event.pageY
                    })
                }
                render() {
                    // 将所有props传递给被包装组件
                    return <ChildDemo {...this.state} />;
                }
            };
        }
        class TestDemo extends React.Component {
            render() {
                return (
                    <div>
                        <div>{this.props.x}</div>
                        <div>{this.props.y}</div>
                    </div>
                )
            }
        }

        // 使用HOC包装组件
        const NewComponent = withComponent(TestDemo);
        let element = (
            <div className="wrapper" content="hello wrold!">
                <NewComponent />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 4、组件跨层级通信Context

>在React中，跨层级组件通信的一种常见方式是使用Context API。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过每一个层级的 props 逐层传递。这对于跨越多层级的全局数据（如当前认证的用户、UI主题或首选语言）来说是非常有用的，而且Context 主要用于跨多个层级的组件共享数据，而不是在父子组件之间传递数据。

* 创建Context

> 首先，需要使用 React.createContext() 创建一个 Context 对象。这个对象包含两个React组件：`Provider` 和 `Consumer`。`defaultValue` 是当没有匹配的 Provider 时被使用的值。

* 使用Provider包裹组件树

> 在父组件树中最高层的地方，使用 MyContext.Provider 包裹你的组件。通过 value prop 传递给 Provider 的值将被传递给其下的所有 Consumer 组件。

* 使用Consumer获取值

> 在需要访问这个值的组件中，使用 MyContext.Consumer。Consumer 接收一个函数作为子元素，这个函数接收当前的 context 值作为参数，并返回一个React节点。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let MyContext = React.createContext(0)
        class ChildDemo extends React.Component {
            static contextType = MyContext
            componentDidMount = () => {
                console.log(this.context, 'this.context')
            }
            render() {
                // 直接使用 this.context 访问上下文的值
                return (
                    <React.Fragment>
                        {this.context}
                    </React.Fragment>
                );
            }
        }
        class TestDemo extends React.Component {
            state = {
                showProvider: true,// 控制是否显示 Provider，不使用使将会使用defaultValue
                count: 5
            }
            render() {
                return (
                    <div>
                        {this.state.showProvider ? (
                            <MyContext.Provider value={this.state.count}>
                                <ChildDemo />
                            </MyContext.Provider>
                        ) : (
                            <ChildDemo />
                        )}
                    </div>
                )
            }
        }
        let element = (
            <div className="wrapper">
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```
