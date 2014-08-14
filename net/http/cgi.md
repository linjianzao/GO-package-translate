Package cgi

Overview ▾

Package cgi implements CGI (Common Gateway Interface) as specified in RFC 3875.

Note that using CGI means starting a new process to handle each request, which is typically less efficient than using a long-running server. 
This package is intended primarily for compatibility with existing systems.
cgi 包 实现 RFC 3875中指定的 CGI （通用网关接口）
注意 使用CGI 意味着 开始一个新的进程 处理每个请求, 通常 效率低于  使用 长时间运行的服务器。
这个包的主要是 兼容 存在的系统.



func Request
```golang
func Request() (*http.Request, error)
```
Request returns the HTTP request as represented in the current environment. 
This assumes the current program is being run by a web server in a CGI environment. 
The returned Request's Body is populated, if applicable.
Request 返回当前环境中的 HTTP 请求.
这假定 当前程序 是在 CGI环境中 运行的 web服务器.
如果适用,返回 Request的 Body 填充 .



func RequestFromMap
```golang
func RequestFromMap(params map[string]string) (*http.Request, error)
```
RequestFromMap creates an http.Request from CGI variables. 
The returned Request's Body field is not populated.
RequestFromMap 从CGI 变量中 创建一个http.Request
返回的Request 的Body 未填充字段. 



func Serve
```golang
func Serve(handler http.Handler) error
```
Serve executes the provided Handler on the currently active CGI request, if any. 
If there's no current CGI environment an error is returned. The provided handler may be nil to use http.DefaultServeMux.
Serve 执行 当前 活动的 CGI 请求 提供的 Handler,如果有的话.
如果有 非当前CGI 环境  返回一个错误. 提供 handler 可能 是 nil ,使用 http.DefaultServeMux.



type Handler
```golang
type Handler struct {
        Path string // path to the CGI executable
        				  //CGI 可执行文件的路径
        
        
        Root string // root URI prefix of handler or empty for "/"
							// 根 URI 程序程序的前缀 或  空的 是 "/"


        // Dir specifies the CGI executable's working directory.
        // If Dir is empty, the base directory of Path is used.
        // If Path has no base directory, the current working
        // directory is used.
        //Dir指定 CGI可执行文件的 工作目录.
          //如果DIR是空的,使用 Path 的 基本目录.
          //如果Path 没有基本目录,使用当前的工作目录.

        Dir string



        Env        []string    // extra environment variables to set, if any, as "key=value"
        								 //设置额外的环境变量, 如果有, 类似 "key=value"
        
        InheritEnv []string   // environment variables to inherit from host, as "key"
        								//继承主机的 环境变量, 例如 "key"
        								
        Logger     *log.Logger // optional log for errors or nil to use log.Print
        								 // 可选的使用 log.Print 记录错误 或 nil
        								
        Args       []string    // optional arguments to pass to child process
										 //可选的  传递到子进程参数

        // PathLocationHandler specifies the root http Handler that
        // should handle internal redirects when the CGI process
        // returns a Location header value starting with a "/", as
        // specified in RFC 3875 § 6.3.2. This will likely be
        // http.DefaultServeMux.
        //
        // If nil, a CGI response with a local URI path is instead sent
        // back to the client and not redirected internally.
        //PathLocationHandler 指定根 http Handler  应该 处理内部的 重定向, 当 CGI 进程以  "/" 返回一个Location 头的值
           //这将可能是http.DefaultServeMux.
        PathLocationHandler http.Handler
}
```
Handler runs an executable in a subprocess with a CGI environment.
Handler 在子 CGI环境里运行一个可执行文件



func (*Handler) ServeHTTP
```golang
func (h *Handler) ServeHTTP(rw http.ResponseWriter, req *http.Request)
```