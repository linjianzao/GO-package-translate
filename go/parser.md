包地址:http://golang.org/pkg/go/parser/           
#Package parser           

##Overview            

Package parser implements a parser for Go source files.            
Input may be provided in a variety of forms (see the various Parse* functions);            
the output is an abstract syntax tree (AST) representing the Go source.            
The parser is invoked through one of the Parse* functions.           
parser包实现 GO源文件的解析           
输入形式可以是各种各样的           
输出是表示GO源的 抽象 语法树           
解析器调用Parse* 中的函数           


##func ParseDir           
```golang
func ParseDir(fset *token.FileSet, path string, filter func(os.FileInfo) bool, mode Mode) (pkgs map[string]*ast.Package, first error)
```
ParseDir calls ParseFile for all files with names ending in ".go" in the directory specified by path            
	and returns a map of package name -> package AST with all the packages found.           

If filter != nil, only the files with os.FileInfo entries passing through the filter (and ending in ".go") are considered.            
The mode bits are passed to ParseFile unchanged. Position information is recorded in fset.           

If the directory couldn't be read, a nil map and the respective error are returned.            
If a parse error occurred, a non-nil but incomplete map and the first error encountered are returned.           
ParseDir 对指定路径的目录的 以".go"结尾的所有文件名 调用ParseFile并 返回 所有包里发现的 name -> package AST 的map           

如果filter!=nil,只有通过过滤的 文件 被认可 (并且要以".go"结尾)           
mode 位 传递给ParseFile不变.位置信息记录在fset           
           
如果目录不能读取,返回nil 的map 和各自的错误           
如果解析遇到错误, 返回非空 但是不完整的map 和第一个遇到的错误           


##func ParseExpr           
```golang
func ParseExpr(x string) (ast.Expr, error)
```
ParseExpr is a convenience function for obtaining the AST of an expression x.            
The position information recorded in the AST is undefined.            
The filename used in error messages is the empty string.           
ParseExpr方便函数从 表达式x 获取AST           
记录在AST的位置信息是不确定的。           
在错误消息中使用的文件名是空字符串。           


##func ParseFile           
```golang
func ParseFile(fset *token.FileSet, filename string, src interface{}, mode Mode) (f *ast.File, err error)
```
ParseFile parses the source code of a single Go source file and returns the corresponding ast.File node.            
The source code may be provided via the filename of the source file, or via the src parameter.           

If src != nil, ParseFile parses the source from src and the filename is only used when recording position information.            
The type of the argument for the src parameter must be string, []byte, or io.Reader.            
If src == nil, ParseFile parses the file specified by filename.           

The mode parameter controls the amount of source text parsed and other optional parser functionality.            
Position information is recorded in the file set fset.           

If the source couldn't be read, the returned AST is nil and the error indicates the specific failure.            
If the source was read but syntax errors were found, the result is a partial AST (with ast.Bad* nodes representing the fragments of erroneous source code).            
Multiple errors are returned via a scanner.ErrorList which is sorted by file position.           
解析单个GO文件的源码 返回对应的ast.File节点           
源代码也可能 会通过filename的 源文件 或 通过 src的参数           

如果src!=nil,ParseFile从src中解析源 并使用 filename 记录位置信息           
src参数的类型必须是 string, []byte, 或 io.Reader.            
如果src==nil,ParseFile  根据指定的filename 解析文件           

mode参数控制与解析源文本的数量和其他可选的解析器功能。           
位置信息被记录在该文件集fset。           

如果源不能读取,返回的AST是nil并且 该错误指示的具体故障。           
如果源可读 但是发现语法错误,结果是一部分AST(ast.Bad*节点 表示错误的源代码片断)           
通过scanner.ErrorList返回多个错误,用文件位置排序           


▾ Example           
```golang
package main

import (
	"fmt"
	"go/parser"
	"go/token"
)

func main() {
	fset := token.NewFileSet() // positions are relative to fset  相对fset的位置

	// Parse the file containing this very example
	// but stop after processing the imports.
	//解析文件包含的例子,但停止处理后的导入
	f, err := parser.ParseFile(fset, "example_test.go", nil, parser.ImportsOnly)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Print the imports from the file's AST.
	//打印 文件AST的导入
	for _, s := range f.Imports {
		fmt.Println(s.Path.Value)
	}

}
```



##type Mode           
```golang
type Mode uint
```
A Mode value is a set of flags (or 0). They control the amount of source code parsed and other optional parser functionality.           
Mode 值是 一个 flag 集(或者0).他们控制源代码的解析数量 和其他可选的解析器功能。           
```golang
const (
        PackageClauseOnly Mode             = 1 << iota // stop parsing after package clause
        ImportsOnly                                    // stop parsing after import declarations
        ParseComments                                  // parse comments and add them to AST
        Trace                                          // print a trace of parsed productions
        DeclarationErrors                              // report declaration errors
        SpuriousErrors                                 // same as AllErrors, for backward-compatibility
        AllErrors         = SpuriousErrors             // report all errors (not just the first 10 on different lines)
)
```





































