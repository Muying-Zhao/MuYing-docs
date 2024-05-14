# 一、函数组件基本使用

> React 的函数组件是 React 组件的一种形式，它们是无状态的（stateless），并且只接收 props 作为输入，然后返回 JSX。从 React 16.8 开始，引入了 Hooks API，这使得函数组件也能具有类似于类组件的 state 和其他功能。

## 1、基本使用

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let TestDemo=()=>{
            return (
                <div>hello world</div>
            )
        }
        let element = (
            <TestDemo />
        )
        root.render(element)
    </script>
</body>
```

## 2、传递参数(props)

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let TestDemo = (props) => {
            return (
                <div>{props.count}</div>
            )
        }
        TestDemo.defaultProps = {
            count: 0
        }
        let element = (
            <TestDemo count={5} />
        )
        root.render(element)
    </script>
</body>
```

* defaultProps

> 在函数组件中，不能直接在组件函数上定义 defaultProps，但可以使用 ES6 模块的默认参数或者在组件外部定义一个对象来模拟 defaultProps 的功能。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let TestDemo=(props)=>{
            return (
                <div>{props.count}</div>
            )
        }
        TestDemo.defaultProps={
            count: 0
        }
        let element = (
            <TestDemo count={5}/>
        )
        root.render(element)
    </script>
</body>
```

## 3、定义类型检查(propTypes)

> 也可以使用 PropTypes 库（来自 prop-types 包）来为函数组件的 props 定义类型检查。

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
        let TestDemo=(props)=>{
            return (
                <div>{props.count}</div>
            )
        }
        TestDemo.propTypes={
            count: PropTypes.number
        }
        let element = (
            <TestDemo count={5}/>
        )
        root.render(element)
    </script>
</body>
</html>
```

## 4、事件处理

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let TestDemo=(props)=>{
            const handleClick=(event)=>{
                console.log(event)
            }
            return (
                <div>
                    <div>{props.count}</div>
                    <button onClick={handleClick}>点击</button>
                </div>
            )
        }
        let element = (
            <TestDemo count={0}/>
        )
        root.render(element)
    </script>
</body>
```

## 5、组件嵌套在对象中

> 无论是函数组件还是类组件，都可以实现嵌套函数组件和类组件

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        const TestDemo = {
            // 类组件
            Child1Demo: class extends React.Component {
                render() {
                    return (
                        <div>0</div>
                    )
                }
            },
            // 函数组件
            Child2Demo: () => {
                return (
                    <button>点击</button>
                )
            }
        }
        let element = (
            <React.Fragment>
                <TestDemo.Child1Demo />
                <TestDemo.Child2Demo />
            </React.Fragment>
        )
        root.render(element)
    </script>
</body>
```

## 6、`<React.Fragment>`和`<Context.Provider>`

> 解决特定问题的组件

* `<React.Fragment>`

> `<React.Fragment>`用于在返回多个元素的组件中，避免创建一个额外的 DOM 元素包裹它们。相当于Vue中的`template`

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let TestDemo=(props)=>{
            return (
                 <React.Fragment>
                    <div>{props.count}</div>
                 </React.Fragment>
            )
        }
        let element = (
            <TestDemo count={0}/>
        )
        root.render(element)
    </script>
