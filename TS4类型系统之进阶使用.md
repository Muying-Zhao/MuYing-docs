# 一、字面量类型和keyof关键字

## 1、字面量类型（Literal Types）

> 字面量类型允许指定一个变量只能是几个特定的字面量值之一。这些值通常是字符串字面量、数字字面量或布尔字面量。

```ts
let x: "hello" | "world"; // x只能是"hello"或"world"
let y: 1 | 2 | 3; // y只能是1、2或3
x = "hello"; // true
x = "world"; // true
x = "other"; // false
y = 1; // true
y = 4; // false
```

## 2、keyof关键字

* keyof使用

> keyof关键字用于获取一个对象类型的所有键（key），生成一个字符串字面量类型的联合类型。

```ts
interface Person {
    username: string;
    sex: string;
    age: number;
}
type PersonKeys = keyof Person; // 等同于 "username" | "sex" | "age" |
let key: PersonKeys;
key = "username"; // true
key = "sex"; // true
key = "age"; // true
key = "other"; // false
```

* keyof来索引对象类型的属性

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
let person = { name: "xiaomu", age: 20 };
let name = getProperty(person, "name");
let age = getProperty(person, "age");
```

# 二、详解接口与类型别名

## 1、接口（Interface）

> 接口定义了一个对象的形状，它包含了一组属性的类型声明，但不包含属性的具体值。接口可以定义方法签名，但不能定义方法的具体实现。

```ts
interface Person {
    name: string;
    age: number;
}
interface Arr{
    [index:number]:number;
}
interface Fn {
    ():void;
}
let a：Person={
    name: 'xiammu';
    age: 20;
}
let b:Arr=[1,2,3]
let f:Fn=()=>{}
```

* 接口具有对象类型

> 接口在 TypeScript 中通常用于描述对象的形状，即该对象应该有哪些属性，以及这些属性的类型是什么。即可以将接口看作是一种特殊的对象类型。

```ts
interface Person {
    name: string;
    age: number;
}
const john: Person = {
    name: 'xiammu';
    age: 20;
};
```

* 接口合并

> TypeScript 支持接口合并，这意味着可以定义多个接口，并将它们合并成一个接口。合并后的接口将包含所有合并前接口的属性。

```ts
interface Person {
    name: string;
    age: number;
}
interface Employee {
    employeeId: number;
}
type FullPerson = Person & Employee;
const employee: FullPerson = {
    name: 'xiammu';
    age: 20;
    employeeId: 111
};
```

* 接口继承

> TypeScript 中的接口也可以继承其他接口，这意味着一个接口可以继承另一个接口的所有属性和方法。

```ts
interface Person {
    name: string;
    age: number;
}
interface Employee extends Person {
    employeeId: number;
}
const employee: Employee = {
    name: string;
    age: number;
    employeeId: 111
};
```

## 2、类型别名

* 映射类型

> 映射类型是一种高级类型，它允许根据另一个类型的所有属性键来创建新的类型。在 TypeScript 中，可以使用 in 关键字来定义映射类型，其中映射类型不能直接在interface使用

```ts
// 定义一个字符串字面量类型的联合
type Keys = 'username' | 'sex' | 'age';
// 该对象类型的属性名是T中的每一个值，属性值是string类型
type KeyedStrings<T extends Keys> = {
    [P in T]: string;
};
// 使用映射类型创建一个新的类型
type PersonProperties = KeyedStrings<Keys>;
// PersonProperties 现在等同于：
// type PersonProperties = {
//     username: string;
//     sex: string;
//     age: string;
// }
// 创建一个符合PersonProperties类型的对象
const person: PersonProperties = {
    username: 'xiaomu',
    sex: 'Boy',
    age: '20'
};
```

* 映射类型与接口Interface相结合

> 映射类型本身并不是直接在interface中使用的，因为映射类型是通过type关键字定义的，而不是interface。但是，你可以在一个interface中使用映射类型作为属性的类型。

```ts
type Keys = 'username' | 'sex';
type KeyedStrings<T extends Keys> = {
    [P in T]: string;
};
interface Person {
    // details属性是一个KeyedStrings类型，其属性为username和sex，类型为string
    details: KeyedStrings<Keys>;
    age: number;
}
// 创建一个符合Person接口的对象
const person: Person = {
    details: {
        username: 'xiaomu',
        sex: 'Boy',
    },
    age: 20
};
```

# 三、ts类型保护

## 1、内置类型保护

* typeof 类型保护

> 使用 typeof 运算符来检查值的类型。

```ts
function isString(x: any): x is string {
    return typeof x === "string";
}
```

* instanceof 类型保护

> 使用 instanceof 运算符来检查值是否是某个类的实例。在这个例子中，`x is Dog` 是一个类型保护，它告诉TypeScript编译器，如果 isDog 函数返回 true，那么 x 一定是 Dog 类型。

```ts
class Animal {
    eat(): void {
        console.log('Animal eats');
    }
}
class Dog extends Animal {
    bark(): void {
        console.log('Dog barks');
    }
}
function isDog(x: Animal): x is Dog {
    return x instanceof Dog;
}
let pet: Animal = new Dog();
if (isDog(pet)) {
    pet.bark();
} else {
    pet.eat();
}
```

## 2、自定义类型保护

> 当内置的类型保护不足以满足需求时，可以创建自定义的类型保护。这通常是通过编写一个返回类型保护签名的函数来实现的

```ts
type Fish = { swim: () => void; };
type Bird = { fly: () => void; };
function isFish(x: Fish | Bird): x is Fish {
    return (x as Fish).swim !== undefined;
}
function handleAnimal(animal: Fish | Bird) {
    if (isFish(animal)) {
        animal.swim(); // 这里TypeScript知道animal是Fish类型
    } else {
        (animal as Bird).fly(); // 这里我们需要手动断言，因为TypeScript不知道else分支中animal是Bird类型
    }
}
```

# 四、泛型

> 泛型（Generics）允许定义灵活的组件，这些组件可以工作于多种数据类型。通过使用泛型，可以创建可重用的组件，这些组件可以适应多种数据类型，而无需为每种数据类型都重新编写代码。

## 1、定义泛型

> 泛型是通过在类型或函数名后面添加尖括号（`< >`）和类型参数来定义的。这些类型参数通常是大写字母（如T、U、V等），但它们可以是任何有效的标识符。

```ts
type A<T, U> = T | U;
type Arr<T>=T[]
function fn<T>(arg: T): T {
    return arg;
}
function Fn<T>(n:T){
    return n;
}
let a: A<string,number> = 'hello world'
let arr:Arr<number>=[1,2,3]
let output = fn<string>("xiaomu");
let output2 = fn(20);
let output3 = Fn('hello world'); // 这里不需要 <string>，因为 TypeScript 会自动推断,也是常用写法
let output4=Fn<string>('hello world')
```

* 泛型接口

```ts
interface Fn<T> {
    (arg: T): T;
}
let identityFn: Fn<number> = (x) => x;
let result = identityFn(20); // result 的类型是 number，值为 20
console.log(result); // 输出 20
```

* 泛型类

```ts
class Foo<T> {
    username!:T;
}
class Tfo extends FOO<string>{}
let f=new Foo<string>();
let tf=new Tfo()
f.username='xiaomu'
tf.username='extends'
```

* 多参数泛型类的使用

```ts
class Fn<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
    constructor(zeroValue: T, add: (x: T, y: T) => T) {
        this.zeroValue = zeroValue;
        this.add = add;
    }
    zero(): T {
        return this.zeroValue;
    }
    addNumbers(x: T, y: T): T {
        return this.add(x, y);
    }
}
let myFn= new Fn<number>(0, (x, y) => x + y);
// 使用 zero 方法获取零值
let zero = myFn.zero();
console.log(zero); // 输出：0
// 使用 addNumbers 方法执行加法
let sum = myFn.addNumbers(5, 3);
console.log(sum); // 输出：8
```

## 2、泛型常见操作

* 类型参数

> 在泛型定义中，类型参数（如T）用于表示类型占位符，这些占位符将在使用泛型时由具体的类型来替换。

* 类型推断

> 在调用泛型函数或实例化泛型类时，TypeScript编译器会尝试根据提供的参数来推断类型参数。如果编译器无法推断出类型参数，可能需要显式地指定它们。

* 泛型约束

> 可以使用extends关键字为泛型类型参数添加约束。这允许指定类型参数必须满足的接口或类型。

```ts
interface Fn {
    length: number;
}
function Foo<T extends Fn>(arg: T): T {
    console.log(arg.length);  // 现在我们可以访问arg.length属性了
    return arg;
}
```

* 泛型类型别名

> 可以使用type关键字为泛型创建类型别名

```ts
type Fn<T> = { value: T };
let f: Fn<string> = { value: "Hello, world!" };
```

* 默认泛型类型

> 在TypeScript 2.3及更高版本中，可以为泛型类型参数提供默认类型

```ts
//接受两个参数：length（表示数组的长度）和 value（表示数组中每个元素的值）
function Arr<T = string>(length: number, value: T = "default"): Array<T> {
    let result: T[] = new Array(length);
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
let a = Arr(3); // a 的类型是 string[]
let b = Arr<number>(3, 18); // b 的类型是 number[]
console.log(a); // 输出: ["default", "default", "default"]
console.log(b); // 输出: [42, 42, 42]
```

* 泛型数组

> 泛型可以与数组一起使用，以创建可以包含任何数据类型的数组。

```ts
let list: Array<number> = [1, 2, 3];
let list2: number[] = [1, 2, 3];
```


* 泛型元组

> ypeScript 3.0引入了泛型元组类型，允许你创建具有特定数量和类型的元素的元组。

```ts
type Pair<T, U> = [T, U];
let pair: Pair<string, number> = ["hello world", 20];
```

* 泛型映射类型

> 使用泛型映射类型，你可以基于一个已知的类型创建新的类型。

```ts
type Fn<T> = {
    [P in keyof T]?: T[P];
};
type  Foo= {
    name: string;
    sex: string;
    age: number;
};

type f = Fn<Foo>; // { name?: string;  sex?: string;age?: number; }
```

# 五、类型兼容性

## 1、类型兼容性规则

* 属性兼容性

> 如果一个类型（源类型）的所有属性都存在于另一个类型（目标类型）中，并且属性类型也是兼容的，那么源类型就可以被赋值给目标类型。这确保了不会丢失任何属性信息。

```ts
interface Person {
    name: string;
    age: number;
}
let person: Person = { name: "xiaomu", age: 20 };
let obj: { name: string }
obj=person; // 兼容，因为obj包含了person的name属性
//person=obj; // 不兼容，因为obj不包含了person的age属性
console.log(obj,'obj')
// obj {
//     name: 'xiaomu';
//     age: 20;
// }
```

* 函数兼容性

> 函数之间的兼容性判断主要依据参数列表和返回值类型。目标函数的参数可以比源函数的参数少，但是不能多出来。此外，源函数的返回值类型应该是目标函数返回值类型的子类型或与其兼容。
参数列表是“协变”的（contravariant）：这意味着源函数的参数类型必须是目标函数参数类型的超集或完全相同。换句话说，源函数可以接收更少或更“宽松”类型的参数。
返回值是“逆变”的（covariant）：这意味着源函数的返回类型必须是目标函数返回类型的子集或完全相同。换句话说，源函数可以返回更具体或更“严格”类型的值。

```ts
// 目标函数类型
type TargetFunction = (x: number, y: string) => void;

// 源参数与返回值均兼容
function sourceFunction1(x: number, y: string) {
    console.log(`x: ${x}, y: ${y}`);
}
let target1: TargetFunction = sourceFunction1; // OK

// 参数少(兼容)、返回值不兼容
function sourceFunction2(x: number) {
    return x;
}
// let target2: TargetFunction = sourceFunction2; // Error
// 使用类型断言绕过类型检查，但在实际代码中不推荐这样做
let target2: TargetFunction = sourceFunction2 as any as TargetFunction;
// 参数多（不兼容）

function sourceFunction3(x: number, y: string, z: boolean) {
    console.log(`x: ${x}, y: ${y}, z: ${z}`);
}
// let target3: TargetFunction = sourceFunction3; // Error
```

* 其他类型兼容性

> TypeScript还支持其他类型的兼容性判断，如枚举、交叉类型、联合类型等。

## 2、类型断言

> 当TypeScript的类型推断不符合你的预期时，可以使用类型断言来明确告诉TypeScript编译器某个值的类型。类型断言使用as关键字或`<Type>`语法。

```ts
let value: any = "Hello World";
let length: number = (value as string).length;
```

## 3、显式类型转换与隐式类型转换

* 显式类型转换

> 使用类型断言操作符（`< >`）或as关键字来进行转换。


* 隐式类型转换

>编译器自动将变量从一种类型转换为另一种类型的过程。当数值类型的变量与字符串类型的变量进行运算时，TypeScript会自动将数值类型转换为字符串类型。

## 4、类型推断

> 在TypeScript中，当在声明一个变量时没有指定其类型，TypeScript会依据类型推论的规则推断出一个类型

* 声明变量时赋值
> TypeScript会推断出该变量的类型，之后若赋值其他类型会报错。

* 声明变量时未赋值

> TypeScript会推断该值为any类型，之后可以赋值为任何类型。

# 六、映射类型与内置工具类型

## 1、映射类型

> 映射类型能够将一个类型（通常是一个对象类型）的每个属性映射到新的类型。在映射类型中，新类型以相同的形式去转换旧类型里每个属性。

```ts
interface Obj {
    x: number;
    y: string;
    n: any;
}
type Mapping<T> = {
    [P in keyof T]: number;
};
type MappedObj = Mapping<Obj>;
// MappedObj= { x: number; y: number; n: number; }
```

## 2、内置工具类型

> TypeScript 提供了一些内置的工具类型，这些类型可以作为构建更复杂类型的基础。这些内置类型工具可以直接使用，用于更方便地处理各种类型，以及生成新的类型。以下是一些常用的内置工具类型：

*  `Partial<T>`

> `Partial<T>`构造类型将一个类型的所有属性都变为可选。

```ts
interface Todo {
    title: string;
    description: string;
}
type PartialTodo = Partial<Todo>;
// 等同于：
// type PartialTodo = {
//     title?: string;
//     description?: string;
// }
```

*  `Required<T>`

> `Required<T>`构造类型将一个类型的所有属性都变为必选。

```ts
interface Todo {
    title: string;
    description: string;
}
type RequiredTodo = Required<PartialTodo>;
// 等同于：
// type RequiredTodo = {
//     title: string;
//     description: string;
// }
```

*  `Readonly<T>`

> `Readonly<T>` 构造类型将一个类型的所有属性都变为只读，意味着你不能修改这些属性的值。

```ts
interface Todo {
    title: string;
    description: string;
}
type ReadonlyTodo = Readonly<Todo>;
// 等同于：
// type ReadonlyTodo = {
//     readonly title: string;
//     readonly description: string;
// }
```

*  `Record<K, T>`

> `Record<K, T>`构造类型允许你创建一个对象类型，其属性名为 K 类型的值的集合，属性值为 T 类型。

```ts
type Key = 'id' | 'name';
type KeyValuePairs = Record<Key, string>;
// 等同于：
// type KeyValuePairs = {
//     id: string;
//     name: string;
// }
```

*  `Pick<T, K extends keyof T>`

> `Pick<T, K>`构造类型从一个类型 T 中选择一组属性 K 来构造一个新的类型。

```ts
interface Todo {
    title: string;
    description: string;
}
type TodoPreview = Pick<Todo, 'title'>;
// 等同于：
// type TodoPreview = {
//     title: string;
// }
```

*  `Omit<T, K extends keyof T>`

> `Omit<T, K extends keyof T>`构造类型与 Pick 相反，它从类型 T 中排除一组属性 K 来构造一个新的类型。

```ts
interface Todo {
    title: string;
    description: string;
}
type TodoWithoutDescription = Omit<Todo, 'description'>;
// 等同于：
// type TodoWithoutDescription = {
//     title: string;
// }
```

*  `Exclude<T, U>`

> `Exclude<T, U>`构造类型从类型 T 中排除所有可以赋值给类型 U 的成员。

```ts
type T0 = Exclude<'a' | 'b' | 'c', 'a' | 'b'>;  // 'c'
```

*  `Extract<T, U>`

> `Extract<T, U>`构造类型从类型 T 中提取所有可以赋值给类型 U 的成员

```ts
type T0 = Extract<'a' | 'b' | 'c', 'a' | 'f'>;  // 'a'
```

*  `NonNullable<T>`

> `NonNullable<T>`构造类型从类型 T 中排除 null 和 undefined。

```ts
type T0 = NonNullable<string | null | undefined>;  // string
```

*  `Parameters<T extends (...args: any) => any>`

> `Parameters<T>`构造类型用于获取函数类型 T 的参数类型元组。

```ts
type FuncParams = Parameters<(x: number, y: string) => void>;
// 等同于：
// type FuncParams = [x: number, y: string]
```

*  `ReturnType<T extends (...args: any) => any>`

> `ReturnType<T>`构造类型用于获取函数类型 T 的返回类型。

```ts
type FuncReturn = ReturnType<(x: number) => string>;
// 等同于：
// type FuncReturn = string1
```

# 七、条件类型和infer关键字

## 1、条件类型

> 条件类型（Conditional Types）是一种高级类型特性，它允许你基于条件表达式来创建新的类型。

```ts
T extends U ? X : Y
```

## 2、infer 关键字

> infer 关键字用于在条件类型中声明一个类型变量，并基于给定的条件表达式来推断这个类型变量的实际类型。它常与 extends 关键字和类型别名一起使用，特别是在使用映射类型（Mapped Types）和条件类型结合的高级场景中。

```ts
type A<T> = T extends Array<infer U> ? U : never;
type B = A<Array<number>>;//B 的类型是 number，
type C = A<string>;//C 的类型是 never，
```

# 八、类中如何使用类型

> 类（class）可以使用类型来定义其成员变量（属性）和成员方法（方法）的参数和返回值。这有助于确保类的实例在使用时具有正确的类型，并在编译时捕获类型错误。

## 1、类的属性类型

```ts
class Person {
    // 使用类型注解来定义属性
    name: string;
    age: number;
    // 构造函数也可以有类型注解
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    // ... 其他方法 ...
}
// 创建 Person 类的实例
const person = new Person('xiaomu', 18);
// person.name='xiaobu'
// person.age=20
```

## 2、方法的参数和返回类型

```ts
class Greeter {
    // 方法的参数和返回类型都可以有类型注解
    greet(name: string): string {
        return `Hello, ${name}!`;
    }
    // 也可以定义可选参数和默认值
    greetWithDefault(name: string = 'World'): string {
        return `Hello, ${name}!`;
    }
}
// 使用 Greeter 类的实例
const greeter = new Greeter();
console.log(greeter.greet('Xiaomu')); // 输出: Hello, Xiaomu!
console.log(greeter.greetWithDefault()); // 输出: Hello, World!
```

## 3、使用克隆方法创建新实例

```ts
class Greeter {
    // 定义类的属性
    name: string;
    age: number;
    // 构造函数，用于初始化属性
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    // 用于返回一个新的 Greeter 实例，其属性与当前实例相同
    clone(): Greeter {
        return new Greeter(this.name, this.age);
    }

    greet(): string {
        return `My name is${this.name} and I'm ${this.age} years old.`;
    }
}

// 使用 Greeter 类的实例
const greeter = new Greeter('xiaomu', 20);
console.log(greeter.greet()); // 输出: My name is xiaomu and I'm 20 years old.

// 使用 clone 方法创建一个新的 Greeter 实例
const clonedGreeter = greeter.clone();
console.log(clonedGreeter.greet()); // 输出:My name is xiaomu and I'm 20 years old.（与原始实例相同）

// 原始实例和克隆实例是不同的对象
console.log(greeter === clonedGreeter); // 输出: false
```

## 4、类的静态属性和方法

```ts
class MyClass {
    // 静态属性
    static staticProperty: string = 'xiaomu';
    // 静态方法
    static staticMethod(param: number): string {
        return `${param}`;
    }
}
// 访问静态属性和方法
console.log(MyClass.staticProperty); // 输出: xiaomu
console.log(MyClass.staticMethod(20)); // 输出: 20
```
