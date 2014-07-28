Package reflect

Overview ▾

Package reflect implements run-time reflection, allowing a program to manipulate objects with arbitrary types. 
The typical use is to take a value with static type interface{} and extract its dynamic type information by calling TypeOf, which returns a Type.

A call to ValueOf returns a Value representing the run-time data. Zero takes a Type and returns a Value representing a zero value for that type.

See "The Laws of Reflection" for an introduction to reflection in Go: http://golang.org/doc/articles/laws_of_reflection.html

reflect包实现运行时反射,允许一个程序操作任意类型的对象.
通常用来获取一个值的静态类型 interface{} 和通过调用TypeOf来获取它的动态类型 , 返回一个Type.

调用ValueOf 返回 Value 表示运行时的数据.Type 获取 零并返回一个Value 表示 那个类型时零值.
查看 "反射定律" 来了解GO里的反射:http://golang.org/doc/articles/laws_of_reflection.html


Constants

const (
        SelectSend    // case Chan <- Send
        SelectRecv    // case <-Chan:
        SelectDefault // default
)



func Copy
```golang
func Copy(dst, src Value) int
```
Copy copies the contents of src into dst until either dst has been filled or src has been exhausted. 
It returns the number of elements copied. Dst and src each must have kind Slice or Array, and dst and src must have the same element type.
Copy 复制src的内容到dst 直到 dst满了 或 src 空了.
它返回复制的元素 数量 . dst和src 每个必须有 类型 Slice 或 数组,并且 dst和src必须有相同的元素类型.



func DeepEqual
```golang
func DeepEqual(a1, a2 interface{}) bool
```
DeepEqual tests for deep equality. It uses normal == equality where possible but will scan elements of arrays, slices, maps, and fields of structs. 
In maps, keys are compared with == but elements use deep equality. DeepEqual correctly handles recursive types. Functions are equal only if they are both nil. 
An empty slice is not equal to a nil slice.
DeepEqual 测试深平等.在可能的情况下,它使用 正常的 == 比较 .但是将会 扫描 元素  arrays, slices, maps, 和 structs字段.
在map, 键 使用== 比较,但是元素使用深平等.DeepEqual 正确处理递归类型. 只有两个都是nil, 函数会相等. 空的slice 不等于 nil 的slice.



func Select
```golang
func Select(cases []SelectCase) (chosen int, recv Value, recvOK bool)
```
Select executes a select operation described by the list of cases. 
Like the Go select statement, it blocks until at least one of the cases can proceed, makes a uniform pseudo-random choice, and then executes that case. 
It returns the index of the chosen case and, if that case was a receive operation, 
	the value received and a boolean indicating whether the value corresponds to a send on the channel (as opposed to a zero value received because the channel is closed).

Select执行一个 select 操作 通过cases的列表描述.
类似GO select 声明, 它阻塞直到至少有一个case可以继续,做一个统一的伪随机chosen，然后执行那个case.
它返回chosen case的索引, 如果case 有一个接收操作,接收值和一个布尔说明 是否该值对应于一个发送信道的(相对于接收到的零值，因为信道被关闭)。



type ChanDir
```golang
type ChanDir int
```

ChanDir represents a channel type's direction.
```golang
const (
        RecvDir ChanDir             = 1 << iota // <-chan
        SendDir                                 // chan<-
        BothDir = RecvDir | SendDir             // chan
)
```


func (ChanDir) String
```golang
func (d ChanDir) String() string
```


type Kind
```golang
type Kind uint
```
A Kind represents the specific kind of type that a Type represents. The zero Kind is not a valid kind.
Kind表示 具体是哪种类型,一个Type的表示。零 Kind不是一个有效的kind。



```golang
const (
        Invalid Kind = iota
        Bool
        Int
        Int8
        Int16
        Int32
        Int64
        Uint
        Uint8
        Uint16
        Uint32
        Uint64
        Uintptr
        Float32
        Float64
        Complex64
        Complex128
        Array
        Chan
        Func
        Interface
        Map
        Ptr
        Slice
        String
        Struct
        UnsafePointer
)
```


func (Kind) String
```golang
func (k Kind) String() string
```


type Method
```golang
type Method struct {
        // Name is the method name.
        // PkgPath is the package path that qualifies a lower case (unexported)
        // method name.  It is empty for upper case (exported) method names.
        // The combination of PkgPath and Name uniquely identifies a method
        // in a method set.
        // See http://golang.org/ref/spec#Uniqueness_of_identifiers
        //Name是方法的名称。
        //PkgPath是该包的路径，它限定小写字母方法名（不可导出）。
           //大写方法名是空的（可导出）
           //组合PkgPath和Name成一个方法在方法集里的唯一标示。
           //查看http://golang.org/ref/spec#Uniqueness_of_identifiers
        
        Name    string
        PkgPath string

        Type  Type  // method type
        Func  Value // func with receiver as first argument
        Index int   // index for Type.Method
}
```
Method represents a single method.
Method表示单个方法。



type SelectCase
```golang
type SelectCase struct {
        Dir  SelectDir // direction of case
        Chan Value     // channel to use (for send or receive)
        Send Value     // value to send (for send)
}
```
A SelectCase describes a single case in a select operation. The kind of case depends on Dir, the communication direction.

