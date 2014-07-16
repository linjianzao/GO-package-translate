Package http

import "net/http"

Overview ▾

Package http provides HTTP client and server implementations.


Get, Head, Post, and PostForm make HTTP (or HTTPS) requests:
http包提供 HTTP 客户端和服务端实现.
Get, Head, Post 和 PostForm 的HTTP (或 HTTPS)请求: 
```golang
resp, err := http.Get("http://example.com/")
...
resp, err := http.Post("http://example.com/upload", "image/jpeg", &buf)
...
resp, err := http.PostForm("http://example.com/form",
	url.Values{"key": {"Value"}, "id": {"123"}})
```	

The client must close the response body when finished with it:
结束的时候 客户端必须关闭响应体:
```golang
resp, err := http.Get("http://example.com/")
if err != nil {
	// handle error
}
defer resp.Body.Close()
body, err := ioutil.ReadAll(resp.Body)
// ...
```

For control over HTTP client headers, redirect policy, and other settings, create a Client:
对于通过HTTP客户端头 ,重定向策略 ,和其他设置 的控制,创建一个Client:
```golang
client := &http.Client{
	CheckRedirect: redirectPolicyFunc,
}

resp, err := client.Get("http://example.com")
// ...

req, err := http.NewRequest("GET", "http://example.com", nil)
// ...
req.Header.Add("If-None-Match", `W/"wyzzy"`)
resp, err := client.Do(req)
// ...
```

For control over proxies, TLS configuration, keep-alives, compression, and other settings, create a Transport:
对于代理，TLS配置,保持连接,压缩 和其他设置 创建一个Transport:
```golang
tr := &http.Transport{
	TLSClientConfig:    &tls.Config{RootCAs: pool},
	DisableCompression: true,
}
client := &http.Client{Transport: tr}
resp, err := client.Get("https://example.com")
```

Clients and Transports are safe for concurrent use by multiple goroutines and for efficiency should only be created once and re-used.
ListenAndServe starts an HTTP server with a given address and handler. 
The handler is usually nil, which means to use DefaultServeMux. Handle and HandleFunc add handlers to DefaultServeMux:
Clients 和 Transports 对于多个 goroutines 并发是安全的 并且为了 效率应该只创建一次 并重复使用
ListenAndServe 用给定的地址和处理程序 启动一个HTTP 服务.
handler通常是nil, 那意味着使用DefaultServeMux.Handle 和HandleFunc 添加处理程序到 DefaultServeMux
```golang
http.Handle("/foo", fooHandler)

http.HandleFunc("/bar", func(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
})

log.Fatal(http.ListenAndServe(":8080", nil))
```

More control over the server's behavior is available by creating a custom Server:
更好的控制 服务器的行为 , 可通过创建自定义  Server:
```golang
s := &http.Server{
	Addr:           ":8080",
	Handler:        myHandler,
	ReadTimeout:    10 * time.Second,
	WriteTimeout:   10 * time.Second,
	MaxHeaderBytes: 1 << 20,
}
log.Fatal(s.ListenAndServe())
```


Constants

const (
        StatusContinue           = 100
        StatusSwitchingProtocols = 101

        StatusOK                   = 200
        StatusCreated              = 201
        StatusAccepted             = 202
        StatusNonAuthoritativeInfo = 203
        StatusNoContent            = 204
        StatusResetContent         = 205
        StatusPartialContent       = 206

        StatusMultipleChoices   = 300
        StatusMovedPermanently  = 301
        StatusFound             = 302
        StatusSeeOther          = 303
        StatusNotModified       = 304
        StatusUseProxy          = 305
        StatusTemporaryRedirect = 307

        StatusBadRequest                   = 400
        StatusUnauthorized                 = 401
        StatusPaymentRequired              = 402
        StatusForbidden                    = 403
        StatusNotFound                     = 404
        StatusMethodNotAllowed             = 405
        StatusNotAcceptable                = 406
        StatusProxyAuthRequired            = 407
        StatusRequestTimeout               = 408
        StatusConflict                     = 409
        StatusGone                         = 410
        StatusLengthRequired               = 411
        StatusPreconditionFailed           = 412
        StatusRequestEntityTooLarge        = 413
        StatusRequestURITooLong            = 414
        StatusUnsupportedMediaType         = 415
        StatusRequestedRangeNotSatisfiable = 416
        StatusExpectationFailed            = 417
        StatusTeapot                       = 418

        StatusInternalServerError     = 500
        StatusNotImplemented          = 501
        StatusBadGateway              = 502
        StatusServiceUnavailable      = 503
        StatusGatewayTimeout          = 504
        StatusHTTPVersionNotSupported = 505
)
HTTP status codes, defined in RFC 2616.
HTTP状态码,定义在 RFC 2616.
```golang
const DefaultMaxHeaderBytes = 1 << 20 // 1 MB
```

DefaultMaxHeaderBytes is the maximum permitted size of the headers in an HTTP request. 
This can be overridden by setting Server.MaxHeaderBytes.
DefaultMaxHeaderBytes 是一个HTTP请求的 最大的允许 头 的大小. 这可以通过设置Server.MaxHeaderBytes 来 重写
```golang
const DefaultMaxIdleConnsPerHost = 2
```

DefaultMaxIdleConnsPerHost is the default value of Transport's MaxIdleConnsPerHost.
DefaultMaxIdleConnsPerHost 是 Transport的MaxIdleConnsPerHost 的 默认值.
```golang
const TimeFormat = "Mon, 02 Jan 2006 15:04:05 GMT"
```

TimeFormat is the time format to use with time.Parse and time.Time.Format when parsing or generating times in HTTP headers. 
It is like time.RFC1123 but hard codes GMT as the time zone.
TimeFormat 是 在HTTP头中 在解析或者生成时间的时候, 和time.Parse 和 time.Time.Format 一起使用的 时间格式


Variables
```golang
var (
        ErrHeaderTooLong        = &ProtocolError{"header too long"}
        ErrShortBody            = &ProtocolError{"entity body too short"}
        ErrNotSupported         = &ProtocolError{"feature not supported"}
        ErrUnexpectedTrailer    = &ProtocolError{"trailer header without chunked transfer encoding"}
        ErrMissingContentLength = &ProtocolError{"missing ContentLength in HEAD response"}
        ErrNotMultipart         = &ProtocolError{"request Content-Type isn't multipart/form-data"}
        ErrMissingBoundary      = &ProtocolError{"no multipart boundary param in Content-Type"}
)
```
```golang
var (
        ErrWriteAfterFlush = errors.New("Conn.Write called after Flush")
        ErrBodyNotAllowed  = errors.New("http: request method or response status code does not allow body")
        ErrHijacked        = errors.New("Conn has been hijacked")
        ErrContentLength   = errors.New("Conn.Write wrote more than the declared Content-Length")
)
```
Errors introduced by the HTTP server.
Errors 由HTTP服务器导入。

```golang
var DefaultClient = &Client{}
```
DefaultClient is the default Client and is used by Get, Head, and Post.
DefaultClient 是默认的Client 并且被用于Get, Head, 和 Post.

```golang
var DefaultServeMux = NewServeMux()
```
DefaultServeMux is the default ServeMux used by Serve.
DefaultServeMux 是默认的ServeMux  用于 Serve

```golang
var ErrBodyReadAfterClose = errors.New("http: invalid Read on closed Body")
```
ErrBodyReadAfterClose is returned when reading a Request or Response Body after the body has been closed.
This typically happens when the body is read after an HTTP Handler calls WriteHeader or Write on its ResponseWriter.
ErrBodyReadAfterClose 是 在body 关闭后读取一个 Request 或Response 返回的
通常时 在一个 HTTP Handler 在它的ResponseWriter  调用 WriteHeader 或 Write 之后读取 body 时发生的

```golang
var ErrHandlerTimeout = errors.New("http: Handler timeout")
```
ErrHandlerTimeout is returned on ResponseWriter Write calls in handlers which have timed out.
ErrHandlerTimeout 是在 handlers  中 调用 ResponseWriter Write 超时 时 返回的. 

```golang
var ErrLineTooLong = errors.New("header line too long")
```

```golang
var ErrMissingFile = errors.New("http: no such file")
```
ErrMissingFile is returned by FormFile when the provided file field name is either not present in the request or not a file field.
ErrMissingFile  当提供的文件字段名要么是不存在请求 要么 不是一个文件字段  通过 FormFile 返回


```golang
var ErrNoCookie = errors.New("http: named cookie not present")
```

```golang
var ErrNoLocation = errors.New("http: no Location header in response")
```



func CanonicalHeaderKey

func CanonicalHeaderKey(s string) string
CanonicalHeaderKey returns the canonical format of the header key s. 
The canonicalization converts the first letter and any letter following a hyphen to upper case; 
the rest are converted to lowercase. For example, the canonical key for "accept-encoding" is "Accept-Encoding".
CanonicalHeaderKey 返回 头 key  s的 规范格式.
规范化 第一个字母 和任何跟着连字符号的字母 转换 成大写.其余的都转换为小写。
例如 规范的key "accept-encoding"  是 "Accept-Encoding".


func DetectContentType

func DetectContentType(data []byte) string
DetectContentType implements the algorithm described at http://mimesniff.spec.whatwg.org/ to determine the Content-Type of the given data. 
It considers at most the first 512 bytes of data. 
DetectContentType always returns a valid MIME type: if it cannot determine a more specific one, it returns "application/octet-stream".
DetectContentType 用给定的 确定Content-Type 的  data  实现在 http://mimesniff.spec.whatwg.org/  描述的计算.
它考虑最多的前512个字节的数据。
DetectContentType 经常返回 一个 有效的 MIME 类型: 如果 它不能确定更具体的, 它返回 "application/octet-stream".


