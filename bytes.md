包地址：http://golang.org/pkg/bytes/

```golang
Package bytes implements functions for the manipulation of byte slices. It is analogous to the facilities of the strings package.
bytes 包实现了操作byte slices的方法。和它类似的是strings包
```
```golang
func Compare(a, b []byte) int
  Compare returns an integer comparing two byte slices lexicographically. 
  The result will be 0 if a==b, -1 if a < b, and +1 if a > b. 
  A nil argument is equivalent to an empty slice.
  
  对比两个byte slices 返回整型对比结果。
  如果a==b返回0,如果a<b返回-1,如果a>b返回+1。
  nil参数等价于空的slice
```

```golang
	例子：
	// Interpret Compare's result by comparing it to zero.
	var a, b []byte
	if bytes.Compare(a, b) < 0 {
	    // a less b
	}
	if bytes.Compare(a, b) <= 0 {
	    // a less or equal b
	}
	if bytes.Compare(a, b) > 0 {
	    // a greater b
	}
	if bytes.Compare(a, b) >= 0 {
	    // a greater or equal b
	}
	
	// Prefer Equal to Compare for equality comparisons.
	if bytes.Equal(a, b) {
	    // a equal b
	}
	if !bytes.Equal(a, b) {
	    // a not equal b
	}
```

```golang
	搜索例子：
	// Binary search to find a matching byte slice. 搜索匹配的byte slice
	var needle []byte
	var haystack [][]byte // Assume sorted
	i := sort.Search(len(haystack), func(i int) bool {
	    // Return haystack[i] >= needle.
	    return bytes.Compare(haystack[i], needle) >= 0
	})
	if i < len(haystack) && bytes.Equal(haystack[i], needle) {
	    // Found it!
	}
```

```golang
func Contains(b, subslice []byte) bool
  Contains returns whether subslice is within b.
  Contains 返回subslice（切片）是否存在b
```

```golang  
func Count(s, sep []byte) int
  Count counts the number of non-overlapping instances of sep in s.
  Count 统计s里出现的sep的次数，如果sep是空的 返回len(s)+1
```

```golang
func Equal(a, b []byte) bool
  Equal returns a boolean reporting whether a and b are the same length and contain the same bytes. A nil argument is equivalent to an empty slice.
  Equal返回a和b是否长度和bytes都相等。nil和空slice相等
```

```golang
func EqualFold(s, t []byte) bool
  EqualFold reports whether s and t, interpreted as UTF-8 strings, are equal under Unicode case-folding.
  EqualFold 判断s和t是否相等，忽略大小写，并对特殊字符进行转换
```

```golang
func Fields(s []byte) [][]byte
  Fields splits the slice s around each instance of one or more consecutive white space characters, 
  	returning a slice of subslices of s or an empty list if s contains only white space.
  Fields 以空白符做分隔符 将s 切分为多个子串，返回s的子切片，如果s只有空格符那返回空列
```

```golang
func FieldsFunc(s []byte, f func(rune) bool) [][]byte
  FieldsFunc interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It splits the slice s at each run of code points c satisfying f(c) and returns a slice of subslices of s. 
  If no code points in s satisfy f(c), an empty slice is returned.
  
  以一个或多个满足f(rune)的字符为分隔符。
  将s切分为多个子串，结果中不包含分隔符本身
  如果s中没有满足f(rune)的字符，返回空列表
```

```golang
func HasPrefix(s, prefix []byte) bool
  HasPrefix tests whether the byte slice s begins with prefix。.
  HasPrefix 判断s是不是以prefix开头
```

```golang
func HasSuffix(s, suffix []byte) bool
  HasSuffix tests whether the byte slice s ends with suffix.
  HasSuffix 判断s是不是以suffix结束的
```

```golang
func Index(s, sep []byte) int
  Index returns the index of the first instance of sep in s, or -1 if sep is not present in s.
  Index返回sep在s里第一次出现的索引，如果sep没有在s里返回-1
```

