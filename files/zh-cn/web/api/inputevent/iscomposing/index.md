---
title: InputEvent.isComposing
slug: Web/API/InputEvent/isComposing
---

{{APIRef("DOM Events")}}

The **`InputEvent.isComposing`** read-only property returns a {{jsxref("Boolean")}} value indicating if the event is fired after [`compositionstart`](/zh-CN/docs/Web/API/Element/compositionstart_event) and before [`compositionend`](/zh-CN/docs/Web/API/Element/compositionend_event).

## 这是一个只读属性，返回 boolean 类型。表示正处于输入事件的开始与结束之间，表示正在输入状态。

## Syntax

```plain
var bool = event.isComposing;
```

## Example

```js
var inputEvent = new InputEvent("syntheticInput", false);
console.log(inputEvent.isComposing); // return false
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [`compositionstart`](/zh-CN/docs/Web/API/Element/compositionstart_event) and [`compositionend`](/zh-CN/docs/Web/API/Element/compositionend_event)
- {{domxref("InputEvent")}}
