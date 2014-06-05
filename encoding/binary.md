包地址：http://golang.org/pkg/encoding/binary/
```golang
Package binary implements simple translation between numbers and byte sequences and encoding and decoding of varints.

Numbers are translated by reading and writing fixed-size values. 
A fixed-size value is either a fixed-size arithmetic type (int8, uint8, int16, float32, complex64, ...) or 
an array or struct containing only fixed-size values.

Varints are a method of encoding integers using one or more bytes; 
numbers with smaller absolute value take a smaller number of bytes. 
For a specification, see http://code.google.com/apis/protocolbuffers/docs/encoding.html.
	
This package favors simplicity over efficiency. 
Clients that require high-performance serialization, especially for large data structures, 
should look at more advanced solutions such as the encoding/gob package or protocol buffers.

binary包 简单实现了在 数字 和字节序列  之间转换的 编码和解码

数字的转换使用 固定的值  读和写
一个固定大小值 可以是  固定大小的 算术类型(int8, uint8, int16, float32, complex64, ...) 或数组或只包含固定大小值的结构体

Varints 是一个使用一个或多个字节编码整型的方法
规范：http://code.google.com/apis/protocolbuffers/docs/encoding.html

这个包用利于简单效率的用
其他包包含高性能的序列化，尤其是大数据结构
应该更多的看先进的解决方案，比如encoding/gob 包或者缓冲区协议
```

Constants
```golang
const (
    MaxVarintLen16 = 3
    MaxVarintLen32 = 5
    MaxVarintLen64 = 10
)
MaxVarintLenN is the maximum length of a varint-encoded N-bit integer.
MaxVarintLenN是一个 varint编码的N位整型的最大长度 
```

Variables

```golang
var BigEndian bigEndian
```
BigEndian is the big-endian implementation of ByteOrder.
BigEndian 是大端实现的ByteOrder

```golang
var LittleEndian littleEndian
```
LittleEndian is the little-endian implementation of ByteOrder.
LittleEndian 是小端实现的ByteOrder
```

func PutUvarint
```golang
func PutUvarint(buf []byte, x uint64) int
	PutUvarint encodes a uint64 into buf and returns the number of bytes written. 
	If the buffer is too small, PutUvarint will panic.
	PutUvarint 编码一个 uint64到buf 返回写入的字节数
	如果缓冲区太小，会引发panic
```

func PutVarint
```golang
func PutVarint(buf []byte, x int64) int
	PutVarint encodes an int64 into buf and returns the number of bytes written. 
	If the buffer is too small, PutVarint will panic.
	PutVarint 编码一个 int64到buf 返回写入的字节数
	如果缓冲区太小，会引发panic
```

func Read
```golang
func Read(r io.Reader, order ByteOrder, data interface{}) error
	Read reads structured binary data from r into data. 
	Data must be a pointer to a fixed-size value or a slice of fixed-size values. 
	Bytes read from r are decoded using the specified byte order and written to successive fields of the data. 
	When reading into structs, the field data for fields with blank (_) field names is skipped; 
	i.e., blank field names may be used for padding.
	
	从r读取二进制数据的结构体 到data
	Data必须是固定大小值的指针或者固定大小值的slice
	从r中按指定字节顺序解码 读取 字节，然后连续写入数据字段
	当读取进结构体的时候，会跳过 (_)的字段。（注：结构体定义_就可以不赋值该字段了）
	即可以用于填充空白字段名

Code:
```golang
var pi float64
b := []byte{0x18, 0x2d, 0x44, 0x54, 0xfb, 0x21, 0x09, 0x40}
buf := bytes.NewReader(b)
err := binary.Read(buf, binary.LittleEndian, &pi)
if err != nil {
    fmt.Println("binary.Read failed:", err)
}
fmt.Print(pi)
```
Output:
```golang
3.141592653589793
```
```

func ReadUvarint
```golang
func ReadUvarint(r io.ByteReader) (uint64, error)
	ReadUvarint reads an encoded unsigned integer from r and returns it as a uint64.
	从r中读取编码的无符号整型，然后以uint64返回
```

func ReadVarint
```golang
func ReadVarint(r io.ByteReader) (int64, error)
	ReadVarint reads an encoded signed integer from r and returns it as an int64.
	从r中读取编码的有符号整型，然后以uint64返回
```

func Size
```golang
func Size(v interface{}) int
	Size returns how many bytes Write would generate to encode the value v, 
	which must be a fixed-size value or a slice of fixed-size values, or a pointer to such data.
	Size返回有多少字节写入编码 v的值，	必须有固定的值或者固定的slice的值或者数据的指针
```

func Uvarint
```golang
func Uvarint(buf []byte) (uint64, int)
	Uvarint decodes a uint64 from buf and returns that value and the number of bytes read (> 0). 
	If an error occurred, the value is 0 and the number of bytes n is <= 0 meaning:
	从buf解码一个uint64 然后返回那个值 和读取的字节数(> 0).
	如果遇到错误，值是0 ，字节数小于等于0 ：
	
	n == 0: buf too small buf太小
	n  < 0: value larger than 64 bits (overflow) and -n is the number of bytes read，值大于64位（溢出），-n 是读取的字节数
```

func Varint
```golang
func Varint(buf []byte) (int64, int)
	Varint decodes an int64 from buf and returns that value and the number of bytes read (> 0). 
	If an error occurred, the value is 0 and the number of bytes n is <= 0 with the following meaning:
	从buf解码一个int64 然后返回那个值 和读取的字节数(> 0).
	如果遇到错误，值是0 ，字节数小于等于0 ：

	n == 0: buf too small  buf太小
	n  < 0: value larger than 64 bits (overflow)  and -n is the number of bytes read  值大于64位（溢出），-n 是读取的字节数
```

func Write
```golang
func Write(w io.Writer, order ByteOrder, data interface{}) error
	Write writes the binary representation of data into w. 
	Data must be a fixed-size value or a slice of fixed-size values, or a pointer to such data. 
	Bytes written to w are encoded using the specified byte order and read from successive fields of the data.
   When writing structs, zero values are written for fields with blank (_) field names.
	把二进制data写入w
	Data必须是固定大小值的指针或者固定大小值的slice
	从r中按指定字节顺序解码 读取 字节，然后连续写入数据字段
	当读取进结构体的时候，会跳过 (_)的字段。（注：结构体定义_就可以不赋值该字段了）

Code:
```golang
buf := new(bytes.Buffer)
var pi float64 = math.Pi
err := binary.Write(buf, binary.LittleEndian, pi)
if err != nil {
    fmt.Println("binary.Write failed:", err)
}
fmt.Printf("% x", buf.Bytes())
```
Output:
```golang
18 2d 44 54 fb 21 09 40
```

Code:
```golang
buf := new(bytes.Buffer)
var data = []interface{}{
    uint16(61374),
    int8(-54),
    uint8(254),
}
for _, v := range data {
    err := binary.Write(buf, binary.LittleEndian, v)
    if err != nil {
        fmt.Println("binary.Write failed:", err)
    }
}
fmt.Printf("%x", buf.Bytes())
```
Output:
```golang
beefcafe
```
```

type ByteOrder
```golang
type ByteOrder interface {
    Uint16([]byte) uint16
    Uint32([]byte) uint32
    Uint64([]byte) uint64
    PutUint16([]byte, uint16)
    PutUint32([]byte, uint32)
    PutUint64([]byte, uint64)
    String() string
}
A ByteOrder specifies how to convert byte sequences into 16-, 32-, or 64-bit unsigned integers.
指定怎么把字节序列转化成无符号的16-, 32-, or 64-bit 
```
