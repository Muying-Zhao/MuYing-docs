# 前言

在软件开发的浩渺星海中，编程规范如同航海的罗盘，为我们指引方向，确保我们的代码之旅能够顺利、高效地到达目的地。无论是个人开发者还是大型团队，编程规范都是提升代码质量、保障项目成功不可或缺的一环。
因此，了解和掌握编程规范对于每一个开发者来说都至关重要。在接下来的内容中，我们将深入探讨编程规范的意义、作用以及如何在实际项目中应用它们。希望这些内容能够为您的编程之旅提供有益的指导和帮助。

# 一、为什么需要编程规范

> 编程规范是软件开发中的关键准则，它确保了代码的可读性、可维护性和一致性。在团队项目中，规范是协同开发的基石，有助于减少冲突，提升效率。学习和遵循编程规范，不仅有助于提升代码质量，还能提升开发者的职业素养。以下是为什么需要编程规范的几个主要原因：

1. 可读性：

- 编程规范确保了代码的一致性和可预测性，使得其他开发者能够更容易地理解和维护代码。
- 遵循一致的命名约定、缩进和格式规范可以提高代码的可读性。

2. 可维护性：

- 清晰、结构化的代码更易于修改和扩展。
- 编程规范可以确保代码在多人协作的环境中保持一致性，减少因个人风格差异而导致的维护困难。

3. 减少错误：

- 遵循编程规范可以减少常见的编程错误，如拼写错误、语法错误和逻辑错误。
- 通过强制使用特定的命名约定和格式，可以减少因误解或混淆而导致的错误。

4. 团队合作：

- 在团队项目中，编程规范可以确保所有成员遵循相同的代码风格和质量标准。
- 这有助于减少团队成员之间的摩擦，提高协作效率。

5. 版本控制：
- 当代码库在版本控制系统中进行迭代和合并时，一致的编程规范可以确保合并冲突更少，并减少因格式差异而产生的噪音。

6. 自动化工具：

- 编程规范可以与自动化工具（如代码格式化器、代码检查器和代码分析工具）结合使用，以自动修复常见的代码问题并提高代码质量。

7. 文档生成：

- 一些编程规范支持从代码中自动生成文档。通过遵循这些规范，可以确保生成的文档具有一致性和准确性。

8. 提高代码质量：

- 编程规范通常包括一系列最佳实践，这些实践有助于提高代码的可读性、可维护性和性能。

9. 易于传承：

- 遵循编程规范的代码更易于被新加入团队的成员理解和接手，从而加速项目的传承和交接。

10. 提升开发者技能：

- 学习和遵循编程规范可以帮助开发者提升他们的编程技能，并使他们更加熟悉行业内的最佳实践

## 二、使用 vue-cli 创建项目并配置

### 1、如何新建一个Vue3项目

1. 在桌面新建一个Vue3项目，`cmd`打开命令行工具

```shell
vue create bms-blog
```

2. Vue CLI 提供的预设配置(根据团队偏好、项目需求以及你希望实现的代码风格。)

> 上下箭头移动，空格选中/取消，回车确认

- 选择`Vue`版本
- `dart-sass`作为 CSS 预处理器
- `Babel`用来进行代码转译
- `Vue Router`用于路由管理
- `Vuex`用于状态管理
-  `ESLint`用于代码质量和格式检查

```js
? Please pick a preset: (Use arrow keys)
  n ([Vue 3] dart-sass, babel, router, vuex, eslint) 使用 Vue 3版本、CSS 预处理器、Babel 代码转译、路由、状态管理、ESLint
  Default ([Vue 3] babel, eslint) // 使用 Vue 3,其中包含Babel、ESLint
  Default ([Vue 2] babel, eslint) // 使用 Vue 2,其中包含Babel、ESLint
> Manually select features // 手动选择需要的特性，包括Vue 版本、是否使用 Babel、CSS 预处理器、路由、状态管理、ESLint 等。
```

> 通常会选择使用`Manually select features `手动选择需要的每一个特性和工具

