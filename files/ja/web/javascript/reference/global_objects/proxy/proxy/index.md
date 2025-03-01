---
title: Proxy() コンストラクター
slug: Web/JavaScript/Reference/Global_Objects/Proxy/Proxy
l10n:
  sourceCommit: fcd80ee4c8477b6f73553bfada841781cf74cf46
---

{{JSRef}}

**`Proxy()`** コンストラクターは {{jsxref("Proxy")}} オブジェクトを生成するために使用します。

## 構文

```js-nolint
new Proxy(target, handler)
```

> **メモ:** `Proxy()` は、[`new`](/ja/docs/Web/JavaScript/Reference/Operators/new) を使ってのみ構築することができます。`new` なしで呼び出そうとすると、{{jsxref("TypeError")}} が発生します。

### 引数

- `target`
  - : `Proxy` でラップするターゲットのオブジェクトです。あらゆる種類のオブジェクト、例えばネイティブ配列や関数、別のプロキシーなどがなることができます。
- `handler`
  - : 関数をプロパティとして持つオブジェクトで、その関数で、 Proxy `p` に対して操作が行われた場合の挙動を定義します。

## 解説

`Proxy()` コンストラクターを使用すると、新しい `Proxy` オブジェクトを生成できます。
このコンストラクターは 2 つの必須の引数を取ります。

- `target` はプロキシーを作成するオブジェクトです。
- `handler` はプロキシーのカスタム動作を定義するオブジェクトです。

handler を空にすると、ほとんどすべての点でターゲットとまったく同じように振る舞うプロキシーを作成します。 `handler` オブジェクト上で関数群のいずれかを定義することで、プロキシーの動作の特定の側面をカスタマイズすることができます。例えば、 `get()` を定義することで、 ターゲットの[プロパティアクセサー](/ja/docs/Web/JavaScript/Reference/Operators/Property_Accessors)のカスタマイズされたバージョンを提供することができます。

### ハンドラー関数

この節では、定義することができるすべてのハンドラー関数を列挙します。ハンドラー関数は、対象オブジェクトの呼び出しをトラップするので、*トラップ*と呼ばれることがあります。

- {{JSxRef("Global_Objects/Proxy/Proxy/apply", "handler.apply()")}}
  - : 関数呼び出しのトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/construct", "handler.construct()")}}
  - : {{JSxRef("Operators/new", "new")}} 演算子のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/defineProperty", "handler.defineProperty()")}}
  - : {{JSxRef("Object.defineProperty")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/deleteProperty", "handler.deleteProperty()")}}
  - : {{JSxRef("Operators/delete", "delete")}} 演算子のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/get", "handler.get()")}}
  - : プロパティ値の取得のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/getOwnPropertyDescriptor", "handler.getOwnPropertyDescriptor()")}}
  - : {{JSxRef("Object.getOwnPropertyDescriptor")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/getPrototypeOf", "handler.getPrototypeOf()")}}
  - : {{JSxRef("Object.getPrototypeOf")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/has", "handler.has()")}}
  - : {{JSxRef("Operators/in", "in")}} 演算子のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/isExtensible", "handler.isExtensible()")}}
  - : {{JSxRef("Object.isExtensible")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/ownKeys", "handler.ownKeys()")}}
  - : {{JSxRef("Object.getOwnPropertyNames")}} や {{JSxRef("Object.getOwnPropertySymbols")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/preventExtensions", "handler.preventExtensions()")}}
  - : {{JSxRef("Object.preventExtensions")}} のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/set", "handler.set()")}}
  - : プロパティ値の設定のトラップです。
- {{JSxRef("Global_Objects/Proxy/Proxy/setPrototypeOf", "handler.setPrototypeOf()")}}
  - : {{JSxRef("Object.setPrototypeOf")}} のトラップです。

## 例

### 選択的にプロパティアクセサーのプロキシーを行う

この例では、ターゲットは `notProxied` と `proxied` の 2 つのプロパティを持っています。 `proxied` に別の値を返し、それ以外のアクセスをターゲットに許可するハンドラーを定義します。

```js
const target = {
  notProxied: "original value",
  proxied: "original value",
};

const handler = {
  get(target, prop, receiver) {
    if (prop === "proxied") {
      return "replaced value";
    }
    return Reflect.get(...arguments);
  },
};

const proxy = new Proxy(target, handler);

console.log(proxy.notProxied); // "original value"
console.log(proxy.proxied); // "replaced value"
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- [JavaScript Guide ガイドの `Proxy` と `Reflect`](/ja/docs/Web/JavaScript/Guide/Meta_programming)
- {{jsxref("Global_Objects/Reflect", "Reflect")}}
