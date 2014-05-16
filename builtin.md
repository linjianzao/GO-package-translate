包地址：http://golang.org/pkg/builtin/

```golang
Package builtin provides documentation for Go's predeclared identifiers.
The items documented here are not actually in package builtin but their descriptions here allow godoc to present documentation for the language's special identifiers.
builtin包提供GO的预先声明标识符文档。
这里的文档实际上没有都在builtin包里。但是他们描述了 允许在现有的文档里的 特殊语言标识符

说明： 内置函数 不用引入包就可以直接用
```

```golang
Constants

const (
    true  = 0 == 0 // Untyped bool.
    false = 0 != 0 // Untyped bool.
)
true and false are the two untyped boolean values.
true 和 false 是两个无类型的布尔值

const iota = 0 // Untyped int.
iota is a predeclared identifier representing the untyped integer ordinal number of the current const specification in a (usually parenthesized) const declaration. It is zero-indexed.
```

```golang   
func append(slice []Type, elems ...Type) []Type
  The append built-in function appends elements to the end of a slice. 
  If it has sufficient capacity, the destination is resliced to accommodate the new elements. 
  If it does not, a new underlying array will be allocated. Append returns the updated slice. 
  It is therefore necessary to store the result of append, often in the variable holding the slice itself:
  append是从切片末尾加入切片的函数。
  如果有足够大小，原来的切片最终会容纳新的元素。
  如果没有足够大小。会申请一个新的底层数组。append返回新的切片。
  因此需要存储附加的结果，通常切片保持在变量里
  
	slice = append(slice, elem1, elem2)
	slice = append(slice, anotherSlice...)
	As a special case, it is legal to append a string to a byte slice, like this:
	
	slice = append([]byte("hello "), "world"...)
```

```golang
func cap(v Type) int
	The cap built-in function returns the capacity of v, according to its type:
		返回v的容量
		
	Array: the number of elements in v (same as len(v)).
		数组： v的元素数量（类似len(v)）
		
	Pointer to array: the number of elements in *v (same as len(v)).
		数组指针： *v 的元素数量（类似len(v)）
		
	Slice: the maximum length the slice can reach when resliced;
		切片：切片能接受的最大长度
		
	if v is nil, cap(v) is zero.
		如果v是nil  cap(v) 是0
		
	Channel: the channel buffer capacity, in units of elements;
		channel:channel的缓冲区容量
		
	if v is nil, cap(v) is zero.
		如果v是nil  cap(v) 是0
```

```golang
func close(c chan<- Type)
  The close built-in function closes a channel, which must be either bidirectional or send-only. 
  It should be executed only by the sender, never the receiver, and has the effect of shutting down the channel after the last sent value is received. 
  After the last value has been received from a closed channel c, any receive from c will succeed without blocking, returning the zero value for the channel element. The form
  内置函数close 关闭一个双向的或只发送的channel。
  它应该是只执行过的发送者而不是接收者，在最后发送的值被接收之后再关闭。
  当最后一个值从关闭的channel c 中被接收，任何从c接收的都成功而不会阻塞 ，从channel元素中返回零值
  
  x, ok := <-c
  will also set ok to false for a closed channel.
  
```

```golang
func complex(r, i FloatType) ComplexType
	The complex built-in function constructs a complex value from two floating-point values. 
	The real and imaginary parts must be of the same size, either float32 or float64 (or assignable to them), 
	and the return value will be the corresponding complex type (complex64 for float32, complex128 for float64).
	从两个浮点值中组成一个 复数（注：数学里的复数）。
	实数和虚数部分必须是相同长度，float32 或float64，
	返回相应的complex类型的值 (complex64 for float32, complex128 for float64)。
```

```golang
func copy(dst, src []Type) int
	The copy built-in function copies elements from a source slice into a destination slice. 
	(As a special case, it also will copy bytes from a string to a slice of bytes.) The source and destination may overlap. 
	Copy returns the number of elements copied, which will be the minimum of len(src) and len(dst).
	把元素从源slice复制到目标slice
	（特例，它也会从一个 string复制字节到一个bytes类型的slice）
	返回复制的元素数量，是 src 和 dst 两个中最小的长度
```

