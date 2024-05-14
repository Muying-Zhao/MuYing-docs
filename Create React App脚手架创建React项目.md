# 一、创建React应用

## 1、使用create-react-app创建React应用

```shell
npx create-react-app react-study
cd react-study
npm start
```
* `npx create-react-app react-study`

使用npx（Node Package Executor）来执行create-react-app的npm包，并创建一个名为react-study的新React应用。

* `cd react-study`

切换到新创建的react-study目录。

* npm start

启动开发服务器，并在浏览器中打开你的React应用。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240512/1715483277437806264.png)

## 2、VS Code插件

* ES7+ React/Redux/React-Native snippets

> 一个代码片段库，它提供了React、Redux和React Native的快速代码生成功能。

## 3、Chrome插件

* React Developer Tools

> 一个Chrome扩展，允许你在Chrome开发者工具中检查React组件的状态和props。

## 4、代码片段生成

> 来自使用的VS Code插件提供的代码片段快捷键。输入rcc（或rfc）后，按下Tab或Enter键可以生成React类组件（或函数组件）的骨架。

* 新建一个`src/components/TestDemo.jsx`文件，输入`rcc`快捷键

```js
import React, { Component } from 'react'
export default class TestDemo extends Component {
  render() {
    return (
      <div>TestDemo</div>
    )
  }
}
```

* 输入`rfc`快捷键

```js
import React from 'react'
export default function TestDemo() {
  return (
    <div>TestDemo</div>
  )
}
```

## 5、引入组件

* 在`App.js`或其他组件中，通过相对路径引入自定义组件，并使用<TestDemo/>来渲染它。

```js
import React from "react";
import TestDemo from "./components/TestDemo";
function App() {
  return (
    <React.Fragment>
      <div className="App">
        <div>hello world</div>
      </div>
      <TestDemo/>
    </React.Fragment>
  );
}
export default App;
```

## 6、`Fragment`

> `<React.Fragment></React.Fragment>`可以简写成`<></>`,它允许返回一个没有额外DOM节点的多个子元素。

```
import React from "react";
import TestDemo from "./components/TestDemo";
function App() {
  return (
    <>
      <div className="App">
        <div>hello world</div>
      </div>
      <TestDemo/>
    </>
  );
}
export default App;
```

## 7、React 17及更高版本中的import React from "react";

> 在React 17及更高版本中，JSX的转换已经内置了对React的自动引入支持。因此，在许多情况下，可以省略`import React from "react";`。但是，如果你使用JSX的某些特性（如JSX转换的`/** @jsx */`注释），或者你的构建工具/环境不支持此功能，则可能需要显式导入React。如`TestDemo.jsx`中省略React的自动引入

```jsx
export default function TestDemo() {
  return (
    <div>TestDemo</div>
  )
}
```

## 8、{/*  */}注释

> 在 JSX 中，{/* ... */} 是用于添加注释的语法。这种注释方式与 JavaScript 中的多行注释 /* ... */ 类似，但它在 JSX 中被特别处理以允许在 JSX 标签和表达式之间添加注释，而不会影响渲染的 DOM 结构。

```js
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div>TestDemo</div>
        {/* 这是一个注释，它不会出现在渲染的 DOM 中 */}
      </>
    )
  }
}
```


## 9、`<React.StrictMode>`严格模式

> `<React.StrictMode>`是React的一个高阶组件，用于在开发模式下检测应用中的潜在问题，比如未定义的组件或无效的DOM属性。它不会影响生产构建。

* 识别不安全的生命周期
* 关于使用过时字符串 ref API 的警告
* 关于使用废弃的 findDOMNode 方法的警告
* 检测意外的副作用
* 检测过时的 context API
* 确保可复用的 state

## 10、ESLint配置

> CRA默认包含了一个ESLint配置，可以在`package.json`中的`eslintConfig`部分修改或扩展它。在这个配置中，可以定义或覆盖ESLint的规则。如果要禁用某个规则，可以在"rules"添加配置规则。

