Cookie
====
Cookie 是服务器保存在浏览器的一小段文本信息，每个 Cookie 的大小一般不能超过4KB。浏览器每次向服务器发出请求，就会自动附上这段信息。

Cookie 主要用来分辨两个请求是否来自同一个浏览器，以及用来保存一些状态信息。它的常用场合有以下一些。

对话（session）管理：保存登录、购物车等需要记录的信息。
个性化：保存用户的偏好，比如网页的字体大小、背景色等等。
追踪：记录和分析用户行为


# Cookie 的生成

http响应放置Set-Cookie字段

服务器如果希望在浏览器保存 Cookie，就要在 HTTP 回应的头信息里面，放置一个Set-Cookie字段。

`Set-Cookie:foo=bar`会在浏览器保存一个名为foo的 Cookie，它的值为bar

除了 Cookie 的值，Set-Cookie字段还可以附加 Cookie 的属性。可以同时包括多个属性，没有次序的要求。

`Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly`

服务器想改变一个早先设置的 Cookie，必须同时满足四个条件：Cookie 的key、domain、path和secure都匹配。举例来说，如果原始的 Cookie 是用如下的Set-Cookie设置的

# Cookie 的发送

浏览器向服务器发送 HTTP 请求时，每个请求都会带上相应的 Cookie。也就是说，把服务器早前保存在浏览器的这段信息，再发回服务器。这时要使用 HTTP 头信息的Cookie字段。

# Cookie 的属性

- Expires， Max-Age

Expires： 到期时间，指定一个具体的到期时间，到了指定时间以后，浏览器就不再保留这个 Cookie。

Max-Age： Cookie 存在的秒数。比如60 * 60 * 24 * 365（即一年），过了这个时间以后，浏览器就不再保留这个 Cookie。

如果同时指定了Expires和Max-Age，那么Max-Age的值将优先生效

- Domain，Path

Domain：域名，哪些域名要附带这个 Cookie

Path： 路径，指定浏览器发出 HTTP 请求时，哪些路径要附带这个 Cookie。

- Secure，HttpOnly

Secure： 开关，没有值。指定浏览器只有在加密协议 HTTPS 下，才能将这个 Cookie 发送到服务器。另一方面，如果当前协议是 HTTP，浏览器会自动忽略服务器发来的Secure属性。该属性只是一个开关，不需要指定值。如果通信是 HTTPS 协议，该开关自动打开。

HttpOnly： 指定该 Cookie 无法通过 JavaScript 脚本拿到

# 读写当前网页的 Cookie

读： `document.cookie // "foo=bar;baz=bar"`

写：`document.cookie = 'fontSize=14';`