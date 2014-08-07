Package testing

Overview ▾

Package testing provides support for automated testing of Go packages. 
It is intended to be used in concert with the “go test” command, which automates execution of any function of the form
testing包提供 GO 包 自动化测试支持. 它的目的是用来 和 “go test” 命令一起用,它可以自动执行的形式中的任何功能


```golang
func TestXxx(*testing.T)
```
where Xxx can be any alphanumeric string (but the first letter must not be in [a-z]) and serves to identify the test routine.

Within these functions, use the Error, Fail or related methods to signal failure.

To write a new test suite, create a file whose name ends _test.go that contains the TestXxx functions as described here. 
Put the file in the same package as the one being tested. The file will be excluded from regular package builds but will be included when the “go test” command is run. 
For more detail, run “go help test” and “go help testflag”.

Tests and benchmarks may be skipped if not applicable with a call to the Skip method of *T and *B:

Xxx可以 是任何字母数字字符串(首字母必须是 [a-z]) 和用于识别的测试例程。

在这些功能,使用Error, Fail 或 related 方法 表示 失败.

写一个新的测试, 创建一个 名字以 _test.go  结尾的文件 ,此处的描述说明包含 TestXxx函数.
将文件放在同一个包作为一个测试. 该文件将被排除在正规包构建,但是当 “go test”命令执行时会包含进去. 更多相信信息查看“go help test” 和 “go help testflag”.


```golang
func TestTimeConsuming(t *testing.T) {
    if testing.Short() {
        t.Skip("skipping test in short mode.")
    }
    ...
}
```



Benchmarks

Functions of the form
函数形式
```golang
func BenchmarkXxx(*testing.B)
```
are considered benchmarks, and are executed by the "go test" command when its -bench flag is provided. Benchmarks are run sequentially.

For a description of the testing flags, see http://golang.org/cmd/go/#hdr-Description_of_testing_flags.

A sample benchmark function looks like this:

深思熟虑的基准，和由被执行 "go test"命令 当它提供  -bench 标示时.基准按顺序运行。

有关testing 标示 说明 查看http://golang.org/cmd/go/#hdr-Description_of_testing_flags.

一个简单的基准测试类似这样:

```golang
func BenchmarkHello(b *testing.B) {
    for i := 0; i < b.N; i++ {
        fmt.Sprintf("hello")
    }
}
```

The benchmark function must run the target code b.N times. The benchmark package will vary b.N until the benchmark function lasts long enough to be timed reliably. The output
基准函数 必须 执行 目标代码  b.N  次. benchmark包 将改变  b.N 直到 基准函数 持续足够长的时间，以可靠地进行计时。
```golang
BenchmarkHello    10000000    282 ns/op
```

means that the loop ran 10000000 times at a speed of 282 ns per loop.
这个输出意味着 以282 ns 速度 循环10000000 次.

If a benchmark needs some expensive setup before running, the timer may be reset:
如果 benchmark 运行前需要一些 昂贵的安装, timer 必须重值:
```golang
func BenchmarkBigLen(b *testing.B) {
    big := NewBig()
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        big.Len()
    }
}
```
If a benchmark needs to test performance in a parallel setting, it may use the RunParallel helper function; such benchmarks are intended to be used with the go test -cpu flag:
如果 benchmark 需要 在一个平行的设置里性能测试, 它可能使用 RunParallel 助手函数;  这样的基准测试的目的是   go test -cpu 标签:

```golang
func BenchmarkTemplateParallel(b *testing.B) {
    templ := template.Must(template.New("test").Parse("Hello, {{.}}!"))
    b.RunParallel(func(pb *testing.PB) {
        var buf bytes.Buffer
        for pb.Next() {
            buf.Reset()
            templ.Execute(&buf, "World")
        }
    })
}
```


Examples