```golang
func IndexAny(s []byte, chars string) int
  IndexAny interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It returns the byte index of the first occurrence in s of any of the Unicode code points in chars. 
  It returns -1 if chars is empty or if there is no code point in common.
  IndexAny 返回 chars 中的任何一个字符在 s 中第一次出现的位置。如果chars是空的或者找不到返回-1
```

```golang
func IndexByte(s []byte, c byte) int
  IndexByte returns the index of the first instance of c in s, or -1 if c is not present in s.
  返回 c在s中第一次出现的位置。如果c没有在s里，返回-1
```

```golang
func IndexFunc(s []byte, f func(r rune) bool) int
  IndexFunc interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It returns the byte index in s of the first Unicode code point satisfying f(c), or -1 if none do.
 	返回s中第一个满足f(r rune)的字符的字节位置
 	如果没有返回-1
```

```golang
func IndexRune(s []byte, r rune) int
  IndexRune interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It returns the byte index of the first occurrence in s of the given rune. 
  It returns -1 if rune is not present in s.
  返回r在s中第一次出现的位置。如果找不到返回-1
```

```golang
func Join(s [][]byte, sep []byte) []byte
  Join concatenates the elements of s to create a new byte slice. 
  The separator sep is placed between elements in the resulting slice.
  
  Join 连接s里的元素形成一个新的byte slice。sep是两个元素之间的分隔符。
```

```golang
func LastIndex(s, sep []byte) int
  LastIndex returns the index of the last instance of sep in s, or -1 if sep is not present in s.
  LastIndex 返回在s中出现sep的最后的索引，如果sep没有在s里返回-1
```

```golang
func LastIndexAny(s []byte, chars string) int
  LastIndexAny interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It returns the byte index of the last occurrence in s of any of the Unicode code points in chars. 
  It returns -1 if chars is empty or if there is no code point in common.
  LastIndexAny 返回在s中最后出现的chars的索引，如果chars没有在s中 返回-1。
```

```golang
func LastIndexFunc(s []byte, f func(r rune) bool) int
  LastIndexFunc interprets s as a sequence of UTF-8-encoded Unicode code points. 
  It returns the byte index in s of the last Unicode code point satisfying f(c), or -1 if none do.
  LastIndexFunc  返回s中最后一个满足f(r rune) 的索引
```

```golang
func Map(mapping func(r rune) rune, s []byte) []byte
  Map returns a copy of the byte slice s with all its characters modified according to the mapping function. 
  If mapping returns a negative value, the character is dropped from the string with no replacement. 
  The characters in s and the output are interpreted as UTF-8-encoded Unicode code points.
  Map 将s中满足mapping(rune)的字符替换为mapping(rune)的返回值
   如果mapping返回负值，则相应的字符被删除。
```  

```golang
func Repeat(b []byte, count int) []byte
  Repeat returns a new byte slice consisting of count copies of b.
  Repeat 返回一个从b拷贝的count长度的新byte slice 
```

```golang
func Replace(s, old, new []byte, n int) []byte
  Replace returns a copy of the slice s with the first n non-overlapping instances of old replaced by new. 
  If n < 0, there is no limit on the number of replacements.
  Replace 将s中的old 替换为new 并返回
   替换次数为n次
   如果n<0，则全部替换
```

```golang
func Runes(s []byte) []rune
  Runes returns a slice of runes (Unicode code points) equivalent to s.
  Runes 返回一个runes slice 等价于 s
```

```golang
func Split(s, sep []byte) [][]byte
  Split slices s into all subslices separated by sep and returns a slice of the subslices between those separators. 
  If sep is empty, Split splits after each UTF-8 sequence. It is equivalent to SplitN with a count of -1.
  Split 以s为分隔符，将s切分成多个字串，结果不包含sep本身
  如果sep是空的，splits 按每个UTF8序列分割。等价于SplitN cout -1
```

