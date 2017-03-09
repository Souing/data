# bable

Babel是一个通用的多用途JavaScript编译器

例如,Babel能够将新的ES6箭头函数语法：

```
const double = n => n * n;

```

转译为：

```
const double = function square(n) {
  return n * n;
};

```

## 安装Babel

Babel的CLI是一种在命令行下使用Babel编译文件的简单方法 全局安装

```
npm install --global babel-cli

```

## 命令行下使用Babel

这将把编译后的结果直接输出至终端

```
$ babel es6.js

```

加上`-o`参数 可以将结果写入到指定的文件

```
$ babel es6.js -o es5.js

```

把一个目录整个编译成一个新的目录

```
$ babel src -d lib

```

## 配置 Babel

Babel是一个通用编译器，因此默认情况下它反而什么都不做,你必须明确地告诉Babel应该要做什么 在项目根目录下创建.babelrc文件,这是用来让*Babel*做你要它做的事情的配置文件

```
{
  "presets": [],
  "plugins": []
}

```

## babel-preset-es2015

把ES6编译成 ES5

安装依赖的预设

```
$ npm install --save-dev babel-preset-es2015

```

修改.babelrc

```
{
    "presets": [
+     "es2015"
    ],
    "plugins": []
  }

```

## babel-preset-stage-x ES7提案

JavaScript还有一些提案还没有完全确定,分为5个阶段

1. babel-preset-stage-0
2. babel-preset-stage-1
3. babel-preset-stage-2
4. babel-preset-stage-3

### 安装依赖模块

```
npm install --save-dev babel-preset-stage-2

```

配置.babelrc

```
 "presets": [
+    "stage-2"
 ]

```

> 以上每种预设都依赖于紧随的后期阶段预设。例如，babel-preset-stage-1 依赖 babel-preset-stage-2，后者又依赖 babel-preset-stage-3。

## babel-polyfill

执行`Babel`生成的代码 Babel默认只转换新的JavaScript句法，而不转换新的API

```
function addAll() {
  return Array.from(arguments).reduce((a, b) => a + b);
}
function addAll() {
  return Array.from(arguments).reduce(function(a, b) {
    return a + b;
  });
}
```

为了解决这个问题，我们使用一种叫做Polyfill(兼容性补丁)的技术

babel-polyfill' 为了解决ES6 babel（只转换js语法）不转换新API。

```
$ npm install --save babel-polyfill
$ import "babel-polyfill";

```

## 资源链接

- [https://babeljs.io/](https://babeljs.io/)
- [https://www.npmjs.com/package/babel-core](https://www.npmjs.com/package/babel-core)
- [https://github.com/thejameskyle/babel-handbook/blob/master/translations/en/user-handbook.md](https://github.com/thejameskyle/babel-handbook/blob/master/translations/en/user-handbook.md)
- https://wakeupmypig.github.io/jw_blog/html/自动化工具/babel.html
- https://zhufengnodejs.github.io/doc/html/node课程/3.babel.html

