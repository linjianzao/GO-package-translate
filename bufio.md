```golang
Package bufio implements buffered I/O. It wraps an io.Reader or io.Writer object, creating another object (Reader or Writer) that also implements the interface but provides buffering and some help for textual I/O.
bufio包实现缓冲区I/O。包含一个io.Reader或io.Writer对象，创建其他对象也实现了接口 提供缓冲和一些帮助 I/O。
```

Constants
```golang
const (
    // Maximum size used to buffer a token. The actual maximum token size
    // may be smaller as the buffer may need to include, for instance, a newline.
    MaxScanTokenSize = 64 * 1024
)
```

Variables
```golang
var (
    ErrInvalidUnreadByte = errors.New("bufio: invalid use of UnreadByte")
    ErrInvalidUnreadRune = errors.New("bufio: invalid use of UnreadRune")
    ErrBufferFull        = errors.New("bufio: buffer full")
    ErrNegativeCount     = errors.New("bufio: negative count")
)
var (
    ErrTooLong         = errors.New("bufio.Scanner: token too long")
    ErrNegativeAdvance = errors.New("bufio.Scanner: SplitFunc returns negative advance count")
    ErrAdvanceTooFar   = errors.New("bufio.Scanner: SplitFunc returns advance count beyond input")
)
Errors returned by Scanner.
```

```golang
func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)
  ScanBytes is a split function for a Scanner that returns each byte as a token.
  ScanBytes是一个切分Scanner函数，返回data中的第一个单个字节
```

```golang
func ScanLines(data []byte, atEOF bool) (advance int, token []byte, err error)
  ScanLines is a split function for a Scanner that returns each line of text, stripped of any trailing end-of-line marker. 
  The returned line may be empty. The end-of-line marker is one optional carriage return followed by one mandatory newline. 
  In regular expression notation, it is `\r?\n`. The last non-empty line of input will be returned even if it has no newline.
  ScanLines是一个切分函数。返回每一行文本，消除任何结束标记。
  返回的行有可能是空的，行尾标记是可选项，强制性的接在返回的行尾。
  行尾可能是\n 或 	\r\n， 返回值不包含行尾标记
```

```golang  
func ScanRunes(data []byte, atEOF bool) (advance int, token []byte, err error)
  ScanRunes is a split function for a Scanner that returns each UTF-8-encoded rune as a token.
  The sequence of runes returned is equivalent to that from a range loop over the input as a string, which means that erroneous UTF-8 encodings translate to U+FFFD = "\xef\xbf\xbd". 
  Because of the Scan interface, this makes it impossible for the client to distinguish correctly encoded replacement runes from encoding errors.
  ScanRunes是一个切分函数 找出data中的单个utf8编码并返回
   返回的顺序是输入字符串的顺序，如果utf8解码错误，U+FFFD 会被做为 "\xef\xbf\xbd"返回. 
   因为输入接口，可能让正确的编码替换掉错误的
```

```golang
func ScanWords(data []byte, atEOF bool) (advance int, token []byte, err error)
  ScanWords is a split function for a Scanner that returns each space-separated word of text, 
  with surrounding spaces deleted. It will never return an empty string. The definition of space is set by unicode.IsSpace.
  找出data中的单词，以空白字符分隔。不会返回空字符串。空白由unicode.IsSpace定义
```

```golang
type ReadWriter
  type ReadWriter struct {
        *Reader
        *Writer
  }
  ReadWriter stores pointers to a Reader and a Writer. It implements io.ReadWriter.
  存储一个Reader 和 Writer 指针，实现了io.ReadWriter
```

```golang
func NewReadWriter(r *Reader, w *Writer) *ReadWriter
  NewReadWriter allocates a new ReadWriter that dispatches to r and w.
  分配一个新的ReadWriter 封装r  和 w
```  

```golang
  type Reader struct {
        // contains filtered or unexported fields
  }
  Reader implements buffering for an io.Reader object.
   实现缓冲的io.Reader对象。
```

```golang
  func NewReader(rd io.Reader) *Reader
    NewReader returns a new Reader whose buffer has the default size.
     返回一个新的Reader指针，默认缓冲区大小
```

