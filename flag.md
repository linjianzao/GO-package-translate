包地址：http://golang.org/pkg/flag/

Overview ▾
```golang
Package flag implements command-line flag parsing.
Usage:
Define flags using flag.String(), Bool(), Int(), etc.
This declares an integer flag, -flagname, stored in the pointer ip, with type *int.

flag包实现对命令行 标示的 解析
用法:
使用 flag.String(), Bool(), Int() 等 定义flags
声明一个整型的 flag ,-flagname, 并存储在*int 类型的指针ip

```golang
import "flag"
var ip = flag.Int("flagname", 1234, "help message for flagname")
```

If you like, you can bind the flag to a variable using the Var() functions.
如果你喜欢,你可以使用Var()函数绑定flagd到变量上

```golang
var flagvar int
func init() {
	flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")
}
```
Or you can create custom flags that satisfy the Value interface (with pointer receivers) and couple them to flag parsing by
或者你可以创建自定义的 flags,满足Value接口(含有接收者指针) 解析一对flag
```golang
flag.Var(&flagVal, "name", "help message for flagname")
```

For such flags, the default value is just the initial value of the variable.
After all flags are defined, call
对于这样的flags,默认值值是初始化变量的值
当所有的flags都定义后,再调用
```golang
flag.Parse()
```

to parse the command line into the defined flags.
Flags may then be used directly. If you're using the flags themselves, they are all pointers; if you bind to variables, they're values.
解析命令行到定义的flags
Flags也可以直接使用.如果你使用flags他们自己, 他们全部都指针;如果你绑定变量, 那他们都是值.
```golang
fmt.Println("ip has value ", *ip)
fmt.Println("flagvar has value ", flagvar)
After parsing, the arguments after the flag are available as the slice flag.Args() or individually as flag.Arg(i). The arguments are indexed from 0 through flag.NArg()-1.
```
Command line flag syntax:
命令行flag语法
```golang
-flag
-flag=x
-flag x  // non-boolean flags only
```

One or two minus signs may be used; they are equivalent. The last form is not permitted for boolean flags because the meaning of the command
 使用一个或两个减号;他们是等价的. 最后的形式不允许是布尔值 因为这意味着命令
```golang
cmd -x *
```
will change if there is a file called 0, false, etc. You must use the -flag=false form to turn off a boolean flag.
Flag parsing stops just before the first non-flag argument ("-" is a non-flag argument) or after the terminator "--".
Integer flags accept 1234, 0664, 0x1234 and may be negative. Boolean flags may be 1, 0, t, f, true, false, TRUE, FALSE, True, False. 
	Duration flags accept any input valid for time.ParseDuration.
The default set of command-line flags is controlled by top-level functions. 
	The FlagSet type allows one to define independent sets of flags, such as to implement subcommands in a command-line interface. 
	The methods of FlagSet are analogous to the top-level functions for the command-line flag set.
	
将会改变 如果有一个文件调用 0,false 等.你必须使用-flag=false 的形式 关闭布尔值的flag
Flag 只有在遇到第一个不是flag参数("-"不是flag参数)之前或者终结符"--" 解析才会停止
整型flags接受1234, 0664, 0x1234 可能还有负数.布尔型flags 可能是 1, 0, t, f, true, false, TRUE, FALSE, True, False. 
长 flags接口任何time.ParseDuration 的有效值
默认设置命令行flags 用高级函数控制. FlagSet类型 允许一个 独立设置的flags,例如 在命令行接口 实现子命令
FlagSet的方法 匿名高级函数  设置 命令行flag

▾ Example

Code:
```golang
// These examples demonstrate more intricate uses of the flag package. 这个例子演示 更复杂的flag 使用方式
package flag_test

import (
    "errors"
    "flag"
    "fmt"
    "strings"
    "time"
)

// Example 1: A single string flag called "species" with default value "gopher".
//例子1:单个字符串flag 使用默认值"gopher" 调用"species" 

var species = flag.String("species", "gopher", "the species we are studying")

