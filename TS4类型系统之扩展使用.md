# 一、declare关键字

## 1、declare关键字

> 在 TypeScript 中，declare 关键字主要用于声明全局变量、函数、模块、类型别名或枚举，以便在 TypeScript 代码中引用它们，而无需实际定义它们。

* 声明全局变量或函数

> 当在 TypeScript 代码中访问某个全局变量或函数，但这个变量或函数并不是在 TypeScript 代码中定义的，而是在 JavaScript 运行时环境中定义的（如浏览器 API）。

```ts
declare var document: any;
declare function alert(message?: any): void;
```

* 声明模块

> 当需要告诉 TypeScript 编译器某个模块存在，但不想（或不能）在 TypeScript 中实际定义它时。

```ts
declare module "party" {
  export function doSomething(): void;
}
```

* 声明文件（.d.ts）

> 在 .d.ts 文件中使用 declare 关键字来定义类型。这些文件被称为声明文件，它们为 TypeScript 提供了关于 JavaScript 代码的额外类型信息。声明文件对于第三方库和框架特别有用，因为它们允许在 TypeScript 中使用这些库和框架，同时保留类型检查的好处。

## 2、"declaration": true,

> 在现代 TypeScript 项目中，通常不需要手动声明浏览器或 Node.js 的全局变量和函数，因为 TypeScript 已经内置了这些环境的声明文件（如 lib.dom.d.ts 和 lib.es2015.d.ts）。
在 TypeScript 的配置选项（通常是 tsconfig.json 文件中的选项）中，指定 TypeScript 编译器是否生成相应的 .d.ts 声明文件。这些声明文件包含了 TypeScript 源文件的类型信息，但不包含实现细节。它们的主要用途是允许其他 TypeScript 文件导入和使用这些类型，而无需直接访问实现文件。

# 二、@types和DefinitelyTyped仓库

> DefinitelyTyped是一个高质量的TypeScript类型定义的仓库。通过@types方式来安装常见的第三方JavaScript库的声明适配模块

## 1、@types:

> @types 是一个 npm 上的命名空间，用于托管 TypeScript 的类型声明文件。这些文件通常以 @types/包名 的形式发布，为那些没有自带 TypeScript 类型声明的 JavaScript 库提供类型信息。并且所有通过 @types 安装的类型声明文件都会保存在项目的 node_modules/@types 目录下。

## 2、DefinitelyTyped 仓库

- DefinitelyTyped 是一个 GitHub 仓库，由社区维护，提供了大量流行的 JavaScript 代码库的 TypeScript 类型声明文件。
- 如果你需要使用某个第三方 JavaScript 库，并希望在 TypeScript 中获得类型检查的支持，你可以先在 DefinitelyTyped 仓库中查找是否已经存在对应的类型声明文件。如果找到了，你可以直接使用；如果没有，你也可以参考已有的类型声明文件自己编写一个。
- DefinitelyTyped 仓库中的类型声明文件通常也是通过 npm 发布到 @types 命名空间下的。

> 生成`packjson.json`
```shell
npm init
```

> 安装第三方模块
```shell
npm i moment
```

```
import moment from 'moment'
moment().format('YYYYY')
```

> 在`.ts`文件中引入模块，如果模块中有ts声明则可以直接使用，不行是一般通常会有@types/包名 的形式发布的相同模块，为那些没有自带 TypeScript 类型声明的 JavaScript 库提供类型信息

# 三、lib.d.ts和global.d.ts

## 1、lib.d.ts

> lib.d.ts 文件是 TypeScript 编译器自带的一组核心类型声明文件。这些文件定义了 JavaScript 运行时环境（如浏览器环境或 Node.js 环境）中的全局对象、函数、接口等。当你安装 TypeScript 时，这些文件通常已经包含在TypeScript 安装包中。在浏览器环境中，lib.d.ts 文件会包含 window、document、HTMLElement 等全局对象的类型定义。在 Node.js 环境中，它会包含 process、Buffer、__dirname 等全局对象或变量的类型定义

## 2、global.d.ts

> global.d.ts 文件（或具有类似名称的其他全局声明文件）不是 TypeScript 的一部分，但它们是 TypeScript 社区中广泛使用的一种模式，用于声明全局变量、类型或函数。这些文件通常位于项目的根目录或某个特定的类型声明目录中。要在 TypeScript 项目中使用 global.d.ts 或其他全局声明文件，你需要确保 TypeScript 编译器能够找到它们。这可以通过在 tsconfig.json 中设置 include、files 或 typeRoots 和 types 选项来实现。


# 四、详解tsconfig.json配置文件

```json
compilerOptions: {} // 编译选项
files: [] // 包含在程序中的文件的允许列表
extends: " // 继承的另一个配置文件
include: [] // 指定的进行编译解析
exclude: [] // 指定的不进行编译解析
references: [] // 项目引用，提升性能
```

