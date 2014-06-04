包地址：http://golang.org/pkg/encoding/ascii85/
```golang
Package ascii85 implements the ascii85 data encoding as used in the btoa tool and Adobe's PostScript and PDF document formats.
ascii85包实现 在btoa工具 和Adobe 的  PostScript 和 PDF文档中 的ascii85  数据编码
```

func Decode
```golang
func Decode(dst, src []byte, flush bool) (ndst, nsrc int, err error)
  Decode decodes src into dst, returning both the number of bytes written to dst and the number consumed from src. 
  	If src contains invalid ascii85 data, Decode will return the number of bytes successfully written and a CorruptInputError. 
  	Decode ignores space and control characters in src. Often, ascii85-encoded data is wrapped in <~ and ~> symbols. 
  	Decode expects these to have been stripped by the caller.
  	
  If flush is true, Decode assumes that src represents the end of the input stream and processes it completely rather than wait for the completion of another 32-bit block.
  
  NewDecoder wraps an io.Reader interface around Decode.
  
  src 解码成dst，返回写入dst的字节数和src消耗的字节数。
   如果src包含无效的ascii85 数据，将会返回写入成功的字节数和一个CorruptInputError。
  Decode 忽略src空间和控制src字符。通常ascii85编码数据包裹在<~ 和 ~> 符号之间。
  
  如果flush是true，src代表输入流的末尾并完全处理它，而不是等待其他32-bit
```

func Encode
```golang
func Encode(dst, src []byte) int
	Encode encodes src into at most MaxEncodedLen(len(src)) bytes of dst, returning the actual number of bytes written.
	The encoding handles 4-byte chunks, using a special encoding for the last fragment, 
		so Encode is not appropriate for use on individual blocks of a large data stream. Use NewEncoder() instead.
	Often, ascii85-encoded data is wrapped in <~ and ~> symbols. Encode does not add these.
	
	把src编码成 最多MaxEncodedLen(len(src))字节长度的dst，返回写入的数量
	编码以4字节处理堆， 使用 特殊的编码处理最后的段，所以Encode不适合处理大的数据流。用NewEncoder()代替
	通常ascii85数据编码包含在 <~ 和 ~> 字符之间。
```

func MaxEncodedLen
```golang
func MaxEncodedLen(n int) int
	MaxEncodedLen returns the maximum length of an encoding of n source bytes.
	返回n字节的源编码最大长度
```

func NewDecoder
```golang
func NewDecoder(r io.Reader) io.Reader
	NewDecoder constructs a new ascii85 stream decoder.
	构造一个新的ascii85流解码器
```

func NewEncoder
```golang
func NewEncoder(w io.Writer) io.WriteCloser
	NewEncoder returns a new ascii85 stream encoder. 
	Data written to the returned writer will be encoded and then written to w. 
	Ascii85 encodings operate in 32-bit blocks; 
	when finished writing, the caller must Close the returned encoder to flush any trailing partial block.
	返回一个新的ascii85流编码器
	要返回的数据会 被编码然后写入到w
	Ascii85 编码 在32-bit块里操作
	当写入结束的时候， 调用者必须Close 返回编码刷新的任何尾随的块
```

type CorruptInputError
```golang
    func (e CorruptInputError) Error() string
```

func (CorruptInputError) Error
```golang
	func (e CorruptInputError) Error() string
```