3. 选择项目需要的特性

|特性|描|
|:--:|:--:|
|Babel|用于将现代 JavaScript 代码转换为向后兼容的 JavaScript 版本，以便更广泛的浏览器支持。|
|TypeScript|一种强类型的 JavaScript 超集，提供了更好的类型检查和代码组织方式。|
|Progressive Web App (PWA) Support|为你的应用添加渐进式网络应用支持，包括 Service Workers、Web App Manifest 等，以提供更接近原生应用的体验。|
|Router (Vue Router)|Vue Router 是 Vue.js 官方的路由管理器。|
|Vuex|Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。|
|CSS Pre-processors|CSS 预处理器如 Sass、Less 或 Stylus 可以让你使用变量、嵌套规则、混合、函数等特性编写 CSS，然后编译成普通的 CSS 文件。|
|Linter / Formatter|代码检查（Linter）和格式化（Formatter）工具可以帮助你保持代码质量、一致性和可读性。|
|Unit Testing|单元测试用于测试代码的各个部分（单元）在隔离的环境中是否按预期工作。|
|E2E Testing (End-to-End Testing)|端到端（E2E）测试模拟用户与应用的交互，确保整个应用流程按预期工作。|

```js
? Check the features needed for your project: (Press <space> to select, <a> to toggle all,
<i> to invert selection, and <enter> to proceed)
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors
>(*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

4. 选择Vue.js的版本

```js
? Choose a version of Vue.js that you want to start the project with (Use arrow keys)
> 3.x
  2.x
```

5. 是否要使用“history模式”为Vue Router

> 通常选择`n`并使用默认的"hash模式"

```js
? Use history mode for router? (Requires proper server setup for index fallback in
production) (Y/n) n
```

6. 选择CSS预处理器

|CSS预处理器|描述|
|:--:|:--:|
|Sass/SCSS (with dart-sass)|Sass是最早且最流行的CSS预处理器之一。它提供了变量、嵌套规则、混合（mixin）、函数、控制指令等特性，使得CSS编写更加可维护和易于组织。|
|Less|Less是另一个流行的CSS预处理器，它的语法与Sass类似，但有一些细微的差别。Less也提供了变量、嵌套规则、混合等功能。|
|Stylus|Stylus是一个更简洁、更富有表达力的CSS预处理器。它允许你使用缩进和空格来定义嵌套规则，而不是使用大括号和分号。Stylus也支持变量、混合、函数等特性。|

> 通常选择`Sass/SCSS (with dart-sass)`

```js
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
(Use arrow keys)
> Sass/SCSS (with dart-sass)
  Less
  Stylus
```

7. 选择ESLint的配置

> 通常选择`ESLint + Standard config`

```js
? Pick a linter / formatter config:
  ESLint with error prevention only // 仅包含错误的 ESLint
  ESLint + Airbnb config // Airbnb 的 ESLint 延伸规则
> ESLint + Standard config // 标准的 ESLint 规则
  ESLint + Prettier // Prettier来格式化代码
```

8. 选择ESLint的附加功能

> 通常选择`Lint on save`

|ESLint的附加功能|描述|
|:--:|:--:|
|Lint on save|表示每次保存文件时都会运行ESLint检查|
|Lint and fix on commit|表示在每次提交代码时都会运行ESLint检查，并尝试自动修复一些可以自动修复的问题（如缩进、空格等）。|

```js
? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert
selection, and <enter> to proceed)
>(*) Lint on save
 ( ) Lint and fix on commit
```

9. 选择Babel、ESLint等配置文件的放置位置

|配置文件的放置位置|描述|
|:--:|:--:|
|In dedicated config files|为每个工具（如Babel、ESLint等）创建一个单独的配置文件。对于Babel，这通常是一个.babelrc文件或项目根目录下的babel.config.js文件。对于ESLint，这通常是一个.eslintrc.js、.eslintrc.json或.eslintrc.yml文件（取决于你选择的配置格式）。|
|In package.json|将Babel、ESLint等工具的配置作为package.json文件中的一部分进行定义。这通常是在package.json的babel、eslintConfig等字段中完成的。|

> 通常选择`In dedicated config files`

```js
? Where do you prefer placing config for Babel, ESLint, etc.?
> In dedicated config files
  In package.json
