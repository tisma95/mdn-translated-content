---
title: Document.exitPointerLock()
slug: Web/API/Document/exitPointerLock
---

{{ apiref("DOM") }}

{{ seeCompatTable }}

`exitPointerLock` 方法可异步的解锁鼠标（通过{{domxref("Element.requestPointerLock")}}锁定的）。

追踪是否解锁成功，需要监听[`pointerlockchange`](/zh-CN/docs/Web/API/Document/pointerlockchange_event) 和[`pointerlockerror`](/zh-CN/docs/Web/API/Document/pointerlockerror_event) 事件。

## 语法

```plain
document.exitPointerLock();
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- {{ domxref("Document.pointerLockElement") }}
- {{ domxref("Element.requestPointerLock()") }}
- [Pointer Lock](/zh-CN/docs/WebAPI/Pointer_Lock)
