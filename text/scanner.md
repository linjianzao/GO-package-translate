Package scanner

Overview ▾

Package scanner provides a scanner and tokenizer for UTF-8-encoded text. 
It takes an io.Reader providing the source, which then can be tokenized through repeated calls to the Scan function. 
For compatibility with existing tools, the NUL character is not allowed. If the first character in the source is a UTF-8 encoded byte order mark (BOM), it is discarded.

By default, a Scanner skips white space and Go comments and recognizes all literals as defined by the Go language specification. 
It may be customized to recognize only a subset of those literals and to recognize different white space characters.

scanner包 为UTF-8编码 文本 提供一个 扫描器和分词器.
这需要一个  io.Reader 提供 源,  然后 可以通过反复调用Scan函数 进行标记化。
为了兼容已经存在的工具, 不允许 NUL 字符. 如果在源里的第一个字符 是UTF-8 编码 字节 顺序标记（BOM）,它被丢弃。

默认情况下,Scanner 跳过空格 和GO 注释 和Go语言规范定义的所有文字。

它可以自定义的 只有 这些文字只有子集  来认出不同的空格字符。


Basic usage pattern:
基本的使用模式：
```golang
var s scanner.Scanner
s.Init(src)
tok := s.Scan()
for tok != scanner.EOF {
	// do something with tok
	tok = s.Scan()
}
```


Constants
```golang
const (
        ScanIdents     = 1 << -Ident
        ScanInts       = 1 << -Int
        ScanFloats     = 1 << -Float // includes Ints
        ScanChars      = 1 << -Char
        ScanStrings    = 1 << -String
        ScanRawStrings = 1 << -RawString
        ScanComments   = 1 << -Comment
        SkipComments   = 1 << -skipComment // if set with ScanComments, comments become white space
        GoTokens       = ScanIdents | ScanFloats | ScanChars | ScanStrings | ScanRawStrings | ScanComments | SkipComments
)
```
Predefined mode bits to control recognition of tokens. 
For instance, to configure a Scanner such that it only recognizes (Go) identifiers, integers, and skips comments, set the Scanner's Mode field to:
预定义模式位来控制识别标记。
例如，配置一个Scanner  它只认出 (Go)标识符,整型,和跳过注释, 设置Scanner 的 Mode 字段成:

```golang
ScanIdents | ScanInts | SkipComments
```

```golang
const (
        EOF = -(iota + 1)
        Ident
        Int
        Float
        Char
        String
        RawString
        Comment
)
```
The result of Scan is one of the following tokens or a Unicode character.
Scan的结果是 下面的一个标记 或 一个 Unicode字符.

```golang
const GoWhitespace = 1<<'\t' | 1<<'\n' | 1<<'\r' | 1<<' '
```

GoWhitespace is the default value for the Scanner's Whitespace field. Its value selects Go's white space characters.
GoWhitespace 是默认的Scanner的Whitespace 字段值.  它的值选择GO 的空白字符。


func TokenString
```golang
func TokenString(tok rune) string
```
TokenString returns a printable string for a token or Unicode character.
TokenString  返回  可打印字符串的token 或 Unicode字符


type Position
```golang
type Position struct {
        Filename string // filename, if any
        Offset   int    // byte offset, starting at 0
        Line     int    // line number, starting at 1
        Column   int    // column number, starting at 1 (character count per line)
}
```
A source position is represented by a Position value. A position is valid if Line > 0.
source position 表示一个 Position值.  如果Line > 0 position 是有效的.



func (*Position) IsValid
```golang
func (pos *Position) IsValid() bool
```
IsValid returns true if the position is valid.
IsValid 如果位置 有效 返回 true


func (Position) String
```golang
func (pos Position) String() string
```


