---
title: vue3+vite2构建项目
date: 2022-08-22 10:04:13
tags:
  - vue
  - vite
categories: 'vue'

---

### 1. sass 

```
npm install --save-dev sass
```

### 2. 获取浏览器兼容

```
npm i @vitejs/plugin-legacy --save-dev   获取传统浏览器对scrapt标签的支持


import legacy from '@vitejs/plugin-legacy'

export default {
  plugins: [
    legacy({
      targets: ['chrome 52', 'not IE 11'],
      additionalLegacyPolyfills: ['regenerator-runtime/runtime']
    })
  ]
}
```

### 3. postcss.config.js

postcss 常用插件

- autoprefixer是PostCSS最著名的一款插件，就不过多介绍了，相信同学们都使用过
- postcss-cssnext (内置autoprefixer) 允许你使用未来的css语法，如css4（可以理解为css中的Babel）
- postcss-sprites 自动制作雪碧图，不用手动拼接啦，哈哈哈
- cssnano 压缩css代码(如果你是用webpack的话，css-loader集成了cssnano，你不需要再次引入)
- postcss-hash-classname 把转换后的css文件名附上哈希值
- pixrem 将rem转换为px
- postcss-px-to-viewport 将px转换为vh和vw（推荐作为移动端的计量单位，而不是rem）
- postcss-pxtorem 将px转换为rem

```javascript
- npm install postcss postcss-pxtorem --save-dev    px生成css
- npm install --save -dev autoprefixer          处理CSS前缀问题的神器

// postcss.config.js
module.exports = {
  plugins: {
    autoprefixer: {
      overrideBrowserslist: [
        'Android 4.1',
        'iOS 7.1',
        'Chrome > 31',
        'ff > 31',
        'ie >= 8',
        'last 10 versions' // 所有主流浏览器最近10版本用
      ],
      grid: true
    },
    'postcss-pxtorem': {
      rootValue: 14,
      propList: ['*'],
      unitPrecision: 2,
      // 'video-js'
      selectorBlackList: ['echarts_', 'lds-ripple-spinner']
    }
  }
}

```

### 4. eslist+prettier 

于 vetur 完全支持了 prettier 的格式化风格，所以 vue 提倡使用 prettier 而不是 eslint 来格式化代码。为了解决 eslint 规则和 prettier 规则的冲突，就要用到 eslint-config-prettier 依赖包。
使用 eslint-config-prettier 禁用所有与格式化相关的 ESLint 规则后，需要在根目录下新建 prettier 的配置文件，本文创建的是 prettier.config.js 文件。在 .eslintrc.js 文件的 extends 中，通过 plugin:prettier/recommended 属性，将 prettier.config.js 文件中的配置合并到 .eslintrc.js 文件中。需要注意的是，plugin:prettier/recommended 必须作为你的最后一个扩展。
在 vite 配置文件中，同一属性下，后引入的规则会覆盖前面的规则。

#### 4.1 推荐的依赖包

- eslint
- [eslint-plugin-vue](https://eslint.vuejs.org/)：Vue.js 的官方 ESLint 插件，它允许我们使用 ESLint 检查文件中的 Vue 代码。
- [vue-eslint-parser](https://www.npmjs.com/package/vue-eslint-parser)：文件的 ESLint 自定义解析器.vue。
- [prettier](https://www.npmjs.com/package/prettier)：代码格式化程序
- [eslint-plugin-prettier](https://www.npmjs.com/package/eslint-plugin-prettier)：
  - 将 Prettier 作为 ESLint 规则运行。
  - 如果你想禁用与代码格式相关的所有其他 ESLint 规则，并且仅启用检测潜在错误的规则，则此插件效果最佳。
  - 如果你安装了 eslint 那么你应该会遇到 eslint 规则和 prettier 规则冲突。可用 eslint-config-prettier 解决 eslint 规则和 prettier 规则的冲突。
- [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)：关闭所有不必要或可能与 Prettier 冲突的规则。

```
npm install --save-dev eslint
npm install --save-dev eslint-plugin-vue vue-eslint-parser
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
```

#### 4.2 步骤

1.  配置.eslintrc.js

- npx eslint --init

![image-20220818110401364](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220818110401364.png)

- vue的官方eslint插件配置: https://eslint.vuejs.org/rules/

```
extends: ["eslint:recommended", "plugin:vue/vue3-recommended"],
parser: "vue-eslint-parser",
```

- eslint官方rules配置规则：http://eslint.cn/docs/rules/

2. 配置.eslintignore

```
*.sh
*.md
*.woff
*.ttf
.vscode
.idea
.husky
.local
dist
node_modules
Dockerfile
/public
/docs
/bin
```

3. 配置

```
module.exports = {
  printWidth: 100, // 最大行长规则通常设置为 100 或 120。
  tabWidth: 2, // 指定每个标签缩进级别的空格数。
  useTabs: false, // 使用制表符而不是空格缩进行。
  semi: false, // true（默认）: 在每条语句的末尾添加一个分号。false：仅在可能导致 ASI 失败的行的开头添加分号。
  vueIndentScriptAndStyle: true, // Vue 文件脚本和样式标签缩进
  singleQuote: true, // 使用单引号而不是双引号
  quoteProps: 'as-needed', // 引用对象中的属性时，仅在需要时在对象属性周围添加引号。
  bracketSpacing: true, // 在对象文字中的括号之间打印空格。
  trailingComma: 'none', // "none":没有尾随逗号。"es5": 在 ES5 中有效的尾随逗号（对象、数组等），TypeScript 中的类型参数中没有尾随逗号。"all"- 尽可能使用尾随逗号。
  bracketSameLine: false, // 将>多行 HTML（HTML、JSX、Vue、Angular）元素放在最后一行的末尾，而不是单独放在下一行（不适用于自闭合元素）。
  jsxSingleQuote: false, // 在 JSX 中使用单引号而不是双引号。
  arrowParens: 'always', // 在唯一的箭头函数参数周围始终包含括号。
  insertPragma: false, // 插入编译指示
  requirePragma: false, // 需要编译指示
  proseWrap: 'never', // 如果散文超过打印宽度，则换行
  htmlWhitespaceSensitivity: 'strict', // 所有标签周围的空格（或缺少空格）被认为是重要的。
  endOfLine: 'lf', // 确保在文本文件中仅使用 ( \n)换行，常见于 Linux 和 macOS 以及 git repos 内部。
  rangeStart: 0, // 格式化文件时，回到包含所选语句的第一行的开头。
};
```

4. 配置 .prettierignore ：https://prettier.io/docs/en/options.html

```
/dist/*
/public/*
/node_modules/**
.local
.output.js
**/*.svg
**/*.sh
```

### 5. 其他

```
// 进度条
npm install --save nprogress
```

【参考文献】

https://blog.csdn.net/mChales_Liu/article/details/124324862

https://juejin.cn/post/7043702363156119565
