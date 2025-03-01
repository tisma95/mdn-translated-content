---
title: 域名分片 (Domain sharding)
slug: Glossary/Domain_sharding
---

{{GlossarySidebar}}

由于浏览器限制了每个域（domain）的活动连接数。为了可以同时下载超过该限制数的资源，**域名分片**（**domain sharding**）会将内容拆分到多个子域中。当使用多个域来处理多个资源时，浏览器能够同时下载更多资源，从而缩短了页面加载时间并改善了用户体验。

就性能而言，域名分片的问题在于每个域都需要额外的 DNS 查找成本以及建立每个 TCP 连接的开销。

来自 HTTP 请求的初始响应通常是 HTML 文件，其中列出了其他资源，例如 JavaScript，CSS，图像和其他需要下载的媒体文件。由于浏览器限制每个域的活动连接数，因此从一个域提供所有必需的资源可能会变慢，因为需要按顺序下载资源。使用域名分片后，所需的下载可以来自多个域，从而使浏览器可以同时下载所需的资源。但是，由于 DNS 查找会减慢初始加载时间，因此多个域是一种反模式。

由于 HTTP/2 没有限制并发请求（unlimited concurrent requests），因此启用 HTTP/2 后，就没必要再使用域名分片来解决并发限制了。

## See Also

- [Transport Layer Security (TLS)](/zh-CN/docs/Archive/Security/SSL_and_TLS)
- [DNS](/zh-CN/docs/Glossary/DNS)
- [HTTP/2](/zh-CN/docs/Glossary/HTTP_2)
