Package rpc

Overview ▾

Package rpc provides access to the exported methods of an object across a network or other I/O connection. 
A server registers an object, making it visible as a service with the name of the type of the object. 
After registration, exported methods of the object will be accessible remotely. 
A server may register multiple objects (services) of different types but it is an error to register multiple objects of the same type.
rpc 提供 通过网络 或 其他I/O 连接 访问 对象的导出方法.
服务注册一个对象, 让服务端对象的类型名称可见.
在注册之后, 对象的导出方法 将可以远程访问。
一个服务可以注册 多个 不同类型的对象 但是 注册多个  同一个类型 的对象会返回一个错误,.

Only methods that satisfy these criteria will be made available for remote access; other methods will be ignored:
只有满足这些条件的方法，将提供远程访问;其他的方法会忽略:

```golang
- the method is exported.
- the method has two arguments, both exported (or builtin) types.
- the method's second argument is a pointer.
- the method has return type error.

- 方法可导出
- 方法有两个参数, 都是可以导出的类型(或内建).
- 方法的第二个参数是一个 指针.
- 该方法有返回类型错误
```

In effect, the method must look schematically like
在效果上,该方法 语义 必须看起来像

```golang
func (t *T) MethodName(argType T1, replyType *T2) error
```

where T, T1 and T2 can be marshaled by encoding/gob. These requirements apply even if a different codec is used. (In the future, these requirements may soften for custom codecs.)

The method's first argument represents the arguments provided by the caller; 
the second argument represents the result parameters to be returned to the caller. 
The method's return value, if non-nil, is passed back as a string that the client sees as if created by errors.New. 
If an error is returned, the reply parameter will not be sent back to the client.

The server may handle requests on a single connection by calling ServeConn. 
More typically it will create a network listener and call Accept or, for an HTTP listener, HandleHTTP and http.Serve.

A client wishing to use the service establishes a connection and then invokes NewClient on the connection. 
The convenience function Dial (DialHTTP) performs both steps for a raw network connection (an HTTP connection). 
The resulting Client object has two methods, Call and Go, that specify the service and method to call, a pointer containing the arguments, and a pointer to receive the result parameters.

The Call method waits for the remote call to complete while the Go method launches the call asynchronously and signals completion using the Call structure's Done channel.

Unless an explicit codec is set up, package encoding/gob is used to transport the data.

Here is a simple example. A server wishes to export an object of type Arith:


T, T1 和 T2 可以 被encoding/gob 编码. 即使不同的编解码器使用,这些要求都适用。(未来,对于定制编解码器,这些要求可能软化)

方法的第一个参数表示  调用者提供的参数;
第二个参数表示要返回给调用者的结果参数.
方法的返回值, 如果 不是nil, 如果用 errors.New 创建  会返回 一个自字符串给客户端查看.
如果返回一个错误, 回复参数会返回给客户端.

服务可以用过调用 ServeConn 来处理单个连接上的请求.
通常 它将会创建一个网络监听 并调用 Accept 或, 一个 HTTP 监听, HandleHTTP 和http.Serve.

客户端希望用该服务建立一个连接  然后在连接上调用NewClient.
函数  Dial (DialHTTP)为 一个原始的网络连接(HTTP 连接)执行这两个步骤.
结果的Client 对象 有两个方法 Call 和 Go, 那用来指定调用 的 服务端 和 方法, 含参数的指针, 和一个 接收结果的指针参数.

Call 方法 等待远程调用完成, 直到Go 方法启动异步调用 并且  使用Call 结构体的 Done channel 完成信号.

除非设置显式的编解码器,encoding/gob包用来传输数据.

者有个简单的例子. 一个服务端系统 导出 Arith 对象 的类型:

```golang
package server

type Args struct {
	A, B int
}

type Quotient struct {
	Quo, Rem int
}

type Arith int

func (t *Arith) Multiply(args *Args, reply *int) error {
	*reply = args.A * args.B
	return nil
}

func (t *Arith) Divide(args *Args, quo *Quotient) error {
	if args.B == 0 {
		return errors.New("divide by zero")
	}
	quo.Quo = args.A / args.B
	quo.Rem = args.A % args.B
	return nil
}
```

The server calls (for HTTP service):
该服务调用(对于HTTP服务):

```golang
arith := new(Arith)
rpc.Register(arith)
rpc.HandleHTTP()
l, e := net.Listen("tcp", ":1234")
if e != nil {
	log.Fatal("listen error:", e)
}
go http.Serve(l, nil)
```

At this point, clients can see a service "Arith" with methods "Arith.Multiply" and "Arith.Divide". To invoke one, a client first dials the server:
这一点 ,客户端可以用方法 "Arith.Multiply" 和  "Arith.Divide" 查看 服务 "Arith". 一调用一个客户端首先dials服务端 ,