```

10. 是否要将当前的配置保存为一个预设

是否要将当前的配置保存为一个预设，以便在未来的项目中重复使用，你需要根据你的需求来决定。

如果你发现你经常为不同的项目设置类似的配置，或者你的团队希望保持一致的配置设置，那么保存为一个预设可能是一个好主意。这样，当你开始一个新的项目时，你可以快速应用这个预设，而不需要手动配置所有的工具。

如果你只是偶尔需要这些配置，或者每个项目都有不同的需求，那么可能不需要保存为一个预设。

> 可以选择`y`，后面可以经常使用该手动好的默认配置

```js
? Save this as a preset for future projects? (y/N) y
```

11. 选择保存配置为预设时的名称，自定义

```ja
? Save preset as: bms-blog
```

12. 等待创建完成，桌面文件使用vscode打开

13. 如果需要提速安装插件等速度，可以使用淘宝镜像

```shell
npm config set registry https://registry.npm.taobao.org/
```

14. 运行项目

> 使用`npm run serve`

```shell
# 安装项目所依赖的包
npm install
# 启动一个开发服务器，运行项目
npm run serve
# 构建项目的生产版本
npm run build
```

> 下面是常用命令，小提示更换一些依赖有时需要删除`node_modules`文件夹，在项目中普通删除需要提示确认命令获取权限，可以自己使用强制删除命令

```shell
rm -rf node_modules
```

### 2、创建Vue 3项目后，常见的产生一系列文件和目录

|文件/目录|描述|
|:--:|:--:|
|node_modules|项目所依赖的第三方包（如Vue.js、Vue Router等）的存储位置。通常通过npm或yarn进行安装和管理。|
|public|存放公共资源，如index.html（项目的主页模板）、favicon.ico（网站图标）等。这些文件不会被Webpack处理，而是直接复制到dist目录中。|
|src|项目的主要源代码目录。|
|assets|存放项目所需的静态资源，如图片、字体文件、CSS文件等。这些文件会被Webpack处理并包含在最终构建的输出中。|
|components|存放Vue组件。这些组件是构建用户界面的可重用部分。|
|views|存放页面级别的Vue组件。每个页面通常对应一个文件夹，其中包含该页面的组件、样式、逻辑等。|
|router|存放Vue Router的配置文件，用于定义项目的路由规则。|
|store|（如果使用Vuex）存放Vuex状态管理相关的文件，如actions、mutations、getters和state。|
|utils|存放一些工具函数和常用方法的JavaScript文件。|
|App.vue|项目的根组件，是Vue实例的挂载点。通常包含其他组件和页面级别的布局。|
|main.js|项目的入口文件。它导入了Vue实例、App.vue组件以及可能的其他插件或库，并初始化Vue应用。|
|.browserslistrc|指定项目所支持的浏览器范围，用于Babel等前端工具进行代码转换和兼容性处理。|
|.gitignore|指定哪些文件和目录不应被Git跟踪和提交到版本控制系统。|
|package.json|项目的元数据文件和npm配置文件。它包含了项目的依赖、脚本命令、版本信息等。|
|README.md|项目的说明文档，通常包含项目的介绍、安装指南、使用说明等。|
|vue.config.js|用于修改和扩展Vue CLI项目的默认配置。例如，可以配置Webpack选项、添加新的插件等。|
|babel.config.js|Babel的配置文件，用于定义Babel的转换规则和插件。|
|tsconfig.json|（如果使用TypeScript）TypeScript的配置文件，用于定义TypeScript的编译选项和类型检查规则。|
|.prettier|通过自定义规则来重新规范项目中的代码，去掉原始的代码风格，确保团队的代码使用统一相同的格式。|

# 三、代码检测`Eslint`工具深入解析

> ESLint是一个强大的JavaScript代码检测工具，它可以帮助开发者识别和修复代码中的错误，同时确保代码风格的一致性。通过ESLint，开发者可以避免低级错误，提高代码质量，并维持团队间一致的编码风格。详细使用可以浏览查看官网地址

[ESLint 中文网](https://eslint.nodejs.cn/#google_vignette)


### 1、如果需要手动安装`eslint`配置

```shell
npm init @eslint/config
```

* `.eslintrc.js` 文件，标准的 ESLint 规则

```js
// ESLint 配置文件遵循 commonJS 的导出规则，所导出的对象就是 ESLint 的配置对象
module.exports = {
  // 表示当前目录即为根目录，ESLint 规则将被限制到该目录下
  root: true,
  // env 表示启用 ESLint 检测的环境
  env: {
    // 在 node 环境下启动 ESLint 检测
    node: true
  },
  // ESLint 中基础配置需要继承的配置
  extends: ["plugin:vue/vue3-essential", "@vue/standard"],
  // 解析器
  parserOptions: {
    parser: "babel-eslint"
  },
  // 需要修改的启用规则及其各自的错误级别
  /**
   * 错误级别分为三种：
   * "off" 或 0 - 关闭规则
   * "warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
   * "error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)
   */
   // 这种配置允许开发者在开发环境中自由地使用console和debugger，而在生产环境中则警告他们不要使用，从而避免潜在的敏感信息泄露或不必要的性能开销。
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",
    // 字符串引号不符合指定的规则时，ESLint会发出一个警告,可以用来消除error问题（实例，不是典型的解决方案）
    "quotes":"warn"
  }
};
```

### 2、代码格式化 `Prettier`使用

> Prettier是一个代码格式化工具，它可以支持多种文件格式，如JS、JSX、TS、Flow、JSON、CSS、LESS等。其主要作用是通过自定义规则来重新规范项目中的代码，去掉原始的代码风格，确保团队的代码使用统一相同的格式，是一个非常强大的代码格式化工具，可以帮助团队提高代码的可读性和可维护性。

- 代码格式化工具
- 开箱即用
- 直接集成到VScode
- 保存时，让代码直接符合ESLint

### 1、如何简单操作`Prettier`

[Prettier官网](https://www.prettier.cn/)

> 进入官网点击在线试一试，左则为规则配置项，中间为需要格式化的源代码，右侧为格式化后的效果，可以在线代码格式化

### 2、查看Prettier中文文档，使用Prettier插件

1. 使用vscode安装`Prettier`插件

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715622378868611190.png)

2. 项目中新建`.prettierrc`文件（注意是`.prettierrc`不是`.prettier`）

当你在项目中配置Prettier时，`.prettierrc` 和 `.prettierrc.json` 是两种常见的配置文件名。不过，通常Prettier并不直接支持名为`.prettier`的文件作为配置文件。它期望的配置文件名称应该具有扩展名，如.json、.yaml、.yml、.js等。

因此，当你尝试使用.prettier作为配置文件时，Prettier（或者它的编辑器插件）可能无法识别这个文件，从而导致配置不生效或报错。

```js
{
     "semi": false, // 不尾随分号
     "singleQuote": true, // 使用单引号
     "trailingComma": "none" // 多行逗号分割的语法中，最后一行不加逗号
}
```

3. VSCode设置一些功能

* 保存时格式化文件，自动化格式代码

> VSCode打开设置 -> save -> 寻找`Editor: Format On Save`

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715622689962222551.png)

* VSCode与Eslint的空格内容

> VSCode打开设置 -> tab -> 寻找`Editor: Tab Size`

VSCode而言，默认一个 tab 等于4个空格，而 ESLint希望一个tab为2个空格,所以需要改为2

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715622933718499513.png)

* VSCode含有多个代码格式化工具时，进行切换

> 鼠标右键 -> 选择使用...格式化文档 -> Prettier - Code formatter

由于采用了`Prettier`,故切换至Prettier

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715623088018259583.png)

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715623103455582431.png)

4. Eslint与Prettier冲突问题

* 解决script中方法编写的`（）`需要被缩进的问题

> demo

```js
created() {
   console.log("created")
}
// 每次save后()会向前缩进
created () {
   console.log("created")
}
```

* 在`.eslintrc.js`配置解决的规则

```js
rules: {
    'space-before-function-paren':'off'
  }
