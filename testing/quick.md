Package quick

Overview ▾

Package quick implements utility functions to help with black box testing.
quick包实现实用工具函数来帮助 黑盒测试.


func Check
```golang
func Check(function interface{}, config *Config) (err error)
```
Check looks for an input to f, any function that returns bool, such that f returns false. It calls f repeatedly, with arbitrary values for each argument. 
If f returns false on a given input, Check returns that input as a *CheckError. For example:
Check 查看 出入到f, 任何函数 返回 布尔, 即使f 返回错误. 它对每个参数 任意值 反复 调用f. 如果在给定的输入上 f返回false,Check 返回那个输入 *CheckError. 例如:

```golang
func TestOddMultipleOfThree(t *testing.T) {
	f := func(x int) bool {
		y := OddMultipleOfThree(x)
		return y%2 == 1 && y%3 == 0
	}
	if err := quick.Check(f, nil); err != nil {
		t.Error(err)
	}
}
```


func CheckEqual
```golang
func CheckEqual(f, g interface{}, config *Config) (err error)
```
CheckEqual looks for an input on which f and g return different results. It calls f and g repeatedly with arbitrary values for each argument. 
If f and g return different answers, CheckEqual returns a *CheckEqualError describing the input and the outputs.
CheckEqual 查看  在f上的输入 和返回不同的结果 g. 它对每个参数 任意值 反复 调用f和g.
如果f 和g 返回不同答案, CheckEqual 返回 *CheckEqualError 描述输入 并输出.



func Value
```golang
func Value(t reflect.Type, rand *rand.Rand) (value reflect.Value, ok bool)
```
Value returns an arbitrary value of the given type. If the type implements the Generator interface, that will be used. 
Note: To create arbitrary values for structs, all the fields must be exported.
Value 给定的类型返回 任意值. 如果 该类型实现Generator 接口, 将被使用。
注: 为 结构 创建任意值, 所有字段必须可导出.


type CheckEqualError
```golang
type CheckEqualError struct {
        CheckError
        Out1 []interface{}
        Out2 []interface{}
}
```
A CheckEqualError is the result CheckEqual finding an error.
CheckEqualError 是 CheckEqual 发现的错误 的 结果.



func (*CheckEqualError) Error
```golang
func (s *CheckEqualError) Error() string
```


type CheckError
```golang
type CheckError struct {
        Count int
        In    []interface{}
}
```
A CheckError is the result of Check finding an error.
CheckError是 Check 发现的错误 的 结果.


func (*CheckError) Error
```golang
func (s *CheckError) Error() string
```


type Config
```golang
type Config struct {
        // MaxCount sets the maximum number of iterations. If zero,
        // MaxCountScale is used.
        MaxCount int
        // MaxCountScale is a non-negative scale factor applied to the default
        // maximum. If zero, the default is unchanged.
        MaxCountScale float64
        // If non-nil, rand is a source of random numbers. Otherwise a default
        // pseudo-random source will be used.
        Rand *rand.Rand
        // If non-nil, the Values function generates a slice of arbitrary
        // reflect.Values that are congruent with the arguments to the function
        // being tested. Otherwise, the top-level Values function is used
        // to generate them.
        Values func([]reflect.Value, *rand.Rand)
}
```
A Config structure contains options for running a test.
Config 结构体 包含执行测试 的选项.


type Generator
```golang
type Generator interface {
        // Generate returns a random instance of the type on which it is a
        // method using the size as a size hint.
        Generate(rand *rand.Rand, size int) reflect.Value
}
```
A Generator can generate random values of its own type.
Generator 生成 它自己类型的随机值.


type SetupError
```golang
type SetupError string
```
A SetupError is the result of an error in the way that check is being used, independent of the functions being tested.
SetupError 在方法中的错误的检查正在使用的结果 ,独立的函数测试。


func (SetupError) Error
```golang
func (s SetupError) Error() string
```