The package also runs and verifies example code. 
Example functions may include a concluding line comment that begins with "Output:" and is compared with the standard output of the function when the tests are run. 
(The comparison ignores leading and trailing space.) These are examples of an example:
该包也执行和 验证 例子代码.  当执行测试时 例子函数 可能包含 以"Output:" 开始的结论行注释 并且  相比 标准输出函数.(这些比较忽略开始和结尾空格) 这些是一个例子的例子：

```golang
func ExampleHello() {
        fmt.Println("hello")
        // Output: hello
}

func ExampleSalutations() {
        fmt.Println("hello, and")
        fmt.Println("goodbye")
        // Output:
        // hello, and
        // goodbye
}
```

Example functions without output comments are compiled but not executed.

The naming convention to declare examples for the package, a function F, a type T and method M on type T are:
例子函数 不包含的注释会编译但不会执行.
该包的命名 约定了声明的例子, 函数a , a的类型T 和类型T上的 方法M :
```golang
func Example() { ... }
func ExampleF() { ... }
func ExampleT() { ... }
func ExampleT_M() { ... }
```

Multiple example functions for a package/type/function/method may be provided by appending a distinct suffix to the name. The suffix must start with a lower-case letter.
package/type/function/method  的多元函数的例子 可能提供  追加不同的后缀 到名称. 后缀必须以小写字母开始.
```golang
func Example_suffix() { ... }
func ExampleF_suffix() { ... }
func ExampleT_suffix() { ... }
func ExampleT_M_suffix() { ... }
```
The entire test file is presented as the example when it contains a single example function, at least one other function, type, variable, or constant declaration, and no test or benchmark functions.
当它包含单个例子函数时 , 整个测试文件 由本例子表现 , 最少有一个其他的函数 , 类型 ,变量 或 常量声明 和  没有测试或基准测试功能



func AllocsPerRun
```golang
func AllocsPerRun(runs int, f func()) (avg float64)
```
AllocsPerRun returns the average number of allocations during calls to f. Although the return value has type float64, it will always be an integral value.

To compute the number of allocations, the function will first be run once as a warm-up. 
The average number of allocations over the specified number of runs will then be measured and returned.

AllocsPerRun sets GOMAXPROCS to 1 during its measurement and will restore it before returning.

AllocsPerRun 返回调用f期间的平均值. 返回值是float64 类型, 它通常是整型值.

为了计算分配数量, 该函数 首先执行一次 热身. 分配 在指定的执行数量的平均数 会被测量 和返回.

AllocsPerRun 在测量它的期间 设置 GOMAXPROCS 为1 并且 在返回之前存储它.



func Main
```golang
func Main(matchString func(pat, str string) (bool, error), tests []InternalTest, benchmarks []InternalBenchmark, examples []InternalExample)
```
An internal function but exported because it is cross-package; part of the implementation of the "go test" command.
一个内部函数 但是可导出, 因为它是 跨包的. 实现 "go test"命令的一部分.


func RegisterCover
```golang
func RegisterCover(c Cover)
```
RegisterCover records the coverage data accumulators for the tests. NOTE: This function is internal to the testing infrastructure and may change. 
It is not covered (yet) by the Go 1 compatibility guidelines.
RegisterCover 记录 测试 覆盖的数据累加器.注: 该函数是内部测试的  可能会修改. 它不会覆盖GO1 兼容指南.



func RunBenchmarks
```golang
func RunBenchmarks(matchString func(pat, str string) (bool, error), benchmarks []InternalBenchmark)
```
An internal function but exported because it is cross-package; part of the implementation of the "go test" command.
一个内部函数 但是可导出, 因为它是 跨包的, 实现 "go test"命令的一部分.


func RunExamples
```golang
func RunExamples(matchString func(pat, str string) (bool, error), examples []InternalExample) (ok bool)
```


func RunTests
```golang
func RunTests(matchString func(pat, str string) (bool, error), tests []InternalTest) (ok bool)
```


func Short
```golang
func Short() bool
```
Short reports whether the -test.short flag is set.
Short报告 -test.short 标签是否设置.


