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
        //MethodByName返回在 type 的方法集里的那个name的方法 ，如果该方法被找到，用一个布尔值说明。
           //对于一个 非接口 类型 T 或 *T,返回 Method的Type和Func字段描述其第一个参数是接收器的功能。
           //对于一个接口类型，返回的Method的 Type 字段 给出该方法签名，没有一个接收器，并使用FUNC字段是零。
        
        MethodByName(string) (Method, bool)



        // NumMethod returns the number of methods in the type's method set.
        //NumMethod 返回 在type的方法集的方法数。
        NumMethod() int



        // Name returns the type's name within its package.
        // It returns an empty string for unnamed types.
        // Name 返回该包里的 type的 名字
       	   // 它为未命名的type 返回一个空字符串
        Name() string



        // PkgPath returns a named type's package path, that is, the import path
        // that uniquely identifies the package, such as "encoding/base64".
        // If the type was predeclared (string, error) or unnamed (*T, struct{}, []int),
        // the package path will be the empty string.
        //PkgPath 返回一个指定的type的包路径，就是说，该包引入路径的唯一标示。例如"encoding/base64".
           //如果type预声明 (string, error)或 未命名 (*T, struct{}, []int),该包路径将是空字符串。
        PkgPath() string

        // Size returns the number of bytes needed to store
        // a value of the given type; it is analogous to unsafe.Sizeof.
        //Size返回给定类型的值需要存储的字节数。它类似unsafe.Sizeof。
        
        Size() uintptr



        // String returns a string representation of the type.
        // The string representation may use shortened package names
        // (e.g., base64 instead of "encoding/base64") and is not
        // guaranteed to be unique among types.  To test for equality,
        // compare the Types directly.
        //String 返回一个type的字符串表示。
           //该字符串表示可能使用缩短包名（例如，用base64代替 "encoding/base64"）并且它不能保证 在type中是唯一的。
            //为了测试是否相等，直接比较Type
        
        String() string



        // Kind returns the specific kind of this type.
        //Kind 返回具体是哪种这种类型。
        Kind() Kind



        // Implements returns true if the type implements the interface type u.
        //Implements 如果 该type  实现 接口type u 返回 true
        Implements(u Type) bool


        // AssignableTo returns true if a value of the type is assignable to type u.
        //AssignableTo 如果一个type值 分配给  type u ,返回true
        AssignableTo(u Type) bool



        // ConvertibleTo returns true if a value of the type is convertible to type u.
        //ConvertibleTo 如果type值转换成 type u,返回true
        ConvertibleTo(u Type) bool



        // Bits returns the size of the type in bits.
        // It panics if the type's Kind is not one of the
        // sized or unsized Int, Uint, Float, or Complex kinds.
        //Bits 返回bits里的type大小。
           //如果type的Kind 不是sized 或unsized Int, Uint, Float, or Complex 类型， 它引发panic。
        Bits() int




        // ChanDir returns a channel type's direction.
        // It panics if the type's Kind is not Chan.
        //ChanDir返回一个 channel type 的方向。
           //如果type的Kind 不是Chan，它引发panic。
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
        //IsVariadic  如果函数 type的最后输入参数是"..." ， 返回true.
           //如果是那样，t.In(t.NumIn() - 1) 返回parameter隐含的实际类型 []T.
           //对于具体，如果t表示func(x int, y ... float64)，那
        //	t.NumIn() == 2
        //	t.In(0) is the reflect.Type for "int"
        //	t.In(1) is the reflect.Type for "[]float64"
        //	t.IsVariadic() == true
        
        IsVariadic() bool



        // Elem returns a type's element type.
        // It panics if the type's Kind is not Array, Chan, Map, Ptr, or Slice.
        //Elem 返回一个 type的元素类型。
        //如果type的Kind 不是Array, Chan, Map, Ptr, 或 Slice，它引发panic 
        Elem() Type




        // Field returns a struct type's i'th field.
        // It panics if the type's Kind is not Struct.
        // It panics if i is not in the range [0, NumField()).
        //Field 返回一个struct type的 第i个字段。
          //如果type的Kind 不是Struct 引发panic。
        //如果i不在 [0, NumField()) 范围，它引发panic
        
        Field(i int) StructField



        // FieldByIndex returns the nested field corresponding
        // to the index sequence.  It is equivalent to calling Field
        // successively for each index i.
        // It panics if the type's Kind is not Struct.
        //FieldByIndex 返回嵌套字段对应的索引顺序。
   		   //它相当于为每个索引i调用Field。
   		   //如果type的Kind不是Struct ，它引发panic
        
        FieldByIndex(index []int) StructField



        // FieldByName returns the struct field with the given name
        // and a boolean indicating if the field was found.
        //FieldByName 用给定的name返回struct 字段并且如果发现该字段，用一个布尔值说明。
        FieldByName(name string) (StructField, bool)



        // FieldByNameFunc returns the first struct field with a name
        // that satisfies the match function and a boolean indicating if
        // the field was found.
		  //FieldByNameFunc 返回 满足匹配函数的 name的 第一个struct字段 并且如果发现该字段，用一个布尔值说明
        
        FieldByNameFunc(match func(string) bool) (StructField, bool)



        // In returns the type of a function type's i'th input parameter.
        // It panics if the type's Kind is not Func.
        // It panics if i is not in the range [0, NumIn()).
           //In返回 type函数 type的第i个输入参数。
           //如果 type不是 Func ，引发panic，
           //如果i不在[0, NumIn())范围内，引发panic。
        In(i int) Type

        
        
        // Key returns a map type's key type.
        // It panics if the type's Kind is not Map.
        //Key返回 map type的key类型。
           //如果type的Kind不是Map，引发panic
        Key() Type



        // Len returns an array type's length.
        // It panics if the type's Kind is not Array.
        //Len返回 array type的长度。
        //如果type的Kind不是Array ，引发panic
        Len() int



        // NumField returns a struct type's field count.
        // It panics if the type's Kind is not Struct.
        //NumField 返回一个struct type的字段总数。
           //如果type的 Kind不是Struct，引发panic。
           
        NumField() int



        // NumIn returns a function type's input parameter count.
        // It panics if the type's Kind is not Func.
        //NumIn 返回一个函数 type的输入参数总数。
		  //如果type Kind不是 Func ，引发panic
		  
        NumIn() int
		  


        // NumOut returns a function type's output parameter count.
        // It panics if the type's Kind is not Func.
        //NumOut返回一个函数 type 的输出参数总数
           //如果type Kind不是 Func ，引发panic
           
        NumOut() int



        // Out returns the type of a function type's i'th output parameter.
        // It panics if the type's Kind is not Func.
        // It panics if i is not in the range [0, NumOut()).
        //Out返回 type参数  type的 第i个输出参数。
          //如果type 的 Kind  不是 Func，引发panic。
          //如果i不在 [0, NumOut())范围内 引发panic。
        Out(i int) Type
        // contains filtered or unexported methods
}
```
Type is the representation of a Go type.

Not all methods apply to all kinds of types.
Restrictions, if any, are noted in the documentation for each method. 
Use the Kind method to find out the kind of type before calling kind-specific methods. 
Calling a method inappropriate to the kind of type causes a run-time panic.
Type表示一个GO的类型。
并不是所有的方法适用于所有类型的类型。
如果有任何限制，备注在每个方法的文档里。
使用Kind方法 在调用 那种特有的方法之前 去找出 哪种类型的
不恰当的调用方法的 那种类型 会导致 运行时panic。



func ChanOf
```golang
func ChanOf(dir ChanDir, t Type) Type
```
ChanOf returns the channel type with the given direction and element type. 
For example, if t represents int, ChanOf(RecvDir, t) represents <-chan int.

The gc runtime imposes a limit of 64 kB on channel element types. If t's size is equal to or exceeds this limit, ChanOf panics.

ChanOf 用给定的 方向 和元素类型返回 该channel 类型。
例如， 如果t表示int， ChanOf(RecvDir, t) 表示  <-chan int。

运行时 gc规定 在channel 的元素类型 限制64KB。如果t的大小等于或大于这个限制，ChanOf 引发 panic。



func MapOf
```golang
func MapOf(key, elem Type) Type
```
MapOf returns the map type with the given key and element types. 
For example, if k represents int and e represents string, MapOf(k, e) represents map[int]string.

If the key type is not a valid map key type (that is, if it does not implement Go's == operator), MapOf panics.
MapOf 用给定的key和元素类型 返回 map的类型。
例如，如果k表示int 并且e表示 string， MapOf(k, e) 表示 map[int]string.

如果key类型不是一个有效的map key 类型（就是说，如果它没有实现 GO的== 操作）， MapOf引发panic



func PtrTo
```golang
func PtrTo(t Type) Type
```
PtrTo returns the pointer type with element t. For example, if t represents type Foo, PtrTo(t) represents *Foo.
PtrTo 根据t 返回 指针类型。例如 如果t表示Foo类型， PtrTo(t)表示 *Foo



func SliceOf
```golang
func SliceOf(t Type) Type
```
SliceOf returns the slice type with element type t. For example, if t represents int, SliceOf(t) represents []int.
SliceOf 根据元素类型 t 返回 slice 类型。 例如 ， 如果t表示int,SliceOf(t)表示  []int.



func TypeOf
```golang
func TypeOf(i interface{}) Type
```
TypeOf returns the reflection Type of the value in the interface{}. TypeOf(nil) returns nil.
TypeOf 在TypeOf里的值的 反射 Type。TypeOf(nil) 返回nil




type Value

type Value struct {
        // contains filtered or unexported fields
}
Value is the reflection interface to a Go value.

Not all methods apply to all kinds of values. Restrictions, if any, are noted in the documentation for each method. 
Use the Kind method to find out the kind of value before calling kind-specific methods. 
Calling a method inappropriate to the kind of type causes a run time panic.

The zero Value represents no value. Its IsValid method returns false, its Kind method returns Invalid, its String method returns "<invalid Value>", and all other methods panic. 
Most functions and methods never return an invalid value. If one does, its documentation states the conditions explicitly.

A Value can be used concurrently by multiple goroutines provided that the underlying Go value can be used concurrently for the equivalent direct operations.

Value是GO值的反射接口。
并不是所有的方法适用于所有类型的类型。
如果有任何限制，备注在每个方法的文档里。
使用Kind方法 在调用 那种特有的方法之前 去找出 哪种类型的
不恰当的调用方法的 那种类型 会导致 运行时panic。

零Value表示没有值。 它的IsValid方法返回false，它的Kind 方法 返回 Invalid，它的String 方法返回 "<invalid Value>"，所有其他方法 引发panic。
大部分函数和方法不会返回一个无效的值。如果有，它的文档明确规定的条件。

Value可以同时 通过多个goroutines 提供，那样 底层GO值 可以相当于同时操作。




func Append
```golang
func Append(s Value, x ...Value) Value
```
Append appends the values x to a slice s and returns the resulting slice. As in Go, each x's value must be assignable to the slice's element type.
Append追加值x到 slice s 并返回结果的 slice。正如在GO里的，每个x的值必须分配到slice的元素类型。



func AppendSlice
```golang
func AppendSlice(s, t Value) Value
```
AppendSlice appends a slice t to a slice s and returns the resulting slice. The slices s and t must have the same element type.
AppendSlice 追加一个slicet到 slice  s 并返回结果slice。slice s 和t 必须有相同的元素类型。



func Indirect
```golang
func Indirect(v Value) Value
```
Indirect returns the value that v points to. If v is a nil pointer, Indirect returns a zero Value. If v is not a pointer, Indirect returns v.
Indirect 返回 v 指向的值。如果v是一个 nil 指针，间接返回零Value。 如果v不是一个指针，间接返回v。



func MakeChan
```golang
func MakeChan(typ Type, buffer int) Value
```
MakeChan creates a new channel with the specified type and buffer size.
MakeChan用指定的类型和缓冲区大小 创建一个新的channel



func MakeFunc
```golang
func MakeFunc(typ Type, fn func(args []Value) (results []Value)) Value
```
MakeFunc returns a new function of the given Type that wraps the function fn. When called, that new function does the following:
MakeFunc 用给定的Type 封装函数fn ，返回一个新的函数。 当调用时，新函数跟着：

```golang
- converts its arguments to a slice of Values.
- runs results := fn(args).
- returns the results as a slice of Values, one per formal result.

