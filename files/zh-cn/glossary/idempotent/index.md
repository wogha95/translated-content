---
title: 幂等
slug: Glossary/Idempotent
---

{{GlossarySidebar}}

一个 HTTP 方法是**幂等**的，指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的。换句话说就是，幂等方法不应该具有副作用（统计用途除外）。在正确实现的条件下， {{HTTPMethod("GET")}} ， {{HTTPMethod("HEAD")}} ， {{HTTPMethod("PUT")}} 和 {{HTTPMethod("DELETE")}} 等方法都是**幂等**的，而 {{HTTPMethod("POST")}} 方法不是。所有的 {{glossary("safe")}} 方法也都是幂等的。

幂等性只与后端服务器的实际状态有关，而每一次请求接收到的状态码不一定相同。例如，第一次调用 {{HTTPMethod("DELETE")}} 方法有可能返回 {{HTTPStatus("200")}} ，但是后续的请求可能会返回 {{HTTPStatus("404")}} 。 {{HTTPMethod("DELETE")}} 的言外之意是，开发者不应该使用 `DELETE` 法实现具有删除最后条目功能的 RESTful API。

需要注意的是，服务器不一定会确保请求方法的幂等性，有些应用可能会错误地打破幂等性约束。

`GET /pageX HTTP/1.1` 幂等的。连续调用多次，客户端接收到的结果都是一样的：

```
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
GET /pageX HTTP/1.1
```

`POST /add_row HTTP/1.1` 不是幂等的。如果调用多次，就会增加多行记录：

```
POST /add_row HTTP/1.1
POST /add_row HTTP/1.1   -> Adds a 2nd row
POST /add_row HTTP/1.1   -> Adds a 3rd row
```

`DELETE /idX/delete HTTP/1.1` 是幂等的，即便是不同请求之间接收到的状态码不一样：

```
DELETE /idX/delete HTTP/1.1   -> Returns 200 if idX exists
DELETE /idX/delete HTTP/1.1   -> Returns 404 as it just got deleted
DELETE /idX/delete HTTP/1.1   -> Returns 404
```

## 参见

- 在 HTTP 协议中[幂等](https://tools.ietf.org/html/rfc7231#section-4.2.2)的定义。
- 常见的幂等方法：{{HTTPMethod("GET")}}、{{HTTPMethod("HEAD")}}、{{HTTPMethod("PUT")}}、{{HTTPMethod("DELETE")}}、{{HTTPMethod("OPTIONS")}}、{{HTTPMethod("TRACE")}}
- 常见的非幂等方法：{{HTTPMethod("POST")}}、{{HTTPMethod("PATCH")}}、{{HTTPMethod("CONNECT")}}
