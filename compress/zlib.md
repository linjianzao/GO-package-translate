包地址：http://golang.org/pkg/compress/zlib/
Package zlib implements reading and writing of zlib format compressed data, as specified in RFC 1950.
包zlib 在RFC 1950规定 实现读和写zlib压缩数据的形式

```golang
The implementation provides filters that uncompress during reading and compress during writing. For example, to write compressed data to a buffer:
实现了过滤 读取解压 和写入压缩。比如：把压缩数据写入缓冲区。
```

```golang
func NewReader(r io.Reader) (io.ReadCloser, error)
  NewReader creates a new io.ReadCloser that satisfies reads by decompressing data read from r. The implementation buffers input and may read more data than necessary from r. It is the caller's responsibility to call Close on the ReadCloser when done.
```  
```golang
func NewReaderDict(r io.Reader, dict []byte) (io.ReadCloser, error)
```

```golang
type Writer
    func NewWriter(w io.Writer) *Writer
    func NewWriterLevel(w io.Writer, level int) (*Writer, error)
    func NewWriterLevelDict(w io.Writer, level int, dict []byte) (*Writer, error)
    func (z *Writer) Close() error
    func (z *Writer) Flush() error
    func (z *Writer) Write(p []byte) (n int, err error)
```