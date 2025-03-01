---
title: KeyboardEvent.isComposing
slug: Web/API/KeyboardEvent/isComposing
l10n:
  sourceCommit: 277e5969c63b97cfb55ab4a0e612e8040810f49b
---

{{APIRef("UI Events")}}

**`KeyboardEvent.isComposing`** は読み取り専用プロパティで、イベントが変換セッションの途中、すなわち {{domxref("Element/compositionstart_event", "compositionstart")}} の後かつ {{domxref("Element/compositionend_event", "compositionend")}} の前に発行されたことを示す論理値を返します。

## 値

論理値です。

## 例

```js
const kbdEvent = new KeyboardEvent("syntheticKey", false);
console.log(kbdEvent.isComposing); // false を返す
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- {{domxref("Element/compositionstart_event", "compositionstart")}} および {{domxref("Element/compositionend_event", "compositionend")}}
- {{domxref("KeyboardEvent")}}