```golang    
  func NewReaderSize(rd io.Reader, size int) *Reader
    NewReaderSize returns a new Reader whose buffer has at least the specified size. 
    If the argument io.Reader is already a Reader with large enough size, it returns the underlying Reader.
     返回一个新的有指定大小的Reader指针，如果 io.Reader的缓冲区够大，返回*Reader
```

```golang
  func (b *Reader) Buffered() int
    Buffered returns the number of bytes that can be read from the current buffer.
     返回当前可读的缓冲区字节数
```

```golang  
  func (b *Reader) Peek(n int) ([]byte, error)
    Peek returns the next n bytes without advancing the reader. The bytes stop being valid at the next read call. 
    If Peek returns fewer than n bytes, it also returns an error explaining why the read is short. 
    The error is ErrBufferFull if n is larger than b's buffer size.
     没有超过缓冲区的时候，返回n字节的切片。引用的数据在下一次读取之前是有效的。
     如果返回的长度少于n的长度，返回为什么的短的错误信息
     如果n大于buffer的大小错误是ErrBufferFull
```

```golang  
  func (b *Reader) Read(p []byte) (n int, err error)
    Read reads data into p. It returns the number of bytes read into p.
    It calls Read at most once on the underlying Reader, hence n may be less than len(p). 
    At EOF, the count will be zero and err will be io.EOF.
     读取数据到P， 返回写入P的字节数。只调用一次Reader，只有在b中没有数据可读时才返回 0,io.EOF
```

```golang  
  func (b *Reader) ReadByte() (c byte, err error)
    ReadByte reads and returns a single byte. If no byte is available, returns an error.
     读取和返回单个字节，如果没有字节符合返回错误
```

```golang  
  func (b *Reader) ReadBytes(delim byte) (line []byte, err error)
    ReadBytes reads until the first occurrence of delim in the input, returning a slice containing the data up to and including the delimiter.
    If ReadBytes encounters an error before finding a delimiter, it returns the data read before the error and the error itself (often io.EOF).
    ReadBytes returns err != nil if and only if the returned data does not end in delim. For simple uses, a Scanner may be more convenient.
     读取输入的字节，直到第一次遇到delim，返回包含数据和分隔符的切片。
     如果函数在遇到分隔符之前出现错误，它返回读取到错误之前的数据和错误（错误经常是io.EOF）。
    ReadBytes只有在数据没有遇到分隔符但却已经读到结束的时候会返回err!=nil。对于简单的运用，Scanner 可能更适合
```

```golang  
  func (b *Reader) ReadLine() (line []byte, isPrefix bool, err error)
    ReadLine is a low-level line-reading primitive. Most callers should use ReadBytes('\n') or ReadString('\n') instead or use a Scanner.
    	ReadLine是一个低级的行读取方式。更多的会使用ReadBytes('\n')或者 ReadString('\n') 替代 或者用Scanner
    ReadLine tries to return a single line, not including the end-of-line bytes. If the line was too long for the buffer then isPrefix is set and the beginning of the line is returned.
    	ReadLine试图返回单行，不包含结束字节。如果缓冲区的行太长，isPrefix 设置返回行的开始。
	 The rest of the line will be returned from future calls. isPrefix will be false when returning the last fragment of the line. The returned buffer is only valid until the next call to ReadLine.
	 	其余的行将会在调用的时候返回。isPrefix 在返回完所有的行后，会变成false。返回的缓冲区在下次调用ReadLine之前是有效的。
	 ReadLine either returns a non-nil line or it returns an error, never both.
    	ReadLine 要嘛返回非空的行要嘛返回错误，不会两个都同时返回
    The text returned from ReadLine does not include the line end ("\r\n" or "\n"). No indication or error is given if the input ends without a final line end.
    	ReadLine 返回的文本不包含行结束的 ("\r\n" or "\n")。在最后一行结束之前是不会有任何迹象和错误的
```
   
```golang 
  func (b *Reader) ReadRune() (r rune, size int, err error)
    ReadRune reads  single UTF-8 encoded Unicode character and returns the rune and its size in bytes.
    If the encoded rune is invalid, it consumes one byte and returns unicode.ReplacementChar (U+FFFD) with a size of 1.
    ReadRune 读取单个utf8 格式的字符返回rune和rune的size。
     如果rune编码无效，它会消耗一个字节 返回unicode.ReplacementChar(U+FFFD)的1个大小
```

