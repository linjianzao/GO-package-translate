Package cookiejar

Overview ▾

Package cookiejar implements an in-memory RFC 6265-compliant http.CookieJar.
cookiejar包 实现一个 内存 的 于 RFC 6265 兼容的 http.CookieJar. 


type Jar
```golang
type Jar struct {
        // contains filtered or unexported fields
}
```
Jar implements the http.CookieJar interface from the net/http package.
Jar 从net/http 中 实现 http.CookieJar接口



func New
```golang
func New(o *Options) (*Jar, error)
```
New returns a new cookie jar. A nil *Options is equivalent to a zero Options.
New返回一个新的 cookie jar. 一个 nil *Options 等价于 零 Options.



func (*Jar) Cookies
```golang
func (j *Jar) Cookies(u *url.URL) (cookies []*http.Cookie)
```
Cookies implements the Cookies method of the http.CookieJar interface.

It returns an empty slice if the URL's scheme is not HTTP or HTTPS.
Cookies 实现http.CookieJar接口的Cookie 的方法。
如果 URL 不是  HTTP 或 HTTPS 它返回一个空的 slice.



func (*Jar) SetCookies
```golang
func (j *Jar) SetCookies(u *url.URL, cookies []*http.Cookie)
```
SetCookies implements the SetCookies method of the http.CookieJar interface.

It does nothing if the URL's scheme is not HTTP or HTTPS.
SetCookies 实现http.CookieJar接口的SetCookies方法。
如果 URL 不是  HTTP 或 HTTPS 它不会做任何事情.


type Options
```golang
type Options struct {
        // PublicSuffixList is the public suffix list that determines whether
        // an HTTP server can set a cookie for a domain.
        //
        // A nil value is valid and may be useful for testing but it is not
        // secure: it means that the HTTP server for foo.co.uk can set a cookie
        // for bar.co.uk.
        //PublicSuffixList 是一个 公共后缀列表, 确定HTTP 服务端 是否可以 和给域设置 cookie.
         // nil值 对测试来说是有效的 和有用的 但是 是不安全的: 它意味着 foo.co.uk的 HTTP 服务器 可以设置 bar.co.uk 的cookie.
        
        PublicSuffixList PublicSuffixList
}
```
Options are the options for creating a new Jar.
Options 是 创建一个新的Jar 选项.


type PublicSuffixList
```golang
type PublicSuffixList interface {
        // PublicSuffix returns the public suffix of domain.
        //
        // TODO: specify which of the caller and callee is responsible for IP
        // addresses, for leading and trailing dots, for case sensitivity, and
        // for IDN/Punycode.
        //PublicSuffix 返回 域的公共后缀。
        //TODO: 指定 调用者和 被调用者响应的 IP地址,为开头和结尾点，为区分大小写 , 关于IDN/Punycode的。
        
        PublicSuffix(domain string) string


        // String returns a description of the source of this public suffix
        // list. The description will typically contain something like a time
        // stamp or version number.
        //String 返回一个描述的 公用后缀列表 的源. 描述 通常 包含 一些 类似时间戳或版本号
        
        String() string
}
```
PublicSuffixList provides the public suffix of a domain. For example:
PublicSuffixList 提供域的公用后缀 . 例如:

```golang
- the public suffix of "example.com" is "com",
- the public suffix of "foo1.foo2.foo3.co.uk" is "co.uk", and
- the public suffix of "bar.pvt.k12.ma.us" is "pvt.k12.ma.us".
```

Implementations of PublicSuffixList must be safe for concurrent use by multiple goroutines.

An implementation that always returns "" is valid and may be useful for testing but it is not secure: it means that the HTTP server for foo.com can set a cookie for bar.com.

A public suffix list implementation is in the package code.google.com/p/go.net/publicsuffix.
 
实现PublicSuffixList  多个goroutines 并发必须是安全的.

对于测试 总是返回 " " 是有效的  并且可能很有用 但是 它是不安全的:它意味者 HTTP服务器  foo.com 可以设置  bar.com的cookie
公用后缀列表实现在  code.google.com/p/go.net/publicsuffix 包里.




