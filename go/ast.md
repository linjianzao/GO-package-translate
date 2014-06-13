包地址：http://golang.org/pkg/go/ast/    
#Package ast    

##Overview    
Package ast declares the types used to represent syntax trees for Go packages.    
ast包声明了用于表示语法树的GO包的类型     


##func FileExports     
```golang
func FileExports(src *File) bool
```
FileExports trims the AST for a Go source file in place such that only exported nodes remain:      
	all top-level identifiers which are not exported and their associated information (such as type, initial value, or function body) are removed.      
	Non-exported fields and methods of exported types are stripped. The File.Comments list is not changed.     
FileExports returns true if there are exported declarations; it returns false otherwise.     
FileExports 在适当的地方修饰GO源文件的 AST, 那样只剩下出口节点:     
	所有的顶级标识符都不会导出 并且有相关的信息被删除(例如类型,初始化值,或者函数体)     
	非导出字段和方法 跳过导出类型.File.Comments列表不会变化     
如果FileExports有导出声明返回true;否则返回false.     

##func FilterDecl     
```golang
func FilterDecl(decl Decl, f Filter) bool
```
FilterDecl trims the AST for a Go declaration in place by removing all names      
	(including struct field and interface method names, but not from parameter lists) that don't pass through the filter f.          
FilterDecl returns true if there are any declared names left after filtering; it returns false otherwise.          
FilterDecl 在适当的地方通过删除所有未通过过滤器f 的名字 修饰GO定义的AST (包含结构体字段和接口方法名字,但是不是从参数列表来的)     


##func FilterFile     
```golang
func FilterFile(src *File, f Filter) bool
```
FilterFile trims the AST for a Go file in place by removing all names from top-level declarations      
	(including struct field and interface method names, but not from parameter lists) that don't pass through the filter f.      
If the declaration is empty afterwards, the declaration is removed from the AST. The File.Comments list is not changed.     
FilterFile returns true if there are any top-level declarations left after filtering; it returns false otherwise.     
在适当的地方 从顶级声明中 删除所有的 未通过过滤器f 的名称 来修饰GO定义的AST (包含结构体字段和接口方法名字,但是不是从参数列表来的)     
如果后来的声明是空的,这个声明从AST中删除.File.Comments 列表不会改变     
如果有任何的过滤后剩下的顶级声明, FilterFile 返回true.否则返回false     

##func FilterPackage     
```golang
func FilterPackage(pkg *Package, f Filter) bool
```
FilterPackage trims the AST for a Go package in place by removing all names from top-level declarations      
	(including struct field and interface method names, but not from parameter lists) that don't pass through the filter f.      
	If the declaration is empty afterwards, the declaration is removed from the AST.      
	The pkg.Files list is not changed, so that file names and top-level package comments don't get lost.     
FilterPackage returns true if there are any top-level declarations left after filtering; it returns false otherwise.     
在适当的地方 从顶级声明中 删除所有的 未通过过滤器f 的名称 来修饰GO定义的AST (包含结构体字段和接口方法名字,但是不是从参数列表来的)     
如果后来的声明是空的,这个声明从AST中删除. pkg.Files 列表不会改变,所以那样文件名和顶级包不会丢失.     
如果有任何的过滤后剩下的顶级声明, FilterPackage 返回true.否则返回false     


##func Fprint     
```golang
func Fprint(w io.Writer, fset *token.FileSet, x interface{}, f FieldFilter) (err error)
```
Fprint prints the (sub-)tree starting at AST node x to w.      
If fset != nil, position information is interpreted relative to that file set.      
Otherwise positions are printed as integer values (file set specific offsets).     

A non-nil FieldFilter f may be provided to control the output: struct fields for which f(fieldname, fieldvalue) is true are printed;      
all others are filtered from the output. Unexported struct fields are never printed.     
打印出 从AST 开始的 (子)树 从x到w的节点     
如果fset != nil,位置信息被解释成该文件集     
否则 位置被打印成整数值(文件集指定的偏移量)     
非nil FieldFilter f 可以控制输出:结构体字段  f(fieldname, fieldvalue) 打印 true; 从所以其他输出过滤.未导出的字段不会打印     


##func Inspect     
```golang
func Inspect(node Node, f func(Node) bool)
```
Inspect traverses an AST in depth-first order:      
	It starts by calling f(node); node must not be nil.      
	If f returns true, Inspect invokes f for all the non-nil children of node, recursively.     
Inspect 根据深度优先顺序遍历AST:从调用 f(node)开始;node必须不为nil.     
	如果f返回true,Inspect 为所有的非空子节点递归调用f     
	
	
