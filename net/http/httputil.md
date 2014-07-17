Package httputil


Overview ▾

Package httputil provides HTTP utility functions, complementing the more common ones in the net/http package.
httputil包 提供 HTTP使用功能函数,  在 net/http 包 补充了较常见的.


Variables
```golang
var (
        ErrPersistEOF = &http.ProtocolError{ErrorString: "persistent connection closed"}
        ErrClosed     = &http.ProtocolError{ErrorString: "connection closed by user"}
        ErrPipeline   = &http.ProtocolError{ErrorString: "pipeline error"}
)
```

```golang
var ErrLineTooLong = errors.New("header line too long")
```


func DumpRequest
```golang
func DumpRequest(req *http.Request, body bool) (dump []byte, err error)
```
DumpRequest returns the as-received wire representation of req, optionally including the request body, for debugging. 
DumpRequest is semantically a no-op, but in order to dump the body, it reads the body data into memory and changes req.Body to refer to the in-memory copy. 
The documentation for http.Request.Write details which fields of req are used.
DumpRequest 返回  作为接收的 req表示, 可选的包含 请求体, 进行调试.
DumpRequest在语义上是一个空操作, 但是 为了 body, 它读取 body 数据 到 内存 和 改变req.Body 引用的内存复制.
http.Request.Write 文档 详细 字段 req 的使用 



func DumpRequestOut
```golang
func DumpRequestOut(req *http.Request, body bool) ([]byte, error)
```
DumpRequestOut is like DumpRequest but includes headers that the standard http.Transport adds, such as User-Agent.
DumpRequestOut 类似DumpRequest 但是  包含 头 ,   标准http.Transport补充,   例如  User-Agent.


func DumpResponse
```golang
func DumpResponse(resp *http.Response, body bool) (dump []byte, err error)
```
DumpResponse is like DumpRequest but dumps a response.
DumpResponse 类似 DumpRequest 但是转储 一个响应.



func NewChunkedReader
```golang
func NewChunkedReader(r io.Reader) io.Reader
```
NewChunkedReader returns a new chunkedReader that translates the data read from r out of HTTP "chunked" format before returning it.
The chunkedReader returns io.EOF when the final 0-length chunk is read.

NewChunkedReader is not needed by normal applications. 
The http package automatically decodes chunking when reading response bodies.
NewChunkedReader 返回一个新的 chunkedReader, 在返回它之前 转换从r中读取的数据 成HTTP "chunked" 格式.
当 结束0 长度 块 读取. chunkedReader 返回一个io.EOF.

正确的应用 不需要 NewChunkedReader



func NewChunkedWriter
```golang
func NewChunkedWriter(w io.Writer) io.WriteCloser
```
NewChunkedWriter returns a new chunkedWriter that translates writes into HTTP "chunked" format before writing them to w. 
Closing the returned chunkedWriter sends the final 0-length chunk that marks the end of the stream.

NewChunkedWriter is not needed by normal applications. 
The http package adds chunking automatically if handlers don't set a Content-Length header. 
Using NewChunkedWriter inside a handler would result in double chunking or chunking with a Content-Length length, both of which are wrong.
NewChunkedWriter 返回一个新的  chunkedWriter 在写入他们到w  之前 转化成 写入 HTTP "chunked"  格式.
正确的应用 不需要 NewChunkedWriter.
如果 处理器 没有设置 Content-Length 头, http包 自动添加块.
在内部处理器 使用 NewChunkedWriter  会导致 双分块或分块用的Content-Length长, 都是错误的.



type ClientConn
```golang
type ClientConn struct {
        // contains filtered or unexported fields
}
```
A ClientConn sends request and receives headers over an underlying connection, while respecting the HTTP keepalive logic. 
ClientConn supports hijacking the connection calling Hijack to regain control of the underlying net.Conn and deal with it as desired.

ClientConn is low-level and old. Applications should instead use Client or Transport in the net/http package.
ClientConn 在底层连接 发送 请求 和接收头,  遵循 HTTP保持连接逻辑。
ClientConn 支持 挟持连接 调用 Hijack 来 重新获得底层 的net.Conn  控制权 和它应有的作用。
ClientConn 是低级 和旧的. 应用应该用 net/http包里的 Client 或 Transport代替.



func NewClientConn
```golang
func NewClientConn(c net.Conn, r *bufio.Reader) *ClientConn
```
NewClientConn returns a new ClientConn reading and writing c. If r is not nil, it is the buffer to use when reading c.

ClientConn is low-level and old. Applications should use Client or Transport in the net/http package.
NewClientConn 返回一个新的 ClientConn 读取和写 c. 如果r 不是 nil, 当读取c的时候 使用缓冲区.
ClientConn 是低级 和旧的.  应用应该用 net/http包里的 Client 或 Transport代替.



func NewProxyClientConn
```golang
func NewProxyClientConn(c net.Conn, r *bufio.Reader) *ClientConn
```
NewProxyClientConn works like NewClientConn but writes Requests using Request's WriteProxy method.

New code should not use NewProxyClientConn. See Client or Transport in the net/http package instead.
NewProxyClientConn 工作类似 NewClientConn 但是 使用 Request的WriteProxy 方法 写 Requests.
新的代码 不应该使用 NewProxyClientConn. 查看 net/http包里的 Client 或 Transport代替.