```golang
  func (b *Reader) ReadSlice(delim byte) (line []byte, err error)
    ReadSlice reads until the first occurrence of delim in the input, returning a slice pointing at the bytes in the buffer. 
    The bytes stop being valid at the next read call. If ReadSlice encounters an error before finding a delimiter, it returns all the data in the buffer and the error itself (often io.EOF). 
    ReadSlice fails with error ErrBufferFull if the buffer fills without a delim. 
    Because the data returned from ReadSlice will be overwritten by the next I/O operation, most clients should use ReadBytes or ReadString instead. 
    ReadSlice returns err != nil if and only if line does not end in delim.
     读取直到遇到分隔符delim，返回一个指向字节的缓冲区切片。
     下一次调用后失效。如果ReadSlice在遇到分隔符之前发生错误，将会返回缓冲区里的所有数据和错误，如果缓冲区满了，但是还没遇到分隔符就会有 ErrBufferFull的错误。
     因为ReadSlice 返回的数据会覆盖下一个IO操作，所以大部分的客户端用 ReadBytes 或 ReadString 替代。 
    ReadSlice 只会在行没有分隔符结束的情况下返回err != nil
```
  
```golang
  func (b *Reader) ReadString(delim byte) (line string, err error)
    ReadString reads until the first occurrence of delim in the input, returning a string containing the data up to and including the delimiter. 
    If ReadString encounters an error before finding a delimiter, it returns the data read before the error and the error itself (often io.EOF). 
    ReadString returns err != nil if and only if the returned data does not end in delim. For simple uses, a Scanner may be more convenient.
    ReadString读取直接遇到第一个分隔符。返回包含分隔符的字符串。
     如果ReadString在没有发现分隔符就遇到错误，返回发生错误之前读取的数据和错误。
    ReadString 只会在行没有分隔符结束的情况下返回err != nil。对于简单的运用，Scanner 更方便
```

```golang  
  func (b *Reader) UnreadByte() error
    UnreadByte unreads the last byte. Only the most recently read byte can be unread.
     撤销最后一次读书的字节。只有最后读的字节可以被撤销
```

```golang
  func (b *Reader) UnreadRune() error
    UnreadRune unreads the last rune. If the most recent read operation on the buffer was not a ReadRune, UnreadRune returns an error.
     撤销最后一次读出的rune.如果最后一次操作不是ReadRune，返回错误。
    (In this regard it is stricter than UnreadByte, which will unread the last byte from any read operation.)
    （在这方面，它比UnreadByte严格）   
```    

```golang
  func (b *Reader) WriteTo(w io.Writer) (n int64, err error)
    WriteTo implements io.WriterTo.
     实现io.WriterTo
```

```golang
type Scanner
  type Scanner struct {
        // contains filtered or unexported fields
  }
  Scanner provides a convenient interface for reading data such as a file of newline-delimited lines of text.
  	Scanner提供一个更方便的接口读取数据，像文件的新行分隔文本的行。
  Successive calls to the Scan method will step through the 'tokens' of a file, skipping the bytes between the tokens. 
  	继承Scan方法的使用tokens对文件单步调试调用，跳出标记之间的字节。
  The specification of a token is defined by a split function of type SplitFunc; the default split function breaks the input into lines with line termination stripped. 
  	token是一个SplitFunc类型的分割函数，默认的分割函数将输入的行中断。
  Split functions are defined in this package for scanning a file into lines, bytes, UTF-8-encoded runes, and space-delimited words. The client may instead provide a custom split function.
  	分割函数在这个包的是一个文件的行，字节，UTF8字节和间隔符。客户端可以自定义函数来替代分割函数。
  Scanning stops unrecoverably at EOF, the first I/O error, or a token too large to fit in the buffer. When a scan stops, the reader may have advanced arbitrarily far past the last token. 
  	搜索在遇到结束符、第一个I/O错误、或者token大于缓冲区的大小。当一个搜索结束，读取者也许会离最后一个token任意远。
  Programs that need more control over error handling or large tokens, or must run sequential scans on a reader, should use bufio.Reader instead.
  	程序在必须更多的控制错误处理或太大的tokens，或者连续的搜索，应该用 bufio.Reader代替
```


