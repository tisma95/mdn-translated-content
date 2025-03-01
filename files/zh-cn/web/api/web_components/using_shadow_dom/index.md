---
title: 使用 shadow DOM
slug: Web/API/Web_components/Using_shadow_DOM
---

{{DefaultAPISidebar("Web Components")}}

Web components 的一个重要属性是封装——可以将标记结构、样式和行为隐藏起来，并与页面上的其他代码相隔离，保证不同的部分不会混在一起，可使代码更加干净、整洁。其中，Shadow DOM 接口是关键所在，它可以将一个隐藏的、独立的 DOM 附加到一个元素上。本篇文章将会介绍 Shadow DOM 的基础使用。

> **备注：** Firefox（从版本 63 开始），Chrome，Opera 和 Safari 默认支持 Shadow DOM。基于 Chromium 的新 Edge 也支持 Shadow DOM；而旧 Edge 未能撑到支持此特性。

## 概况

本文章假设你对 [DOM（文档对象模型）](/zh-CN/docs/Web/API/Document_Object_Model/Introduction)有一定的了解，它是由不同的元素节点、文本节点连接而成的一个树状结构，应用于标记文档中（例如 Web 文档中常见的 HTML 文档）。请看如下示例，一段 HTML 代码：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Simple DOM example</title>
  </head>
  <body>
    <section>
      <img
        src="dinosaur.png"
        alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />
      <p>
        Here we will add a link to the
        <a href="https://www.mozilla.org/">Mozilla homepage</a>
      </p>
    </section>
  </body>
</html>
```

这个片段会生成如下的 DOM 结构：

![](dom-screenshot.png)

_Shadow_ DOM 允许将隐藏的 DOM 树附加到常规的 DOM 树中——它以 shadow root 节点为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 DOM 元素一样。

![](shadowdom.svg)

这里，有一些 Shadow DOM 特有的术语需要我们了解：

- Shadow host：一个常规 DOM 节点，Shadow DOM 会被附加到这个节点上。
- Shadow tree：Shadow DOM 内部的 DOM 树。
- Shadow boundary：Shadow DOM 结束的地方，也是常规 DOM 开始的地方。
- Shadow root: Shadow tree 的根节点。

你可以使用同样的方式来操作 Shadow DOM，就和操作常规 DOM 一样——例如添加子节点、设置属性，以及为节点添加自己的样式（例如通过 `element.style` 属性），或者为整个 Shadow DOM 添加样式（例如在 {{htmlelement("style")}} 元素内添加样式）。不同的是，Shadow DOM 内部的元素始终不会影响到它外部的元素（除了 {{CSSxRef(":focus-within")}}），这为封装提供了便利。

注意，不管从哪个方面来看，Shadow DOM 都不是一个新事物——在过去的很长一段时间里，浏览器用它来封装一些元素的内部结构。以一个有着默认播放控制按钮的 {{htmlelement("video")}} 元素为例。你所能看到的只是一个 `<video>` 标签，实际上，在它的 Shadow DOM 中，包含了一系列的按钮和其他控制器。Shadow DOM 标准允许你为你自己的元素（custom element）维护一组 Shadow DOM。

## 基本用法

可以使用 {{domxref("Element.attachShadow()")}} 方法来将一个 shadow root 附加到任何一个元素上。它接受一个配置对象作为参数，该对象有一个 `mode` 属性，值可以是 `open` 或者 `closed`：

```js
let shadow = elementRef.attachShadow({ mode: "open" });
let shadow = elementRef.attachShadow({ mode: "closed" });
```

`open` 表示可以通过页面内的 JavaScript 方法来获取 Shadow DOM，例如使用 {{domxref("Element.shadowRoot")}} 属性：

```js
let myShadowDom = myCustomElem.shadowRoot;
```

如果你将一个 Shadow root 附加到一个 Custom element 上，并且将 `mode` 设置为 `closed`，那么就不可以从外部获取 Shadow DOM 了——`myCustomElem.shadowRoot` 将会返回 `null`。浏览器中的某些内置元素就是如此，例如`<video>`，包含了不可访问的 Shadow DOM。

> **备注：** 正如[这篇文章所阐述的](https://blog.revillweb.com/open-vs-closed-shadow-dom-9f3d7427d1af)，处理 closed Shadow DOM 实际上很简单，往往也很值得这么做。

如果你想将一个 Shadow DOM 附加到 custom element 上，可以在 custom element 的构造函数中添加如下实现（目前，这是 shadow DOM 最实用的用法）：

```js
let shadow = this.attachShadow({ mode: "open" });
```

将 Shadow DOM 附加到一个元素之后，就可以使用 DOM APIs 对它进行操作，就和处理常规 DOM 一样。

```js
var para = document.createElement('p');
shadow.appendChild(para);
etc.
```

## 编写简单示例

现在，让我们着手实现一个示例——[`<popup-info-box>`](https://github.com/mdn/web-components-examples/tree/master/popup-info-box-web-component)（也可以查看[在线示例](https://mdn.github.io/web-components-examples/popup-info-box-web-component/)），来说明 Shadow DOM 在 custom element 中的实际运用。它包含一个图片 icon 和一段文字，并将 icon 嵌入页面之中。每当 icon 获取到焦点时，文字会在一个弹框中显示，以提供更加详细的信息。首先，在 JavaScript 文件中，我们需要定义一个叫做 `PopUpInfo` 的类，它继承自 `HTMLElement`：

```js
class PopUpInfo extends HTMLElement {
  constructor() {
    // 必须首先调用 super 方法
    super();

    // 元素的具体功能写在下面

    ...
  }
}
```

在上面的类中，我们将会在它的构造函数中定义元素所有的功能。当类实例化后，所有的实例元素都会有相同功能。

### 创建 shadow root

在构造函数中，我们首先将 Shadow root 附加到 custom element 上：

```js
// 创建 shadow root
var shadow = this.attachShadow({ mode: "open" });
```

### 创建 shadow DOM 结构

接下来，我们会使用相关 DOM 操作来创建元素的 Shadow DOM 结构：

```js
// 创建 span
var wrapper = document.createElement("span");
wrapper.setAttribute("class", "wrapper");
var icon = document.createElement("span");
icon.setAttribute("class", "icon");
icon.setAttribute("tabindex", 0);
var info = document.createElement("span");
info.setAttribute("class", "info");

