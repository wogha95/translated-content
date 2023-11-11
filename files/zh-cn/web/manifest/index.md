---
title: Web App Manifest
slug: Web/Manifest
---

{{QuickLinksWithSubpages("/zh-CN/docs/Web/Manifest")}}

[Web 应用程序清单](<web app manifest>)在一个 JSON 文本文件中提供有关应用程序的信息（如名称，作者，图标和描述）。manifest 的目的是将 Web 应用程序安装到设备的主屏幕，为用户提供更快的访问和更丰富的体验。

Web 应用程序清单是被称为[渐进式 Web 应用程序 (PWA)](/zh-CN/docs/Web/Apps/Progressive)的 Web 技术集合的一部分，它们是可以安装到设备的主屏幕的网络应用程序，而不需要用户通过应用商店，伴随着其他功能，比如离线可用和接收推送通知。

## 部署一个 manifest

Web 应用程序清单部署在你的 HTML 页面中，使用在你的文件的头部的一个链接标记：

```html
<link rel="manifest" href="/manifest.json" />
```

## manifest 范例

```json
{
  "name": "HackerWeb",
  "short_name": "HackerWeb",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#fff",
  "description": "A simply readable Hacker News app.",
  "icons": [
    {
      "src": "images/touch/homescreen48.png",
      "sizes": "48x48",
      "type": "image/png"
    },
    {
      "src": "images/touch/homescreen72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "images/touch/homescreen96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "images/touch/homescreen144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "images/touch/homescreen168.png",
      "sizes": "168x168",
      "type": "image/png"
    },
    {
      "src": "images/touch/homescreen192.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "related_applications": [
    {
      "platform": "web"
    },
    {
      "platform": "play",
      "url": "https://play.google.com/store/apps/details?id=cheeaun.hackerweb"
    }
  ]
}
```

## 成员

### `background_color`

为 web 应用程序预定义的背景颜色。此值在应用程序样式表中可以再次声明。它主要用于在样式表加载之前，当加载 manifest，浏览器可以用来绘制 web 应用程序的背景颜色。在启动 web 应用程序和加载应用程序的内容之间创建了一个平滑的过渡。

```json
"background_color": "red"
```

> **备注：** background_color 只是在 web 应用程序加载时提高用户体验，而当 web 应用程序的样式表可用时，不能替代作为背景颜色使用。

### `description`

提供有关 Web 应用程序的一般描述。

```json
"description": "The app that helps you find the best food in town!"
```

### `dir`

指定名称、短名称和描述成员的主文本方向。与 lang 一起配置，可以帮助正确显示右到左文本。

```json
"dir": "rtl",
"lang": "ar",
"short_name": "أنا من التطبيق!"
```

可选值：