// Example 2: Two flags sharing a variable, so we can have a shorthand.  两个flag 共享一个变量,所以我们可以有一个速记
// The order of initialization is undefined, so make sure both use the	 初始化顺序是undefined,所有要确保都有相同的默认值. 他们必须有一个初始化函数
// same default value. They must be set up with an init function.
var gopherType string

func init() {
    const (
        defaultGopher = "pocket"
        usage         = "the variety of gopher"
    )
    flag.StringVar(&gopherType, "gopher_type", defaultGopher, usage)
    flag.StringVar(&gopherType, "g", defaultGopher, usage+" (shorthand)")
}

// Example 3: A user-defined flag type, a slice of durations. 用户定义flag类型,持续时间的slice
type interval []time.Duration

// String is the method to format the flag's value, part of the flag.Value interface.  String是格式化flag的值的方法,是flag.Value接口的一部分 
// The String method's output will be used in diagnostics.	String方法的输出会 判断
func (i *interval) String() string {
    return fmt.Sprint(*i)
}

// Set is the method to set the flag value, part of the flag.Value interface. Set是设置flag值的方法,是 flag.Value接口的一部分
// Set's argument is a string to be parsed to set the flag.	 它的参数是一个解析过的字符串 用来设置 flag			
// It's a comma-separated list, so we split it.			它是逗号分隔的列 所以我们可以分隔它
func (i *interval) Set(value string) error {
    // If we wanted to allow the flag to be set multiple times, 如果我们想要允许flag 可以设置多个时间
    // accumulating values, we would delete this if statement.  累计值, 我们将会删除这个if语句
    // That would permit usages such as	可以和其他组合这样使用
    //	-deltaT 10s -deltaT 15s
    // and other combinations. 
    if len(*i) > 0 {
        return errors.New("interval flag already set")
    }
    for _, dt := range strings.Split(value, ",") {
        duration, err := time.ParseDuration(dt)
        if err != nil {
            return err
        }
        *i = append(*i, duration)
    }
    return nil
}

// Define a flag to accumulate durations. Because it has a special type, 定义一个flag积累持续时间. 因为它有一个特殊类型,我们需要使用Var函数,然后在初始化的时候创建那个flag
// we need to use the Var function and therefore create the flag during
// init.

var intervalFlag interval

func init() {
    // Tie the command-line flag to the intervalFlag variable and 绑定命令行flag 到 intervalFlag变量 并且设置 用法消息
    // set a usage message.
    flag.Var(&intervalFlag, "deltaT", "comma-separated list of intervals to use between events")
}

func Example() {
    // All the interesting pieces are with the variables declared above, but
    // to enable the flag package to see the flags defined there, one must
    // execute, typically at the start of main (not init!):
    //	flag.Parse()
    // We don't run it here because this is not a main function and
    // the testing suite has already parsed the flags.
    
    //上面的变量声明都是有趣的用法.但是 启用flag包 看flags定义, 必须一个执行, 通常是在main里开始 (不是init!)
   //	flag.Parse()
    //我们在这没办法运行 因为这没有 main函数 并且 测试 已经解析了flag
	    
}
```
```

Variables
```golang
```golang
var CommandLine = NewFlagSet(os.Args[0], ExitOnError)
```
CommandLine is the default set of command-line flags, parsed from os.Args. 
The top-level functions such as BoolVar, Arg, and on are wrappers for the methods of CommandLine.
CommandLine 从os.Args 解析的 命令行flags的默认设置
高级函数 如BoolVar, Arg, 和 对CommandLine有封装的 方法

```golang
var ErrHelp = errors.New("flag: help requested")
```
ErrHelp is the error returned if the flag -help is invoked but no such flag is defined.
如果flag  调用 -help 但是 这个flag没有定义 就会返回错误

```golang
var Usage = func() {
    fmt.Fprintf(os.Stderr, "Usage of %s:\n", os.Args[0])
    PrintDefaults()
}
```
Usage prints to standard error a usage message documenting all defined command-line flags. 
The function is a variable that may be changed to point to a custom function
打印所有定义的命令行flags到标准错误 
这个函数是变量值 可能会改变  指向的自定义函数
```