</body>
```

* `<Context.Provider>`

> `<Context.Provider>` 是 React 的 Context API 的一部分，它允许在组件树中不必显式地逐层传递 props，就可以让深层的组件访问到某个值。Context 提供了一种在组件之间共享值的方式，而不必显式地通过每一层的 props 传递。如下是一个简单加法示例，后面会详细解释React函数

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState,useContext, createContext } = React
        const defaultValue = { count: 0, setValue: () => { } };
        const MyContext = createContext(defaultValue);
        const TestDemo = () => {
            const [count, setCount] = useState(0);
            return (
                <MyContext.Provider value={{ count, setCount }}>
                    <ChildDemo />
                </MyContext.Provider>
            )
        }
        const ChildDemo=()=>{
            const { count, setCount } = useContext(MyContext);
            return (
                <div>
                    <p>{count}</p>
                    <button onClick={() => setCount(count +1)}>按钮</button>
                </div>
            );
        }
        let element = (
            <div>
                <TestDemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

# 二、Hook概念

> 在React中，Hook是React 16.8版本新增的一个特性，它允许在不编写class的情况下使用state（状态）以及其他的React特性。Hook的主要目的是用来实现所有class组件的功能，并解决了class组件中状态逻辑复用的问题。在函数组件中，Hook允许使用“钩入”React state（状态）及生命周期等特性。

|Hook 名称|常用/不常用|描述|
|:--:|:--:|:--:|
|useState|常用 |允许在函数组件中添加状态，并返回状态值和更新状态的函数。|
|useEffect|常用|在组件渲染后执行副作用，如数据获取或手动DOM更改|
|useContext|常用|无需显式传递props，即可使用React的Context。|
|useReducer|常用|替代useState，用于管理更复杂的本地状态逻辑。|
|useCallback|常用|返回一个记忆化的回调函数，避免不必要的渲染或重新计算。|
|useMemo|常用|返回一个记忆化的值，在依赖项不变时避免不必要的计算和重渲染。|
|useRef|不常用|返回一个可变的ref对象，可用于访问DOM元素或保存可变值。|
|useLayoutEffect|不常用|与useEffect类似，但在DOM更新后立即同步触发。|
|useImperativeHandle|不常用|允许父组件通过ref获取子组件的实例值。|
|useDebugValue|不常用|在React开发者工具中显示自定义Hook的标签。|
|useDeferredValue|实验性 Hooks|提供一个可以“延迟”的值，直到屏幕更新。|
|useTransition|实验性 Hooks|将状态更新标记为“过渡”，延迟执行以保持UI流畅性|


## 1、useState函数

### 1)、在函数组件中使用useState函数

> 允许在函数组件中添加状态，并返回状态值和更新状态的函数。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState } = React
        const TestDemo = () => {
            const [count, setCount] = useState(0)
            const handleClick=()=>{
                setCount(count+1)
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 只能在最顶层使用Hook

>  React Hooks 必须始终在 React 函数组件的顶层调用。这意味着你不能在循环、条件语句或嵌套函数中调用 Hooks。这个规则的原因在于 React 需要跟踪每个组件的 Hook 调用顺序，这样它才能在后续的渲染中以相同的顺序调用它们。如果你尝试在条件语句或循环中调用 Hooks，这可能会打破这种顺序，导致不一致和难以追踪的错误。

```html
let { useState } = React
```

* 只能在函数组件中使用Hook

> React Hooks 是为函数组件设计的，并且只能在函数组件内部使用。不能在类组件中使用 Hooks，因为类组件已经提供了类似的功能（例如生命周期方法和状态），并且它们的内部机制与函数组件不同。 如果尝试在类组件中使用 Hooks，将会得到一个错误。同时，也不能在普通的 JavaScript 函数或回调函数中调用 Hooks，因为 React 需要知道 Hooks 是在哪个组件中调用的。

```html
const TestDemo = () => {
            const [count, setCount] = useState(0)
            return (
                < div >
                    <div>{count}</div>
                </div >
            )
        }
