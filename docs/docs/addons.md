---
id: addons
title: Add-Ons
permalink: docs/addons.html
---

The React add-ons are a collection of useful utility modules for building React apps. **These should be considered experimental** and tend to change more often than the core.

- [`TransitionGroup` and `CSSTransitionGroup`](animation.html), for dealing with animations and transitions that are usually not simple to implement, such as before a component's removal.
- [`createFragment`](create-fragment.html), to create a set of externally-keyed children.

The add-ons below are in the development (unminified) version of React only:

- [`Perf`](perf.html), a performance profiling tool for finding optimization opportunities.
- [`ReactTestUtils`](test-utils.html), simple helpers for writing test cases.

### 遗留的 Add-ons

The add-ons below are considered legacy and their use is discouraged.

- [`PureRenderMixin`](pure-render-mixin.html). Use [`React.PureComponent`](/react/docs/react-api.html#react.purecomponent) instead.
- [`shallowCompare`](shallow-compare.html), a helper function that performs a shallow comparison for props and state in a component to decide if a component should update.
- [`update`](update.html). Use [`kolodny/immutability-helper`](https://github.com/kolodny/immutability-helper) instead.

### 不赞成使用的 Add-ons

[`LinkedStateMixin`](two-way-binding-helpers.html)不赞成使用。

[`LinkedStateMixin`](two-way-binding-helpers.html) has been deprecated.

## 使用带有 Add-ons 的 React

如果你使用 npm，可以独立地安装 add-ons （如：npm install react-addons-test-utils）并导入它们。

If using npm, you can install the add-ons individually from npm (e.g. `npm install react-addons-test-utils`) and import them:

```javascript
import Perf from 'react-addons-perf'; // ES6
var Perf = require('react-addons-perf'); // ES5 with npm
```

当使用 CDN 时，你可以使用 react-with-addons.js 代替 react.js：

When using a CDN, you can use `react-with-addons.js` instead of `react.js`:

```html
<script src="https://unpkg.com/react@15/dist/react-with-addons.js"></script>
```

Add-ons 将通过全局的 React.addons（如：React.addons.TestUtils）来开启。

The add-ons will be available via the `React.addons` global (e.g. `React.addons.TestUtils`).
