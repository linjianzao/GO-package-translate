Package runtime

Overview ▾

Package runtime contains operations that interact with Go's runtime system, such as functions to control goroutines. 
It also includes the low-level type information used by the reflect package; see reflect's documentation for the programmable interface to the run-time type system.
runtime包 包含操作 GO的 运行系统交互，例如函数控制goroutines。
它也包含用在reflect包的低级类型信息;查看reflect的文档 可编程接口的运行时类型系统。


Environment Variables


The following environment variables ($name or %name%, depending on the host operating system) control the run-time behavior of Go programs. 
The meanings and use may change from release to release.

The GOGC variable sets the initial garbage collection target percentage. 
A collection is triggered when the ratio of freshly allocated data to live data remaining after the previous collection reaches this percentage. 
The default is GOGC=100. Setting GOGC=off disables the garbage collector entirely. 
The runtime/debug package's SetGCPercent function allows changing this percentage at run time. See http://golang.org/pkg/runtime/debug/#SetGCPercent.

The GODEBUG variable controls debug output from the runtime. GODEBUG value is a comma-separated list of name=val pairs. Supported names are:

下面的环境变量控制GO程序运行时的习惯（$name or %name%, 依赖主机的操作系统）。
那意味着 使用可能修改 从发布到解除。

GOGC变量设置最初的垃圾收集目标百分比。
在以前的收集达到这个百分比后  当新分配数据到达剩余数据的比例，垃圾回收被触发。
默认的 GOGC=100。 设置GOGC=off 完全禁用垃圾收集。
runtime/debug 包 的SetGCPercent 函数 允许在运行的时候 改变这个百分比。查看  http://golang.org/pkg/runtime/debug/#SetGCPercent.

GODEBUG变量 从运行时 控制 debug 输出。GODEBUG值是逗号分隔的name=val 对列表。支持的名字是：

```golang
allocfreetrace: setting allocfreetrace=1 causes every allocation to be
profiled and a stack trace printed on each object's allocation and free.

efence: setting efence=1 causes the allocator to run in a mode
where each object is allocated on a unique page and addresses are
never recycled.

gctrace: setting gctrace=1 causes the garbage collector to emit a single line to standard
error at each collection, summarizing the amount of memory collected and the
length of the pause. Setting gctrace=2 emits the same summary but also
repeats each collection.

gcdead: setting gcdead=1 causes the garbage collector to clobber all stack slots
that it thinks are dead.

scheddetail: setting schedtrace=X and scheddetail=1 causes the scheduler to emit
detailed multiline info every X milliseconds, describing state of the scheduler,
processors, threads and goroutines.

schedtrace: setting schedtrace=X causes the scheduler to emit a single line to standard
error every X milliseconds, summarizing the scheduler state.

allocfreetrace: 设置allocfreetrace=1  会使每个分配分析和 一个堆栈跟踪打印每个对象的分配和释放。

efence:设置efence=1 使分配器在一个模式下运行 的每个对象分配一个独特的网页和地址 从不会被回收。

gctrace:设置gctrace=1 使垃圾收集器在每个回收 发送一个单行的标准错误。




```

The GOMAXPROCS variable limits the number of operating system threads that can execute user-level Go code simultaneously. There is no limit to the number of threads that can be blocked in system calls on behalf of Go code; those do not count against the GOMAXPROCS limit. This package's GOMAXPROCS function queries and changes the limit.

The GOTRACEBACK variable controls the amount of output generated when a Go program fails due to an unrecovered panic or an unexpected runtime condition. By default, a failure prints a stack trace for every extant goroutine, eliding functions internal to the run-time system, and then exits with exit code 2. If GOTRACEBACK=0, the per-goroutine stack traces are omitted entirely. If GOTRACEBACK=1, the default behavior is used. If GOTRACEBACK=2, the per-goroutine stack traces include run-time functions. If GOTRACEBACK=crash, the per-goroutine stack traces include run-time functions, and if possible the program crashes in an operating-specific manner instead of exiting. For example, on Unix systems, the program raises SIGABRT to trigger a core dump.

The GOARCH, GOOS, GOPATH, and GOROOT environment variables complete the set of Go environment variables. They influence the building of Go programs (see http://golang.org/cmd/go and http://golang.org/pkg/go/build). GOARCH, GOOS, and GOROOT are recorded at compile time and made available by constants or functions in this package, but they do not influence the execution of the run-time system.