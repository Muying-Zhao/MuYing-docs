# 前言

> TS编写代码的好处诸多，下面我们列出几点：
友好的IDE提示
强制类型，防止报错
语言编写更加严谨
快速查找到拼写错误
JS的超集，扩展新功能

# 一、TS运行环境搭建

> TypeScript是微软开发的一个开源的编程语言，通过在JavaScript的基础上添加静态类型定义构建而成。TypeScript通过TypeScript编译器或Babel转译为JavaScript代码，可运行在任何浏览器，任何操作系统。

## 1、TS文件需要编译成JS文件

* 新建`ts-study`项目，

* 安装ts插件

> 首先我们的浏览器是不认识TS文件的，所以需要把TS编译成JS文件才可以，TS官网提供了一种方式，就是去全局安装typescript这个模块，命令如下：

```shell
npm init -y
npm install -g typescript
```

* 新建`01_demo.ts`

```ts
let test=123;
console.log(test,'test')
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
    <script src="./01_demo.js"></script>
</head>
<body>
</body>
</html>
```

* 编译`ts`文件命令

> 默认自动编译对应的js文件

```shell
tsc 01_demo.ts
```

## 2、ts文件自动编译js文件

> 在`tsc`命令进行转换操作的时候，是不能实时进行转换的，那么可以通过添加一个`-w`的参数来完成实时转换的操作

```shell
tsc 01_demo.ts -w
```

## 3、ts文件使用模块化变成局部环境

> 在编译后，我们会发现TS文件中定义的变量会产生错误的波浪线，这主要是因为TS默认是全局环境下的，所以跟其他文件中的同名变量冲突了，所以需要使用模块化操作来使其变成局部环境

```ts
let test=123;
console.log(test,'test')
export {}
```

## 4、`tsconfig.json`文件

* 生成`tsconfig.json`文件

```shell
tsc --init
```

> `"outDir": "./"`换成`"outDir": "./dist"`,自动将其打包到dist目录下
`"include": ["01_demo.ts"]`只转化01_dmeo.ts文件，
默认是`"include": ["*"]·将其所有ts文件转化

* 默认将所有ts文件转化命令

```shell
ts
```

* `"module": "commonjs"` 

> 转化`cjs`模块风格,还可以选择`ES6`，

* "target": "es2016"

> 可以指定模块版本,如`ES5`

# 二、类型声明空间，变量声明空间

* 类型注解

> 同时包含类型声明空间（type A =string）与变量声明空间（let a='hello'）

```ts
let a:string=''hello world'
```

* 类型声明空间（type A =string）

```ts
type A =string
```

* 变量声明空间（let a='hello'）

```ts
let a='hello world'
```

* 还可以省略类型注解（如果 TypeScript 可以从初始值中推断出类型

```ts
let a = 'hello world';
```

* 类在TS中即是变量声明空间也是类型声明空间

```ts
class fn {}
let a=fn
type A=fn
```

# 三、类型分类与使用

|类型归类|类型|
|:-:|:-:|
|基本类型|string number boolean null undefined symbol bigint|
|对象类型|[] {} function()[]|
|TS新增类型|any never void unknown enum|

## 1、基本类型

* bigint

> 使用时ES不能低于2020，任意精度的整数类型，用于表示大于 Number.MAX_SAFE_INTEGER（即 2^53 - 1）的整数。

```
let b : bigint =1n
```

* symbol

> 符号类型，用于表示唯一的标识符，通常用于对象的属性键

```ts
// 创建一个 symbol 类型的值
let sym: symbol = Symbol('mySymbol');
// 创建一个对象，并使用 symbol 作为属性键
let obj: { [key: symbol]: string } = {};
obj[sym] = 'Hello, symbol!';
// 尝试使用普通字符串作为键来访问该属性会失败
console.log(obj['mySymbol']); // undefined，因为属性键是 symbol 类型，不是字符串
// 使用正确的 symbol 键来访问属性
console.log(obj[sym]); // 输出："Hello, symbol!"
```

## 2、联合类型,(或`|`)

> 类型之间进行或的操作

```ts
let a:string|number = 'hello'
a='hello world';
a=000
```

## 3、交叉类型,(与`&`)

> 类型之间进行与的操作

```ts
type A={
    username:string
}
type B={
    age:number
}
let a: A & B = { username: 'muying', age: 20 }
```

## 4、never类型、any类型、 unknown类型

* never类型

> never 类型在 TypeScript 中代表那些永不存在的值的类型。具体来说，它表示的是那些永远不会有返回值的函数（如抛出错误的函数或无限循环的函数）的返回类型。

```ts
function fn(n:1|2|3) {
    switch (n) {
        case 1:
            break;
        case 2:
            break;
        case 3:
            break;
        default:
             // 检测n是否可以走到这里，看所有值全部被使用到
            let m:never=n;
            break;
    }
}
fn(1)
```

* any类型

> 当声明一个变量为 any 类型时，可以在这个变量上执行任何操作，而 TypeScript 编译器不会给出类型错误。这在一定程度上类似于 JavaScript 的动态类型系统，但在 TypeScript 中，any 类型是显式声明的。

```ts
let a:any='hello'
a=123
```

* unknown类型

> 与 any 类型相似，但 unknown 更加安全，因为它不允许你在不知道其确切类型的情况下对值进行任何操作。在类型检查上，unknown 类型的值被当作是安全的，因为任何值都可以被赋值给 unknown 类型的变量。

```ts
let a:unknown='hello'
a=123
```

## 5、类型断言

* 类型断言

> 类型断言有两种形式：尖括号形式（`<Type>value`）和 as 关键字形式（`value as Type`）

```ts
let a:unknown='hello world';
(a as []).map(()=>{});
```

* 非空类型断言

> 使用 ! 后缀来明确地告诉 TypeScript 编译器某个值不是 null 或 undefined。

```ts
let a:string|undefined=undefined;
b!.length
```

## 6、数组类型

* 类型`[]`

```ts
let a:number[]=[1,2,3]
```

* Array<类型>

```ts
let a:Array<number>=[1,2,3]
```

* 混合数组类型

```ts
let arr:(number|string)[]=[1,2,3,'hello','world']
```

* 元组类型

> 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同，限定了个数，顺序也需要保持一致

```ts
let arr:[number:string]=[1,'hello'];
```

## 7、对象类型

* 直接字面量

> 当你有一个具有确切属性名和类型的对象时，可以直接使用字面量形式定义其类型。

```ts
type Person = {
  name: string;
  age: number;
  greet(): void;
};

