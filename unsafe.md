Package unsafe


Overview ▾

Package unsafe contains operations that step around the type safety of Go programs.

unsafe包  包含GO程序操作步骤的类型安全。



func Alignof
```golang
func Alignof(v ArbitraryType) uintptr
```
Alignof returns the alignment of the value v. 
It is the maximum value m such that the address of a variable with the type of v will always be zero mod m. 
If v is of the form structValue.field, it returns the alignment of field f within struct object obj.
Alignof 返回值的一致性. 
它的 最大值m  是变量的地址  类型v 通过是零 模m.
如果 v 是 structValue.field形式, 它返回在结构体对象obj里的 字段f的一致性



func Offsetof
```golang
func Offsetof(v ArbitraryType) uintptr
```
Offsetof returns the offset within the struct of the field represented by v, which must be of the form structValue.field.
 In other words, it returns the number of bytes between the start of the struct and the start of the field.
Offsetof返回偏移内部字段的结构由v表示,必须形成structValue.field.
换句话说它返回的字节数之间的结构和该字节的开始。



func Sizeof
```golang
func Sizeof(v ArbitraryType) uintptr
```
Sizeof returns the size in bytes occupied by the value v. The size is that of the "top level" of the value only. 
For instance, if v is a slice, it returns the size of the slice descriptor, not the size of the memory referenced by the slice.
Sizeof  返回v占用的大小.  大小是“顶级”的值。
例如 如果v是一个slice, 它返回slice描述符的大小, 而不是slice引用内存的大小

type ArbitraryType
```golang
type ArbitraryType int
```
ArbitraryType is here for the purposes of documentation only and is not actually part of the unsafe package. It represents the type of an arbitrary Go expression.
ArbitraryType 在这里仅供文档的目的,并不是真的不安全的是unsafe包里的一部分. 它表示任意GO 表达式 的类型 



type Pointer
```golang
type Pointer *ArbitraryType
```
Pointer represents a pointer to an arbitrary type. There are four special operations available for type Pointer that are not available for other types.
Pointer 表示一个任意类型的指针. 有四个特殊类型不能用于其他类型的指针。
 
```golang
1) A pointer value of any type can be converted to a Pointer.
2) A Pointer can be converted to a pointer value of any type.
3) A uintptr can be converted to a Pointer.
4) A Pointer can be converted to a uintptr.

1)任何类型的指针的值可以转换为一个指针。
2) 指针可以被转换成任何类型的指针的值。
3)uintptr可以转换为一个指针。
4)指针可以被转换成一个uintptr。

```
Pointer therefore allows a program to defeat the type system and read and write arbitrary memory. It should be used with extreme care.
Pointer 因此允许一个程序失败类型系统和任意内存读写。 它必须 极度小心使用












