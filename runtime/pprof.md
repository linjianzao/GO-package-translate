Package pprof

Overview ▾

Package pprof writes runtime profiling data in the format expected by the pprof visualization tool. 
For more information about pprof, see http://code.google.com/p/google-perftools/.
pprof包 写运行时分析数据 成 pprof可视化工具预期的格式。更多有关pprof的信息查看 http://code.google.com/p/google-perftools/.


func Profiles
```golang
func Profiles() []*Profile
```
Profiles returns a slice of all the known profiles, sorted by name.
Profiles 返回所有已知分析的slice, 根据 名称排序.



func StartCPUProfile
```golang
func StartCPUProfile(w io.Writer) error
```
StartCPUProfile enables CPU profiling for the current process. 
While profiling, the profile will be buffered and written to w. StartCPUProfile returns an error if profiling is already enabled.
StartCPUProfile为当前进程启用CPU分析.
 分析将缓冲并写入w.如果分析已经启用,StartCPUProfile 返回一个错误.



func StopCPUProfile
```golang
func StopCPUProfile()
```
StopCPUProfile stops the current CPU profile, if any. StopCPUProfile only returns after all the writes for the profile have completed.
StopCPUProfile 停止任何当前CPU 分析. StopCPUProfile 只会在所有的分析 完全写完后才会返回.



func WriteHeapProfile
```golang
func WriteHeapProfile(w io.Writer) error
```
WriteHeapProfile is shorthand for Lookup("heap").WriteTo(w, 0). It is preserved for backwards compatibility.
WriteHeapProfile 是Lookup("heap").WriteTo(w, 0)的简写.它保留了向后兼容性。


type Profile
```golang
type Profile struct {
        // contains filtered or unexported fields
}
```
A Profile is a collection of stack traces showing the call sequences that led to instances of a particular event, such as allocation. 
Packages can create and maintain their own profiles; the most common use is for tracking resources that must be explicitly closed, such as files or network connections.

A Profile's methods can be called from multiple goroutines simultaneously.
Profile是一个 堆栈跟踪集合 展示 特定事件的情况下 调用顺序 ,如分配.
包可以创建和保留他们自己的分析. 最常见的使用是 跟踪明确关闭的源, 例如 文件或网络链接.

Profile的方法可以让多个goroutines同时调用.

Each Profile has a unique name. A few profiles are predefined:
每个Profile有唯一的名称.一些分析是预定义的:
```golang
goroutine    - stack traces of all current goroutines 追踪所有当前的goroutines
heap         - a sampling of all heap allocations   所有堆分配的抽样
threadcreate - stack traces that led to the creation of new OS threads  追踪导致创建新的OS 线程的 堆栈.
block        - stack traces that led to blocking on synchronization primitives  追踪 导致 在同步原语上阻塞的堆栈
```

These predefined profiles maintain themselves and panic on an explicit Add or Remove method call.

The CPU profile is not available as a Profile. It has a special API, the StartCPUProfile and StopCPUProfile functions, because it streams output to a writer during profiling.
这些预定义的 分析 保留他们 并且在 Add或Remove方法调用 会panic.
CPU分析不是一个可用的Profile.它有特定的API,StartCPUProfile 和 StopCPUProfile 函数, 因为它的流在分析期间输出到writer.



func Lookup
```golang
func Lookup(name string) *Profile
```
Lookup returns the profile with the given name, or nil if no such profile exists.
Lookup 用给定的name返回profile,如果profile存在 返回nil



func NewProfile
```golang
func NewProfile(name string) *Profile
```
NewProfile creates a new profile with the given name. If a profile with that name already exists, NewProfile panics. 
The convention is to use a 'import/path.' prefix to create separate name spaces for each package.
NewProfile用给定的name创建一个新的profile. 如果 已经存在,NewProfile panic.
通常使用 'import/path.' 前缀去为每个包 创建分隔 命名空间.



func (*Profile) Add
```golang
func (p *Profile) Add(value interface{}, skip int)
```
Add adds the current execution stack to the profile, associated with value. 
Add stores value in an internal map, so value must be suitable for use as a map key and will not be garbage collected until the corresponding call to Remove. 
Add panics if the profile already contains a stack for value.
Add添加关联的value到 当前执行堆栈的profile.
Add 存在 value到内部map, 所以value 必须适合用作映射键 并且 将不会垃圾回收 直到 调用对应的Remove.
如果profile 已经包含 value 堆栈, Add panic.

The skip parameter has the same meaning as runtime.Caller's skip and controls where the stack trace begins. 
Passing skip=0 begins the trace in the function calling Add. For example, given this execution stack:
skip 参数和runtime.Caller的skip一样  控制 堆栈跟踪开始.
通过skip=0 开始堆栈 在函数里调用Add.例如,给这些执行堆栈:

```golang
Add
called from rpc.NewClient
called from mypkg.Run
called from main.main
```
Passing skip=0 begins the stack trace at the call to Add inside rpc.NewClient. Passing skip=1 begins the stack trace at the call to NewClient inside mypkg.Run.
通过 skip=0  开始  堆栈跟踪 在 rpc.NewClient里调用 Add. 通过  skip=1 开始 堆栈跟踪在 mypkg.Run 调用 NewClient



func (*Profile) Count
```golang
func (p *Profile) Count() int
```
Count returns the number of execution stacks currently in the profile.
Count 返回在 profile里的 当前执行堆栈数.



func (*Profile) Name
```golang
func (p *Profile) Name() string
```
Name returns this profile's name, which can be passed to Lookup to reobtain the profile.
Name 返回 profile的名称, 其可以被传递给查找到reobtain的信息。


func (*Profile) Remove
```golang
func (p *Profile) Remove(value interface{})
```
Remove removes the execution stack associated with value from the profile. It is a no-op if the value is not in the profile.
Remove 从profile中 删除执行堆栈对应的value. 如果value不在 profile里 它是一个空操作.



func (*Profile) WriteTo
```golang
func (p *Profile) WriteTo(w io.Writer, debug int) error
```
WriteTo writes a pprof-formatted snapshot of the profile to w. If a write to w returns an error, WriteTo returns that error. Otherwise, WriteTo returns nil.

The debug parameter enables additional output. Passing debug=0 prints only the hexadecimal addresses that pprof needs. 
Passing debug=1 adds comments translating addresses to function names and line numbers, so that a programmer can read the profile without tools.

The predefined profiles may assign meaning to other debug values; 
for example, when printing the "goroutine" profile, debug=2 means to print the goroutine stacks in the same form that a Go program uses when dying due to an unrecovered panic.

WriteTo 写 一个 分析 pprof格式快照 到w.如果 写如w 返回 错误, WriteTo返回那个错误.否则WriteTo 返回nil.

debug参数 启用 额外输出.通过 debug=0  只打印 pprof 必须的16进制地址.
通过debug=1 添加注释转换地址 到函数名 和行号, 所以 那样 程序不通过工具就能读取,

预订分析可以指定其他debug值.
例如, 当打印"goroutine"  分析,debug=2  意味着 打印 panic 未恢复的  goroutine 堆栈 成GO程序可使用的形式,




