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
ServeContent 使用 提供的ReadSeeker 的内容 响应请求
ServeContent 比io.Copy 最主要的好处是  它正确处理Range请求,设置MIME类型,和处理If-Modified-Since 请求.
如果响应的Content-Type头 没有设置,ServeContent 首先尝试 推断出 文件扩展名的类型, 如果失败的话, 返回读取的第一块的内容 并传给DetectContentType.
这个名字是其他未使用; 特别是在响应里它可以是空的 并且 没有 发送过的

如果modtime 不是 零时间,ServeContent 在响应时 在 Last-Modified头包含它



func ServeFile

func ServeFile(w ResponseWriter, r *Request, name string)
ServeFile replies to the request with the contents of the named file or directory.

func SetCookie

func SetCookie(w ResponseWriter, cookie *Cookie)
SetCookie adds a Set-Cookie header to the provided ResponseWriter's headers.

func StatusText

func StatusText(code int) string
StatusText returns a text for the HTTP status code. It returns the empty string if the code is unknown.



































