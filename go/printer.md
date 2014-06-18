包地址:http://golang.org/pkg/go/printer/       
#Package printer       

##Overview        
Package printer implements printing of AST nodes.       
实现打印AST节点       

##func Fprint       
```golang
func Fprint(output io.Writer, fset *token.FileSet, node interface{}) error
```
Fprint "pretty-prints" an AST node to output. It calls Config.Fprint with default settings.       
Fprint "漂亮的打印"AST节点输出. 它调用Config.Fprint 默认设置       

Example       
       
Code:       
```golang
    // Parse source file and extract the AST without comments for
    // this function, with position information referring to the
    // file set fset.
     //解析源文件并提取AST, 没有该函数的注释,位置信息 指的是文件集FSET
    funcAST, fset := parseFunc("example_test.go", "ExampleFprint")

    // Print the function body into buffer buf.
    // The file set is provided to the printer so that it knows
    // about the original source formatting and can add additional
    // line breaks where they were present in the source.
     // 打印函数体到缓冲区buf.文件集提供给printer那样  
     // 它可以知道源文件的格式并且可以在他们目前在源代码中添加另外的换行符
    
    var buf bytes.Buffer
    printer.Fprint(&buf, fset, funcAST.Body)

    // Remove braces {} enclosing the function body, unindent,
    // and trim leading and trailing white space.
     //删除大括号{}包围 函数体,取消缩进 和 去掉 开头和结尾的空白
    s := buf.String()
    s = s[1 : len(s)-1]
    s = strings.TrimSpace(strings.Replace(s, "\n\t", "\n", -1))

    // Print the cleaned-up body text to stdout.
    fmt.Println(s)
```
    
Output:       
```golang
funcAST, fset := parseFunc("example_test.go", "ExampleFprint")

var buf bytes.Buffer
printer.Fprint(&buf, fset, funcAST.Body)

s := buf.String()
s = s[1 : len(s)-1]
s = strings.TrimSpace(strings.Replace(s, "\n\t", "\n", -1))

fmt.Println(s)
type CommentedNode

type CommentedNode struct {
        Node     interface{} // *ast.File, or ast.Expr, ast.Decl, ast.Spec, or ast.Stmt
        Comments []*ast.CommentGroup
}
A CommentedNode bundles an AST node and corresponding comments. 
It may be provided as argument to any of the Fprint functions.
绑定一个AST节点和对应的注释.
它可以被当成参数提供给 任何的Fprint函数
```


##type Config       
```golang
type Config struct {
        Mode     Mode // default: 0
        Tabwidth int  // default: 8
        Indent   int  // default: 0 (all code is indented at least by this much)
}
```
A Config node controls the output of Fprint.       
控制Fprint的输出       

##func (*Config) Fprint       
```golang
func (cfg *Config) Fprint(output io.Writer, fset *token.FileSet, node interface{}) error
```
Fprint "pretty-prints" an AST node to output for a given configuration cfg.        
Position information is interpreted relative to the file set fset.        
The node type must be *ast.File, *CommentedNode, []ast.Decl, []ast.Stmt, or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt.       
使用给定的配置cfg输出 "漂亮的打印" AST节点       
位置信息 相对于文件集fset       
节点类型必须为  *ast.File, *CommentedNode, []ast.Decl, []ast.Stmt, or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt.       
       
##type Mode       
```golang
type Mode uint
```
A Mode value is a set of flags (or 0). They control printing.       
```golang
const (
        RawFormat Mode = 1 << iota // do not use a tabwriter; if set, UseSpaces is ignored
        									   //不使用tabwriter;如果设置,忽略UseSpaces
        
        TabIndent                  // use tabs for indentation independent of UseSpaces
        									   //使用独立的制表符缩进UseSpaces
        
        UseSpaces                  // use spaces instead of tabs for alignment
        								 	   //使用空格代替制表符
        
        SourcePos                  // emit //line comments to preserve original source positions
        									   //行注释以保留原始源位置
)
```