func Error

func Error(w ResponseWriter, error string, code int)
Error replies to the request with the specified error message and HTTP code. 
The error message should be plain text.
Error 响应请求使用指定的错误消息和HTTP代码。
错误信息必须是纯文本



func Handle

func Handle(pattern string, handler Handler)
Handle registers the handler for the given pattern in the DefaultServeMux. 
The documentation for ServeMux explains how patterns are matched.
Handle 在 DefaultServeMux 里 用给定的 pattern 注册handler
ServeMux的文档解释了 patterns 怎么匹配


func HandleFunc

func HandleFunc(pattern string, handler func(ResponseWriter, *Request))
HandleFunc registers the handler function for the given pattern in the DefaultServeMux. 
The documentation for ServeMux explains how patterns are matched.
HandleFunc 在 DefaultServeMux 里 用给定的 pattern 注册handler 函数
ServeMux的文档解释了 patterns 怎么匹配



func ListenAndServe

func ListenAndServe(addr string, handler Handler) error
ListenAndServe listens on the TCP network address addr and then calls Serve with handler to handle requests on incoming connections. 
Handler is typically nil, in which case the DefaultServeMux is used.
ListenAndServe 监听TCP 网络地址addr  然后 调用Serve 处理程序来处理对传入的连接请求

A trivial example server is:
一个简单的服务器例子:
```golang
package main

import (
	"io"
	"net/http"
	"log"
)

// hello world, the web server
func HelloServer(w http.ResponseWriter, req *http.Request) {
	io.WriteString(w, "hello, world!\n")
}

func main() {
	http.HandleFunc("/hello", HelloServer)
	err := http.ListenAndServe(":12345", nil)
	if err != nil {
		log.Fatal("ListenAndServe: ", err)
	}
}
```


func ListenAndServeTLS

func ListenAndServeTLS(addr string, certFile string, keyFile string, handler Handler) error
ListenAndServeTLS acts identically to ListenAndServe, except that it expects HTTPS connections. 
Additionally, files containing a certificate and matching private key for the server must be provided. 
If the certificate is signed by a certificate authority, the certFile should be the concatenation of the server's certificate followed by the CA's certificate.
ListenAndServeTLS 和ListenAndServe 作用相同, 不同之处在于，它预计HTTPS连接。
另外,文件包含一个 证书 和匹配的 私钥 ,服务器必须提供.
如果证书由证书颁发机构签署，certFile 必须 是服务器证书的连接，其后为CA的证书.

A trivial example server is:
一个简单的服务器例子:
```golang
import (
	"log"
	"net/http"
)

func handler(w http.ResponseWriter, req *http.Request) {
	w.Header().Set("Content-Type", "text/plain")
	w.Write([]byte("This is an example server.\n"))
}

func main() {
	http.HandleFunc("/", handler)
	log.Printf("About to listen on 10443. Go to https://127.0.0.1:10443/")
	err := http.ListenAndServeTLS(":10443", "cert.pem", "key.pem", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```
One can use generate_cert.go in crypto/tls to generate cert.pem and key.pem.
可以使用 crypto/tls 里的 generate_cert.go 来生成cert.pem 和key.pem.


func MaxBytesReader
```golang
func MaxBytesReader(w ResponseWriter, r io.ReadCloser, n int64) io.ReadCloser
```
MaxBytesReader is similar to io.LimitReader but is intended for limiting the size of incoming request bodies. 
In contrast to io.LimitReader, MaxBytesReader's result is a ReadCloser, returns a non-EOF error for a Read beyond the limit, and Closes the underlying reader when its Close method is called.
MaxBytesReader prevents clients from accidentally or maliciously sending a large request and wasting server resources.
MaxBytesReader  类似 io.LimitReader  但是用于限制 请求体的大小.
对比 io.LimitReader,MaxBytesReader的结果是一个ReadCloser, 对于一个Read  达到限制返回 一个非EOF错误,并且当调用Close 方法的时候关闭底层的 reader
MaxBytesReader 防止客户端 意外或恶意 的发送一个巨大的请求 来浪费服务器资源。


func NotFound
```golang
func NotFound(w ResponseWriter, r *Request)
```
NotFound replies to the request with an HTTP 404 not found error.
NotFound 响应  请求 HTTP 404  没有发现的错误 


func ParseHTTPVersion
```golang
func ParseHTTPVersion(vers string) (major, minor int, ok bool)
```
ParseHTTPVersion parses a HTTP version string. "HTTP/1.0" returns (1, 0, true).
ParseHTTPVersion解析一个HTTP 版本字符串. "HTTP/1.0" 返回(1, 0, true).


func ParseTime
```golang
func ParseTime(text string) (t time.Time, err error)
```
ParseTime parses a time header (such as the Date: header), trying each of the three formats allowed by HTTP/1.1: TimeFormat, time.RFC850, and time.ANSIC.
ParseTime解析一个时间头(例如  Date: header),试图通过每一个HTTP/1.1允许的三种格式:TimeFormat, time.RFC850, and time.ANSIC.


func ProxyFromEnvironment
```golang
func ProxyFromEnvironment(req *Request) (*url.URL, error)
```
ProxyFromEnvironment returns the URL of the proxy to use for a given request, as indicated by the environment variables $HTTP_PROXY and $NO_PROXY (or $http_proxy and $no_proxy). 
An error is returned if the proxy environment is invalid. 
A nil URL and nil error are returned if no proxy is defined in the environment, or a proxy should not be used for the given request.
As a special case, if req.URL.Host is "localhost" (with or without a port number), then a nil URL and nil error will be returned.
ProxyFromEnvironment 使用给定请求代理的URL，由环境变量所指示 $HTTP_PROXY 和 $NO_PROXY (或 $http_proxy 和 $no_proxy).
如果代理环境是无效的 返回一个错误.
如果环境里没有定义代理或 代理没有被用到 给定的 请求  返回一个nil URL 和 nil错误
特例,如果  req.URL.Host 是  "localhost" (包含或不包含一个端口号),然后会返回 nil URL 和 nil error



func ProxyURL
```golang
func ProxyURL(fixedURL *url.URL) func(*Request) (*url.URL, error)
```
ProxyURL returns a proxy function (for use in a Transport) that always returns the same URL.
ProxyURL 返回一个代理 函数(用于传输) 经常返回相同的 URL.
 


func Redirect
```golang
func Redirect(w ResponseWriter, r *Request, urlStr string, code int)
```
Redirect replies to the request with a redirect to url, which may be a path relative to the request path.
Redirect 响应 重定向url 请求, 可能是请求路径的相对路径.


func Serve
```golang
func Serve(l net.Listener, handler Handler) error
```
Serve accepts incoming HTTP connections on the listener l, creating a new service goroutine for each. 
The service goroutines read requests and then call handler to reply to them. Handler is typically nil, in which case the DefaultServeMux is used.
Serve 接口l的HTTP连接,对每个创建一个新的服务goroutine


func ServeContent
```golang
func ServeContent(w ResponseWriter, req *Request, name string, modtime time.Time, content io.ReadSeeker)
```
ServeContent replies to the request using the content in the provided ReadSeeker. 
The main benefit of ServeContent over io.Copy is that it handles Range requests properly, sets the MIME type, and handles If-Modified-Since requests.

If the response's Content-Type header is not set, ServeContent first tries to deduce the type from name's file extension and, if that fails, falls back to reading the first block of the content and passing it to DetectContentType. 
The name is otherwise unused; in particular it can be empty and is never sent in the response.

If modtime is not the zero time, ServeContent includes it in a Last-Modified header in the response. 
If the request includes an If-Modified-Since header, ServeContent uses modtime to decide whether the content needs to be sent at all.

The content's Seek method must work: ServeContent uses a seek to the end of the content to determine its size.

If the caller has set w's ETag header, ServeContent uses it to handle requests using If-Range and If-None-Match.

Note that *os.File implements the io.ReadSeeker interface.
ServeContent 使用 提供的ReadSeeker 的content 响应请求
ServeContent 比io.Copy 最主要的好处是  它正确处理Range请求,设置MIME类型,和处理If-Modified-Since 请求.
如果响应的Content-Type头 没有设置,ServeContent 首先尝试 推断出 文件扩展名的类型, 如果失败的话, 返回读取的第一块的content 并传给DetectContentType.
这个名字是其他未使用; 特别是在响应里它可以是空的 并且 没有 发送过的

如果modtime 不是 零时间,ServeContent 响应时 在 Last-Modified头包含它.
如果请求包含一个  If-Modified-Since  头,ServeContent 使用 modtime 来推断 content是否需要全部发送.

内部的 Seek 方法 必须:ServeContent 使用一个使用搜索到的content的最后来确定它的大小.

如果调用者设置 w的 ETag 头,ServeContent 用  If-Range 和 If-None-Match 来处理请求.



func ServeFile
```golang
func ServeFile(w ResponseWriter, r *Request, name string)
```
ServeFile replies to the request with the contents of the named file or directory.
ServeFile 用 命名文件 或目录来响应请求


func SetCookie
```golang
func SetCookie(w ResponseWriter, cookie *Cookie)
```
SetCookie adds a Set-Cookie header to the provided ResponseWriter's headers.
SetCookie 添加一个 Set-Cookie头来 提供 ResponseWriter的头.


func StatusText

func StatusText(code int) string
StatusText returns a text for the HTTP status code. It returns the empty string if the code is unknown.
StatusText 对 HTTP 状态码 返回一个文本. 如果code 是未知的 它 返回 空字符串