```json
{
    "compilerOptions": {
      /* 基本选项 */
      "target": "es5", /* 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ESNEXT'. */
      "module": "commonjs", /* 指定使用模块系统: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
      "strict": true, /* 启用所有严格类型检查选项。 */
      "esModuleInterop": true, /* 允许默认导入和 CommonJS 模块之间的互操作性。 */
      "jsx": "react", /* 指定 JSX 代码的生成: 'preserve', 'react-native', or 'react'. */
      "jsxFactory": "React.createElement", /* 指定要用于 JSX 元素的工厂函数。 */
      "declaration": false, /* 生成相应的 '.d.ts' 文件。 */
      "declarationMap": false, /* 生成 '.d.ts.map' 源文件映射以调试你的 '.d.ts' 文件。 */
      "sourceMap": true, /* 生成相应的 '.map' 文件。 */
      "outFile": "./", /* 将所有输出合并为一个文件。 */
      "outDir": "./dist/", /* 重定向输出结构到目录。 */
      "rootDir": "./src", /* 用来控制输出目录结构以匹配输入的目录结构。 */
      "composite": false, /* 启用项目编译以支持使用 --build 标志。 */
      "tsBuildInfoFile": "./", /* 指定文件来存储增量编译信息以加速后续编译。 */
      "removeComments": true, /* 不在输出文件中生成注释。 */
      "noEmit": false, /* 不生成输出文件。 */
      "importHelpers": true, /* 导入 emit helpers（例如 `__extends`，`__rest` 等）时从 tslib 导入。 */
      "downlevelIteration": true, /* 提供完全的向下兼容性，用于迭代器和生成器。 */
      "isolatedModules": false, /* 将每个文件作为单独的模块（与 'ts.transpileModule' 类似）。 */
      /* 严格检查选项 */
      "strictNullChecks": true, /* 启用严格的 null 检查。 */
      "strictFunctionTypes": true, /* 启用严格的函数类型检查。 */
      "strictBindCallApply": true, /* 启用严格的 `bind`，`call`，和 `apply` 方法检查。 */
      "strictPropertyInitialization": true, /* 启用严格的属性初始化检查。 */
      "noImplicitThis": true, /* 启用对 `this` 表达式的严格检查。 */
      "alwaysStrict": true, /* 以严格模式解析每个模块。 */
      /* 额外的检查 */
      "noUnusedLocals": true, /* 报告未使用的局部变量。 */
      "noUnusedParameters": true, /* 报告未使用的函数参数。 */
      "noImplicitReturns": true, /* 报告没有显式返回值的函数。 */
      "noFallthroughCasesInSwitch": true, /* 报告 switch 语句的 fallthrough 错误（即没有 break 的 case）。 */
      /* 模块解析选项 */
      "moduleResolution": "node", /* 选择模块解析策略：'node' (Node.js) 或 'classic' (TypeScript pre-1.6)。 */
      "baseUrl": "./", /* 解析非相对模块名的基准目录。 */
      "paths": {}, /* 映射模块名到基于 `baseUrl` 的路径。 */
      "rootDirs": [], /* 将多个目录的根合并为一个。 */
      "typeRoots": [], /* 指定要包含的类型声明文件的目录列表。 */
      "types": [], /* 要包含的类型声明文件名列表。 */
      "allowSyntheticDefaultImports": true, /* 允许从没有默认导出的模块中默认导入。 */
      /* Source Map 选项 */  
    "sourceRoot": "./src", /* 指定调试器应该找到 TypeScript 文件的位置，如果它们不在生成的位置。 */
      "mapRoot": "./", /* 指定调试器应该找到映射文件的位置。 */
      "inlineSourceMap": false, /* 生成单个源文件的内联 source maps。 */
      "inlineSources": false, /* 在生成的 source maps 中包含源文件的完整内容。 */
      /* 实验性选项 */
      "experimentalDecorators": true, /* 启用实验性的装饰器功能。 */
      "emitDecoratorMetadata": true, /* 为装饰器提供元数据的支持。 */
      /* 错误诊断 */
      "diagnostics": false, /* 报告这个编译器的额外诊断信息。 */
      "traceResolution": false, /* 报告模块解析的日志信息。 */
      "listFiles": false, /* 列出编译的文件。 */
      /* 其他选项 */
      "noResolve": false, /* 如果设为 true，则不会解析三方模块标识符。 */
      "forceConsistentCasingInFileNames": true, /* 确保在读取文件时大小写一致。 */
      "skipLibCheck": false, /* 跳过所有声明文件的类型检查。 */
      /* 路径别名配置（通过 tsconfig-paths 或其他插件使用）*/
      "baseUrl": "./", /* 解析非相对模块名的基准目录。 */
      "paths": {
        "@/*": [
          "src/*"
        ]
      },
      /* 插件配置 */
      "plugins": [], /* 插件列表，用于自定义 TypeScript 的编译过程。 */
      /* 编译时包含/排除文件 */
      "include": [
        "src/**/*"
      ],
      "exclude": [
        "node_modules",
        "**/*.spec.ts"
      ],
      /* 编译上下文配置 */
      "extends": "./configs/base", /* 从另一个文件继承 tsconfig 选项。 */
      /* 使用 TypeScript 编译器 API 的配置 */
      "compilerHost": {}, /* 允许自定义编译器主机。 */
      /* 自定义 TypeScript 编译器选项 */
      "//": "在这里可以添加任何自定义的 TypeScript 编译器选项，但请确保它们是有效的并受到 TypeScript 的支持。",
      /* 列表文件 */
      "files": [], /* 显式指定要编译的文件列表。 */
      /* 格式化 */
      "pretty": true, /* 以格式化的 JSON 输出文件。 */
      // ... 可能还有其他不常见的或实验性的配置选项 ...  
    }
  }
```