```

### 2)、函数组件依然具有自动批处理能力（异步操作）

* 自动批处理能力

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState } = React
        const TestDemo = () => {
            const [count, setCount] = useState(0)
            const [text, setText] = useState('hello world')
            const handleClick=()=>{
                setCount(count+1)
                setText('world hello')
            }
            return (
                < div >
                    <div>{count}</div>
                    <div>{text}</div>
                    <button onClick={handleClick}>点击</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* `flushSync`阻止自动批处理能力

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState } = React;
        let {flushSync} = ReactDOM;
        const TestDemo = () => {
            const [count, setCount] = useState(0)
            const [text, setText] = useState('hello world')
            const handleClick=()=>{
                flushSync(()=>{
                    setCount(count+1)
                    console.log('flushSync')
                })
                flushSync(()=>{
                    setText('world hello')
                    console.log('flushSync')
                })
            }
            return (
                < div >
                    <div>{count}</div>
                    <div>{text}</div>
                    <button onClick={handleClick}>点击</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 接受回调函数，只调用一次

> `setCount((count) => count + 1)`与`setCount(count + 1)`不相同

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState } = React;
        const TestDemo = () => {
            const [count, setCount] = useState(0)
            const handleClick = () => {
                // 只会执行一次
                setCount(count + 1)
                setCount(count + 1)
                setCount(count + 1)
            }
            const handleCallClick = () => {
                //回调方法，执行三次
                setCount(count=>count + 1)
                setCount(count=>count + 1)
                setCount(count=>count + 1)
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>handleClick</button>|
                    <button onClick={handleCallClick}>handleCallClick</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 3)、在useState()添加回调函数

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState } = React;
        let TestDemo = () => {
            const [info, setInfo] = useState({
                username: 'xiaomu',
                age: 20
            })
            return (
                < div >
                    <div>{info.username}{info.age}</div>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 状态更新时机

> 在TestDemo组件中，setInfo的调用是在渲染过程中（即在函数体内，而非在事件处理函数或副作用中）直接进行的。这会导致在React的渲染过程中更新状态，从而可能引发无限循环的渲染,所以会产生报错

```html
// error示例
setInfo({
      ...info,
      age: 21
});
```

> 使用useEffect来模拟组件加载后的状态更新

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const [info, setInfo] = useState({
                username: 'xiaomu',
                age: 20
            })
            useEffect(() => {
                setInfo({
                    ...info,
                    age: 21
                });
            }, []);
            return (
                < div >
                    <div>{info.username}{info.age}</div>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 4)、惰性初始state

> 在React中，惰性初始化state通常指的是在组件的初始渲染时执行一些可能比较昂贵的计算或初始化操作，并在后续的渲染中跳过这些操作。这可以通过在useState的初始化函数中传入一个函数来实现。下例代码会重复初始渲染initCount方法

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const initCount=()=>{
                console.log('initCount')
                return 0
            }
            const [count, setCount] = useState(initCount())
            const handleClick=()=>{
                setCount(count+1)
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* useState的初始化函数回调

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const initCount=()=>{
                console.log('initCount')
                return 0
            }
            const [count, setCount] = useState(()=>{
                return initCount()
            })
            const handleClick=()=>{
                setCount(count+1)
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 2、useEffect函数

> useEffect 是 React 中的一个 Hook，它允许函数组件中执行副作用操作。副作用操作包括数据获取、订阅或手动更改 DOM 等。同时可以接受两个参数：一个用于执行副作用的函数，以及一个可选的数组，用于指定依赖项，当这些依赖项变化时，副作用函数会重新执行。

### 1)、在组件渲染后在和在组件更新前执行代码

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            useEffect(() => {
                console.log(count,'count')
            });
            const handleClick=()=>{
                setCount(count+1);
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击切换</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 提供依赖项空数组，只在组件首次渲染后触发

> 只在组件首次渲染后执行，这是因为没有为 useEffect 提供依赖项数组（第二个参数）。当未提供依赖项数组时，useEffect 会在组件挂载（即首次渲染）后运行，并且只会在那个时候运行一次。下例代码使用的组件加载后的状态更新，这里只能进行首次渲染，如果监听info会造成死循环

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const [info, setInfo] = useState({
                username: 'xiaomu',
                age: 20
            })
            useEffect(() => {
                setInfo({
                    ...info,
                    age: 21
                });
                console.log('info')
            }, []);//这里传递了一个空数组作为依赖项，确保只在挂载和卸载时运行
            const handleClick=()=>{
                setInfo({
                    username:"xiaobu",
                    age: 22
                });
            }
            return (
                < div >
                    <div>{info.username}{info.age}</div>
                    <button onClick={handleClick}>点击切换</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 提供依赖项数组，在组件监听`count`更新前也会执行

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            useEffect(() => {
                console.log(count,'count')
            }, [count]);
            const handleClick=()=>{
                setCount(count+1);
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击切换</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

### 2)、在组件卸载前执行清理操作触发

>  在实际应用中，直接通过引用根`root.unmount()`渲染实例来卸载组件通常不是一个好的做法，因为这可能导致难以追踪和维护的代码。相反，使用 React 的路由系统（如 React Router）来控制组件的挂载和卸载，或者通过条件渲染来控制组件的显示和隐藏。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState, useEffect } = React;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            useEffect(() => {
                return ()=>{
                    console.log('unmount')
                }
            });
            const handleClick=()=>{
              root.unmount()
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击切换</button>
                </div >
            )
        }
        let element = (
            <div>
                <TestDemo />
            </div>
        )
        root.render(element)
    </script>
</body>
```

* 通过条件渲染来控制组件的显示和隐藏触发

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useEffect } = React;
        let { createRoot} = ReactDOM;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            useEffect(() => {
                console.log('TestDemo mount')
                return () => {
                    console.log('TestDemo unmount')
                }
            }, []);
            const handleClick = () => {
                setCount(count+1)
            }
            return (
                < div >
                    <div>{count}</div>
                    <button onClick={handleClick}>点击切换</button>
                </div >
            )
        }
        let App = () => {
                const [showTestDemo, setShowTestDemo] = useState(true);
                const toggleTestDemo = () => {
                    setShowTestDemo(!showTestDemo);
                };

                return (
                    <div>
                        {showTestDemo && <TestDemo />}
                        <button onClick={toggleTestDemo}>切换 TestDemo 显示/隐藏</button>
                    </div>
                );
            };
         const root = createRoot(document.querySelector('#app'));
            root.render(<App />);
    </script>
