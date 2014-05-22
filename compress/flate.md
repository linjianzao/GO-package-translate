包地址：http://golang.org/pkg/compress/flate/
```golang
Package flate implements the DEFLATE compressed data format, described in RFC 1951. The gzip and zlib packages implement access to DEFLATE-based file formats.
 实现 RFC 1951 中的 DEFLATE格式的数据压缩.gzip和zlib包实现访问 DEFLATE-based文件格式
```

```golang
Constants

const (
    NoCompression = 0
    BestSpeed     = 1

    BestCompression    = 9
    DefaultCompression = -1
)
```

```golang
func NewReader(r io.Reader) io.ReadCloser
  NewReader returns a new ReadCloser that can be used to read the uncompressed version of r. 
  It is the caller's responsibility to call Close on the ReadCloser when finished reading.
  返回一个可以用来被读取 未压缩的r的 ReadCloser。在结束读取后调用者应该 调用ReadCloser中的Close。
 	注:也就是实现了 io.ReadCloser
```

```golang
func NewReaderDict(r io.Reader, dict []byte) io.ReadCloser
  NewReaderDict is like NewReader but initializes the reader with a preset dictionary. 
  The returned Reader behaves as if the uncompressed data stream started with the given dictionary, which has already been read. 
  NewReaderDict is typically used to read data compressed by NewWriterDict.
  像NewReader 但是初始化预设字典。
  以给定的字典开始返回未压缩且已读取的的数据流。（注：给定的dict）
  NewReaderDict通常用于读取数据压缩NewWriterDict。
```

```golang
type CorruptInputError
  A CorruptInputError reports the presence of corrupt input at a given offset.
  报告存在的错误输入的偏移量
```

```golang  
  func (e CorruptInputError) Error() string
```

```golang    
type InternalError
    An InternalError reports an error in the flate code itself.
    报告它本身的错误。
```

```golang    
    func (e InternalError) Error() string
```

```golang
type ReadError
	type ReadError struct {
    Offset int64 // byte offset where error occurred
    Err    error // error returned by underlying Read
	}
  A ReadError reports an error encountered while reading input.
  报告读取输入遇到的错误
```

```golang  
    func (e *ReadError) Error() string
```

```golang    
type Reader
  type Reader interface {
        io.Reader
        ReadByte() (c byte, err error)
  }
  The actual read interface needed by NewReader. 
  If the passed in io.Reader does not also have ReadByte, the NewReader will introduce its own buffering.
  实际读取接口需要 NewReader。如果传递的io.Reader也没有ReadByte。NewReader会自增自己的缓冲区
```

```golang
	func (e *ReadError) Error() string
```

```golang
type WriteError
  type WriteError struct {
        Offset int64 // byte offset where error occurred
        Err    error // error returned by underlying Write
  }
  A WriteError reports an error encountered while writing output.
  写输出遇到问题时报告
```

```golang
    func (e *WriteError) Error() string
```

```golang    
type Writer
	type Writer struct {
    // contains filtered or unexported fields
	}
  A Writer takes data written to it and writes the compressed form of that data to an underlying writer (see NewWriter).
  从底层写入数据和写入数据的压缩形式。
```

```golang  
    func NewWriter(w io.Writer, level int) (*Writer, error)
      NewWriter returns a new Writer compressing data at the given level. 
      Following zlib, levels range from 1 (BestSpeed) to 9 (BestCompression);
      higher levels typically run slower but compress more. 
      Level 0 (NoCompression) does not attempt any compression; 
      it only adds the necessary DEFLATE framing. 
      Level -1 (DefaultCompression) uses the default compression level.
      If level is in the range [-1, 9] then the error returned will be nil. Otherwise the error returned will be non-nil.
      	
      	根据给定的level返回压缩数据的对象。
     	zlib情况，级别时从1（最快压缩速度）到9（最优压缩）;
     	更高级别会更慢但是会压缩得更多。
     	级别0不进行任何压缩。
     	它只会添加必须的DEFLATE。
     	级别-1是使用默认的压缩级别
      	如果级别在-1到9之间发生错误会返回nil。其他的错误会返回非nil
```      

```golang
    func NewWriterDict(w io.Writer, level int, dict []byte) (*Writer, error)
      NewWriterDict is like NewWriter but initializes the new Writer with a preset dictionary. 
      The returned Writer behaves as if the dictionary had been written to it without producing any compressed output. 
      The compressed data written to w can only be decompressed by a Reader initialized with the same dictionary.
        和NewWriter一样，但是会使用预设的字典初始化。
        返回不产生任何压缩输出的Writer。
        写入w的是以相同字典初始化的读取压缩的数据。
```

```golang    
    func (w *Writer) Close() error
      Close flushes and closes the writer.
      刷新和关闭writer
```

```golang    
    func (w *Writer) Flush() error
      Flush flushes any pending compressed data to the underlying writer. 
      It is useful mainly in compressed network protocols, to ensure that a remote reader has enough data to reconstruct a packet. 
      Flush does not return until the data has been written. If the underlying writer returns an error, Flush returns that error.
        刷新任何底层writer追加的压缩数据。
        主要用来压缩网络协议保证远程的reader有足够的数据重建包。
      Flush直到数据写完才运行。如果底层的writer返回错误，Flush返回那个错误。
      In the terminology of the zlib library, Flush is equivalent to Z_SYNC_FLUSH.
      	在zlib类库里，Flush相当于Z_SYNC_FLUSH
```

```golang    
    func (w *Writer) Write(data []byte) (n int, err error)
      Write writes data to w, which will eventually write the compressed form of data to its underlying writer.
        写入数据到w
```