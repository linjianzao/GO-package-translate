Package io

Overview ▾

Package io provides basic interfaces to I/O primitives. 
Its primary job is to wrap existing implementations of such primitives, such as those in package os, into shared public interfaces that abstract the functionality, plus some other related primitives.

Because these interfaces and primitives wrap lower-level operations with various implementations, unless otherwise informed clients should not assume they are safe for parallel execution.

io包提供基础I/O基元接口
它的主要工作是包装了这样的原语的现有实现,这些在os包里也是这样的, 共享公共的接口的抽象的功能,再加上一些其他相关的原函数。
因为这些接口和原函数包装 低级操作和各种实现,除非另有通知客户端不应该假定他们是安全的并行执行。


Variables

var EOF = errors.New("EOF")
EOF is the error returned by Read when no more input is available. 
Functions should return EOF only to signal a graceful end of input. 
If the EOF occurs unexpectedly in a structured data stream, the appropriate error is either ErrUnexpectedEOF or some other error giving more detail.
当没有更多的有效输入是 Read返回 EOF错误.
函数只有在单个输入结束时才 返回 EOF错误.
如果EOF在 一个结构体数据流遇到 未导出, 相应的错误 不是 ErrUnexpectedEOF 就是其他给更详细的错误.


var ErrClosedPipe = errors.New("io: read/write on closed pipe")
ErrClosedPipe is the error used for read or write operations on a closed pipe.
用于读取或在封闭的管道写入操作的错误。

var ErrNoProgress = errors.New("multiple Read calls return no data or error")
ErrNoProgress is returned by some clients of an io.Reader when many calls to Read have failed to return any data or error, usually the sign of a broken io.Reader implementation.
使用  io.Reader 返回  当 多个调用Read 有失败的 任何数据或错误,通常是一个坏的io.Reader实现符号。

var ErrShortBuffer = errors.New("short buffer")
ErrShortBuffer means that a read required a longer buffer than was provided.
也就是说，读出 所需的比提供了缓冲器要长。

var ErrShortWrite = errors.New("short write")
ErrShortWrite means that a write accepted fewer bytes than requested but failed to return an explicit error.
就是说 一个写入接收少于需要的字节但是 失败返回一个明确的错误。

var ErrUnexpectedEOF = errors.New("unexpected EOF")
ErrUnexpectedEOF means that EOF was encountered in the middle of reading a fixed-size block or data structure.
也就是说，EOF在阅读一个固定大小的块或数据结构，中间遇到的。



func Copy

func Copy(dst Writer, src Reader) (written int64, err error)
Copy copies from src to dst until either EOF is reached on src or an error occurs. 
It returns the number of bytes copied and the first error encountered while copying, if any.

A successful Copy returns err == nil, not err == EOF. 
Because Copy is defined to read from src until EOF, it does not treat an EOF from Read as an error to be reported.

If src implements the WriterTo interface, the copy is implemented by calling src.WriteTo(dst). 
Otherwise, if dst implements the ReaderFrom interface, the copy is implemented by calling dst.ReadFrom(src).
从SRC拷贝到DST，直到在SRC达到EOF或发生错误
它返回复制字节的数量,  如果有遇到错误 返回到第一次遇到错误的 复制

一个成功的Copy 返回err == nil 而不是  err == EOF.
因为Copy 从 src 读取直接EOF, 它不是真的 从Read中 把EOF当成一个错误来报告的.

如果src实现WriterTo 接口,copy 调用  src.WriteTo(dst)实现.
否则, 如果 dst实现 ReaderFrom 接口,copy 调用 dst.ReadFrom(src)实现


func CopyN

func CopyN(dst Writer, src Reader, n int64) (written int64, err error)
CopyN copies n bytes (or until an error) from src to dst. 
It returns the number of bytes copied and the earliest error encountered while copying. 
On return, written == n if and only if err == nil.

If dst implements the ReaderFrom interface, the copy is implemented using it.
从src复制n个字节到dst
它返回复制字节数并且 复制中遇到错误 返回最早的错误.
在返回是那个, 只有 err == nil才会 written == n
如果dst实现ReaderFrom接口,copy 实现使用它实现


