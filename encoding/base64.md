包地址：http://golang.org/pkg/encoding/base64/
```golang
Package base64 implements base64 encoding as specified by RFC 4648.
通过RFC 4648 实现 base64 编码
```

Variables
```golang
var StdEncoding = NewEncoding(encodeStd)
	StdEncoding is the standard base64 encoding, as defined in RFC 4648.
	StdEncoding是base64编码标准

var URLEncoding = NewEncoding(encodeURL)
	URLEncoding is the alternate base64 encoding defined in RFC 4648. 
	It is typically used in URLs and file names.
	URLEncoding备用base64编码
	通常被用在URL和文件名
```

func NewDecoder
```golang
func NewDecoder(enc *Encoding, r io.Reader) io.Reader
	NewDecoder constructs a new base64 stream decoder.
	构建一个新的 base64流解码器
```

func NewEncoder
```golang
func NewEncoder(enc *Encoding, w io.Writer) io.WriteCloser
	NewEncoder returns a new base64 stream encoder. 
	Data written to the returned writer will be encoded using enc and then written to w. 
	Base64 encodings operate in 4-byte blocks; when finished writing, the caller must Close the returned encoder to flush any partially written blocks.
	返回base64流编码器
	把返回的writer 使用enc编码方式写入 w
	Base64以4个字节为块操作;写入结束，刷新写入的部分
	
Code:
```golang
input := []byte("foo\x00bar")
encoder := base64.NewEncoder(base64.StdEncoding, os.Stdout)
encoder.Write(input)
// Must close the encoder when finished to flush any partial blocks.
// If you comment out the following line, the last partial block "r"
// won't be encoded.
encoder.Close()
```
Output:
```golang
Zm9vAGJhcg==
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
An Encoding is a radix 64 encoding/decoding scheme, defined by a 64-character alphabet. 
The most common encoding is the "base64" encoding defined in RFC 4648 and used in MIME (RFC 2045) and PEM (RFC 1421). 
RFC 4648 also defines an alternate encoding, which is the standard encoding with - and _ substituted for + and /.

Encoding 是64位的编码/解码方案
最常用的编码是 RFC 4648定义的  "base64"  , 在MIME (RFC 2045) 和 PEM (RFC 1421)使用
使用 - 和 _ 代替+ 和 /
```

func NewEncoding 
```golang
func NewEncoding(encoder string) *Encoding
	NewEncoding returns a new Encoding defined by the given alphabet, which must be a 64-byte string.
	使用给定的字母定义新的 编码 ，必须是64字节 字符串
```

func (*Encoding) Decode
```golang
func (enc *Encoding) Decode(dst, src []byte) (n int, err error)
	Decode decodes src using the encoding enc. 
	It writes at most DecodedLen(len(src)) bytes to dst and returns the number of bytes written. 
	If src contains invalid base64 data, it will return the number of bytes successfully written and CorruptInputError. 
	New line characters (\r and \n) are ignored.
	使用enc编码方式 解码 src
	最多写入DecodedLen(len(src))字节长度到dst，返回写入的字节数
	如果src包含无效的base64数据，返回写入成功的数据 和 CorruptInputError错误
	新行的(\r 和 \n)字符被忽略
```	

func (*Encoding) DecodeString
```golang
func (enc *Encoding) DecodeString(s string) ([]byte, error)
	DecodeString returns the bytes represented by the base64 string s.
	使用base64 字符串s 返回bytes
	
Code:
```golang
str := "c29tZSBkYXRhIHdpdGggACBhbmQg77u/"
data, err := base64.StdEncoding.DecodeString(str)
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
	DecodedLen returns the maximum length in bytes of the decoded data corresponding to n bytes of base64-encoded data.
	返回base64编码数据 解码后对应的最大长度
```

func (*Encoding) Encode
```golang
func (enc *Encoding) Encode(dst, src []byte)
	Encode encodes src using the encoding enc, writing EncodedLen(len(src)) bytes to dst.
	The encoding pads the output to a multiple of 4 bytes, so Encode is not appropriate for use on individual blocks of a large data stream. 
	Use NewEncoder() instead.
	
	使用enc编码方式 编码src， 写入 EncodedLen(len(src))长度的字节到dst
	以4字节输出。所以不支持大数据流
	大数据使用NewEncoder()代替
```

func (*Encoding) EncodeToString
```golang
func (enc *Encoding) EncodeToString(src []byte) string
	EncodeToString returns the base64 encoding of src.
	返回base64编码的src
Code:
```golang
data := []byte("any + old & data")
str := base64.StdEncoding.EncodeToString(data)
fmt.Println(str)
```
Output:
```golang
YW55ICsgb2xkICYgZGF0YQ==
```
```

func (*Encoding) EncodedLen
```golang
func (enc *Encoding) EncodedLen(n int) int
	EncodedLen returns the length in bytes of the base64 encoding of an input buffer of length n.
	返回长度为n的输入缓冲区的base64编码的字节长度。
```







