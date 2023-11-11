---
title: History API
slug: Web/API/History_API
---

{{DefaultAPISidebar("History API")}}

DOM {{ domxref("window") }} 对象通过 {{ domxref("window.history", "history") }} 对象提供了对浏览器的会话历史的访问 (不要与 [WebExtensions history](/zh-CN/docs/Mozilla/Add-ons/WebExtensions/API/history)搞混了)。它暴露了很多有用的方法和属性，允许你在用户浏览历史中向前和向后跳转，同时——从 HTML5 开始——提供了对 history 栈中内容的操作。

## 意义及使用

使用 {{DOMxRef("History.back","back()")}}, {{DOMxRef("History.forward","forward()")}}和 {{DOMxRef("History.go","go()")}} 方法来完成在用户历史记录中向后和向前的跳转。

### 向前和向后跳转

在 history 中向后跳转：

```js
window.history.back();
```

这和用户点击浏览器回退按钮的效果相同。

类似地，你可以向前跳转（如同用户点击了前进按钮）：

```js
window.history.forward();
```

### 跳转到 history 中指定的一个点

你可以用 `go()` 方法载入到会话历史中的某一特定页面，通过与当前页面相对位置来标志 (当前页面的相对位置标志为 0).

向后移动一个页面 (等同于调用 `back()`):

```js
window.history.go(-1);
```

向前移动一个页面，等同于调用了 `forward()`:

```js
window.history.go(1);
```

类似地，你可以传递参数值 2 并向前移动 2 个页面，等等。

你可以通过查看长度属性的值来确定的历史堆栈中页面的数量：

```js
let numberOfEntries = window.history.length;
```

> **备注：** IE 支持传递 URLs 作为参数给 go()；这在 Gecko 是不标准且不支持的。

## 添加和修改历史记录中的条目