```

* 重启开发服务器

```
npm run serve
```

# 四、Git提交代码规范

## 1、git命令分之上传项目管理

[git命令分之上传项目管理](https://zhuanlan.zhihu.com/p/693476835)

## 2、Git提交代码规范

```shell
git commit -m "你的提交信息"
// 以前经常只会使用save提交
git commit -m "save" // error
```

> Git提交代码规范对于维护代码库的整洁、可读性和可维护性至关重要。以下是一些建议的Git提交代码规范：

1. 提交频率:

- 尽量保持较小的提交频率，每次提交应该只包含一个逻辑上的更改或修复。
- 避免进行大规模的、包含多个不相关更改的提交。

2. 提交信息格式

- 每个提交都应该包含一个简明扼要的提交信息。
- 使用一种固定的格式来描述提交信息，如 [类型]: 描述
- 常见的类型包括:

|常见的类型|作用|
|:--:|:--:|
|feat|新增特性或功能|
|fix|修复Bug|
|docs|文档相关的变更|
|style|代码风格的调整，如格式化、空格等|
|refactor|重构代码|
|test|增加或修改测试用例|
|chore|构建过程或辅助工具的变更|

* 修复Bug

`fix(button): 修复按钮点击无效的问题`

* 添加新功能

`feat(search): 添加搜索框及搜索功能`

* 重构代码

`refactor(home-page): 重构主页布局和样式`

* 更新文档

`docs(readme): 更新Readme文件以包含新的安装说明`

*  添加测试用例

`test(button): 添加按钮组件的单元测试`

3. 提交信息内容

- 提交信息应该描述清楚修改的内容，避免使用模糊的词汇。
- 可以提供一些上下文信息，如问题编号、参考文档等。

> 根据需要选择适合业务的约定式提交规范

|常见约定式提交规范网站|官方网站|描述|
|:--:|:--:|:--:|
|Conventional Commits|[Conventional Commits约定式提交规范链接](https://www.conventionalcommits.org/)|这是一个定义了一套提交消息规范的网站，它鼓励开发者遵循一套明确的约定，以便更好地生成自动化的更改日志和确定发行号。|
|Angular Commit Message Conventions|[Angular约定式提交规范链接](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-format)|Angular团队是最早提出并使用约定式提交规范的团队之一。他们的提交规范在Angular项目中广泛使用，并成为了许多其他项目的参考。|
|Commitizen|[Commitizen约定式提交规范链接](https://github.com/commitizen/cz-cli)|Commitizen是一个工具，它提供了一个交互式的命令行界面来指导你创建符合约定式提交规范的提交信息。它还支持多种插件，以使用不同的提交规范。|
|Semantic Release|[Semantic Release约定式提交规范链接](https://github.com/semantic-release/semantic-release)|Semantic Release是一个自动化版本管理和包发布的工具。它依赖于Git提交信息来确定新版本的类型和发布内容，因此它与约定式提交规范非常契合。|
|Standard-Version|[Standard-Version约定式提交规范链接](https://github.com/conventional-changelog/standard-version)|standard-version是一个轻量级的版本和变更日志管理工具，它基于约定式提交规范来生成变更日志并更新版本信息。|
|Commitlint|[Commitlint约定式提交规范链接](https://github.com/conventional-changelog/commitlint)|commitlint是一个用于验证提交信息的工具，它可以根据你选择的提交规范来检查提交信息是否符合规范。|
|lerna|[lerna约定式提交规范链接](https://github.com/lerna/lerna)|lerna是一个用于管理具有多个包的JavaScript项目的工具。它也支持使用约定式提交规范来生成变更日志和确定版本信息。|

4. 分支管理

- 使用分支进行开发是一个好的实践。
- 开发分支（如dev）用于进行功能开发和集成测试。
- 修复分支（如bugfix/xxx）用于解决问题和修复bug。
- 完成开发后，通过合并请求（Pull Request）将代码合并到主分支（如master或main）。

5. 代码审查

- 在提交代码之前，进行代码审查可以确保代码的质量和一致性。
- 代码审查有助于发现潜在的问题和bug，并提供有价值的反馈和建议

6. 处理合并冲突

- 在团队协作中，合并冲突是常见的情况。
- 当你的更改与他人的更改冲突时，需要手动解决冲突并重新提交代码。

7. 及时回顾和整理：

- 使用git log命令查看提交记录，以便回顾和追踪代码的历史更改。
- 如果需要修改已提交的代码，可以使用git commit --amend命令进行修改。

8. 使用自动格式化工具：

- 为了保持代码风格的一致性，可以使用自动格式化工具（如Prettier）来格式化代码。
- 在提交代码之前，确保代码已经通过了自动格式化工具的检查。


10. 提交到仓库：

- 使用git push命令将你的代码提交到远程仓库，让其他人可以访问和下载。
- 确保你的提交被推送到正确的分支上。

## 3、Commitizen

> Commitizen是一个工具，它提供了一个交互式的命令行界面来指导你创建符合约定式提交规范的提交信息。它还支持多种插件，以使用不同的提交规范。

[Commitizen约定式提交规范链接](https://github.com/commitizen/cz-cli)

1. 全局安装`Commitizen`

> 避免管理员权限问题,这个最好在有管理员权限问题下面安装

`C:\Windows\system32>`

```shell
npm install -g commitizen@4.2.4
```

2. 项目中安装并配置`cz-customizable`插件

* 安装`cz-customizable`插件

```shell
npm i cz-customizable@6.3.0 --save-dev
```

如果安装是解析依赖关系时遇到了冲突，使用--legacy-peer-deps选项,尝试使用--legacy-peer-deps选项来忽略peer依赖冲突。这个选项告诉npm使用旧版的依赖解析策略，这可能会忽略某些peer依赖冲突。

```shell
npm install cz-customizable@6.3.0 --save-dev --legacy-peer-deps
```

* package.json中配置`cz-customizable`

```json
"config": {
    "commitizen":{
      "path":"node_modules/cz-customizable"
    }
  },