</body>
```

## 3、useLayoutEffect函数

> 与useEffect类似，但在DOM更新后立即`同步`触发。
大部分情况下我们采用useEffect()，性能更好。但是当useEffect里面的操作需要处理DOM，并且会改变页面的样式，就需要用useLayoutEffect，否则可能会出现闪屏问题


## 4 、useEffect函数

> 返回一个可变的ref对象，可用于访问DOM元素或保存可变值。

### 1)、通过ref获取dom

> 在 React 中，ref 是一个特殊的属性，它允许访问 DOM 节点或 React 元素实例。其中ref 是 React 框架自带的功能，用于获取对 DOM 节点的直接引用。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState} = React;
        let { createRoot } = ReactDOM;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            const testDOM =(event)=>{
                console.log(event,'event')
            }
            return (
                < div ref={testDOM}>
                    <div>{count}</div>
                </div >
            )
        }
        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

* 通过useEffect函数获取dom

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useRef } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = () => {
            const [count, setCount] = useState(0)
            const testRef= useRef()
            const handleClick=()=>{
                console.log(testRef.current,'myRef')
            }
            return (
                < div ref={testRef}>
                    <div>{count}</div>
                    <button onClick={handleClick}>按钮</button>
                </div >
            )
        }
        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 2)、forwardRef接收 ref转发到其子组件

> 使用React.forwardRef 来创建一个可以接收 ref 的函数组件。在 React 中，默认情况下函数组件不接收 ref 属性，因为函数组件没有实例。但是，通过 React.forwardRef，您可以创建一个函数组件，它能够接收一个 ref 并将其转发到其子组件（通常是一个 DOM 元素或类组件）。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useRef } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = React.forwardRef((props,ref) => {
            const [count, setCount] = useState(0)
            return (
                < div ref={ref}>
                    <div>{count}</div>
                </div >
            )
        })
        let App = () => {
            const testRef=useRef()
            const handleClick=()=>{
                console.log(testRef.current,'testRef')
            }
            return (
                <div>
                    <TestDemo ref={testRef}/>
                    <button onClick={handleClick}>按钮</button>
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 3)、useRef 用于存储组件的可变值,不会触发组件重新渲染的值

> 使用useRef钩子并初始化为一个值（比如let count = useRef(0)），实际上是创建了一个可变的ref对象，其`.current`属性可以被读取或更新。与Vue 3中的ref函数非常相似.但是通常 useRef 用于存储不会触发组件重新渲染的值，比如 DOM 元素的引用。由于 useRef 的值变化不会触发组件的重新渲染，下例打印count值会增加，但页面count还是0

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useRef } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = React.forwardRef((props,ref) => {
            let count=useRef(0)
            const handleClick=()=>{
                count.current++
                console.log(count.current,'count')
            }
            return (
                < div>
                    <div>{count.current}</div>
                    <button onClick={handleClick}>按钮</button>
                </div >
            )
        })
        let App = () => {
            return (
                <div>
                    <TestDemo/>
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

* 通过条件判断来控制首次渲染不会打印count

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useRef,useState,useEffect } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = React.forwardRef((props,ref) => {
            const [count,setCount]= useState(0)
            let isShow=useRef(false)
            useEffect(()=>{
                if(isShow.current){
                    console.log(count,'count')
                }
            })
            const handleClick=()=>{
                setCount(count + 1)
                isShow.current=true
            }
            return (
                < div>
                     <div>{count}</div>
                    <button onClick={handleClick}>按钮</button>
                </div >
            )
        })
        let App = () => {
            return (
                <div>
                    <TestDemo/>
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

## 5 、useContext函数

> useContext 是 React 的一个 Hook，它允许在组件树中不必显式地逐层传递 props，就能将值深入组件的传递。它接收一个 React Context 对象（由 React.createContext() 创建），并返回该 Context 的当前值。

* `<Context.Provider>`

> `<Context.Provider>` 是 React 的 Context API 的一部分，它允许在组件树中不必显式地逐层传递 props，就可以让深层的组件访问到某个值。Context 提供了一种在组件之间共享值的方式，而不必显式地通过每一层的 props 传递。如下是一个简单加法示例，后面会详细解释React函数

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let root = ReactDOM.createRoot(app);
        let { useState,useContext, createContext } = React
        const defaultValue = { count: 0, setValue: () => { } };
        const MyContext = createContext(defaultValue);
        const TestDemo = () => {
            const [count, setCount] = useState(0);
            return (
                <MyContext.Provider value={{ count, setCount }}>
                    <ChildDemo />
                </MyContext.Provider>
            )
        }
        const ChildDemo=()=>{
            const { count, setCount } = useContext(MyContext);
            return (
                <div>
                    <p>{count}</p>
                    <button onClick={() => setCount(count +1)}>按钮</button>
                </div>
            );
        }
        let element = (
            <div>
                <TestDemo/>
            </div>
        )
        root.render(element)
    </script>
</body>
```

