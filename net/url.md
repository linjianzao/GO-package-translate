Package url

Overview ▾

Package url parses URLs and implements query escaping. See RFC 3986.
url包 解析URL 并实现查询转义



func QueryEscape
```golang
func QueryEscape(s string) string
```
QueryEscape escapes the string so it can be safely placed inside a URL query.
QueryEscape 转义 字符串 让它能安全地放置在URL查询。


func QueryUnescape
```golang
func QueryUnescape(s string) (string, error)
```
QueryUnescape does the inverse transformation of QueryEscape, converting %AB into the byte 0xAB and '+' into ' ' (space). 
It returns an error if any % is not followed by two hexadecimal digits.
QueryUnescape 逆转换 QueryEscape,转换  %AB  成字节0xAB 和 '+'  成 ' ' (空格).
如果任何% 没有随后跟着两个16进制 码  它返回一个错误.



type Error
```golang
type Error struct {
        Op  string
        URL string
        Err error
}
```
Error reports an error and the operation and URL that caused it.
Error 报告 一个错误和 操作 和引起它的URL


func (*Error) Error
```golang
func (e *Error) Error() string
```


type EscapeError
```golang
type EscapeError string
```



func (EscapeError) Error
```golang
func (e EscapeError) Error() string
```


type URL
```golang
type URL struct {
        Scheme   string
        Opaque   string    // encoded opaque data
        User     *Userinfo // username and password information
        Host     string    // host or host:port
        Path     string
        RawQuery string // encoded query values, without '?'
        Fragment string // fragment for references, without '#'
}
```
A URL represents a parsed URL (technically, a URI reference). The general form represented is:
URL 表示一个解析的URL(技术上，一个URI引用). 通常的形式时:
```golang
scheme://[userinfo@]host/path[?query][#fragment]
```

URLs that do not start with a slash after the scheme are interpreted as:
URL 在解释成 以下的形式 后不以一个斜杆开头:  

```golang
scheme:opaque[?query][#fragment]
```

Note that the Path field is stored in decoded form: /%47%6f%2f becomes /Go/. 
A consequence is that it is impossible to tell which slashes in the Path were slashes in the raw URL and which were %2f. 
This distinction is rarely important, but when it is a client must use other routines to parse the raw URL or construct the parsed URL. 
For example, an HTTP server can consult req.RequestURI, and an HTTP client can use URL{Host: "example.com", Opaque: "//example.com/Go%2f"} instead of URL{Host: "example.com", Path: "/Go/"}.
注意 Path 字段 存储 解码形式: /%47%6f%2f 成/Go/.
意味着  它可能告诉 Path里的 斜杆 是 斜线的原始URL 是 %2f.
这个区别是重要的很少. 但是 当它是一个 客户端 必须使用其他例程 来解析原始URL 或构造解析的URL。
例如, 一个HTTP 服务器 可以  参照  req.RequestURI, 和一个 HTTP 客户端可以使用URL{Host: "example.com", Opaque: "//example.com/Go%2f"} 代替URL{Host: "example.com", Path: "/Go/"}.


▾ Example
```golang
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	u, err := url.Parse("http://bing.com/search?q=dotnet")
	if err != nil {
		log.Fatal(err)
	}
	u.Scheme = "https"
	u.Host = "google.com"
	q := u.Query()
	q.Set("q", "golang")
	u.RawQuery = q.Encode()
	fmt.Println(u)
}
```


func Parse
```golang
func Parse(rawurl string) (url *URL, err error)
```
Parse parses rawurl into a URL structure. The rawurl may be relative or absolute.
Parse 解析rawurl 成 URL结构.rawurl 可以是相对路径或绝对路径


func ParseRequestURI
```golang
func ParseRequestURI(rawurl string) (url *URL, err error)
```
ParseRequestURI parses rawurl into a URL structure. 
It assumes that rawurl was received in an HTTP request, so the rawurl is interpreted only as an absolute URI or an absolute path. 
The string rawurl is assumed not to have a #fragment suffix. (Web browsers strip #fragment before sending the URL to a web server.)
ParseRequestURI 解析rawurl 成 URL结构.
它假设 rawurl 接收HTTP请求, 那样 rawurl 解释 成 一个绝对URI 或一个绝对路径.
字符串 rawurl 假设  没有 一个  #判断 后缀.(Web 浏览器 在发送URL到一个web浏览器之前带#片段)


func (*URL) IsAbs
```golang
func (u *URL) IsAbs() bool
```
IsAbs returns true if the URL is absolute.
IsAbs 如果URL是 绝对路径返回一个true


func (*URL) Parse
```golang
func (u *URL) Parse(ref string) (*URL, error)
```
Parse parses a URL in the context of the receiver. 
The provided URL may be relative or absolute. 
Parse returns nil, err on parse failure, otherwise its return value is the same as ResolveReference.
Parse 解析一个URL 在接收器的上下文中。
提供的URL 可能是 相对或绝对.
Parse 返回nil, err 解析失败,否则 它返回 值 相同的ResolveReference



func (*URL) Query
```golang
func (u *URL) Query() Values
```
Query parses RawQuery and returns the corresponding values.
Query解析RawQuery 并 返回 相应的值.



func (*URL) RequestURI
```golang
func (u *URL) RequestURI() string
```
RequestURI returns the encoded path?query or opaque?query string that would be used in an HTTP request for u.
RequestURI 返回编码  path?query或 opaque?query 字符串, 将在u的 HTTP请求中使用。



