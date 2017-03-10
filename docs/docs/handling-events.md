---
id: handling-events
title: 处理事件
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

用 React 元素与 DOM 元素在处理事件上很相似。它们有一些语法上的不同：

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:

* React 事件用驼峰命名法来命名，而不是都用小写。React events are named using camelCase, rather than lowercase.
* 通过 JSX，你传递一个函数来处理事件，而不是一个字符串。With JSX you pass a function as the event handler, rather than a string.

例如，这个 HTML：

For example, the HTML:

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

与 React 中有点不同：

is slightly different in React:

```js{1}
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

React中的另一个不同就是你不能返回 false 来阻止默认行为。你必须明确地调用 preventDefault。例如，普通的 HTML 中为了阻止链接打开新页面的默认行为，你可以这样写：

Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly. For example, with plain HTML, to prevent the default link behavior of opening a new page, you can write:

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

React 中，你可以这么写来代替：

In React, this could instead be:

```js{2-5,8}
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

这里，e 是一个虚拟的事件。React 根据 W3C spec 来定义这些 虚拟事件，所以你不用担心跨平台的兼容性。你可以看 SyntheticEvent 相关向导来深入学习。

Here, `e` is a synthetic event. React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), so you don't need to worry about cross-browser compatibility. See the [`SyntheticEvent`](/react/docs/events.html) reference guide to learn more.

使用 React 时，当一个 DOM 元素被创建后你一般不需要调用 addEventListener 来添加事件监听。

When using React you should generally not need to call `addEventListener` to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.

当你使用 ES6 类 来定义组件，例如，这个 Toggle 组件渲染了一个能让用户切换 “开” 和 “关” 状态的一个按钮。

When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), a common pattern is for an event handler to be a method on the class. For example, this `Toggle` component renders a button that lets the user toggle between "ON" and "OFF" states:

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

你必须注意 JSX 回调函数中 this 的意思。JavaScript 中，类方法默认是不[限制](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)的。如果你忘了绑定 `this.handleClick` 并传递给 `onClick`，`this` 在函数被实际调用时将会是 `undefined`。

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.

This is not React-specific behavior; it is a part of [how functions work in JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Generally, if you refer to a method without `()` after it, such as `onClick={this.handleClick}`, you should bind that method.

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [property initializer syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use property initializers to correctly bind callbacks:

```js{2-6}
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

这个语法在 [Create React App](https://github.com/facebookincubator/create-react-app) 是被默认启用。

This syntax is enabled by default in [Create React App](https://github.com/facebookincubator/create-react-app).

如果你不使用属性初始化语法，你可以在回调方法中使用 [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：

If you aren't using property initializer syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the property initializer syntax, to avoid this sort of performance problem.