```

* 新建`.cz-config.js`自定义提示文件

```js
module.exports = {
  // 可选类型
  types: [
    { value: 'feat', name: 'feat:     新功能' },
    { value: 'fix', name: 'fix:      修复' },
    { value: 'docs', name: 'docs:     文档变更' },
    { value: 'style', name: 'style:    代码格式(不影响代码运行的变动)' },
    {
      value: 'refactor',
      name: 'refactor: 重构(既不是增加feature，也不是修复bug)'
    },
    { value: 'perf', name: 'perf:     性能优化' },
    { value: 'test', name: 'test:     增加测试' },
    { value: 'chore', name: 'chore:    构建过程或辅助工具的变动' },
    { value: 'revert', name: 'revert:   回退' },
    { value: 'build', name: 'build:    打包' }
  ],
  // 消息步骤
  messages: {
    type: '请选择提交类型:',
    customScope: '请输入修改范围(可选):',
    subject: '请简要描述提交(必填):',
    body: '请输入详细描述(可选):',
    footer: '请输入要关闭的issue(可选):',
    confirmCommit: '确认使用以上信息提交？(y/n/e/h)'
  },
  // 跳过问题
  skipQuestions: ['body', 'footer'],
  // subject文字长度默认是72
  subjectLimit: 72
}
```

* 使用`git cz`代替`git commit`

3. git cz提交信息

```shell
git add .
git cz
git config --global user.name <user.name>
git config --global user.email <user.email>
git checkout -b main 
git branch    
git remote add origin <存放的git项目url>
git push -u origin main
```

## 4、Git Hooks

1. 什么是Git Hooks

Git Hooks是Git的一个重要特性，它允许在Git仓库中定义一些自动化的脚本，这些脚本会在特定的Git事件（如提交代码、接收代码等）发生时被触发执行。这些脚本本质上就是可执行的程序，可以用任何你喜欢的脚本语言来编写（如Bash、Python、Node.js等），只要该语言在你的系统环境中可执行即可。

|Git Hook |调用时机 |说明|
|:--:|:--:|:--:|
| pre-applypatch | `git am`执行前 ||
| applypatch-msg | `git am`执行前 ||
| post-applypatch | `git am`执行后 | 不影响`git am`的结果 |
| **pre-commit** | `git commit`执行前 | 可以用`git commit --no-verify`绕过 |
| **commit-msg** | `git commit`执行前 | 可以用`git commit --no-verify`绕过  |
| post-commit  | `git commit`执行后| 不影响`git commit`的结果 |
| pre-merge-commit | `git merge`执行前| 可以用`git merge --no-verify`绕过。 |
| prepare-commit-msg  | `git commit`执行后，编辑器打开之前 | |
| pre-rebase | `git rebase`执行前 ||
| post-checkout   | `git checkout`或`git switch`执行后  | 如果不使用`--no-checkout`参数，则在`git clone`之后也会执行。 |
| post-merge  | `git commit`执行后  | 在执行`git pull`时也会被调用 |
| pre-push   | `git push`执行前   |  |
| pre-receive | `git-receive-pack`执行前 |  |
| update  | | |
| post-receive | `git-receive-pack`执行后 | 不影响`git-receive-pack`的结果 |
| post-update | 当 `git-receive-pack`对 `git push` 作出反应并更新仓库中的引用时 | |
| push-to-checkout| 当`git-receive-pack`对`git push`做出反应并更新仓库中的引用时，以及当推送试图更新当前被签出的分支且`receive.denyCurrentBranch`配置被设置为`updateInstead`时 |  |
| pre-auto-gc  | `git gc --auto`执行前 | |
| post-rewrite  | 执行`git commit --amend`或`git rebase`时 ||
| sendemail-validate  | `git send-email`执行前 | |
| fsmonitor-watchman    | 配置`core.fsmonitor`被设置为`.git/hooks/fsmonitor-watchman`或`.git/hooks/fsmonitor-watchmanv2`时 ||
| p4-pre-submit | `git-p4 submit`执行前| 可以用`git-p4 submit --no-verify`绕过  |
| p4-prepare-changelist | `git-p4 submit`执行后，编辑器启动前  | 可以用`git-p4 submit --no-verify`绕过   |
| p4-changelist | `git-p4 submit`执行并编辑完`changelist message`后       | 可以用`git-p4 submit --no-verify`绕过 |
| p4-post-changelist    | `git-p4 submit`执行后 |  |
| post-index-change     | 索引被写入到`read-cache.c do_write_locked_index`后||

2. 在实际开发中，最常用的两个钩子

|Git Hook |调用时机 |说明|
|:--:|:--:|:--:|
| **pre-commit** | `git commit`执行前<br />它不接受任何参数，并且在获取提交日志消息并进行提交之前被调用。脚本`git commit`以非零状态退出会导致命令在创建提交之前中止。 | 可以用`git commit --no-verify`绕过 |
| **commit-msg** | `git commit`执行前<br />可用于将消息规范化为某种项目标准格式。<br />还可用于在检查消息文件后拒绝提交。 | 可以用`git commit --no-verify`绕过 |

- `commit-msg`：可以用来规范提交信息的标准格式，并且按需指定是否要拒绝本次提交。
- `pre-commit`：在提交前被调用，可以按需指定是否要拒绝本次提交。

## 5、使用`husky` + `commitlint`检查提交描述是否符合规范要求

1. `npm`需要在7.X以上版本

* 查看npm 版本

```shell
npm -v
```

* 更新 npm 到最新版本

```shell
npm install -g npm@latest
```

*  指定 npm 的版本

```shell
npm install -g npm@8.19.3
```

2. 安装`commitlint`工具

* 安装依赖

```shell
npm install --save-dev @commitlint/config-conventional@12.1.4 @commitlint/cli@12.1.4
```

* 可能出现的兼容问题，需要降级 eslint-plugin-vue

> 这个会与安装的工具产生冲突`@vue/eslint-config-standard@6.1.0` 需要一个与 eslint-plugin-vue 版本 ^7.0.0 兼容的版本，但是你当前的项目中已经安装了 eslint-plugin-vue@8.7.1。

```shell
npm install eslint-plugin-vue@7 --save-dev
```

* 新建`commitlint.config.js`文件

```js
module.exports = {
  // 继承的规则
  extends: ['@commitlint/config-conventional'],
  // 定义规则类型
  rules: {
    // type 类型定义，表示 git 提交的 type 必须在以下类型范围内
    'type-enum': [
      // 当前验证的错误级别
      2,
      //  在什么情况下进行验证
      'always',
      //  泛型类容
      [
        'feat', // 新功能 feature
        'fix', // 修复 bug
        'docs', // 文档注释
        'style', // 代码格式(不影响代码运行的变动)
        'refactor', // 重构(既不增加新功能，也不是修复bug)
        'perf', // 性能优化
        'test', // 增加测试
        'chore', // 构建过程或辅助工具的变动
        'revert', // 回退
        'build' // 打包
      ]
    ],
    // subject 大小写不做校验
    'subject-case': [0]
  }
}
```

* 需要使用`UTF-8`编程格式

> Vscode -> 右下角选择编码

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715672516496426531.png)

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715672537854710534.png)

3. 安装`husky`工具

* 安装依赖：

```shell
npm install husky@7.0.1 --save-dev
```

* 启动 `hooks` ， 生成 `.husky` 文件夹

```shell
npx husky install
```

* 在 `package.json` 中生成 `prepare` 指令

```shell
npm set-script prepare "husky install"
```

> 或者手动添加

```json
"prepare": "husky install"
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715672878131991638.png)