func Verbose
```golang
func Verbose() bool
```
Verbose reports whether the -test.v flag is set.
Verbose 报告 -test.v 标签是否设置.


type B
```golang
type B struct {
        N int
        // contains filtered or unexported fields
}
```
B is a type passed to Benchmark functions to manage benchmark timing and to specify the number of iterations to run.
B 是一个通过 Benchmark函数 管理 benchmark 时间 和 指定迭代运行的次数的 类型.


func (*B) Error
```golang
func (c *B) Error(args ...interface{})
```
Error is equivalent to Log followed by Fail.
Error 等价于 Log 跟着 Fail



func (*B) Errorf
```golang
func (c *B) Errorf(format string, args ...interface{})
```
Errorf is equivalent to Logf followed by Fail.
Errorf 等价于 Logf 跟着 Fail



func (*B) Fail
```golang
func (c *B) Fail()
```
Fail marks the function as having failed but continues execution.
Fail 标记函数已经失败，但继续执行。


func (*B) FailNow
```golang
func (c *B) FailNow()
```
FailNow marks the function as having failed and stops its execution. Execution will continue at the next test or benchmark. 
FailNow must be called from the goroutine running the test or benchmark function, not from other goroutines created during the test. 
Calling FailNow does not stop those other goroutines.
FailNow  标记函数已经失败，并停止它的执行..下一个测试或基准时 会继续执行。
FailNow 必须从例程执行测试或 基本函数 调用, 而不是从其他例程 创建这个测试的期间.
调用FailNow 不会停止其他的例程


func (*B) Failed
```golang
func (c *B) Failed() bool
```
Failed reports whether the function has failed.
Failed 报告 函数是否有失败.


func (*B) Fatal
```golang
func (c *B) Fatal(args ...interface{})
```
Fatal is equivalent to Log followed by FailNow.
Fatal  等价 Log 跟着FailNow



func (*B) Fatalf
```golang
func (c *B) Fatalf(format string, args ...interface{})
```
Fatalf is equivalent to Logf followed by FailNow.
Fatalf  等价 Logf 跟着FailNow


func (*B) Log
```golang
func (c *B) Log(args ...interface{})
```
Log formats its arguments using default formatting, analogous to Println, and records the text in the error log. 
The text will be printed only if the test fails or the -test.v flag is set.
Log 使用默认的格式 格式化它的参数, 类似 Println 并记录错误日志里的 文本.
只在 测试失败或 -test.v 标志设置时 文本才会被打印.


func (*B) Logf
```golang
func (c *B) Logf(format string, args ...interface{})
```
Logf formats its arguments according to the format, analogous to Printf, and records the text in the error log. 
The text will be printed only if the test fails or the -test.v flag is set.
Logf 根据format 格式化它的参数, 类似Printf ,并记录错误日志里的 文本.
只在 测试失败或 -test.v 标志设置时 文本才会被打印.



func (*B) ReportAllocs
```golang
func (b *B) ReportAllocs()
```
ReportAllocs enables malloc statistics for this benchmark. It is equivalent to setting -test.benchmem, but it only affects the benchmark function that calls ReportAllocs.
ReportAllocs 使该基准的malloc统计 . 它等价于设置 -test.benchmem, 但是 只 影响调用 ReportAllocs的 基准函数.



func (*B) ResetTimer
```golang
func (b *B) ResetTimer()
```
ResetTimer zeros the elapsed benchmark time and memory allocation counters. It does not affect whether the timer is running.
ResetTimer 零  基准经过时间 和 内存分配计数器。不管 timer有没有在执行都不会有影响.



func (*B) RunParallel
```golang
func (b *B) RunParallel(body func(*PB))
```
RunParallel runs a benchmark in parallel. It creates multiple goroutines and distributes b.N iterations among them. 
The number of goroutines defaults to GOMAXPROCS. To increase parallelism for non-CPU-bound benchmarks, call SetParallelism before RunParallel.
RunParallel is usually used with the go test -cpu flag.