If Dir is SelectDefault, the case represents a default case. Chan and Send must be zero Values.

If Dir is SelectSend, the case represents a send operation. 
Normally Chan's underlying value must be a channel, and Send's underlying value must be assignable to the channel's element type. 
As a special case, if Chan is a zero Value, then the case is ignored, and the field Send will also be ignored and may be either zero or non-zero.

If Dir is SelectRecv, the case represents a receive operation. Normally Chan's underlying value must be a channel and Send must be a zero Value. 
If Chan is a zero Value, then the case is ignored, but Send must still be a zero Value. When a receive operation is selected, the received Value is returned by Select.

SelectCase描述在一个select操作中的单个例子。那种情况通信方向取决于Dir。
如果Dir是SelectDefault，case表示一个默认的case。Chan和 Send 必须是零值。
如果Dir是SelectSend，case 表示一个发送操作。通常Chan的底层值必须是一个 channel， 并且 Send的底层值必须对应channel的元素类型。
一个特殊的例子，如果Chan是一个零值， 该case被忽略，并且 该字段Send也将被忽略 并可能是零或非零。
如果Dir是SelectRecv，case表示一个接收操作。通常 Chan的底层值必须是一个channel并且Send 必须是一个零值。
如果Chan是零值， case被忽略， 但是 Send 必须 仍然是 零值。当接收操作是selected，接收 Value 通过 Select返回。



type SelectDir
```golang
type SelectDir int
```
A SelectDir describes the communication direction of a select case.
SelectDir描述 一个select case 的通信方向。


type SliceHeader
```golang
type SliceHeader struct {
        Data uintptr
        Len  int
        Cap  int
}
```
SliceHeader is the runtime representation of a slice. 
It cannot be used safely or portably and its representation may change in a later release. 
Moreover, the Data field is not sufficient to guarantee the data it references will not be garbage collected, 
	so programs must keep a separate, correctly typed pointer to the underlying data.

SliceHeader是运行时的slice表示。
它不能被安全地或携带使用并且它代表的可能会在以后的版本更改。
而且，Data字段不足以保证数据的引用不会被垃圾收集，这样的程序必须保持一个独立的，正确键入指向基础数据。



type StructField
```golang
type StructField struct {
        // Name is the field name.
        // PkgPath is the package path that qualifies a lower case (unexported)
        // field name.  It is empty for upper case (exported) field names.
        // See http://golang.org/ref/spec#Uniqueness_of_identifiers
        //Name是字段的名称。
        //PkgPath是该包的路径，它限定小写字母方法名（不可导出）。
           //大写方法名是空的（可导出）
           //查看  http://golang.org/ref/spec#Uniqueness_of_identifiers
           
        Name    string
        PkgPath string

        Type      Type      // field type
        Tag       StructTag // field tag string
        Offset    uintptr   // offset within struct, in bytes
        Index     []int     // index sequence for Type.FieldByIndex
        Anonymous bool      // is an embedded field
}
```
A StructField describes a single field in a struct.
StructField描述一个struct里的单个字段。


type StructTag
```golang
type StructTag string
```
A StructTag is the tag string in a struct field.

By convention, tag strings are a concatenation of optionally space-separated key:"value" pairs. 
Each key is a non-empty string consisting of non-control characters other than space (U+0020 ' '), quote (U+0022 '"'), and colon (U+003A ':'). 
Each value is quoted using U+0022 '"' characters and Go string literal syntax.
StructTag 是struct字段的 标签字符串。
按照惯例，标签字符串 是 任选串联空格分隔 key:"value" 对.
每个key 都是一个非空字符串 由 非控制字符  空格 (U+0020 ' '), 引号 (U+0022 '"'), 和 冒号 (U+003A ':')以外的字符组成. 
每个value 都使用引号 U+0022 '"' 字符 和GO 字符串文本语法引用。


▹ Example
```golang
package main

import (
	"fmt"
	"reflect"
)

func main() {
	type S struct {
		F string `species:"gopher" color:"blue"`
	}

	s := S{}
	st := reflect.TypeOf(s)
	field := st.Field(0)
	fmt.Println(field.Tag.Get("color"), field.Tag.Get("species"))

}
```


func (StructTag) Get
```golang
func (tag StructTag) Get(key string) string
```
Get returns the value associated with key in the tag string. 
If there is no such key in the tag, Get returns the empty string. 
If the tag does not have the conventional format, the value returned by Get is unspecified.
Get返回在tag字符串里 key对应的值。
如果tag里没有这个key，Get 返回空字符串。
如果tag没有 传统的格式，Get返回的值是未指定。



