包地址：http://golang.org/pkg/encoding/gob/
```golang
Package gob manages streams of gobs - binary values exchanged between an Encoder (transmitter) and a Decoder (receiver). 
A typical use is transporting arguments and results of remote procedure calls (RPCs) such as those provided by package "rpc".

The implementation compiles a custom codec for each data type in the stream and is most efficient 
	when a single Encoder is used to transmit a stream of values, amortizing the cost of compilation.
	
gob包管理 gobs二进制流 Encoder(发射) 和 Decoder(接收) 的值交换 
一个典型的应用是 传输参数和返回远程调用的结果，如同 提供rpc包

当单个编码器传输一个流的值，编译摊销的成本, 实现一个自定义的编解码器  给每个流里的数据类型  编译并且 它是最有效的,



Basics
```golang
A stream of gobs is self-describing. 
Each data item in the stream is preceded by a specification of its type, expressed in terms of a small set of predefined types. 
Pointers are not transmitted, but the things they point to are transmitted; that is, the values are flattened. 
Recursive types work fine, but recursive values (data with cycles) are problematic. This may change.

To use gobs, create an Encoder and present it with a series of data items as values or addresses that can be dereferenced to values. 
The Encoder makes sure all type information is sent before it is needed. 
At the receive side, a Decoder retrieves values from the encoded stream and unpacks them into local variables.

gobs流的自我描述
流里的每个数据项 前面的类型规范，表示一小部分的预定义类型
指针不传输，但是指针指向的东西都会传输; 那样 值就会平坦化
递归类型做工精细。但是递归的值是问题（同周期数据）.这个也许会改变

使用gobs 创建编码器 然后 用一序列的数据项作为目前的值或者地址可以取消值引用
编码器确保所有的类型信息在发送之前都是有的
在接收端，解码器从编码流中接收值，解包他们到局部变量
```

Types and Values
```golang
The source and destination values/types need not correspond exactly. 
For structs, fields (identified by name) that are in the source but absent from the receiving variable will be ignored. 
Fields that are in the receiving variable but missing from the transmitted type or value will be ignored in the destination. 
If a field with the same name is present in both, their types must be compatible. 
Both the receiver and transmitter will do all necessary indirection and dereferencing to convert between gobs and actual Go values. 
For instance, a gob type that is schematically,
源和目标 值/类型 不必完全对应
比如 构造体，在源的字段不存在接收的变量将被忽略

如果一个字段存在两个相同的名字，他们的类型必须兼容
接收者和发送者都必须间接的可以在gobs 和GO值之间转换
例如：

```golang
struct { A, B int }
```

can be sent from or received into any of these Go types:
能发送 或接收任何GO类型
```golang
struct { A, B int }	// the same
*struct { A, B int }	// extra indirection of the struct
struct { *A, **B int }	// extra indirection of the fields
struct { A, B int64 }	// different concrete value type; see below
```

It may also be received into any of these:
也可以接收任何这些：

```golang
struct { A, B int }	// the same
struct { B, A int }	// ordering doesn't matter; matching is by name
struct { A, B, C int }	// extra field (C) ignored
struct { B int }	// missing field (A) ignored; data will be dropped
struct { B, C int }	// missing field (A) ignored; extra field (C) ignored.
```

Attempting to receive into these types will draw a decode error:
尝试接收以下的类型会导致解码错误：
```golang
struct { A int; B uint }	// change of signedness for B
struct { A int; B float }	// change of type for B
struct { }			// no field names in common
struct { C, D int }		// no field names in common
```

Integers are transmitted two ways: arbitrary precision signed integers or arbitrary precision unsigned integers. 
There is no int8, int16 etc. discrimination in the gob format; there are only signed and unsigned integers. 
As described below, the transmitter sends the value in a variable-length encoding; 
the receiver accepts the value and stores it in the destination variable. 
Floating-point numbers are always sent using IEEE-754 64-bit precision (see below).
整型的发送有两种方式：有符号的任意精度整型或者无符号的任意精度整型或者
没有int8, int16等。在gob格式中区别;

Signed integers may be received into any signed integer variable: int, int16, etc.; 
unsigned integers may be received into any unsigned integer variable; 
and floating point values may be received into any floating point variable. 
However, the destination variable must be able to represent the value or the decode operation will fail.
有符号整型可以接收任何有符号整型的变量:int, int16 等 
无符号整型可以接收任何无符号整型的变量
浮点型的值可以接收任何浮点型的变量
然而 目标变量 必须能表示值 不然解码就会出错