func ReadAtLeast

func ReadAtLeast(r Reader, buf []byte, min int) (n int, err error)
ReadAtLeast reads from r into buf until it has read at least min bytes. 
It returns the number of bytes copied and an error if fewer bytes were read. 
The error is EOF only if no bytes were read. 
If an EOF happens after reading fewer than min bytes, ReadAtLeast returns ErrUnexpectedEOF. 
If min is greater than the length of buf, ReadAtLeast returns ErrShortBuffer. 
On return, n >= min if and only if err == nil.
从r中读取 到buf 直到读取到最小字节
它返回复制的字节数,如果被读取的字节数太小会有个错误
如果没有字节读取, 错误是EOF
如果在读取少于最小字节会发生EOF,ReadAtLeast 返回ErrUnexpectedEOF
如果min比buf的长度大,ReadAtLeast返回ErrShortBuffer
返回值中, 只有 err == nil ,  n >= min



func ReadFull

func ReadFull(r Reader, buf []byte) (n int, err error)
ReadFull reads exactly len(buf) bytes from r into buf. 
It returns the number of bytes copied and an error if fewer bytes were read. 
The error is EOF only if no bytes were read. 
If an EOF happens after reading some but not all the bytes, ReadFull returns ErrUnexpectedEOF. 
On return, n == len(buf) if and only if err == nil.
ReadFull 从r中读取刚好len(buf)字节数 到buf
它返回复制的字节数 ,如果读取字节少 会有个错误.
如果没有字节读, 错误是EOF
如果读取一些但不是所有的字节 会发生EOF ,ReadFull返回 ErrUnexpectedEOF
返回值中, 只有 err == nil , n == len(buf)


func WriteString

func WriteString(w Writer, s string) (n int, err error)
WriteString writes the contents of the string s to w, which accepts an array of bytes. 
If w already implements a WriteString method, it is invoked directly.
把字符串s的内容写入w, 接收任何的 数组字节
如果w已经实现了 WriteString 方法,它会直接调用


type ByteReader

type ByteReader interface {
        ReadByte() (c byte, err error)
}
ByteReader is the interface that wraps the ReadByte method.

ReadByte reads and returns the next byte from the input. If no byte is available, err will be set.
ByteReader是包装 ReadByte方法的接口
从输入中读取和返回下一个字节.如果没有字节有效,会设置 err


type ByteScanner

type ByteScanner interface {
        ByteReader
        UnreadByte() error
}
ByteScanner is the interface that adds the UnreadByte method to the basic ReadByte method.

UnreadByte causes the next call to ReadByte to return the same byte as the previous call to ReadByte. 
It may be an error to call UnreadByte twice without an intervening call to ReadByte.
添加UnreadByte方法 到 基础ReadByte 方法的接口
UnreadByte 导致 下一个调用ReadByte 和前一个调用ReadByte返回字节一样
在调用两次UnreadByte之间 没有调用ReadByte 可能会遇到错误


type ByteWriter

type ByteWriter interface {
        WriteByte(c byte) error
}
ByteWriter is the interface that wraps the WriteByte method.
包装WriteByte方法 接口


type Closer

type Closer interface {
        Close() error
}
Closer is the interface that wraps the basic Close method.

The behavior of Close after the first call is undefined. 
Specific implementations may document their own behavior.
包装基础Close方法的接口
在第一次调用后 Close 是undefined


type LimitedReader

type LimitedReader struct {
        R Reader // underlying reader 底层读写器
        N int64  // max bytes remaining 最大剩余字节
}
A LimitedReader reads from R but limits the amount of data returned to just N bytes. 
Each call to Read updates N to reflect the new amount remaining.
从R中读取 但是 限制返回数据数量 刚好到 N字节.
每次调用Read 更新N ,反射的 剩余数量

func (*LimitedReader) Read

func (l *LimitedReader) Read(p []byte) (n int, err error)



type PipeReader

