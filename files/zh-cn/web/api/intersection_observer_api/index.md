---
title: Intersection Observer API
slug: Web/API/Intersection_Observer_API
---

{{DefaultAPISidebar("Intersection Observer API")}}

Intersection Observer API 提供了一种异步检测目标元素与祖先元素或 {{Glossary("viewport")}} 相交情况变化的方法。

过去，要检测一个元素是否可见或者两个元素是否相交并不容易，很多解决办法不可靠或性能很差。然而，随着互联网的发展，这种需求却与日俱增，比如，下面这些情况都需要用到相交检测：

- 图片懒加载——当图片滚动到可见时才进行加载
- 内容无限滚动——也就是用户滚动到接近内容底部时直接加载更多，而无需用户操作翻页，给用户一种网页可以无限滚动的错觉
- 检测广告的曝光情况——为了计算广告收益，需要知道广告元素的曝光情况
- 在用户看见某个区域时执行任务或播放动画

过去，相交检测通常要用到事件监听，并且需要频繁调用 {{domxref("Element.getBoundingClientRect()")}} 方法以获取相关元素的边界信息。事件监听和调用 {{domxref("Element.getBoundingClientRect()")}} 都是在主线程上运行，因此频繁触发、调用可能会造成性能问题。这种检测方法极其怪异且不优雅。

假如有一个无限滚动的网页，开发者使用了一个第三方库来管理整个页面的广告，又用了另外一个库来实现消息盒子和点赞，并且页面有很多动画（译注：动画往往意味着较高的性能消耗）。两个库都有自己的相交检测程序，都运行在主线程里，而网站的开发者对这些库的内部实现知之甚少，所以并未意识到有什么问题。但当用户滚动页面时，这些相交检测程序就会在页面滚动回调函数里不停触发调用，造成性能问题，体验效果让人失望。

Intersection Observer API 会注册一个回调函数，每当被监视的元素进入或者退出另外一个元素时 (或者 {{Glossary("viewport")}} )，或者两个元素的相交部分大小发生变化时，该回调方法会被触发执行。这样，我们网站的主线程不需要再为了监听元素相交而辛苦劳作，浏览器会自行优化元素相交管理。

注意 Intersection Observer API 无法提供重叠的像素个数或者具体哪个像素重叠，他的更常见的使用方式是——当两个元素相交比例在 N% 左右时，触发回调，以执行某些逻辑。

## Intersection observer 的概念和用法

Intersection Observer API 允许你配置一个回调函数，当以下情况发生时会被调用

- 每当目标 (**target**) 元素与设备视窗或者其他指定元素发生交集的时候执行。设备视窗或者其他元素我们称它为根元素或根 (**root**)。
- Observer 第一次监听目标元素的时候

通常，你需要关注文档最接近的可滚动祖先元素的交集更改，如果元素不是可滚动元素的后代，则默认为设备视窗。如果要观察相对于根 (**root**) 元素的交集，请指定根 (**root**) 元素为`null`。

无论你是使用视口还是其他元素作为根，API 都以相同的方式工作，只要目标元素的可见性发生变化，就会执行你提供的回调函数，以便它与所需的交叉点交叉。

目标 (**target**) 元素与根 (**root**) 元素之间的交叉度是交叉比 (**intersection ratio**)。这是目标 (**target**) 元素相对于根 (**root**) 的交集百分比的表示，它的取值在 0.0 和 1.0 之间。

### 创建一个 intersection observer

创建一个 IntersectionObserver 对象，并传入相应参数和回调用函数，该回调函数将会在目标 (**target**) 元素和根 (**root**) 元素的交集大小超过阈值 (**threshold**) 规定的大小时候被执行。

```js
let options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

let observer = new IntersectionObserver(callback, options);
```

阈值为 1.0 意味着目标元素完全出现在 root 选项指定的元素中可见时，回调函数将会被执行。

#### Intersection observer options

传递到 {{domxref("IntersectionObserver.IntersectionObserver", "IntersectionObserver()")}} 构造函数的 `options` 对象，允许你控制观察者的回调函数的被调用时的环境。它有以下字段：

- `root`
  - : 指定根 (**root**) 元素，用于检查目标的可见性。必须是目标元素的父级元素。如果未指定或者为`null`，则默认为浏览器视窗。