其中`some-eslint-rule`是你要修改的ESLint规则的名称。`"off"`表示禁用该规则。其他可能的值包括`"warn"`（作为警告显示）和`"error"`（作为错误显示）。

```
"eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ],
    "rules": {
      "some-eslint-rule": "off"
     }
  },
```

# 二、常见的扩展和插件

## 1、react-app-rewired

> 当需要自定义(CRA)脚手架的webpack配置时，`react-app-rewired` 是一个很好的选择。由于CRA默认隐藏了webpack配置，使用`react-app-rewired`可以在不eject项目的情况下修改webpack配置。

## 2、ant-design

[Ant Design](https://ant.design/components/overview-cn)

> `ant-design`是一个流行的React UI库，提供了丰富的组件和样式。可以通过安装`antd`包并在项目中引入相应的CSS或less文件来使用它。

1. 安装

```shell
npm i antd
```

2. 使用

> 通过import语句来引入Ant Design的组件

```js
import React, { Component } from 'react'
import { Button } from 'antd';

export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div>TestDemo</div>
        <Button type="primary">Primary Button</Button>
      </>
    )
  }
}
```

3. 引入样式文件

* 引入样式文件(全局样式)

> ant-design版本5.0以上，不再需要手动引入样式文件，直接使用组件即可。但5.0版本以前需要引入

```js
import 'antd/dist/antd.css';
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240512/1715497877983713658.png)


* 或者按需引入样式

> 这个配置通常放在.babelrc、babel.config.js或项目的package.json文件的babel字段中。

```shell
npm install babel-plugin-import --save-dev
```

> 如果使用`babel.config.js`则配置

```js
module.exports = {
    plugins: [
        ["import", { libraryName: "antd", libraryDirectory: "es", style: "css" }],
    ],
};
```

> 如果使用`.babelrc`则配置

```json
{
  "plugins": [
    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
  ]
}
```

> 或者直接在package.json的babel字段中配置（但是不推荐）

```json
{
  "name": "your-project",
  "version": "1.0.0",
  "babel": {
    "plugins": [
      ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
    ]
  }
  // ... 其他字段 ...
}
```

4. 直接获取样式源代码，`App.js`中展示demo

```
import React from 'react';
import { TinyColor } from '@ctrl/tinycolor';
import { Button, ConfigProvider, Space } from 'antd';
const colors1 = ['#6253E1', '#04BEFE'];
const colors2 = ['#fc6076', '#ff9a44', '#ef9d43', '#e75516'];
const colors3 = ['#40e495', '#30dd8a', '#2bb673'];
const getHoverColors = (colors) =>
  colors.map((color) => new TinyColor(color).lighten(5).toString());
const getActiveColors = (colors) =>
  colors.map((color) => new TinyColor(color).darken(5).toString());
const App = () => (
  <Space>
    <ConfigProvider
      theme={{
        components: {
          Button: {
            colorPrimary: `linear-gradient(135deg, ${colors1.join(', ')})`,
            colorPrimaryHover: `linear-gradient(135deg, ${getHoverColors(colors1).join(', ')})`,
            colorPrimaryActive: `linear-gradient(135deg, ${getActiveColors(colors1).join(', ')})`,
            lineWidth: 0,
          },
        },
      }}
    >
      <Button type="primary" size="large">
        Gradient 1
      </Button>
    </ConfigProvider>
    <ConfigProvider
      theme={{
        components: {
          Button: {
            colorPrimary: `linear-gradient(90deg,  ${colors2.join(', ')})`,
            colorPrimaryHover: `linear-gradient(90deg, ${getHoverColors(colors2).join(', ')})`,
            colorPrimaryActive: `linear-gradient(90deg, ${getActiveColors(colors2).join(', ')})`,
            lineWidth: 0,
          },
        },
      }}
    >
      <Button type="primary" size="large">
        Gradient 2
      </Button>
    </ConfigProvider>
    <ConfigProvider
      theme={{
        components: {
          Button: {
            colorPrimary: `linear-gradient(116deg,  ${colors3.join(', ')})`,
            colorPrimaryHover: `linear-gradient(116deg, ${getHoverColors(colors3).join(', ')})`,
            colorPrimaryActive: `linear-gradient(116deg, ${getActiveColors(colors3).join(', ')})`,
            lineWidth: 0,
          },
        },
      }}
    >
      <Button type="primary" size="large">
        Gradient 3
      </Button>
    </ConfigProvider>
  </Space>
);
export default App;
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240512/1715496212691642268.png)