```golang
func delete(m map[Type]Type1, key Type)
  The delete built-in function deletes the element with the specified key (m[key]) from the map. 
  If m is nil or there is no such element, delete is a no-op.
  内置函数delete是通过指定的 key来删除map的元素的，delete(m[key])。 
  如果m是空的或者没有这个元素，delete是一个空操作。不会报错
``` 

```golang
func imag(c ComplexType) FloatType
	The imag built-in function returns the imaginary part of the complex number c. 
	The return value will be floating point type corresponding to the type of c.	
	返回复数c中的 虚数部分
	返回的浮点类型必须是c中相应的类型
```

```golang
func len(v Type) int
	The len built-in function returns the length of v, according to its type:

	Array: the number of elements in v.
	Pointer to array: the number of elements in *v (even if v is nil).
	Slice, or map: the number of elements in v; if v is nil, len(v) is zero.
	String: the number of bytes in v.
	Channel: the number of elements queued (unread) in the channel buffer;
	if v is nil, len(v) is zero.
	
	返回v的长度。以下是根据不同类型的情况：
	
	数组：v的元素数量;
	数组的指针：*v的元素数量（即使v是nil）;
	slice或map：v中的元素数量;如果v是nil，len(v) 的值是0;	
	字符串：v中的字节数;
	channel:channel 缓冲区中的队列元素数量(未读的元素);
	如果v是nil，len(v) 的值是0;	
```

```golang
func make(Type, size IntegerType) Type
	The make built-in function allocates and initializes an object of type slice, map, or chan (only). 
	Like new, the first argument is a type, not a value. Unlike new, make's return type is the same as the type of its argument, not a pointer to it. 
	The specification of the result depends on the type:
	
	Slice: The size specifies the length. The capacity of the slice is equal to its length. A second integer argument may be provided tospecify a different capacity; 
		it must be no smaller than the length, so make([]int, 0, 10) allocates a slice of length 0 and capacity 10.
	
	Map: An initial allocation is made according to the size but the resulting map has length 0. 
		The size may be omitted, in which case a small starting size is allocated.
	
	Channel: The channel's buffer is initialized with the specified buffer capacity. If zero, or the size is omitted, the channel is unbuffered.
	
	分配和初始化slice, map, or chan （只有这些）
	类似new方法，第一个参数是类型，而不是值。和like方法不一样的是，make返回和参数一样类型的类型，而不是值。
	结果依赖以下的类型：
	
	Slice:size指定长度。slice的容量等于它的长度。第二个整数参数可能是不同的容量。
		它必须不能比第一个整数小，所以 make([]int, 0, 10)分配一个长度是0 ，容量为10的slice.
		(注：make中第二个参数是长度，第三个参数是容量。容量不能比长度小)
		
	Map:初始化 分配一个size大小 ， 但长度为0的 map。
		size可以省略，在这种情况下size以最小单位分配。
	
	Channel: 根据指定的容量初始化一个 缓冲区。如果是0 或者size省略，那channel无缓冲区
```

```golang
	func new(Type) *Type
		The new built-in function allocates memory. 
		The first argument is a type, not a value, and the value returned is a pointer to a newly allocated zero value of that type.
		分配内存。
		第一个参数是类型，不是值，返回指向新分配的那个类型的零值的指针。
```