```golang
func SplitAfter(s, sep []byte) [][]byte
  SplitAfter slices s into all subslices after each instance of sep and returns a slice of those subslices. 
  If sep is empty, SplitAfter splits after each UTF-8 sequence. It is equivalent to SplitAfterN with a count of -1.
  和Split一样，但是包含了sep本身
```

```golang
func SplitN(s, sep []byte, n int) [][]byte
  SplitN slices s into subslices separated by sep and returns a slice of the subslices between those separators. 
  If sep is empty, SplitN splits after each UTF-8 sequence. The count determines the number of subslices to return:
  n > 0: at most n subslices; the last subslice will be the unsplit remainder.
  n == 0: the result is nil (zero subslices)
  n < 0: all subslices
  
	以s为分隔符，将s切分成多个字串，结果不包含sep本身
	如果 sep 为空，则将 s 切分成 UTF-8 字符列表
	返回子串的数量：
	n>0:多个n个子串，最后一个子串是没有切分的。(就是如果切分到n个之后还可以切， 但是就不切分了直接返回最后的部分)
	n==0:结果是nil
	n<0:所有的子串，不限制切分个数。
```

```golang  
func SplitAfterN(s, sep []byte, n int) [][]byte
  SplitAfterN slices s into subslices after each instance of sep and returns a slice of those subslices. 
  If sep is empty, SplitAfterN splits after each UTF-8 sequence. The count determines the number of subslices to return:
  	n > 0: at most n subslices; the last subslice will be the unsplit remainder.
  	n == 0: the result is nil (zero subslices)
  	n < 0: all subslices
  
和SplitN一样，但包含了sep本身
  
```

```golang
func Title(s []byte) []byte
  Title returns a copy of s with all Unicode letters that begin words mapped to their title case.
  将s中的所有单词的首字母修改为Title格式
  BUG: The rule Title uses for word boundaries does not handle Unicode punctuation properly.
  BUG：title规则不能正确处理Unicode 标点符号
```

```golang
func ToLower(s []byte) []byte
  ToLower returns a copy of the byte slice s with all Unicode letters mapped to their lower case.
  把s的字符改成小写
```

```golang
func ToLowerSpecial(_case unicode.SpecialCase, s []byte) []byte
  ToLowerSpecial returns a copy of the byte slice s with all Unicode letters mapped to their lower case,
  giving priority to the special casing rules.
  把s的字符改成小写，优先使用 _case 中的规则进行转换
```

```golang
func ToTitle(s []byte) []byte
  ToTitle returns a copy of the byte slice s with all Unicode letters mapped to their title case.
  把s的字符改成ToTitle格式
```

```golang
func ToTitleSpecial(_case unicode.SpecialCase, s []byte) []byte
  ToTitleSpecial returns a copy of the byte slice s with all Unicode letters mapped to their title case,
  giving priority to the special casing rules.
  把s的字符改成ToTitle格式，优先使用 _case 中的规则进行转换
```

```golang
func ToUpper(s []byte) []byte
  ToUpper returns a copy of the byte slice s with all Unicode letters mapped to their upper case.
	把s的字符改成大写。
 ```

```golang 
func ToUpperSpecial(_case unicode.SpecialCase, s []byte) []byte
  ToUpperSpecial returns a copy of the byte slice s with all Unicode letters mapped to their upper case, 
  giving priority to the special casing rules.
   把s的字符改成大写，优先使用 _case 中的规则进行转换
```

```golang
func Trim(s []byte, cutset string) []byte
  Trim returns a subslice of s by slicing off all leading and trailing UTF-8-encoded Unicode code points contained in cutset.
  Trim 返回 s 根据 cutset 去除头部和尾部的UTF8编码切片。（相当于其他语言的trim ，就是能设置要trim的字符）
```