func (*ClientConn) Close
```golang
func (cc *ClientConn) Close() error
```
Close calls Hijack and then also closes the underlying connection
Close 调用 Hijack 然后关闭底层的连接


func (*ClientConn) Do
```golang
func (cc *ClientConn) Do(req *http.Request) (resp *http.Response, err error)
```
Do is convenience method that writes a request and reads a response.
DO 是方便的  写一个请求 和  读取一个响应的方法.


func (*ClientConn) Hijack
```golang
func (cc *ClientConn) Hijack() (c net.Conn, r *bufio.Reader)
```
Hijack detaches the ClientConn and returns the underlying connection as well as the read-side bufio which may have some left over data. 
Hijack may be called before the user or Read have signaled the end of the keep-alive logic. 
The user should not call Hijack while Read or Write is in progress.
Hijack 分离 ClientConn 并返回 底层的连接   以及 作为读取端bufio其中可能有一些遗留的数据。
Hijack 可能 会在 用户 或 Read 有信号keep-alive逻辑的结束 之前调用.
用户 在 进程  Read  或 Write 的时候 不应该 调用 Hijack.



func (*ClientConn) Pending
```golang
func (cc *ClientConn) Pending() int
```
Pending returns the number of unanswered requests that have been sent on the connection.
Pending 返回  在连接上  已经发送的  未应答 请求 的数量,



func (*ClientConn) Read
```golang
func (cc *ClientConn) Read(req *http.Request) (resp *http.Response, err error)
```
Read reads the next response from the wire. 
A valid response might be returned together with an ErrPersistEOF, which means that the remote requested that this be the last request serviced. 
Read can be called concurrently with Write, but not with another Read.
Read 从wire 读取 下一个响应.




func (*ClientConn) Write

func (cc *ClientConn) Write(req *http.Request) (err error)
Write writes a request. An ErrPersistEOF error is returned if the connection has been closed in an HTTP keepalive sense. If req.Close equals true, the keepalive connection is logically closed after this request and the opposing server is informed. An ErrUnexpectedEOF indicates the remote closed the underlying TCP connection, which is usually considered as graceful close.

type ReverseProxy

type ReverseProxy struct {
        // Director must be a function which modifies
        // the request into a new request to be sent
        // using Transport. Its response is then copied
        // back to the original client unmodified.
        Director func(*http.Request)

        // The transport used to perform proxy requests.
        // If nil, http.DefaultTransport is used.
        Transport http.RoundTripper

        // FlushInterval specifies the flush interval
        // to flush to the client while copying the
        // response body.
        // If zero, no periodic flushing is done.
        FlushInterval time.Duration
}
ReverseProxy is an HTTP Handler that takes an incoming request and sends it to another server, proxying the response back to the client.

func NewSingleHostReverseProxy

func NewSingleHostReverseProxy(target *url.URL) *ReverseProxy
NewSingleHostReverseProxy returns a new ReverseProxy that rewrites URLs to the scheme, host, and base path provided in target. If the target's path is "/base" and the incoming request was for "/dir", the target request will be for /base/dir.

func (*ReverseProxy) ServeHTTP

func (p *ReverseProxy) ServeHTTP(rw http.ResponseWriter, req *http.Request)
type ServerConn

type ServerConn struct {
        // contains filtered or unexported fields
}
A ServerConn reads requests and sends responses over an underlying connection, until the HTTP keepalive logic commands an end. ServerConn also allows hijacking the underlying connection by calling Hijack to regain control over the connection. ServerConn supports pipe-lining, i.e. requests can be read out of sync (but in the same order) while the respective responses are sent.

ServerConn is low-level and old. Applications should instead use Server in the net/http package.

func NewServerConn

func NewServerConn(c net.Conn, r *bufio.Reader) *ServerConn
NewServerConn returns a new ServerConn reading and writing c. If r is not nil, it is the buffer to use when reading c.

ServerConn is low-level and old. Applications should instead use Server in the net/http package.

func (*ServerConn) Close

func (sc *ServerConn) Close() error
Close calls Hijack and then also closes the underlying connection

func (*ServerConn) Hijack

func (sc *ServerConn) Hijack() (c net.Conn, r *bufio.Reader)
Hijack detaches the ServerConn and returns the underlying connection as well as the read-side bufio which may have some left over data. Hijack may be called before Read has signaled the end of the keep-alive logic. The user should not call Hijack while Read or Write is in progress.

func (*ServerConn) Pending

func (sc *ServerConn) Pending() int
Pending returns the number of unanswered requests that have been received on the connection.

func (*ServerConn) Read

func (sc *ServerConn) Read() (req *http.Request, err error)
Read returns the next request on the wire. An ErrPersistEOF is returned if it is gracefully determined that there are no more requests (e.g. after the first request on an HTTP/1.0 connection, or after a Connection:close on a HTTP/1.1 connection).

func (*ServerConn) Write

func (sc *ServerConn) Write(req *http.Request, resp *http.Response) error
Write writes resp in response to req. To close the connection gracefully, set the Response.Close field to true. Write should be considered operational until it returns an error, regardless of any errors returned on the Read side.