```golang
func panic(v interface{})
  The panic built-in function stops normal execution of the current goroutine. 
  	内置函数panic 用来停止正常执行的当前并发。
  When a function F calls panic, normal execution of F stops immediately.
  	 当一个函数F调用panic，F的执行就立即停止。
  Any functions whose execution was deferred by F are run in the usual way, and then F returns to its caller. 
  	任何F中的延迟函数会正常执行，然后F返回调用者。
  To the caller G, the invocation of F then behaves like a call to panic, terminating G's execution and running any deferred functions. 
  	调用者G，调用了F就相当于调用panic。停止G的执行然后运行任何延迟的函数。
  This continues until all functions in the executing goroutine have stopped, in reverse order. 
  	这个过程是倒序的直到所有的并发函数停止。
  At that point, the program is terminated and the error condition is reported, including the value of the argument to panic. 
  	这时，程序会终止，会报告错误条件，包括参数值的panic。
  This termination sequence is called panicking and can be controlled by the built-in function recover.
  	这个终止的序列被成为恐慌，能被内置函数recover操作。
  	（注：一般用来panic 错误 然后 revocer恢复操作）
```

```golang
	func print(args ...Type)
		The print built-in function formats its arguments in an implementation-specific way and writes the result to standard error. 
		Print is useful for bootstrapping and debugging; it is not guaranteed to stay in the language.
		把参数格式化特定的格式，写入结果到标准错误
		在引导和调试bug比较有用;不能保证未来还会包含在语言里（注：可能会取消，所以一般用来调bug 又不用 引fmt）
```

```golang
	func println(args ...Type)
		The println built-in function formats its arguments in an implementation-specific way and writes the result to standard error. 
		Spaces are always added between arguments and a newline is appended. Println is useful for bootstrapping and debugging; it is not guaranteed to stay in the language.
		把参数格式化特定的格式，写入结果到标准错误
		通常会在参数和行之间追加换行符。在引导和调试bug比较有用;不能保证未来还会包含在语言里
```

```golang
	func real(c ComplexType) FloatType
	The real built-in function returns the real part of the complex number c. The return value will be floating point type corresponding to the type of c.
	从复数c中返回实数部分。返回的值 浮点类型对应c的类型
```

```golang
	func recover() interface{}
		The recover built-in function allows a program to manage behavior of a panicking goroutine. 
		Executing a call to recover inside a deferred function (but not any function called by it) stops the panicking sequence by restoring normal execution and retrieves the error value passed to the call of panic. 
		If recover is called outside the deferred function it will not stop a panicking sequence. 
		In this case, or when the goroutine is not panicking, or if the argument supplied to panic was nil, recover returns nil. 
		Thus the return value from recover reports whether the goroutine is panicking.
  		
  		内置函数recover 允许程序管理恐慌goroutine的行为。
  		在延迟函数里调用revcoer停止恐慌序列，恢复正常的流程，取回错误的值传给调用的panic。
  		如果revocer不是在延迟函数里调用的，它不会停止恐慌。
  		在这种情况下，或者goroutine没有恐慌，或者如果提供给panic的参数是nil，recover返回nil。
  		因此recover的返回值和goroutine是不是恐慌无关。
```

```golang
	type ComplexType complex64
		ComplexType is here for the purposes of documentation only. It is a stand-in for either complex type: complex64 or complex128.
		只用做文档说明。替代 complex64 或 complex128类型
```

```golang
	type FloatType float32
		FloatType is here for the purposes of documentation only. It is a stand-in for either float type: float32 or float64.
		只用做文档说明。替代 float32 or float64类型
```

```golang
	type IntegerType int
		IntegerType is here for the purposes of documentation only. It is a stand-in for any integer type: int, uint, int8 etc.
		只用做文档说明。替代 int, uint, int8 
```

```golang
	type Type int
		Type is here for the purposes of documentation only. It is a stand-in for any Go type, but represents the same type for any given function invocation.
		只用做文档说明。替代GO的类型，表示调用的函数的类型
		var nil Type // Type must be a pointer, channel, func, interface, map, or slice type
			type必须是 ointer, channel, func, interface, map, or slice 类型
		nil is a predeclared identifier representing the zero value for a pointer, channel, func, interface, map, or slice type.
			nil是预定义的   pointer, channel, func, interface, map, or slice的零值
```