- `ltr` (由左到右)
- `rtl` (由右到左)
- `auto` (由浏览器自动判断。

> **备注：** 当省略时，默认为 auto

### `display`

定义开发人员对 Web 应用程序的首选显示模式。

```json
"display": "standalone"
```

有效值：

| 显示模式     | 描述                                                                                                                                                                                   | 后备显示模式 |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| `fullscreen` | 全屏显示，所有可用的显示区域都被使用，并且不显示状态栏{{Glossary("chrome")}}。                                                                                                         | `standalone` |
| `standalone` | 让这个应用看起来像一个独立的应用程序，包括具有不同的窗口，在应用程序启动器中拥有自己的图标等。这个模式中，用户代理将移除用于控制导航的 UI 元素，但是可以包括其他 UI 元素，例如状态栏。 | `minimal-ui` |
| `minimal-ui` | 该应用程序将看起来像一个独立的应用程序，但会有浏览器地址栏。样式因浏览器而异。                                                                                                         | `browser`    |
| `browser`    | 该应用程序在传统的浏览器标签或新窗口中打开，具体实现取决于浏览器和平台。这是默认的设置。                                                                                               | (None)       |

> **备注：** 你可以使用显示模式媒体功能，根据[显示模式](/zh-CN/docs/Web/CSS/@media/display-mode)选择性地将 CSS 应用到你的应用程序。这可用于在从 URL 启动网站和从桌面图标启动网站之间提供一致的用户体验。

### `icons`

指定可在各种环境中用作应用程序图标的图像对象数组。例如，它们可以用来在其他应用程序列表中表示 Web 应用程序，或者将 Web 应用程序与 OS 的任务切换器和/或系统偏好集成在一起。

```json
"icons": [
  {
    "src": "icon/lowres.webp",
    "sizes": "48x48",
    "type": "image/webp"
  },
  {
    "src": "icon/lowres",
    "sizes": "48x48"
  },
  {
    "src": "icon/hd_hi.ico",
    "sizes": "72x72 96x96 128x128 256x256"
  },
  {
    "src": "icon/hd_hi.svg",
    "sizes": "72x72"
  }
]
```

图像对象可能包含以下值：

| 字段    | 描述                                                                           |
| ------- | ------------------------------------------------------------------------------ |
| `sizes` | 包含空格分隔的图像尺寸的字符串。                                               |
| `src`   | 图像文件的路径。如果`src`是一个相对 URL，则基本 URL 将是 manifest 的 URL。     |
| `type`  | 提示图像的媒体类型。此字段的目的是允许用户代理快速忽略不支持的媒体类型的图像。 |

### `lang`

指定`name`和`short_name`成员中的值的主要语言。该值是包含单个语言标记的字符串。

```json
"lang": "en-US"
```

### `name`

为应用程序提供一个人类可读的名称，例如在其他应用程序的列表中或作为图标的标签显示给用户。

```json
"name": "Google I/O 2017"
```

### `orientation`

定义所有 Web 应用程序顶级的默认方向 {{Glossary("Browsing context", "browsing contexts")}}.

```json
"orientation": "portrait-primary"
```

方向可以是以下值之一：

- `any`
- `natural`
- `landscape`
- `landscape-primary`
- `landscape-secondary`
- `portrait`
- `portrait-primary`
- `portrait-secondary`

### `prefer_related_applications`

指定一个布尔值，提示用户代理向用户指示指定的相关应用程序（请参见下文）可用，并建议通过 Web 应用程序。只有当相关的本地应用程序确实提供了某些 Web 应用程序无法做到的事情时，才应该使用它。

```json
"prefer_related_applications": false
```

> **备注：** 如果省略，默认设置为 `false`.

### `related_applications`

指定一个“应用程序对象”数组，代表可由底层平台安装或可访问的本机应用程序 - 例如可通过 Google Play Store 获取的原生 Android 应用程序。这样的应用程序旨在替代提供类似或等同功能的 Web 应用程序 - 就像 Web 应用程序的本机应用程序版本一样。

```json
"related_applications": [
  {
    "platform": "play",
    "url": "https://play.google.com/store/apps/details?id=com.example.app1",
    "id": "com.example.app1"
  }, {
    "platform": "itunes",
    "url": "https://itunes.apple.com/app/example-app1/id123456789"
  }]
```

应用程序对象可能包含以下值：

| Member     | Description                         |
| ---------- | ----------------------------------- |
| `platform` | 可以找到应用程序的平台。            |
| `url`      | 可以找到应用程序的 URL。            |
| `id`       | 用于表示指定平台上的应用程序的 ID。 |

### `scope`

定义此 Web 应用程序的应用程序上下文的导航范围。这基本上限制了 manifest 可以查看的网页。如果用户在范围之外浏览应用程序，则返回到正常的网页。

如果`scope`是相对 URL，则基本 URL 将是 manifest 的 URL。

```json
"scope": "/myapp/"
```

### `short_name`

为应用程序提供简短易读的名称。这是为了在没有足够空间显示 Web 应用程序的全名时使用。

```json
"short_name": "I/O 2017"
```

### `start_url`

指定用户从设备启动应用程序时加载的 URL。如果以相对 URL 的形式给出，则基本 URL 将是 manifest 的 URL。

```json
"start_url": "./?utm_source=web_app_manifest"
```

### `theme_color`

定义应用程序的默认主题颜色。这有时会影响操作系统显示应用程序的方式（例如，在 Android 的任务切换器上，主题颜色包围应用程序）。

```json
"theme_color": "aliceblue"
```

## 初始屏幕

在 Chrome 47 及更高版本中，从主屏幕启动的 Web 应用程序将显示启动画面。这个启动画面是使用 Web 应用程序清单中的属性自动生成的，具体来说就是：`name`，`background_color`以及`icons` 中距设备最近 128dpi 的图标。

## Mime 类型

Manifests 应使用 `application/manifest+json` MIME 类型。但是，你不必非得这样做。

## 浏览器兼容性

{{Compat}}
