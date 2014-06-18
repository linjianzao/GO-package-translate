包地址:http://golang.org/pkg/go/format/              
#Package format         

##Overview
Package format implements standard formatting of Go source.         
format包实现GO源的标准格式         

##func Node         
```golang
func Node(dst io.Writer, fset *token.FileSet, node interface{}) error
```
Node formats node in canonical gofmt style and writes the result to dst.         

The node type must be *ast.File, *printer.CommentedNode, []ast.Decl, []ast.Stmt, or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt.          
Node does not modify node.          
Imports are not sorted for nodes representing partial source files (i.e., if the node is not an *ast.File or a *printer.CommentedNode not wrapping an *ast.File).         

The function may return early (before the entire result is written) and return a formatting error, for instance due to an incorrect AST.         

Node格式化 成 gofmt规范格式并 把结果写到dst         
node类型必须是*ast.File, *printer.CommentedNode, []ast.Decl, []ast.Stmt, or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt         
Node不会修改节点         
导入不会排序  表示部分源文件节点(例如:如果节点没有*ast.File 或 *printer.CommentedNode 没有 *ast.File )         
这个函数可能会提前返回 (在整个结果写入完之前) 并且返回一个格式化的错误,例如不正确的AST         


##func Source         
```golang
func Source(src []byte) ([]byte, error)
```
Source formats src in canonical gofmt style and returns the result or an (I/O or syntax) error.          
src is expected to be a syntactically correct Go source file, or a list of Go declarations or statements.         

If src is a partial source file, the leading and trailing space of src is applied to the result          
(such that it has the same leading and trailing space as src), and the result is indented by the same amount as the first line of src containing code.          
Imports are not sorted for partial source files.         

格式化src成规范的gofmt风格 并且返回结构 或 错误(I/O 或语法).         
src是预期的 正确的GO源文件语法,或 GO声明列表 或语句         

如果src是源文件的一部分,src的开始和结束空格应用到结果(使得它和src有同样的开头和结尾), 并且 结果是有同样数量的缩进 的src第一行代码         
部分源文件不会排序         