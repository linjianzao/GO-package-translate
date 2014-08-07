Overview ▾

Package iotest implements Readers and Writers useful mainly for testing.
iotest 包 实现 主要用于测试Readers 和 Writers


Variables
```golang
var ErrTimeout = errors.New("timeout")
```


func DataErrReader
```golang
func DataErrReader(r io.Reader) io.Reader
```
DataErrReader changes the way errors are handled by a Reader. 
Normally, a Reader returns an error (typically EOF) from the first Read call after the last piece of data is read. 
DataErrReader wraps a Reader and changes its behavior so the final error is returned along with the final data, instead of in the first call after the final data.
DataErrReader 修改 错误是由Reader来处理。 通常 一个Reader 返回错误(通常是EOF) 从 第一次 Read 调用  在最后 一片数据读取后.
DataErrReader 封装 Reader 和 修改它的 行为  因此最终的错误与最终数据一起返回,  而不是在第一次调用  最终的数据后.



func HalfReader
```golang
func HalfReader(r io.Reader) io.Reader
```
HalfReader returns a Reader that implements Read by reading half as many requested bytes from r.
HalfReader 返回一个Reader  通过 从r中读取一半请求字节数 实现Read.
 


func NewReadLogger
```golang
func NewReadLogger(prefix string, r io.Reader) io.Reader
```
NewReadLogger returns a reader that behaves like r except that it logs (using log.Print) each read to standard error, printing the prefix and the hexadecimal data written.
NewReadLogger 返回 行为类型r的 reader  除了 它 记录每个读取到 标准错误(使用log.Print) ,  打印前缀和 16进制数据 写入.



func NewWriteLogger
```golang
func NewWriteLogger(prefix string, w io.Writer) io.Writer
```
NewWriteLogger returns a writer that behaves like w except that it logs (using log.Printf) each write to standard error, printing the prefix and the hexadecimal data written.
NewWriteLogger 返回 行为类型w的 writer  除了 它 记录每个写到 标准错误(使用log.Print) ,  打印前缀和 16进制数据 写入.



func OneByteReader
```golang
func OneByteReader(r io.Reader) io.Reader
```
OneByteReader returns a Reader that implements each non-empty Read by reading one byte from r.
OneByteReader 返回一个Reader 根据从r 读取 一个 字节 实现 每个非空 Read .



func TimeoutReader
```golang
func TimeoutReader(r io.Reader) io.Reader
```
TimeoutReader returns ErrTimeout on the second read with no data. Subsequent calls to read succeed.
TimeoutReader 返回 ErrTimeout 在第二次读取没有数据时. 后续调用读取成功。



func TruncateWriter
```golang
func TruncateWriter(w io.Writer, n int64) io.Writer
```
TruncateWriter returns a Writer that writes to w but stops silently after n bytes.
TruncateWriter  返回   写入 到w 但是在n个字节后停止  的Writer