```golang
client, err := rpc.DialHTTP("tcp", serverAddress + ":1234")
if err != nil {
	log.Fatal("dialing:", err)
}
```

Then it can make a remote call:
然后它可以远程调用:

```golang
// Synchronous call
args := &server.Args{7,8}
var reply int
err = client.Call("Arith.Multiply", args, &reply)
if err != nil {
	log.Fatal("arith error:", err)
}
fmt.Printf("Arith: %d*%d=%d", args.A, args.B, reply)
```

or

```golang
// Asynchronous call
quotient := new(Quotient)
divCall := client.Go("Arith.Divide", args, quotient, nil)
replyCall := <-divCall.Done	// will be equal to divCall
// check errors, print, etc.
```
A server implementation will often provide a simple, type-safe wrapper for the client.
一个服务端 实现, 通常提供一个简单的,类型安全包装的 给客户端.


Constants
```golang
const (
        // Defaults used by HandleHTTP
        DefaultRPCPath   = "/_goRPC_"
        DefaultDebugPath = "/debug/rpc"
)
```

Variables
```golang
var DefaultServer = NewServer()
```
DefaultServer is the default instance of *Server.
DefaultServer 是 默认的 实例*Server.


```golang
var ErrShutdown = errors.New("connection is shut down")
```


func Accept
```golang
func Accept(lis net.Listener)
```
Accept accepts connections on the listener and serves requests to DefaultServer for each incoming connection. 
Accept blocks; the caller typically invokes it in a go statement.
Accept 接收 监听上的连接 并且 服务 对每个传入的连接 请求 DefaultServer
接收块; 调用者通常 在 go声明上调用它



func HandleHTTP
```golang
func HandleHTTP()
```
HandleHTTP registers an HTTP handler for RPC messages to DefaultServer on DefaultRPCPath and a debugging handler on DefaultDebugPath. 
It is still necessary to invoke http.Serve(), typically in a go statement.
HandleHTTP   注册一个HTTP 处理器 为RPC信息在DefaultRPCPath 上的DefaultServer 和一个DefaultDebugPath 上的调试处理程序



func Register
```golang
func Register(rcvr interface{}) error
```
Register publishes the receiver's methods in the DefaultServer.
Register 公开 在 DefaultServer 里 接收的方法



func RegisterName
```golang
func RegisterName(name string, rcvr interface{}) error
```
RegisterName is like Register but uses the provided name for the type instead of the receiver's concrete type.
RegisterName 类似 Register 但是 使用 提供的名字  代替该类型 成 接收者的 具体类型


func ServeCodec
```golang
func ServeCodec(codec ServerCodec)
```
ServeCodec is like ServeConn but uses the specified codec to decode requests and encode responses.
ServeCodec 类似 ServeConn 但是使用指定的编解码器 解码请求和编码响应.



func ServeConn
```golang
func ServeConn(conn io.ReadWriteCloser)
```
ServeConn runs the DefaultServer on a single connection. 
ServeConn blocks, serving the connection until the client hangs up. 
The caller typically invokes ServeConn in a go statement. 
ServeConn uses the gob wire format (see package gob) on the connection. 
To use an alternate codec, use ServeCodec.
ServeConn 在单个连接上 执行 DefaultServer.
ServeConn 块, 服务连接直到连接 断了.
调用者通常在go声明中 调用 ServeConn.
ServeConn 在连接中 使用 gob wire 格式(查看gob包).
要使用替代的编解码器,使用ServeCodec.



func ServeRequest
```golang
func ServeRequest(codec ServerCodec) error
```
ServeRequest is like ServeCodec but synchronously serves a single request. 
It does not close the codec upon completion.
ServeRequest 类似 ServeCodec 但是 同步提供一个单一的请求。
编解码器完成后不会关闭.


type Call
```golang
type Call struct {
        ServiceMethod string      // The name of the service and method to call.
        									  // 调用的 服务和方法 名字
        
        Args          interface{} // The argument to the function (*struct).
        									 //function (*struct)的参数
        
        Reply         interface{} // The reply from the function (*struct).
        									 //function (*struct)的回复.
        
        Error         error       // After completion, the error status.
        									  //完成后的 错误状态
        
        Done          chan *Call  // Strobes when call is complete.
        									
}
```
Call represents an active RPC.
Call表示一个活动的 RPC

type Client
```golang
type Client struct {
        // contains filtered or unexported fields
}
```
Client represents an RPC Client. 
There may be multiple outstanding Calls associated with a single Client, and a Client may be used by multiple goroutines simultaneously.
Client表示一个RPC Client.
有可能多个未完成 Calls 对应的 单个 Client, 一个 Client 可能 使用多Go例程同时进行。


