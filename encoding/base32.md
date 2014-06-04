包地址：http://golang.org/pkg/encoding/base32/
```golang
Package base32 implements base32 encoding as specified by RFC 4648.
通过RFC 4648 实现 base32 编码
```

Variables
```golang
var HexEncoding = NewEncoding(encodeHex)
	HexEncoding is the “Extended Hex Alphabet” defined in RFC 4648. It is typically used in DNS.
	HexEncoding 扩展的十六进制字母.它通常用于在DNS中
```

```golang
var StdEncoding = NewEncoding(encodeStd)
	StdEncoding is the standard base32 encoding, as defined in RFC 4648.
	StdEncoding 是base32编码标准
```

func NewDecoder
```golang
func NewDecoder(enc *Encoding, r io.Reader) io.Reader
	NewDecoder constructs a new base32 stream decoder.
	构建一个新的base32流解码器
```

func NewEncoder
```golang
func NewEncoder(enc *Encoding, w io.Writer) io.WriteCloser
	NewEncoder returns a new base32 stream encoder. 
	Data written to the returned writer will be encoded using enc and then written to w. 
	Base32 encodings operate in 5-byte blocks; 
	when finished writing, the caller must Close the returned encoder to flush any partially written blocks.
	返回一个新的base32流编码器
	数据使用enc编码并写入w
	编码以5个字节为块
	写入结束时，刷新编码块
	
	Code:
	```golang
	input := []byte("foo\x00bar")
	encoder := base32.NewEncoder(base32.StdEncoding, os.Stdout)
	encoder.Write(input)
	// Must close the encoder when finished to flush any partial blocks.
	// If you comment out the following line, the last partial block "r"
	// won't be encoded.
	encoder.Close()
	```
	Output:
	```golang
	MZXW6ADCMFZA====
	```
```

type CorruptInputError
```golang
type CorruptInputError int64
```

func (CorruptInputError) Error
```golang
func (e CorruptInputError) Error() string
```

type Encoding
```golang
type Encoding struct {
    // contains filtered or unexported fields
}
An Encoding is a radix 32 encoding/decoding scheme, defined by a 32-character alphabet. 
The most common is the "base32" encoding introduced for SASL GSSAPI and standardized in RFC 4648. 
The alternate "base32hex" encoding is used in DNSSEC.

Encoding是32  编码/解码 的基数，根据32字母 字符定义
最常见的“base32”编码介绍是SASL GSSAPI,在 RFC 4648中规定
备用的"base32hex"编码 在DNSSEC使用
```

func NewEncoding
```golang
func NewEncoding(encoder string) *Encoding
	NewEncoding returns a new Encoding defined by the given alphabet, which must be a 32-byte string.
```