func Arg
```golang
func Arg(i int) string
	Arg returns the i'th command-line argument. Arg(0) is the first remaining argument after flags have been processed.
	Arg返回 i 命令行参数.  Arg(0)是flag 处理后 剩余的第一个参数
```

func Args
```golang
func Args() []string
	Args returns the non-flag command-line arguments.
	返回没有flag的命令行参数
```

func Bool
```golang
func Bool(name string, value bool, usage string) *bool
	Bool defines a bool flag with specified name, default value, and usage string. 
	The return value is the address of a bool variable that stores the value of the flag.
	用指定的 name , value  和usage 定义一个 布尔 flag
	返回值是存储flag值的 布尔值变量的 地址
```

func BoolVar
```golang
func BoolVar(p *bool, name string, value bool, usage string)
	BoolVar defines a bool flag with specified name, default value, and usage string. 
	The argument p points to a bool variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 布尔 flag
	参数p 指向一个存储flag值的布尔值变量 
```

func Duration
```golang
func Duration(name string, value time.Duration, usage string) *time.Duration
	Duration defines a time.Duration flag with specified name, default value, and usage string. 
	The return value is the address of a time.Duration variable that stores the value of the flag.
	用指定的 name , value  和usage 定义一个 time.Duration  flag
	返回值是存储flag值的 time.Duration 变量的 地址
```

func DurationVar
```golang
func DurationVar(p *time.Duration, name string, value time.Duration, usage string)
	DurationVar defines a time.Duration flag with specified name, default value, and usage string. 
	The argument p points to a time.Duration variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 time.Duration  flag
	参数p 指向一个存储flag值的time.Duration 变量 
```

func Float64
```golang
func Float64(name string, value float64, usage string) *float64
	Float64 defines a float64 flag with specified name, default value, and usage string. 
	The return value is the address of a float64 variable that stores the value of the flag.
	用指定的 name , value  和usage 定义一个 float64  flag
	返回值是存储flag值的 float64 变量的 地址
```

func Float64Var
```golang
func Float64Var(p *float64, name string, value float64, usage string)
	Float64Var defines a float64 flag with specified name, default value, and usage string. 
	The argument p points to a float64 variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 float64  flag
	参数p 指向一个存储flag值的float64变量 
```

func Int
```golang
func Int(name string, value int, usage string) *int
	Int defines an int flag with specified name, default value, and usage string. 
	The return value is the address of an int variable that stores the value of the flag.
	用指定的 name , value  和usage 定义一个 int  flag
	返回值是存储flag值的 int 变量的 地址
```

func Int64
```golang
func Int64(name string, value int64, usage string) *int64
	Int64 defines an int64 flag with specified name, default value, and usage string. 
	The return value is the address of an int64 variable that stores the value of the flag.
	用指定的 name , value  和usage 定义一个 int64  flag
	返回值是存储flag值的 int64 变量的 地址
```

func Int64Var
```golang
func Int64Var(p *int64, name string, value int64, usage string)
	Int64Var defines an int64 flag with specified name, default value, and usage string. 
	The argument p points to an int64 variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 int64  flag
	参数p 指向一个存储flag值的int64变量 
``

func IntVar
```golang
func IntVar(p *int, name string, value int, usage string)
	IntVar defines an int flag with specified name, default value, and usage string. 
	The argument p points to an int variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 int  flag
	参数p 指向一个存储flag值的int变量 
```

func NArg
```golang
func NArg() int
	NArg is the number of arguments remaining after flags have been processed.
	是被处理flags后 剩余的参数数量
```

func NFlag
```golang
func NFlag() int
	NFlag returns the number of command-line flags that have been set.
	返回命令行flag已经设置的数量
```

func Parse
```golang
func Parse()
	Parse parses the command-line flags from os.Args[1:]. 
	Must be called after all flags are defined and before flags are accessed by the program.
	从os.Args[1:] 中解析命令行flag
	必须在所有的flag定义之后 和程序访问之前调用
```

func Parsed
```golang
func Parsed() bool
	Parsed returns true if the command-line flags have been parsed.
	如果flag已经解析过了 返回true
