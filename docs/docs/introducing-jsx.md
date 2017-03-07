---
id: introducing-jsx
title: 介绍 JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

考虑这个变量声明：

Consider this variable declaration:

```js
const element = <h1>Hello, world!</h1>;
```

这个奇怪的标签语法既不是字符串也不是HTML。

This funny tag syntax is neither a string nor HTML.

它被称作 JSX，是 JavaScript 的一个语法扩展。我们推荐在 React 中使用它像这样描述UI。JSX 可能会让你想起一个模板语言，但是它完全来源于 JavaScript。

It is called JSX, and it is a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like. JSX may remind you of a template language, but it comes with the full power of JavaScript.

JSX 产生 React “元素”。我们将在[下一章节](/react/docs/rendering-elements.html)中探究如何把它们渲染进 DOM。下面，你能找到 JSX 入门的必须基础知识。

JSX produces React "elements". We will explore rendering them to the DOM in the [next section](/react/docs/rendering-elements.html). Below, you can find the basics of JSX necessary to get you started.

### 在 JSX 中嵌入表达式

你能在 JSX 中通过大括号包裹它的形式嵌入任何 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)

You can embed any [JavaScript expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) in JSX by wrapping it in curly braces.

例如， `2 + 2`, `user.firstName`, 和 `formatName(user)` 都是合法的表达式：

For example, `2 + 2`, `user.firstName`, and `formatName(user)` are all valid expressions:

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)

为了可读性，我们将 JSX 分成多行。这么做尽管不是必须，我们同样推荐用一个括号包裹它以避免自动分号插入的陷阱。

We split JSX over multiple lines for readability. While it isn't required, when doing this, we also recommend wrapping it in parentheses to avoid the pitfalls of [automatic semicolon insertion](http://stackoverflow.com/q/2846283).

### JSX 也是一个表达式

编译后，JSX 表达式变成一个正常的 JavaScript 对象。

After compilation, JSX expressions become regular JavaScript objects.

这表明你能在 `if` 状态和 `for` 循环中使用 JSX，把它赋值给变量，以参数的形式接收它，从函数中返回它。

This means that you can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### 为 JSX 指定属性

你可以使用引号指定字符串字面量来作为属性：

You may use quotes to specify string literals as attributes:

```js
const element = <div tabIndex="0"></div>;
```

你也可以使用大括号来嵌入一个 JavaScript 表达式作为属性：

You may also use curly braces to embed a JavaScript expression in an attribute:

```js
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入一个 JavaScript 表达式时不要用引号包裹大括号，否则 JSX 会认为这个属性是一个字符串字面量而不是一个表达式。你应该用引号来表示字符串值或用大括号表示表达式，不能再同一个属性中同时使用它们。

Don't put quotes around curly braces when embedding a JavaScript expression in an attribute. Otherwise JSX will treat the attribute as a string literal rather than an expression. You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute.

### 为 JSX 指定子元素

如果一个标签是空标签，你可以直接用 `/>` 关闭它，就像 XML 一样：

If a tag is empty, you may close it immediately with `/>`, like XML:

```js
const element = <img src={user.avatarUrl} />;
```

JSX 标签可以包含子元素：

JSX tags may contain children:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

>**警告：**
>
>Since JSX is closer to JavaScript than HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names.
>
>For example, `class` becomes [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) in JSX, and `tabindex` becomes [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### JSX 阻止注入攻击

在 JSX 中嵌入用户输入是安全的

It is safe to embed user input in JSX:

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

默认情况下，React DOM 在渲染他们前会 [escapes](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) 嵌入到 JSX 中的任何值。这样它就能确保你的程序不会被注入任何不明确的内容。所有东西会在渲染前被转化成字符串，这能帮你阻止 [XSS(cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) 攻击。

By default, React DOM [escapes](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that's not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.

### JSX 代表对象

Babel compiles JSX down to `React.createElement()` calls.

这两个例子是一样的：

These two examples are identical:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

这些对象被称作 “React 元素”。你可以把它们想象成你需要在屏幕上看到的界面的描述。React 读取这些对象，使用它们构造 DOM 并保持最新状态。

These objects are called "React elements". You can think of them as descriptions of what you want to see on the screen. React reads these objects and uses them to construct the DOM and keep it up to date.

我们将在下一部分探究把 React 元素渲染进 DOM。

We will explore rendering React elements to the DOM in the next section.

>**提示：**
>
>We recommend searching for a "Babel" syntax scheme for your editor of choice so that both ES6 and JSX code is properly highlighted.