5. 使用小图标

[ant.design使用小图标链接](https://ant.design/components/icon-cn)

* 安装

```js
npm install @ant-design/icons --save
```

* `TestDemo.jsx`中使用小图标

```js
import React, { Component } from 'react'
import { UserOutlined } from '@ant-design/icons'
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div>TestDemo</div>
        <UserOutlined />
      </>
    )
  }
}
```

## 3、TypeScript

> 如果想使用TypeScript代替JavaScript编写React代码，可以通过添加`@typescript-eslint/parser`、`@typescript-eslint/eslint-plugin`、`typescript`等npm包来支持TypeScript。同时，可能还需要配置一些TypeScript相关的webpack规则。

## 4、redux/redux-toolkit

> `redux`是一个流行的状态管理库，而`redux-toolkit`是它的一个现代化工具集。可以通过安装这些npm包并在项目中设置redux store来使用它们。

## 5、react-router-dom

> `react-router-dom`是React的官方路由库，它允许在React应用中添加页面导航和路由功能。

## 6、eslint/prettier

> `eslint`和`prettier`是代码质量和格式化的工具。可以通过安装这些npm包并在项目中配置它们来确保代码的一致性和可读性。

## 7、dotenv

> `dotenv`是一个从`.env`文件中加载环境变量的库。这对于在开发、测试和生产环境中管理不同的配置非常有用。


## 8、react-icons

> `react-icons`是一个包含流行图标的React组件库。可以通过安装这个npm包并在项目中引入所需的图标来使用它。

## 9、styled-components/emotion

> 这些是CSS-in-JS库，允许在JavaScript中编写样式。它们提供了一种更灵活的方式来处理样式，特别是在根据组件的props或状态动态改变样式时。

# 三、样式处理方式

## 1.内联样式

> React允许你在JSX中使用内联样式，通过传递一个对象到style属性来实现。但是需要注意的是，CSS属性名需要驼峰化（camelCase），并且值需要是JavaScript能够理解的格式（如字符串、数字等）。

* TestDemo.jsx

```
import React, { Component } from 'react'
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div style={{ backGround: 'orange', width: 200, height :200}}>
          <div style={{ background: 'pink', width: 200, height: 100 }}>TestDemo</div>
        </div>
      </>
    )
  }
}

```

## 2、CSS类名

> 可以像在传统HTML中一样使用CSS类名。在React中，需要定义一个CSS文件并在你的组件中导入它。

* 新建`TestDemo.css`

```CSS
.wrapper {
    width: 200px;
    height: 200px;
    background: orange;
}
.box {
    width: 200px;
    height: 100px;
    background: pink;
}
```

* `TestDemo.jsx`引入css

```js
import React, { Component } from 'react'
import "./TestDemo.css";
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div className='wrapper'>
          <div className='box'>TestDemo</div>
        </div>
      </>
    )
  }
}
```

## 3、CSS Modules

> CSS Modules是一种流行的样式解决方案，它允许你使用类名作为局部作用域内的标识符。CSS Modules会自动为类名添加唯一的哈希，从而防止全局命名冲突。

* 新建`TestDemo.module.css`

```css
.wrapper {
    width: 200px;
    height: 200px;
    background: orange;
}
.box {
    width: 200px;
    height: 100px;
    background: pink;
}
```

* `TestDemo.jsx`引入CSS Modules

```js
import React, { Component } from 'react'
import style from "./TestDemo.module.css";

export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div className={style.wrapper}>
          <div className={style.box}>TestDemo</div>
        </div>
      </>
    )
  }
}
```

## 4、Styled Components

> Styled Components是一个流行的库，它允许你使用组件化的方式编写CSS。它允许你在JavaScript中编写CSS，并自动为类名添加唯一的哈希。

1. 安装

```shell
npm i styled-components
```

2. 使用`Styled Components`库

* `TestDemo.jsx

```js
import React, { Component } from 'react'
import styled from 'styled-components';
const StyledWrapper = styled.div`
  width: 200px;
  height: 200px;
  background: orange;
`;
const StyledBox = styled.div`
  width: 200px;
  height: 100px;
  background: pink;
`;
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <StyledWrapper>
          <StyledBox>TestDemo</StyledBox>
        </StyledWrapper>
      </>
    )
  }
}
```

## 5、`CSS-in-JS`

> CSS-in-JS是一种将CSS样式与JavaScript代码结合的方法，它允许开发人员在JavaScript组件中编写和管理CSS样式。这种方法在许多现代前端框架和库（如React、Vue和Angular）中变得越来越流行，因为它提供了一种组件化的方式来处理样式。

1. 安装

```shell
npm i styled-components
```

2. 使用`Styled Components`库

* `TestDemo.jsx