```

func PrintDefaults
```golang
func PrintDefaults()
	PrintDefaults prints to standard error the default values of all defined command-line flags.
	打印 所有的 命令行 flag 默认值 到标准错误
```

func Set
```golang
func Set(name, value string) error
	Set sets the value of the named command-line flag.
	设置命令行flag的名称
```

func String
```golang
func String(name string, value string, usage string) *string
	String defines a string flag with specified name, default value, and usage string. 
	The return value is the address of a string variable that stores the value of the flag.
	使用指定的 name ,value ,usage 定义 字符串 flag
	返回值是存储flag值的 字符串 变量 的地址
```

func StringVar
````golang
func StringVar(p *string, name string, value string, usage string)
	StringVar defines a string flag with specified name, default value, and usage string. 
	The argument p points to a string variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 string  flag
	参数p 指向一个存储flag值的string变量 
```

func Uint
```golang
func Uint(name string, value uint, usage string) *uint
	Uint defines a uint flag with specified name, default value, and usage string. 
	The return value is the address of a uint variable that stores the value of the flag.
	使用指定的 name ,value ,usage 定义 uint flag
	返回值是存储flag值的 uint 变量 的地址
```

func Uint64
```golang
func Uint64(name string, value uint64, usage string) *uint64
	Uint64 defines a uint64 flag with specified name, default value, and usage string. 
	The return value is the address of a uint64 variable that stores the value of the flag.
	使用指定的 name ,value ,usage 定义 uint64 flag
	返回值是存储flag值的 uint64 变量 的地址
```

func Uint64Var
```golang
func Uint64Var(p *uint64, name string, value uint64, usage string)
	Uint64Var defines a uint64 flag with specified name, default value, and usage string. 
	The argument p points to a uint64 variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 uint64  flag
	参数p 指向一个存储flag值的uint64变量 
```

func UintVar
```golang
func UintVar(p *uint, name string, value uint, usage string)
	UintVar defines a uint flag with specified name, default value, and usage string. 
	The argument p points to a uint variable in which to store the value of the flag.
	用指定的 name , value  和usage 定义一个 uint  flag
	参数p 指向一个存储flag值的uint变量 
```

func Var
```golang
func Var(value Value, name string, usage string)
	Var defines a flag with the specified name and usage string. 
	The type and value of the flag are represented by the first argument, of type Value, which typically holds a user-defined implementation of Value. 
	For instance, the caller could create a flag that turns a comma-separated string into a slice of strings by giving the slice the methods of Value; 
	in particular, Set would decompose the comma-separated string into the slice.
	使用指定的name和usage 定义一个flag
	flag的类型和值由第一个参数表示,Value 类型, 通常是用户定义的Value
	例如,调用者可以创建一个flag, 把一个逗号分隔的字符串转为字符串的切片的方法
	尤其, 将 逗号分隔的字符串转换成slice
```

func Visit
```golang
func Visit(fn func(*Flag))
	Visit visits the command-line flags in lexicographical order, calling fn for each. It visits only those flags that have been set.
	顺序 访问 命令行flag 的字典, 给每个 flag 调用fn. 只访问有设置的flag
```
	
func VisitAll
```golang
func VisitAll(fn func(*Flag))
	VisitAll visits the command-line flags in lexicographical order, calling fn for each. It visits all flags, even those not set.
	顺序 访问 命令行flag 的字典, 给每个 flag 调用fn. 会访问所有的flag ,即使没有设置
```

type ErrorHandling
```golang
```golang
type ErrorHandling int
```
ErrorHandling defines how to handle flag parsing errors.
定义了怎么处理flag解析 错误

```golang
const (
    ContinueOnError ErrorHandling = iota
    ExitOnError
    PanicOnError
)
```
```

type Flag
```golang
type Flag struct {
    Name     string // name as it appears on command line
    Usage    string // help message
    Value    Value  // value as set
    DefValue string // default value (as text); for usage message
}
	A Flag represents the state of a flag.
	表示一个flag的状态
```

func Lookup
```golang
func Lookup(name string) *Flag
	Lookup returns the Flag structure of the named command-line flag, returning nil if none exists.
	返回 给定name的 命令行flag 结构,如果不存在返回nil
```

