包地址:http://golang.org/pkg/go/doc/      
#Package doc         

##Overview 
Package doc extracts source code documentation from a Go AST.         
doc包从 GO AST中提取 源代码文件         

##Variables         
```golang
var IllegalPrefixes = []string{
        "copyright",
        "all rights",
        "author",
}
```
##func Examples         
```golang
func Examples(files ...*ast.File) []*Example
```
Examples returns the examples found in the files, sorted by Name field.          
The Order fields record the order in which the examples were encountered.         
返回文件里发现的例子,以名字字段排序         
Order字段记录 遇到例子的顺序         


##func Synopsis         
```golang
func Synopsis(s string) string
```
Synopsis returns a cleaned version of the first sentence in s.          
That sentence ends after the first period followed by space and not preceded by exactly one uppercase letter.          
The result string has no \n, \r, or \t characters and uses only single spaces between words.          
If s starts with any of the IllegalPrefixes, the result is the empty string.         
返回s中干净版本的第一句话         
句子以 第一个周期遇到 空格之后 并且前面没有大写字母 结束         
返回的字符串没有\n, \r,  \t  字符 并且 单词之间只用 一个空格         
如果s以任何IllegalPrefixes开始,结果是空字符串         

##func ToHTML         
```golang
func ToHTML(w io.Writer, text string, words map[string]string)
```
ToHTML converts comment text to formatted HTML.          
The comment was prepared by DocReader, so it is known not to have leading, trailing blank lines nor to have trailing spaces at the end of lines.          
The comment markers have already been removed.         

Each span of unindented non-blank lines is converted into a single paragraph.          
There is one exception to the rule:          
a span that consists of a single line, is followed by another paragraph span, begins with a capital letter, and contains no punctuation is formatted as a heading.         

A span of indented lines is converted into a <pre> block, with the common indent prefix removed.         

URLs in the comment text are converted into links;          
if the URL also appears in the words map, the link is taken from the map          
(if the corresponding map value is the empty string, the URL is not converted into a link).         

Go identifiers that appear in the words map are italicized;          
if the corresponding map value is not the empty string, it is considered a URL and the word is converted into a link.         

转换注释文本成 HTML格式         
注释以DocReader准备,因此它 没有前导,尾随的空行，也不在行的末尾有尾随空格。         
注释标示已经被删除了         

每个非空白的跨度被转成单一的段         
有个例外:         
一个跨度是由一个单一的行,跟着其他的段跨度,以一个大写字母开始,并且标题不包含任何标点符号的格式         

一个缩进的行间距 转换为<pre>块,常见的缩进前缀移除。         

注释里的URL文本转换成连接;         
如果URL 也出现在map上, 该连接取自 map(如果相应的map值是非空的字符串,它被认为是一个URL，该词被转换成一个链接。)         


##func ToText         
```golang
func ToText(w io.Writer, text string, indent, preIndent string, width int)
```
ToText prepares comment text for presentation in textual output.          
It wraps paragraphs of text to width or fewer Unicode code points and then prefixes each line with the indent.         
In preformatted sections (such as program text), it prefixes each non-blank line with preIndent.         
ToText 在文本输出中 准备注释文本显示         
它包含 文字段落 宽 或 更少的 Unicode 然后前缀与缩进每一行         
在预格式化的章节(例如程序文本),它的前缀预缩进每个非空行         


##type Example         
```golang
type Example struct {
        Name        string // name of the item being exemplified
        Doc         string // example function doc string
        Code        ast.Node
        Play        *ast.File // a whole program version of the example
        Comments    []*ast.CommentGroup
        Output      string // expected output
        EmptyOutput bool   // expect empty output
        Order       int    // original source code order
}
```
An Example represents an example function found in a source files.         
代表在源文件中发现了一个示例函数。         


##type Filter         
```golang
type Filter func(string) bool
```

##type Func         
```golang
type Func struct {
        Doc  string
        Name string
        Decl *ast.FuncDecl

        // methods
        // (for functions, these fields have the respective zero value)
        Recv  string // actual   receiver "T" or "*T"
        Orig  string // original receiver "T" or "*T"
        Level int    // embedding level; 0 means not embedded
}
```
Func is the documentation for a func declaration.         
是一个函数声明的文件         


##type Mode         
```golang
type Mode int
```
Mode values control the operation of New.         
Mode值 控制New操作         
```golang
const (
        // extract documentation for all package-level declarations,
        // not just exported ones
           // 提取文档的所有包级声明,而不是只有1个
        AllDecls Mode = 1 << iota

        // show all embedded methods, not just the ones of
        // invisible (unexported) anonymous fields
           //显示所有嵌入方法,不只是 一个 隐藏的 匿名字段(不可以导出)
        AllMethods
)
```

##type Note         
```golang
type Note struct {
        Pos, End token.Pos // position range of the comment containing the marker
        						    //含有标记注释的位置范围
        						    
        UID      string    // uid found with the marker
        							//发现标示
        
        Body     string    // note body text
        							//注意正文
}
```
A Note represents a marked comment starting with "MARKER(uid): note body".          
Any note with a marker of 2 or more upper case [A-Z] letters and a uid of at least one character is recognized.          
The ":" following the uid is optional. Notes are collected in the Package.Notes map indexed by the notes marker.         
标示一个以"MARKER(uid): note body"开始的注释标示.         
任何备注 有2个标记或更多的  大写字母 [A-Z] 并且uid至少有一个字符         
":" 跟着可选的uid.Notes 根据备注标记集 Package.Notes  map 里的索引         


##type Package         
```golang
type Package struct {
        Doc        string
        Name       string
        ImportPath string
        Imports    []string
        Filenames  []string
        Notes      map[string][]*Note
        // DEPRECATED. For backward compatibility Bugs is still populated,
        // but all new code should use Notes instead.
           //为了向后兼容Bugs依旧是受欢迎的, 但是所有的新代码应该使用Notes代替
        Bugs []string

        // declarations
        Consts []*Value
        Types  []*Type
        Vars   []*Value
        Funcs  []*Func
}
```
Package is the documentation for an entire package.         
Package是整个包的文档         

##func New         
```golang
func New(pkg *ast.Package, importPath string, mode Mode) *Package
```
New computes the package documentation for the given package AST.          
New takes ownership of the AST pkg and may edit or overwrite it.         
New使用给定的包AST 计算 包文档.New取得ASTpkg所有权 然后 编辑或覆盖它         


##func (*Package) Filter         
```golang
func (p *Package) Filter(f Filter)
```
Filter eliminates documentation for names that don't pass through the filter f.          
TODO(gri): Recognize "Type.Method" as a name.         
消除没有通过过滤f的文档名         


##type Type         
```golang
type Type struct {
        Doc  string
        Name string
        Decl *ast.GenDecl

        // associated declarations
        Consts  []*Value // sorted list of constants of (mostly) this type
        						 //排序这个类型的常量列表
        						  
        Vars    []*Value // sorted list of variables of (mostly) this type
        						 //排序这个类型的变量列表
        						 
        Funcs   []*Func  // sorted list of functions returning this type
        						 //排序这个类型的函数返回列表
        
        Methods []*Func  // sorted list of methods (including embedded ones) of this type
        						 //排序这个类型的方法列表
}
```
Type is the documentation for a type declaration.         
Type 是一个类型声明的文档         


##type Value         
```golang
type Value struct {
        Doc   string
        Names []string // var or const names in declaration order
        Decl  *ast.GenDecl
        // contains filtered or unexported fields
}
```
Value is the documentation for a (possibly grouped) var or const declaration.         
Value 是一个var 或 const 声明 的文档         