type PipeReader struct {
        // contains filtered or unexported fields
}
A PipeReader is the read half of a pipe.
是读出的管道的一半。


func Pipe

func Pipe() (*PipeReader, *PipeWriter)
Pipe creates a synchronous in-memory pipe. 
It can be used to connect code expecting an io.Reader with code expecting an io.Writer. 
Reads on one end are matched with writes on the other, copying data directly between the two; 
there is no internal buffering. 
It is safe to call Read and Write in parallel with each other or with Close. 
Close will complete once pending I/O is done. 
Parallel calls to Read, and parallel calls to Write, are also safe: the individual calls will be gated sequentially.
创建一个 同步的内存 管道.
它可以用来 连接代码io.Reader  和 代码 io.Writer. 
读取一个结束匹配 写入的其他,在两个之间直接复制数据;
没有内部缓冲区.
它安全的并联调用Read和Write 每个其他的 或关闭的
Close将完成一次挂起的I/ O完成
并行调用Read, 并且 并行的调用Write,都是安全的:个别调用将被依次选通。


func (*PipeReader) Close

func (r *PipeReader) Close() error
Close closes the reader; 
subsequent writes to the write half of the pipe will return the error ErrClosedPipe.
关闭reader
随后写入的管道 将会返回错误ErrClosedPipe


func (*PipeReader) CloseWithError

func (r *PipeReader) CloseWithError(err error) error
CloseWithError closes the reader; 
subsequent writes to the write half of the pipe will return the error err.
关闭reader
随后写入的管道 将会返回错误err


func (*PipeReader) Read

func (r *PipeReader) Read(data []byte) (n int, err error)
Read implements the standard Read interface: it reads data from the pipe, blocking until a writer arrives or the write end is closed. 
If the write end is closed with an error, that error is returned as err; otherwise err is EOF.
实现标准的Read接口:从管道中读取数据, 阻塞 直到到达writer或 writer关闭
如果写入接口是 和一个错误关闭的,那个错误会以 err 返回, 否则err是EOF


type PipeWriter

type PipeWriter struct {
        // contains filtered or unexported fields
}
A PipeWriter is the write half of a pipe.
PipeWriter是 写入管道的一半

func (*PipeWriter) Close

func (w *PipeWriter) Close() error
Close closes the writer; 
subsequent reads from the read half of the pipe will return no bytes and EOF.
关闭writer
从读取管道的一半读取,将会返回没有字节和EOF


func (*PipeWriter) CloseWithError

func (w *PipeWriter) CloseWithError(err error) error
CloseWithError closes the writer; 
subsequent reads from the read half of the pipe will return no bytes and the error err.
关闭writer
随后 从管道的一半读取, 将会返回没有字节和错误 err


func (*PipeWriter) Write

func (w *PipeWriter) Write(data []byte) (n int, err error)
Write implements the standard Write interface: 
	it writes data to the pipe, blocking until readers have consumed all the data or the read end is closed. 
	If the read end is closed with an error, that err is returned as err; otherwise err is ErrClosedPipe.
Write实现标准Write接口:
写入数据到管道,阻塞 直到消耗所有的数据或者读取关闭
如果读取关闭 有错误, err返回 err  否则 err是ErrClosedPipe


type ReadCloser

type ReadCloser interface {
        Reader
        Closer
}
ReadCloser is the interface that groups the basic Read and Close methods.
基础Read和Close方法 组 接口


type ReadSeeker

type ReadSeeker interface {
        Reader
        Seeker
}
ReadSeeker is the interface that groups the basic Read and Seek methods.
基础Read和Seek方法 组 接口


type ReadWriteCloser

type ReadWriteCloser interface {
        Reader
        Writer
        Closer
}
ReadWriteCloser is the interface that groups the basic Read, Write and Close methods.
基础Read,Write和Close方法 组 接口


type ReadWriteSeeker

type ReadWriteSeeker interface {
        Reader
        Writer
        Seeker
}
ReadWriteSeeker is the interface that groups the basic Read, Write and Seek methods.
基础Read,Write和Seek方法 组 接口


type ReadWriter