* 执行 `prepare` 指令

```shell
npm run prepare
```

* 执行成功，提示

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240514/1715672969214472727.png)

* 添加 `commitlint` 的 `hook` 到 `husky`中，并指令在 `commit-msg` 的 `hooks` 下执行 `npx --no-install commitlint --edit "$1"` 指令

```shell
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

4. 不符合规范的 commit 将不再可提交

> 强制规范化的提交要求

## 6、通过 pre-commit 检测提交时代码规范

> `ESLint` 与 `Prettier` 配合解决代码格式问题,通过 pre-commit 钩子来检测提交时的代码规范是一个很好的实践，它可以在代码被推送到远程仓库之前确保代码符合团队的代码风格和质量要求。

* 通过 `husky` 监测 `pre-commit` 钩子，在该钩子下执行 `npx eslint --ext .js,.vue src`** 指令来去进行相关检测

```shell
npx husky add .husky/pre-commit "npx eslint --ext .js,.vue src"
```

## 7、lint-staged 自动修复格式错误

> lint-staged 是一个在 Git 暂存区文件上运行 linters 的工具。它允许你只对 Git 暂存区中的更改运行 linting 和可能的自动修复，而不是对整个项目运行。这对于保持代码库清洁和一致非常有用。


* `package.json` 配置`lint-staged`

> 如上配置，每次它只会在你本地 `commit` 之前，校验你提交的内容是否符合你本地配置的 `eslint`规则

1. 如果符合规则：则会提交成功。

2. 如果不符合规则：它会自动执行 `eslint --fix` 尝试帮你自动修复，如果修复成功则会帮你把修复好的代码提交，如果失败，则会提示你错误，让你修好这个错误之后才能允许你提交代码。

```js
   "lint-staged": {
       "src/**/*.{js,vue}": [
         "eslint --fix",
         "git add"
       ]
     }
```

3. 修改 `.husky/pre-commit` 文件

```js
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```