HTML5 引入了 [history.pushState()](/zh-CN/docs/Web/API/History/pushState) 和 [history.replaceState()](</zh-CN/docs/Web/API/History_API#The_replaceState()_method>) 方法，它们分别可以添加和修改历史记录条目。这些方法通常与{{ domxref("window.onpopstate") }} 配合使用。

使用 `history.pushState()` 可以改变 referrer，它在用户发送 [`XMLHttpRequest`](/zh-CN/DOM/XMLHttpRequest) 请求时在 HTTP 头部使用，改变 state 后创建的 [`XMLHttpRequest`](/zh-CN/DOM/XMLHttpRequest) 对象的 referrer 都会被改变。因为 referrer 是标识创建 [`XMLHttpRequest`](/zh-CN/DOM/XMLHttpRequest) 对象时 `this` 所代表的 window 对象中 document 的 URL。

### pushState() 方法的例子

假设在 `http://mozilla.org/foo.html` 页面的 console 中执行了以下 JavaScript 代码：

```js
window.onpopstate = function (e) {
  alert(2);
};

let stateObj = {
  foo: "bar",
};

history.pushState(stateObj, "page 2", "bar.html");
```

这将使浏览器地址栏显示为 `http://mozilla.org/bar.html`，但并不会导致浏览器加载 `bar.html` ，甚至不会检查`bar.html` 是否存在。

假如现在用户在 bar.html 点击了返回按钮，将会执行 alert(2)。

假设现在用户在 bar.html 又访问了 `http://google.com`，然后点击了返回按钮。此时，地址栏将显示 `http://mozilla.org/bar.html`， `history.state` 中包含了 `stateObj` 的一份拷贝。页面此时展现为`bar.html`。且因为页面被重新加载了，所以 `popstate` 事件将不会被触发，也不会执行 alert(2)。

如果我们再次点击返回按钮，页面 URL 会变为 `http://mozilla.org/foo.html`，文档对象 document 会触发另外一个 `popstate` 事件 (如果有 bar.html，且 bar.html 注册了 onpopstate 事件，将会触发此事件，因此也不会执行 foo 页面注册的 onpopstate 事件，也就是不会执行 alert(2))，这一次的事件对象 state object 为 null。这里也一样，返回并不改变文档的内容，尽管文档在接收 `popstate` 事件时可能会改变自己的内容，其内容仍与之前的展现一致。

如果我们再次点击返回按钮，页面 URL 变为其他页面的 url，依然不会执行 alert(2)。因为在返回到 foo 页面的时候并没有 pushState。

### pushState() 方法

`pushState()` 需要三个参数：一个状态对象，一个标题 (目前被忽略), 和 (可选的) 一个 URL. 让我们来解释下这三个参数详细内容：

- **状态对象** — 状态对象 state 是一个 JavaScript 对象，通过 pushState () 创建新的历史记录条目。无论什么时候用户导航到新的状态，popstate 事件就会被触发，且该事件的 state 属性包含该历史记录条目状态对象的副本。

  状态对象可以是能被序列化的任何东西。原因在于 Firefox 将状态对象保存在用户的磁盘上，以便在用户重启浏览器时使用，我们规定了状态对象在序列化表示后有 640k 的大小限制。如果你给 `pushState()` 方法传了一个序列化后大于 640k 的状态对象，该方法会抛出异常。如果你需要更大的空间，建议使用 `sessionStorage` 以及 `localStorage`.

- **标题** — Firefox 目前忽略这个参数，但未来可能会用到。在此处传一个空字符串应该可以安全的防范未来这个方法的更改。或者，你可以为跳转的 state 传递一个短标题。
- **URL** — 该参数定义了新的历史 URL 记录。注意，调用 `pushState()` 后浏览器并不会立即加载这个 URL，但可能会在稍后某些情况下加载这个 URL，比如在用户重新打开浏览器时。新 URL 不必须为绝对路径。如果新 URL 是相对路径，那么它将被作为相对于当前 URL 处理。新 URL 必须与当前 URL 同源，否则 `pushState()` 会抛出一个异常。该参数是可选的，缺省为当前 URL。

> **备注：** 从 Gecko 2.0 到 Gecko 5.0，传递的对象是使用 JSON 进行序列化的。从 Gecko 6.0 开始，该对象的序列化将使用[结构化克隆算法](/zh-CN/DOM/The_structured_clone_algorithm)。这将会使更多对象可以被安全的传递。

在某种意义上，调用 `pushState()` 与 设置 `window.location = "#foo"` 类似，二者都会在当前页面创建并激活新的历史记录。但 `pushState()` 具有如下几条优点：

- 新的 URL 可以是与当前 URL 同源的任意 URL。相反，只有在修改哈希时，设置 `window.location` 才能是同一个 {{ domxref("document") }}。
- 如果你不想改 URL，就不用改。相反，设置 `window.location = "#foo";`在当前哈希不是 `#foo` 时，才能创建新的历史记录项。
- 你可以将任意数据和新的历史记录项相关联。而基于哈希的方式，要把所有相关数据编码为短字符串。
- 如果 `标题` 随后还会被浏览器所用到，那么这个数据是可以被使用的（哈希则不是）。

注意 `pushState()` 绝对不会触发 `hashchange` 事件，即使新的 URL 与旧的 URL 仅哈希不同也是如此。

在 [XUL](/zh-CN/docs/Mozilla/Tech/XUL) 文档中，它创建指定的 XUL 元素。

在其他文档中，它创建一个命名空间 URI 为`null`的元素。

### replaceState() 方法

`history.replaceState()` 的使用与 `history.pushState()` 非常相似，区别在于 `replaceState()` 是修改了当前的历史记录项而不是新建一个。注意这并不会阻止其在全局浏览器历史记录中创建一个新的历史记录项。

`replaceState()` 的使用场景在于为了响应用户操作，你想要更新状态对象 state 或者当前历史记录的 URL。

> **备注：** 从 Gecko 2.0 到 Gecko 5.0，传递的对象是使用 JSON 进行序列化的。从 Gecko 6.0 开始，该对象的序列化将使用[结构化克隆算法](/zh-CN/docs/Web/API/Web_Workers_API/Structured_clone_algorithm)。这将会使更多对象可以被安全的传递。

### replaceState() 方法示例

假设 `http://mozilla.org/foo.html` 执行了如下 JavaScript 代码：

```js
let stateObj = {
  foo: "bar",
};

history.pushState(stateObj, "page 2", "bar.html");
```

上文 2 行代码可以在 "pushState() 方法示例" 部分找到。然后，假设 `http://mozilla.org/bar.html` 执行了如下 JavaScript：

```js
history.replaceState(stateObj, "page 3", "bar2.html");
```

这将会导致地址栏显示 `http://mozilla.org/bar2.html`，但是浏览器并不会去加载`bar2.html` 甚至都不需要检查 `bar2.html` 是否存在。

假设现在用户重新导向到了 `http://www.microsoft.com`，然后点击了回退按钮。这里，地址栏会显示 `http://mozilla.org/bar2.html`。假如用户再次点击回退按钮，地址栏会显示 `http://mozilla.org/foo.html`，完全跳过了 bar.html。

### popstate 事件

每当活动的历史记录项发生变化时， `popstate` 事件都会被传递给 window 对象。如果当前活动的历史记录项是被 `pushState` 创建的，或者是由 `replaceState` 改变的，那么 `popstate` 事件的状态属性 `state` 会包含一个当前历史记录状态对象的拷贝。

使用示例请参见 {{ domxref("window.onpopstate") }} 。

### 获取当前状态

页面加载时，或许会有个非 null 的状态对象。这是有可能发生的，举个例子，假如页面（通过`pushState()` 或 `replaceState()` 方法）设置了状态对象而后用户重启了浏览器。那么当页面重新加载时，页面会接收一个 onload 事件，但没有 popstate 事件。然而，假如你读取了 history.state 属性，你将会得到如同 popstate 被触发时能得到的状态对象。

你可以读取当前历史记录项的状态对象 state，而不必等待`popstate` 事件，只需要这样使用`history.state` 属性：

```js
// 尝试通过 pushState 创建历史条目，然后再刷新页面查看 state 状态对象变化;
window.addEventListener("load", () => {
  let currentState = history.state;
  console.log("currentState", currentState);
});
```

## 例子

完整的 AJAX 网站示例，请参阅： [Ajax navigation example](/zh-CN/docs/Web/Guide/API/DOM/Manipulating_the_browser_history/Example).

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 另见

### 参考

- {{ domxref("window.history") }}
- {{ domxref("window.onpopstate") }}

### Guides

- [Working with the History API](/zh-CN/docs/Web/API/History_API/Working_with_the_History_API)