type ReadWriter interface {
        Reader
        Writer
}
ReadWriter is the interface that groups the basic Read and Write methods.
基础Read和Write方法 组 接口


type Reader

type Reader interface {
        Read(p []byte) (n int, err error)
}
Reader is the interface that wraps the basic Read method.
包装基础Read方法的接口
Read reads up to len(p) bytes into p. 
It returns the number of bytes read (0 <= n <= len(p)) and any error encountered. 
Even if Read returns n < len(p), it may use all of p as scratch space during the call. 
If some data is available but not len(p) bytes, Read conventionally returns what is available instead of waiting for more.
读取 len(p)长度字节到p
返回读取的字节数 (0 <= n <= len(p))和任何遇到的错误
即使Read 返回n < len(p),它可以在调用过程中使用的所有P的暂存空间。
如果一些数据有效,但没有len(p) 字节,Read返回这些有效数据 而不是等待更多

When Read encounters an error or end-of-file condition after successfully reading n > 0 bytes, it returns the number of bytes read. 
It may return the (non-nil) error from the same call or return the error (and n == 0) from a subsequent call.
An instance of this general case is that a Reader returning a non-zero number of bytes at the end of the input stream may return either err == EOF or err == nil. 
The next Read should return 0, EOF regardless.
当Read遇到错误或 文件结束条件后，成功地读取n>0字节,它返回读取的字节数
同一个调用可能会返回错误 或者 从随后的调用中返回错误(和 n == 0).
这个一般情况下，一个实例是 一个Reader 返回 非零 字节数 在输入流的最后可能返回  err == EOF 或 err == nil.
下一个 Read应该返回0,EOF不管。

Callers should always process the n > 0 bytes returned before considering the error err. 
Doing so correctly handles I/O errors that happen after reading some bytes and also both of the allowed EOF behaviors.

Implementations of Read are discouraged from returning a zero byte count with a nil error, and callers should treat that situation as a no-op.
调用者应该总是处理之前 考虑 错误 err 返回n>0字节.
正确地这样处理的I/O读取一些字节，也都允许EOF行为之后发生的错误。
从返回一个零字节数和一个 nil 错误 实现Read被阻,调用者应该把这种情况当成一个空操作


func LimitReader

func LimitReader(r Reader, n int64) Reader
LimitReader returns a Reader that reads from r but stops with EOF after n bytes. 
The underlying implementation is a *LimitedReader.
返回一个从r读取的 Reader 但是在n个字节值之后应该以EOF停止
底层实现是一个*LimitedReader

func MultiReader

func MultiReader(readers ...Reader) Reader
MultiReader returns a Reader that's the logical concatenation of the provided input readers. 
They're read sequentially. 
Once all inputs have returned EOF, Read will return EOF. 
If any of the readers return a non-nil, non-EOF error, Read will return that error.
返回一个逻辑串联提供的输入readers 的Reader
他们按顺序读取。
所有的输入一有返回EOF, Read 将会返回EOF
如果有任何readers返回非 nil , 非EOF 错误,Read会返回那个错误


func TeeReader

func TeeReader(r Reader, w Writer) Reader
TeeReader returns a Reader that writes to w what it reads from r. 
All reads from r performed through it are matched with corresponding writes to w. 
There is no internal buffering - the write must complete before the read completes. 
Any error encountered while writing is reported as a read error.
返回一个从r读取写入w的Reader.
所有的通过它执行r读取都可以匹配相应的写入w
者没有内部缓冲区,写入必须在读取完全后.
任何遇到的错误 写入都会报告 成 读取错误


type ReaderAt

type ReaderAt interface {
        ReadAt(p []byte, off int64) (n int, err error)
}
ReaderAt is the interface that wraps the basic ReadAt method.
ReaderAt包装基础ReadAt 方法的接口

ReadAt reads len(p) bytes into p starting at offset off in the underlying input source. 
It returns the number of bytes read (0 <= n <= len(p)) and any error encountered.
读取len(p)  字节到 p  从底层输入源的偏移开始

