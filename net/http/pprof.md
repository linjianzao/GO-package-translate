Package pprof
	
	import "net/http/pprof"
	

Overview ▾

Package pprof serves via its HTTP server runtime profiling data in the format expected by the pprof visualization tool. 
For more information about pprof, see http://code.google.com/p/google-perftools/.

The package is typically only imported for the side effect of registering its HTTP handlers. The handled paths all begin with /debug/pprof/.

To use pprof, link this package into your program:

pprof 包 服务 通过它的HTTP服务器 运行时分析数据 成预期的pprof可视化工具的格式。
查看  http://code.google.com/p/google-perftools/ 了解更多和pprof 有关的信息.

这个包通常只引入到 注册它的HTTP 处理器.所有的处理路径 以/debug/pprof/ 开始

使用 pprof, 连接这个包到你的系统:

```golang
import _ "net/http/pprof"
```

If your application is not already running an http server, you need to start one. 
Add "net/http" and "log" to your imports and the following code to your main function:
如果你的应用  没有 执行 http服务器, 你必须启动一个.
添加"net/http" 和 "log" 到你的引入 和 下面的代码到你的main函数

```golang
go func() {
	log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

Then use the pprof tool to look at the heap profile:
然后使用pprof工具 查看 堆 介绍:
```golang
go tool pprof http://localhost:6060/debug/pprof/heap
```

Or to look at a 30-second CPU profile:
或者查看30秒 内的CPU 介绍:
```golang
go tool pprof http://localhost:6060/debug/pprof/profile
```

Or to look at the goroutine blocking profile:
或者查看 goroutine 块介绍:
```golang
go tool pprof http://localhost:6060/debug/pprof/block
```

To view all available profiles, open http://localhost:6060/debug/pprof/ in your browser.

For a study of the facility in action, visit
在你的浏览器里 打开 http://localhost:6060/debug/pprof/  查看所有的变量介绍

```golang
http://blog.golang.org/2011/06/profiling-go-programs.html
```


func Cmdline
```golang
func Cmdline(w http.ResponseWriter, r *http.Request)
```
Cmdline responds with the running program's command line, with arguments separated by NUL bytes. 
The package initialization registers it as /debug/pprof/cmdline.
Cmdline 响应 执行中的程序的命名行, 用参数 分隔NUL 字节.
该包初始化 注册它 /debug/pprof/cmdline.


func Handler
```golang
func Handler(name string) http.Handler
```
Handler returns an HTTP handler that serves the named profile.
Handler返回一个 HTTP 处理器 服务命名的配置文件。


func Index
```golang
func Index(w http.ResponseWriter, r *http.Request)
```
Index responds with the pprof-formatted profile named by the request. 
For example, "/debug/pprof/heap" serves the "heap" profile. 
Index responds to a request for "/debug/pprof/" with an HTML page listing the available profiles.
Index 用 pprof 格式的 命的配置文件 响应 请求.
Index 用HTML页面  列出 有效的配置, 来响应 一个 "/debug/pprof/" 请求.


func Profile
```golang
func Profile(w http.ResponseWriter, r *http.Request)
```
Profile responds with the pprof-formatted cpu profile. The package initialization registers it as /debug/pprof/profile.
Profile 用 pprof 格式 响应 cpu的配置文件. 该包 初始化 注册它 成/debug/pprof/profile.


func Symbol
```golang
func Symbol(w http.ResponseWriter, r *http.Request)
```
Symbol looks up the program counters listed in the request, responding with a table mapping program counters to function names. 
The package initialization registers it as /debug/pprof/symbol.
Symbol查看请求里的程序计数器列表, 用 一个表映射 程序计数器 成 函数名  响应.
 该包 初始化 注册它 成/debug/pprof/profile.


