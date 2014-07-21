Package textproto


Overview ▾

Package textproto implements generic support for text-based request/response protocols in the style of HTTP, NNTP, and SMTP.
textproto包 实现通用的 支持 基于文本的HTTP, NNTP, 和 SMTP风格的 request/response 协议.


The package provides:

Error, which represents a numeric error response from a server.

Pipeline, to manage pipelined requests and responses in a client.

Reader, to read numeric response code lines, key: value headers, lines wrapped with leading spaces on continuation lines, and whole text blocks ending with a dot on a line by itself.

Writer, to write dot-encoded text blocks.

Conn, a convenient packaging of Reader, Writer, and Pipeline for use with a single network connection.

该包提供:
Error, 表示一个 服务器响应的 错误号.
Pipeline, 管理 在一个客户端里的请求和响应 流水线.
Reader,读取数字响应号码行,key:value 头,线包裹着的续行前导空格,整个文本块结束与线路上的点本身。
Writer,写点编码 文本块.
Conn, 一个方便的Reader, Writer, and Pipeline 包, 使用一个单一的网络连接.



func CanonicalMIMEHeaderKey
```golang
func CanonicalMIMEHeaderKey(s string) string
```
CanonicalMIMEHeaderKey returns the canonical format of the MIME header key s. 
The canonicalization converts the first letter and any letter following a hyphen to upper case; the rest are converted to lowercase. 
For example, the canonical key for "accept-encoding" is "Accept-Encoding". 
MIME header keys are assumed to be ASCII only.
CanonicalMIMEHeaderKey 返回MIME头键s的规范格式。
规范化转换第一个字母为 和任何字符后面跟着连字符成大写字母;其余的都转换为小写。
例如 规范  "accept-encoding" 成  "Accept-Encoding". 
MIME 头 键 假定只有ASCII码。


func TrimBytes
```golang
func TrimBytes(b []byte) []byte
```
TrimBytes returns b without leading and trailing ASCII space.
TrimBytes 返回b 没有开头和结尾的ASCII空格。


func TrimString
```golang
func TrimString(s string) string
```
TrimString returns s without leading and trailing ASCII space.
TrimString 返回s 没有开头和结尾的ASCII空格。


type Conn
```golang
type Conn struct {
        Reader
        Writer
        Pipeline
        // contains filtered or unexported fields
}
```
A Conn represents a textual network protocol connection. 
It consists of a Reader and Writer to manage I/O and a Pipeline to sequence concurrent requests on the connection. 
These embedded types carry methods with them; see the documentation of those types for details.
Conn 表示一个文本的网络协议连接。
它由一个Reader和 Writer 管理 I/O 并管道的并发请求序列的连接。
这些嵌入式类型带的方法;查看那些类型的详细信息的文档。



func Dial
```golang
func Dial(network, addr string) (*Conn, error)
```
Dial connects to the given address on the given network using net.Dial and then returns a new Conn for the connection.
Dial 使用 net.Dial 在给定的network中 连接到给定的地址 然后为连接返回一个新的Conn



func NewConn
```golang
func NewConn(conn io.ReadWriteCloser) *Conn
```
NewConn returns a new Conn using conn for I/O.
NewConn 用conn 为 I/O 返回一个新的 Conn


func (*Conn) Close
```golang
func (c *Conn) Close() error
```
Close closes the connection.
Close 关闭连接


func (*Conn) Cmd
```golang
func (c *Conn) Cmd(format string, args ...interface{}) (id uint, err error)
```
Cmd is a convenience method that sends a command after waiting its turn in the pipeline. 
The command text is the result of formatting format with args and appending \r\n. 
Cmd returns the id of the command, for use with StartResponse and EndResponse.
Cmd是一个 方便 在管道 等待它转 后 发送指令的方法.
指令文本 是 用format 和 args 和追加 \r\n 格式化 的结果.

For example, a client might run a HELP command that returns a dot-body by using:
例如, 一个客户端可能执行一个 HELP 指令, 那返回一个点体 通过使用:
```golang
id, err := c.Cmd("HELP")
if err != nil {
	return nil, err
}

c.StartResponse(id)
defer c.EndResponse(id)

if _, _, err = c.ReadCodeLine(110); err != nil {
	return nil, err
}
text, err := c.ReadDotBytes()
if err != nil {
	return nil, err
}
return c.ReadCodeLine(250)
```


type Error
```golang
type Error struct {
        Code int
        Msg  string
}
```
An Error represents a numeric error response from a server.
Error 表示一个服务器响应的 错误号