const john: Person = {
  name: 'XiaoMu',
  age: 20,
  greet() {
    console.log('Hello World');
  }
};
```

* 可选属性

> 可以通过在属性名后加上 ? 来标记属性为可选的。

```ts
type Person = {
  name: string;
  age?: number; // 可选属性
  greet(): void;
};
const jane: Person = {
  name: 'XiaoMu',
  greet() {
    console.log('Hello World');
  }
  // age 属性在这里是可选的，所以可以省略
};
```

* 只读属性

> 使用 readonly 关键字可以定义只读属性，这些属性在对象被创建后不能被修改。

```ts
type Person = {
  readonly id: number; // 只读属性
  name: string;
};
const person: Person = {
  id: 1,
  name: 'XiaoMu'
};
// 下面这行代码将会引发错误，因为 id 是只读的,不能被修改
// person.id = 2;
```

*  索引签名

> 如果不确定对象会有哪些属性，如果知道它们的类型，可以使用索引签名。

```ts
type Dictionary = {
  [key: string]: string; // 使用 string 类型的键和值的索引签名
};
const dict: Dictionary = {
  firstName: 'XiaoMu',
  lastName: 'XiaoBu'
};
// 索引签名允许你使用任何 string 类型的键来访问值
console.log(dict['firstName']); // 输出: XiaoMu
```

* 复杂类型写法

```ts
//type one
type Obj={username:string}
let obj={} as Obj
obj.username='nice'
//type two
let users ：{username：string，age：number}[]=[]
// 可以向这个数组中添加符合 { username: string, age: number } 类型的对象
users .push({ username: 'XiaoMu', age: 20 });
```

## 8、函数类型与void类型

* 函数类型使用

```ts
// TS要求：实参的个数跟形参的个数必须相同
function fn(count:number,m?:string) {
    console.log(count,'count')
}
fn(123)
fn(123,'hello world')

// // 如果()后在跟一个类型值，表示需要返回的类型值
// function fn1(count: number, m?: string): number {
//     console.log(count, 'count')
//     return count
// }

// fn1(20)