When ReadAt returns n < len(p), it returns a non-nil error explaining why more bytes were not returned. 
In this respect, ReadAt is stricter than Read.
当ReadAt 返回n < len(p), 它返回一个 非nil错误解释为什么没有更多字节返回
为此,ReadAt 比Read 更严格.

Even if ReadAt returns n < len(p), it may use all of p as scratch space during the call. 
If some data is available but not len(p) bytes, ReadAt blocks until either all the data is available or an error occurs. 
In this respect ReadAt is different from Read.
即使 如果 ReadAt 返回 n < len(p),它可以在调用过程中使用的所有P的暂存空间。
如果一些数据有效 但没有 到 len(p)字节,ReadAt块,直到所有的数据是可用或发生错误.
在这方面ReadAt和Read是不一样的

If the n = len(p) bytes returned by ReadAt are at the end of the input source, ReadAt may return either err == EOF or err == nil.

If ReadAt is reading from an input source with a seek offset, ReadAt should not affect nor be affected by the underlying seek offset.

Clients of ReadAt can execute parallel ReadAt calls on the same input source.

如果 n = len(p) 字节  , ReadAt  返回输入源的结尾,ReadAt 也可能会返回 err == EOF  或 err == nil
如果ReadAt 从输入源以一个 seek 偏移读取,ReadAt 不会影响到底层的 seek 偏移
ReadAt客户端 可以执行并行, ReadAt可以调用相同的输入源


type ReaderFrom

type ReaderFrom interface {
        ReadFrom(r Reader) (n int64, err error)
}
ReaderFrom is the interface that wraps the ReadFrom method.

ReadFrom reads data from r until EOF or error. 
The return value n is the number of bytes read. 
Any error except io.EOF encountered during the read is also returned.

The Copy function uses ReaderFrom if available.

ReaderFrom是包装了ReadFrom方法接口
ReadFrom从r中读取数据 直到遇到 EOF 或 错误.
返回值n 是 读取的字节数
任何在读取的过程遇到的错误除 io.EOF 都会被返回 
如果有效的话,Copy函数会使用 ReaderFrom


type RuneReader

type RuneReader interface {
        ReadRune() (r rune, size int, err error)
}
RuneReader is the interface that wraps the ReadRune method.

ReadRune reads a single UTF-8 encoded Unicode character and returns the rune and its size in bytes.
If no character is available, err will be set.
RuneReader是包装了ReadRune方法的接口。
ReadRune读取单个的 UTF-8 编码 Unicode 字符 并且返回 以字节为单位的rune和它的大小。


type RuneScanner

type RuneScanner interface {
        RuneReader
        UnreadRune() error
}
RuneScanner is the interface that adds the UnreadRune method to the basic ReadRune method.

UnreadRune causes the next call to ReadRune to return the same rune as the previous call to ReadRune. 
It may be an error to call UnreadRune twice without an intervening call to ReadRune.
RuneScanner是添加了 UnreadRune方法到 基础 ReadRune 方法的接口
UnreadRune引起下一个调用 ReadRune返回 和上一次调用ReadRune  相同的 rune 
在两次调用UnreadRune中间没有调用ReadRune  可能会有错误.


func NewSectionReader

func NewSectionReader(r ReaderAt, off int64, n int64) *SectionReader
NewSectionReader returns a SectionReader that reads from r starting at offset off and stops with EOF after n bytes.
NewSectionReader 返回一个 SectionReader 用来 从r中从 偏移开始 读取  到n个字节后  的EOF结束


func (*SectionReader) Read

func (s *SectionReader) Read(p []byte) (n int, err error)


func (*SectionReader) ReadAt

func (s *SectionReader) ReadAt(p []byte, off int64) (n int, err error)


func (*SectionReader) Seek

func (s *SectionReader) Seek(offset int64, whence int) (int64, error)


func (*SectionReader) Size

func (s *SectionReader) Size() int64
Size returns the size of the section in bytes.
以字节 返回 章节的大小


type Seeker

type Seeker interface {
        Seek(offset int64, whence int) (int64, error)
}
Seeker is the interface that wraps the basic Seek method.

