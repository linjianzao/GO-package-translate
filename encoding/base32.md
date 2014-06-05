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

Encoding是32位  编码/解码 的方案，根据32字母 字符定义
最常见的“base32”编码介绍是SASL GSSAPI,在 RFC 4648中规定
备用的"base32hex"编码 在DNSSEC使用
```

func NewEncoding
```golang
func NewEncoding(encoder string) *Encoding
	NewEncoding returns a new Encoding defined by the given alphabet, which must be a 32-byte string.
```

func (*Encoding) Decode
```golang
func (enc *Encoding) Decode(dst, src []byte) (n int, err error)
	Decode decodes src using the encoding enc. 
	It writes at most DecodedLen(len(src)) bytes to dst and returns the number of bytes written. 
	If src contains invalid base32 data, it will return the number of bytes successfully written and CorruptInputError. 
	New line characters (\r and \n) are ignored.
	Decode 使用enc的编码  解码src
	它最多写入DecodedLen(len(src))字节 到dst 返回写入的字节数量
	如果src包含无效的base32数据，它会返回写入成功的字节数量并返回CorruptInputError错误
	新行字符(\r 和 \n) 被忽略
```

func (*Encoding) DecodeString
```golang
func (enc *Encoding) DecodeString(s string) ([]byte, error)
	DecodeString returns the bytes represented by the base32 string s.
	返回 base32 s字符串  解码的 bytes
Code:
```golang
str := "ONXW2ZJAMRQXIYJAO5UXI2BAAAQGC3TEEDX3XPY="
data, err := base32.StdEncoding.DecodeString(str)
if err != nil {
    fmt.Println("error:", err)
    return
}
fmt.Printf("%q\n", data)
```
Output:
```golang
"some data with \x00 and \ufeff"
```
```

func (*Encoding) DecodedLen
```golang
func (enc *Encoding) DecodedLen(n int) int
	DecodedLen returns the maximum length in bytes of the decoded data corresponding to n bytes of base32-encoded data.
	返回n个字节的base32编码数据对应的解码数据的最大字节数（注 base32 的n个字节解码后的长度）
```

func (*Encoding) Encode
```golang
func (enc *Encoding) Encode(dst, src []byte)
	Encode encodes src using the encoding enc, writing EncodedLen(len(src)) bytes to dst.
	The encoding pads the output to a multiple of 8 bytes, so Encode is not appropriate for use on individual blocks of a large data stream. 
	Use NewEncoder() instead.
	使用enc编码方式编码 src ,写入 EncodedLen(len(src)) 长度 字节 到dst。
	编码后 以多个 8字节输出， 所以不支持大数据流
	遇到大数据流 用NewEncoder()  代替
```

func (*Encoding) EncodeToString
```golang
func (enc *Encoding) EncodeToString(src []byte) string
	EncodeToString returns the base32 encoding of src.
	返回src的base32 编码
Code:
```golang
data := []byte("any + old & data")
str := base32.StdEncoding.EncodeToString(data)
fmt.Println(str)
```
Output:
```golang
MFXHSIBLEBXWYZBAEYQGIYLUME======
```
```

func (*Encoding) EncodedLen
```golang
func (enc *Encoding) EncodedLen(n int) int
	EncodedLen returns the length in bytes of the base32 encoding of an input buffer of length n.
	返回长度为n的输入缓冲区的base64编码的字节长度。
```