```golang
func TrimFunc(s []byte, f func(r rune) bool) []byte
  TrimFunc returns a subslice of s by slicing off all leading and trailing UTF-8-encoded Unicode code points c that satisfy f(c).
  TrimFunc 根据 满足f(rune)的字符，去除头部和尾部
```

```golang
func TrimLeft(s []byte, cutset string) []byte
  TrimLeft returns a subslice of s by slicing off all leading UTF-8-encoded Unicode code points contained in cutset.
  TrimLeft 返回 s 根据 cutset 去除头部的UTF8编码切片
```

```golang
func TrimLeftFunc(s []byte, f func(r rune) bool) []byte
  TrimLeftFunc returns a subslice of s by slicing off all leading UTF-8-encoded Unicode code points c that satisfy f(c).
  TrimFunc 返回 根据 满足f(rune)的字符  去除头部的UTF8编码切片
```

```golang
func TrimPrefix(s, prefix []byte) []byte
  TrimPrefix returns s without the provided leading prefix string. If s doesn't start with prefix, s is returned unchanged.
  返回去掉prefix的开始的字符串s，如果s不是以prefix开始的，S没有被改变
```

```golang
例子:
	代码:
		var b = []byte("Goodbye,, world!")
		b = bytes.TrimPrefix(b, []byte("Goodbye,"))
		b = bytes.TrimPrefix(b, []byte("See ya,"))
		fmt.Printf("Hello%s", b)
	
	输出:
		Hello, world!
```

```golang
func TrimRight(s []byte, cutset string) []byte
  TrimRight returns a subslice of s by slicing off all trailing UTF-8-encoded Unicode code points that are contained in cutset.
  返回 s 根据 cutset 去除尾部的UTF8编码切片
```

```golang
func TrimRightFunc(s []byte, f func(r rune) bool) []byte
  TrimRightFunc returns a subslice of s by slicing off all trailing UTF-8 encoded Unicode code points c that satisfy f(c).
  返回 根据函数f  去除尾部的UTF8编码切片
```

```golang
func TrimSpace(s []byte) []byte
  TrimSpace returns a subslice of s by slicing off all leading and trailing white space, as defined by Unicode.
  返回根据空格去掉头部和尾部的UTF8编码切片
```

```golang
func TrimSuffix(s, suffix []byte) []byte
  TrimSuffix returns s without the provided trailing suffix string. If s doesn't end with suffix, s is returned unchanged.
  返回去掉prefix的结束的字符串s，如果s不是以prefix结束的，S没有被改变
```

```golang
	代码:
		var b = []byte("Hello, goodbye, etc!")
		b = bytes.TrimSuffix(b, []byte("goodbye, etc!"))
		b = bytes.TrimSuffix(b, []byte("gopher"))
		b = append(b, bytes.TrimSuffix([]byte("world!"), []byte("x!"))...)
		os.Stdout.Write(b)
		
	输出:
		Hello, world!
```

```golang
type Buffer
  A Buffer is a variable-sized buffer of bytes with Read and Write methods. The zero value for Buffer is an empty buffer ready to use.
  Buffer 是一个可变大小 可读写缓冲区的方法。缓冲区的0值是一个空的准备被使用的缓冲区
```

```golang
	Code:
		var b bytes.Buffer // A Buffer needs no initialization.
		b.Write([]byte("Hello "))
		fmt.Fprintf(&b, "world!")
		b.WriteTo(os.Stdout)
	Output:
		Hello world!
```
  
```golang
	Code:
		// A Buffer can turn a string or a []byte into an io.Reader.
		buf := bytes.NewBufferString("R29waGVycyBydWxlIQ==")
		dec := base64.NewDecoder(base64.StdEncoding, buf)
		io.Copy(os.Stdout, dec)
	Output:
		Gophers rule!
```