- `rootMargin`
  - : 根 (**root**) 元素的外边距。类似于 CSS 中的 {{cssxref("margin")}} 属性，比如 "`10px 20px 30px 40px"` (top、right、bottom、left)。如果有指定 root 参数，则 rootMargin 也可以使用百分比来取值。该属性值是用作 root 元素和 target 发生交集时候的计算交集的区域范围，使用该属性可以控制 root 元素每一边的收缩或者扩张。默认值为四个边距全是 0。
- `threshold`
  - : 可以是单一的 number 也可以是 number 数组，target 元素和 root 元素相交程度达到该值的时候 IntersectionObserver 注册的回调函数将会被执行。如果你只是想要探测当 target 元素的在 root 元素中的可见性超过 50% 的时候，你可以指定该属性值为 0.5。如果你想要 target 元素在 root 元素的可见程度每多 25% 就执行一次回调，那么你可以指定一个数组 `[0, 0.25, 0.5, 0.75, 1]`。默认值是 0 (意味着只要有一个 target 像素出现在 root 元素中，回调函数将会被执行)。该值为 1.0 含义是当 target 完全出现在 root 元素中时候 回调才会被执行。

#### Targeting an element to be observed

创建一个 observer 后需要给定一个目标元素进行观察。

```js
let target = document.querySelector("#listItem");
observer.observe(target);
```

每当目标满足该 IntersectionObserver 指定的 threshold 值，回调被调用。

只要目标满足为 IntersectionObserver 指定的阈值，就会调用回调。回调接收 {{domxref("IntersectionObserverEntry")}} 对象和观察者的列表：

```js
let callback = (entries, observer) => {
  entries.forEach((entry) => {
    // Each entry describes an intersection change for one observed target element:
    // entry.boundingClientRect
    // entry.intersectionRatio
    // entry.intersectionRect
    // entry.isIntersecting
    // entry.rootBounds
    // entry.target
    // entry.time
  });
};
```

请留意，你注册的回调函数将会在主线程中被执行。所以该函数执行速度要尽可能的快。如果有一些耗时的操作需要执行，建议使用 {{domxref("Window.requestIdleCallback()")}} 方法。

### 交集的计算

所有区域均被 Intersection Observer API 当做一个矩形看待。如果元素是不规则的图形也将会被看成一个包含元素所有区域的最小矩形，相似的，如果元素发生的交集部分不是一个矩形，那么也会被看作是一个包含他所有交集区域的最小矩形。

上述解释有助于理解`IntersectionObserverEntry` 提供的属性，其用于描述 target 和 root 的交集。

#### The intersection root and root margin

在我们开始跟踪 target 元素和容器元素之前，我们要先知道什么是容器 (root) 元素。容器元素又称为 **intersection root**，或 **root element**。这个既可以是 target 元素祖先元素也可以是指定 null 则使用浏览器视口做为容器 (root)。

**_root intersection rectangle_** 是用来对目标元素进行相交检测的矩形，它的大小有以下几种情况：

- 如果 intersection root 隐含 root (值为`null`) (也就是顶级 {{domxref("Document")}}), 那么 root intersection 矩形就是视窗的矩形大小。
- 如果 intersection root 有溢出部分，则 root intersection 矩形是 root 元素的内容 (content) 区域。
- 否则，root intersection 矩形就是 intersection root 的 bounding client rectangle (可以调用元素的 {{domxref("Element.getBoundingClientRect", "getBoundingClientRect()")}} 方法获取).

用作描述 intersection root 元素边界的矩形可以使用 **root margin** 来调整矩形大小，即 `rootMargin` 属性，在我们创建 IntersectionObserver 对象的时候使用。`rootMargin` 的属性值将会做为 margin 偏移值添加到 intersection root 元素的对应的 margin 位置，并最终形成 root 元素的矩形边界 (在执行回调函数时可由 {{domxref("IntersectionObserverEntry.rootBounds")}} 得到)。

#### Thresholds

IntersectionObserver API 并不会每次在元素的交集发生变化的时候都会执行回调。相反它使用了 **thresholds** 参数。当你创建一个 observer 的时候，你可以提供一个或者多个 number 类型的数值用来表示 target 元素在 root 元素的可见程序的百分比，然后，API 的回调函数只会在元素达到 **thresholds** 规定的阈值时才会执行。

例如，当你想要在 target 在 root 元素中中的可见性每超过 25% 或者减少 25% 的时候都通知一次。你可以在创建 observer 的时候指定 **thresholds** 属性值为 `[0, 0.25, 0.5, 0.75, 1]`，你可以通过检测在每次交集发生变化的时候的都会传递回调函数的参数 {{domxref("IntersectionObserverEntry.isIntersecting", "isIntersecting")}} 的属性值来判断 target 元素在 root 元素中的可见性是否发生变化。如果 `isIntersecting` 为真，target 元素的至少已经达到 **thresholds** 属性值当中规定的其中一个阈值，如果为假，则 target 元素不在给定的阈值范围内可见。

Note that it's possible to have a non-zero intersection rectangle, which can happen if the intersection is exactly along the boundary between the two or the area of {{domxref("IntersectionObserverEntry.boundingClientRect", "boundingClientRect")}} is zero. This state of the target and root sharing a boundary line is not considered enough to be considered transitioning into an intersecting state.

为了让我们感受下 thresholds 是如何工作的，尝试滚动以下的例子，每一个 colored box 的四个边角都会展示自身在 root 元素中的可见程度百分比，所以在你滚动 root 的时候你将会看到四个边角的数值一直在发生变化。每一个 box 都有不同的 thresholds：

- 第一个盒子的 thresholds 包含每个可视百分比，也就是说，{{domxref("IntersectionObserver.thresholds")}} 数组是 `[0.00, 0.01, 0.02, ..., 0.99, 1.00]`。
- 第二个盒子只有唯一的值 `[0.5]`。
- 第三个盒子的 thresholds 按 10% 从 0 递增 (0%, 10%, 20%, etc.)。
- 最后一个盒子为 `[0, 0.25, 0.5, 0.75, 1.0]`。

```html hidden
<template id="boxTemplate">
  <div class="sampleBox">
    <div class="label topLeft"></div>
    <div class="label topRight"></div>
    <div class="label bottomLeft"></div>
    <div class="label bottomRight"></div>
  </div>
