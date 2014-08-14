Package jsonrpc

Overview ▾

Package jsonrpc implements a JSON-RPC ClientCodec and ServerCodec for the rpc package.
jsonrpc包实现rpc包的 JSON-RPC ClientCodec 和 ServerCodec


func Dial
```golang
func Dial(network, address string) (*rpc.Client, error)
```
Dial connects to a JSON-RPC server at the specified network address.
Dial在指定的  network address 连接到 一个 JSON-RPC 服务器


func NewClient
```golang
func NewClient(conn io.ReadWriteCloser) *rpc.Client
```
NewClient returns a new rpc.Client to handle requests to the set of services at the other end of the connection.
NewClient 返回一个新的 rpc.Client 处理 请求 以连接到另一端的一组服务。



func NewClientCodec
```golang
func NewClientCodec(conn io.ReadWriteCloser) rpc.ClientCodec
```
NewClientCodec returns a new rpc.ClientCodec using JSON-RPC on conn.
NewClientCodec 在conn 上 使用JSON-RPC 返回一个新的 rpc.ClientCodec


func NewServerCodec
```golang
func NewServerCodec(conn io.ReadWriteCloser) rpc.ServerCodec
```
NewServerCodec returns a new rpc.ServerCodec using JSON-RPC on conn.
NewServerCodec 用 conn 上的 JSON-RPC 返回一个新的 rpc.ServerCodec


func ServeConn
```golang
func ServeConn(conn io.ReadWriteCloser)
```
ServeConn runs the JSON-RPC server on a single connection. 
ServeConn blocks, serving the connection until the client hangs up. 
The caller typically invokes ServeConn in a go statement.
ServeConn 在单个 连接上执行 JSON-RPC服务器.
ServeConn块, 服务  连接直到客户端断开.
调用者 通常在go 声明中调用ServeConn. 


