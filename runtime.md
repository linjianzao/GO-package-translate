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

gcdead:设置gcdead=1 使垃圾收集器 认为所有的堆栈已经死了。

scheddetail:  设置schedtrace=X 和 scheddetail=1 使 每隔X毫秒调度发出详细的多行信息，描述所有的调度，处理器，线程和Go例程 的状态，

schedtrace: 设置schedtrace=X 使调度器每隔X毫秒发送单行到标准错误，总结调度状态。

```

The GOMAXPROCS variable limits the number of operating system threads that can execute user-level Go code simultaneously. 
There is no limit to the number of threads that can be blocked in system calls on behalf of Go code; those do not count against the GOMAXPROCS limit. 
This package's GOMAXPROCS function queries and changes the limit.

The GOTRACEBACK variable controls the amount of output generated when a Go program fails due to an unrecovered panic or an unexpected runtime condition. 
By default, a failure prints a stack trace for every extant goroutine, eliding functions internal to the run-time system, and then exits with exit code 2. 
If GOTRACEBACK=0, the per-goroutine stack traces are omitted entirely. 
If GOTRACEBACK=1, the default behavior is used. 
If GOTRACEBACK=2, the per-goroutine stack traces include run-time functions. 
If GOTRACEBACK=crash, the per-goroutine stack traces include run-time functions, and if possible the program crashes in an operating-specific manner instead of exiting. 
For example, on Unix systems, the program raises SIGABRT to trigger a core dump.

The GOARCH, GOOS, GOPATH, and GOROOT environment variables complete the set of Go environment variables. 
They influence the building of Go programs (see http://golang.org/cmd/go and http://golang.org/pkg/go/build). 
GOARCH, GOOS, and GOROOT are recorded at compile time and made available by constants or functions in this package, but they do not influence the execution of the run-time system.

GOMAXPROCS变量限制  操作系统可以同时执行用户级的Go代码线程数量。 
它没有限制 阻塞 操作系统线程调用的GO代码数量;这些不计算在GOMAXPROCS限制内。
该包的GOMAXPROCS 函数 查询和修改限制。

GOTRACEBACK变量控制  当一个GO程序由于未收回的panic和意想不到的运行条件失败时  输出的产生量。
默认情况下，故障打印堆栈跟踪每一个现存的goroutine，eliding内部功能的运行时系统，并用 退出码2  退出。
如果GOTRACEBACK=0, 每个goroutine 堆栈跟踪完全忽略.
如果GOTRACEBACK=1,使用默认的特性。
如果GOTRACEBACK=2,每个goroutine堆栈跟踪包含运行时的函数。
如果GOTRACEBACK=crash，每个goroutine堆栈跟踪包含运行时的函数，并且 如果程序在 特定的操作 下 可能会崩溃 用 退出 代替。
例如，在Unix系统上，程序唤起SIGABRT来引发核心转储。

GOARCH, GOOS, GOPATH, 和 GOROOT 环境变量 是完整的 Go环境变量集。
他们影响GO程序的构建（查看 http://golang.org/cmd/go and http://golang.org/pkg/go/build）。
GOARCH, GOOS, 和 GOROOT 记录这个包里 完整的时间和 提供 常数或函数， 但是他们不会影响 运行时系统的 运行。


Constants
```golang
const Compiler = "gc"
```
Compiler is the name of the compiler toolchain that built the running binary. Known toolchains are:
Compiler 是建成运行的二进制文件的编译器工具链的名字。已知的工具链是：

```golang
gc      The 5g/6g/8g compiler suite at code.google.com/p/go.
gccgo   The gccgo front end, part of the GCC compiler suite.
```

```golang
const GOARCH string = theGoarch
```
GOARCH is the running program's architecture target: 386, amd64, or arm.
GOARCH 是正在运行的程序的架构: 386, amd64, or arm.

```golang
const GOOS string = theGoos
```
GOOS is the running program's operating system target: one of darwin, freebsd, linux, and so on.
GOOS 是正在运行的程序操作系统：darwin中的一种，freebsd, linux, 等等。


Variables
```golang
var MemProfileRate int = 512 * 1024
```
MemProfileRate controls the fraction of memory allocations that are recorded and reported in the memory profile. 
The profiler aims to sample an average of one allocation per MemProfileRate bytes allocated.

To include every allocated block in the profile, set MemProfileRate to 1. To turn off profiling entirely, set MemProfileRate to 0.

The tools that process the memory profiles assume that the profile rate is constant across the lifetime of the program and equal to the current value. 
Programs that change the memory profiling rate should do so just once, as early as possible in the execution of the program (for example, at the beginning of main).

MemProfileRate控制 内存分配的分数 那记录和报告 内存里的分析。
分析器目的是平均分配MemProfileRate字节。

要包含在配置文件中每一个分配的块，设置MemProfileRate为1.  MemProfileRate为0 来关闭配置。

该工具处理内存配置文件 假定 配置速率在整个程序的生命周期恒定并等于当前的值。
程序修改内存配置 应该只一次， 在程序的执行时尽可能早的修改（例如，在main开头）。



func BlockProfile
```golang
func BlockProfile(p []BlockProfileRecord) (n int, ok bool)
```
BlockProfile returns n, the number of records in the current blocking profile. 
If len(p) >= n, BlockProfile copies the profile into p and returns n, true. 
If len(p) < n, BlockProfile does not change p and returns n, false.

Most clients should use the runtime/pprof package or the testing package's -test.blockprofile flag instead of calling BlockProfile directly.
BlockProfile 返回n，当前阻塞信息的记录数
如果 len(p) >= n,BlockProfile复制配置到p 并返回n,true。
如果len(p) < n,BlockProfile 不会修改p 并且返回 n,false.



func Breakpoint
```golang
func Breakpoint()
```
Breakpoint executes a breakpoint trap.
Breakpoint 执行一个断点。



func CPUProfile
```golang
func CPUProfile() []byte
```
CPUProfile returns the next chunk of binary CPU profiling stack trace data, blocking until data is available. 
If profiling is turned off and all the profile data accumulated while it was on has been returned, CPUProfile returns nil. 
The caller must save the returned data before calling CPUProfile again.

Most clients should use the runtime/pprof package or the testing package's -test.cpuprofile flag instead of calling CPUProfile directly.

CPUProfile 返回下一个 二进制 CPU 分析堆栈追踪数据 的块，阻塞直到数据可用。
如果 当它已经返回 分析 关闭 和所有的信息数据积累，CPUProfile返回nil。
调用者在再一次调用CPUProfile之前 必须保存返回的数据。

大部分的客户端应该使用 runtime/pprof包或 testing包的-test.cpuprofile 标签代替直接调用 CPUProfile



func Caller
```golang
func Caller(skip int) (pc uintptr, file string, line int, ok bool)
```
Caller reports file and line number information about function invocations on the calling goroutine's stack. 
The argument skip is the number of stack frames to ascend, with 0 identifying the caller of Caller. 
(For historical reasons the meaning of skip differs between Caller and Callers.) 
The return values report the program counter, file name, and line number within the file of the corresponding call. 
The boolean ok is false if it was not possible to recover the information.
Caller 报告 在调用goroutine的堆栈 上 和函数调用有关的  file和line数 信息。
参数 skip 是 堆栈帧 提升的数量， 用0 来确定Caller的调用者。（历史原因  skip 在 Caller和 Callers 之间的含义是不一样的）
返回值报告 和调用 file对应的 程序计数器， 文件名称，和行数
如果它不可能复原信息，布尔值 ok 是 false。



func Callers
```golang
func Callers(skip int, pc []uintptr) int
```
Callers fills the slice pc with the program counters of function invocations on the calling goroutine's stack. 
The argument skip is the number of stack frames to skip before recording in pc, with 0 identifying the frame for Callers itself and 1 identifying the caller of Callers. 
It returns the number of entries written to pc.
Callers 在goroutine的堆栈上 用 程序计数器函数的调用 填充 slice pc。
参数skip  是记录在pc之前 堆栈 帧 到skip的数量， 0表示 Callers本身的 帧 ，1表示 Callers的调用。
返回的全部数量写到pc。


func GC
```golang
func GC()
```
GC runs a garbage collection.
GC执行一个垃圾回收


func GOMAXPROCS
```golang
func GOMAXPROCS(n int) int
```
GOMAXPROCS sets the maximum number of CPUs that can be executing simultaneously and returns the previous setting. 
If n < 1, it does not change the current setting. The number of logical CPUs on the local machine can be queried with NumCPU. 
This call will go away when the scheduler improves.
GOMAXPROCS 设置 可同时执行的CPU最大数量 并返回之前的设置。
如果n<1 , 它不会修改当前的设置。在本地的机器上 逻辑CPU的数量 可以通过NumCPU 查询。
当调度提高时 这个调用将消失。



func GOROOT
```golang
func GOROOT() string
```
GOROOT returns the root of the Go tree. It uses the GOROOT environment variable, if set, or else the root used during the Go build.
GOROOT 返回GO树的 根。如果有设置GOROOT，或在GO build 期间 有使用root，它使用 GOROOT环境变量。



func Goexit
```golang
func Goexit()
```
Goexit terminates the goroutine that calls it. No other goroutine is affected. Goexit runs all deferred calls before terminating the goroutine.

Calling Goexit from the main goroutine terminates that goroutine without func main returning. 
Since func main has not returned, the program continues execution of other goroutines. If all other goroutines exit, the program crashes.

Goexit 中断 调用goroutine。其他的goroutine不会受影响。Goexit在中断goroutine之前执行所有延迟调用。

调用Goexit 从主goroutine 中断 那 没有main函数 返回。
main函数一没有返回，程序继续执行其他的goroutines。如果所有其他的goroutines 退出， 程序 崩溃。


func GoroutineProfile
```golang
func GoroutineProfile(p []StackRecord) (n int, ok bool)
```
GoroutineProfile returns n, the number of records in the active goroutine stack profile. 
If len(p) >= n, GoroutineProfile copies the profile into p and returns n, true. 
If len(p) < n, GoroutineProfile does not change p and returns n, false.

Most clients should use the runtime/pprof package instead of calling GoroutineProfile directly.

GoroutineProfile返回n，在活动的 goroutine堆栈的信息记录数。
如果len(p) >= n,GoroutineProfile 复制配置到p 并返回n,true。
如果len(p) < n, GoroutineProfile不会修改p 并返回 n,false。

大部分客户端应该使用runtime/pprof 包 替换 直接调用GoroutineProfile。



func Gosched
```golang
func Gosched()
```
Gosched yields the processor, allowing other goroutines to run. It does not suspend the current goroutine, so execution resumes automatically.
Gosched产生的处理器，允许其他的goroutines执行。它不会挂起当前的goroutine， 所以 执行自动恢复。



func LockOSThread
```golang
func LockOSThread()
```
LockOSThread wires the calling goroutine to its current operating system thread. 
Until the calling goroutine exits or calls UnlockOSThread, it will always execute in that thread, and no other goroutine can.
LockOSThread 调用 goroutine 到它当前的操作系统线程。
直到调用goroutine 退出 或调用UnlockOSThread，它总是在该线程执行，并没有其他的goroutine可以。



func MemProfile
```golang
func MemProfile(p []MemProfileRecord, inuseZero bool) (n int, ok bool)
```
MemProfile returns n, the number of records in the current memory profile. 
If len(p) >= n, MemProfile copies the profile into p and returns n, true. 
If len(p) < n, MemProfile does not change p and returns n, false.

If inuseZero is true, the profile includes allocation records where r.AllocBytes > 0 but r.AllocBytes == r.FreeBytes. 
These are sites where memory was allocated, but it has all been released back to the runtime.

Most clients should use the runtime/pprof package or the testing package's -test.memprofile flag instead of calling MemProfile directly.

MemProfile 返回n，在当前内存中信息的记录数。
如果 len(p) >= n，MemProfile 复制信息到p并返回n，true。
如果len(p) < n，MemProfile 不会修改p 并返回 n, false.

如果inuseZero是true，信息包括分配记录 其中 r.AllocBytes > 0 但是 r.AllocBytes == r.FreeBytes. 
这些是分配内存的方面，但是它这已全部释放回运行。

大部分客户端 应该使用  runtime/pprof包 或 testing包的 -test.memprofile 标签 代替 直接调用MemProfile



func NumCPU
```golang
func NumCPU() int
```
NumCPU returns the number of logical CPUs on the local machine.
NumCPU 返回 在本地机器上的逻辑CPU 数量。



func NumCgoCall
```golang
func NumCgoCall() int64
```
NumCgoCall returns the number of cgo calls made by the current process.
NumCgoCall 返回 当前进程 调用的 cgo数量



func NumGoroutine
```golang
func NumGoroutine() int
```
NumGoroutine returns the number of goroutines that currently exist.
NumGoroutine返回当前存在的goroutines 数。



func ReadMemStats
```golang
func ReadMemStats(m *MemStats)
```
ReadMemStats populates m with memory allocator statistics.
ReadMemStats 以内存分配器的统计数据填充m。



func SetBlockProfileRate
```golang
func SetBlockProfileRate(rate int)
```
SetBlockProfileRate controls the fraction of goroutine blocking events that are reported in the blocking profile. 
The profiler aims to sample an average of one blocking event per rate nanoseconds spent blocked.

To include every blocking event in the profile, pass rate = 1. To turn off profiling entirely, pass rate <= 0.

SetBlockProfileRate控制 报告的阻塞信息中的goroutine阻塞事件的分数。
分析器的目的 是 样品平均每秒发生的阻塞事件。

通过设置rate = 1， 让信息里包含每个阻塞事件。通过 rate <= 0， 完全关闭分析。



func SetCPUProfileRate
```golang
func SetCPUProfileRate(hz int)
```
SetCPUProfileRate sets the CPU profiling rate to hz samples per second. 
If hz <= 0, SetCPUProfileRate turns off profiling. 
If the profiler is on, the rate cannot be changed without first turning it off.

Most clients should use the runtime/pprof package or the testing package's -test.cpuprofile flag instead of calling SetCPUProfileRate directly.

SetCPUProfileRate 设置CPU 每秒采样的HZ。
如果 hz <= 0, SetCPUProfileRate 关闭 分析

大部分客户端应该使用 runtime/pprof  包 或 testing 包的 -test.cpuprofile 标示来代替直接调用SetCPUProfileRate。


func SetFinalizer
```golang
func SetFinalizer(x, f interface{})
```
SetFinalizer sets the finalizer associated with x to f. 
When the garbage collector finds an unreachable block with an associated finalizer, it clears the association and runs f(x) in a separate goroutine. 
This makes x reachable again, but now without an associated finalizer. 
Assuming that SetFinalizer is not called again, the next time the garbage collector sees that x is unreachable, it will free x.

SetFinalizer(x, nil) clears any finalizer associated with x.

The argument x must be a pointer to an object allocated by calling new or by taking the address of a composite literal. 
The argument f must be a function that takes a single argument to which x's type can be assigned, and can have arbitrary ignored return values. 
If either of these is not true, SetFinalizer aborts the program.

Finalizers are run in dependency order: 
	if A points at B, both have finalizers, and they are otherwise unreachable, only the finalizer for A runs; once A is freed, the finalizer for B can run. 
	If a cyclic structure includes a block with a finalizer, that cycle is not guaranteed to be garbage collected and the finalizer is not guaranteed to run, because there is no ordering that respects the dependencies.

The finalizer for x is scheduled to run at some arbitrary time after x becomes unreachable. 
There is no guarantee that finalizers will run before a program exits, so typically they are useful only for releasing non-memory resources associated with an object during a long-running program. 
For example, an os.File object could use a finalizer to close the associated operating system file descriptor when a program discards an os.File without calling Close, 
	but it would be a mistake to depend on a finalizer to flush an in-memory I/O buffer such as a bufio.Writer, because the buffer would not be flushed at program exit.

It is not guaranteed that a finalizer will run if the size of *x is zero bytes.

A single goroutine runs all finalizers for a program, sequentially. If a finalizer must run for a long time, it should do so by starting a new goroutine.





func Stack

func Stack(buf []byte, all bool) int
Stack formats a stack trace of the calling goroutine into buf and returns the number of bytes written to buf. 
If all is true, Stack formats stack traces of all other goroutines into buf after the trace for the current goroutine.
Stack格式化一个堆栈跟踪 调用 goroutine 成 buf 并 返回写入buf的字节数。
如果all是true,在当前goroutine 跟踪后 Stack 格式化 所有其他goroutines堆栈跟踪 到 buf。


func ThreadCreateProfile
```golang
func ThreadCreateProfile(p []StackRecord) (n int, ok bool)
```
ThreadCreateProfile returns n, the number of records in the thread creation profile. 
If len(p) >= n, ThreadCreateProfile copies the profile into p and returns n, true. 
If len(p) < n, ThreadCreateProfile does not change p and returns n, false.

Most clients should use the runtime/pprof package instead of calling ThreadCreateProfile directly.

ThreadCreateProfile 返回n， 记录在线程里创建配置文件数。
如果len(p) >= n,ThreadCreateProfile 复制信息到p 并返回 n, true。
如果 len(p) < n,ThreadCreateProfile不会修改p并返回 n, false。



func UnlockOSThread
```golang
func UnlockOSThread()
```
UnlockOSThread unwires the calling goroutine from its fixed operating system thread. If the calling goroutine has not called LockOSThread, UnlockOSThread is a no-op.
UnlockOSThread 从固定的操作系统线程调用的goroutine。 如果调用的goroutine没有调用 LockOSThread，UnlockOSThread 是一个空操作。



func Version
```golang
func Version() string
```
Version returns the Go tree's version string. It is either the commit hash and date at the time of the build or, when possible, a release tag like "go1.3".
Version 返回 GO 树的版本字符串。 可能的时候，它要么 提交hash 和构建时候的日期， 一个发布标签 类似 "go1.3"。



type BlockProfileRecord
```golang
type BlockProfileRecord struct {
        Count  int64
        Cycles int64
        StackRecord
}
```
BlockProfileRecord describes blocking events originated at a particular call sequence (stack trace).
BlockProfileRecord 描述阻塞事件起源 特别是调用的顺序（堆栈跟踪）。


type Error
```golang
type Error interface {
        error

        // RuntimeError is a no-op function but
        // serves to distinguish types that are runtime
        // errors from ordinary errors: a type is a
        // runtime error if it has a RuntimeError method.
        RuntimeError()
}
```
The Error interface identifies a run time error.
Error 接口表示一个运行时错误。


type Func
```golang
type Func struct {
        // contains filtered or unexported fields
}
```


func FuncForPC
```golang
func FuncForPC(pc uintptr) *Func
```
FuncForPC returns a *Func describing the function that contains the given program counter address, or else nil.
FuncForPC 返回一个  *Func 描述  包含给定的程序计数器地址 或其他 nil 的函数


func (*Func) Entry
```golang
func (f *Func) Entry() uintptr
```
Entry returns the entry address of the function.
Entry返回函数入口地址


func (*Func) FileLine
```golang
func (f *Func) FileLine(pc uintptr) (file string, line int)
```
FileLine returns the file name and line number of the source code corresponding to the program counter pc. 
The result will not be accurate if pc is not a program counter within f.
FileLine 返回程序计数器pc对应的文件名和行号。
如果pc不是一个f的程序计数器，结果不准确。



func (*Func) Name
```golang
func (f *Func) Name() string
```
Name returns the name of the function.
Name 返回 函数名。


type MemProfileRecord
```golang
type MemProfileRecord struct {
        AllocBytes, FreeBytes     int64       // number of bytes allocated, freed
        AllocObjects, FreeObjects int64       // number of objects allocated, freed
        Stack0                    [32]uintptr // stack trace for this record; ends at first 0 entry
}
```
A MemProfileRecord describes the live objects allocated by a particular call sequence (stack trace).
MemProfileRecord 描述活动对象的分配 特别是调用顺序。


func (*MemProfileRecord) InUseBytes
```golang
func (r *MemProfileRecord) InUseBytes() int64
```
InUseBytes returns the number of bytes in use (AllocBytes - FreeBytes).
InUseBytes 返回 使用的字节数



func (*MemProfileRecord) InUseObjects
```golang
func (r *MemProfileRecord) InUseObjects() int64
```
InUseObjects returns the number of objects in use (AllocObjects - FreeObjects).
InUseObjects 返回使用的对象数。



func (*MemProfileRecord) Stack
```golang
func (r *MemProfileRecord) Stack() []uintptr
```
Stack returns the stack trace associated with the record, a prefix of r.Stack0.
Stack 返回 堆栈跟踪对应的记录，前缀 r.Stack0。



type MemStats
```golang
type MemStats struct {
        // General statistics.
        Alloc      uint64 // bytes allocated and still in use
        TotalAlloc uint64 // bytes allocated (even if freed)
        Sys        uint64 // bytes obtained from system (sum of XxxSys below)
        Lookups    uint64 // number of pointer lookups
        Mallocs    uint64 // number of mallocs
        Frees      uint64 // number of frees

        // Main allocation heap statistics.
        HeapAlloc    uint64 // bytes allocated and still in use
        HeapSys      uint64 // bytes obtained from system
        HeapIdle     uint64 // bytes in idle spans
        HeapInuse    uint64 // bytes in non-idle span
        HeapReleased uint64 // bytes released to the OS
        HeapObjects  uint64 // total number of allocated objects

        // Low-level fixed-size structure allocator statistics.
        //	Inuse is bytes used now.
        //	Sys is bytes obtained from system.
        StackInuse  uint64 // bootstrap stacks
        StackSys    uint64
        MSpanInuse  uint64 // mspan structures
        MSpanSys    uint64
        MCacheInuse uint64 // mcache structures
        MCacheSys   uint64
        BuckHashSys uint64 // profiling bucket hash table
        GCSys       uint64 // GC metadata
        OtherSys    uint64 // other system allocations

        // Garbage collector statistics.
        NextGC       uint64 // next run in HeapAlloc time (bytes)
        LastGC       uint64 // last run in absolute time (ns)
        PauseTotalNs uint64
        PauseNs      [256]uint64 // circular buffer of recent GC pause times, most recent at [(NumGC+255)%256]
        NumGC        uint32
        EnableGC     bool
        DebugGC      bool

        // Per-size allocation statistics.
        // 61 is NumSizeClasses in the C code.
        BySize [61]struct {
                Size    uint32
                Mallocs uint64
                Frees   uint64
        }
}
```
A MemStats records statistics about the memory allocator.
MemStats记录统计信息的内存分配器。


type StackRecord
```golang
type StackRecord struct {
        Stack0 [32]uintptr // stack trace for this record; ends at first 0 entry
}
```
A StackRecord describes a single execution stack.
StackRecord 描述一个 单个执行堆栈。


func (*StackRecord) Stack
```golang
func (r *StackRecord) Stack() []uintptr
```
Stack returns the stack trace associated with the record, a prefix of r.Stack0.
Stack 返回 堆栈跟踪对应的记录，前缀 r.Stack0。



type TypeAssertionError
```golang
type TypeAssertionError struct {
        // contains filtered or unexported fields
}
```
A TypeAssertionError explains a failed type assertion.
TypeAssertionError 解释一个失败的类型声明。



func (*TypeAssertionError) Error
```golang
func (e *TypeAssertionError) Error() string
```



func (*TypeAssertionError) RuntimeError
```golang
func (*TypeAssertionError) RuntimeError()
```




