```golang
  func NewBuffer(buf []byte) *Buffer
    NewBuffer creates and initializes a new Buffer using buf as its initial contents. 
    It is intended to prepare a Buffer to read existing data. It can also be used to size the internal buffer for writing. 
    To do that, buf should have the desired capacity but a length of zero.
    In most cases, new(Buffer) (or just declaring a Buffer variable) is sufficient to initialize a Buffer.
    
     创建和初始化一个新的Buffer使用buf初始化内容。
     它的目的是准备读取存在的数据。它也可以用来写入缓冲区。
     为了做到这点，buf应该有足够的大小，但是长度为0
     大部分情况下，new(Buffer) （或者只是声明一个缓冲变量）足够初始化缓冲区。
     就是用来创建bytes.Buffer对象的
```

```golang    
  func NewBufferString(s string) *Buffer
    NewBufferString creates and initializes a new Buffer using string s as its initial contents. 
    It is intended to prepare a buffer to read an existing string.
    In most cases, new(Buffer) (or just declaring a Buffer variable) is sufficient to initialize a Buffer.
    创建和初始化一个新的Buffer使用s初始化内容它的目的是准备读取存在的数据。
    大部分情况下，new(Buffer) （或者只是声明一个缓冲变量）足够初始化缓冲区。
```

```golang  
  func (b *Buffer) Bytes() []byte
    Bytes returns a slice of the contents of the unread portion of the buffer; len(b.Bytes()) == b.Len(). 
    If the caller changes the contents of the returned slice, 
    the contents of the buffer will change provided there are no intervening method calls on the Buffer.
     返回未读取内容部分的缓冲区slice。 len(b.Bytes()) == b.Len()。如果调用者修改了缓冲区，那返回的内容也会跟着改变。（这边b是引用）
```

```golang  
  func (b *Buffer) Grow(n int)
    Grow grows the buffer's capacity, if necessary, to guarantee space for another n bytes. 
    After Grow(n), at least n bytes can be written to the buffer without another allocation. 
    If n is negative, Grow will panic. If the buffer can't grow it will panic with ErrTooLarge.
    增加缓冲区的容量，如果需要，保证空间的n字节。增长之后，在没有分配其他的时候至少能写入n个字节。如果n是负数，Grow 将会panic。如果缓冲区不能增加，将会panic ErrTooLarge
```

```golang   
  func (b *Buffer) Len() int
    Len returns the number of bytes of the unread portion of the buffer; b.Len() == len(b.Bytes()).
    返回 缓冲区内未读的字符长度。b.Len() == len(b.Bytes()).
    注：b.Bytes()就是缓冲区里未读的内容
```

```golang
  func (b *Buffer) Next(n int) []byte
    Next returns a slice containing the next n bytes from the buffer, advancing the buffer as if the bytes had been returned by Read. 
    If there are fewer than n bytes in the buffer, Next returns the entire buffer. 
    The slice is only valid until the next call to a read or write method.
    从缓冲区里返回下n个字节的切片，如果返回的字节是读取的，推进缓冲区。
    如果缓冲区的字节比n少，返回所有的缓冲区内容。
    读取的数据在下一次读写之前都有效
```  

```golang  
  func (b *Buffer) Read(p []byte) (n int, err error)
    Read reads the next len(p) bytes from the buffer or until the buffer is drained. 
    The return value n is the number of bytes read. 
    If the buffer has no data to return, err is io.EOF (unless len(p) is zero); otherwise it is nil.
    从缓冲区里读取p长度的字节，直到缓冲区为空。
    返回值n是读取的缓冲区字节的数量。
    如果缓冲区没有数据返回，错误 要嘛是io.EOF 要嘛是nil
```  

```golang
  func (b *Buffer) ReadByte() (c byte, err error)
    ReadByte reads and returns the next byte from the buffer. If no byte is available, it returns error io.EOF.
    读取 和返回缓冲区的下一个字节。如果字节无效，会返回错误 io.EOF。
```