Structs, arrays and slices are also supported. Structs encode and decode only exported fields. 
Strings and arrays of bytes are supported with a special, efficient representation (see below). 
When a slice is decoded, if the existing slice has capacity the slice will be extended in place; 
if not, a new array is allocated. Regardless, the length of the resulting slice reports the number of elements decoded.
结构体，数组和slices都支持。结构体的编码和解码只是导出字段
字符串和 字节数组 支持一个特例，
当一个slice解码的时候如果存在slice容量，slice会被扩展
如果没有，会申请一个新的数组。无论解码后的slice元素长度多长

Functions and channels will not be sent in a gob. Attempting to encode such a value at top the level will fail. 
A struct field of chan or func type is treated exactly like an unexported field and is ignored.
函数和channel 不会发送。尝试像值一样在最顶级编码会导致错误

Gob can encode a value of any type implementing the GobEncoder or encoding.BinaryMarshaler interfaces by calling the corresponding method, 
	in that order of preference.
Gob可以按优先顺序调用对应的方法编码任何实现了GobEncoder或者encoding.BinaryMarshaler接口类型的值

Gob can decode a value of any type implementing the GobDecoder or encoding.BinaryUnmarshaler interfaces by calling the corresponding method, 
	again in that order of preference.
Gob可以按优先顺序调用对应的方法解码任何实现了GobDecoder或者encoding.BinaryUnmarshaler接口类型的值

```


Encoding Details
```golang
This section documents the encoding, details that are not important for most users. Details are presented bottom-up.

An unsigned integer is sent one of two ways. If it is less than 128, it is sent as a byte with that value. 
Otherwise it is sent as a minimal-length big-endian (high byte first) byte stream holding the value, preceded by one byte holding the byte count, negated. 
Thus 0 is transmitted as (00), 7 is transmitted as (07) and 256 is transmitted as (FE 01 00).

A boolean is encoded within an unsigned integer: 0 for false, 1 for true.

A signed integer, i, is encoded within an unsigned integer, u. Within u, bits 1 upward contain the value; 
bit 0 says whether they should be complemented upon receipt. The encode algorithm looks like this:

这章编码文档，详细信息对大部分使用者来说不是太重要.详细信息自下向上
无符号整型发送一个有两种方式：如果比128小，和发送字节一样发送值
否则就和发送最小长度的大端字节流一样 发送
从而0 变成00 发送，7变成07发送 ，256 就是  FE 01 00

无符号整型编码的布尔值 ： 0表示false ，1表示true
有符号整型，i，编码进无符号整型u。 在u里 1位 包含值
位0 表示是否应该收到补充。 编码的计算方式如下：

```golang
uint u;
if i < 0 {
	u = (^i << 1) | 1	// complement i, bit 0 is 1
} else {
	u = (i << 1)	// do not complement i, bit 0 is 0
}
encodeUnsigned(u)
```

The low bit is therefore analogous to a sign bit, but making it the complement bit instead guarantees that the largest negative integer is not a special case. 
For example, -129=^128=(^256>>1) encodes as (FE 01 01).
低位类似符号位，把它补位 而不是保证最大的负整型是一个特例（注：负数超过最大值用补位的方式处理）
例如-129=^128=(^256>>1) 编码成(FE 01 01).

Floating-point numbers are always sent as a representation of a float64 value. 
That value is converted to a uint64 using math.Float64bits. 
The uint64 is then byte-reversed and sent as a regular unsigned integer. 
The byte-reversal means the exponent and high-precision part of the mantissa go first. 
Since the low bits are often zero, this can save encoding bytes. For instance, 17.0 is encoded in only three bytes (FE 31 40).
浮点型数字经常当成float64 值发送
使用math.Float64bits 把值转为uint64
uint64 反转字节,当成常规的无符号整数发送
反转字节意味着指数和高精度部分尾数 第一
低位经常是0,这可以保存编码的字节。例子：17.0 只要编码成三个字节 (FE 31 40)

Strings and slices of bytes are sent as an unsigned count followed by that many uninterpreted bytes of the value.
字符串和slice字节 当成无符号数跟着许多未解释的字节值发送
All other slices and arrays are sent as an unsigned count followed by that many elements using the standard gob encoding for their type, recursively.
所有的其他slice和数组 当成无符号数跟着许多元素根据他们类型递归的进行标准gob编码。

Maps are sent as an unsigned count followed by that many key, element pairs. 
Empty but non-nil maps are sent, so if the sender has allocated a map, the receiver will allocate a map even if no elements are transmitted.
Map 当成无符号数发送跟着许多元素对的key
空的 但是非 nil的map会发送 所以如果发送者申请一个map，接收者也会申请一个map即使是元素没有发送到

