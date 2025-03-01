---
title: CSSRule.parentRule
slug: Web/API/CSSRule/parentRule
l10n:
  sourceCommit: bf30e32f3b51f59080f2c76795beadb247a551eb
---

{{ APIRef("CSSOM") }}

**`parentRule`** は {{domxref("CSSRule")}} インターフェイスのプロパティで、現在のルールを内包するルールがあれば、それを返します。なければ null を返します。

## 値

包含するルールの型の {{domxref("CSSRule")}} を返します。現在のルールがメディアクエリーの中にある場合は、 {{domxref("CSSMediaRule")}} になります。包含するルールがない場合は null を返します。

## 例

```css
@media (min-width: 500px) {
  .box {
    width: 100px;
    height: 200px;
    background-color: red;
  }

  body {
    color: blue;
  }
}
```

```js
let myRules = document.styleSheets[0].cssRules;
let childRules = myRules[0].cssRules;
console.log(childRules[0].parentRule); // a CSSMediaRule
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}
