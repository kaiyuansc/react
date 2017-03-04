---
id: hello-world
title: Hello World
permalink: docs/hello-world.html
prev: installation.html
next: introducing-jsx.html
redirect_from:
  - "docs/index.html"
  - "docs/getting-started.html"
  - "docs/getting-started-ko-KR.html"
  - "docs/getting-started-zh-CN.html"
---

开始学习React的最简单方法就使用[this Hello World example code on CodePen](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)。你不需要安装任何东西，你只需要在新标签页中打开它并跟着做。如果你想要使用本地开发环境，查看[安装](react/docs/installation)页。
The easiest way to get started with React is to use [this Hello World example code on CodePen](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010). You don't need to install anything; you can just open it in another tab and follow along as we go through examples. If you'd rather use a local development environment, check out the [Installation](/react/docs/installation.html) page.

React中最小的例子如下:

The smallest React example looks like this:

```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

它会在页面上渲染一个“Hello, world!”标题。

It renders a header saying "Hello, world!" on the page.

后面的章节将逐步地向你介绍如何使用 React。我们将检查 React apps 的创建模块：元素和组件。一旦你精通它们，你就能从小的可重用的部件开始创建复杂的 apps 了。

The next few sections will gradually introduce you to using React. We will examine the building blocks of React apps: elements and components. Once you master them, you can create complex apps from small reusable pieces.

## 关于 JavaScript 的注释

React是一个JavaScript库，所以它假设你已经有JavaScript语言的基础了。如果你没有足够的信心，我们建议[加强你的JavaScript基础](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)，以便顺利学习。

React is a JavaScript library, and so it assumes you have a basic understanding of the JavaScript language. If you don't feel very confident, we recommend [refreshing your JavaScript knowledge](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) so you can follow along more easily.

在例子中我们用了一些ES6的语法。我们试着少使用ES6语法因为它仍然比较新，但是我们鼓励你去熟悉 [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), 和 [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 声明。你能用 Babel REPL 检验从ES6代码编译过去的代码。

We also use some of the ES6 syntax in the examples. We try to use it sparingly because it's still relatively new, but we encourage you to get familiar with [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) statements. You can use <a href="http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A">Babel REPL</a> to check what ES6 code compiles to.