func (*URL) ResolveReference
```golang
func (u *URL) ResolveReference(ref *URL) *URL
```
ResolveReference resolves a URI reference to an absolute URI from an absolute base URI, per RFC 3986 Section 5.2. 
The URI reference may be relative or absolute. 
ResolveReference always returns a new URL instance, even if the returned URL is identical to either the base or reference. 
If ref is an absolute URL, then ResolveReference ignores base and returns a copy of ref.
ResolveReference 从绝对基础URI 解析一个URI 引用成一个绝对URI, 在RFC 3986 的 5.2节.
ResolveReference 经常会返回一个新的URL实例, 即使 返回的URL 和基础或引用的一样.
如果ref是一个绝对URL,然后ResolveReference 会忽略基础 并返回一个ref的复制.


func (*URL) String
```golang
func (u *URL) String() string
```
String reassembles the URL into a valid URL string.
String重新组合URL成一个有效的字符串.


type Userinfo
```golang
type Userinfo struct {
        // contains filtered or unexported fields
}
```
The Userinfo type is an immutable encapsulation of username and password details for a URL. 
An existing Userinfo value is guaranteed to have a username set (potentially empty, as allowed by RFC 2396), and optionally a password.
Userinfo 类型 是一个 不可改变 封装username 和 password 详细信息的URL.
一个存在的Userinfo 值 保证由 设置 username(潜在的空,在 RFC 2396中是允许的 ),  password 是可选的.


func User
```golang
func User(username string) *Userinfo
```
User returns a Userinfo containing the provided username and no password set.
User 返回一个  包含提供的username 并没有设置password 的 Userinfo


func UserPassword
```golang
func UserPassword(username, password string) *Userinfo
```
UserPassword returns a Userinfo containing the provided username and password. 
This functionality should only be used with legacy web sites. 
RFC 2396 warns that interpreting Userinfo this way “is NOT RECOMMENDED, 
	because the passing of authentication information in clear text (such as URI) has proven to be a security risk in almost every case where it has been used.”
UserPassword 返回一个Userinfo 包含 提供的 username 和 password.
这个功能 应该只使用在传统的web站点.
RFC 2396 警告 解释Userinfo的方式 “is NOT RECOMMENDED, 因为 通过 验证的信息 明文(例如 URI) 已被证明是在它已被用于几乎所有的情况下的安全风险



func (*Userinfo) Password
```golang
func (u *Userinfo) Password() (string, bool)
```
Password returns the password in case it is set, and whether it is set.
Password 返回密码设置的情况,和 它是否设置.


func (*Userinfo) String
```golang
func (u *Userinfo) String() string
```
String returns the encoded userinfo information in the standard form of "username[:password]".
String 返回 编码 用户信息 成标准形式"username[:password]".


func (*Userinfo) Username
```golang
func (u *Userinfo) Username() string
``
Username returns the username.
Username 返回用户名



type Values
```golang
type Values map[string][]string
```
Values maps a string key to a list of values. 
It is typically used for query parameters and form values. 
Unlike in the http.Header map, the keys in a Values map are case-sensitive.
Values映射一个字符串键成值列表.
它通常 使用 在查询参数和值 形式.
不像在 http.Header map, Values里的map 的键是区分大小写的.



▹ Example
```golang
package main

import (
	"fmt"
	"net/url"
)

func main() {
	v := url.Values{}
	v.Set("name", "Ava")
	v.Add("friend", "Jess")
	v.Add("friend", "Sarah")
	v.Add("friend", "Zoe")
	// v.Encode() == "name=Ava&friend=Jess&friend=Sarah&friend=Zoe"
	fmt.Println(v.Get("name"))
	fmt.Println(v.Get("friend"))
	fmt.Println(v["friend"])
}
```



func ParseQuery
```golang
func ParseQuery(query string) (m Values, err error)
```
ParseQuery parses the URL-encoded query string and returns a map listing the values specified for each key. 
ParseQuery always returns a non-nil map containing all the valid query parameters found; 
err describes the first decoding error encountered, if any.
ParseQuery 解析URL编码查询字符串 并 返回 map列出每个键指定的值。
ParseQuery通常返回一个 非nilmao 包含所有发现的有效的查询参数.
如果有任何错误,err描述 遇到的第一个解码错误.



func (Values) Add
```golang
func (v Values) Add(key, value string)
```
Add adds the value to key. It appends to any existing values associated with key.
Add 添加value 到key.它追加到任何key对应的存在的值.



func (Values) Del
```golang
func (v Values) Del(key string)
```
Del deletes the values associated with key.
Del 删除和key对应的值.


func (Values) Encode
```golang
func (v Values) Encode() string
```
Encode encodes the values into “URL encoded” form ("bar=baz&foo=quux") sorted by key.
Encode 编码值成  “URL encoded” 形式("bar=baz&foo=quux") 以键值排序 


func (Values) Get
```golang
func (v Values) Get(key string) string
```
Get gets the first value associated with the given key. 
If there are no values associated with the key, Get returns the empty string. 
To access multiple values, use the map directly.
Get 获取 给定key对应的 第一个值.
如果key 没有对应的值,Get 返回空字符串.
若要访问 多个值,直接使用map


func (Values) Set
```golang
func (v Values) Set(key, value string)
```
Set sets the key to value. It replaces any existing values.
Set 设置key 对应的value. 它替代任何存在的值
