type Scanner
```golang
type Scanner struct {

        // Error is called for each error encountered. If no Error
        // function is set, the error is reported to os.Stderr.
        // Error为每个遇到的错误调用. 如果没有 Error函数设置, 错误报告到os.Stderr.
        Error func(s *Scanner, msg string)

        // ErrorCount is incremented by one for each error encountered.
        //ErrorCount 对每个遇到的错误 加1
        ErrorCount int

        // The Mode field controls which tokens are recognized. For instance,
        // to recognize Ints, set the ScanInts bit in Mode. The field may be
        // changed at any time.
        // Mode 字段 控制 标记确认.例如, 辨认Ints, 在Mode里设置ScanInts位.该字段可能在任何时候改变.
        Mode uint

        // The Whitespace field controls which characters are recognized
        // as white space. To recognize a character ch <= ' ' as white space,
        // set the ch'th bit in Whitespace (the Scanner's behavior is undefined
        // for values ch > ' '). The field may be changed at any time.
        //Whitespace 字段 控制 字符 辨认 空格. 
           //为了 辨认一个字符 ch <= ' '  成 空格, 在Whitespace里设置 ch'th 的位(扫描器 对值 ch > ' ' 的行为是 undefined)
           //该字段可能在任何时候改变.
        Whitespace uint64

        // Start position of most recently scanned token; set by Scan.
        // Calling Init or Next invalidates the position (Line == 0).
        // The Filename field is always left untouched by the Scanner.
        // If an error is reported (via Error) and Position is invalid,
        // the scanner is not inside a token. Call Pos to obtain an error
        // position in that case.
           //启动最近扫描的记号位置;通过Scan设置.
           //调用 Init 或 Next 废止 position (Line == 0).
        //Filename 字段始终保持扫描器不变的。
           //如果错误被报告并且 Position 是无效的,扫描器不是一个token里面.在这种情况下,调用 Pos 获取 错误位置.
        Position
        // contains filtered or unexported fields
}
```
A Scanner implements reading of Unicode characters and tokens from an io.Reader.
Scanner 实现从io.Reader 读取Unicode字符和token.


func (*Scanner) Init
```golang
func (s *Scanner) Init(src io.Reader) *Scanner
```
Init initializes a Scanner with a new source and returns s. Error is set to nil, ErrorCount is set to 0, Mode is set to GoTokens, and Whitespace is set to GoWhitespace.
Init 用新的 源 初始化一个 Scanner 并返回s.Error 设置为nil, ErrorCount 设置为0,Mode 设置为GoTokens 并且 Whitespace 设置为 GoWhitespace



func (*Scanner) Next
```golang
func (s *Scanner) Next() rune
```
Next reads and returns the next Unicode character. It returns EOF at the end of the source. 
It reports a read error by calling s.Error, if not nil; otherwise it prints an error message to os.Stderr. 
Next does not update the Scanner's Position field; use Pos() to get the current position.
Next 读取和返回 下一个 Unicode 字符. 它返回 源 结尾 的 EOF.  如果error不是nil,它 调用  s.Error 报告读取时的错误. 否则它打印 错误信息到  os.Stderr.
Next 不会更新 Scanner的 Position 字段. 使用 Pos() 来获取 当前的位置.



func (*Scanner) Peek
```golang
func (s *Scanner) Peek() rune
```
Peek returns the next Unicode character in the source without advancing the scanner. It returns EOF if the scanner's position is at the last character of the source.
Peek 返回 在源里不包含 推进扫描器 的 下一个 Unicode 字符 . 如果 scanner的位置在 源的最后一个字符,它返回EOF.



func (*Scanner) Pos
```golang
func (s *Scanner) Pos() (pos Position)
```
Pos returns the position of the character immediately after the character or token returned by the last call to Next or Scan.
Pos 在最后调用Next 或 Scan 返回字符 或 token 之后 立即返回字符的位置.


func (*Scanner) Scan
```golang
func (s *Scanner) Scan() rune
```
Scan reads the next token or Unicode character from source and returns it. 
It only recognizes tokens t for which the respective Mode bit (1<<-t) is set. 
It returns EOF at the end of the source. 
It reports scanner errors (read and token errors) by calling s.Error, if not nil; otherwise it prints an error message to os.Stderr.
Scan 从源读取 下一个token 或 Unicode 字符 并返回它.
它只认 tokens t 其中各个模式位（1<< - t）设定。 它在 源的的最后返回EOF. 如果 错误不是nil, 它调用 s.Error返回扫描器的错误(读取 和token错误 ); 否则 打印错误信息到 os.Stderr.
 


func (*Scanner) TokenText
```golang
func (s *Scanner) TokenText() string
```
TokenText returns the string corresponding to the most recently scanned token. Valid after calling Scan().
TokenText 返回 字符串 对应于最近扫描的token. 在调用 Scan() 后 有效.