```js
import React, { Component } from 'react'
import styled from 'styled-components';
const StyledButton = styled.button`
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: ${(props) => props.primary ? 'rebeccapurple' : 'blue'};  // 直接在这里使用 props
  border: none;
  border-radius: 4px;
  cursor: pointer;
  &:hover {
    background-color: ${(props) => props.primary ? 'darkpurple' : 'darkblue'};  // hover 状态的动态样式
  }
`;
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div>
          <StyledButton primary>按钮一</StyledButton>
          <StyledButton>按钮二</StyledButton>
        </div>
      </>
    )
  }
}
```

## 6、less/sass/scss

> 当需要使用less、sass或scss作为样式预处理器，可以通过安装相应的npm包（如less、less-loader、node-sass、sass-loader等）并在webpack配置中做相应设置来使用它们。

1. 安装

```shell
npm i sass
```

2. 使用scss

* 新建`src/components/TestDemo.scss`

```css
.wrapper{
    width: 200px;
    height: 200px;
    background: orange;
    .box{
        width: 200px;
        height: 100px;
        background: pink;
    }
}
```

* `TestDemo.jsx`引入sass

```js
import React, { Component } from 'react'
import "./TestDemo.scss";
export default class TestDemo extends Component {
  render() {
    return (
      <>
        <div className='wrapper'>
          <div className='box'>TestDemo</div>
        </div>
      </>
    )
  }
}
```

## 7、操作样式模块

1. 安装

```shell
npm i classnames
```

2. 使用

* 修改`TestDemo.css`

```css
.wrapper {
    width: 200px;
    height: 200px;
    background: orange;
}
.box1 {
    width: 200px;
    height: 100px;
    background: pink;
}

.box2 {
    width: 200px;
    height: 100px;
    background: aqua;
}
```

* `TestDemo.jsx`使用操作样式模块

```js
import React, { Component } from 'react'
import "./TestDemo.css";
import Classnames from "classnames";
export default class TestDemo extends Component {
  TestDemoClass = Classnames({
    box1:false,
    box2:true
  })
  render() {
    return (
      <>
        <div className='wrapper'>
          <div className={this.TestDemoClass}>按钮一</div>
        </div>
      </>
    )
  }
}
```

# 四、react-transition-group模块实现动画

