---
title: 客户端 Web API
slug: Learn/JavaScript/Client-side_web_APIs
---

{{LearnSidebar}}

当你给网页或者网页应用编写客户端的 JavaScript 时，你很快会遇上应用程序接口（API）—— 这些编程特性可用来操控网站所基于的浏览器与操作系统的不同方面，或是操控由其他网站或服务端传来的数据。在这个单元里，我们将一同探索什么是 API，以及如何使用一些在你开发中将经常遇见的 API。

## 预备知识

若想深入理解这个单元的内容，你必须能够以自己的能力较好地学完之前的几个章节 ([JavaScript 第一步](/zh-CN/docs/Learn/JavaScript/First_steps), [JavaScript](/zh-CN/docs/Learn/JavaScript/First_steps)[基础要件](/zh-CN/docs/Learn/JavaScript/Building_blocks), 和[JavaScript](/zh-CN/docs/Learn/JavaScript/First_steps)[对象介绍](/zh-CN/docs/Learn/JavaScript/Objects)). 这几部分涉及到了许多简单的 API 的使用，如果没有它们我们将很难做一些实际的事情。在这个教程中，我们会认为你懂得 JavaScript 的核心知识，而且我们将更深入地探索常见的网页 API。

若你知道 [HTML](/zh-CN/docs/Learn/HTML) 和 [CSS](/zh-CN/docs/Learn/CSS) 的基本知识，也会对理解这个单元的内容大有裨益。

> **备注：** 如果你正在使用一台无法创建你自身文件的电脑、平板或其他设备，你可以尝试使用一些在线网页编辑器如[JSBin](http://jsbin.com/)或者[Glitch](https://glitch.com/)，来尝试编辑一些代码示例。

## 向导

- [Web API 简介](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)
  - : 首先，我们将从一个更高的角度来看这些 API —它们是什么，它们怎么起作用的，你该怎么在自己的代码中使用它们以及他们是怎么构成的？我们依旧会再来看一看这些 API 有哪些主要的种类和他们会有哪些用处。
- [操作文档](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)
  - : 当你在制作 WEB 页面和 APP 时，一个你最经常想要做的事就是通过一些方法来操作 WEB 文档。这其中最常见的方法就是使用文档对象模型 Document Object Model (DOM)，它是一系列大量使用了 {{domxref("Document")}} object 的 API 来控制 HTML 和样式信息。通过这篇文章，我们来看看使用 DOM 方面的一些细节，以及其他一些有趣的 API 能够通过一些有趣的方式改变你的环境。
- [从服务器获取数据](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data)
  - : 在现代网页及其 APP 中另外一个很常见的任务就是与服务器进行数据交互时不再刷新整个页面，这看起来微不足道，但却对一个网页的展现和交互上起到了很大的作用，在这篇文章里，我们将阐述这个概念，然后来了解实现这个功能的技术，例如 {{domxref("XMLHttpRequest")}} 和 [Fetch API](/zh-CN/docs/Web/API/Fetch_API).（抓取 API）。
- [第三方 API](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Third_party_APIs)
  - : 到目前为止我们所涉及的 API 都是浏览器内置的，但并不代表所有。许多大网站如 Google Maps, Twitter, Facebook, PayPal 等，都提供他们的 API 给开发者们去使用他们的数据（比如在你的博客里展示你分享的推特内容）或者服务（如在你的网页里展示定制的谷歌地图或接入 Facebook 登录功能）。这篇文章介绍了浏览器 API 和第三方 API 的差别以及一些最新的典型应用。
- [绘制图形](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Drawing_graphics)
  - : 浏览器包含多种强大的图形编程工具，从可缩放矢量图形语言 Scalable Vector Graphics ([SVG](/zh-CN/docs/Web/SVG)) language，到 HTML 绘制元素 {{htmlelement("canvas")}} 元素 ([The Canvas API](/zh-CN/docs/Web/API/Canvas_API) and [WebGL](/zh-CN/docs/Web/API/WebGL_API)). 这篇文章提供了部分 canvas 的简介，以及让你更深入学习的资源。
- [视频和音频 API](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Video_and_audio_APIs)
  - : HTML5 能够通过元素标签嵌入富媒体——{{htmlelement("video")}} and {{htmlelement("audio")}}——而将有自己的 API 来控制回放，搜索等功能。本文向你展示了如何创建自定义播放控制等常见的任务。
- [客户端存储](/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage)
  - : 现代 web 浏览器拥有很多不同的技术，能够让你存储与网站相关的数据，并在需要时调用它们，能够让你长期保存数据、保存离线网站及其他实现其他功能。本文解释了这些功能的基本原理。