The body function will be run in each goroutine. It should set up any goroutine-local state and then iterate until pb.Next returns false. 
It should not use the StartTimer, StopTimer, or ResetTimer functions, because they have global effect.

RunParallel 并行执行一个基准. 它创建多例程 并分发b.N 其中迭代。
默认的例程数 在GOMAXPROCS. 为了增加非CPU绑定基准并行, 在RunParallel之前 调用 SetParallelism.
RunParallel 通常 和  go test -cpu 标示一起使用.

该函数体将在每个例程执行.它应该设置 任何Go例程本地状态 然后 迭代 直到  pb.Next 返回false
它不应该使用StartTimer, StopTimer, 或 ResetTimer 函数, 因为他们会全局影响.



▹ Example
```golang
package main

import (
	"bytes"
	"testing"
	"text/template"
)

func main() {
	// Parallel benchmark for text/template.Template.Execute on a single object.
	testing.Benchmark(func(b *testing.B) {
		templ := template.Must(template.New("test").Parse("Hello, {{.}}!"))
		// RunParallel will create GOMAXPROCS goroutines
		// and distribute work among them.
		b.RunParallel(func(pb *testing.PB) {
			// Each goroutine has its own bytes.Buffer.
			var buf bytes.Buffer
			for pb.Next() {
				// The loop body is executed b.N times total across all goroutines.
				buf.Reset()
				templ.Execute(&buf, "World")
			}
		})
	})
}
```



func (*B) SetBytes
```golang
func (b *B) SetBytes(n int64)
```
SetBytes records the number of bytes processed in a single operation. If this is called, the benchmark will report ns/op and MB/s.
SetBytes记录在操作里处理字节数. 如果调用了, 基准将 报告 ns/op 和 MB/s.



func (*B) SetParallelism
```golang
func (b *B) SetParallelism(p int)
```
SetParallelism sets the number of goroutines used by RunParallel to p*GOMAXPROCS. There is usually no need to call SetParallelism for CPU-bound benchmarks. 
If p is less than 1, this call will have no effect.
SetParallelism 使用 RunParallel 设置到p*GOMAXPROCS例程数. 通常不比为CPU限制 基准 调用   SetParallelism.如果p小于1,调用不会有影响.



func (*B) Skip
```golang
func (c *B) Skip(args ...interface{})
```
Skip is equivalent to Log followed by SkipNow.
Skip 等价于 Log 跟着 SkipNow


func (*B) SkipNow
```golang
func (c *B) SkipNow()
```
SkipNow marks the test as having been skipped and stops its execution. Execution will continue at the next test or benchmark. 
See also FailNow. SkipNow must be called from the goroutine running the test, not from other goroutines created during the test. 
Calling SkipNow does not stop those other goroutines.
SkipNow 标记测试跳过和停止它的执行. 在下一次 测试或基准的时候 会继续执行.
查看 FailNow . SkipNow 必须 从 例程 执行测试 调用,而不是 在例程创建 测试的期间.
调用SkipNow 不会停止其他例程


func (*B) Skipf
```golang
func (c *B) Skipf(format string, args ...interface{})
```
Skipf is equivalent to Logf followed by SkipNow.
Skipf 等价于 Logf跟着 SkipNow



func (*B) Skipped
```golang
func (c *B) Skipped() bool
```
Skipped reports whether the test was skipped.
Skipped报告 测试是否跳过.


func (*B) StartTimer
```golang
func (b *B) StartTimer()
```
StartTimer starts timing a test. This function is called automatically before a benchmark starts, but it can also used to resume timing after a call to StopTimer.
StartTimer 开始计时测试.该函数在基准开始之前会自动调用,  它也能在调用StopTimer后 用来恢复计时.



func (*B) StopTimer
```golang
func (b *B) StopTimer()
```
StopTimer stops timing a test. This can be used to pause the timer while performing complex initialization that you don't want to measure.
StopTimer 停止计时一个测试. 这可以用来暂停计时  在执行你不想来衡量复杂的初始化.



