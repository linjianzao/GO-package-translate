Package debug


Overview ▾

Package debug contains facilities for programs to debug themselves while they are running.
debug包含 程序运行时 的debug。


func FreeOSMemory
```golang
func FreeOSMemory()
```
FreeOSMemory forces a garbage collection followed by an attempt to return as much memory to the operating system as possible. 
(Even if this is not called, the runtime gradually returns memory to the operating system in a background task.)
FreeOSMemory 强制垃圾回收 随后尝试尽可能多的内存返回给操作系统。
（即使如果这没有调用，运行时在后台逐步返回内存给操作系统）


func PrintStack
```golang
func PrintStack()
```
PrintStack prints to standard error the stack trace returned by Stack.
PrintStack 打印通过Stack返回堆栈跟踪的标准错误。


func ReadGCStats
```golang
func ReadGCStats(stats *GCStats)
```
ReadGCStats reads statistics about garbage collection into stats. The number of entries in the pause history is system-dependent; 
stats.Pause slice will be reused if large enough, reallocated otherwise. ReadGCStats may use the full capacity of the stats.Pause slice. 
If stats.PauseQuantiles is non-empty, ReadGCStats fills it with quantiles summarizing the distribution of pause time. 
For example, if len(stats.PauseQuantiles) is 5, it will be filled with the minimum, 25%, 50%, 75%, and maximum pause times.
ReadGCStats 读取和垃圾回收有关的统计到stats。在暂停历史项的数量取决于系统;如果够大stats.Pause slice 将会重复使用，否则真正的回收。
ReadGCStats也可能使用stats.Pause slice的全部容量.如果stats.PauseQuantiles是非空,ReadGCStats 同位数总结了填充它的暂停次数的分布。
例如 如果len(stats.PauseQuantiles) 是5, 它将会以最低,25%, 50%, 75%,和最高  暂停次数 填充.



func SetGCPercent
```golang
func SetGCPercent(percent int) int
```
SetGCPercent sets the garbage collection target percentage: 
	a collection is triggered when the ratio of freshly allocated data to live data remaining after the previous collection reaches this percentage. 
	SetGCPercent returns the previous setting. The initial setting is the value of the GOGC environment variable at startup, or 100 if the variable is not set. 
	A negative percentage disables garbage collection.
SetGCPercent 设置垃圾回收目标的百分比:
	回收被触发时新分配的数据的比例，以住先前采集后达到此百分比的剩余数据。
	SetGCPercent返回前面的设置.开始时的初始化设置是GOGC环境变量的值, 或 如果变量没设置  是100.
	负百分比禁用垃圾回收.



func SetMaxStack
```golang
func SetMaxStack(bytes int) int
```
SetMaxStack sets the maximum amount of memory that can be used by a single goroutine stack. 
If any goroutine exceeds this limit while growing its stack, the program crashes. SetMaxStack returns the previous setting. 
The initial setting is 1 GB on 64-bit systems, 250 MB on 32-bit systems.

SetMaxStack is useful mainly for limiting the damage done by goroutines that enter an infinite recursion. It only limits future stack growth.
SetMaxStack 设置单个goroutine 堆栈可用的最大内存数.
如果任何 goroutine 增长它的堆栈到这个限制,程序会崩溃.SetMaxStack 返回之前的设置.
在64位系统上初始化设置是1GB,在32位系统上是250MB

SetMaxStack 主要用于帮助限制由进入无限递归Go例程所造成的损害.它只限制未来的堆栈增长。



func SetMaxThreads
```golang
func SetMaxThreads(threads int) int
```
SetMaxThreads sets the maximum number of operating system threads that the Go program can use. 
If it attempts to use more than this many, the program crashes. SetMaxThreads returns the previous setting. The initial setting is 10,000 threads.

The limit controls the number of operating system threads, not the number of goroutines. 
A Go program creates a new thread only when a goroutine is ready to run but all the existing threads are blocked in system calls, cgo calls, or are locked to other goroutines due to use of runtime.LockOSThread.

SetMaxThreads is useful mainly for limiting the damage done by programs that create an unbounded number of threads. 
The idea is to take down the program before it takes down the operating system.

SetMaxThreads 设置GO程序可用的操作系统最大线程数.
如果尝试使用多于这个数量, 程序会崩溃. SetMaxThreads 返回之前的设置.初始设置线程是10000

该限制控制操作系统线程数,而不是goroutines数.
当一个goroutine准备运行但是所有存在的系统中的调用,cgo调用或使用runtime.LockOSThread 锁定其他的goroutines , 线程都阻塞的时候,GO程序只创建一个新的线程,

SetMaxThreads主要用于帮助限制由创建线程的数量无限的程序所造成的损害.
关闭操作系统之前要关闭程序.



func SetPanicOnFault
```golang
func SetPanicOnFault(enabled bool) bool
```
SetPanicOnFault controls the runtime's behavior when a program faults at an unexpected (non-nil) address. 
Such faults are typically caused by bugs such as runtime memory corruption, so the default response is to crash the program. 
Programs working with memory-mapped files or unsafe manipulation of memory may cause faults at non-nil addresses in less dramatic situations; 
SetPanicOnFault allows such programs to request that the runtime trigger only a panic, not a crash. 
SetPanicOnFault applies only to the current goroutine. It returns the previous setting.

SetPanicOnFault 当程序 在一个不可导出的地址(非nil)故障时,控制运行时的行为.
故障通常是运行时内存损坏bug,所以默认影响是程序崩溃.
在更戏剧性的情况,程序工作以内存映射文件或 不安全的操作内存可能引起的故障 在非nil地址
SetPanicOnFault 允许 程序 请求运行时只触发panic, 不是崩溃
SetPanicOnFault 只适用当前goroutine. 它返回之前的设置.


func Stack
```golang
func Stack() []byte
```
Stack returns a formatted stack trace of the goroutine that calls it. 
For each routine, it includes the source line information and PC value, then attempts to discover, for Go functions, the calling function or method and the text of the line containing the invocation.

This function is deprecated. Use package runtime's Stack instead.
Stack 返回一个 那个调用的格式化的堆栈跟踪goroutine.
对于每个事务,它包含源行信息和PC值, 然后尝试覆盖, 对于GO函数,调用函数或方法 和包含调用行的文本.

该函数不推荐.使用runtime的Stack代替.



func WriteHeapDump
```golang
func WriteHeapDump(fd uintptr)
```
WriteHeapDump writes a description of the heap and the objects in it to the given file descriptor. The heap dump format is defined at http://golang.org/s/go13heapdump.
WriteHeapDump 写一个 描述 堆和 对象到给定的文件描述符.


type GCStats
```golang
type GCStats struct {
        LastGC         time.Time       // time of last collection
        NumGC          int64           // number of garbage collections
        PauseTotal     time.Duration   // total pause for all collections
        Pause          []time.Duration // pause history, most recent first
        PauseQuantiles []time.Duration
}
```
GCStats collect information about recent garbage collections.
GCStats收集和最近垃圾回收有关的信息.