##Example     
This example demonstrates how to inspect the AST of a Go program.     
这个例子演示了 GO 程序怎么检查AST.     
```golang
package main

import (
	"fmt"
	"go/ast"
	"go/parser"
	"go/token"
)

func main() {
	// src is the input for which we want to inspect the AST.
	//src 是我们项检查的AST输入
	src := `
package p
const c = 1.0
var X = f(3.14)*2 + c
`

	// Create the AST by parsing src.
	//解析src创建AST
	fset := token.NewFileSet() // positions are relative to fset   位置相对fset
	f, err := parser.ParseFile(fset, "src.go", src, 0)
	if err != nil {
		panic(err)
	}

	// Inspect the AST and print all identifiers and literals.
	//检查AST 并 打印所有的标识符和文字。
	ast.Inspect(f, func(n ast.Node) bool {
		var s string
		switch x := n.(type) {
		case *ast.BasicLit:
			s = x.Value
		case *ast.Ident:
			s = x.Name
		}
		if s != "" {
			fmt.Printf("%s:\t%s\n", fset.Position(n.Pos()), s)
		}
		return true
	})

}
```

##func IsExported     
```golang
func IsExported(name string) bool
```
IsExported reports whether name is an exported Go symbol (that is, whether it begins with an upper-case letter).     
报道 name 是否是GO的可导出符号(也就是说，无论是开头的大写字母).     


##func NotNilFilter     
```golang
func NotNilFilter(_ string, v reflect.Value) bool
```
NotNilFilter returns true for field values that are not nil; it returns false otherwise.     
字段值非nil时返回true;否则返回false     


##func PackageExports     
```golang
func PackageExports(pkg *Package) bool
```
PackageExports trims the AST for a Go package in place such that only exported nodes remain.      
The pkg.Files list is not changed, so that file names and top-level package comments don't get lost.     
PackageExports returns true if there are exported declarations; it returns false otherwise.     

PackageExports 在适当的地方删除Go包里的AST,这样仅保留出口节点     
pkg.Files列表不会改变,所以 那个 文件名 和顶级包 不会丢失     
如果PackageExports有导出声明返回true;否则返回false.         

##func Print     
```golang
func Print(fset *token.FileSet, x interface{}) error
```
Print prints x to standard output, skipping nil fields.      
Print(fset, x) is the same as Fprint(os.Stdout, fset, x, NotNilFilter).     
打印x到标准输出,跳过nil字段     
Print(fset, x)  和Fprint(os.Stdout, fset, x, NotNilFilter) 一样     


##Example     
This example shows what an AST looks like when printed for debugging.     
这个例子展示了当打印debugging时 AST 看起来像什么     
```golang
package main

import (
	"go/ast"
	"go/parser"
	"go/token"
)

func main() {
	// src is the input for which we want to print the AST.
	//src是我们想打印AST的输入
	src := `
package main
func main() {
	println("Hello, World!")
}
`

	// Create the AST by parsing src.
	/解析src创建AST
	fset := token.NewFileSet() // positions are relative to fset fset的相对位置偏移量
	f, err := parser.ParseFile(fset, "", src, 0)
	if err != nil {
		panic(err)
	}

	// Print the AST.
	ast.Print(fset, f)

}
```


##func SortImports     
```golang
func SortImports(fset *token.FileSet, f *File)
```
SortImports sorts runs of consecutive import lines in import blocks in f.      
It also removes duplicate imports when it is possible to do so without data loss.     
SortImports 排序 连续运行的 在f中引入块 中的引入线       
它也会删除重复 引入, 可以这样做, 不会损失数据     

##func Walk     
```golang
func Walk(v Visitor, node Node)
```
Walk traverses an AST in depth-first order:      
It starts by calling v.Visit(node); node must not be nil.      
	If the visitor w returned by v.Visit(node) is not nil,      
	Walk is invoked recursively with visitor w for each of the non-nil children of node,      
	followed by a call of w.Visit(nil).     
根据深度优先顺序遍历AST:     
它以调用v.Visit(node)开始;node必须不为nil.如果w以v.Visit(node)返回 是nil,     
为每个非nil的子节点w 递归调用Walk      
跟着调用w.Visit(nil).     


##type ArrayType     
```golang
type ArrayType struct {
        Lbrack token.Pos // position of "["
        						 //"["的位置
        						 
        Len    Expr      // Ellipsis node for [...]T array types, nil for slice types
								 //省略[...]T 数组类型的节点,slice类型是nil
								 
        Elt    Expr      // element type
        						 //元素类型
}
```
An ArrayType node represents an array or slice type.     
ArrayType节点表示一个数组或slice     


##func (*ArrayType) End     
```golang
func (x *ArrayType) End() token.Pos
```

##func (*ArrayType) Pos     
```golang
func (x *ArrayType) Pos() token.Pos
```


