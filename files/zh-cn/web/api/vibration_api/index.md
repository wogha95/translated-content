---
title: Vibration API
slug: Web/API/Vibration_API
---

{{DefaultAPISidebar("Vibration API")}}

大多数现代移动设备包括振动硬件，其允许软件代码通过使设备摇动来向用户提供物理反馈。Vibration API 为 Web 应用程序提供访问此硬件（如果存在）的功能，如果设备不支持此功能，则不会执行任何操作。

## 振动描述

振动被抽象成【开 - 关】脉冲的模式，且可以具有变化的长度。?参数可以是单个整数，表示持续振动的毫秒数 (ms)；或可由多个整数组成的数组，达到振动和暂停循环的效果。只要单一 [`window.navigator.vibrate()`](/zh-CN/docs/Web/API/window.navigator.vibrate) 函式即可控制振动。

### 一个单次振动

你可以通过指定单个值或仅由一个值组成的数组来一次振荡振动硬件：

```js
window.navigator.vibrate(200);
window.navigator.vibrate([200]);
```

以上两个例子都可以使设备振动 200 ms.

### 振动模式

一组数值描述了设备振动并且不振动的交替时间段。数组中的每个值都将转换成一个整数，然后交替解释为设备应该振动的毫秒数和不振动的毫秒数。例如：

```js
window.navigator.vibrate([200, 100, 200]);
```

这会使设备振动 200 ms，然后暂停 100 ms，然后再次振动设备 200 ms。

你可以根据需要设定多个振动/暂停对，数组的值可以是偶数或奇数个；值得注意的是，由于振动在每个振动周期结束时自动停止，因此你不必提供最后一个值去暂停，换句话说，数组长度只需要设置奇数个。

### 停止振动

当调用 {{DOMxRef("Navigator.vibrate()")}} 的参数为 `0`、空数组，或包含全 0 值的数组时会取消任何正在进行中的振动。

### 持续振动

一些基于 setInterval 和 clearInterval 操作将允许你创建持续的振动：

```js
var vibrateInterval;

// Starts vibration at passed in level
function startVibrate(duration) {
  navigator.vibrate(duration);
}

// Stops vibration
function stopVibrate() {
  // Clear interval and stop persistent vibrating
  if (vibrateInterval) clearInterval(vibrateInterval);
  navigator.vibrate(0);
}

// Start persistent vibration at given duration and interval
// Assumes a number value is given
function startPeristentVibrate(duration, interval) {
  vibrateInterval = setInterval(function () {
    startVibrate(duration);
  }, interval);
}
```

当然上面的代码片段没有考虑到振动参数为数组情况; 基于阵列的持久性振动将需要计算数组项的和，并基于该数量创建周期（可能具有额外的延迟）。

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- {{domxref("Navigator.vibrate()")}}