type BenchmarkResult
```golang
type BenchmarkResult struct {
        N         int           // The number of iterations.
        T         time.Duration // The total time taken.
        Bytes     int64         // Bytes processed in one iteration.
        MemAllocs uint64        // The total number of memory allocations.
        MemBytes  uint64        // The total number of bytes allocated.
}
```
The results of a benchmark run.
基准执行的结果


func Benchmark
```golang
func Benchmark(f func(b *B)) BenchmarkResult
```
Benchmark benchmarks a single function. Useful for creating custom benchmarks that do not use the "go test" command.
Benchmark 基准 单个函数. 对于没有使用  "go test" 命令 而 创建自定义基准的 很有用.



func (BenchmarkResult) AllocedBytesPerOp
```golang
func (r BenchmarkResult) AllocedBytesPerOp() int64
```


func (BenchmarkResult) AllocsPerOp
```golang
func (r BenchmarkResult) AllocsPerOp() int64
```


func (BenchmarkResult) MemString
```golang
func (r BenchmarkResult) MemString() string
```


func (BenchmarkResult) NsPerOp
```golang
func (r BenchmarkResult) NsPerOp() int64
```


func (BenchmarkResult) String
```golang
func (r BenchmarkResult) String() string
```



type Cover
```golang
type Cover struct {
        Mode            string
        Counters        map[string][]uint32
        Blocks          map[string][]CoverBlock
        CoveredPackages string
}
```
Cover records information about test coverage checking. 
NOTE: This struct is internal to the testing infrastructure and may change. It is not covered (yet) by the Go 1 compatibility guidelines.
Cover 记录 测试覆盖检查 的有关信息.
注:这个结构是内部的测试 并且可能发生变化。 不兼容GO1



type CoverBlock
```golang
type CoverBlock struct {
        Line0 uint32
        Col0  uint16
        Line1 uint32
        Col1  uint16
        Stmts uint16
}
```
CoverBlock records the coverage data for a single basic block.
NOTE: This struct is internal to the testing infrastructure and may change. It is not covered (yet) by the Go 1 compatibility guidelines.
CoverBlock 记录单个基本模块 覆盖 的数据.
注:这个结构是内部的测试 并且可能发生变化。 不兼容GO1


type InternalBenchmark
```golang
type InternalBenchmark struct {
        Name string
        F    func(b *B)
}
```
An internal type but exported because it is cross-package; part of the implementation of the "go test" command.
一个内部类型但是可导出 因为它的跨包的; 实现  "go test"  命令的 一部分.



type InternalExample
```golang
type InternalExample struct {
        Name   string
        F      func()
        Output string
}
```



type InternalTest
```golang
type InternalTest struct {
        Name string
        F    func(*T)
}
```
An internal type but exported because it is cross-package; part of the implementation of the "go test" command.
一个内部类型但是可导出 因为它的跨包的; 实现  "go test"  命令的 一部分.


type PB
```golang
type PB struct {
        // contains filtered or unexported fields
}
```
A PB is used by RunParallel for running parallel benchmarks.
PB 为执行中的并行基准 使用RunParallel 



func (*PB) Next
```golang
func (pb *PB) Next() bool
```
Next reports whether there are more iterations to execute.
Next报告 是否有更多迭代来执行.



type T
```golang
type T struct {
        // contains filtered or unexported fields
}
```
T is a type passed to Test functions to manage test state and support formatted test logs. Logs are accumulated during execution and dumped to standard error when done.
T 是一个 传给 Test函数来管理测试状态 和 支持 格式化 测试日志的 类型.  日志是执行过程中积累的 并转储到标准错误



func (*T) Error
```golang
func (c *T) Error(args ...interface{})
```
Error is equivalent to Log followed by Fail.
Error 等价于 Log跟着 Fail



func (*T) Errorf
```golang
func (c *T) Errorf(format string, args ...interface{})
```
Errorf is equivalent to Logf followed by Fail.
Errorf等价于 Logf跟着 Fail