Example (Custom)
Use a Scanner with a custom split function (built by wrapping ScanWords) to validate 32-bit decimal input.
Code:
```golang
// An artificial input source.
const input = "1234 5678 1234567901234567890"
scanner := bufio.NewScanner(strings.NewReader(input))
// Create a custom split function by wrapping the existing ScanWords function.
split := func(data []byte, atEOF bool) (advance int, token []byte, err error) {
    advance, token, err = bufio.ScanWords(data, atEOF)
    if err == nil && token != nil {
        _, err = strconv.ParseInt(string(token), 10, 32)
    }
    return
}
// Set the split function for the scanning operation.
scanner.Split(split)
// Validate the input
for scanner.Scan() {
    fmt.Printf("%s\n", scanner.Text())
}

if err := scanner.Err(); err != nil {
    fmt.Printf("Invalid input: %s", err)
}
```
Output:
```golang
1234
5678
Invalid input: strconv.ParseInt: parsing "1234567901234567890": value out of range
```


```golang
Example (Lines)
The simplest use of a Scanner, to read standard input as a set of lines.

Code:

scanner := bufio.NewScanner(os.Stdin)
for scanner.Scan() {
    fmt.Println(scanner.Text()) // Println will add back the final '\n'
}
if err := scanner.Err(); err != nil {
    fmt.Fprintln(os.Stderr, "reading standard input:", err)
}
```



Use a Scanner to implement a simple word-count utility by scanning the input as a sequence of space-delimited tokens.
Code:
```golang
// An artificial input source.
const input = "Now is the winter of our discontent,\nMade glorious summer by this sun of York.\n"
scanner := bufio.NewScanner(strings.NewReader(input))
// Set the split function for the scanning operation.
scanner.Split(bufio.ScanWords)
// Count the words.
count := 0
for scanner.Scan() {
    count++
}
if err := scanner.Err(); err != nil {
    fmt.Fprintln(os.Stderr, "reading input:", err)
}
fmt.Printf("%d\n", count)
```
Output:
```golang
15
```


```golang
  func NewScanner(r io.Reader) *Scanner
    NewScanner returns a new Scanner to read from r. The split function defaults to ScanLines
    NewScanner 从r返回一个新的*Scanner。分割函数默认是ScanLines
```

```golang
  func (s *Scanner) Bytes() []byte
    Bytes returns the most recent token generated by a call to Scan. The underlying array may point to data that will be overwritten by a subsequent call to Scan. It does no allocation.
    Bytes 将最后一次扫描出的切片返回。调用Scan 会覆盖 底层数组指向的数据。
```

```golang
  func (s *Scanner) Err() error
    Err returns the first non-EOF error that was encountered by the Scanner.
    Err 在Scanner遇到第一个非EOF错误返回
```

```golang
  func (s *Scanner) Scan() bool
    Scan advances the Scanner to the next token, which will then be available through the Bytes or Text method. 
    It returns false when the scan stops, either by reaching the end of the input or an error. 
    After Scan returns false, the Err method will return any error that occurred during scanning, except that if it was io.EOF, Err will return nil.
    Scan 搜索下一个标记， 然后通过Bytes或者Text方法取出。
    当然搜索停止、搜索到末尾、或遇到错误时返回false。
    当Scan 返回false。Err 方法将会返回在搜索期间遇到的任何错误，除了io.EOF会返回nil。
```

```golang
  func (s *Scanner) Split(split SplitFunc)
    Split sets the split function for the Scanner. If called, it must be called before Scan. The default split function is ScanLines.
    Split 设置搜索的分割函数，如果有调用，必须在Scan之前调用，默认的分割函数是ScanLines
```

```golang
  func (s *Scanner) Text() string
    Text returns the most recent token generated by a call to Scan as a newly allocated string holding its bytes.
    Text 返回在调用Scan重新分配字符串的时候,最后一次扫描的字符串
```