```golang  
  func (b *Buffer) ReadBytes(delim byte) (line []byte, err error)
    ReadBytes reads until the first occurrence of delim in the input, 
    returning a slice containing the data up to and including the delimiter. 
    If ReadBytes encounters an error before finding a delimiter, 
    it returns the data read before the error and the error itself (often io.EOF). 
    ReadBytes returns err != nil if and only if the returned data does not end in delim.
     读取输入直到第一次遇到delim，返回切片数据包含分隔符。
     如果在遇到分隔符之前遇到错误，返回遇到错误之前读取的错误和错误本身（经常是io.EOF）。当且仅当不以delim结束时返回nil
```

```golang 
  func (b *Buffer) ReadFrom(r io.Reader) (n int64, err error)
    ReadFrom reads data from r until EOF and appends it to the buffer, growing the buffer as needed. 
    The return value n is the number of bytes read. 
    Any error except io.EOF encountered during the read is also returned. 
    If the buffer becomes too large, ReadFrom will panic with ErrTooLarge.
     从r中读取数据直到 EOF 然后把数据加入缓冲区，缓冲区容量会增加。
     返回值n时读取的字节数。
     除了 io.EOF 之外的错误都会返回。
     如果缓冲区变得太大，ReadFrom 会引发panic  ErrTooLarge
```

```golang  
  func (b *Buffer) ReadRune() (r rune, size int, err error)
    ReadRune reads and returns the next UTF-8-encoded Unicode code point from the buffer. 
    If no bytes are available, the error returned is io.EOF. 
    If the bytes are an erroneous UTF-8 encoding, it consumes one byte and returns U+FFFD, 1.
     从缓冲区里读取UTF-8字节。
     如果字节无效，将会返回错误 io.EOF。
     如果字节不是utf编码，会消耗一个字节 返回 U+FFFD, 1
```

```golang  
  func (b *Buffer) ReadString(delim byte) (line string, err error)
  		ReadString reads until the first occurrence of delim in the input, 
  		returning a string containing the data up to and including the delimiter. 
  		If ReadString encounters an error before finding a delimiter, 
  		it returns the data read before the error and the error itself (often io.EOF). 
  		ReadString returns err != nil if and only if the returned data does not end in delim.
  		从输入中读取直接遇到第一个分隔符，返回包含分隔符的字符串。
  		如果在遇到分隔符之前出现错误，返回遇到错误之前读取的数据和错误本身（通常是io.EOF）。只有在结束时也没有遇到分隔符才返回err!=nil
```

```golang  
  func (b *Buffer) Reset()
    Reset resets the buffer so it has no content. b.Reset() is the same as b.Truncate(0).
     重置缓冲区，使它没有内容。b.Reset()和b.Truncate(0)一样
```

```golang 
  func (b *Buffer) String() string
    String returns the contents of the unread portion of the buffer as a string. 
    If the Buffer is a nil pointer, it returns "<nil>".
     返回缓冲区中未读取的字符串。如果缓冲区是一个空指针，会返回 "<nil>"。
```

```golang  
  func (b *Buffer) Truncate(n int)
    Truncate discards all but the first n unread bytes from the buffer. 
    It panics if n is negative or greater than the length of the buffer.
     从缓冲区截取前n个字节，其他部分丢弃。n是负数或者大于缓冲区大小的时候会引发panic
```

```golang  
  func (b *Buffer) UnreadByte() error
    UnreadByte unreads the last byte returned by the most recent read operation. 
    If write has happened since the last read, UnreadByte returns an error.
     不读取最后一个字节，返回最近的读取操作。
     如果在上一次读取的时候发生写操作，会返回一个错误。
```

```golang
  func (b *Buffer) UnreadRune() error
    UnreadRune unreads the last rune returned by ReadRune. 
    If the most recent read or write operation on the buffer was not a ReadRune, UnreadRune returns an error. 
    (In this regard it is stricter than UnreadByte, which will unread the last byte from any read operation.)
```