type Type
```golang
type Type interface {

        // Align returns the alignment in bytes of a value of
        // this type when allocated in memory.
        //Align返回 在内存分配时对齐这种类型的值的字节。
        Align() int


        // FieldAlign returns the alignment in bytes of a value of
        // this type when used as a field in a struct.
           //返回 在struct里使用一个字段时 对齐这种类型的值的字节。
        FieldAlign() int


        // Method returns the i'th method in the type's method set.
        // It panics if i is not in the range [0, NumMethod()).
        //
        // For a non-interface type T or *T, the returned Method's Type and Func
        // fields describe a function whose first argument is the receiver.
        //
        // For an interface type, the returned Method's Type field gives the
        // method signature, without a receiver, and the Func field is nil.
        //Method 返回在该类型 方法集里的第i个方法。
           //如果i没有在 [0, NumMethod())范围内， 它会引发panic
           //对于 非接口类型T或者 *T,返回的Method的Type和Func字段 描述函数接口的第一个参数。
           //对于一个接口类型，返回值的Type字段给该方法签名，没有一个接收器，并且 Func字段是nil。
        Method(int) Method


        // MethodByName returns the method with that name in the type's
        // method set and a boolean indicating if the method was found.
        //
        // For a non-interface type T or *T, the returned Method's Type and Func
        // fields describe a function whose first argument is the receiver.
        //
        // For an interface type, the returned Method's Type field gives the
        // method signature, without a receiver, and the Func field is nil.
        
        MethodByName(string) (Method, bool)



        // NumMethod returns the number of methods in the type's method set.
        NumMethod() int

        // Name returns the type's name within its package.
        // It returns an empty string for unnamed types.
        Name() string

        // PkgPath returns a named type's package path, that is, the import path
        // that uniquely identifies the package, such as "encoding/base64".
        // If the type was predeclared (string, error) or unnamed (*T, struct{}, []int),
        // the package path will be the empty string.
        PkgPath() string

        // Size returns the number of bytes needed to store
        // a value of the given type; it is analogous to unsafe.Sizeof.
        Size() uintptr

        // String returns a string representation of the type.
        // The string representation may use shortened package names
        // (e.g., base64 instead of "encoding/base64") and is not
        // guaranteed to be unique among types.  To test for equality,
        // compare the Types directly.
        String() string

        // Kind returns the specific kind of this type.
        Kind() Kind

        // Implements returns true if the type implements the interface type u.
        Implements(u Type) bool

        // AssignableTo returns true if a value of the type is assignable to type u.
        AssignableTo(u Type) bool

        // ConvertibleTo returns true if a value of the type is convertible to type u.
        ConvertibleTo(u Type) bool

        // Bits returns the size of the type in bits.
        // It panics if the type's Kind is not one of the
        // sized or unsized Int, Uint, Float, or Complex kinds.
        Bits() int

        // ChanDir returns a channel type's direction.
        // It panics if the type's Kind is not Chan.
        ChanDir() ChanDir

        // IsVariadic returns true if a function type's final input parameter
        // is a "..." parameter.  If so, t.In(t.NumIn() - 1) returns the parameter's
        // implicit actual type []T.
        //
        // For concreteness, if t represents func(x int, y ... float64), then
        //
        //	t.NumIn() == 2
        //	t.In(0) is the reflect.Type for "int"
        //	t.In(1) is the reflect.Type for "[]float64"
        //	t.IsVariadic() == true
        //
        // IsVariadic panics if the type's Kind is not Func.
        IsVariadic() bool

        // Elem returns a type's element type.
        // It panics if the type's Kind is not Array, Chan, Map, Ptr, or Slice.
        Elem() Type

        // Field returns a struct type's i'th field.
        // It panics if the type's Kind is not Struct.
        // It panics if i is not in the range [0, NumField()).
        Field(i int) StructField

        // FieldByIndex returns the nested field corresponding
        // to the index sequence.  It is equivalent to calling Field
        // successively for each index i.
        // It panics if the type's Kind is not Struct.
        FieldByIndex(index []int) StructField

        // FieldByName returns the struct field with the given name
        // and a boolean indicating if the field was found.
        FieldByName(name string) (StructField, bool)

        // FieldByNameFunc returns the first struct field with a name
        // that satisfies the match function and a boolean indicating if
        // the field was found.
        FieldByNameFunc(match func(string) bool) (StructField, bool)

        // In returns the type of a function type's i'th input parameter.
        // It panics if the type's Kind is not Func.
        // It panics if i is not in the range [0, NumIn()).
        In(i int) Type

        // Key returns a map type's key type.
        // It panics if the type's Kind is not Map.
        Key() Type

        // Len returns an array type's length.
        // It panics if the type's Kind is not Array.
        Len() int

        // NumField returns a struct type's field count.
        // It panics if the type's Kind is not Struct.
        NumField() int

        // NumIn returns a function type's input parameter count.
        // It panics if the type's Kind is not Func.
        NumIn() int

        // NumOut returns a function type's output parameter count.
        // It panics if the type's Kind is not Func.
        NumOut() int

        // Out returns the type of a function type's i'th output parameter.
        // It panics if the type's Kind is not Func.
        // It panics if i is not in the range [0, NumOut()).
        Out(i int) Type
        // contains filtered or unexported methods
}
```
Type is the representation of a Go type.

Not all methods apply to all kinds of types. Restrictions, if any, are noted in the documentation for each method. Use the Kind method to find out the kind of type before calling kind-specific methods. Calling a method inappropriate to the kind of type causes a run-time panic.


















