```golang
	type Type1 int
		Type1 is here for the purposes of documentation only. It is a stand-in for any Go type, but represents the same type for any given function invocation.
		只用做文档说明。表示调用的函数的类型
```

```golang
	type bool bool
		bool is the set of boolean values, true and false.
		代表 true 和false
```

```golang
	type byte byte
		byte is an alias for uint8 and is equivalent to uint8 in all ways. It is used, by convention, to distinguish byte values from 8-bit unsigned integer values.
		uint8的别名，任何情况下都等于uint8。 按照惯例被用在从 8位无符号整型值区分字节值
```

```golang
	type complex128 complex128
		complex128 is the set of all complex numbers with float64 real and imaginary parts.
		集float64的实数和虚数部分
```

```golang
	type complex64 complex64
		complex64 is the set of all complex numbers with float32 real and imaginary parts.
		集float32的实数和虚数部分
```

```golang
	type error interface {
    Error() string
	}
		The error built-in interface type is the conventional interface for representing an error condition, with the nil value representing no error.
```

```golang
	type float32 float32
		float32 is the set of all IEEE-754 32-bit floating-point numbers. 
		IEEE-754 32位的浮点数
```

```golang
	type float64 float64
		float64 is the set of all IEEE-754 64-bit floating-point numbers.
		IEEE-754 64位的浮点数
```

```golang
	type int int
		int is a signed integer type that is at least 32 bits in size. It is a distinct type, however, and not an alias for, say, int32.
		至少32位大小的带符号的整型。这是个单独的类型， 别名不是int32.
```

```golang
	type int16 int16
		int16 is the set of all signed 16-bit integers. Range: -32768 through 32767.
		16位整型。 范围：-32768 到 32767.
```

```golang
	type int32 int32
		int32 is the set of all signed 32-bit integers. Range: -2147483648 through 2147483647.
		32位整型。范围：-2147483648 到 2147483647.
```

```golang
	type int64 int64
		int64 is the set of all signed 64-bit integers. Range: -9223372036854775808 through 9223372036854775807.
		64位整型。范围：-9223372036854775808 到 9223372036854775807.
```

```golang
	type int8 int8
		int8 is the set of all signed 8-bit integers. Range: -128 through 127.
		8位整型。 范围： -128 到 127.
```

```golang
	type rune rune
		rune is an alias for int32 and is equivalent to int32 in all ways. It is used, by convention, to distinguish character values from integer values.
		rune是 int32的别名，在任何情况下都等于int32。和int 是不同类型的
```

```golang
	type string string
		string is the set of all strings of 8-bit bytes, conventionally but not necessarily representing UTF-8-encoded text. 
		A string may be empty, but not nil. Values of string type are immutable.
		string是一组8位，通常但不一定就是 UTF-8 编码的文本（注：通常是UTF-8, 但不是所有的都是UTF-8）
```

```golang
	type uint uint
		uint is an unsigned integer type that is at least 32 bits in size. It is a distinct type, however, and not an alias for, say, uint32.
		至少32位大小的无符号整型。别名不是uint32
```

```golang
	type uint16 uint16
		uint16 is the set of all unsigned 16-bit integers. Range: 0 through 65535.
		16位无符号整型。范围： 0 到 65535.
```

```golang
	type uint32 uint32
		uint32 is the set of all unsigned 32-bit integers. Range: 0 through 4294967295.
		32位无符号整型。 范围： 0 到 4294967295.
```

```golang
	type uint64 uint64
		uint64 is the set of all unsigned 64-bit integers. Range: 0 through 18446744073709551615.
		64位无符号整型。范围：0 到 18446744073709551615.
```

```golang
	type uint8 uint8
		uint8 is the set of all unsigned 8-bit integers. Range: 0 through 255.
		8位无符号整型。范围：0 到 255.
```

```golang
	type uintptr uintptr
		uintptr is an integer type that is large enough to hold the bit pattern of any pointer.
		足够容纳任何指针字节的整型类型
```
  
