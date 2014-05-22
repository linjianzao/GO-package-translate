包地址：http://golang.org/pkg/compress/lzw/
Package lzw implements the Lempel-Ziv-Welch compressed data format, described in T. A. Welch, “A Technique for High-Performance Data Compression”, Computer, 17(6) (June 1984), pp 8-19.

```golang
lzw实现Lempel-Ziv-Welch 压缩数据形式，T. A. Welch的描述：“一个高性能数据压缩技术”
In particular, it implements LZW as used by the GIF, TIFF and PDF file formats, which means variable-width codes up to 12 bits and the first two non-literal codes are a clear code and an EOF code
特别是，LZW用在GIF、TIFF、PDF文件,这意味着可变宽度高达12位的代码，前两个非字面的代码一个清晰的代码和一个EOF代码
```

```golang
func NewReader(r io.Reader, order Order, litWidth int) io.ReadCloser
  NewReader creates a new io.ReadCloser that satisfies reads by decompressing the data read from r. It is the caller's responsibility to call Close on the ReadCloser when finished reading. The number of bits to use for literal codes, litWidth, must be in the range [2,8] and is typically 8.
  创建一个新的io.ReadCloser 满足从r解压数据读取。读取结束后会关闭。字节数使用的代码必须在2到8之前，通常是8。
```
```golang  
func NewWriter(w io.Writer, order Order, litWidth int) io.WriteCloser
  NewWriter creates a new io.WriteCloser that satisfies writes by compressing the data and writing it to w. It is the caller's responsibility to call Close on the WriteCloser when finished writing. The number of bits to use for literal codes, litWidth, must be in the range [2,8] and is typically 8.
  常见一个新的io.WriteCloser满足解压数据和写入数据到w。字节数使用的代码必须在2到8之前，通常是8。
```

```golang  
type Order
  Order specifies the bit ordering in an LZW data stream.
  Order规定LZW数据流的字节序列
```