// // 函数表达式
// let test: (count: number, m?: string) =>number=function fn(m,n) {
//     console.log(m, 'count')
//     return 123
// }
```

* void类型

> void 类型在 TypeScript 中通常表示没有返回值的函数。当看到函数的返回类型是 void 时，这意味着这个函数不返回任何值（或者更确切地说，它返回 undefined）。如果写的类型为undefined，则不能不返回reAturn，如果为void，既可以返回return也可以不写

## 9、函数重载与可调用注解

* 模拟函数重载

> 在 TypeScript 中，你可以使用联合类型来模拟函数重载。每个重载签名都是一个独立的函数签名，它们被组合成一个类型，该类型作为函数的实际类型。但是，你只能在函数体内部实现一个实现逻辑

```ts
function overloadFunc(name: string): string;
function overloadFunc(age: number): string;
function overloadFunc(nameOrAge: string | number): string {
  // 注意：这里只有一个实现
  if (typeof nameOrAge === 'string') {
    return `Hello, ${nameOrAge}`;
  } else {
    return `Age: ${nameOrAge}`;
  }
}
// 使用函数
const greeting = overloadFunc('XiaoMu'); // 调用字符串版本的函数
const ageStatement = overloadFunc(20);   // 调用数字版本的函数
```

* 可调用注解

> 在 TypeScript 中，可以定义一个类型，该类型表示一个可调用的对象（即函数）。这通常是通过定义一个接口，并在该接口中声明一个带有特定签名的调用签名（call signature）来实现的。

```ts
type A = {
    username: string
}
let fn: A = {
    username: 'xiaomu'
};
```

> 定义了一个名为A的类型，该类型表示一个对象

```
type A={
 (n:number):number
 username?:string
}
let fn:A=(n)=>{return n}
fn.username='xiaomu'
```

>  函数重载与可调用注解相结合

```ts
type A = {
  (n:number,m:number):any
  (n:string,m:string):any
}
function fn(n:number,m:number):any
function fn(n:string,m:string):any
function fn(n:number|string,m:number|string):any{
}
let a:A=fn;
```

> 通过定义一个接口，并在该接口中声明一个带有特定签名的调用签名（call signature）来实现的。

```ts
interface Callable {
  (arg: string): void;
}
// 实现这个接口的函数
const myFunction: Callable = (arg) => {
  console.log(arg);
};
// 调用函数 
myFunction('Hello, world!');
```

## 10、枚举类型

* 枚举（Enum）

> 枚举类型定义了一组命名的常量。默认情况下，枚举成员的值是递增的整数，从0开始。也可以为枚举成员指定任何值。

```ts
enum Color {
    Red,
    Green,
    Blue
}
console.log(Color.Red);  // 输出: 0
console.log(Color.Green); // 输出: 1
console.log(Color.Blue); // 输出: 2
```

> 为枚举成员指定特定的值

```ts
enum Color {
    Red = 1,
    Green,
    Blue = 4
}
console.log(Color.Green); // 输出: 2，因为Green在Red之后，所以它的值是Red的值加1
console.log(Color);//未映射过的原有枚举
```

> 反向映射

```ts
enum Color {
    Red,
    Green,
    Blue
}
console.log(Color[0]);  // 输出: Red
console.log(Color[1]); // 输出: Green
console.log(Color[2]); // 输出: Blue
console.log(Color);//映射过的原有枚举
```

> 枚举既可以作为值也可以作为类型

```ts
enum Roles {
    SUPER_ADMIN = 'super_admin ',
    ADMIN = 'admin',
    USER = 'user'
}
if(Roles.ADMIN=='admin'){
    console.log(Roles.ADMIN,'Roles.ADMIN')
}
```

* const枚举（Const Enum）

> const枚举与普通枚举的主要区别在于它们在编译时的处理方式。当使用const枚举时，TypeScript编译器会在编译时尽可能地消除对枚举的引用，并直接内联枚举成员的值。这可以提高性能，并减少生成的代码大小。

```ts
const enum Color {
    Red,
    Green,
    Blue
}
let myColor: Color = Color.Red; // 在TypeScript代码中，这是类型安全的 let myColor=0
// 假设你有一个接受Color类型参数的函数
function printColor(color: Color) {
    // ...
}
printColor(Color.Green); // 在TypeScript代码中，这也是类型安全的 printColor(1);
```