- 转换它的参数成一个 slice Values。
- 执行结果 := fn(args)。
- 返回结果 slice Values，每个正式的结果之一。

```
The implementation fn can assume that the argument Value slice has the number and type of arguments given by typ. 
If typ describes a variadic function, the final Value is itself a slice representing the variadic arguments, as in the body of a variadic function. 
The result Value slice returned by fn must have the number and type of results given by typ.

The Value.Call method allows the caller to invoke a typed function in terms of Values; in contrast, MakeFunc allows the caller to implement a typed function in terms of Values.

The Examples section of the documentation includes an illustration of how to use MakeFunc to build a swap function for different types.
实现fn可以假设 Value slice 通过typ 给定数字和参数类型。
如果typ描述可变参数函数，最后Value是 自己 一个slice 表示 可变参数，如在一个可变参数函数体内。
结果 Value slice 通过fn返回， 必须 是通过typ给定数字和结果类型。

Value.Call方法允许 调用者 调用 一个 在Values里的 类型方法;相反，MakeFunc允许调用者 在Values实现一个类型方法。
该节文档的例子 一个插图 来说 怎么使用MakeFunc 为不同的类型构建一个交互方法。


▹ Example
```golang
package main

import (
	"fmt"
	"reflect"
)

func main() {
	// swap is the implementation passed to MakeFunc.
	// It must work in terms of reflect.Values so that it is possible
	// to write code without knowing beforehand what the types
	// will be.
	swap := func(in []reflect.Value) []reflect.Value {
		return []reflect.Value{in[1], in[0]}
	}

	// makeSwap expects fptr to be a pointer to a nil function.
	// It sets that pointer to a new function created with MakeFunc.
	// When the function is invoked, reflect turns the arguments
	// into Values, calls swap, and then turns swap's result slice
	// into the values returned by the new function.
	makeSwap := func(fptr interface{}) {
		// fptr is a pointer to a function.
		// Obtain the function value itself (likely nil) as a reflect.Value
		// so that we can query its type and then set the value.
		fn := reflect.ValueOf(fptr).Elem()

		// Make a function of the right type.
		v := reflect.MakeFunc(fn.Type(), swap)

		// Assign it to the value fn represents.
		fn.Set(v)
	}

	// Make and call a swap function for ints.
	var intSwap func(int, int) (int, int)
	makeSwap(&intSwap)
	fmt.Println(intSwap(0, 1))

	// Make and call a swap function for float64s.
	var floatSwap func(float64, float64) (float64, float64)
	makeSwap(&floatSwap)
	fmt.Println(floatSwap(2.72, 3.14))

}
```


func MakeMap
```golang
func MakeMap(typ Type) Value
```
MakeMap creates a new map of the specified type.
MakeMap用指定的类型创建一个新的map


func MakeSlice
```golang
func MakeSlice(typ Type, len, cap int) Value
```
MakeSlice creates a new zero-initialized slice value for the specified slice type, length, and capacity.
MakeSlice 用指定的slice 类型， 长度和容量 创建一个 零初始化的slice 值。



func New
```golang
func New(typ Type) Value
```
New returns a Value representing a pointer to a new zero value for the specified type. That is, the returned Value's Type is PtrTo(typ).
New 用指定的类型 返回一个Value 表示一个指向新零值。 就是说，返回Value的Type 是PtrTo(typ).



func NewAt
```golang
func NewAt(typ Type, p unsafe.Pointer) Value
```
NewAt returns a Value representing a pointer to a value of the specified type, using p as that pointer.
NewAt 用指定的类型 返回一个Value 表示一个指向新零值，以p作为指针。



func ValueOf
```golang
func ValueOf(i interface{}) Value
```
ValueOf returns a new Value initialized to the concrete value stored in the interface i. ValueOf(nil) returns the zero Value.
ValueOf 返回一个新的Value ，初始化成 存储在接口i里的具体值。 ValueOf(nil) 返回 零Value



func Zero
```golang
func Zero(typ Type) Value
```
Zero returns a Value representing the zero value for the specified type. 
The result is different from the zero value of the Value struct, which represents no value at all. 
For example, Zero(TypeOf(42)) returns a Value with Kind Int and value 0. The returned value is neither addressable nor settable.
Zero 用指定的类型返回一个Value 表示零值
结果和Value结构体的零值不一样，这表示什么值都没有。
例如，Zero(TypeOf(42)) 返回Kind Int 和值0的 Value。返回值 既不寻址也可设置。



func (Value) Addr
```golang
func (v Value) Addr() Value
```
Addr returns a pointer value representing the address of v. 
It panics if CanAddr() returns false. 
Addr is typically used to obtain a pointer to a struct field or slice element in order to call a method that requires a pointer receiver.
Addr 返回一个指针值 表示v的地址。
如果CanAddr()  返回false ，它引发panic。
Addr通常用来获得 struct 字段 或slice元素 的指针 ,为了调用方法  需要一个指针接收器。



func (Value) Bool
```golang
func (v Value) Bool() bool
```
Bool returns v's underlying value. It panics if v's kind is not Bool.
Bool 返回v的底层值。如果v的种类不是布尔值，引发panic


func (Value) Bytes
```golang
func (v Value) Bytes() []byte
```
Bytes returns v's underlying value. It panics if v's underlying value is not a slice of bytes.
Bytes 返回v的底层值。如果v的底层值不是一个 bytes slice ，引发panic


func (Value) Call
```golang
func (v Value) Call(in []Value) []Value
```
Call calls the function v with the input arguments in.
For example, if len(in) == 3, v.Call(in) represents the Go call v(in[0], in[1], in[2]).
Call panics if v's Kind is not Func. It returns the output results as Values. 
As in Go, each input argument must be assignable to the type of the function's corresponding input parameter. 
If v is a variadic function, Call creates the variadic slice parameter itself, copying in the corresponding values.
Call 用in 的输入参数 调用函数v
例如，如果 len(in) == 3, v.Call(in) 表示 GO调用 v(in[0], in[1], in[2]).
如果v的Kind 不是Func，Call 引发panic。它返回输出结果Values。
正如在GO里，每个输入参数必须分配给 函数对应参数类型。
如果v是一个可变参函数，Call创建可变slice 参数本身，复制相应的值。



func (Value) CallSlice
```golang
func (v Value) CallSlice(in []Value) []Value
```
CallSlice calls the variadic function v with the input arguments in, assigning the slice in[len(in)-1] to v's final variadic argument. 
For example, if len(in) == 3, v.Call(in) represents the Go call v(in[0], in[1], in[2]...). 
Call panics if v's Kind is not Func or if v is not variadic. It returns the output results as Values. 
As in Go, each input argument must be assignable to the type of the function's corresponding input parameter.
CallSlice用in的输入参数 调用可变参函数v，分配slice  in[len(in)-1] 给v的最后可变参。
例如，如果len(in) == 3,v.Call(in)表示GO调用v(in[0], in[1], in[2]...). 
如果v的Kind 不是Func或v不是可变参，Call 引发panic。它返回输出结果Values。
正如在GO里，每个输入参数必须分配给 函数对应参数类型。



func (Value) CanAddr
```golang
func (v Value) CanAddr() bool
```
CanAddr returns true if the value's address can be obtained with Addr. 
Such values are called addressable. 
A value is addressable if it is an element of a slice, an element of an addressable array, a field of an addressable struct, or the result of dereferencing a pointer. 
If CanAddr returns false, calling Addr will panic.
CanAddr如果value的地址可以用Addr获取，返回true。
这样的值被称为寻址。
value 如果它是一个slice的元素，一个可寻址的数组元素，以个可寻址struct的字段，一个指针引用的结果，那它是可寻址的。



func (Value) CanInterface
```golang
func (v Value) CanInterface() bool
```
CanInterface returns true if Interface can be used without panicking.
CanInterface 如果Interface可以使用 不会产生 panic，返回true。



func (Value) CanSet
```golang
func (v Value) CanSet() bool
```
CanSet returns true if the value of v can be changed. 
A Value can be changed only if it is addressable and was not obtained by the use of unexported struct fields. 
If CanSet returns false, calling Set or any type-specific setter (e.g., SetBool, SetInt64) will panic.
CanSet如果v的值可以被修改，返回true。
如果它是可寻址的，并没有因使用未导出结构域获得，Value可以被修改
如果CanSet 返回false，调用Set或任何特殊类型设置都会引发panic（比如SetBool, SetInt64）



func (Value) Cap
```golang
func (v Value) Cap() int
```
Cap returns v's capacity. It panics if v's Kind is not Array, Chan, or Slice.
Cap 返回v的容量。如果v的Kind不是 Array, Chan, 或 Slice ，它引发panic。



func (Value) Close
```golang
func (v Value) Close()
```
Close closes the channel v. It panics if v's Kind is not Chan.
Close关闭channel v。如果v的Kind不是 Chan ，引发panic。


func (Value) Complex
```golang
func (v Value) Complex() complex128
```
Complex returns v's underlying value, as a complex128. 
It panics if v's Kind is not Complex64 or Complex128
Complex 返回v的底层值，类似complex128。
如果v的Kind不是Complex64 或 Complex128，它引发panic。



func (Value) Convert
```golang
func (v Value) Convert(t Type) Value
```
Convert returns the value v converted to type t. 
If the usual Go conversion rules do not allow conversion of the value v to type t, Convert panics.
Convert 返回 值 v 转化成类型t。
如果用GO转换规则不允许转换值v成类似t，Convert引发panic。



func (Value) Elem
```golang
func (v Value) Elem() Value
```
Elem returns the value that the interface v contains or that the pointer v points to. 
It panics if v's Kind is not Interface or Ptr. It returns the zero Value if v is nil.
Elem 返回接口v包含的值 或指针v指向的值。
如果v的Kind 不是Interface 或 Ptr，它引发panic。如果v是nil它返回零值。



func (Value) Field
```golang
func (v Value) Field(i int) Value
```
Field returns the i'th field of the struct v. It panics if v's Kind is not Struct or i is out of range.
Field 返回struct v的第i个字段。如果v的Kind不是Struct或i 超出范围，它引发panic。


func (Value) FieldByIndex
```golang
func (v Value) FieldByIndex(index []int) Value
```
FieldByIndex returns the nested field corresponding to index. It panics if v's Kind is not struct.
FieldByIndex 返回嵌套字段对应的索引。如果v的Kind不是struct ，引发panic。



func (Value) FieldByName
```golang
func (v Value) FieldByName(name string) Value
```
FieldByName returns the struct field with the given name. 
It returns the zero Value if no field was found. It panics if v's Kind is not struct.
FieldByName 用给定 name 返回 struct字段。
如果字段没发现 它返回零值。如果v的Kind不是 struct，它引发panic。



func (Value) FieldByNameFunc
```golang
func (v Value) FieldByNameFunc(match func(string) bool) Value
```
FieldByNameFunc returns the struct field with a name that satisfies the match function. 
It panics if v's Kind is not struct. It returns the zero Value if no field was found.
FieldByNameFunc 返回 满足匹配函数的name 的struct 字段
如果v的Kind不是struct ，引发panic。如果字段没发现，返回零Value。



func (Value) Float
```golang
func (v Value) Float() float64
```
Float returns v's underlying value, as a float64. It panics if v's Kind is not Float32 or Float64
Float返回v的底层值，当成float64。 如果v的Kind不是Float32或 Float64，它引发panic。


func (Value) Index
```golang
func (v Value) Index(i int) Value
```
Index returns v's i'th element. It panics if v's Kind is not Array, Slice, or String or i is out of range.
Index返回v的第i个元素。 如果v的Kind不是Array ,Slice, 或 String 或i超出范围 ，引发panic。



func (Value) Int
```golang
func (v Value) Int() int64
```
Int returns v's underlying value, as an int64. It panics if v's Kind is not Int, Int8, Int16, Int32, or Int64.
Int 返回v的底层值，当成int64。如果v的Kind不是 Int, Int8, Int16, Int32, or Int64，它引发panic。



func (Value) Interface
```golang
func (v Value) Interface() (i interface{})
```
Interface returns v's current value as an interface{}. It is equivalent to:
Interface 返回v的当前值 成一个interface{}。它相当于：

```golang
var i interface{} = (v's underlying value)
```

It panics if the Value was obtained by accessing unexported struct fields.
如果Value 包含访问 未导出的struct字段，它引发panic。



func (Value) InterfaceData
```golang
func (v Value) InterfaceData() [2]uintptr
```
InterfaceData returns the interface v's value as a uintptr pair. It panics if v's Kind is not Interface.
InterfaceData返回 接口v的值成一个uintptr对。如果v的Kind不是 Interface ，引发panic。


func (Value) IsNil
```golang
func (v Value) IsNil() bool
```
IsNil reports whether its argument v is nil. The argument must be a chan, func, interface, map, pointer, or slice value; 
if it is not, IsNil panics. Note that IsNil is not always equivalent to a regular comparison with nil in Go. 
For example, if v was created by calling ValueOf with an uninitialized interface variable i, i==nil will be true but v.IsNil will panic as v will be the zero Value.
IsNil报告它的参数v是否是nil。参数必须是chan, func, interface, map, pointer, or slice 值; 
如果它不是，IsNil引发panic。注意，在GO里  IsNil 通常不等价于 正则比较nil。
例子：如果v是通过调用 ValueOf和未初始化接口变量i来创建的，i==nil将是true 但是v.IsNil会引发panic，v将是零值。



func (Value) IsValid
```golang
func (v Value) IsValid() bool
```
IsValid returns true if v represents a value.
It returns false if v is the zero Value. If IsValid returns false, all other methods except String panic. 
Most functions and methods never return an invalid value. 
If one does, its documentation states the conditions explicitly.
IsValid 如果v表示一个值 返回true。
如果v是零Value 返回false。如果IsValid 返回false，所有其他方法除了String 都引发panic。
大部分的函数和方法不会返回无效值。
如果有一个这样，它的文档明确规定条件。



func (Value) Kind
```golang
func (v Value) Kind() Kind
```
Kind returns v's Kind. If v is the zero Value (IsValid returns false), Kind returns Invalid.
Kind返回v的Kind。 如果v是零Value（IsValid 返回 false），Kind 返回 Invalid。



func (Value) Len
```golang
func (v Value) Len() int
```
Len returns v's length. It panics if v's Kind is not Array, Chan, Map, Slice, or String.
Len返回v的长度。如果v的Kind不是Array, Chan, Map, Slice, or String，引发panic。



func (Value) MapIndex
```golang
func (v Value) MapIndex(key Value) Value
```
MapIndex returns the value associated with key in the map v. 
It panics if v's Kind is not Map. It returns the zero Value if key is not found in the map or if v represents a nil map. 
As in Go, the key's value must be assignable to the map's key type.
MapIndex 返回map v里 key对应的值。
如果v的Kind不是map，引发panic。如果key没有在map里发现或 v是一个nil map，它返回零值。
在GO里，key的值必须分配到map的key类型。



func (Value) MapKeys
```golang
func (v Value) MapKeys() []Value
```
MapKeys returns a slice containing all the keys present in the map, in unspecified order. 
It panics if v's Kind is not Map. It returns an empty slice if v represents a nil map.
MapKeys 返回一个slice 包含所有在map里出现的key，不按顺序的。
如果v的Kind不是Map，它引发panic。如果v表示一个nil map ，它返回空slice。
 


func (Value) Method
```golang
func (v Value) Method(i int) Value
```
Method returns a function value corresponding to v's i'th method. 
The arguments to a Call on the returned function should not include a receiver; 
the returned function will always use v as the receiver. 
Method panics if i is out of range or if v is a nil interface value.
Method 返回一个函数值对应的 v的第i个方法。
在返回函数上 Call 参数 不应该包含接收器。
返回函数将通常使用v作为接收器。
如果i超出范围或v是一个nil 接口值，Method引发panic。



func (Value) MethodByName
```golang
func (v Value) MethodByName(name string) Value
```
MethodByName returns a function value corresponding to the method of v with the given name. 
The arguments to a Call on the returned function should not include a receiver; 
the returned function will always use v as the receiver. 
It returns the zero Value if no method was found.
MethodByName 用给定name返回方法v对应的函数值。 
在返回函数上 Call 参数 不应该包含接收器。
返回函数将通常使用v作为接收器。
如果没有发现方法，返回零Value


func (Value) NumField
```golang
func (v Value) NumField() int
```
NumField returns the number of fields in the struct v. It panics if v's Kind is not Struct.
NumField 返回在 struct v里的字段数。 如果v的Kind不是Struct， 引发panic。


func (Value) NumMethod
```golang
func (v Value) NumMethod() int
```
NumMethod returns the number of methods in the value's method set.
NumMethod 返回在value的方法集里的方法数数量。



func (Value) OverflowComplex
```golang
func (v Value) OverflowComplex(x complex128) bool
```
OverflowComplex returns true if the complex128 x cannot be represented by v's type. It panics if v's Kind is not Complex64 or Complex128.
OverflowComplex 如果complex128 x 不能表示 v的类型，返回true。如果v的Kind不是Complex64 或 Complex128，引发panic。



func (Value) OverflowFloat
```golang
func (v Value) OverflowFloat(x float64) bool
```
OverflowFloat returns true if the float64 x cannot be represented by v's type. It panics if v's Kind is not Float32 or Float64.
OverflowFloat 如果float64 x 不能表示 v的类型，返回true。 如果v的Kind不是 Float32 or Float64 ，它引发panic。



func (Value) OverflowInt
```golang
func (v Value) OverflowInt(x int64) bool
```
OverflowInt returns true if the int64 x cannot be represented by v's type. It panics if v's Kind is not Int, Int8, int16, Int32, or Int64.
OverflowInt 如果int64 x 不能表示 v的类型， 返回true。如果v的Kind 不是Int, Int8, int16, Int32, or Int64，它引发panic。



func (Value) OverflowUint
```golang
func (v Value) OverflowUint(x uint64) bool
```
OverflowUint returns true if the uint64 x cannot be represented by v's type. It panics if v's Kind is not Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64.
OverflowUint 如果uint64 x 不能表示 v的类型，返回true。 如果v的Kind不是Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64， 引发panic。



func (Value) Pointer
```golang
func (v Value) Pointer() uintptr
```
Pointer returns v's value as a uintptr. It returns uintptr instead of unsafe.Pointer so that code using reflect cannot obtain unsafe.Pointers without importing the unsafe package explicitly. 
It panics if v's Kind is not Chan, Func, Map, Ptr, Slice, or UnsafePointer.

If v's Kind is Func, the returned pointer is an underlying code pointer, but not necessarily enough to identify a single function uniquely. 
The only guarantee is that the result is zero if and only if v is a nil func Value.

If v's Kind is Slice, the returned pointer is to the first element of the slice. If the slice is nil the returned value is 0. 
If the slice is empty but non-nil the return value is non-zero.

Pointer返回v的值 作为uintptr。它返回 uintptr 代替unsafe.Pointer  所以 代码 使用reflect 而没有引入unsafe包 不能 获取unsafe.Pointers。
如果v的Kind不是  Chan, Func, Map, Ptr, Slice, or UnsafePointer， 引发panic。

如果v的Kind是 Func， 返回的指针是底层代码指针，但是未必足以唯一标识单个函数。
唯一确定的是  如果结果是零 那 v 是一个nil func 值。

如果v的Kind是Slice，返回的指针 指向 slice的第一个元素。如果slice是nil 返回值是 0.
如果slice是空的 但 非nil 返回值是非零。



func (Value) Recv
```golang
func (v Value) Recv() (x Value, ok bool)
```
Recv receives and returns a value from the channel v. 
It panics if v's Kind is not Chan. The receive blocks until a value is ready. 
The boolean value ok is true if the value x corresponds to a send on the channel, false if it is a zero value received because the channel is closed.
Recv 从v中 接收和返回 一个值。
如果v的Kind 不是Chan ，引发panic。 接收阻塞直到 值准备好。
如果值x 对应 channle上的发送，布尔值 ok 是true。 如果它接收一个零值，那是false， 因为channel 是关闭的。



func (Value) Send
```golang
func (v Value) Send(x Value)
```
Send sends x on the channel v. It panics if v's kind is not Chan or if x's type is not the same type as v's element type. 
As in Go, x's value must be assignable to the channel's element type.
Send 在channel v上发送x。 如果v的种类 不是 Chan 或 x的类型和v的元素类型不一样，引发panic。
在GO里，x的值必须分配到对应channel的元素类型。


func (Value) Set
```golang
func (v Value) Set(x Value)
```
Set assigns x to the value v. It panics if CanSet returns false. As in Go, x's value must be assignable to v's type.
Set 分配x到 值v。 如果CanSet 返回false，它引发panic。在GO里，x 的值必须分配到 v的类型。



func (Value) SetBool
```golang
func (v Value) SetBool(x bool)
```
SetBool sets v's underlying value. It panics if v's Kind is not Bool or if CanSet() is false.
SetBool 设置 v的底层值。如果v的Kind不是Bool 或 如果CanSet() 是false，它引发panic。



func (Value) SetBytes
```golang
func (v Value) SetBytes(x []byte)
```
SetBytes sets v's underlying value. It panics if v's underlying value is not a slice of bytes.
SetBytes 设置v的底层值。 如果v的底层值不是一个bytes slice，它引发panic。



func (Value) SetCap
```golang
func (v Value) SetCap(n int)
```
SetCap sets v's capacity to n. It panics if v's Kind is not Slice or if n is smaller than the length or greater than the capacity of the slice.
SetCap 设置v的容量成n。 如果v的Kind 不是Slice或 如果n小于长度 或大于slice的容量，它引发panic。


func (Value) SetComplex
```golang
func (v Value) SetComplex(x complex128)
```
SetComplex sets v's underlying value to x. It panics if v's Kind is not Complex64 or Complex128, or if CanSet() is false.
SetComplex 设置v的底层值 成x。 如果v的Kind不是Complex64 or Complex128,  或 如果CanSet() 是false，它引发panic。



func (Value) SetFloat
```golang
func (v Value) SetFloat(x float64)
```
SetFloat sets v's underlying value to x. It panics if v's Kind is not Float32 or Float64, or if CanSet() is false.
SetFloat 设置v的底层值成x。 如果v的Kind 不是 Float32 or Float64, 或如果 CanSet()是 false，它引发panic。



func (Value) SetInt
```golang
func (v Value) SetInt(x int64)
```
SetInt sets v's underlying value to x. It panics if v's Kind is not Int, Int8, Int16, Int32, or Int64, or if CanSet() is false.
SetInt 设置v的底层值成x。如果 v的Kind 不是 Int, Int8, Int16, Int32, or Int64，或如果 CanSet()是 false，它引发panic。



func (Value) SetLen
```golang
func (v Value) SetLen(n int)
```
SetLen sets v's length to n. It panics if v's Kind is not Slice or if n is negative or greater than the capacity of the slice.
SetLen 设置v的长度成n。如果v的Kind不是Slice 或如果n是负的或大于 slice的容量，它引发panic。



func (Value) SetMapIndex
```golang
func (v Value) SetMapIndex(key, val Value)
```
SetMapIndex sets the value associated with key in the map v to val. 
It panics if v's Kind is not Map. If val is the zero Value, SetMapIndex deletes the key from the map. 
Otherwise if v holds a nil map, SetMapIndex will panic. 
As in Go, key's value must be assignable to the map's key type, and val's value must be assignable to the map's value type.
SetMapIndex设置 map v 里 和key对应的值 成 val。
如果v的Kind不是Map，它引发panic。如果val是零Value，SetMapIndex 从map里删除那个key。
否则，如果v 保留 一个nil map，SetMapIndex将会引发panic。
在GO里，key的值必须分配 到 map的key类型，并且 val的值必须分配给map的值类型。



func (Value) SetPointer
```golang
func (v Value) SetPointer(x unsafe.Pointer)
```
SetPointer sets the unsafe.Pointer value v to x. It panics if v's Kind is not UnsafePointer.
SetPointer 设置 unsafe.Pointer 值v成x。 如果v的Kind不是UnsafePointer，它引发 panic。



func (Value) SetString
```golang
func (v Value) SetString(x string)
```
SetString sets v's underlying value to x. It panics if v's Kind is not String or if CanSet() is false.
SetString设置v的底层值成x。如果v的Kind不是String 或如果CanSet() 是false，它引发panic。



func (Value) SetUint
```golang
func (v Value) SetUint(x uint64)
```
SetUint sets v's underlying value to x. It panics if v's Kind is not Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64, or if CanSet() is false.
SetUint 设置v的底层值成x。如果v的Kind 不是Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64,或如果 CanSet()是 false，它引发panic。
 


func (Value) Slice
```golang
func (v Value) Slice(i, j int) Value
```
Slice returns v[i:j]. It panics if v's Kind is not Array, Slice or String, or if v is an unaddressable array, or if the indexes are out of bounds.
Slice 返回v[i:j]. 如果v的Kind不是Array, Slice or String, 或如果v是不可寻址的array ，或如果索引超出范围，引发panic。



func (Value) Slice3
```golang
func (v Value) Slice3(i, j, k int) Value
```
Slice3 is the 3-index form of the slice operation: it returns v[i:j:k]. 
It panics if v's Kind is not Array or Slice, or if v is an unaddressable array, or if the indexes are out of bounds.
Slice3 是3指数形式的slice操作：返回v[i:j:k]. 
如果v的Kind 不是Array or Slice,或 如果v是不可寻址的array，或如果索引超出范围，引发panic。



func (Value) String
```golang
func (v Value) String() string
```
String returns the string v's underlying value, as a string. 
String is a special case because of Go's String method convention. 
Unlike the other getters, it does not panic if v's Kind is not String. 
Instead, it returns a string of the form "<T value>" where T is v's type.
String 返回 字符串 v的底层值 成string。
String 是一个特例， 因为GO的String方法转换。
不像其他， 如果v的Kind不是String 它不会引发panic。
替代的，它返回字符串形式"<T value>"  其中 T是 v的类型。



func (Value) TryRecv
```golang
func (v Value) TryRecv() (x Value, ok bool)
```
TryRecv attempts to receive a value from the channel v but will not block. 
It panics if v's Kind is not Chan. If the receive delivers a value, x is the transferred value and ok is true. 
If the receive cannot finish without blocking, x is the zero Value and ok is false. 
If the channel is closed, x is the zero value for the channel's element type and ok is false.
TryRecv尝试 从channelv 接收一个值，但是不会阻塞。
如果v的Kind不是Chan，它引发panic。如果接收提供了一个值，x转义值 并且ok是 true。
如果接收 没有阻塞不能结束， x是零Value 并且 ok是false。
如果channel关闭，对于channel的元素类型 x是零值 并且ok是false



func (Value) TrySend
```golang
func (v Value) TrySend(x Value) bool
```
TrySend attempts to send x on the channel v but will not block. 
It panics if v's Kind is not Chan. It returns true if the value was sent, false otherwise. 
As in Go, x's value must be assignable to the channel's element type.
TrySend 尝试在channel v上发送x，但是不会阻塞。
如果v的Kind不是Chan，它引发panic。如果 值发送，它是true， 否则 false。
在GO里，x的 值必须分配到chennel的元素类型。



func (Value) Type
```golang
func (v Value) Type() Type
```
Type returns v's type.
Type 返回 v的类型。



func (Value) Uint
```golang
func (v Value) Uint() uint64
```
Uint returns v's underlying value, as a uint64. It panics if v's Kind is not Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64.
Uint 返回v的底层值 当成 uint64。 如果 v的Kind 不是 Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64， 引发 panic。



func (Value) UnsafeAddr
```golang
func (v Value) UnsafeAddr() uintptr
```
UnsafeAddr returns a pointer to v's data. It is for advanced clients that also import the "unsafe" package. It panics if v is not addressable.
UnsafeAddr 返回一个指向v的数据。 它是为高级用户也导入了"unsafe" 包。 如果v不可寻址， 它引发panic。


type ValueError
```golang
type ValueError struct {
        Method string
        Kind   Kind
}
```
A ValueError occurs when a Value method is invoked on a Value that does not support it. Such cases are documented in the description of each method.
当Value 调用一个不支持它的Value 时，遇到一个ValueError。这种情况被记录在每个方法的描述。


func (*ValueError) Error
```golang
func (e *ValueError) Error() string
```














