</template>

<main>
  <div class="contents">
    <div class="wrapper"></div>
  </div>
</main>
```

```css hidden
.contents {
  position: absolute;
  width: 700px;
  height: 1725px;
}

.wrapper {
  position: relative;
  top: 600px;
}

.sampleBox {
  position: relative;
  left: 175px;
  width: 150px;
  background-color: rgb(245, 170, 140);
  border: 2px solid rgb(201, 126, 17);
  padding: 4px;
  margin-bottom: 6px;
}

#box1 {
  height: 200px;
}

#box2 {
  height: 75px;
}

#box3 {
  height: 150px;
}

#box4 {
  height: 100px;
}

.label {
  font:
    14px "Open Sans",
    "Arial",
    sans-serif;
  position: absolute;
  margin: 0;
  background-color: rgba(255, 255, 255, 0.7);
  border: 1px solid rgba(0, 0, 0, 0.7);
  width: 3em;
  height: 18px;
  padding: 2px;
  text-align: center;
}

.topLeft {
  left: 2px;
  top: 2px;
}

.topRight {
  right: 2px;
  top: 2px;
}

.bottomLeft {
  bottom: 2px;
  left: 2px;
}

.bottomRight {
  bottom: 2px;
  right: 2px;
}
```

```js hidden
let observers = [];

startup = () => {
  let wrapper = document.querySelector(".wrapper");

  // Options for the observers

  let observerOptions = {
    root: null,
    rootMargin: "0px",
    threshold: [],
  };

  // An array of threshold sets for each of the boxes. The
  // first box's thresholds are set programmatically
  // since there will be so many of them (for each percentage
  // point).

  let thresholdSets = [
    [],
    [0.5],
    [0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0],
    [0, 0.25, 0.5, 0.75, 1.0],
  ];

  for (let i = 0; i <= 1.0; i += 0.01) {
    thresholdSets[0].push(i);
  }

  // Add each box, creating a new observer for each

  for (let i = 0; i < 4; i++) {
    let template = document
      .querySelector("#boxTemplate")
      .content.cloneNode(true);
    let boxID = "box" + (i + 1);
    template.querySelector(".sampleBox").id = boxID;
    wrapper.appendChild(document.importNode(template, true));

    // Set up the observer for this box

    observerOptions.threshold = thresholdSets[i];
    observers[i] = new IntersectionObserver(
      intersectionCallback,
      observerOptions,
    );
    observers[i].observe(document.querySelector("#" + boxID));
  }

  // Scroll to the starting position

  document.scrollingElement.scrollTop =
    wrapper.firstElementChild.getBoundingClientRect().top + window.scrollY;
  document.scrollingElement.scrollLeft = 750;
};