type FlagSet
```golang
type FlagSet struct {
    // Usage is the function called when an error occurs while parsing flags. 当解析flag遇到错误时候 函数的用法
    // The field is a function (not a method) that may be changed to point to  字段是一个函数, 可以改变指向 自定义错误处理
    // a custom error handler.
    Usage func()
    // contains filtered or unexported fields   包含过滤或 未导出字段
}
	A FlagSet represents a set of defined flags. The zero value of a FlagSet has no name and has ContinueOnError error handling.
	FlagSet 表示  一组定义的 flag.FlagSet的零值 没有名称 并且 有ContinueOnError 错误处理
```

func NewFlagSet
```golang
func NewFlagSet(name string, errorHandling ErrorHandling) *FlagSet
	NewFlagSet returns a new, empty flag set with the specified name and error handling property.
	使用指定的name和 错误处理属性  返回一个新的 空的 flag
```

func (*FlagSet) Arg
```golang
func (f *FlagSet) Arg(i int) string
	Arg returns the i'th argument. Arg(0) is the first remaining argument after flags have been processed.
	返回i的参数 .  Arg(0)是flags 处理过  剩余的第一个参数
```

func (*FlagSet) Args
```golang
func (f *FlagSet) Args() []string
	Args returns the non-flag arguments.
	返回非flag参数
```

func (*FlagSet) Bool
```golang
func (f *FlagSet) Bool(name string, value bool, usage string) *bool
	Bool defines a bool flag with specified name, default value, and usage string. 
	The return value is the address of a bool variable that stores the value of the flag.
	使用给定的name ,value 和usage定义一个布尔值flag
	返回值是 存储flag值的 布尔值变量的地址
```

func (*FlagSet) BoolVar
```golang
func (f *FlagSet) BoolVar(p *bool, name string, value bool, usage string)
	BoolVar defines a bool flag with specified name, default value, and usage string. 
	The argument p points to a bool variable in which to store the value of the flag.
	使用给定的name ,value 和usage定义一个布尔值flag
	参数p指向 存储 flag值的布尔值变量
```

func (*FlagSet) Duration
```golang
func (f *FlagSet) Duration(name string, value time.Duration, usage string) *time.Duration
	Duration defines a time.Duration flag with specified name, default value, and usage string. 
	The return value is the address of a time.Duration variable that stores the value of the flag.
	使用给定的name ,value 和usage定义一个time.Duration  flag
	返回值是 存储flag值的 time.Duration变量的地址
```

func (*FlagSet) DurationVar
```golang
func (f *FlagSet) DurationVar(p *time.Duration, name string, value time.Duration, usage string)
	DurationVar defines a time.Duration flag with specified name, default value, and usage string. 
	The argument p points to a time.Duration variable in which to store the value of the flag.
	使用给定的name ,value 和usage定义一个 time.Duration flag
	参数p指向 存储 flag值的time.Duration变量
```

func (*FlagSet) Float64
```golang
func (f *FlagSet) Float64(name string, value float64, usage string) *float64
	Float64 defines a float64 flag with specified name, default value, and usage string. 
	The return value is the address of a float64 variable that stores the value of the flag.
	使用给定的name ,value 和usage定义一个 float64 flag
	返回值是 存储flag值的 float64变量的地址
```

func (*FlagSet) Float64Var
```golang
func (f *FlagSet) Float64Var(p *float64, name string, value float64, usage string)
	Float64Var defines a float64 flag with specified name, default value, and usage string. 
	The argument p points to a float64 variable in which to store the value of the flag.
	使用给定的name ,value 和usage定义一个 float64 flag
	参数p指向 存储 flag值的float64变量
```

func (*FlagSet) Init
```golang
func (f *FlagSet) Init(name string, errorHandling ErrorHandling)
	Init sets the name and error handling property for a flag set. 
	By default, the zero FlagSet uses an empty name and the ContinueOnError error handling policy.
	设置flag的 name 和 错误处理属性
	默认情况下, FlagSet的零值 使用空的name和 ContinueOnError 错误处理方法
```