type Client
```golang
type Client struct {
        // Transport specifies the mechanism by which individual
        // HTTP requests are made.
        // If nil, DefaultTransport is used.
        //Transport 指定 个别HTTP请求的机制
           //如果nil, 使用DefaultTransport
        Transport RoundTripper



        // CheckRedirect specifies the policy for handling redirects.
        // If CheckRedirect is not nil, the client calls it before
        // following an HTTP redirect. The arguments req and via are
        // the upcoming request and the requests made already, oldest
        // first. If CheckRedirect returns an error, the Client's Get
        // method returns both the previous Response and
        // CheckRedirect's error (wrapped in a url.Error) instead of
        // issuing the Request req.
        //
        // If CheckRedirect is nil, the Client uses its default policy,
        // which is to stop after 10 consecutive requests.
        //CheckRedirect指定用于处理重定向的方式
        //如果 CheckRedirect 不是 nil,客户端在 HTTP 重定向之前调用它.
        //参数 req和 via 是即将到来的请求，已作出的请求,大的先.
        //如果CheckRedirect 返回一个错误,Client的Get 方法 返回之前的Response 和CheckRedirect的错误来代替 发出的Request req.
        
        //如果CheckRedirect是nil,Client 使用它 默认的政策, 停止到之后的10个连续的请求之后.
        
        CheckRedirect func(req *Request, via []*Request) error



        // Jar specifies the cookie jar.
        // If Jar is nil, cookies are not sent in requests and ignored
        // in responses.
        //Jar 指定 cookie jar
           //如果Jar 是nil,cookie 在请求的时候不会发送 并且 在响应的时候会忽略
        
        Jar CookieJar



        // Timeout specifies a time limit for requests made by this
        // Client. The timeout includes connection time, any
        // redirects, and reading the response body. The timer remains
        // running after Get, Head, Post, or Do return and will
        // interrupt reading of the Response.Body.
        //
        // A Timeout of zero means no timeout.
        //
        // The Client's Transport must support the CancelRequest
        // method or Client will return errors when attempting to make
        // a request with Get, Head, Post, or Do. Client's default
        // Transport (DefaultTransport) supports CancelRequest.
        //Timeout指定 这个Client 的 请求的时间限制.
        //timeout 包含 链接时间, 任何重定向 和读取 响应体.
           // 计时器在Get, Head, Post, 或 Do 返回 和 中断读取Response.Body之后 运行.
        //Timeout的零值 表示 不会超时.
        //Client的Transport 必须支持 CancelRequest 方法 或 当 尝试用  Get, Head, Post, 或 Do创建一个请求的时候 Client会返回一个错误
        //Client的默认Transport(DefaultTransport) 支持CancelRequest.
        
        Timeout time.Duration
}
```
A Client is an HTTP client. Its zero value (DefaultClient) is a usable client that uses DefaultTransport.

The Client's Transport typically has internal state (cached TCP connections), so Clients should be reused instead of created as needed. 
Clients are safe for concurrent use by multiple goroutines.

A Client is higher-level than a RoundTripper (such as Transport) and additionally handles HTTP details such as cookies and redirects.
一个Client是一个HTTP 客户端.它的零值(DefaultClient) 是使用DefaultTransport 的一个可用的客户端.

Client的Transport 通常有内部状态(缓存TCP链接), 所以Clients 需要的时候应该用重复使用 来代替创建.
Clients多个goroutines 并行 是安全的.

一个Client 比RoundTripper更高级别(例如Transport) , 另外详细处理HTTP  例如 cookie 和重定向.



func (*Client) Do

func (c *Client) Do(req *Request) (resp *Response, err error)
Do sends an HTTP request and returns an HTTP response, following policy (e.g. redirects, cookies, auth) as configured on the client.

An error is returned if caused by client policy (such as CheckRedirect), or if there was an HTTP protocol error. A non-2xx response doesn't cause an error.

When err is nil, resp always contains a non-nil resp.Body.

Callers should close resp.Body when done reading from it. 
If resp.Body is not closed, the Client's underlying RoundTripper (typically Transport) may not be able to re-use a persistent TCP connection to the server for a subsequent "keep-alive" request.

The request Body, if non-nil, will be closed by the underlying Transport, even on errors.

Generally Get, Post, or PostForm will be used instead of Do.

Do 根据客户端配置里的策略(例如重定向,cookie,验证),发送一个 HTTP 请求 并 返回 HTTP 响应.

如果通过客户端策略 返回一个错误(例如CheckRedirect),或如果有 HTTP 协议错误. 非 2xx 响应不会引起错误.

当 err是nil,resp通常包含一个非nil resp.Body.

调用者应该在从它读取的时候 关闭 resp.Body.
如果 resp.Body 没有关闭,Client的底层RoundTripper(通常是Transport) 可能 不会 重复使用一个固定的TCP链接 来 保持子请求"keep-alive".

请求Body, 如果非nil , 将会关闭底层的Transport, 即使有错误.

通常Get, Post, 或 PostForm 将会代替 Do.



func (*Client) Get
```golang
func (c *Client) Get(url string) (resp *Response, err error)
```
Get issues a GET to the specified URL. If the response is one of the following redirect codes, Get follows the redirect after calling the Client's CheckRedirect function.
Get 给指定的URL 发 一个GET. 如果响应是 重定向码中的一个,Get 在重定向后调用Client的CheckRedirect 函数.


```golang
301 (Moved Permanently)
302 (Found)
303 (See Other)
307 (Temporary Redirect)
```
An error is returned if the Client's CheckRedirect function fails or if there was an HTTP protocol error. A non-2xx response doesn't cause an error.

When err is nil, resp always contains a non-nil resp.Body. Caller should close resp.Body when done reading from it.

如果Client的CheckRedirect 函数 失败或 如果有一个HTTP协议错误  返回一个错误.  一个 非 2xx响应不会引起错误.
当 err是nil,resp 通常包含一个非 nil resp.Body.  当读取它的时候 调用者应该关闭 resp.Body



func (*Client) Head

