---
title: HSTS
slug: Glossary/HSTS
---

{{GlossarySidebar}}

**HTTP 严格传输安全** 让网站可以通知浏览器它不应该再使用 HTTP 加载该网站，而是自动转换该网站的所有的 HTTP 链接至更安全的 HTTPS。它包含在 HTTP 的协议头 {{HTTPHeader("Strict-Transport-Security")}} 中，在服务器返回资源时带上。

换句话说，它告诉浏览器将 URL 协议从 HTTP 更改为 HTTPS（会更安全），并要求浏览器对每个请求执行此操作。

## 参见

- {{HTTPHeader("Strict-Transport-Security")}}
- OWASP 文章：[HTTP Strict Transport Security](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security)
- 维基百科上的 [HTTP 严格传输安全](https://zh.wikipedia.org/wiki/HTTP_严格传输安全)