func Dial
```golang
func Dial(network, address string) (*Client, error)
```
Dial connects to an RPC server at the specified network address.
Dial 连接一个RPC 服务器到 指定的网络地址.


func DialHTTP
```golang
func DialHTTP(network, address string) (*Client, error)
```
DialHTTP connects to an HTTP RPC server at the specified network address listening on the default HTTP RPC path.
DialHTTP 连接 HTTP RPC 服务器到指定的network address  监听默认的HTTP RPC 路径


func DialHTTPPath
```golang
func DialHTTPPath(network, address, path string) (*Client, error)
```
DialHTTPPath connects to an HTTP RPC server at the specified network address and path.
DialHTTPPath 连接一个HTTP RPC 到 指定的 network address 和path


func NewClient
```golang
func NewClient(conn io.ReadWriteCloser) *Client
```
NewClient returns a new Client to handle requests to the set of services at the other end of the connection. 
It adds a buffer to the write side of the connection so the header and payload are sent as a unit.
NewClient 返回一个新的 Client 来处理 请求 以连接到另一端的一组服务。
它添加一个缓冲区到 写入端 连接, 让头 和有效载荷 当成一个单元发送.



func NewClientWithCodec
```golang
func NewClientWithCodec(codec ClientCodec) *Client
```
NewClientWithCodec is like NewClient but uses the specified codec to encode requests and decode responses.
NewClientWithCodec 类似NewClient 但是使用指定的编解码器来编码请求和解码响应.



func (*Client) Call
```golang
func (client *Client) Call(serviceMethod string, args interface{}, reply interface{}) error
```
Call invokes the named function, waits for it to complete, and returns its error status.
Call 调用 命名函数, 等待它完成 并返回它的错误状态


func (*Client) Close
```golang
func (client *Client) Close() error
```



func (*Client) Go
```golang
func (client *Client) Go(serviceMethod string, args interface{}, reply interface{}, done chan *Call) *Call
```
Go invokes the function asynchronously. 
It returns the Call structure representing the invocation. 
The done channel will signal when the call is complete by returning the same Call object. 
If done is nil, Go will allocate a new channel. 
If non-nil, done must be buffered or Go will deliberately crash.
Go 异步调用的函数。
它返回 Call 结构代表调用。
当 调用完成  通过 返回同一个 Call对象,done channel将信号.
如果done 是nil GO 将会 分配一个新的channel.
如果非nil,done  必须缓冲 或 Go 将会故意 崩溃



type ClientCodec
```golang
type ClientCodec interface {
        // WriteRequest must be safe for concurrent use by multiple goroutines.
        //WriteRequest 使用多个goroutines 并发必须是安全的
        WriteRequest(*Request, interface{}) error
        ReadResponseHeader(*Response) error
        ReadResponseBody(interface{}) error

        Close() error
}
```
A ClientCodec implements writing of RPC requests and reading of RPC responses for the client side of an RPC session. 
The client calls WriteRequest to write a request to the connection and calls ReadResponseHeader and ReadResponseBody in pairs to read responses. 
The client calls Close when finished with the connection. 
ReadResponseBody may be called with a nil argument to force the body of the response to be read and then discarded.
ClientCodec实现 对客户端 的RPC  session  写RPC请求 并 读取 RPC 响应  .
客户端调用WriteRequest 来写一个请求到 连接 并 调用ReadResponseHeader 和 ReadResponseBody 对 读取 响应.
当连接结束时客户端调用Close
ReadResponseBody 可能 调用一个nil参数 来强制响应体被读取 然后丢弃


type Request
```golang
type Request struct {
        ServiceMethod string // format: "Service.Method"
        Seq           uint64 // sequence number chosen by client
        // contains filtered or unexported fields
}
```
Request is a header written before every RPC call. 
It is used internally but documented here as an aid to debugging, such as when analyzing network traffic.
Request 是一个 在 每次RPC调用前的头写.
它在内部使用 但是 这里的记录作为一种辅助手段，调试, 例如 当 分析网络流量时。



type Response
```golang
type Response struct {
        ServiceMethod string // echoes that of the Request
        Seq           uint64 // echoes that of the request
        Error         string // error, if any.
        // contains filtered or unexported fields
}
```
Response is a header written before every RPC return. 
It is used internally but documented here as an aid to debugging, such as when analyzing network traffic.
Response 是一个 在 每次RPC返回前的头写.
它在内部使用 但是 这里的记录作为一种辅助手段，调试, 例如 当 分析网络流量时。


type Server
```golang
type Server struct {
        // contains filtered or unexported fields
}
```
Server represents an RPC Server.
Server 表示一个 RPC Server



func NewServer
```golang
func NewServer() *Server
```
NewServer returns a new Server.
NewServer 返回一个新的Server