func (c *Client) Head(url string) (resp *Response, err error)
Head issues a HEAD to the specified URL. If the response is one of the following redirect codes, Head follows the redirect after calling the Client's CheckRedirect function.
Head 给指定的URL 发一个HEAD. 如果响应是 重定向码中的一个,Head 在重定向后调用Client的CheckRedirect 函数.
``golang
301 (Moved Permanently)
302 (Found)
303 (See Other)
307 (Temporary Redirect)
```


func (*Client) Post
```golang
func (c *Client) Post(url string, bodyType string, body io.Reader) (resp *Response, err error)
```
Post issues a POST to the specified URL.

Caller should close resp.Body when done reading from it.

If the provided body is also an io.Closer, it is closed after the request.
Post 给指定的URL 发一个POST
当读取它的时候调用者应该关闭resp.Body
如果提供的body 也是一个 io.Closer, 在请求之后关闭.



func (*Client) PostForm
```golang
func (c *Client) PostForm(url string, data url.Values) (resp *Response, err error)
```
PostForm issues a POST to the specified URL, with data's keys and values urlencoded as the request body.

When err is nil, resp always contains a non-nil resp.Body. Caller should close resp.Body when done reading from it.

PostForm 向指定的URL 发一个POST,包含 data的keys 和keys的URL编码的 请求体.

当err是nil, resp通常包含一个非 nil resp.Body. 当读取它的时候调用者应该关闭resp.Body.


type CloseNotifier
```golang
type CloseNotifier interface {
        // CloseNotify returns a channel that receives a single value
        // when the client connection has gone away.
        CloseNotify() <-chan bool
}
```
The CloseNotifier interface is implemented by ResponseWriters which allow detecting when the underlying connection has gone away.

This mechanism can be used to cancel long operations on the server if the client has disconnected before the response is ready.

CloseNotifier接口 根据ResponseWriters 实现, 当底层的连接消失时 允许检测.

如果客户端在响应准备好之前断开,这个机制可以用来放弃服务器的长操作,


type ConnState
```golang
type ConnState int
```
A ConnState represents the state of a client connection to a server. It's used by the optional Server.ConnState hook.
ConnState 表示客户端连接到服务端的状态. 它使用Server.ConnState  钩子选项.

const (
        // StateNew represents a new connection that is expected to
        // send a request immediately. Connections begin at this
        // state and then transition to either StateActive or
        // StateClosed.
        //StateNew 表示一个新的连接,预计用来立即发送一个请求.
           //连接以这样的状态开始,然后 过渡到StateActive 或StateClosed.
        StateNew ConnState = iota


        // StateActive represents a connection that has read 1 or more
        // bytes of a request. The Server.ConnState hook for
        // StateActive fires before the request has entered a handler
        // and doesn't fire again until the request has been
        // handled. After the request is handled, the state
        // transitions to StateClosed, StateHijacked, or StateIdle.
        //StateActive表示一个读取1 或更多字节请求的连接.
        //StateActive 在请求进入处理程序之前激活 并且不会再次激活 直到 请求被处理了.
           //请求被处理之后,过渡状态成StateClosed, StateHijacked, or StateIdle.
        StateActive
        

        // StateIdle represents a connection that has finished
        // handling a request and is in the keep-alive state, waiting
        // for a new request. Connections transition from StateIdle
        // to either StateActive or StateClosed.
        //StateIdle 表示一个结束处理请求 并且保持激活状态的连接 ,等待一个新的请求.
           //连接从StateIdle 过渡到StateActive 或 StateClosed.
        StateIdle


        // StateHijacked represents a hijacked connection.
        // This is a terminal state. It does not transition to StateClosed.
        //StateHijacked 表示一个被劫持的连接。
           //它是一个终端状态.它不会过渡到StateClosed.
        StateHijacked


        // StateClosed represents a closed connection.
        // This is a terminal state. Hijacked connections do not
        // transition to StateClosed.
        //StateClosed表示一个关闭的连接.
           //它是一个终端状态.劫持连接不会过渡到StateClosed
        StateClosed
)


func (ConnState) String
```golang
func (c ConnState) String() string
```


type Cookie
```golang
type Cookie struct {
        Name       string
        Value      string
        Path       string
        Domain     string
        Expires    time.Time
        RawExpires string

        // MaxAge=0 means no 'Max-Age' attribute specified.
        // MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
        // MaxAge>0 means Max-Age attribute present and given in seconds
        MaxAge   int
        Secure   bool
        HttpOnly bool
        Raw      string
        Unparsed []string // Raw text of unparsed attribute-value pairs
}
```
A Cookie represents an HTTP cookie as sent in the Set-Cookie header of an HTTP response or the Cookie header of an HTTP request.
Cookie 表示一个HTTP cookie  在HTTP 响应的 Set-Cookie里 发送,或者 在HTTP请求头里的 Cookie 


func (*Cookie) String
```golang
func (c *Cookie) String() string
```
String returns the serialization of the cookie for use in a Cookie header (if only Name and Value are set) or a Set-Cookie response header (if other fields are set).
String 返回序列化后的cookie 在Cookie头里使用(如果只有设置Name 和 Value) 或  一个 Set-Cookie 响应头(如果其他字段设置了).



type CookieJar
```golang
type CookieJar interface {
        // SetCookies handles the receipt of the cookies in a reply for the
        // given URL.  It may or may not choose to save the cookies, depending
        // on the jar's policy and implementation.
        //SetCookies 处理 给定URL的 在响应收到的cookies.
           //它可能会或可能不会选择保存 cookie,依赖jar的策略和实现.
        
        SetCookies(u *url.URL, cookies []*Cookie)



        // Cookies returns the cookies to send in a request for the given URL.
        // It is up to the implementation to honor the standard cookie use
        // restrictions such as in RFC 6265.
        //Cookies 为给定的URL 返回 发送一个请求的cookie.
        
        Cookies(u *url.URL) []*Cookie
}
```
A CookieJar manages storage and use of cookies in HTTP requests.

Implementations of CookieJar must be safe for concurrent use by multiple goroutines.

The net/http/cookiejar package provides a CookieJar implementation.

CookieJar管理 存储 和使用在HTTP请求里的cookie.
多个goroutines 并发实现的 CookieJar 必须是安全的.
net/http/cookiejar包 提供一个CookieJar实现.


type Dir
```golang
type Dir string
```
A Dir implements http.FileSystem using the native file system restricted to a specific directory tree.

An empty Dir is treated as ".".

Dir 用  本地文件系统 限制在一个指定的目录树上实现http.FileSystem.
一个空的Dir 是  ".".


func (Dir) Open
```golang
func (d Dir) Open(name string) (File, error)
```


type File
```golang
type File interface {
        io.Closer
        io.Reader
        Readdir(count int) ([]os.FileInfo, error)
        Seek(offset int64, whence int) (int64, error)
        Stat() (os.FileInfo, error)
}
```
A File is returned by a FileSystem's Open method and can be served by the FileServer implementation.

The methods should behave the same as those on an *os.File.

File  通过 FileSystem的Open 方法 和   可以 通过FileServer实现服务 返回.
该方法 表示应该和其他 的*os.File 一样.



type FileSystem
```golang
type FileSystem interface {
        Open(name string) (File, error)
}
```
A FileSystem implements access to a collection of named files. 
The elements in a file path are separated by slash ('/', U+002F) characters, regardless of host operating system convention.
FileSystem 实现访问命名文件集.
文件路径的元素  用斜线字符 分隔,无论主机操作系统公约.


type Flusher
```golang
type Flusher interface {
        // Flush sends any buffered data to the client.
        Flush()
}
```
The Flusher interface is implemented by ResponseWriters that allow an HTTP handler to flush buffered data to the client.

Note that even for ResponseWriters that support Flush, if the client is connected through an HTTP proxy, the buffered data may not reach the client until the response completes.
Flusher接口 通过 ResponseWriters实现, 允许 客户端HTTP 处理  刷新 缓冲区 数据.
备注 ResponseWriters也支持Flush, 如果客户端通过HTTP代理连接, 缓冲区数据可能不会接收到客户端 直到 响应完成.



type Handler
```golang
type Handler interface {
        ServeHTTP(ResponseWriter, *Request)
}
```
Objects implementing the Handler interface can be registered to serve a particular path or subtree in the HTTP server.

ServeHTTP should write reply headers and data to the ResponseWriter and then return. 
Returning signals that the request is finished and that the HTTP server can move on to the next request on the connection.
Objects 实现 Handler接口 可以 在HTTP 服务里  注册 一个具体的路径或子树到服务里.
ServeHTTP应该写 回复头 和数据 到ResponseWriter 然后返回.
返回请求结束信号 , 在连接里 HTTP服务 可以移到 下次请求.



func FileServer
```golang
func FileServer(root FileSystem) Handler
```
FileServer returns a handler that serves HTTP requests with the contents of the file system rooted at root.

To use the operating system's file system implementation, use http.Dir:

http.Handle("/", http.FileServer(http.Dir("/tmp")))
FileServer 返回一个处理 HTTP 服务 请求和文件root里的 文件系统 的内容  的程序 .
使用操作系统 的文件系统实现,使用 http.Dir:

▾ Example:
```golang
package main

import (
	"log"
	"net/http"
)

func main() {
	// Simple static webserver:
	log.Fatal(http.ListenAndServe(":8080", http.FileServer(http.Dir("/usr/share/doc"))))
}
```


Example (StripPrefix):
```golang
package main

import (
	"net/http"
)

func main() {
	// To serve a directory on disk (/tmp) under an alternate URL
	// path (/tmpfiles/), use StripPrefix to modify the request
	// URL's path before the FileServer sees it:
	http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))
}
```


func NotFoundHandler
```golang
func NotFoundHandler() Handler
```
NotFoundHandler returns a simple request handler that replies to each request with a “404 page not found” reply.
NotFoundHandler 返回一个简单的请求处理器, 以“404 page not found” 回复 每个请求.


func RedirectHandler
```golang
func RedirectHandler(url string, code int) Handler
```
RedirectHandler returns a request handler that redirects each request it receives to the given url using the given status code.
RedirectHandler返回一个请求处理器,  使用给定的状态码 重定向每个 接收到给定url的请求.


func StripPrefix

func StripPrefix(prefix string, h Handler) Handler
StripPrefix returns a handler that serves HTTP requests by removing the given prefix from the request URL's Path and invoking the handler h. 
StripPrefix handles a request for a path that doesn't begin with prefix by replying with an HTTP 404 not found error.
StripPrefix 返回一个处理器 服务HTTP请求 ,从请求URL的 Path 移除给定的 prefix 和调用 处理器h
StripPrefix  处理一个路径的请求,没有以prefix 开始的 用 HTTP 404 未发现 错误 回复.


▾ Example
```golang
package main

import (
	"net/http"
)