func (*T) Fail
```golang
func (c *T) Fail()
```
Fail marks the function as having failed but continues execution.
Fail 标记函数失败 但继续执行


func (*T) FailNow
```golang
func (c *T) FailNow()
```
FailNow marks the function as having failed and stops its execution. Execution will continue at the next test or benchmark. 
FailNow must be called from the goroutine running the test or benchmark function, not from other goroutines created during the test. 
Calling FailNow does not stop those other goroutines.
FailNow  标记函数已经失败，并停止它的执行.下一个测试或基准时 会继续执行。
FailNow 必须从例程执行测试或 基本函数 调用, 而不是从其他例程 创建这个测试的期间.
调用FailNow 不会停止其他的例程



func (*T) Failed
```golang
func (c *T) Failed() bool
```
Failed reports whether the function has failed.
Failed 报告 函数是否失败.


func (*T) Fatal
```golang
func (c *T) Fatal(args ...interface{})
```
Fatal is equivalent to Log followed by FailNow.
Fatal 等价于 Log 跟着 FailNow



func (*T) Fatalf
```golang
func (c *T) Fatalf(format string, args ...interface{})
```
Fatalf is equivalent to Logf followed by FailNow.
Fatalf 等价于 Logf跟着 FailNow


func (*T) Log
```golang
func (c *T) Log(args ...interface{})
```
Log formats its arguments using default formatting, analogous to Println, and records the text in the error log. 
The text will be printed only if the test fails or the -test.v flag is set.
Log 使用默认的格式 格式化它的参数, 类似 Println 并记录错误日志里的 文本.
只在 测试失败或 -test.v 标志设置时 文本才会被打印.


func (*T) Logf
```golang
func (c *T) Logf(format string, args ...interface{})
```
Logf formats its arguments according to the format, analogous to Printf, and records the text in the error log. 
The text will be printed only if the test fails or the -test.v flag is set.
Logf 根据format 格式化它的参数, 类似Printf ,并记录错误日志里的 文本.
只在 测试失败或 -test.v 标志设置时 文本才会被打印.


func (*T) Parallel
```golang
func (t *T) Parallel()
```
Parallel signals that this test is to be run in parallel with (and only with) other parallel tests.
Parallel 示意 这测试 和其他测试并行



func (*T) Skip
```golang
func (c *T) Skip(args ...interface{})
```
Skip is equivalent to Log followed by SkipNow.
Skip 等价于 Log 跟着 SkipNow



func (*T) SkipNow
```golang
func (c *T) SkipNow()
```
SkipNow marks the test as having been skipped and stops its execution. 
Execution will continue at the next test or benchmark. See also FailNow. 
SkipNow must be called from the goroutine running the test, not from other goroutines created during the test. Calling SkipNow does not stop those other goroutines.
SkipNow 标记测试跳过和停止它的执行. 在下一次 测试或基准的时候 会继续执行.
查看 FailNow . SkipNow 必须 从 例程 执行测试 调用,而不是 在例程创建 测试的期间.
调用SkipNow 不会停止其他例程


func (*T) Skipf
```golang
func (c *T) Skipf(format string, args ...interface{})
```
Skipf is equivalent to Logf followed by SkipNow.
Skipf 等价于 Logf跟着 SkipNow



func (*T) Skipped
```golang
func (c *T) Skipped() bool
```
Skipped reports whether the test was skipped.
Skipped 报告测试是否跳过.


type TB
```golang
type TB interface {
        Error(args ...interface{})
        Errorf(format string, args ...interface{})
        Fail()
        FailNow()
        Failed() bool
        Fatal(args ...interface{})
        Fatalf(format string, args ...interface{})
        Log(args ...interface{})
        Logf(format string, args ...interface{})
        Skip(args ...interface{})
        SkipNow()
        Skipf(format string, args ...interface{})
        Skipped() bool
        // contains filtered or unexported methods
}
```
TB is the interface common to T and B.
TB 是 T 和 B接口共用





