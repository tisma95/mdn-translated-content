---
title: "EventTarget: dispatchEvent() メソッド"
slug: Web/API/EventTarget/dispatchEvent
l10n:
  sourceCommit: 339595951b78774e951b1a9d215a6db6b856f6b2
---

{{APIRef("DOM")}}

**`dispatchEvent()`** は {{domxref("EventTarget")}} のメソッドで、 {{domxref("Event")}} をそのオブジェクトへ送り、関連する[イベントリスナー](/ja/docs/Web/API/EventTarget/addEventListener)を（同期的に）適切に起動させます。通常のイベント処理ルール（キャプチャとオプションのバブリングフェーズを含む）は `dispatchEvent()` で手動で配信されたイベントにも適用されます。

`dispatchEvent()` の呼び出しは、イベントを発行させるための最後のステップです。イベントは既に {{domxref("Event/Event", "Event()")}} コンストラクターを使って作成され、初期化されているはずです。

> **メモ:** このメソッドを呼び出すとき、 {{domxref("Event.target")}} プロパティは現在の `EventTarget` に初期化されます。

「ネイティブ」イベントがブラウザーによって発生し、[イベントループ](/ja/docs/Web/JavaScript/Event_loop)を介して非同期にイベントハンドラーを呼び出すのとは異なり、 `dispatchEvent()` はイベントハンドラーを*同期的に*呼び出します。適用可能なすべてのイベントハンドラが呼び出され、 `dispatchEvent()` から戻る前に返されます。

## 構文

```js-nolint
dispatchEvent(event)
```

### 引数

- `event`
  - : 配信する {{domxref("Event")}} オブジェクトです。この {{domxref("Event.target")}} プロパティは現在の {{domxref("EventTarget")}} に設定されます。

### 返値

`event` がキャンセル可能で、かつ `event` を受け取ったイベントハンドラーの少なくとも 1 つが {{domxref("Event.preventDefault()")}} を呼び出した場合は `false` となります。それ以外の場合は `true` です。

### 例外

- `InvalidStateError` {{domxref("DomException")}}
  - : イベントの初期化時に、イベントの種類が指定されなかった場合に発生します。

> **警告:** イベントハンドラーで発生した例外は、捕捉されない例外として報告されます。イベントハンドラーはネストされたコールスタック上で実行されます。イベントハンドラーが完了するまで呼び出し元はブロックされますが、例外は呼び出し元まで伝搬しません。

## 例

[イベントの作成と起動](/ja/docs/Web/Events/Creating_and_triggering_events) を参照してください。

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- [Event オブジェクトリファレンス](/ja/docs/Web/API/Event)