func main() {
	// To serve a directory on disk (/tmp) under an alternate URL
	// path (/tmpfiles/), use StripPrefix to modify the request
	// URL's path before the FileServer sees it:
	http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))
}
```



func TimeoutHandler
```golang
func TimeoutHandler(h Handler, dt time.Duration, msg string) Handler
```
TimeoutHandler returns a Handler that runs h with the given time limit.

The new Handler calls h.ServeHTTP to handle each request, but if a call runs for longer than its time limit, the handler responds with a 503 Service Unavailable error and the given message in its body. 
(If msg is empty, a suitable default message will be sent.) After such a timeout, writes by h to its ResponseWriter will return ErrHandlerTimeout.

TimeoutHandler 返回一个 用给定的时间限制执行h  Handler .

新的Handler 调用h.ServeHTTP 来处理 每个请求, 但是如果一个执行 长于它的时间限制,处理器 响应 503 Service Unavailable 错误并且 在它的 body里给定信息.
(如果msg是空的,会发送一个 合适的 默认信息)在一个超时后, 通过h 写入它的ResponseWriter 将会返回ResponseWriter


type HandlerFunc
```golang
type HandlerFunc func(ResponseWriter, *Request)
```
The HandlerFunc type is an adapter to allow the use of ordinary functions as HTTP handlers. 
If f is a function with the appropriate signature, HandlerFunc(f) is a Handler object that calls f.
HandlerFunc类型是一个适配器 来允许使用普通函数来当成HTTP 处理器.
如果f是一个适当的签名函数,HandlerFunc(f) 是一个 调用f 的Handler 对象.



func (HandlerFunc) ServeHTTP
```golang
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request)
```
ServeHTTP calls f(w, r).
ServeHTTP 调用 f(w, r).


type Header
```golang
type Header map[string][]string
```
A Header represents the key-value pairs in an HTTP header.
Header表示在HTTP 头里的 键-值 对


func (Header) Add
```golang
func (h Header) Add(key, value string)
```
Add adds the key, value pair to the header. It appends to any existing values associated with key.
Add 添加键,值对 到头. 它追加任何和 存在键对应的 值


func (Header) Del
```golang
func (h Header) Del(key string)
```
Del deletes the values associated with key.
Del 删除和 键对应的值


func (Header) Get
```golang
func (h Header) Get(key string) string
```
Get gets the first value associated with the given key. 
If there are no values associated with the key, Get returns "". 
To access multiple values of a key, access the map directly with CanonicalHeaderKey.
Get 获取 和给定键对应的第一个值.
如果键没有对应的值, Get 返回" ".
要访问一个键的多个值,访问 map 直接 用 CanonicalHeaderKey



func (Header) Set
```golang
func (h Header) Set(key, value string)
```
Set sets the header entries associated with key to the single element value. It replaces any existing values associated with key.
Set 设置 和键有关的单个元素值的项  头. 它 根据key替换任何存在的值.



func (Header) Write
```golang
func (h Header) Write(w io.Writer) error
```
Write writes a header in wire format.
Write 以传输格式写一个header



func (Header) WriteSubset
```golang
func (h Header) WriteSubset(w io.Writer, exclude map[string]bool) error
```
WriteSubset writes a header in wire format. If exclude is not nil, keys where exclude[key] == true are not written.
WriteSubset 以传输格式 写一个头. 如果exclude 是 非nil,键  在exclude[key] == true   不会写入.



type Hijacker

type Hijacker interface {
        // Hijack lets the caller take over the connection.
        // After a call to Hijack(), the HTTP server library
        // will not do anything else with the connection.
        // It becomes the caller's responsibility to manage
        // and close the connection.
        //Hijack 让调用者 接管 连接.
           //调用Hijack()后,HTTP 服务 类 将不会用该连接做任何其他的事.
           // 变成 调用者要负责管理和关闭该连接
        Hijack() (net.Conn, *bufio.ReadWriter, error)
}
The Hijacker interface is implemented by ResponseWriters that allow an HTTP handler to take over the connection.
Hijacker接口通过ResponseWriters 实现, 允许一个HTTP处理器 接管 连接.

Example:
```golang
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/hijack", func(w http.ResponseWriter, r *http.Request) {
		hj, ok := w.(http.Hijacker)
		if !ok {
			http.Error(w, "webserver doesn't support hijacking", http.StatusInternalServerError)
			return
		}
		conn, bufrw, err := hj.Hijack()
		if err != nil {
			http.Error(w, err.Error(), http.StatusInternalServerError)
			return
		}
		// Don't forget to close the connection:
		defer conn.Close()
		bufrw.WriteString("Now we're speaking raw TCP. Say hi: ")
		bufrw.Flush()
		s, err := bufrw.ReadString('\n')
		if err != nil {
			log.Printf("error reading string: %v", err)
			return
		}
		fmt.Fprintf(bufrw, "You said: %q\nBye.\n", s)
		bufrw.Flush()
	})
}
```


type ProtocolError
```golang
type ProtocolError struct {
        ErrorString string
}
```
HTTP request parsing errors.
HTTP请求解析错误.



func (*ProtocolError) Error
```golang
func (err *ProtocolError) Error() string
```



type Request

type Request struct {
        // Method specifies the HTTP method (GET, POST, PUT, etc.).
        // For client requests an empty string means GET.
        //Method 指定HTTP方法(GET, POST, PUT, 等.).
          //对于 客户端 请求一个空的字符串 意味着 GET.
        Method string


        // URL specifies either the URI being requested (for server
        // requests) or the URL to access (for client requests).
        //
        // For server requests the URL is parsed from the URI
        // supplied on the Request-Line as stored in RequestURI.  For
        // most requests, fields other than Path and RawQuery will be
        // empty. (See RFC 2616, Section 5.1.2)
        //
        // For client requests, the URL's Host specifies the server to
        // connect to, while the Request's Host field optionally
        // specifies the Host header value to send in the HTTP
        // request.
        //URL 指定 URI 是否被请求(服务端请求)或URL访问(客户端请求).
           // 对于服务端 请求 URL 是 从  提供 存储在RequestURI里的Request-Line 的URI中 解析的 .
           //对于大多数请求, 字段以外的 Path 和 RawQuery 将是空的.
        
           //对于客户端请求,URL的Host指定服务端连接,Request的 Host字段选项指定在HTTP 请求中的Host头 值 发送.
        URL *url.URL


        // The protocol version for incoming requests.
        // Client requests always use HTTP/1.1.
		   //协议版本为传入的请求。
		   //客户端请求通常使用 HTTP/1.1.
        Proto      string // "HTTP/1.0"
        ProtoMajor int    // 1
        ProtoMinor int    // 0



        // A header maps request lines to their values.
        // If the header says
        //
        //	accept-encoding: gzip, deflate
        //	Accept-Language: en-us
        //	Connection: keep-alive
        //
        // then
        //
        //	Header = map[string][]string{
        //		"Accept-Encoding": {"gzip, deflate"},
        //		"Accept-Language": {"en-us"},
        //		"Connection": {"keep-alive"},
        //	}
        //
        // HTTP defines that header names are case-insensitive.
        // The request parser implements this by canonicalizing the
        // name, making the first character and any characters
        // following a hyphen uppercase and the rest lowercase.
        //
        // For client requests certain headers are automatically
        // added and may override values in Header.
        //
        // See the documentation for the Request.Write method.
        
        Header Header

        // Body is the request's body.
        //
        // For client requests a nil body means the request has no
        // body, such as a GET request. The HTTP Client's Transport
        // is responsible for calling the Close method.
        //
        // For server requests the Request Body is always non-nil
        // but will return EOF immediately when no body is present.
        // The Server will close the request body. The ServeHTTP
        // Handler does not need to.
        Body io.ReadCloser

        // ContentLength records the length of the associated content.
        // The value -1 indicates that the length is unknown.
        // Values >= 0 indicate that the given number of bytes may
        // be read from Body.
        // For client requests, a value of 0 means unknown if Body is not nil.
        ContentLength int64

        // TransferEncoding lists the transfer encodings from outermost to
        // innermost. An empty list denotes the "identity" encoding.
        // TransferEncoding can usually be ignored; chunked encoding is
        // automatically added and removed as necessary when sending and
        // receiving requests.
        TransferEncoding []string

        // Close indicates whether to close the connection after
        // replying to this request (for servers) or after sending
        // the request (for clients).
        Close bool

        // For server requests Host specifies the host on which the
        // URL is sought. Per RFC 2616, this is either the value of
        // the "Host" header or the host name given in the URL itself.
        // It may be of the form "host:port".
        //
        // For client requests Host optionally overrides the Host
        // header to send. If empty, the Request.Write method uses
        // the value of URL.Host.
        Host string

        // Form contains the parsed form data, including both the URL
        // field's query parameters and the POST or PUT form data.
        // This field is only available after ParseForm is called.
        // The HTTP client ignores Form and uses Body instead.
        Form url.Values

        // PostForm contains the parsed form data from POST or PUT
        // body parameters.
        // This field is only available after ParseForm is called.
        // The HTTP client ignores PostForm and uses Body instead.
        PostForm url.Values

        // MultipartForm is the parsed multipart form, including file uploads.
        // This field is only available after ParseMultipartForm is called.
        // The HTTP client ignores MultipartForm and uses Body instead.
        MultipartForm *multipart.Form

        // Trailer specifies additional headers that are sent after the request
        // body.
        //
        // For server requests the Trailer map initially contains only the
        // trailer keys, with nil values. (The client declares which trailers it
        // will later send.)  While the handler is reading from Body, it must
        // not reference Trailer. After reading from Body returns EOF, Trailer
        // can be read again and will contain non-nil values, if they were sent
        // by the client.
        //
        // For client requests Trailer must be initialized to a map containing
        // the trailer keys to later send. The values may be nil or their final
        // values. The ContentLength must be 0 or -1, to send a chunked request.
        // After the HTTP request is sent the map values can be updated while
        // the request body is read. Once the body returns EOF, the caller must
        // not mutate Trailer.
        //
        // Few HTTP clients, servers, or proxies support HTTP trailers.
        Trailer Header

        // RemoteAddr allows HTTP servers and other software to record
        // the network address that sent the request, usually for
        // logging. This field is not filled in by ReadRequest and
        // has no defined format. The HTTP server in this package
        // sets RemoteAddr to an "IP:port" address before invoking a
        // handler.
        // This field is ignored by the HTTP client.
        RemoteAddr string

        // RequestURI is the unmodified Request-URI of the
        // Request-Line (RFC 2616, Section 5.1) as sent by the client
        // to a server. Usually the URL field should be used instead.
        // It is an error to set this field in an HTTP client request.
        RequestURI string

        // TLS allows HTTP servers and other software to record
        // information about the TLS connection on which the request
        // was received. This field is not filled in by ReadRequest.
        // The HTTP server in this package sets the field for
        // TLS-enabled connections before invoking a handler;
        // otherwise it leaves the field nil.
        // This field is ignored by the HTTP client.
        TLS *tls.ConnectionState
}
A Request represents an HTTP request received by a server or to be sent by a client.

The field semantics differ slightly between client and server usage. 
In addition to the notes on the fields below, see the documentation for Request.Write and RoundTripper.
Request 表示一个  接收服务端 或 发送 到客户端的HTTP 请求.

字段语义 在服务端和客户端的使用 略有不同.
除了下面的字段中的注意事项, 查看Request.Write 和 RoundTripper 文档.



func NewRequest
```golang
func NewRequest(method, urlStr string, body io.Reader) (*Request, error)
```
NewRequest returns a new Request given a method, URL, and optional body.

If the provided body is also an io.Closer, the returned Request.Body is set to body and will be closed by the Client methods Do, Post, and PostForm, and Transport.RoundTrip.
NewRequest 用给定的method, URL, 和  body 选项 返回一个新的 Request.
如果提供的body也是一个io.Closer, 返回的 Request.Body 设置到body 并且 将会用Client 的Do, Post, 和 PostForm, 和 Transport.RoundTrip. 方法关闭


func ReadRequest
```golang
func ReadRequest(b *bufio.Reader) (req *Request, err error)
```
ReadRequest reads and parses a request from b.
ReadRequest 从b 读取和 解析一个 请求


func (*Request) AddCookie
```golang
func (r *Request) AddCookie(c *Cookie)
```
AddCookie adds a cookie to the request. 
Per RFC 6265 section 5.4, AddCookie does not attach more than one Cookie header field. 
That means all cookies, if any, are written into the same line, separated by semicolon.
AddCookie 添加 cookie 到 请求.
由RFC 6265的 5.4节定义,  AddCookie不会 附加超过一个的Cookie头字段.
那意味着 所有cookie, 如果有的话，被写入同一行,用分号隔开。


func (*Request) Cookie
```golang
func (r *Request) Cookie(name string) (*Cookie, error)
```
Cookie returns the named cookie provided in the request or ErrNoCookie if not found.
Cookie返回 在请求里提供的 命名 cookie 或  没有发现 返回 ErrNoCookie.



func (*Request) Cookies
```golang
func (r *Request) Cookies() []*Cookie
```
Cookies parses and returns the HTTP cookies sent with the request.
Cookies 解析和返回 与请求一起发送的 HTTP cookie 



func (*Request) FormFile
```golang
func (r *Request) FormFile(key string) (multipart.File, *multipart.FileHeader, error)
```
FormFile returns the first file for the provided form key. FormFile calls ParseMultipartForm and ParseForm if necessary.
FormFile 返回 提供的key的 第一个文件.如果需要的话,FormFile 调用 ParseMultipartForm 和 ParseForm.



func (*Request) FormValue
```golang
func (r *Request) FormValue(key string) string
```
FormValue returns the first value for the named component of the query. 
POST and PUT body parameters take precedence over URL query string values. 
FormValue calls ParseMultipartForm and ParseForm if necessary. 
To access multiple values of the same key use ParseForm.
FormValue 返回 查询的 命名组件的第一个值。
POST和 PUT body 参数优先于URL查询字符串值。
如果需要的话 FormValue 调用 ParseMultipartForm 和 ParseForm.
使用ParseForm 来访问相同键的多个值.



func (*Request) MultipartReader
```golang
func (r *Request) MultipartReader() (*multipart.Reader, error)
```
MultipartReader returns a MIME multipart reader if this is a multipart/form-data POST request, else returns nil and an error. 
Use this function instead of ParseMultipartForm to process the request body as a stream.
MultipartReader 如果 是 一个 multipart/form-data  POST 请求, 返回一个 MIME 多重reader  其他的 返回nil和一个错误.


func (*Request) ParseForm
```golang
func (r *Request) ParseForm() error
```
ParseForm parses the raw query from the URL and updates r.Form.

For POST or PUT requests, it also parses the request body as a form and put the results into both r.PostForm and r.Form. 
POST and PUT body parameters take precedence over URL query string values in r.Form.

If the request Body's size has not already been limited by MaxBytesReader, the size is capped at 10MB.

ParseMultipartForm calls ParseForm automatically. It is idempotent.
ParseForm 从URL 和更新r.Form 解析原始查询.
对于 POST 或 PUT 请求, 它也解析请求体 成 表单 并 把结果放入 r.PostForm 和 r.Form.
POST 和 PUT body 参数  优先于URL查询字符串值r.Form。
如果请求的Body大小  没有达到 MaxBytesReader 限制,  大小上限 是10MB.
ParseMultipartForm 自动调用 ParseForm。这是幂等的。



func (*Request) ParseMultipartForm
```golang
func (r *Request) ParseMultipartForm(maxMemory int64) error
```
ParseMultipartForm parses a request body as multipart/form-data. 
The whole request body is parsed and up to a total of maxMemory bytes of its file parts are stored in memory, with the remainder stored on disk in temporary files. 
ParseMultipartForm calls ParseForm if necessary. After one call to ParseMultipartForm, subsequent calls have no effect.
ParseMultipartForm 解析一个请求body为  multipart/form-data. 
对整个请求主体进行分析，并最多合并其文件部分maxMemory字节存储在内存中, 剩余的存储在磁盘 临时文件中.
如果需要的话 ParseMultipartForm 调用 ParseForm. 在调用一次ParseMultipartForm后,子请求调用不会受影响.



func (*Request) PostFormValue
```golang
func (r *Request) PostFormValue(key string) string
```
PostFormValue returns the first value for the named component of the POST or PUT request body. 
URL query parameters are ignored. PostFormValue calls ParseMultipartForm and ParseForm if necessary.
PostFormValue 返回  POST 或 PUT 请求体 的命名组件的第一个值.
URL 查询参数被忽略. 如果需要的话 PostFormValue 调用 ParseMultipartForm 和 ParseForm. 


func (*Request) ProtoAtLeast
```golang
func (r *Request) ProtoAtLeast(major, minor int) bool
```
ProtoAtLeast reports whether the HTTP protocol used in the request is at least major.minor.
ProtoAtLeast 报告 HTTP 协议 使用在请求里 是否至少 major.minor.



func (*Request) Referer
```golang
func (r *Request) Referer() string
```
Referer returns the referring URL, if sent in the request.

Referer is misspelled as in the request itself, a mistake from the earliest days of HTTP. 
This value can also be fetched from the Header map as Header["Referer"]; 
the benefit of making it available as a method is that the compiler can diagnose programs that use the alternate (correct English) spelling req.Referrer() but cannot diagnose programs that use Header["Referrer"].

Referer 返回引用的URL ,如果在请求里发送
Referer 拼写错误 它本身的请求, 是从HTTP的初期一个错误。
这个值也可以从 Header map 中获取, 例如 Header["Referer"]; 
使用它可作为一种方法的好处是，编译器可以诊断使用的备用程序 (正确的英语) 拼写 req.Referrer()  但是 不能诊断 程序 使用 Header["Referrer"].


func (*Request) SetBasicAuth
```golang
func (r *Request) SetBasicAuth(username, password string)
```
SetBasicAuth sets the request's Authorization header to use HTTP Basic Authentication with the provided username and password.

With HTTP Basic Authentication the provided username and password are not encrypted.
SetBasicAuth  使用提供的 username 和 password 设置 请求的验证头来 进行HTTP 基础验证.


func (*Request) UserAgent
```golang
func (r *Request) UserAgent() string
```
UserAgent returns the client's User-Agent, if sent in the request.
UserAgent 返回 客户端的 User-Agent, 如果在请求里一起发送的话.


func (*Request) Write
```golang
func (r *Request) Write(w io.Writer) error
```
Write writes an HTTP/1.1 request -- header and body -- in wire format. This method consults the following fields of the request:
Write 写一个 HTTP/1.1 请求 -- header and body --  成传输的格式.这种方法参照请求的以下字段：
```golang
Host
URL
Method (defaults to "GET")
Header
ContentLength
TransferEncoding
Body
```
If Body is present, Content-Length is <= 0 and TransferEncoding hasn't been set to "identity", Write adds "Transfer-Encoding: chunked" to the header. 
Body is closed after it is sent.
如果 Body 存在,Content-Length<=0 并且 TransferEncoding 没有设置"identity", 写添加 "Transfer-Encoding: chunked" 到头. 发送后 关闭body.


func (*Request) WriteProxy
```golang
func (r *Request) WriteProxy(w io.Writer) error
```
WriteProxy is like Write but writes the request in the form expected by an HTTP proxy. 
In particular, WriteProxy writes the initial Request-URI line of the request with an absolute URI, per section 5.1.2 of RFC 2616, including the scheme and host. 
In either case, WriteProxy also writes a Host header, using either r.Host or r.URL.Host.
WriteProxy 类似 Write 但是  用HTTP 代理写 请求成  预期的形式.
特别是,WriteProxy 以绝对URI 写入 初始化  Request-URI 行请求, RFC 2616 中的 5.1.2  节 , 包含 语义和主机.
在任一情况下,WriteProxy 也  使用 r.Host 或 r.URL.Host 写 一个 Host 头,



type Response
```golang
type Response struct {
        Status     string // e.g. "200 OK"
        StatusCode int    // e.g. 200
        Proto      string // e.g. "HTTP/1.0"
        ProtoMajor int    // e.g. 1
        ProtoMinor int    // e.g. 0

        // Header maps header keys to values.  If the response had multiple
        // headers with the same key, they may be concatenated, with comma
        // delimiters.  (Section 4.2 of RFC 2616 requires that multiple headers
        // be semantically equivalent to a comma-delimited sequence.) Values
        // duplicated by other fields in this struct (e.g., ContentLength) are
        // omitted from Header.
        //
        // Keys in the map are canonicalized (see CanonicalHeaderKey).
        
        //Header 映射 头 的 键和值. 如果响应有多个头是同一个键, 他们可以用逗号级联.
        //(RFC 2616中的4.2节 包含多个头的语义等价于 逗号分隔的序列 )
           //值复制 这个结构体里的其他字段, 从 Header 省略
           //键的映射规范化.
        
        Header Header


        // Body represents the response body.
        //
        // The http Client and Transport guarantee that Body is always
        // non-nil, even on responses without a body or responses with
        // a zero-length body. It is the caller's responsibility to
        // close Body.
        //
        // The Body is automatically dechunked if the server replied
        // with a "chunked" Transfer-Encoding.
       	//Body 表示 响应体
       	//http Client  和传输 确保 Body 通常是非nil,即使响应里不包含body 或 响应包含零长度的body.
       	//Body 会自动分块 如果 服务端回复包含一个 "chunked" Transfer-Encoding.
        Body io.ReadCloser


        // ContentLength records the length of the associated content.  The
        // value -1 indicates that the length is unknown.  Unless Request.Method
        // is "HEAD", values >= 0 indicate that the given number of bytes may
        // be read from Body.
        //ContentLength 记录关联内容的长度. 
           //  值 -1 说明 长度是未知的. 除非 Request.Method 是"HEAD"
           //值 >=0 说明给定的 字节数可能是从Body 读取的.
        ContentLength int64


        // Contains transfer encodings from outer-most to inner-most. Value is
        // nil, means that "identity" encoding is used.
          //Contains 转换编码从 最外层 到 最内层. 值是nil 意味着 使用 "identity" 编码.
        TransferEncoding []string


        // Close records whether the header directed that the connection be
        // closed after reading Body.  The value is advice for clients: neither
        // ReadResponse nor Response.Write ever closes a connection.
        //Close记录 头 连接在读取Body的时候 是否关闭.那个值通知 客户端: 不管是 ReadResponse还是Response.Write 曾经关闭连接。.
        Close bool

        // Trailer maps trailer keys to values, in the same
        // format as the header.
        //Trailer映射trailer的键成值, 和header 格式一样
        Trailer Header

        // The Request that was sent to obtain this Response.
        // Request's Body is nil (having already been consumed).
        // This is only populated for Client requests.
        //Request已发送，获得这个回应。
        //Request的Body 是nil(已经消耗完了).这个Client 请求受欢迎
        Request *Request

        // TLS contains information about the TLS connection on which the
        // response was received. It is nil for unencrypted responses.
        // The pointer is shared between responses and should not be
        // modified.
        //TLS 包含 响应 接收的 TLS 信息. 对于未加密的 响应它是nil. 指针共享 响应, 应该不修改.
        TLS *tls.ConnectionState
}
```
Response represents the response from an HTTP request.
Response表示HTTP请求的响应.


func Get
```golang
func Get(url string) (resp *Response, err error)
```
Get issues a GET to the specified URL. 
If the response is one of the following redirect codes, Get follows the redirect, up to a maximum of 10 redirects:
Get 给指定的URL 发 一个GET. 
如果响应 是重定向码中的一个,Get 遵循重定向,最多10个重定向：
```golang
301 (Moved Permanently)
302 (Found)
303 (See Other)
307 (Temporary Redirect)
```
An error is returned if there were too many redirects or if there was an HTTP protocol error. A non-2xx response doesn't cause an error.

When err is nil, resp always contains a non-nil resp.Body. Caller should close resp.Body when done reading from it.

Get is a wrapper around DefaultClient.Get.
如果有太多重定向 或 如果有 HTTP 协议错误会返回错误. 非2xx 响应不会引起错误.
当err是nil, resp通常包含一个非nil  resp.Body. 调用者在读取它的时候应该关闭resp.Body.


Example:
```golang
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	res, err := http.Get("http://www.google.com/robots.txt")
	if err != nil {
		log.Fatal(err)
	}
	robots, err := ioutil.ReadAll(res.Body)
	res.Body.Close()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s", robots)
}
```


func Head
```golang
func Head(url string) (resp *Response, err error)
```
Head issues a HEAD to the specified URL. 
If the response is one of the following redirect codes, Head follows the redirect after calling the Client's CheckRedirect function.
Head 给指定的URL 发一个HEAD.
如果响应是 重定向码中的一个,Get 在重定向后调用Client的CheckRedirect 函数.

```golang
301 (Moved Permanently)
302 (Found)
303 (See Other)
307 (Temporary Redirect)
```
Head is a wrapper around DefaultClient.Head
Head 是一个 DefaultClient.Head 包装器


func Post
```golang
func Post(url string, bodyType string, body io.Reader) (resp *Response, err error)
```
Post issues a POST to the specified URL.

Caller should close resp.Body when done reading from it.

Post is a wrapper around DefaultClient.Post
Post给指定的URL 发送一个POST.
调用者在读取它的时候应该关闭 resp.Body .
Post 是一个 DefaultClient.Post 包装器



func PostForm
```golang
func PostForm(url string, data url.Values) (resp *Response, err error)
```
PostForm issues a POST to the specified URL, with data's keys and values URL-encoded as the request body.

When err is nil, resp always contains a non-nil resp.Body. Caller should close resp.Body when done reading from it.

PostForm is a wrapper around DefaultClient.PostForm
PostForm 给指定的URL 发送一个 POST, 包含 data的键和值 的URL编码 的请求body.
当err是nil, resp 通常包含一个非 nil resp.Body.  当读取它的时候调用者应该关闭resp.Body. 


func ReadResponse
```golang
func ReadResponse(r *bufio.Reader, req *Request) (*Response, error)
```
ReadResponse reads and returns an HTTP response from r. 
The req parameter optionally specifies the Request that corresponds to this Response. 
If nil, a GET request is assumed. Clients must call resp.Body.Close when finished reading resp.Body. 
After that call, clients can inspect resp.Trailer to find key/value pairs included in the response trailer.
ReadResponse 从r中读取和返回 HTTP 响应.
req参数选项指定Request 成 正确的 Response.
如果nil,假设一个GET请求。当结束读取resp.Body时 Clients 必须调用resp.Body.Close.
在该调用后,客户端可以检查resp.Trailer 来发现 包含在响应的trailer 的 key/value 对. 



func (*Response) Cookies
```golang
func (r *Response) Cookies() []*Cookie
```
Cookies parses and returns the cookies set in the Set-Cookie headers.
Cookies 解析和返回  在 Set-Cookie 头设置的 cookie .



func (*Response) Location
```golang
func (r *Response) Location() (*url.URL, error)
```
Location returns the URL of the response's "Location" header, if present.
Relative redirects are resolved relative to the Response's Request. ErrNoLocation is returned if no Location header is present.
Location 如果存在的话, 返回 响应的"Location" 头 URL.
相对重定向 解析 Response的 相对请求. 如果Location 头没有的话 返回ErrNoLocation



func (*Response) ProtoAtLeast
```golang
func (r *Response) ProtoAtLeast(major, minor int) bool
```
ProtoAtLeast reports whether the HTTP protocol used in the response is at least major.minor.
ProtoAtLeast 报告 响应里使用的HTTP 协议 是否最少是major.minor.



func (*Response) Write
```golang
func (r *Response) Write(w io.Writer) error
```
Writes the response (header, body and trailer) in wire format. This method consults the following fields of the response:
Writes 以传输格式响应.这种方法咨询响应的以下字段：
```golang
StatusCode
ProtoMajor
ProtoMinor
Request.Method
TransferEncoding
Trailer
Body
ContentLength
Header, values for non-canonical keys will have unpredictable behavior
```
Body is closed after it is sent.
Body在发送后关闭.


type ResponseWriter
```golang
type ResponseWriter interface {
        // Header returns the header map that will be sent by WriteHeader.
        // Changing the header after a call to WriteHeader (or Write) has
        // no effect.
        //Header 返回 头映射 将会通过 WriteHeader 发送.
           //调用WriteHeader (或 Write)  改变 header不会受影响.
        Header() Header

        // Write writes the data to the connection as part of an HTTP reply.
        // If WriteHeader has not yet been called, Write calls WriteHeader(http.StatusOK)
        // before writing the data.  If the Header does not contain a
        // Content-Type line, Write adds a Content-Type set to the result of passing
        // the initial 512 bytes of written data to DetectContentType.
        //Write 将数据写入到连接为一个HTTP应答的一部分。
          // 如果 WriteHeader 没有 调用过, Write 在写入数据之前 调用WriteHeader (http.StatusOK).
          //如果Header 不包含一个Content-Type 行,Write 添加一个  Content-Type 设置 传递 初始512字节 写入数据到DetectContentType 的结果
        Write([]byte) (int, error)

        // WriteHeader sends an HTTP response header with status code.
        // If WriteHeader is not called explicitly, the first call to Write
        // will trigger an implicit WriteHeader(http.StatusOK).
        // Thus explicit calls to WriteHeader are mainly used to
        // send error codes.
        //WriteHeader 设置 HTTP 响应头 状态码.
           //如果WriteHeader 没有明确的调用, 第一次调用Write 将触发 一个隐WriteHeader (http.StatusOK).
           //明确调用WriteHeader 最主要的是用来发宋错误码.
        WriteHeader(int)
}
```
A ResponseWriter interface is used by an HTTP handler to construct an HTTP response.
ResponseWriter 接口用来HTTP处理器 构造 HTTP响应.


type RoundTripper
```golang
type RoundTripper interface {
        // RoundTrip executes a single HTTP transaction, returning
        // the Response for the request req.  RoundTrip should not
        // attempt to interpret the response.  In particular,
        // RoundTrip must return err == nil if it obtained a response,
        // regardless of the response's HTTP status code.  A non-nil
        // err should be reserved for failure to obtain a response.
        // Similarly, RoundTrip should not attempt to handle
        // higher-level protocol details such as redirects,
        // authentication, or cookies.
        //
        // RoundTrip should not modify the request, except for
        // consuming and closing the Body, including on errors. The
        // request's URL and Header fields are guaranteed to be
        // initialized.
        
        //RoundTrip执行单个 HTTP事务, 返回请求 req的 Response.
        //RoundTrip 不应该尝试解释响应.
           //特别是RoundTrip  如果它获取一个响应, 无论响应的状态码是什么,必须返回 err == nil.
           //一个非nil err 应该保留获取的失败响应.
           //同样,RoundTrip 不应该 尝试处理高级 协议细节 例如 重定向,验证, 或 cookies.
           
        //RoundTrip 不应该修改请求,除了消耗和 关闭 Body, 包括错误.请求的URL和Header 字段  确保初始化.
        RoundTrip(*Request) (*Response, error)
}
```
RoundTripper is an interface representing the ability to execute a single HTTP transaction, obtaining the Response for a given Request.

A RoundTripper must be safe for concurrent use by multiple goroutines.
RoundTripper是一个表示 能执行 单个 HTTP事务, 从给定的Request 获取响应的 接口.
RoundTripper 使用多个 goroutines 并发 必须是安全的

```golang
var DefaultTransport RoundTripper = &Transport{
        Proxy: ProxyFromEnvironment,
        Dial: (&net.Dialer{
                Timeout:   30 * time.Second,
                KeepAlive: 30 * time.Second,
        }).Dial,
        TLSHandshakeTimeout: 10 * time.Second,
}
```
DefaultTransport is the default implementation of Transport and is used by DefaultClient. 
It establishes network connections as needed and caches them for reuse by subsequent calls. 
It uses HTTP proxies as directed by the $HTTP_PROXY and $NO_PROXY (or $http_proxy and $no_proxy) environment variables.




func NewFileTransport

func NewFileTransport(fs FileSystem) RoundTripper
NewFileTransport returns a new RoundTripper, serving the provided FileSystem. The returned RoundTripper ignores the URL host in its incoming requests, as well as most other properties of the request.

The typical use case for NewFileTransport is to register the "file" protocol with a Transport, as in:

t := &http.Transport{}
t.RegisterProtocol("file", http.NewFileTransport(http.Dir("/")))
c := &http.Client{Transport: t}
res, err := c.Get("file:///etc/passwd")
...
type ServeMux

type ServeMux struct {
        // contains filtered or unexported fields
}
ServeMux is an HTTP request multiplexer. It matches the URL of each incoming request against a list of registered patterns and calls the handler for the pattern that most closely matches the URL.

Patterns name fixed, rooted paths, like "/favicon.ico", or rooted subtrees, like "/images/" (note the trailing slash). Longer patterns take precedence over shorter ones, so that if there are handlers registered for both "/images/" and "/images/thumbnails/", the latter handler will be called for paths beginning "/images/thumbnails/" and the former will receive requests for any other paths in the "/images/" subtree.

Note that since a pattern ending in a slash names a rooted subtree, the pattern "/" matches all paths not matched by other registered patterns, not just the URL with Path == "/".

Patterns may optionally begin with a host name, restricting matches to URLs on that host only. Host-specific patterns take precedence over general patterns, so that a handler might register for the two patterns "/codesearch" and "codesearch.google.com/" without also taking over requests for "http://www.google.com/".

ServeMux also takes care of sanitizing the URL request path, redirecting any request containing . or .. elements to an equivalent .- and ..-free URL.

func NewServeMux

func NewServeMux() *ServeMux
NewServeMux allocates and returns a new ServeMux.

func (*ServeMux) Handle

func (mux *ServeMux) Handle(pattern string, handler Handler)
Handle registers the handler for the given pattern. If a handler already exists for pattern, Handle panics.

▹ Example

func (*ServeMux) HandleFunc

func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request))
HandleFunc registers the handler function for the given pattern.

func (*ServeMux) Handler

func (mux *ServeMux) Handler(r *Request) (h Handler, pattern string)
Handler returns the handler to use for the given request, consulting r.Method, r.Host, and r.URL.Path. It always returns a non-nil handler. If the path is not in its canonical form, the handler will be an internally-generated handler that redirects to the canonical path.

Handler also returns the registered pattern that matches the request or, in the case of internally-generated redirects, the pattern that will match after following the redirect.

If there is no registered handler that applies to the request, Handler returns a “page not found” handler and an empty pattern.

func (*ServeMux) ServeHTTP

func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request)
ServeHTTP dispatches the request to the handler whose pattern most closely matches the request URL.

type Server

type Server struct {
        Addr           string        // TCP address to listen on, ":http" if empty
        Handler        Handler       // handler to invoke, http.DefaultServeMux if nil
        ReadTimeout    time.Duration // maximum duration before timing out read of the request
        WriteTimeout   time.Duration // maximum duration before timing out write of the response
        MaxHeaderBytes int           // maximum size of request headers, DefaultMaxHeaderBytes if 0
        TLSConfig      *tls.Config   // optional TLS config, used by ListenAndServeTLS

        // TLSNextProto optionally specifies a function to take over
        // ownership of the provided TLS connection when an NPN
        // protocol upgrade has occurred.  The map key is the protocol
        // name negotiated. The Handler argument should be used to
        // handle HTTP requests and will initialize the Request's TLS
        // and RemoteAddr if not already set.  The connection is
        // automatically closed when the function returns.
        TLSNextProto map[string]func(*Server, *tls.Conn, Handler)

        // ConnState specifies an optional callback function that is
        // called when a client connection changes state. See the
        // ConnState type and associated constants for details.
        ConnState func(net.Conn, ConnState)

        // ErrorLog specifies an optional logger for errors accepting
        // connections and unexpected behavior from handlers.
        // If nil, logging goes to os.Stderr via the log package's
        // standard logger.
        ErrorLog *log.Logger
        // contains filtered or unexported fields
}
A Server defines parameters for running an HTTP server. The zero value for Server is a valid configuration.

func (*Server) ListenAndServe

func (srv *Server) ListenAndServe() error
ListenAndServe listens on the TCP network address srv.Addr and then calls Serve to handle requests on incoming connections. If srv.Addr is blank, ":http" is used.

func (*Server) ListenAndServeTLS

func (srv *Server) ListenAndServeTLS(certFile, keyFile string) error
ListenAndServeTLS listens on the TCP network address srv.Addr and then calls Serve to handle requests on incoming TLS connections.

Filenames containing a certificate and matching private key for the server must be provided. If the certificate is signed by a certificate authority, the certFile should be the concatenation of the server's certificate followed by the CA's certificate.

If srv.Addr is blank, ":https" is used.

func (*Server) Serve

func (srv *Server) Serve(l net.Listener) error
Serve accepts incoming connections on the Listener l, creating a new service goroutine for each. The service goroutines read requests and then call srv.Handler to reply to them.

func (*Server) SetKeepAlivesEnabled

func (s *Server) SetKeepAlivesEnabled(v bool)
SetKeepAlivesEnabled controls whether HTTP keep-alives are enabled. By default, keep-alives are always enabled. Only very resource-constrained environments or servers in the process of shutting down should disable them.

type Transport

type Transport struct {

        // Proxy specifies a function to return a proxy for a given
        // Request. If the function returns a non-nil error, the
        // request is aborted with the provided error.
        // If Proxy is nil or returns a nil *URL, no proxy is used.
        Proxy func(*Request) (*url.URL, error)

        // Dial specifies the dial function for creating TCP
        // connections.
        // If Dial is nil, net.Dial is used.
        Dial func(network, addr string) (net.Conn, error)

        // TLSClientConfig specifies the TLS configuration to use with
        // tls.Client. If nil, the default configuration is used.
        TLSClientConfig *tls.Config

        // TLSHandshakeTimeout specifies the maximum amount of time waiting to
        // wait for a TLS handshake. Zero means no timeout.
        TLSHandshakeTimeout time.Duration

        // DisableKeepAlives, if true, prevents re-use of TCP connections
        // between different HTTP requests.
        DisableKeepAlives bool

        // DisableCompression, if true, prevents the Transport from
        // requesting compression with an "Accept-Encoding: gzip"
        // request header when the Request contains no existing
        // Accept-Encoding value. If the Transport requests gzip on
        // its own and gets a gzipped response, it's transparently
        // decoded in the Response.Body. However, if the user
        // explicitly requested gzip it is not automatically
        // uncompressed.
        DisableCompression bool

        // MaxIdleConnsPerHost, if non-zero, controls the maximum idle
        // (keep-alive) to keep per-host.  If zero,
        // DefaultMaxIdleConnsPerHost is used.
        MaxIdleConnsPerHost int

        // ResponseHeaderTimeout, if non-zero, specifies the amount of
        // time to wait for a server's response headers after fully
        // writing the request (including its body, if any). This
        // time does not include the time to read the response body.
        ResponseHeaderTimeout time.Duration
        // contains filtered or unexported fields
}
Transport is an implementation of RoundTripper that supports http, https, and http proxies (for either http or https with CONNECT). Transport can also cache connections for future re-use.

func (*Transport) CancelRequest

func (t *Transport) CancelRequest(req *Request)
CancelRequest cancels an in-flight request by closing its connection.

func (*Transport) CloseIdleConnections

func (t *Transport) CloseIdleConnections()
CloseIdleConnections closes any connections which were previously connected from previous requests but are now sitting idle in a "keep-alive" state. It does not interrupt any connections currently in use.

func (*Transport) RegisterProtocol

func (t *Transport) RegisterProtocol(scheme string, rt RoundTripper)
RegisterProtocol registers a new protocol with scheme. The Transport will pass requests using the given scheme to rt. It is rt's responsibility to simulate HTTP request semantics.

RegisterProtocol can be used by other packages to provide implementations of protocol schemes like "ftp" or "file".

func (*Transport) RoundTrip

func (t *Transport) RoundTrip(req *Request) (resp *Response, err error)
RoundTrip implements the RoundTripper interface.

For higher-level HTTP client support (such as handling of cookies and redirects), see Get, Post, and the Client type.