Structs are sent as a sequence of (field number, field value) pairs. 
The field value is sent using the standard gob encoding for its type, recursively. 
If a field has the zero value for its type, it is omitted from the transmission. 
The field number is defined by the type of the encoded struct: the first field of the encoded type is field 0, the second is field 1, etc. 
When encoding a value, the field numbers are delta encoded for efficiency and the fields are always sent in order of increasing field number; 
the deltas are therefore unsigned. 
The initialization for the delta encoding sets the field number to -1, so an unsigned integer field 0 with value 7 is transmitted as unsigned delta = 1, 
	unsigned value = 7 or (01 07). Finally, after all the fields have been sent a terminating mark denotes the end of the struct. 
That mark is a delta=0 value, which has representation (00).
构造体 当成一序列的对发送
字段值使用它的类型的标准gob递归编码
如果字段的类型有零值，传输的时候会省略
字段数用编码类型构造体定义:第一个字段编码类型是0，第二个字段是1 等。
当编码一个值，字段数编码为了效率是增量的，字段通常为了提高字段数 发送
因此，增量是无符号。
初始化增量编码设置字段数为-1，所以无符号整型字段0和值7 传输 无符号增量等1 ，无符号值=7或者(01 07).最后当所有的字段发送完哦 在结构体 后 发送一个中断标记。
标记是delta=0 ,它表示(00)


Interface types are not checked for compatibility; 
all interface types are treated, for transmission, as members of a single "interface" type, 
	analogous to int or []byte - in effect they're all treated as interface{}. 
Interface values are transmitted as a string identifying the concrete type being sent (a name that must be pre-defined by calling Register), 
	followed by a byte count of the length of the following data (so the value can be skipped if it cannot be stored), 
	followed by the usual encoding of concrete (dynamic) value stored in the interface value. 
(A nil interface value is identified by the empty string and transmits no value.) Upon receipt, 
	the decoder verifies that the unpacked concrete item satisfies the interface of the receiving variable.
接口类型不检查兼容性
所有的接口类型 ,发送，单个"interface"类型 成员，类似int或者[]byte 他们都像 interface{}.
接口值


The representation of types is described below. 
When a type is defined on a given connection between an Encoder and Decoder, it is assigned a signed integer type id. When Encoder.Encode(v) is called, 
	it makes sure there is an id assigned for the type of v and all its elements and then it sends the pair 
	(typeid, encoded-v) where typeid is the type id of the encoded type of v and encoded-v is the gob encoding of the value v.

To define a type, the encoder chooses an unused, 
	positive type id and sends the pair (-type id, encoded-type) where encoded-type is the gob encoding of a wireType description, constructed from these types:


```golang
type wireType struct {
	ArrayT  *ArrayType
	SliceT  *SliceType
	StructT *StructType
	MapT    *MapType
}
type arrayType struct {
	CommonType
	Elem typeId
	Len  int
}
type CommonType struct {
	Name string // the name of the struct type
	Id  int    // the id of the type, repeated so it's inside the type
}
type sliceType struct {
	CommonType
	Elem typeId
}
type structType struct {
	CommonType
	Field []*fieldType // the fields of the struct.
}
type fieldType struct {
	Name string // the name of the field.
	Id   int    // the type id of the field, which must be already defined
}
type mapType struct {
	CommonType
	Key  typeId
	Elem typeId
}
```

If there are nested type ids, the types for all inner type ids must be defined before the top-level type id is used to describe an encoded-v.

For simplicity in setup, the connection is defined to understand these types a priori, as well as the basic gob types int, uint, etc. Their ids are:
```golang
bool        1
int         2
uint        3
float       4
[]byte      5
string      6
complex     7
interface   8
// gap for reserved ids.
WireType    16
ArrayType   17
CommonType  18
SliceType   19
StructType  20
FieldType   21
// 22 is slice of fieldType.
MapType     23
```

Finally, each message created by a call to Encode is preceded by an encoded unsigned integer count of the number of bytes remaining in the message. After the initial type name, interface values are wrapped the same way; in effect, the interface value acts like a recursive invocation of Encode.

In summary, a gob stream looks like
```golang
(byteCount (-type id, encoding of a wireType)* (type id, encoding of a value))*
```
where * signifies zero or more repetitions and the type id of a value must be predefined or be defined before the value in the stream.

See "Gobs of data" for a design discussion of the gob wire format: http://golang.org/doc/articles/gobs_of_data.html

```







```