[react-transition-group](https://reactcommunity.org/react-transition-group/)

> `react-transition-group` 是一个用于在 React 应用程序中管理和处理过渡动画效果的库。它提供了一组组件，包括 `Transition`、`CSSTransition`、`SwitchTransition` 和 `TransitionGroup`，这些组件帮助在组件的进入和退出时应用动画效果。

## 1、`react-transition-group`中与过渡动画相关的类名及其作用

|类名|描述|
|:--:|:--:|
|enter|在`组件进入`时，这个类名会被添加到组件的 DOM 节点上。通常这个类名不会包含实际的动画效果，而是作为一个触发点，用于表示进入动画的开始。|
|enter-active|在`组件进入`时，并且 enter 类名被添加后，这个类名也会被添加到组件的 DOM 节点上。这个类名通常包含实际的进入动画效果，比如渐变、滑动等。|
|enter-done|当`进入动画结束后`，enter 和 enter-active 类名会被移除，同时这个 enter-done 类名会被添加到组件的 DOM 节点上。这个类名通常用于表示进入动画的结束状态，但通常不会包含实际的样式。|
|exit|在`组件退出`时，这个类名会被添加到组件的 DOM 节点上。与 enter 类名类似，它通常不会包含实际的动画效果，而是作为一个触发点，用于表示退出动画的开始。|
|exit-active|在`组件退出`时，并且 exit 类名被添加后，这个类名也会被添加到组件的 DOM 节点上。这个类名通常包含实际的退出动画效果，比如渐变、滑动等。|
|exit-done|当`退出动画结束后`，exit 和 exit-active 类名会被移除，同时这个 exit-done 类名会被添加到组件的 DOM 节点上。这个类名通常用于表示退出动画的结束状态，但通常不会包含实际的样式。|
|appear|当组件`首次渲染`到 DOM 上时，这个类名会被添加到组件的 DOM 节点上。它类似于 enter，但只在组件首次渲染时出现。|
|appear-active|当 appear 类名被添加后，这个类名也会被添加到组件的 DOM 节点上。它包含了组件`首次渲染`时的动画效果。|
|appear-done|当`首次渲染`的动画结束后，appear 和 appear-active 类名会被移除，同时这个 appear-done 类名会被添加到组件的 DOM 节点上。它表示首次渲染动画的结束状态。|

> appear 相关的类名并不会在组件的后续进入和退出动画中触发，它们仅用于处理组件的首次渲染动画。

## 2、使用过渡动画

1. 安装

```shell
npm install react-transition-group --save
```

2. 使用`react-transition-group`

* TransitionGroup.scss

```css
.wrapper {
    .box {
        width: 150px;
        height: 150px;
        background: red;
        opacity: 1;
    }

    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        opacity: 1;
        transition: 1s;
    }

    .fade-enter-done {
        opacity: 1;
    }

    .fade-exit {
        opacity: 1;
    }

    .fade-exit-active {
        opacity: 0;
        transition: 1s;
    }

    .fade-exit-done {
        opacity: 0;
    }

    .fade-appear {
        opacity: 0;
    }

    .fade-appear-active {
        opacity: 1;
        transition: 1s;
    }

    .fade-appear-done {
        opacity: 1;
    }
}
```

* TransitionGroup.js

```js
import React, { useState, useRef } from 'react'
import './TransitionGroup.scss'
import { CSSTransition } from 'react-transition-group'

export default function App() {
    const [prop, setProp] = useState(true)
    const nodeRef = useRef(null)
    const handleClick = () => {
        setProp(!prop)
    }
    const handleEntered = () => {
        console.log('entered')
    }
    return (
        <div className="wrapper">
            <button onClick={handleClick}>点击</button>
            <CSSTransition appear nodeRef={nodeRef} in={prop} timeout={1000} classNames="fade" unmountOnExit onEntered={handleEntered}>
                <div className="box" ref={nodeRef}></div>
            </CSSTransition>
        </div>
    )
}
```

## 3、警告问题解决

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240513/1715532100706697384.png)

> 当看到 findDOMNode 的警告时，意味着将ref 直接附加到想要引用的元素上，而不是依赖 findDOMNode。在 React 中，可以使用 useRef Hook（在函数组件中）或类组件的实例变量（在类组件中）来创建和管理 ref。

详细解释官网链接:
[findDOMNode](https://react.dev/reference/react-dom/findDOMNode#alternatives)

```
const nodeRef = useRef(null)
nodeRef={nodeRef}
ref={nodeRef}
```