## 6、React.memo和useMemo

### 1)、React.memo用于优化组件的渲染性能

> React.memo是一个高阶组件，用于包装不想重新渲染的组件。当组件的props没有变化时，它不会触发不必要的渲染。React.memo默认使用浅比较来对比props，但也可以通过传入第二个参数来自定义比较方法。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let {  useState,memo } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = (props) => {
            return (
                <div>{props.count}</div>
            )
        }
        const MemoTestDemo = memo(TestDemo);
        let App = () => {
            const [count, setCount] = useState(0);
            const handleClick=()=>{
                setCount(count+1)
            }
            // 如果setCount(count+1)写成setCount(0)，它不会触发重新渲染
            console.log('memo')
            return (
                <div>
                    <MemoTestDemo count={count} />
                    <button onClick={handleClick}>按钮</button>
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 2)、useMemo函数用于优化组件中昂贵的计算操作。

> 返回一个记忆化的值，在依赖项不变时避免不必要的计算和重渲染。它接收两个参数：一个回调函数（通常是一个计算密集型的函数）和一个依赖项数组。当依赖项数组中的值发生变化时，useMemo 会重新计算回调函数的结果；如果依赖项数组中的值没有变化，useMemo 会返回上一次的计算结果，避免不必要的重复计算。
> 计算斐波那契数列中的某个数，并使用 useMemo 来缓存计算结果，以避免在每次渲染时都重新计算

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let {  useState, useMemo } = React;
        let { createRoot } = ReactDOM;
        // 斐波那契数列计算函数
        function fibonacci(n) {
            if (n <= 1) return n;
            return fibonacci(n - 1) + fibonacci(n - 2);
        }
        let TestDemo = React.forwardRef((props, ref) => {
            const [count, setCount] = useState(5);
            // 使用 useMemo 缓存 fibonacci 的计算结果
            const result = useMemo(() => {
                console.log('斐波那契数列计算函数 ');
                return fibonacci(count);
            }, [count]);
            // 处理 count 的变化
            const handleChange = (event) => {
                setCount(Number(event.target.value));
            };
            return (
                < div>
                    <input type="number" value={count} onChange={handleChange} />
                    <p>斐波那契数列计算count：{count}</p>
                    <p>
                        result：{result}
                    </p>
                </div >
            )
        })
        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 3)、useMemo 和 React.memo 的主要区别和相似之处

1. 相似之处

* 优化性能：两者都可以用来优化React组件的性能，避免不必要的计算和渲染

* 减少不必要的渲染：useMemo 可以避免在组件重新渲染时重复执行昂贵的计算；而 React.memo 可以避免在props没有变化时重新渲染整个组件。

2. 不同之处

> 应用层面

* useMemo通常与 useEffect 一起使用，来避免在每次渲染时都重新执行昂贵的计算。当组件渲染时需要进行一些复杂的计算或者是对一些昂贵的操作进行优化，例如进行大量数据的过滤、排序、格式化等。

* React.memo 是一个高阶组件，用于包装函数组件。它接收一个函数组件作为参数，并返回一个新的组件，该组件具有记忆化（memoization）的能力。它关注的是组件级别的优化，通过比较props来决定是否跳过渲染。

> 使用方式：

* useMemo 接收一个回调函数和一个依赖项数组作为参数。当依赖项数组中的值发生变化时，useMemo 会重新计算回调函数的结果。

* React.memo 接收一个函数组件作为参数，并返回一个记忆化的组件。这个记忆化的组件在props没有变化时不会重新渲染。

> 关注点：

* useMemo 关注的是单个计算或值的缓存，以避免在组件重新渲染时重复计算。

