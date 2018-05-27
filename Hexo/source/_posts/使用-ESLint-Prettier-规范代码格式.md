---
title: 使用 ESLint && Prettier 规范代码格式
date: 2018-05-15 15:08:18
tags: [vscode]
categories: 编程工具
e_title: format-code-by-eslint-and-prettier
---

开始配置之前，先问个问题：[ESLint](https://eslint.org/) 和 [Prettier](https://prettier.io/) 有什么不同？

之所以问这个问题是因为 ESLint 除了代码质量检查（比如有无声明但没有使用的变量）也提供了自动格式化代码的功能，比如，将双引号变为单引号，自定添加 trailing comma 等，既然如此，那干嘛还要再引入 Prettier？毕竟，Prettier 就是用于代码格式化的。

其实，在格式化代码方面， Prettier 确实和 ESLint 有重叠，但他们侧重点不同：ESLint 主要工作就是检查代码质量并给出提示，它所能提供的格式化功能很有限；而 Prettier 在格式化代码方面就强大的多。

> 关于 ESLint 可修复的 style 类目参考这个列表下标有扳手标记的项 [Stylistic Issues](https://eslint.org/docs/rules/#stylistic-issues)

## 举个例子：

```js
methods: {
  async testURL() {
    let TEST = 11;
  },

},
```

在上面的例子中，

ESLint 可以

* 检查

1）TEST 是常量，应该用 const 声明；
2）TEST 声明后没有使用。

* 自动修改

1）将 let 声明改为 const 声明。

但 ESLint 也只能做什么多了，但倒数第二个空行，ESLint 就无能为力了。这时候， Prettier 就登场了：Prettier 可以自动删除倒数第二行的空行。

## 使用

因为 ESLint 和 Prettier 都可以格式化代码，如果他们对格式化代码执行不同规则，那就可能发生冲突，可以通过配置解决大部分冲突，但仍有一些是无法解决的，比如，Prettier 在 `function` 关键字后不允许有空格且不能自定义，那如果想避免 ESLint 不报错，只能配置 ESLint 允许 `function` 关键字后不带空格（主要针对匿名函数）。

这类不能配置的冲突很少，目前我只发现一例，所以，还是选择继续使用 Prettier，毕竟，更多的时候使用 Prettier 很方便，并且，后续 Prettier 版本可能会添加更多自定义选项。

> 关于 `function` 关键字后不带空格更多讨论可参考 [Space after function keyword - MOVED to #3847!](https://github.com/prettier/prettier/issues/1139)

### 安装依赖

* eslint 相关

这里使用 airbnb 代码规范，同时安装了对 react 和 vue 支持的插件

```bash
yarn add
  eslint babel-core babel-eslint eslint-config-airbnb eslint-plugin-import
  eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-vue eslint-plugin-html
-D
```

* prettier 相关

```bash
yarn add eslint-config-prettier eslint-plugin-prettier prettier -D
```

### 编写配置文件

配置 `.eslintrc.js`

```js
module.exports = {
  parser: 'babel-eslint',
  env: {
    browser: true,
    node: true,
  },

  // extending airbnb config and config derived from eslint-config-prettier
  extends: ['airbnb', 'prettier'],

  // activating esling-plugin-prettier (--fix stuff)
  plugins: ['prettier'],

  // 自定义 eslint 检查规则
  rules: {
    // 自定义 prettier 规则 (实际上，可配置项非常有限)
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        trailingComma: 'all',
      },
    ],

    // 其他 eslint 检查规则
    camelcase: 'off', // 强制驼峰法命名
    'no-new': 'off', // 禁止在使用 new 构造一个实例后不赋值
    'space-before-function-paren': 'off', // 函数定义时括号前面不要有空格
    'no-plusplus': 'off', // 禁止使用 ++， ——
    'max-len': 'off', // 字符串最大长度
    'func-names': 'off', // 函数表达式必须有名字
  },
};
```

需要特别注意，在使用 Prettier 的时候，ESLint 自定义规则不能和 Prettier 的重复，不然会重复格式化，比如，如果在 ESLint 自定义规则中添加了下面内容会导致格式化时多出一个分号和逗号：

```js
// 加多这两个自定义规则，导致重复格式化
semi: ['error', 'always'], // 语句强制分号结尾
'comma-dangle': ['error', 'always-multiline'], // 多行对象字面量项尾总是有逗号
```

> 因为自定义规则优先级更高？

### 编辑器配置

* vscode

安装插件：

因为我们的目的是根据 ESLint 规则自动格式化代码，这只需在 `preference -> setting -> user setting` 中添加下面配置即可：

```js
// 点击保存时，根据 eslint 规则自定修复
"eslint.autoFixOnSave": true,
```

* atom

安装插件：

[prettier-atom](https://atom.io/packages/prettier-atom)：格式化代码；  
[atom-beautify](https://atom.io/packages/atom-beautify)：格式化代码（以 prettier 为主，没有 prettier 时用，同时在 setting 设置 vue 的格式化插件为 prettier-atom）；  
[linter-eslint](https://atom.io/packages/linter-eslint)：js 检查，可设置和 prettier 配合使用（还有其他相关依赖，atom 会提示安装）

> 最好手动通过 eslint 进行格式化

## 其他

### 添加对 Vue/React 的支持

* 编辑器更改

安装 VS code 插件 [vetur](https://github.com/vuejs/vetur)（格式化 Vue），同时编辑器 setting 中添加下面配置自动格式化

```js
// eslint 对 vue 支持
"eslint.validate": [
  "javascript",
  // 这里是 react 支持
  "javascriptreact",
  {
    "language": "vue",
    "autoFix": true
  }
],

// 默认 html 没有格式化插件，添加 js-beautify-html
// 官方说不久后不再支持 js-beautify-html
"vetur.format.defaultFormatter.html": "js-beautify-html",
```

> 注意，这里没有开启编辑器保存自动格式化（`"editor.formatOnSave": false`），因为可能和 ESLint 规则冲突，如果需要格式化 html 部分，手动选中格式化（如果编写 markdown 什么的，那就坑了，都得手动格式化。也好了，自己要知道怎么写才规范，然后自定格式化作为辅助）。

* ESLint 配置文件更改

在 ESLint plugins 添加 Vue 和 React 选项：

```js
// vue
extends: ['plugin:vue/essential', 'airbnb-base', 'prettier'],
plugins: ['prettier', 'vue'],

// react
extends: ['airbnb-base', 'prettier'],
plugins: ['prettier', 'react'],
```

如果上面配置完 react 还有问题，加多下面配置：

```js
"plugins": ["react", "jsx-a11y", "import"],
"rules": {
  // 如果在项目中文件名后缀是 .js 而不是 .jsx 避免报错
  "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
  // props validation 错误
  "react/prop-types": 0,
}
```

### 完整配置参考

* .eslintrc.js 配置

```js
module.exports = {
  root: true,
  parserOptions: {
    parser: 'babel-eslint',
  },
  env: {
    browser: true,
  },
  // vue
  extends: ['plugin:vue/essential', 'airbnb-base', 'prettier'],
  plugins: ['prettier', 'vue'],

  // react
  // extends: ['airbnb-base', 'prettier'],
  // plugins: ['prettier', 'react'],

  // add your custom rules here
  rules: {
    // customizing prettier rules (unfortunately not many of them are customizable)
    'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        trailingComma: 'all',
      },
    ],
    camelcase: 'off', // 强制驼峰法命名
    'no-new': 'off', // 禁止在使用new构造一个实例后不赋值
    'space-before-function-paren': 'off', // 函数定义时括号前面不要有空格
    'no-plusplus': 'off', // 禁止使用 ++， ——
    'max-len': 'off', // 字符串最大长度
    'func-names': 'off', // 函数表达式必须有名字
    'no-param-reassign': 'off', // 不准给函数入参赋值
    // react 如果在项目中文件名后缀是 .js 而不是 .jsx 避免报错
    // "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
    // react props validation 错误
    // "react/prop-types": 0,
  },
};
```

* 编辑器完整配置

```js
{
  // 样式相关
  "workbench.iconTheme": "material-icon-theme",
  "workbench.colorTheme": "Atom One Dark",
  "editor.fontFamily": "Monaco, 'Courier New', monospace",
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.detectIndentation": true,
  "workbench.statusBar.visible": true,
  "workbench.activityBar.visible": false,
  "editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?",
  "atomKeymap.promptV3Features": true,
  "editor.multiCursorModifier": "ctrlCmd",
  "explorer.confirmDragAndDrop": false,
  "vetur.validation.template": false,
  "workbench.startupEditor": "newUntitledFile",
  "search.followSymlinks": false,
  "editor.wordWrap": "on",
  "window.zoomLevel": 0,
  // 禁止 CommonJS module 提示报错，JS 检查用 eslint 就好了
  "javascript.suggestionActions.enabled": false,
  // Show Errors & Warnings on files and folder.
  "problems.decorations.enabled": false,
  // 关闭默认 js 检查
  "javascript.format.enable": false,
  // 文件末尾插入新行
  "files.insertFinalNewline": true,

  // markdownlint config object
  "markdownlint.config": {
    // 长度限制
    "MD013": false,
    // list 后必须有一个空格（和prettier自动格式化有冲突）
    "MD030": false,
    // 开头必须 h1
    "MD002": false,
    "MD041": false,
    // 有序列表必须顺序排列并且中间不能有内容
    "MD029": false,
    // 不允许出现相同名字的标题，但不同层级应该允许
    "MD024": false,
  },

  // 粘贴自动格式化，保存不自动格式化
  "editor.formatOnPaste": true,
  "editor.formatOnSave": false,

  // 开启 eslint 支持
  "prettier.eslintIntegration": true,
  "eslint.autoFixOnSave": true,

  // 添加 vue/react 支持
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
        "language": "vue",
        "autoFix": true
    }
  ],
  "vetur.format.defaultFormatter.html": "js-beautify-html",
}
```

## 参考资料

[Your last ESLint config](https://medium.com/@netczuk/your-last-eslint-config-9e35bace2f99)