Seek sets the offset for the next Read or Write to offset, interpreted according to whence: 
	0 means relative to the origin of the file, 
	1 means relative to the current offset, and 
	2 means relative to the end. 
	Seek returns the new offset and an error, if any.

Seeking to a negative offset is an error. 
Seeking to any positive offset is legal, but the behavior of subsequent I/O operations on the underlying object is implementation-dependent.
Seeker是包装了 基础 Seek 方法 的接口
Seek 设置 下一个Read 或 Write 偏移量,根据此处解释:
	0意味着相对于文件的原点，
	1意味着相对于当前的偏移量
	2意味着相对于结束点
	Seek 返回新的 偏移和 遇到的任何错误
Seeking负偏移量是一个错误.
Seeking 任何正偏移都是合法的,但底层的对象在随后的I / O操作的行为是依赖于实现。
	

type WriteCloser

type WriteCloser interface {
        Writer
        Closer
}
WriteCloser is the interface that groups the basic Write and Close methods.
WriteCloser是 群组的基础Write 和 Close方法接口


type WriteSeeker

type WriteSeeker interface {
        Writer
        Seeker
}
WriteSeeker is the interface that groups the basic Write and Seek methods.
WriteSeeker是 群组的基础Write 和 Seek方法接口



type Writer

type Writer interface {
        Write(p []byte) (n int, err error)
}
Writer is the interface that wraps the basic Write method.

Write writes len(p) bytes from p to the underlying data stream. 
It returns the number of bytes written from p (0 <= n <= len(p)) and any error encountered that caused the write to stop early. 
Write must return a non-nil error if it returns n < len(p). 
Write must not modify the slice data, even temporarily.
Writer是包装基础Write方法的基础
Write从p中写入 len(p) 字节长度 到底层数据流
它返回从p写入的字节数(0 <= n <= len(p)) 和任何遇到的导致写入停止的错误
如果n < len(p) 那 Write 必须返回一个非nil错误
Write 不允许修改slice数据,即使只是暂时的 



func MultiWriter

func MultiWriter(writers ...Writer) Writer
MultiWriter creates a writer that duplicates its writes to all the provided writers, similar to the Unix tee(1) command.
MultiWriter创建一个writer 复制其写入到所有的writers提供,类似Unix的tee(1)命令 


type WriterAt

type WriterAt interface {
        WriteAt(p []byte, off int64) (n int, err error)
}
WriterAt is the interface that wraps the basic WriteAt method.

WriteAt writes len(p) bytes from p to the underlying data stream at offset off. 
It returns the number of bytes written from p (0 <= n <= len(p)) and any error encountered that caused the write to stop early. 
WriteAt must return a non-nil error if it returns n < len(p).

If WriteAt is writing to a destination with a seek offset, WriteAt should not affect nor be affected by the underlying seek offset.

Clients of WriteAt can execute parallel WriteAt calls on the same destination if the ranges do not overlap.
WriterAt包装基础WriteAt方法接口.
WriteAt 从p 写入 len(p)长度的 字节到偏移的底层数据流
它返回从p写入的字节数(0 <= n <= len(p)) 和任何遇到的导致写入停止的错误
如果n < len(p) 那 WriteAt 必须返回一个非nil错误
如果WriteAt 写入的 最终是一个 seek 偏移,WriteAt不应该影响也不受到相关 底层seek 偏移影响
如果范围没有重叠,WriteAt客户端可以在同一个目的 并行执行 WriteAt



type WriterTo

type WriterTo interface {
        WriteTo(w Writer) (n int64, err error)
}
WriterTo is the interface that wraps the WriteTo method.

WriteTo writes data to w until there's no more data to write or when an error occurs. 
The return value n is the number of bytes written. 
Any error encountered during the write is also returned.

The Copy function uses WriterTo if available.
WriterTo是包装 WriteTo方法的接口
WriteTo 写入数据到w 直到没有更多的数据 可写 或 遇到错误
返回值n是写入的字节数
任何写入过程遇到的错误都会返回



















































































