func (*Server) Accept
```golang
func (server *Server) Accept(lis net.Listener)
```
Accept accepts connections on the listener and serves requests for each incoming connection. 
Accept blocks; the caller typically invokes it in a go statement.
Accept接受 在监听上的连接  并并提供对每个传入的连接请求。
Accept 块; 调用者通常 在go声明中调用它.



func (*Server) HandleHTTP
```golang
func (server *Server) HandleHTTP(rpcPath, debugPath string)
```
HandleHTTP registers an HTTP handler for RPC messages on rpcPath, and a debugging handler on debugPath. 
It is still necessary to invoke http.Serve(), typically in a go statement.
HandleHTTP 为在rpcPath上的RPC信息 注册一个HTTP处理器, 和 在debugPath 上的调试处理器.
它一直需要调用 http.Serve(), 通常在go声明中.


func (*Server) Register
```golang
func (server *Server) Register(rcvr interface{}) error
```
Register publishes in the server the set of methods of the receiver value that satisfy the following conditions:
Register 公开在服务端的 一组接收器的方法值, 满足以下条件:

```golang
- exported method
- two arguments, both of exported type
- the second argument is a pointer
- one return value, of type error

- 导出方法
- 两个参数,都是可导出类型
- 第二个参数是个指针
- 一个返回值类型的错误，
```
It returns an error if the receiver is not an exported type or has no suitable methods. 
It also logs the error using package log. 
The client accesses each method using a string of the form "Type.Method", where Type is the receiver's concrete type.
如果接受器没有 可导出类型或没有适配的方法,它返回一个错误.
它也 使用log包记录错误.
客户端 使用"Type.Method" 字符串形式 访问每个方法 , Type 是 接收器的具体类型。



func (*Server) RegisterName
```golang
func (server *Server) RegisterName(name string, rcvr interface{}) error
```
RegisterName is like Register but uses the provided name for the type instead of the receiver's concrete type.
RegisterName 类似 Register 但是使用 提供的 name  把 该类型 替换 成 接收器的具体类型。


func (*Server) ServeCodec
```golang
func (server *Server) ServeCodec(codec ServerCodec)
```
ServeCodec is like ServeConn but uses the specified codec to decode requests and encode responses.
ServeCodec 类似ServeConn 但是 使用指定的编解码器 来 解码请求和编码响应.


func (*Server) ServeConn
```golang
func (server *Server) ServeConn(conn io.ReadWriteCloser)
```
ServeConn runs the server on a single connection. 
ServeConn blocks, serving the connection until the client hangs up. 
The caller typically invokes ServeConn in a go statement. 
ServeConn uses the gob wire format (see package gob) on the connection. 
To use an alternate codec, use ServeCodec.
ServeConn 在单个连接上 执行 DefaultServer.
ServeConn 块, 服务连接直到连接 断了.
调用者通常在go声明中 调用 ServeConn.
ServeConn 在连接中 使用 gob wire 格式(查看gob包).
要使用替代的编解码器,使用ServeCodec.


func (*Server) ServeHTTP
```golang
func (server *Server) ServeHTTP(w http.ResponseWriter, req *http.Request)
```
ServeHTTP implements an http.Handler that answers RPC requests.
ServeHTTP实现 一个 http.Handler 应答RPC请求.



func (*Server) ServeRequest
```golang
func (server *Server) ServeRequest(codec ServerCodec) error
```
ServeRequest is like ServeCodec but synchronously serves a single request. 
It does not close the codec upon completion.
ServeRequest 类似 ServeCodec 但是同步提供一个单一的请求。
编解码器完成后它不会关闭.


type ServerCodec

type ServerCodec interface {
        ReadRequestHeader(*Request) error
        ReadRequestBody(interface{}) error
        // WriteResponse must be safe for concurrent use by multiple goroutines.
        WriteResponse(*Response, interface{}) error

        Close() error
}
A ServerCodec implements reading of RPC requests and writing of RPC responses for the server side of an RPC session. 
The server calls ReadRequestHeader and ReadRequestBody in pairs to read requests from the connection, and it calls WriteResponse to write a response back. 
The server calls Close when finished with the connection.
ReadRequestBody may be called with a nil argument to force the body of the request to be read and discarded.
ServerCodec实现 对服务端 的RPC  session  写RPC请求 并 读取 RPC 响应  .
服务端调用 ReadRequestHeader 和 ReadRequestBody 对 来从连接读取请求, 并调用WriteResponse 写一个 返回 响应.
当连接结束时客户端调用Close
ReadRequestBody 可能 调用一个nil参数 来强制请求被读取 然后丢弃


type ServerError
```golang
type ServerError string
```
ServerError represents an error that has been returned from the remote side of the RPC connection.
ServerError 表示 从远程RPC连接中返回的一个错误, 


func (ServerError) Error
```golang
func (e ServerError) Error() string
```