func (*Error) Error
```golang
func (e *Error) Error() string
```



type MIMEHeader
```golang
type MIMEHeader map[string][]string
```
A MIMEHeader represents a MIME-style header mapping keys to sets of values.
MIMEHeader 表示一个 MIME-style 的头  映射 键到 值.



func (MIMEHeader) Add
```golang
func (h MIMEHeader) Add(key, value string)
```
Add adds the key, value pair to the header. It appends to any existing values associated with key.
Add 添加键,值 对到头. 它追加 任何和存在值有关的键



func (MIMEHeader) Del
```golang
func (h MIMEHeader) Del(key string)
```
Del deletes the values associated with key.
Del 删除 和值有关的键



func (MIMEHeader) Get
```golang
func (h MIMEHeader) Get(key string) string
```
Get gets the first value associated with the given key. 
If there are no values associated with the key, Get returns "".
Get is a convenience method. For more complex queries, access the map directly.
Get 获取和给定的键对应的 第一个值.
如果 键对应不到值, Get 返回 " ".
Get 是一个简便的方法.  对于更复杂的查询，直接访问map。



func (MIMEHeader) Set
```golang
func (h MIMEHeader) Set(key, value string)
```
Set sets the header entries associated with key to the single element value. It replaces any existing values associated with key.
Set 设置头项 有关的键到 单一的元素值. 它代替任何已经存在的键对应的值.


type Pipeline
```golang
type Pipeline struct {
        // contains filtered or unexported fields
}
```
A Pipeline manages a pipelined in-order request/response sequence.
Pipeline 管理一个 管道 在阶  request/response 顺序.

To use a Pipeline p to manage multiple clients on a connection, each client should run:
使用 一个 Pipeline p 来管理连接上的多个客户端,每个客户端应该执行:
```golang
id := p.Next()	// take a number

p.StartRequest(id)	// wait for turn to send request
«send request»
p.EndRequest(id)	// notify Pipeline that request is sent

p.StartResponse(id)	// wait for turn to read response
«read response»
p.EndResponse(id)	// notify Pipeline that response is read
```
A pipelined server can use the same calls to ensure that responses computed in parallel are written in the correct order.
管道服务器 可以 使用同样 调用, 确保 响应 计算 并行地写入到正确的顺序。



func (*Pipeline) EndRequest
```golang
func (p *Pipeline) EndRequest(id uint)
```
EndRequest notifies p that the request with the given id has been sent (or, if this is a server, received).
EndRequest 通知p 给定id的请求已经发送(或者如果这是服务端,那就是收到 )


func (*Pipeline) EndResponse
```golang
func (p *Pipeline) EndResponse(id uint)
```
EndResponse notifies p that the response with the given id has been received (or, if this is a server, sent).
EndResponse 通知p 给定id的请求已经接收(或 如果这是服务端那就是发送)



func (*Pipeline) Next
```golang
func (p *Pipeline) Next() uint
```
Next returns the next id for a request/response pair.
Next 返回request/response 对的 下一个id


func (*Pipeline) StartRequest
```golang
func (p *Pipeline) StartRequest(id uint)
```
StartRequest blocks until it is time to send (or, if this is a server, receive) the request with the given id.
StartRequest 中断直到到 用给定的id发送请求  时间 (或者如果这是服务器, 那就是 接收)


func (*Pipeline) StartResponse
```golang
func (p *Pipeline) StartResponse(id uint)
```
StartResponse blocks until it is time to receive (or, if this is a server, send) the request with the given id.
StartResponse 中断直到到 用给定的id接收请求  时间 (或者如果这是服务器, 那就是发送)



type ProtocolError
```golang
type ProtocolError string
```
A ProtocolError describes a protocol violation such as an invalid response or a hung-up connection.
ProtocolError 描述一个 违反协议 例如无效 的响应 或 中断的连接



func (ProtocolError) Error
```golang
func (p ProtocolError) Error() string
```


type Reader
```golang
type Reader struct {
        R *bufio.Reader
        // contains filtered or unexported fields
}
```
A Reader implements convenience methods for reading requests or responses from a text protocol network connection.
Reader 实现 从文本协议网络连接 读取 请求或 响应 的方法


func NewReader
```golang
func NewReader(r *bufio.Reader) *Reader
```
NewReader returns a new Reader reading from r.
NewReader 从r 读取 返回一个新的 Reader 


