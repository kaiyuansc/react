---
layout: hero
title: 一个用来构建用户界面的 JavaScript 库
id: home
---

<section class="light home-section">
  <div class="marketing-row">
    <div class="marketing-col">
      <h3>Declarative</h3>
      <p>React makes it painless to create interactive UIs. Design simple views for each state in your application, and React will efficiently update and render just the right components when your data changes.</p>
      <p>Declarative views make your code more predictable and easier to debug.</p>
    </div>
    <div class="marketing-col">
      <h3>Component-Based</h3>
      <p>Build encapsulated components that manage their own state, then compose them to make complex UIs.</p>
      <p>Since component logic is written in JavaScript instead of templates, you can easily pass rich data through your app and keep state out of the DOM.</p>
    </div>
    <div class="marketing-col">
      <h3>Learn Once, Write Anywhere</h3>
      <p>We don't make assumptions about the rest of your technology stack, so you can develop new features in React without rewriting existing code.</p>
      <p>React can also render on the server using Node and power mobile apps using <a href="https://facebook.github.io/react-native/">React Native</a>.</p>
    </div>
  </div>
</section>
<hr class="home-divider" />
<section class="home-section">
  <div id="examples">
    <div class="example">
      <h3>一个简单的组件</h3>
      <p>
        React 组件通过一个render()方法，接受输入的参数并返回展示的对象。
        以下这个例子使用了 JSX，它类似于XML的语法
        输入的参数通过render()传入组件后，将存储在this.props
      </p>
      <p>
        React components implement a `render()` method that takes input data and
        returns what to display. This example uses an XML-like syntax called
        JSX. Input data that is passed into the component can be accessed by
        `render()` via `this.props`.
      </p>
      <p>
      <strong>JSX 是可选的，并不强制要求使用。</strong>
      点击 "Compiled JS" 可以看到 JSX 编译之后的 JavaScript 代码。
      </p>
      <p>
        <strong>JSX is optional and not required to use React.</strong> Try
        clicking on "Compiled JS" to see the raw JavaScript code produced by
        the JSX compiler.
      </p>
      <div id="helloExample"></div>
    </div>
    <div class="example">
      <h3>一个有状态的组件</h3>
      <p>
        除了接受输入数据（通过 this.props ），组件还可以保持内部状态数据（通过 this.state ）。
        当一个组件的状态数据的变化，展现的标记将被重新调用 render() 更新。
      </p>
      <p>
        In addition to taking input data (accessed via `this.props`), a
        component can maintain internal state data (accessed via `this.state`).
        When a component's state data changes, the rendered markup will be
        updated by re-invoking `render()`.
      </p>
      <div id="timerExample"></div>
    </div>
    <div class="example">
      <h3>一个应用程序</h3>
      <p>
        通过使用 props 和 state, 我们可以组合构建一个小型的 Todo 程序。
        这个例子使用state去监测当前列表的项以及用户已经输入的文本。
        尽管事件绑定似乎是以内联的方式，但他们将被收集起来并以事件代理的方式实现。
      </p>
      <p>
        Using `props` and `state`, we can put together a small Todo application.
        This example uses `state` to track the current list of items as well as
        the text that the user has entered. Although event handlers appear to be
        rendered inline, they will be collected and implemented using event
        delegation.
      </p>
      <div id="todoExample"></div>
    </div>
    <div class="example">
      <h3>一个使用外部插件的组件</h3>
      <p>
        React 是灵活的，并且提供方法允许你跟其他库和框架对接。
        这个例子展现了一个案例，使用外部库 Markdown 实时转化 textarea 的值。
      </p>
      <p>
        React is flexible and provides hooks that allow you to interface with
        other libraries and frameworks. This example uses **remarkable**, an
        external Markdown library, to convert the textarea's value in real time.
      </p>
      <div id="markdownExample"></div>
    </div>
  </div>
  <script src="/react/js/remarkable.min.js"></script>
  <script src="/react/js/examples/hello.js"></script>
  <script src="/react/js/examples/timer.js"></script>
  <script src="/react/js/examples/todo.js"></script>
  <script src="/react/js/examples/markdown.js"></script>
</section>
<hr class="home-divider" />
<section class="home-bottom-section">
  <div class="buttons-unit">
    <a href="docs/hello-world.html" class="button">入门</a>
    <a href="tutorial/tutorial.html" class="button">教程</a>
  </div>
</section>