intersectionCallback = (entries) => {
  entries.forEach((entry) => {
    let box = entry.target;
    let visiblePct = Math.floor(entry.intersectionRatio * 100) + "%";

    box.querySelector(".topLeft").innerHTML = visiblePct;
    box.querySelector(".topRight").innerHTML = visiblePct;
    box.querySelector(".bottomLeft").innerHTML = visiblePct;
    box.querySelector(".bottomRight").innerHTML = visiblePct;
  });
};

startup();
```

{{EmbedLiveSample("Thresholds", 500, 500)}}

#### Clipping and the intersection rectangle

The browser computes the final intersection rectangle as follows; this is all done for you, but it can be helpful to understand these steps in order to better grasp exactly when intersections will occur.

1. The target element's bounding rectangle (that is, the smallest rectangle that fully encloses the bounding boxes of every component that makes up the element) is obtained by calling {{domxref("Element.getBoundingClientRect", "getBoundingClientRect()")}} on the target. This is the largest the intersection rectangle may be. The remaining steps will remove any portions that don't intersect.
2. Starting at the target's immediate parent block and moving outward, each containing block's clipping (if any) is applied to the intersection rectangle. A block's clipping is determined based on the intersection of the two blocks and the clipping mode (if any) specified by the {{cssxref("overflow")}} property. Setting `overflow` to anything but `visible` causes clipping to occur.
3. If one of the containing elements is the root of a nested browsing context (such as the document contained in an {{HTMLElement("iframe")}}, the intersection rectangle is clipped to the containing context's viewport, and recursion upward through the containers continues with the container's containing block. So if the top level of an `<iframe>` is reached, the intersection rectangle is clipped to the frame's viewport, then the frame's parent element is the next block recursed through toward the intersection root.
4. When recursion upward reaches the intersection root, the resulting rectangle is mapped to the intersection root's coordinate space.
5. The resulting rectangle is then updated by intersecting it with the [root intersection rectangle](#root-intersection-rectangle).
6. This rectangle is, finally, mapped to the coordinate space of the target's {{domxref("document")}}.

### Intersection change callbacks

When the amount of a target element which is visible within the root element crosses one of the visibility thresholds, the {{domxref("IntersectionObserver")}} object's callback is executed. The callback receives as input an array of all of {{domxref("IntersectionObserverEntry")}} objects, one for each threshold which was crossed, and a reference to the `IntersectionObserver` object itself.

Each entry in the list of thresholds is an {{domxref("IntersectionObserverEntry")}} object describing one threshold that was crossed; that is, each entry describes how much of a given element is intersecting with the root element, whether or not the element is considered to be intersecting or not, and the direction in which the transition occurred.

The code snippet below shows a callback which keeps a counter of how many times elements transition from not intersecting the root to intersecting by at least 75%. For a threshold value of 0.0 (default) the callback is called [approximately](https://www.w3.org/TR/intersection-observer/#dom-intersectionobserverentry-isintersecting) upon transition of the boolean value of {{domxref("IntersectionObserverEntry.isIntersecting", "isIntersecting")}}. The snippet thus first checks that the transition is a positive one, then determines whether {{domxref("IntersectionObserverEntry.intersectionRatio", "intersectionRatio")}} is above 75%, in which case it increments the counter.

```js
intersectionCallback(entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      let elem = entry.target;

      if (entry.intersectionRatio >= 0.75) {
        intersectionCounter++;
      }
    }
  });
}
```

## Interfaces

- {{domxref("IntersectionObserver")}}
  - : The primary interface for the Intersection Observer API. Provides methods for creating and managing an observer which can watch any number of target elements for the same intersection configuration. Each observer can asynchronously observe changes in the intersection between one or more target elements and a shared ancestor element or with their top-level {{domxref("Document")}}'s {{Glossary('viewport')}}. The ancestor or viewport is referred to as the **root**.
- {{domxref("IntersectionObserverEntry")}}
  - : Describes the intersection between the target element and its root container at a specific moment of transition. Objects of this type can only be obtained in two ways: as an input to your `IntersectionObserver` callback, or by calling {{domxref("IntersectionObserver.takeRecords()")}}.

## A simple example

This simple example causes a target element to change its color and transparency as it becomes more or less visible. At [Timing element visibility with the Intersection Observer API](/zh-CN/docs/Web/API/Intersection_Observer_API/Timing_element_visibility), you can find a more extensive example showing how to time how long a set of elements (such as ads) are visible to the user and to react to that information by recording statistics or by updating elements..

### HTML

The HTML for this example is very short, with a primary element which is the box that we'll be targeting (with the creative ID `"box"`) and some contents within the box.

```html
<div id="box">
  <div class="vertical">Welcome to <strong>The Box!</strong></div>
