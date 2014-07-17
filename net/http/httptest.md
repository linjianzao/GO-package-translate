Package httptest

Overview ▾

Package httptest provides utilities for HTTP testing.
httptest包提供 HTTP 实用 测试.


Constants
```golang
const DefaultRemoteAddr = "1.2.3.4"
```
DefaultRemoteAddr is the default remote address to return in RemoteAddr if an explicit DefaultRemoteAddr isn't set on ResponseRecorder.
DefaultRemoteAddr 是在RemoteAddr 返回的 默认的远程连接地址  , 如果 明确DefaultRemoteAddr 未设置ResponseRecorder.

 
type ResponseRecorder
```golang
type ResponseRecorder struct {
        Code      int           // the HTTP response code from WriteHeader
        								  //WriteHeader 的HTTP 响应码
        
        HeaderMap http.Header   // the HTTP response headers
										   //HTTP响应头        
        
        Body      *bytes.Buffer // if non-nil, the bytes.Buffer to append written data to
        								   //如果非 nil, bytes.Buffer 追加 写入数据
        
        Flushed   bool
        // contains filtered or unexported fields
}
```
ResponseRecorder is an implementation of http.ResponseWriter that records its mutations for later inspection in tests.
ResponseRecorder 是 http.ResponseWriter的实现,  记录 它突变测试以后检查。



▾ Example
```golang
package main

import (
	"fmt"
	"log"
	"net/http"
	"net/http/httptest"
)

func main() {
	handler := func(w http.ResponseWriter, r *http.Request) {
		http.Error(w, "something failed", http.StatusInternalServerError)
	}

	req, err := http.NewRequest("GET", "http://example.com/foo", nil)
	if err != nil {
		log.Fatal(err)
	}

	w := httptest.NewRecorder()
	handler(w, req)

	fmt.Printf("%d - %s", w.Code, w.Body.String())
}
```



func NewRecorder
```golang
func NewRecorder() *ResponseRecorder
```
NewRecorder returns an initialized ResponseRecorder.
NewRecorder 返回一个 初始化的ResponseRecorder


func (*ResponseRecorder) Flush
```golang
func (rw *ResponseRecorder) Flush()
```
Flush sets rw.Flushed to true.
Flush 设置 rw.Flushed 成true



func (*ResponseRecorder) Header
```golang
func (rw *ResponseRecorder) Header() http.Header
```
Header returns the response headers.
Header 返回 响应头



func (*ResponseRecorder) Write
```golang
func (rw *ResponseRecorder) Write(buf []byte) (int, error)
```
Write always succeeds and writes to rw.Body, if not nil.
Write  通常 成功 并 写入rw.Body, 如果没有 nil.


func (*ResponseRecorder) WriteHeader
```golang
func (rw *ResponseRecorder) WriteHeader(code int)
```
WriteHeader sets rw.Code.
WriteHeader 设置 rw.Code.



type Server

type Server struct {
        URL      string // base URL of form http://ipaddr:port with no trailing slash
        Listener net.Listener

        // TLS is the optional TLS configuration, populated with a new config
        // after TLS is started. If set on an unstarted server before StartTLS
        // is called, existing fields are copied into the new config.
        //TLS 是 可选的 TLS 配置,启动后填充了一个新的配置TLS.
           //如果 在StartTLS 调用之前 设置一个尚未启动的服务器, 存在的字段 被复制到新的配置.
           
        TLS *tls.Config



        // Config may be changed after calling NewUnstartedServer and
        // before Start or StartTLS.
        //Config 可能 在调用NewUnstartedServer之后和 Start和 StartTLS之前  改变
        Config *http.Server
        // contains filtered or unexported fields
}
A Server is an HTTP server listening on a system-chosen port on the local loopback interface, for use in end-to-end HTTP tests.
Server 在本地环回接口上的系统选择的端口上的HTTP服务器监听, 用来 做 端对端的 HTTP 测试.

▹ Example
```golang
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"net/http/httptest"
)

func main() {
	ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Hello, client")
	}))
	defer ts.Close()

	res, err := http.Get(ts.URL)
	if err != nil {
		log.Fatal(err)
	}
	greeting, err := ioutil.ReadAll(res.Body)
	res.Body.Close()
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("%s", greeting)
}
```


func NewServer
```golang
func NewServer(handler http.Handler) *Server
```
NewServer starts and returns a new Server. 
The caller should call Close when finished, to shut it down.
NewServer 启动和 返回一个新的 Server.
当结束的时候 调用者应该调用 Close 来关闭它.



func NewTLSServer
```golang
func NewTLSServer(handler http.Handler) *Server
```
NewTLSServer starts and returns a new Server using TLS. 
The caller should call Close when finished, to shut it down.
NewTLSServer 使用TLS 启用 和返回一个  新的Server.
当结束的时候 调用者应该调用 Close 来关闭它.


func NewUnstartedServer
```golang
func NewUnstartedServer(handler http.Handler) *Server
```
NewUnstartedServer returns a new Server but doesn't start it.

After changing its configuration, the caller should call Start or StartTLS.

The caller should call Close when finished, to shut it down.

NewUnstartedServer 返回 一个新的 Server 凡是不启用它.

在修改它的配置后,调用者应该调用Start 或 StartTLS.

当结束的时候 调用者应该调用 Close 来关闭它.



func (*Server) Close
```golang
func (s *Server) Close()
```
Close shuts down the server and blocks until all outstanding requests on this server have completed.
Close 关闭 服务器和 阻塞 直到 所有的 未完成的请求 在这台服务器已完成.



func (*Server) CloseClientConnections
```golang
func (s *Server) CloseClientConnections()
```
CloseClientConnections closes any currently open HTTP connections to the test Server.
CloseClientConnections 关闭任何当前 测试 Server打开的HTTP连接


func (*Server) Start
```golang
func (s *Server) Start()
```
Start starts a server from NewUnstartedServer.
Start 从NewUnstartedServer 启动一个服务


func (*Server) StartTLS
```golang
func (s *Server) StartTLS()
```
StartTLS starts TLS on a server from NewUnstartedServer.
StartTLS 从 NewUnstartedServer 在一个服务器 上 启用TLS.






























 