func (*Reader) DotReader
```golang
func (r *Reader) DotReader() io.Reader
```
DotReader returns a new Reader that satisfies Reads using the decoded text of a dot-encoded block read from r. 
The returned Reader is only valid until the next call to a method on r.

Dot encoding is a common framing used for data blocks in text protocols such as SMTP. 
The data consists of a sequence of lines, each of which ends in "\r\n". 
The sequence itself ends at a line containing just a dot: ".\r\n". 
Lines beginning with a dot are escaped with an additional dot to avoid looking like the end of the sequence.

The decoded form returned by the Reader's Read method rewrites the "\r\n" line endings into the simpler "\n", 
	removes leading dot escapes if present, and stops with error io.EOF after consuming (and discarding) the end-of-sequence line.

DotReader返回一个新的Reader, 满足Reads 使用从r读取的点编码的块的解码的文本。
返回的Reader  只有效 直到 r上的 下一次调用方法 

点编码是文本协议，如SMTP用于数据块的一个共同的框架。
该数据由行的顺序的在"\r\n"每一个结束。
该序列本身在只包含一个点一个行结束: ".\r\n". 
线以点开头的转义一个额外的点，以避免看上去像序列的末尾。

解码形式 返回 Reader的 Read 方法重写 "\r\n"行 结尾 到 简单"\n", 
	去除前导点（如果存在）,在消耗到序列结束的行之后停止并显示错误io.EOF。



func (*Reader) ReadCodeLine
```golang
func (r *Reader) ReadCodeLine(expectCode int) (code int, message string, err error)
```
ReadCodeLine reads a response code line of the form
ReadCodeLine 读取一个响应表单的代码行


```golang
code message
```


where code is a three-digit status code and the message extends to the rest of the line. An example of such a line is:
其中的代码是一个三位状态码和消息扩展到该行的其余部分. 例子是:

```golang
220 plan9.bell-labs.com ESMTP
```

If the prefix of the status does not match the digits in expectCode, ReadCodeLine returns with err set to &Error{code, message}. 
For example, if expectCode is 31, an error will be returned if the status is not in the range [310,319].

If the response is multi-line, ReadCodeLine returns an error.

An expectCode <= 0 disables the check of the status code.

如果前缀的状态码 没有匹配  expectCode, ReadCodeLine里数字, 返回err 设置成&Error{code, message}. 
例如, 如果expectCode 是31, 如果状态 码范围没有在 [310,319] 之间, 将返回错误.

如果响应是多行,ReadCodeLine 返回一个错误.

expectCode <= 0 禁用状态码检查



func (*Reader) ReadContinuedLine
```golang
func (r *Reader) ReadContinuedLine() (string, error)
```
ReadContinuedLine reads a possibly continued line from r, eliding the final trailing ASCII white space. 
Lines after the first are considered continuations if they begin with a space or tab character. 
In the returned data, continuation lines are separated from the previous line only by a single space: the newline and leading white space are removed.
ReadContinuedLine 从r读取一个 可能继续行,到未提及的最后结尾的ASCII空格。
如果 他们以 空格或tab字符开始,第一行后，被认为是延续.
在返回的数据,续行从上一行只能由一个空格隔开：换行和前导空格被删除。


For example, consider this input:
例如,考虑这个输入：

```golang
Line 1
  continued...
Line 2
```

The first call to ReadContinuedLine will return "Line 1 continued..." and the second will return "Line 2".

A line consisting of only white space is never continued.
第一个调用ReadContinuedLine 将返回 "Line 1 continued..."   第二个 将返回  "Line 2".
包括只有空白的行是永远不会继续。


func (*Reader) ReadContinuedLineBytes
```golang
func (r *Reader) ReadContinuedLineBytes() ([]byte, error)
```
ReadContinuedLineBytes is like ReadContinuedLine but returns a []byte instead of a string.
ReadContinuedLineBytes 类似ReadContinuedLine 但是返回 一个  []byte 代替字符串


func (*Reader) ReadDotBytes
```golang
func (r *Reader) ReadDotBytes() ([]byte, error)
```
ReadDotBytes reads a dot-encoding and returns the decoded data.

See the documentation for the DotReader method for details about dot-encoding.
ReadDotBytes 读取一个点编码 并返回 解码数据.
查看 DotReader 方法 有关点编码 的详细文档.


func (*Reader) ReadDotLines
```golang
func (r *Reader) ReadDotLines() ([]string, error)
```
ReadDotLines reads a dot-encoding and returns a slice containing the decoded lines, with the final \r\n or \n elided from each.