func (*FlagSet) Int
```golang
func (f *FlagSet) Int(name string, value int, usage string) *int
	Int defines an int flag with specified name, default value, and usage string. 
	The return value is the address of an int variable that stores the value of the flag.
	使用给定的name ,value 和usage定义一个 int flag
	返回值是 存储flag值的 int变量的地址
```

func (*FlagSet) Int64
```golang
func (f *FlagSet) Int64(name string, value int64, usage string) *int64
	Int64 defines an int64 flag with specified name, default value, and usage string. 
	The return value is the address of an int64 variable that stores the value of the flag.
	使用给定的name ,value 和usage定义一个 int64 flag
	返回值是 存储flag值的 int64变量的地址
```

func (*FlagSet) Int64Var
```golang
func (f *FlagSet) Int64Var(p *int64, name string, value int64, usage string)
	Int64Var defines an int64 flag with specified name, default value, and usage string. 
	The argument p points to an int64 variable in which to store the value of the flag.
	使用给定的name ,value 和usage定义一个 int64 flag
	参数p指向 存储 flag值的int64变量
```

func (*FlagSet) IntVar
```golang
func (f *FlagSet) IntVar(p *int, name string, value int, usage string)
	IntVar defines an int flag with specified name, default value, and usage string. 
	The argument p points to an int variable in which to store the value of the flag.
	使用给定的name ,value 和usage定义一个 int flag
	参数p指向 存储 flag值的int变量
```

func (*FlagSet) Lookup
```golang
func (f *FlagSet) Lookup(name string) *Flag
	Lookup returns the Flag structure of the named flag, returning nil if none exists.
	返回 给定name的 命令行flag 结构,如果不存在返回nil
```

func (*FlagSet) NArg
```golang
func (f *FlagSet) NArg() int
	NArg is the number of arguments remaining after flags have been processed.
	是flag处理后的剩余参数数量
```

func (*FlagSet) NFlag
```golang
func (f *FlagSet) NFlag() int
	NFlag returns the number of flags that have been set.
	返回flag设置的数量
```

func (*FlagSet) Parse
```golang
func (f *FlagSet) Parse(arguments []string) error
	Parse parses flag definitions from the argument list, which should not include the command name.
	Must be called after all flags in the FlagSet are defined and before flags are accessed by the program. 
	The return value will be ErrHelp if -help was set but not defined.
	解析 参数列表 定义的  flag, 必须不包含 命令名
	必须 FlagSet里所有的 flag 都设置后 和 程序访问flag之前 调用
	如果 -help有设置但是没有定义 返回值将会是 ErrHelp
```
	
func (*FlagSet) Parsed
```golang
func (f *FlagSet) Parsed() bool
	Parsed reports whether f.Parse has been called.
	判断f.Parse是否被调用
```

func (*FlagSet) PrintDefaults
```golang
func (f *FlagSet) PrintDefaults()
	PrintDefaults prints, to standard error unless configured otherwise, the default values of all defined flags in the set.
	除非有其他配置 否则 打印到 所有定义的flag集合的默认值到  标准错误,
```

func (*FlagSet) Set
```golang
func (f *FlagSet) Set(name, value string) error
	Set sets the value of the named flag.
	设置 flag名  的值
```

func (*FlagSet) SetOutput
```golang
func (f *FlagSet) SetOutput(output io.Writer)
	SetOutput sets the destination for usage and error messages. If output is nil, os.Stderr is used.
	设置用法定义和错误信息.如果output是nil,使用os.Stderr
```

func (*FlagSet) String
```golang
func (f *FlagSet) String(name string, value string, usage string) *string
	String defines a string flag with specified name, default value, and usage string. 
	The return value is the address of a string variable that stores the value of the flag.
	使用指定的name  value  usage 定义一个 string flag 
	返回值是存储 flag值的string变量的地址
```

func (*FlagSet) StringVar
```golang
func (f *FlagSet) StringVar(p *string, name string, value string, usage string)
	StringVar defines a string flag with specified name, default value, and usage string. 
	The argument p points to a string variable in which to store the value of the flag.
	使用指定的name  value  usage 定义一个 string flag 
	参数p指向 存储 flag值的string变量
```