// 获取属性的内容并将内容添加到 info 元素内
var text = this.getAttribute("text");
info.textContent = text;

// 插入 icon
var imgUrl;
if (this.hasAttribute("img")) {
  imgUrl = this.getAttribute("img");
} else {
  imgUrl = "img/default.png";
}
var img = document.createElement("img");
img.src = imgUrl;
icon.appendChild(img);
```

### 为 shadow DOM 添加样式

之后，我们将要创建 {{htmlelement("style")}} 元素，并加入一些 CSS 样式：

```js
// 为 shadow DOM 添加一些 CSS 样式
var style = document.createElement("style");

style.textContent = `
.wrapper {
  position: relative;
}

.info {
  font-size: 0.8rem;
  width: 200px;
  display: inline-block;
  border: 1px solid black;
  padding: 10px;
  background: white;
  border-radius: 10px;
  opacity: 0;
  transition: 0.6s all;
  position: absolute;
  bottom: 20px;
  left: 10px;
  z-index: 3;
}

img {
  width: 1.2rem;
}

.icon:hover + .info, .icon:focus + .info {
  opacity: 1;
}`;
```

### 将 Shadow DOM 添加到 Shadow root 上

最后，将所有创建的元素添加到 Shadow root 上：

```js
// 将所创建的元素添加到 Shadow DOM 上
shadow.appendChild(style);
shadow.appendChild(wrapper);
wrapper.appendChild(icon);
wrapper.appendChild(info);
```

### 使用我们的 custom element

完成类的定义之后，使用元素也是一样简单，只需将 custom element 放在页面上，正如 [Using custom element](/zh-CN/docs/Web/API/Web_components/Using_custom_elements) 中讲解的那样：

```js
// 定义新的元素
customElements.define("popup-info", PopUpInfo);
```

```html
<popup-info
  img="img/alt.png"
  text="Your card validation code (CVC) is an extra security feature — it is the last 3 or 4 numbers on the back of your card."></popup-info>
```

### 使用外部引入的样式

在上面的示例中，我们使用行内{{htmlelement("style")}} 元素为 Shadow DOM 添加样式，但是完全可以通过{{htmlelement("link")}} 标签引用外部样式表来替代行内样式。

例如，我们可以看下 [popup-info-box-external-stylesheet](https://mdn.github.io/web-components-examples/popup-info-box-external-stylesheet/) 例子 (查看源代码 [source code](https://github.com/mdn/web-components-examples/blob/master/popup-info-box-external-stylesheet/main.js)):

```js
// 将外部引用的样式添加到 Shadow DOM 上
const linkElem = document.createElement("link");
linkElem.setAttribute("rel", "stylesheet");
linkElem.setAttribute("href", "style.css");

// 将所创建的元素添加到 Shadow DOM 上

shadow.appendChild(linkElem);
```

请注意，因为{{htmlelement("link")}} 元素不会打断 shadow root 的绘制，因此在加载样式表时可能会出现未添加样式内容（FOUC），导致闪烁。

许多现代浏览器都对从公共节点克隆的或具有相同文本的{{htmlelement("style")}} 标签实现了优化，以允许它们共享单个支持样式表，通过这种优化，外部和内部样式的性能表现比较接近。

## 参见

- [使用 custom elements](/zh-CN/docs/Web/API/Web_components/Using_custom_elements)
- [使用 templates 与 slots](/zh-CN/docs/Web/API/Web_components/Using_templates_and_slots)