```golang    
  func (b *Buffer) Write(p []byte) (n int, err error)
    Write appends the contents of p to the buffer, growing the buffer as needed. 
    The return value n is the length of p; err is always nil. 
    If the buffer becomes too large, Write will panic with ErrTooLarge.
    把p的内容加入到缓冲区里，如果缓冲区需要就自增大小。
    返回值是n的长度。err 经常是 nil。
    如果缓冲区太大了，Write 会引发 panic ErrTooLarge
```

```golang  
  func (b *Buffer) WriteByte(c byte) error
    WriteByte appends the byte c to the buffer, growing the buffer as needed. 
    The returned error is always nil, but is included to match bufio.Writer's WriteByte. 
    If the buffer becomes too large, WriteByte will panic with ErrTooLarge.
     把字节c加入缓冲区，如果缓冲区需要就自增大小。
     err 经常是 nil,但是包括匹配bufio.Writer's的WriteByte。
     如果缓冲区太大了，Write 会引发 panic ErrTooLarge。
```

```golang  
  func (b *Buffer) WriteRune(r rune) (n int, err error)
    WriteRune appends the UTF-8 encoding of Unicode code point r to the buffer, 
    returning its length and an error, which is always nil but is included to match bufio.Writer's WriteRune. 
    The buffer is grown as needed; if it becomes too large, WriteRune will panic with ErrTooLarge.
     把r追加到缓冲区。返回它的长度或者错误，
```

```golang  
  func (b *Buffer) WriteString(s string) (n int, err error)
    WriteString appends the contents of s to the buffer, growing the buffer as needed. 
    The return value n is the length of s; err is always nil. 
    If the buffer becomes too large, WriteString will panic with ErrTooLarge.
    把s的内容追加到缓冲区，如果需要增加缓冲区。返回值n是s的长度。
```

```golang  
  func (b *Buffer) WriteTo(w io.Writer) (n int64, err error)
    WriteTo writes data to w until the buffer is drained or an error occurs. 
    The return value n is the number of bytes written; 
    it always fits into an int, but it is int64 to match the io.WriterTo interface. 
    Any error encountered during the write is also returned.
     把数据写入w直到缓冲区结束或者遇到错误。
     返回值n是写入的字节数。
     通常是int,但是io.WriterTo接口是int64。
     如果写入的过程遇到错误也会跟着返回
```

```golang    
type Reader
    A Reader implements the io.Reader, io.ReaderAt, io.WriterTo, io.Seeker, io.ByteScanner, and io.RuneScanner 
    interfaces by reading from a byte slice. Unlike a Buffer, a Reader is read-only and supports seeking.
     读取字节切片实现io.Reader, io.ReaderAt, io.WriterTo, io.Seeker, io.ByteScanner, 和 io.RuneScanner interfaces，
     和Buffer不同的是，Reader是只读的并且支持seeking。
```  

```golang    
    func NewReader(b []byte) *Reader
      NewReader returns a new Reader reading from b.
      从b返回一个新的读取对象
```

```golang     
    func (r *Reader) Len() int
      Len returns the number of bytes of the unread portion of the slice.
      返回未读部分的切片的字节数。
```

```golang    
    func (r *Reader) Read(b []byte) (n int, err error)
```

```golang    
    func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)
```    
    
```golang
    func (r *Reader) ReadByte() (b byte, err error)
```

```golang    
    func (r *Reader) ReadRune() (ch rune, size int, err error)
```

```golang
    func (r *Reader) Seek(offset int64, whence int) (int64, error)
      Seek implements the io.Seeker interface.
      实现io.Seeker接口
```    

```golang
    func (r *Reader) UnreadByte() error
```

```golang    
    func (r *Reader) UnreadRune() error
```

```golang    
    func (r *Reader) WriteTo(w io.Writer) (n int64, err error)
      WriteTo implements the io.WriterTo interface.
```