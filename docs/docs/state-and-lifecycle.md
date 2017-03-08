---
id: state-and-lifecycle
title: 状态和生命周期
permalink: docs/state-and-lifecycle.html
redirect_from: "docs/interactivity-and-dynamic-uis.html"
prev: components-and-props.html
next: handling-events.html
---

考虑 [前面章节](/react/docs/rendering-elements.html#updating-the-rendered-element) 中时钟的例子。

Consider the ticking clock example from [one of the previous sections](/react/docs/rendering-elements.html#updating-the-rendered-element).

到目前为止我们只学习了一种更新 UI 的方法。

So far we have only learned one way to update the UI.

我们调用 `ReactDOM.render()` 去改变渲染输出：

We call `ReactDOM.render()` to change the rendered output:

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

这一章中，我们将学习如何使这个 `Clock` 组件真正实现封装的和可重用的。

In this section, we will learn how to make the `Clock` component truly reusable and encapsulated. It will set up its own timer and update itself every second.

我们可以开始封装这个时钟了。

We can start by encapsulating how the clock looks:

```js{3-6,12}
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)

However, it misses a crucial requirement: the fact that the `Clock` sets up a timer and updates the UI every second should be an implementation detail of the `Clock`.

Ideally we want to write this once and have the `Clock` update itself:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

为了实现它，我们需要向 `Clock` 组件添加一个 "state"。

To implement this, we need to add "state" to the `Clock` component.

State 和 props 很像，但它是组件私有的并被组件完全控制。

State is similar to props, but it is private and fully controlled by the component.

我们[之前提到过](/react/docs/components-and-props.html#functional-and-class-components)，组件以类的方式定义时会有一些附加功能。本地 state 恰恰只有类才会有。

We [mentioned before](/react/docs/components-and-props.html#functional-and-class-components) that components defined as classes have some additional features. Local state is exactly that: a feature available only to classes.

## 函数转化成类

你可以通过五步把像 Clock 这个函数式的组件转化成一个类来定义的组件：

You can convert a functional component like `Clock` to a class in five steps:

1. 以同样的名字创建一个 [ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)，这个类继承自 React.Component。Create an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) with the same name that extends `React.Component`.

2. 添加一个叫做 `render()` 的空方法。Add a single empty method to it called `render()`.

3. 把函数组件的主体移进 `render()` 方法。Move the body of the function into the `render()` method.

4. 在 `render()` 中用 `this.props` 代替 `props`。Replace `props` with `this.props` in the `render()` body.

5. 删除之前的空的函数声明。Delete the remaining empty function declaration.

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)

`Cloc`现在被定义为一个类而不是函数。

`Clock` is now defined as a class rather than a function.

这让我们能有诸如本地状态和生命周期钩子的附加功能。

This lets us use additional features such as local state and lifecycle hooks.

## 向类中添加本地状态

我们分三步将来自于 props 的 `date` 移进状态：

We will move the `date` from props to state in three steps:

1) 在 `render()` 方法中用 `this.state.date` 代替 `this.props.date`。Replace `this.props.date` with `this.state.date` in the `render()` method:

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) 添加一个初始化 `this.state` 的[类构造器](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)。Add a [class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor) that assigns the initial `this.state`:

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Note how we pass `props` to the base constructor:

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

Class components should always call the base constructor with `props`.

3) 从 `<Clock />` 元素中移除 `date` prop。Remove the `date` prop from the `<Clock />` element:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

We will later add the timer code back to the component itself.

结果看起来像这样：

The result looks like this:

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)

Next, we'll make the `Clock` set up its own timer and update itself every second.

## 向类中添加生命周期方法

在应用程序中会有很多组件，当这些组件销毁时释放它们占用的资源就会很重要。

In applications with many components, it's very important to free up resources taken by the components when they are destroyed.

We want to [set up a timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) whenever the `Clock` is rendered to the DOM for the first time. This is called "mounting" in React.

We also want to [clear that timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) whenever the DOM produced by the `Clock` is removed. This is called "unmounting" in React.

We can declare special methods on the component class to run some code when a component mounts and unmounts:

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

这些方法里我们调用了“生命周期钩子”。

These methods are called "lifecycle hooks".

The `componentDidMount()` hook runs after the component output has been rendered to the DOM. This is a good place to set up a timer:

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

Note how we save the timer ID right on `this`.

While `this.props` is set up by React itself and `this.state` has a special meaning, you are free to add additional fields to the class manually if you need to store something that is not used for the visual output.

If you don't use something in `render()`, it shouldn't be in the state.

We will tear down the timer in the `componentWillUnmount()` lifecycle hook:

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Finally, we will implement the `tick()` method that runs every second.

It will use `this.setState()` to schedule updates to the component local state:

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/amqdNA?editors=0010)

现在这个时钟每秒都会闪动。

Now the clock ticks every second.

Let's quickly recap what's going on and the order in which the methods are called:

1) When `<Clock />` is passed to `ReactDOM.render()`, React calls the constructor of the `Clock` component. Since `Clock` needs to display the current time, it initializes `this.state` with an object including the current time. We will later update this state.

2) React then calls the `Clock` component's `render()` method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the `Clock`'s render output.

3) When the `Clock` output is inserted in the DOM, React calls the `componentDidMount()` lifecycle hook. Inside it, the `Clock` component asks the browser to set up a timer to call `tick()` once a second.

4) Every second the browser calls the `tick()` method. Inside it, the `Clock` component schedules a UI update by calling `setState()` with an object containing the current time. Thanks to the `setState()` call, React knows the state has changed, and calls `render()` method again to learn what should be on the screen. This time, `this.state.date` in the `render()` method will be different, and so the render output will include the updated time. React updates the DOM accordingly.

5) If the `Clock` component is ever removed from the DOM, React calls the `componentWillUnmount()` lifecycle hook so the timer is stopped.

## 正确地使用状态

关于 `setState()` ，你有三个需要知道的地方。

There are three things you should know about `setState()`.

### 不要直接修改状态

例如，这个将不会重新渲染一个组件：

For example, this will not re-render a component:

```js
// Wrong
this.state.comment = 'Hello';
```
用 `setState()` 来替代：
Instead, use `setState()`:

```js
// Correct
this.setState({comment: 'Hello'});
```
指定 `this.state` 的唯一的地方是构造器方法。

The only place where you can assign `this.state` is the constructor.

### 状态更新可能是异步的

React may batch multiple `setState()` calls into a single update for performance.

Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.

For example, this code may fail to update the counter:

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

We used an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) above, but it also works with regular functions:

```js
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### State Updates are Merged

When you call `setState()`, React merges the object you provide into the current state.

For example, your state may contain several independent variables:

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Then you can update them independently with separate `setState()` calls:

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

The merging is shallow, so `this.setState({comments})` leaves `this.state.posts` intact, but completely replaces `this.state.comments`.

## The Data Flows Down

Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn't care whether it is defined as a function or a class.

This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components:

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

This also works for user-defined components:

```js
<FormattedDate date={this.state.date} />
```

The `FormattedDate` component would receive the `date` in its props and wouldn't know whether it came from the `Clock`'s state, from the `Clock`'s props, or was typed by hand:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)

This is commonly called a "top-down" or "unidirectional" data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components "below" them in the tree.

If you imagine a component tree as a waterfall of props, each component's state is like an additional water source that joins it at an arbitrary point but also flows down.

To show that all components are truly isolated, we can create an `App` component that renders three `<Clock>`s:

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)

Each `Clock` sets up its own timer and updates independently.

In React apps, whether a component is stateful or stateless is considered an implementation detail of the component that may change over time. You can use stateless components inside stateful components, and vice versa.
