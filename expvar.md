包地址：http://golang.org/pkg/expvar/

Package expvar provides a standardized interface to public variables, such as operation counters in servers. 
It exposes these variables via HTTP at /debug/vars in JSON format.

Operations to set or modify these public variables are atomic.

In addition to adding the HTTP handler, this package registers the following variables:

expvar包给 公开变量提供一个统一的接口,如在服务器上运行的计数器。
通过HTTP在JSON格式里 /debug/vars 暴露这些变量

设置或修改公开变量具有原子性
除了增加HTTP处理程序,这个包注册以下的变量:
```golang
cmdline   os.Args
memstats  runtime.Memstats
```

The package is sometimes only imported for the side effect of registering its HTTP handler and the above variables. 
To use it this way, link this package into your program:
包有时候只导入 注册HTTP处理 和上述的变量
在这种情况下,可以这样在你的程序里引入包:
```golang
import _ "expvar"
```

func Do
```golang
func Do(f func(KeyValue))
	Do calls f for each exported variable. The global variable map is locked during the iteration, but existing entries may be concurrently updated.
	给每个导出变量调用f.全局变量 映射 在迭代的区间是锁的,但是现有的项目可以同时更新
```

func Publish
```golang
func Publish(name string, v Var)
	Publish declares a named exported variable. This should be called from a package's init function when it creates its Vars.
	If the name is already registered then this will log.Panic.
	Publish声明一个可导出变量的名称. 当它创建它的Vars,应该在包的init函数调用
	如果name 已经被注册存在,会产生 log.Panic.
```

type Float
```golang
type Float struct {
    // contains filtered or unexported fields
}
Float is a 64-bit float variable that satisfies the Var interface.
是一个 满足Var 接口的 64位的浮点型变量 
```

func NewFloat
``golang
func NewFloat(name string) *Float
```

func (*Float) Add
``golang
func (v *Float) Add(delta float64)
	Add adds delta to v.
	把delta累加到v
```

func (*Float) Set
``golang
func (v *Float) Set(value float64)
	Set sets v to value.
	把v设置成value
```

func (*Float) String
```golang
func (v *Float) String() string
```

type Func
```golang
type Func func() interface{}
Func implements Var by calling the function and formatting the returned value using JSON.
使用JSON 调用和格式化返回值 来 实现Var
```

func (Func) String
```golang
func (f Func) String() string
```

type Int
```golang
type Int struct {
    // contains filtered or unexported fields
}
Int is a 64-bit integer variable that satisfies the Var interface.
Int是 实现Var 接口的64位 整型
````

func NewInt
```golang
func NewInt(name string) *Int
```

func (*Int) Add
```golang
func (v *Int) Add(delta int64)
```

func (*Int) Set
```golang
func (v *Int) Set(value int64)
```

func (*Int) String
```golang
func (v *Int) String() string
```

type KeyValue
```golang
type KeyValue struct {
    Key   string
    Value Var
}
KeyValue represents a single entry in a Map.
表示map里的单个实体
```

type Map
```golang
type Map struct {
    // contains filtered or unexported fields
}
Map is a string-to-Var map variable that satisfies the Var interface.
map是一个满足 Var接口的  字符串到 Var 映射的变量
```

func NewMap
```golang
func NewMap(name string) *Map
```

func (*Map) Add
```golang
func (v *Map) Add(key string, delta int64)
```

func (*Map) AddFloat
```golang
func (v *Map) AddFloat(key string, delta float64)
	AddFloat adds delta to the *Float value stored under the given map key.
	添加delta到*Float 值, 存储在给定的map值
```


func (*Map) Do
```golang
func (v *Map) Do(f func(KeyValue))
	Do calls f for each entry in the map. The map is locked during the iteration, but existing entries may be concurrently updated.
	给map里的每个实体调用f. 迭代过程中 map是锁的.但是存在的项目也会同时更新
```

func (*Map) Get
```golang
func (v *Map) Get(key string) Var
```

func (*Map) Init
```golang
func (v *Map) Init() *Map
```

func (*Map) Set
```golang
func (v *Map) Set(key string, av Var)
```

func (*Map) String
```golang
func (v *Map) String() string
```


type String
```golang
type String struct {
    // contains filtered or unexported fields
}
String is a string variable, and satisfies the Var interface.
是一个满足Var接口的字符串变量
```

func NewString
```golang
func NewString(name string) *String
```

func (*String) Set
```golang
func (v *String) Set(value string)
```

func (*String) String
```golang
func (v *String) String() string
```

type Var
```golang
type Var interface {
    String() string
}
Var is an abstract type for all exported variables.
Var是所有可导出变量的抽象类型
```

func Get
```golang
func Get(name string) Var
	Get retrieves a named exported variable.
	在导出的变量中检索一个 名字
```