##type AssignStmt     
```golang
type AssignStmt struct {
        Lhs    []Expr
        TokPos token.Pos   // position of Tok
        							//Tok的位置
        							
        Tok    token.Token // assignment token, DEFINE
        							//赋值token,
        
        Rhs    []Expr
}
```
An AssignStmt node represents an assignment or a short variable declaration.     
AssignStmt节点表示一个赋值或短的变量声明     


##func (*AssignStmt) End     
```golang
func (s *AssignStmt) End() token.Pos
```


##func (*AssignStmt) Pos     
```golang
func (s *AssignStmt) Pos() token.Pos
```

##type BadDecl     
```golang
type BadDecl struct {
        From, To token.Pos // position range of bad declaration
        							//糟糕的声明位置范围
}
```
A BadDecl node is a placeholder for declarations containing syntax errors for which no correct declaration nodes can be created.     
BadDecl节点是一个占位符,用于声明包含语法错误不正确的声明可以创建节点。     


##func (*BadDecl) End     
```golang
func (d *BadDecl) End() token.Pos
```


##func (*BadDecl) Pos     
```golang
func (d *BadDecl) Pos() token.Pos
```
Pos and End implementations for declaration nodes.     
Pos 和 End实现 声明节点     


##type BadExpr     
```golang
type BadExpr struct {
        From, To token.Pos // position range of bad expression
}
```
A BadExpr node is a placeholder for expressions containing syntax errors for which no correct expression nodes can be created.     
BadExpr节点是一个占位符,用于声明包含语法错误不正确的声明可以创建节点。     

##func (*BadExpr) End     
```golang
func (x *BadExpr) End() token.Pos
```


##func (*BadExpr) Pos     
```golang
func (x *BadExpr) Pos() token.Pos
```
Pos and End implementations for expression/type nodes.     
Pos 和 End实现 表达式/类型 节点     


##type BadStmt     
```golang
type BadStmt struct {
        From, To token.Pos // position range of bad statement
        							//坏的语句位置范围
}
```
A BadStmt node is a placeholder for statements containing syntax errors for which no correct statement nodes can be created.     
BadExpr节点是一个占位符,用于声明包含语法错误不正确的声明可以创建节点。     


##func (*BadStmt) End     
```golang
func (s *BadStmt) End() token.Pos
```


##func (*BadStmt) Pos          
```golang
func (s *BadStmt) Pos() token.Pos
```     
Pos and End implementations for statement nodes.          
Pos 和 End实现  声明 节点       


##type BasicLit     
```golang
type BasicLit struct {
        ValuePos token.Pos   // literal position
        							  //文字位置
        
        Kind     token.Token // token.INT, token.FLOAT, token.IMAG, token.CHAR, or token.STRING
        
        Value    string      // literal string; e.g. 42, 0x7f, 3.14, 1e-9, 2.4i, 'a', '\x7f', "foo" or `\m\n\o`
        							   //文字字符串;例如 42, 0x7f, 3.14, 1e-9, 2.4i, 'a', '\x7f', "foo" 或 `\m\n\o`
}
```
A BasicLit node represents a literal of basic type.     
表示一个基础类型文字     

##func (*BasicLit) End     
```golang
func (x *BasicLit) End() token.Pos
```

func (*BasicLit) Pos     
```golang
func (x *BasicLit) Pos() token.Pos
```

##type BinaryExpr     
```golang
type BinaryExpr struct {
        X     Expr        // left operand
        OpPos token.Pos   // position of Op
        Op    token.Token // operator
        Y     Expr        // right operand
}
```
A BinaryExpr node represents a binary expression.     
表示一个二进制表达式     

##func (*BinaryExpr) End          
```golang
func (x *BinaryExpr) End() token.Pos
```

func (*BinaryExpr) Pos     
```golang
func (x *BinaryExpr) Pos() token.Pos
```

##type BlockStmt     
```golang
type BlockStmt struct {
        Lbrace token.Pos // position of "{"
        List   []Stmt
        Rbrace token.Pos // position of "}"
}
A BlockStmt node represents a braced statement list.     
表示一个支撑声明的列表     
```

##func (*BlockStmt) End     
```golang
func (s *BlockStmt) End() token.Pos
```

##func (*BlockStmt) Pos     
```golang
func (s *BlockStmt) Pos() token.Pos
```

##type BranchStmt     
```golang
type BranchStmt struct {
        TokPos token.Pos   // position of Tok
        Tok    token.Token // keyword token (BREAK, CONTINUE, GOTO, FALLTHROUGH)
        Label  *Ident      // label name; or nil
}
```
A BranchStmt node represents a break, continue, goto, or fallthrough statement.     


##func (*BranchStmt) End     
```golang
func (s *BranchStmt) End() token.Pos
```

##func (*BranchStmt) Pos     
```golang
func (s *BranchStmt) Pos() token.Pos
```































































