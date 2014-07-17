Package fcgi

Overview ▾

Package fcgi implements the FastCGI protocol. 
Currently only the responder role is supported. 
The protocol is defined at http://www.fastcgi.com/drupal/node/6?q=node/22
fcgi包 实现 FastCGI 协议.
目前只支持响应者的角色。
协议定义在  http://www.fastcgi.com/drupal/node/6?q=node/22



func Serve
```golang
func Serve(l net.Listener, handler http.Handler) error
```
Serve accepts incoming FastCGI connections on the listener l, creating a new goroutine for each. 
The goroutine reads requests and then calls handler to reply to them. 
If l is nil, Serve accepts connections from os.Stdin. 
If handler is nil, http.DefaultServeMux is used.

Serve 接收在 监听 l  里的  传入的 FastCGI连接, 为每个 创建新的goroutine.
goroutine读取请求 然后 调用处理器回复他们.
如果l 是nil,Serve 从 os.Stdin 接收连接.
如果 handler 是nil, 使用http.DefaultServeMux


