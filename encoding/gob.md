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
```golang
struct { A, B int }
```
can be sent from or received into any of these Go types:
```golang
struct { A, B int }	// the same
*struct { A, B int }	// extra indirection of the struct
struct { *A, **B int }	// extra indirection of the fields
struct { A, B int64 }	// different concrete value type; see below
```
It may also be received into any of these:
```golang
struct { A, B int }	// the same
struct { B, A int }	// ordering doesn't matter; matching is by name
struct { A, B, C int }	// extra field (C) ignored
struct { B int }	// missing field (A) ignored; data will be dropped
struct { B, C int }	// missing field (A) ignored; extra field (C) ignored.
Attempting to receive into these types will draw a decode error:
```
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


Signed integers may be received into any signed integer variable: int, int16, etc.; 
unsigned integers may be received into any unsigned integer variable; 
and floating point values may be received into any floating point variable. 
However, the destination variable must be able to represent the value or the decode operation will fail.

Structs, arrays and slices are also supported. Structs encode and decode only exported fields. 
Strings and arrays of bytes are supported with a special, efficient representation (see below). 
When a slice is decoded, if the existing slice has capacity the slice will be extended in place; 
if not, a new array is allocated. Regardless, the length of the resulting slice reports the number of elements decoded.


Functions and channels will not be sent in a gob. Attempting to encode such a value at top the level will fail. 
A struct field of chan or func type is treated exactly like an unexported field and is ignored.

Gob can encode a value of any type implementing the GobEncoder or encoding.BinaryMarshaler interfaces by calling the corresponding method, 
	in that order of preference.

Gob can decode a value of any type implementing the GobDecoder or encoding.BinaryUnmarshaler interfaces by calling the corresponding method, 
	again in that order of preference.

```

```