* React.memo 关注的是整个组件的渲染优化，通过记忆化整个组件来避免不必要的渲染。

> 副作用：

* useMemo 并不保证回调函数的执行顺序，因此不应该在 useMemo 的回调函数中执行具有副作用的操作（如 API 调用或修改 DOM）。如果需要执行副作用，应该使用 useEffect Hook。

* React.memo 本身不执行任何副作用，它只是通过记忆化组件来避免不必要的渲染。

## 7、useCallback函数

> 返回一个记忆化的回调函数，避免不必要的渲染或重新计算。它在父组件重新渲染时，如果传递给子组件的回调函数作为 props 没有变化（即引用相同），则避免不必要的子组件重新渲染。这在性能优化和避免不必要的副作用时特别有用。

useCallback类似于useEffect和useMemo 的依赖项数组。当这个数组中的任何一项发生变化时，它返回一个新的回调函数。虽然可以帮助你避免不必要的子组件重新渲染，但过度使用它可能会导致代码变得复杂且难以维护。在大多数情况下，React 的默认行为（即重新渲染整个组件）已经足够高效。只有在确实需要进行性能优化或避免不必要的副作用时，才应该考虑使用 useCallback。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useCallback } = React;
        let { createRoot } = ReactDOM;
        let TestDemo = ({ onClick }) => {
            // 当 onClick 回调函数的引用发生变化时，TestDemo会重新渲染
            // 但由于我们使用了 useCallback，所以只要父组件的其他 props 没有变化，
            // TestDemo就不会因为父组件的重新渲染而重新渲染
            console.log('TestDemo rendered');
            return (
                <div>
                    <button onClick={onClick}>按钮</button>
                </div>
            )
        }
        let App = () => {
            const [count, setCount] = useState(0);
            const handleClick = useCallback(() => {
                setCount(count + 1);
            }, [count]);
            return (
                <div>
                    <div>{count}</div>
                    <TestDemo onClick={handleClick} />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

## 8、useReducer函数

> 替代useState，用于管理更复杂的本地状态逻辑。它接收一个 reducer 函数和一个初始状态作为参数，并返回一个包含当前状态和一个用于触发状态更新的 dispatch 函数的数组。

1. 定义初始状态：确定你的组件需要的初始状态。

2. 定义 reducer 函数：编写一个 reducer 函数，该函数接收当前状态和动作对象，并返回新的状态。在 reducer 函数中，你可以根据动作类型来决定如何更新状态。

3. 使用 useReducer 初始化状态并获取 dispatch 函数：在组件的函数体中，使用 useReducer Hook 并传入你定义的 reducer 函数和初始状态。useReducer 将返回一个数组，其中第一个元素是当前状态，第二个元素是用于触发状态更新的 dispatch 函数。

4. 通过 dispatch 函数发送动作来触发状态更新：在你的组件中，当需要更新状态时，可以调用 dispatch 函数并传入一个动作对象。这个动作对象应该包含一个 type 属性（用于指定要执行的动作类型），以及其他可能需要的属性（用于传递额外的信息给 reducer 函数）。

> 模拟单个商品购物count

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useReducer } = React;
        let { createRoot } = ReactDOM;

        // 定义初始状态
        const initialState = { count: 0 };

        // 定义 reducer 函数
        function reducer(state, action) {
            switch (action.type) {
                case 'plus':
                    return { count: state.count + 1 };
                case 'minus':
                    return { count: state.count - 1 };
                default:
                    throw new Error();
            }
        }

        let TestDemo = () => {
            // 使用 useReducer 初始化状态并获取 dispatch 函数
            const [state, dispatch] = useReducer(reducer, initialState);
            return (
                <div>
                    <p>Count: {state.count}</p>
                    <button onClick={() => dispatch({ type: 'plus' })}>+</button>
                    <button onClick={() => dispatch({ type: 'minus' })}>-</button>
                </div>
            )
        }
        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

> 模拟登录登出状态

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useReducer } = React;
        let { createRoot } = ReactDOM;

        // 定义初始状态
        const initialState = {
            isLogin: false,
            userInfo: null,
        };

        // 定义 reducer 函数
        function authReducer(state, action) {
            switch (action.type) {
                case 'LOGIN':
                    return {
                        isLogin: true,
                        userInfo: action.payload.userInfo,
                    };
                case 'LOGOUT':
                    return {
                        isLogin: false,
                        userInfo: null,
                    };
                default:
                    throw new Error();
            }
        }

        let TestDemo = () => {
            // 使用 useReducer 初始化状态并获取 dispatch 函数
            const [authState, dispatch] = useReducer(authReducer, initialState);
            // 登录函数
            const handleLogin = () => {
                // 这里可以进行发送请求到服务器进行验证，然后调用 dispatch 更新状态
                dispatch({ type: 'LOGIN', payload: { userInfo: { username: '炑焽' } } });
            };

            // 退出函数
            const handleLogout = () => {
                dispatch({ type: 'LOGOUT' });
            };
            // 渲染内容
            return (
                <div>
                    {authState.isLogin ? (
                        <div>
                            <p>用户名: {authState.userInfo.username}</p>
                            <button onClick={handleLogout}>退出登录</button>
                        </div>
                    ) : (
                        <div>
                            <p>请登录</p>
                            <button onClick={handleLogin}>登录</button>
                        </div>
                    )}
                </div>
            );
        }
        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

