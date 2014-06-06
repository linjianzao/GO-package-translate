包地址：http://golang.org/pkg/encoding/hex/
```golang
Package hex implements hexadecimal encoding and decoding.
实现十六进制的 编码和解码
```

Variables
```golang
var ErrLength = errors.New("encoding/hex: odd length hex string")
ErrLength results from decoding an odd length slice.
返回解码奇数长度的slice
```

func Decode
```golang
func Decode(dst, src []byte) (int, error)
	Decode decodes src into DecodedLen(len(src)) bytes, returning the actual number of bytes written to dst.
	If Decode encounters invalid input, it returns an error describing the failure.
	解码src到DecodedLen(len(src)) 长度的字节，返回写入dst的字节数
	如果解码遇到无效字符，返回错误信息
```

func DecodeString
```golang
func DecodeString(s string) ([]byte, error)
	DecodeString returns the bytes represented by the hexadecimal string s.
	使用bytes代表16进制字符串s
```

func DecodedLen
```golang
func DecodedLen(x int) int
```

func Dump
```golang
func Dump(data []byte) string
	Dump returns a string that contains a hex dump of the given data. 
	The format of the hex dump matches the output of `hexdump -C` on the command line.
	返回data里包含的16进制 字符串
	在命令行中 16进制格式匹配输出`hexdump -C` 
```

func Dumper
```golang
func Dumper(w io.Writer) io.WriteCloser
	Dumper returns a WriteCloser that writes a hex dump of all written data to w. 
	The format of the dump matches the output of `hexdump -C` on the command line.
	返回WriteCloser 把所有的16进制数据写入w
```

func Encode
```golang
func Encode(dst, src []byte) int
	Encode encodes src into EncodedLen(len(src)) bytes of dst. 
	As a convenience, it returns the number of bytes written to dst, but this value is always EncodedLen(len(src)). 
	Encode implements hexadecimal encoding.
	编码src 到EncodedLen(len(src)) 长度字节的 dst
	为了方便， 返回写入 dst 字节的长度，这个值通常是EncodedLen(len(src)).
	实现16进制编码
```

func EncodeToString
```golang
func EncodeToString(src []byte) string
	EncodeToString returns the hexadecimal encoding of src.
	返回16进制编码后的src
```

func EncodedLen
```golang
func EncodedLen(n int) int
	EncodedLen returns the length of an encoding of n source bytes.
	返回n源字节编码后的长度
```	

type InvalidByteError
```golang
type InvalidByteError byte
	InvalidByteError values describe errors resulting from an invalid byte in a hex string.
	返回16进制字符串 中 无效字节的错误信息
```

func (InvalidByteError) Error
```golang
fun
c (e InvalidByteError) Error() string
```