</div>
```

### CSS

The CSS isn't terribly important for the purposes of this example; it lays out the element and establishes that the {{cssxref("background-color")}} and {{cssxref("border")}} attributes can participate in [CSS transitions](/zh-CN/docs/Web/CSS/CSS_transitions), which we'll use to affect the changes to the element as it becomes more or less obscured.

```css
#box {
  background-color: rgba(40, 40, 190, 255);
  border: 4px solid rgb(20, 20, 120);
  transition:
    background-color 1s,
    border 1s;
  width: 350px;
  height: 350px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.vertical {
  color: white;
  font: 32px "Arial";
}

.extra {
  width: 350px;
  height: 350px;
  margin-top: 10px;
  border: 4px solid rgb(20, 20, 120);
  text-align: center;
  padding: 20px;
}
```

### JavaScript

Finally, let's take a look at the JavaScript code that uses the Intersection Observer API to make things happen.

#### Setting up

First, we need to prepare some variables and install the observer.

```js
const numSteps = 20.0;

let boxElement;
let prevRatio = 0.0;
let increasingColor = "rgba(40, 40, 190, ratio)";
let decreasingColor = "rgba(190, 40, 40, ratio)";

// Set things up
window.addEventListener(
  "load",
  (event) => {
    boxElement = document.querySelector("#box");

    createObserver();
  },
  false,
);
```

The constants and variables we set up here are:

- `numSteps`
  - : A constant which indicates how many thresholds we want to have between a visibility ratio of 0.0 and 1.0.
- `prevRatio`
  - : This variable will be used to record what the visibility ratio was the last time a threshold was crossed; this will let us figure out whether the target element is becoming more or less visible.
- `increasingColor`
  - : A string defining a color we'll apply to the target element when the visibility ratio is increasing. The word "ratio" in this string will be replaced with the target's current visibility ratio, so that the element not only changes color but also becomes increasingly opaque as it becomes less obscured.
- `decreasingColor`
  - : Similarly, this is a string defining a color we'll apply when the visibility ratio is decreasing.

We call {{domxref("EventTarget.addEventListener", "Window.addEventListener()")}} to start listening for the [`load`](/zh-CN/docs/Web/API/Window/load_event) event; once the page has finished loading, we get a reference to the element with the ID `"box"` using {{domxref("Document.querySelector", "querySelector()")}}, then call the `createObserver()` method we'll create in a moment to handle building and installing the intersection observer.

#### Creating the intersection observer

The `createObserver()` method is called once page load is complete to handle actually creating the new {{domxref("IntersectionObserver")}} and starting the process of observing the target element.

```js
function createObserver() {
  let observer;

  let options = {
    root: null,
    rootMargin: "0px",
    threshold: buildThresholdList(),
  };

  observer = new IntersectionObserver(handleIntersect, options);
  observer.observe(boxElement);
}
```

This begins by setting up an `options` object containing the settings for the observer. We want to watch for changes in visibility of the target element relative to the document's viewport, so `root` is `null`. We need no margin, so the margin offset, `rootMargin`, is specified as "0px". This causes the observer to watch for changes in the intersection between the target element's bounds and those of the viewport, without any added (or subtracted) space.

The list of visibility ratio thresholds, `threshold`, is constructed by the function `buildThresholdList()`. The threshold list is built programmatically in this example since there are a number of them and the number is intended to be adjustable.

Once `options` is ready, we create the new observer, calling the {{domxref("IntersectionObserver.IntersectionObserver", "IntersectionObserver()")}} constructor, specifying a function to be called when intersection crosses one of our thresholds, `handleIntersect()`, and our set of options. We then call {{domxref("IntersectionObserver.observe", "observe()")}} on the returned observer, passing into it the desired target element.

We could opt to monitor multiple elements for visibility intersection changes with respect to the viewport by calling `observer.observe()` for each of those elements, if we wanted to do so.

#### Building the array of threshold ratios

The `buildThresholdList()` function, which builds the list of thresholds, looks like this:

```js
function buildThresholdList() {
  let thresholds = [];
  let numSteps = 20;

  for (let i = 1.0; i <= numSteps; i++) {
    let ratio = i / numSteps;
    thresholds.push(ratio);
  }

  thresholds.push(0);
  return thresholds;
}
```

This builds the array of thresholds—each of which is a ratio between 0.0 and 1.0, by pushing the value `i/numSteps` onto the `thresholds` array for each integer `i` between 1 and `numSteps`. It also pushes 0 to include that value. The result, given the default value of `numSteps` (20), is the following list of thresholds:

| #   | Ratio | #   | Ratio |
| --- | ----- | --- | ----- |
| 1   | 0.05  | 11  | 0.55  |
| 2   | 0.1   | 12  | 0.6   |
| 3   | 0.15  | 13  | 0.65  |
| 4   | 0.2   | 14  | 0.7   |
| 5   | 0.25  | 15  | 0.75  |
| 6   | 0.3   | 16  | 0.8   |
| 7   | 0.35  | 17  | 0.85  |
| 8   | 0.4   | 18  | 0.9   |
| 9   | 0.45  | 19  | 0.95  |
| 10  | 0.5   | 20  | 1.0   |

We could, of course, hard-code the array of thresholds into our code, and often that's what you'll end up doing. But this example leaves room for adding configuration controls to adjust the granularity, for example.

#### Handling intersection changes

When the browser detects that the target element (in our case, the one with the ID `"box"`) has been unveiled or obscured such that its visibility ratio crosses one of the thresholds in our list, it calls our handler function, `handleIntersect()`:

```js
function handleIntersect(entries, observer) {
  entries.forEach((entry) => {
    if (entry.intersectionRatio > prevRatio) {
      entry.target.style.backgroundColor = increasingColor.replace(
        "ratio",
        entry.intersectionRatio,
      );
    } else {
      entry.target.style.backgroundColor = decreasingColor.replace(
        "ratio",
        entry.intersectionRatio,
      );
    }

    prevRatio = entry.intersectionRatio;
  });
}
```

For each {{domxref("IntersectionObserverEntry")}} in the list `entries`, we look to see if the entry's {{domxref("IntersectionObserverEntry.intersectionRatio", "intersectionRatio")}} is going up; if it is, we set the target's {{cssxref("background-color")}} to the string in `increasingColor` (remember, it's `"rgba(40, 40, 190, ratio)"`), replaces the word "ratio" with the entry's `intersectionRatio`. The result: not only does the color get changed, but the transparency of the target element changes, too; as the intersection ratio goes down, the background color's alpha value goes down with it, resulting in an element that's more transparent.

Similarly, if the `intersectionRatio` is going down, we use the string `decreasingColor` and replace the word "ratio" in that with the `intersectionRatio` before setting the target element's `background-color`.

Finally, in order to track whether the intersection ratio is going up or down, we remember the current ratio in the variable `prevRatio`.

### Result

Below is the resulting content. Scroll this page up and down and notice how the appearance of the box changes as you do so.

{{EmbedLiveSample('A_simple_example', 425, 425)}}

There's an even more extensive example at [Timing element visibility with the Intersection Observer API](/zh-CN/docs/Web/API/Intersection_Observer_API/Timing_element_visibility).

## Specifications

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- [Intersection Observer polyfill](https://github.com/w3c/IntersectionObserver)
- [Timing element visibility with the Intersection Observer API](/zh-CN/docs/Web/API/Intersection_Observer_API/Timing_element_visibility)
- {{domxref("IntersectionObserver")}} and {{domxref("IntersectionObserverEntry")}}
