包地址：http://golang.org/pkg/encoding/csv/

```golang
Package csv reads and writes comma-separated values (CSV) files.
A csv file contains zero or more records of one or more fields per record. Each record is separated by the newline character. 
The final record may optionally be followed by a newline character.

csv包 读取和写入逗号分隔的CSV文件
csv文件每行都包含0或者更多的记录 字段。 每条记录用 换行符分隔
最后一条记录的 换行符是可选的
```golang
field1,field2,field3
```


White space is considered part of a field.
Carriage returns before newline characters are silently removed.
Blank lines are ignored. A line with only whitespace characters (excluding the ending newline character) is not considered a blank line.
Fields which start and stop with the quote character " are called quoted-fields. The beginning and ending quote are not part of the field.

空白符是字段的一部分
回车符返回前换行符会被自动删除
空白行被忽略。一个行如果只包含空白字符(除了换行符)它不是空白行
字段 开始和停止 在引号字符 " 被称为引用字段。开始和结束引号不包含在字段里

The source:
```golang
normal string,"quoted-field"
```
results in the fields
```golang
{`normal string`, `quoted-field`}
```

Within a quoted-field a quote character followed by a second quote character is considered a single quote.
引号后面跟着引号是包含在字符里的
```golang
"the ""word"" is true","a ""quoted-field"""
```
results in
```golang
{`the "word" is true`, `a "quoted-field"`}
```

Newlines and commas may be included in a quoted-field
换行符和逗号包含在引用字段里
```golang
"Multi-line
field","comma is ,"
```
results in
```golang
{`Multi-line
field`, `comma is ,`}
```
```

Variables
```golang
var (
    ErrTrailingComma = errors.New("extra delimiter at end of line") // no longer used
    ErrBareQuote     = errors.New("bare \" in non-quoted-field")
    ErrQuote         = errors.New("extraneous \" in field")
    ErrFieldCount    = errors.New("wrong number of fields in line")
)
These are the errors that can be returned in ParseError.Error
返回ParseError.Error里的错误
```

type ParseError
```golang
type ParseError struct {
    Line   int   // Line where the error occurred
    Column int   // Column (rune index) where the error occurred
    Err    error // The actual error
}
A ParseError is returned for parsing errors. The first line is 1. The first column is 0.
返回解析错误。第一行是1. 第一列是0
```

func (*ParseError) Error
```golang
func (e *ParseError) Error() string
```
type Reader
```golang
type Reader struct {
    Comma            rune // field delimiter (set to ',' by NewReader)
    Comment          rune // comment character for start of line
    FieldsPerRecord  int  // number of expected fields per record
    LazyQuotes       bool // allow lazy quotes
    TrailingComma    bool // ignored; here for backwards compatibility
    TrimLeadingSpace bool // trim leading space
    // contains filtered or unexported fields
}
A Reader reads records from a CSV-encoded file.
As returned by NewReader, a Reader expects input conforming to RFC 4180. 
	The exported fields can be changed to customize the details before the first call to Read or ReadAll.
Comma is the field delimiter. It defaults to ','.
Comment, if not 0, is the comment character. Lines beginning with the Comment character are ignored.
If FieldsPerRecord is positive, Read requires each record to have the given number of fields. 
	If FieldsPerRecord is 0, Read sets it to the number of fields in the first record, so that future records must have the same field count. 
	If FieldsPerRecord is negative, no check is made and records may have a variable number of fields.
If LazyQuotes is true, a quote may appear in an unquoted field and a non-doubled quote may appear in a quoted field.
If TrimLeadingSpace is true, leading white space in a field is ignored.

Reader 从CSV编码文件读取记录
根据NewReader返回的Reader，符合RFC 4180
逗号是字段分隔符，默认是 ,
如果不是0，是注释字段。以注释字符开始的行会被忽略
如果FieldsPerRecord是正的，用给定的数字读取每条记录
如果FieldsPerRecord是0，Read设置第一条记录的字段数字，如果之后的记录数必须和这个字段一样
如果FieldsPerRecord是负的，没有检查，并记录可能有一个可变数量的字段。

如果LazyQuotes是true，引用可能似乎一个不带引号的字段，引号字段 可能没有双引号
如果TrimLeadingSpace是true，空白符被忽略
```

func NewReader
```golang
func NewReader(r io.Reader) *Reader
	NewReader returns a new Reader that reads from r.
	从r读取 返回新的Reader
```

func (*Reader) Read
```golang
func (r *Reader) Read() (record []string, err error)
	Read reads one record from r. The record is a slice of strings with each string representing one field.
	从r中读取一条记录。record 是一个字符串slice
```

func (*Reader) ReadAll
```golang
func (r *Reader) ReadAll() (records [][]string, err error)
	ReadAll reads all the remaining records from r. 
	Each record is a slice of fields. 
	A successful call returns err == nil, not err == EOF. 
	Because ReadAll is defined to read until EOF, it does not treat end of file as an error to be reported.
	
	从r中读取剩下的记录
	每个记录都是一个slice字段
	成功调用的话 返回 的err == nil， 而不是err == EOF
	因为ReadAll读取直到EOF，对文件结尾没有错误是不会返回错误的
```

type Writer
```golang
type Writer struct {
    Comma   rune // Field delimiter (set to ',' by NewWriter)
    UseCRLF bool // True to use \r\n as the line terminator
    // contains filtered or unexported fields
}
A Writer writes records to a CSV encoded file.
As returned by NewWriter, a Writer writes records terminated by a newline and uses ',' as the field delimiter. 
The exported fields can be changed to customize the details before the first call to Write or WriteAll.
Comma is the field delimiter.
If UseCRLF is true, the Writer ends each record with \r\n instead of \n.

写入记录到csv编码文件
Writer写入记录直到遇到换行符，然后用 ',' 分隔
第一次调用Write或WriteAll 引入的字段可以被自定义修改
逗号是字段分隔符

```