```golang
type SplitFunc
  type SplitFunc func(data []byte, atEOF bool) (advance int, token []byte, err error)
    SplitFunc is the signature of the split function used to tokenize the input. 
    The arguments are an initial substring of the remaining unprocessed data and a flag, atEOF, that reports whether the Reader has no more data to give. 
    The return values are the number of bytes to advance the input and the next token to return to the user, plus an error, if any. 
    If the data does not yet hold a complete token, for instance if it has no newline while scanning lines,
    SplitFunc can return (0, nil) to signal the Scanner to read more data into the slice and try again with a longer slice starting at the same point in the input.
    
    SplitFunc是分割函数用于标记输入。参数是Reader有没有更多的数据时的一个未处理的字串和一个atEOF标示
     返回值是读取到下一个标示的字节数，如果有错误的话加上一个错误。
     如果数据没有一个完整的标示，例如扫描行的时候没有新行，
    SplitFunc return (0, nil)的标示，Scanner 在输入的时候尝试用一个更长的切片的同一个指针开始 ， 从切片中读取更多的数据
     
    If the returned error is non-nil, scanning stops and the error is returned to the client.
     如果返回的错误不是nil，扫描终止，错误返回给客户端。
    The function is never called with an empty data slice unless atEOF is true. If atEOF is true, however, data may be non-empty and, as always, holds unprocessed text.
     除非atEOF 参数的值是true，否则函数不会调用一个空数据的切片。如果atEOF 是true，data参数有可能是非空的，通常是未处理的数据
```


```golang  
type Writer
  Writer implements buffering for an io.Writer object. If an error occurs writing to a Writer, no more data will be accepted and all subsequent writes will return the error.
  Writer实现 io.Writer 缓冲区对象。如果遇到错误，就不会再写入数据，并且后面的写入都会返回错误。
```

```golang  
  func NewWriter(wr io.Writer) *Writer
    NewWriter returns a new Writer whose buffer has the default size.
    NewWriter 返回一个有默认大小的缓冲区Writer
```

```golang    
  func NewWriterSize(wr io.Writer, size int) *Writer
    NewWriterSize returns a new Writer whose buffer has at least the specified size. If the argument io.Writer is already a Writer with large enough size, it returns the underlying Writer.
    NewWriterSize 返回一个至少有指定大小的缓冲区新对象。如果参数io.Writer是一个足够大小的Writer，会返回Writer。 （注：就是说谁大返回谁）
```

```golang    
  func (b *Writer) Available() int
    Available returns how many bytes are unused in the buffer.
    Available 返回缓冲区还有多少字节没有被使用。
```

```golang  
  func (b *Writer) Buffered() int
    Buffered returns the number of bytes that have been written into the current buffer.
    Buffered 返回已经被写入的缓冲区的字节数。
```

```golang 
  func (b *Writer) Flush() error
    Flush writes any buffered data to the underlying io.Writer.
    Flush 把缓冲区数据写入到底层io.Writer
```

```golang    
  func (b *Writer) ReadFrom(r io.Reader) (n int64, err error)
    ReadFrom implements io.ReaderFrom.
    ReadFrom 实现 io.ReaderFrom.
```

```golang
	func (b *Writer) Reset(w io.Writer)
		Reset discards any unflushed buffered data, clears any error, and resets b to write its output to w.
		丢弃缓冲区里未刷新的数据，清除所有错误，重置b写入到w
```

```golang
  func (b *Writer) Write(p []byte) (nn int, err error)
    Write writes the contents of p into the buffer. It returns the number of bytes written. If nn < len(p), it also returns an error explaining why the write is short.
    Write将p的内容写入缓冲区。返回写入字节数。如果返回的字节数nn 小于 p的长度，它也会返回一个错误说明为什么写入短了。
```

```golang
  func (b *Writer) WriteByte(c byte) error
    WriteByte writes a single byte.
    WriteByte写入单字节
```

```golang  
  func (b *Writer) WriteRune(r rune) (size int, err error)
    WriteRune writes a single Unicode code point, returning the number of bytes written and any error.
    WriteRune写入单个Unicode编码字符。返回写入的字节数并返回遇到的错误。
```

```golang  
  func (b *Writer) WriteString(s string) (int, error)
    WriteString writes a string. It returns the number of bytes written. If the count is less than len(s), it also returns an error explaining why the write is short.
    WriteString 写入一个字符串。返回写入的字节数。如果字节数少于s参数的长度，它也会返回一个错误说明为什么写入短了。
```    
    
    
