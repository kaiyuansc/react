---
id: rendering-elements
title: 渲染元素
permalink: docs/rendering-elements.html
redirect_from: "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

元素 React apps 中最小的组成部分。

Elements are the smallest building blocks of React apps.

一个元素描述了你想要在屏幕上看到的界面：

An element describes what you want to see on the screen:

```js
const element = <h1>Hello, world</h1>;
```

不像浏览器的 DOM 元素，React 元素只是简单的对象，它们很容易被创建。React DOM 只更新 DOM 来匹配 React 元素。

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

>**提示：**
>
>One might confuse elements with a more widely known concept of "components". We will introduce components in the [next section](/react/docs/components-and-props.html). Elements are what components are "made of", and we encourage you to read this section before jumping ahead.

## 把元素渲染进 DOM

比方说在你的 HTML 文件的某个地方有一个 `<div>`：

Let's say there is a `<div>` somewhere in your HTML file:

```html
<div id="root"></div>
```

我们调用这个名为 "root" 的 DOM 节点，那是因为它里面的所有内容都会被 React DOM 管理。

We call this a "root" DOM node because everything inside it will be managed by React DOM.

Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.

为了把一个 React 元素渲染进名为 "root" 的 DOM 节点，需要把它们都传给 `ReactDOM.render()`：

To render a React element into a root DOM node, pass both to `ReactDOM.render()`:

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在 CodePen 中试一试](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)

它在页面中显示“Hello, worl”。

It displays "Hello, world" on the page.

## 更新渲染的元素

React 元素是[不可变的](https://en.wikipedia.org/wiki/Immutable_object)。一旦你创建了一个元素，你不能改变它的子元素或属性。一个元素就像影片中的一帧：它代表了一个时间内 UI 的特定点。

React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

以我们目前的知识，更新 UI 的唯一的方法就是创建一个新元素，然后把它传给 `ReactDOM.render()`。

With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`.

考虑这个时钟的例子：

Consider this ticking clock example:

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

它通过 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) 的回调函数来每秒调用 `ReactDOM.render()`。

It calls `ReactDOM.render()` every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.

>**提示：**
>
>实际开发中，大部分 React apps 只调用一次 `ReactDOM.render()`。下一章节中，我们将学习如何把元素封装进[状态组件](/react/docs/state-and-lifecycle.html)。
>
>In practice, most React apps only call `ReactDOM.render()` once. In the next sections we will learn how such code gets encapsulated into [stateful components](/react/docs/state-and-lifecycle.html).
>
>我们不建议你跳跃学习，因为他们是相辅相成的。
>
>We recommend that you don't skip topics because they build on each other.

## React只更新需要更新的部分

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

You can verify by inspecting the [last example](http://codepen.io/gaearon/pen/gwoJZk?editors=0010) with the browser tools:

![DOM inspector showing granular updates](/react/img/docs/granular-dom-updates.gif)

Even though we create an element describing the whole UI tree on every tick, only the text node whose contents has changed gets updated by React DOM.

In our experience, thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.