See the documentation for the DotReader method for details about dot-encoding.
ReadDotLines 读取点编码 并返回一个 包含解码行的 slice, 最后用 \r\n or \n 每个省略.
查看 DotReader 方法 有关点编码 的详细文档.



func (*Reader) ReadLine
```golang
func (r *Reader) ReadLine() (string, error)
```
ReadLine reads a single line from r, eliding the final \n or \r\n from the returned string.
ReadLine 从r 读取单个行, 从未提及的返回的字符串的最后\ n或\ r\ n。


func (*Reader) ReadLineBytes
```golang
func (r *Reader) ReadLineBytes() ([]byte, error)
```
ReadLineBytes is like ReadLine but returns a []byte instead of a string.
ReadLineBytes 类似ReadLine 但是 返回一个[]byte 代替 字符串


func (*Reader) ReadMIMEHeader
```golang
func (r *Reader) ReadMIMEHeader() (MIMEHeader, error)
```
ReadMIMEHeader reads a MIME-style header from r. 
The header is a sequence of possibly continued Key: Value lines ending in a blank line. 
The returned map m maps CanonicalMIMEHeaderKey(key) to a sequence of values in the same order encountered in the input.
ReadMIMEHeader从r中读取一个 MIME-style 头.
头是一个是可能持续的 Key: Value 在一个空行结束。
返回的map m 映射CanonicalMIMEHeaderKey 以遇到的输入相同的顺序的序列。


For example, consider this input:
例如, 考虑这个输入：

```golang
My-Key: Value 1
Long-Key: Even
       Longer Value
My-Key: Value 2
```

Given that input, ReadMIMEHeader returns the map:
给定那个输入,ReadMIMEHeader 返回map:

```golang
map[string][]string{
	"My-Key": {"Value 1", "Value 2"},
	"Long-Key": {"Even Longer Value"},
}
```

func (*Reader) ReadResponse
```golang
func (r *Reader) ReadResponse(expectCode int) (code int, message string, err error)
```
ReadResponse reads a multi-line response of the form:
ReadResponse 读取一个多行响应形式:


```golang
code-message line 1
code-message line 2
...
code message line n
```


where code is a three-digit status code. 
The first line starts with the code and a hyphen. 
The response is terminated by a line that starts with the same code followed by a space. 
Each line in message is separated by a newline (\n).

See page 36 of RFC 959 (http://www.ietf.org/rfc/rfc959.txt) for details.

If the prefix of the status does not match the digits in expectCode, ReadResponse returns with err set to &Error{code, message}. 
For example, if expectCode is 31, an error will be returned if the status is not in the range [310,319].

An expectCode <= 0 disables the check of the status code.
其中的号码是一个三位状态码.
第一行以号码和 连接符开始.
响应以行中断 , 以相同的号码跟着一个空格 开始.
每一行信息用换行符分隔(\n).
查看RFC 959 的36页 详细内容.(http://www.ietf.org/rfc/rfc959.txt)

如果 状态码的前缀没有匹配 在 expectCode, ReadResponse  里的 数字, 返回 err 设置成的  &Error{code, message}. 
例如, 如果expectCode 是31, 如果状态 码范围没有在 [310,319] 之间, 将返回错误.



type Writer
```golang
type Writer struct {
        W *bufio.Writer
        // contains filtered or unexported fields
}
```
A Writer implements convenience methods for writing requests or responses to a text protocol network connection.
Writer实现简便 写请求 或 响应一个文本协议网络连接 方法


func NewWriter
```golang
func NewWriter(w *bufio.Writer) *Writer
```
NewWriter returns a new Writer writing to w.
NewWriter 返回一个新的 写到w 的Writer.


func (*Writer) DotWriter
```golang
func (w *Writer) DotWriter() io.WriteCloser
```
DotWriter returns a writer that can be used to write a dot-encoding to w. 
It takes care of inserting leading dots when necessary, translating line-ending \n into \r\n, and adding the final .\r\n line when the DotWriter is closed. 
The caller should close the DotWriter before the next call to a method on w.

See the documentation for Reader's DotReader method for details about dot-encoding.
DotWriter 返回一个writer  可以用来写 点编码到w.
当需要时 插入前导 点,转换行结束符\n 成 \r\n. 并 当DotWriter关闭时 添加 结束.\r\n 行


func (*Writer) PrintfLine
```golang
func (w *Writer) PrintfLine(format string, args ...interface{}) error
```
PrintfLine writes the formatted output followed by \r\n.
PrintfLine 写格式化输出其次是\r\n.