func (*FlagSet) Uint
```golang
func (f *FlagSet) Uint(name string, value uint, usage string) *uint
	Uint defines a uint flag with specified name, default value, and usage string. 
	The return value is the address of a uint variable that stores the value of the flag.
	使用指定的name  value  usage 定义一个 uint flag 
	返回值是存储 flag值的uint变量的地址
```

func (*FlagSet) Uint64
```golang
func (f *FlagSet) Uint64(name string, value uint64, usage string) *uint64
	Uint64 defines a uint64 flag with specified name, default value, and usage string. 
	The return value is the address of a uint64 variable that stores the value of the flag.
	使用指定的name  value  usage 定义一个 uint64 flag 
	返回值是存储 flag值的uint64变量的地址
```

func (*FlagSet) Uint64Var
```golang
func (f *FlagSet) Uint64Var(p *uint64, name string, value uint64, usage string)
	Uint64Var defines a uint64 flag with specified name, default value, and usage string. 
	The argument p points to a uint64 variable in which to store the value of the flag.
	使用指定的name  value  usage 定义一个 uint64 flag 
	参数p指向 存储 flag值的uint64变量
```

func (*FlagSet) UintVar
```golang
func (f *FlagSet) UintVar(p *uint, name string, value uint, usage string)
	UintVar defines a uint flag with specified name, default value, and usage string. 
	The argument p points to a uint variable in which to store the value of the flag.
	使用指定的name  value  usage 定义一个 uint flag 
	参数p指向 存储 flag值的uint变量
```

func (*FlagSet) Var
```golang
func (f *FlagSet) Var(value Value, name string, usage string)
	Var defines a flag with the specified name and usage string. 
	The type and value of the flag are represented by the first argument, of type Value, which typically holds a user-defined implementation of Value. 
	For instance, the caller could create a flag that turns a comma-separated string into a slice of strings by giving the slice the methods of Value; 
	in particular, Set would decompose the comma-separated string into the slice.
	使用指定的name和usage 定义一个flag
	flag的类型和值由第一个参数表示,Value 类型, 通常是用户定义的Value
	例如,调用者可以创建一个flag, 把一个逗号分隔的字符串转为字符串的切片的方法
	尤其, 将 逗号分隔的字符串转换成slice
```


func (*FlagSet) Visit
```golang
func (f *FlagSet) Visit(fn func(*Flag))
	Visit visits the flags in lexicographical order, calling fn for each. It visits only those flags that have been set.
	顺序 访问 命令行flag 的字典, 给每个 flag 调用fn. 只访问有设置的flag
```

func (*FlagSet) VisitAll
```golang
func (f *FlagSet) VisitAll(fn func(*Flag))
	VisitAll visits the flags in lexicographical order, calling fn for each. It visits all flags, even those not set.
	顺序 访问 命令行flag 的字典, 给每个 flag 调用fn. 会访问所有的flag ,即使没有设置
```

type Getter
```golang
type Getter interface {
    Value
    Get() interface{}
}
	Getter is an interface that allows the contents of a Value to be retrieved. 
	It wraps the Value interface, rather than being part of it, because it appeared after Go 1 and its compatibility rules. 
	All Value types provided by this package satisfy the Getter interface.
	Getter 是一个允许检索值内容的 接口
	它包括Value 接口, 而不是它的一部分,因为 要兼容GO 1 的规则
	这个包里的所有Value类型都符合Getter接口
```

type Value
```golang
type Value interface {
    String() string
    Set(string) error
}
	Value is the interface to the dynamic value stored in a flag. (The default value is represented as a string.)
	If a Value has an IsBoolFlag() bool method returning true, 
		the command-line parser makes -name equivalent to -name=true rather than using the next command-line argument.
	flag里存储的动态值接口.(默认值是一个 字符串)
	如果 Value 在IsBoolFlag() 方法里 返回true. 命令行解析 -name 标示 等价于 -name=true  而不是使用下一个命令行参数
```




