## 8、useTransition与useDeferredValue函数（实验性 Hooks）

### 1)、startTransition

> startTransition函数可以用来告诉React，对即将进行一次状态进行变化，并且希望在这个过程中进行一些优化。这样，用户就可以清晰地感知到组件状态的变化，并且可以更好地理解正在发生的事情。

* 使用方式

> 用于包裹那些你希望标记为“过渡”状态的状态更新。使用 startTransition 包裹的状态更新会被视为较低优先级的更新，这有助于React在处理高优先级的更新（如用户输入）之后，再处理这些更新。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, memo } = React;
        let { createRoot } = ReactDOM;

        let TestDemo = memo(({query}) => {
            const text ='hello world'
            const items=[]
            if(query!==''&&text.includes(query)){
                // 将字符串分割为子字符串数组，返回一个新数组。这个方法接收一个或多个参数，作为分隔符，用于确定在哪里分割字符串。
                //如果你的 query 是一个空字符串 ""，那么 split() 方法会将字符串中的每个字符都分割成一个独立的数组元素：
                const arr=text.split(query);
                //  console.log(arr,'arr') //如query='hello' 输出: ["", "world"]
                for(let i=0;i<9;i++){
                    // 注意：这里我们假设arr.length >= 2，但在实际应用中你可能需要添加额外的检查
                    // 如果arr只有一个元素，则不显示arr[1]
                    items.push(
                        <li key={i}>
                            {arr[0]}
                            <span style={{ color: 'red' }}>{query}</span>
                            {arr.length > 1 ? arr[1] : ''}
                        </li>
                    );
                }
            }else{
                for (let i = 0; i < 9; i++) {
                    items.push(<li key={i}>{text}</li>);
                }
            }
            return (
                <ul>
                    {items}
                </ul>
            );
        })

        let App = () => {
            const [searchWord,setSearchWord]=useState('')
            const [query,setQuery]= useState('')
            const handleChange=(ev)=>{
                setSearchWord(ev.target.value)
                // 使用 startTransition 标记数据加载为非紧急更新
                startTransition(()=>{
                    setQuery(ev.target.value)
                })
            }
            return (
                <div>
                    <input type="text" value={searchWord} onChange={handleChange}/>
                    <TestDemo query={query} />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 2)、useTransition函数

> useTransition 是 React 18 引入的一个新的 Hook，它允许你指定哪些状态更新是“紧急的”（即应该立即同步渲染的）和哪些更新是“过渡的”（即可以延迟到下一帧渲染的）。这对于处理非紧急的更新（如数据获取或计算）特别有用，因为它们不会阻塞用户界面对用户输入的即时响应。

* 使用方式

> 需要在组件的顶层调用这个Hook，并接收它返回的状态和函数。当数据足够多时会停留loading加载一会儿，少的时候会显示一闪即逝

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, useTransition } = React;
        let { createRoot } = ReactDOM;
        function fetchSearchResults(term) {
            return new Promise((resolve) => {
                setTimeout(() => {
                    resolve([
                        { id: 1, title: `搜索结果 ${term} 第1条` },
                        { id: 2, title: `搜索结果 ${term} 第2条` },
                        // .....
                    ]);
                }, 1000);
            });
        }
        let TestDemo = () => {
            const [searchTerm, setSearchTerm] = useState('');
            const [loading, setLoading] = useState(false);
            const [searchResults, setSearchResults] = useState([]);
            const [pending, startTransition] = useTransition({
                timeoutMs: 2000, // 过渡效果的持续时间（可选
            });
            const handleSearch = (event) => {
                setSearchTerm(event.target.value);
                // 使用 startTransition 标记数据加载为非紧急更新
                startTransition(() => {
                    setLoading(true);
                    fetchSearchResults(event.target.value)
                        .then((results) => {
                            setSearchResults(results);
                        })
                        .finally(() => {
                            setLoading(false);
                        });
                });
            };
            if (searchResults.length > 0) {
                return (
                    <div>
                        <input type="text" value={searchTerm} onChange={handleSearch} />
                        {pending ? <div>loading...</div> : <ul>
                            {searchResults.map((result) => (
                                <li key={result.id}>{result.title}</li>
                            ))}
                        </ul>}
                    </div>
                );
            }else{
                return (
                    <div>
                        <input type="text" value={searchTerm} onChange={handleSearch} />
                        <p>请输入搜索内容...</p>
                    </div>
                );
            }
        }

        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```

### 3)、useDeferredValue函数

> 提供一个可以“延迟”的值，直到屏幕更新。它接受一个值，并返回该值的新副本，该副本将推迟到更紧急地更新之后。

```html
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState, memo, useDeferredValue } = React;
        let { createRoot } = ReactDOM;

        let TestDemo = memo(({ query }) => {
            const text = 'hello world'
            const items = []
            if (query !== '' && text.includes(query)) {
                // 将字符串分割为子字符串数组，返回一个新数组。这个方法接收一个或多个参数，作为分隔符，用于确定在哪里分割字符串。
                //如果你的 query 是一个空字符串 ""，那么 split() 方法会将字符串中的每个字符都分割成一个独立的数组元素：
                const arr = text.split(query);
                //  console.log(arr,'arr') //如query='hello' 输出: ["", "world"]
                for (let i = 0; i < 9; i++) {
                    // 注意：这里我们假设arr.length >= 2，但在实际应用中你可能需要添加额外的检查
                    // 如果arr只有一个元素，则不显示arr[1]
                    items.push(
                        <li key={i}>
                            {arr[0]}
                            <span style={{ color: 'red' }}>{query}</span>
                            {arr.length > 1 ? arr[1] : ''}
                        </li>
                    );
                }
            } else {
                for (let i = 0; i < 9; i++) {
                    items.push(<li key={i}>{text}</li>);
                }
            }
            return (
                <ul>
                    {items}
                </ul>
            );
        })

        let App = () => {
            const [searchWord, setSearchWord] = useState('')
            // query延迟之后的值
            const query = useDeferredValue(searchWord)
            const handleChange = (ev) => {
                setSearchWord(ev.target.value)
                // 使用 startTransition 标记数据加载为非紧急更新
            }
            return (
                <div>
                    <input type="text" value={searchWord} onChange={handleChange} />
                   <TestDemo query={query} />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body
```

## 9、函数组件功能复用之自定义Hook

> 自定义Hook是React中的一个重要概念，它允许开发者将组件逻辑提取到可重用的函数中。这些函数以“use”开头，并可以调用其他的Hooks。自定义Hook的主要作用是抽象组件间的状态逻辑，使得功能组件更纯粹、更易于维护。

* 提炼能复用的逻辑

许多组件有相似的状态逻辑，使用自定义Hook可以很方便地提取出来复用。

* 解决复杂组件的可读性问题

将复杂组件拆分为更小的功能独立的函数，有助于提高代码的可读性。

* 管理数据更新

使用独立的Hook函数来管理数据请求、处理异步逻辑、数据缓存等，易于维护。

```HTML
<body>
    <div id="app"></div>
    <script type="text/babel">
        let app = document.querySelector('#app')
        let { useState,useEffect } = React;
        let { createRoot } = ReactDOM;
        let useMouseXY=()=>{
            const [x,setX]=useState(0)
            const [y,setY]=useState(0)
            useEffect(()=>{
                function move(event) {
                    setX(event.pageX)
                    setY(event.pageY)
                }
                document.addEventListener('mousemove', move,)
                return ()=>{
                    document.removeEventListener( 'mousemove', move)
                }
            },[])
            return {x,y}
        }
        let TestDemo = () => {
            const {x,y}=useMouseXY()
            return (
                <div>
                    X:{x},Y:{y}
                </div>
            );
        }

        let App = () => {
            return (
                <div>
                    <TestDemo />
                </div>
            );
        };
        const root = createRoot(document.querySelector('#app'));
        root.render(<App />);
    </script>
</body>
```
