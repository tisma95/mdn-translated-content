---
title: 实体报头
slug: Glossary/Entity_header
---

{{GlossarySidebar}}

> **备注：** 当前的 HTTP/1.1 规范中不再提及实体、实体报头和实体的体。这中的有些字段现在被称为表示头字段（[RFC 7231, section 3: Representations](https://datatracker.ietf.org/doc/html/rfc7231#section-3)）。

实体报头是描述了一个 HTTP 消息有效载荷（即关于消息主体的元数据）的 HTTP 报头，见 {{glossary("header", "HTTP header")}}。实体报头包括 {{HTTPHeader("Content-Length")}}、{{HTTPHeader("Content-Language")}}、{{HTTPHeader("Content-Encoding")}}、{{HTTPHeader("Content-Type")}} 和 {{HTTPHeader("Expires")}} 等。实体报头可能同时存在于 HTTP 请求和响应信息中。

尽管实体报头既非请求或响应报头，（由于它经常出现在请求头或响应头中）它们通常包含于此类概念中。

在下面的例子中，{{HTTPHeader("Content-Length")}} 是实体报头，而 {{HTTPHeader("Host")}} 和 {{HTTPHeader("User-Agent")}} 是请求报头。

```plain
POST /myform.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Content-Length: 128
```

## 更多信息

### 技术知识

- [所有 HTTP 报头列表](/zh-CN/docs/Web/HTTP/Headers)
