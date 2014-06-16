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
BranchStmt节点表示一个  break, continue, goto, 或 fallthrough语句     

##func (*BranchStmt) End     
```golang
func (s *BranchStmt) End() token.Pos
```

##func (*BranchStmt) Pos     
```golang
func (s *BranchStmt) Pos() token.Pos
```


##type CallExpr     
```golang
type CallExpr struct {
        Fun      Expr      // function expression
        Lparen   token.Pos // position of "("
        Args     []Expr    // function arguments; or nil
        Ellipsis token.Pos // position of "...", if any
        Rparen   token.Pos // position of ")"
}
```
A CallExpr node represents an expression followed by an argument list.     
表示一个表达式的参数列表        

##func (*CallExpr) End        
```golang
func (x *CallExpr) End() token.Pos
```

##func (*CallExpr) Pos        
```golang
func (x *CallExpr) Pos() token.Pos
```


##type CaseClause
```golang
type CaseClause struct {
        Case  token.Pos // position of "case" or "default" keyword
        List  []Expr    // list of expressions or types; nil means default case
        Colon token.Pos // position of ":"
        Body  []Stmt    // statement list; or nil
}
```
A CaseClause represents a case of an expression or type switch statement.        
一个表达式或类型switch语句的情况。        

##func (*CaseClause) End        
```golang
func (s *CaseClause) End() token.Pos
```

##func (*CaseClause) Pos        
```golang
func (s *CaseClause) Pos() token.Pos
```


##type ChanDir        
```golang
type ChanDir int
```
The direction of a channel type is indicated by one of the following constants.        
channel类型的方向是 下列常数操作之一。        

```golang
const (
        SEND ChanDir = 1 << iota
        RECV
)
```

##type ChanType        
```golang
type ChanType struct {
        Begin token.Pos // position of "chan" keyword or "<-" (whichever comes first)
        						//chan关键字 或 "<-" 的位置(哪个是前面就哪个)
        						
        Arrow token.Pos // position of "<-" (token.NoPos if there is no "<-")
        Dir   ChanDir   // channel direction
        Value Expr      // value type
}
```
A ChanType node represents a channel type.        
表示一个channel类型        

##func (*ChanType) End        
```golang
func (x *ChanType) End() token.Pos
```

##func (*ChanType) Pos        
```golang
func (x *ChanType) Pos() token.Pos
```


##type CommClause        
```golang
type CommClause struct {
        Case  token.Pos // position of "case" or "default" keyword
        Comm  Stmt      // send or receive statement; nil means default case
        Colon token.Pos // position of ":"
        Body  []Stmt    // statement list; or nil
}
```
A CommClause node represents a case of a select statement.        
表示一个select语句的情况。        

##func (*CommClause) End        
```golang
func (s *CommClause) End() token.Pos
```

##func (*CommClause) Pos        
```golang
func (s *CommClause) Pos() token.Pos
```

##type Comment        
```golang
type Comment struct {
        Slash token.Pos // position of "/" starting the comment
        Text  string    // comment text (excluding '\n' for //-style comments)
}
A Comment node represents a single //-style or /*-style comment.        
表示单个//-style 或 /*-style 注释

##func (*Comment) End        
```golang
func (c *Comment) End() token.Pos
```

##func (*Comment) Pos        
```golang
func (c *Comment) Pos() token.Pos
```

##type CommentGroup        
```golang
type CommentGroup struct {
        List []*Comment // len(List) > 0
}
```
A CommentGroup represents a sequence of comments with no other tokens and no empty lines between.        
表示 不空的行和没有其他tokens之间的 一序列的注释        

##func (*CommentGroup) End        
```golang
func (g *CommentGroup) End() token.Pos
```

##func (*CommentGroup) Pos        
```golang
##func (g *CommentGroup) Pos() token.Pos
```

##func (*CommentGroup) Text        
```golang
func (g *CommentGroup) Text() string
```
Text returns the text of the comment.         
Comment markers (//, /*, and */), the first space of a line comment, and leading and trailing empty lines are removed.         
Multiple empty lines are reduced to one, and trailing space on lines is trimmed.         
Unless the result is empty, it is newline-terminated.        
返回注释文本,注释标记(//, /*, 和 */),第一种是行注释,并且开头和结尾的空行都删除        
多个空行合并成一行,结果的空白会去掉.除非结果是空的,它是换行结束符        


##type CommentMap        
```golang
type CommntMap map[Node][]*CommentGroup
```
A CommentMap maps an AST node to a list of comment groups associated with it.         
See NewCommentMap for a description of the association.        
CommentMap 映射 AST节点 到对应的 注释组的列表        


##func NewCommentMap        
```golang
func NewCommentMap(fset *token.FileSet, node Node, comments []*CommentGroup) CommentMap
```
NewCommentMap creates a new comment map by associating comment groups of the comments list with the nodes of the AST specified by node.        
NewCommentMap 用指定的节点创建 一个新的注释 映射到对应的注释组 的 AST节点注释列表        

A comment group g is associated with a node n if:        
注释组g 对应节点n 如果:        
```golang
- g starts on the same line as n ends
     g 以同一行开始, n 结束

- g starts on the line immediately following n, and there is
    at least one empty line after g and before the next node
     开始的行紧接者n, 并且 在g 之后和下一个节点之前 至少有一个空行
    
- g starts before n and is not associated to the node before n
    via the previous rules
   g 在n之间开始 并且 没有相关的节点在n之间通过前面的规则
```
NewCommentMap tries to associate a comment group to the "largest" node possible:         
For instance, if the comment is a line comment trailing an assignment,         
the comment is associated with the entire assignment rather than just the last operand in the assignment.        
NewCommentMap尝试关联一个注释组 到 "最大的"节点:        
例如:如果注释是 行注释 结尾的赋值, 注释和整个过程有关联而不是最后一个操作数赋值 .        

##func (CommentMap) Comments        
```golang
func (cmap CommentMap) Comments() []*CommentGroup
```
Comments returns the list of comment groups in the comment map. The result is sorted is source order.        
返回在注释映射里的 注释组列表. 结果的顺序是源顺序        

##func (CommentMap) Filter        
```golang
func (cmap CommentMap) Filter(node Node) CommentMap
```
Filter returns a new comment map consisting of only those entries of cmap for which a corresponding node exists in the AST specified by node.        
		返回 一个新的  注释 映射 由 只有这些   指定的node  在AST里相应存在的node的      cmap条目             

##func (CommentMap) String             
```golang
func (cmap CommentMap) String() string
```

##func (CommentMap) Update             
```golang
func (cmap CommentMap) Update(old, new Node) Node
```
Update replaces an old node in the comment map with the new node and returns the new node.              
Comments that were associated with the old node are associated with the new node.             
使用新的节点 替换一个注释里的旧节点 并返回新的节点           
注释关联 旧节点和新节点的关联            
 

##type CompositeLit            
```golang
type CompositeLit struct {
        Type   Expr      // literal type; or nil
        Lbrace token.Pos // position of "{"
        Elts   []Expr    // list of composite elements; or nil
        Rbrace token.Pos // position of "}"
}
```
A CompositeLit node represents a composite literal.            
表示一个复合文字            

##func (*CompositeLit) End            
```golang
func (x *CompositeLit) End() token.Pos
```


##func (*CompositeLit) Pos            
```golang
func (x *CompositeLit) Pos() token.Pos
```


##type Decl            
```golang
type Decl interface {
        Node
        // contains filtered or unexported methods
}
```
All declaration nodes implement the Decl interface.            
所有声明实现Decl接口的 节点            


##type DeclStmt            
type DeclStmt struct {
        Decl Decl // *GenDecl with CONST, TYPE, or VAR token
}
A DeclStmt node represents a declaration in a statement list.            
表示一个在声明列表里的声明            

##func (*DeclStmt) End            
```golang
func (s *DeclStmt) End() token.Pos
```

##func (*DeclStmt) Pos            
```golang
func (s *DeclStmt) Pos() token.Pos
```


##type DeferStmt            
```golang
type DeferStmt struct {
        Defer token.Pos // position of "defer" keyword
        Call  *CallExpr
}
```
A DeferStmt node represents a defer statement.            
表示一个 延迟声明            

##func (*DeferStmt) End            
```golang
func (s *DeferStmt) End() token.Pos
```

##func (*DeferStmt) Pos            
```golang
func (s *DeferStmt) Pos() token.Pos
```


##type Ellipsis            
```golang
type Ellipsis struct {
        Ellipsis token.Pos // position of "..."
        Elt      Expr      // ellipsis element type (parameter lists only); or nil
}
```
An Ellipsis node stands for the "..." type in a parameter list or the "..." length in an array type.            
代表参数列表里的 "..." 类型 或  数组类型里 "..." 代表长度            

##func (*Ellipsis) End            
```golang
func (x *Ellipsis) End() token.Pos
```

##func (*Ellipsis) Pos            
```golang
func (x *Ellipsis) Pos() token.Pos
```


##type EmptyStmt            
```golang
type EmptyStmt struct {
        Semicolon token.Pos // position of preceding ";"
}
```
An EmptyStmt node represents an empty statement.             
The "position" of the empty statement is the position of the immediately preceding semicolon.            
表示一个空的声明. 空声明里的"position" 是紧接在前分号的位置。           

##func (*EmptyStmt) End            
```golang
func (s *EmptyStmt) End() token.Pos
```

##func (*EmptyStmt) Pos            
```golang
func (s *EmptyStmt) Pos() token.Pos
```

##type Expr            
```golang
type Expr interface {
        Node
        // contains filtered or unexported methods
}
```
All expression nodes implement the Expr interface.            
所有实现Expr 接口的 表达式节点            


##type ExprStmt            
```golang
type ExprStmt struct {
        X Expr // expression
}
```
An ExprStmt node represents a (stand-alone) expression in a statement list.                        
表示 声明列表里的 一个表达式            

##func (*ExprStmt) End            
```golang
func (s *ExprStmt) End() token.Pos
```

##func (*ExprStmt) Pos            
```golang
func (s *ExprStmt) Pos() token.Pos
```


##type Field            
```golang
type Field struct {
        Doc     *CommentGroup // associated documentation; or nil
        Names   []*Ident      // field/method/parameter names; or nil if anonymous field
        Type    Expr          // field/method/parameter type
        Tag     *BasicLit     // field tag; or nil
        Comment *CommentGroup // line comments; or nil
}
```
A Field represents a Field declaration list in a struct type, a method list in an interface type, or a parameter/result declaration in a signature.
表示 结构体类型的 声明的字段列表, 接口类型的方法列表,或者在一个签名里的  参数/结果  声明

##func (*Field) End            
```golang
func (f *Field) End() token.Pos
```

##func (*Field) Pos            
```golang
func (f *Field) Pos() token.Pos
```


##type FieldFilter            
```golang
type FieldFilter func(name string, value reflect.Value) bool           
```
A FieldFilter may be provided to Fprint to control the output.           
可以让Fprint 控制 输出          


##type FieldList            
```golang
type FieldList struct {
        Opening token.Pos // position of opening parenthesis/brace, if any
        List    []*Field  // field list; or nil
        Closing token.Pos // position of closing parenthesis/brace, if any
}
```
A FieldList represents a list of Fields, enclosed by parentheses or braces.            
表示一个 字段 列表,用括号或大括号括起来。            

##func (*FieldList) End            
```golang
func (f *FieldList) End() token.Pos
```

##func (*FieldList) NumFields            
```golang
func (f *FieldList) NumFields() int
```
NumFields returns the number of (named and anonymous fields) in a FieldList.            
返回FieldList里的数量(命名和匿名字段)            

##func (*FieldList) Pos            
```golang
func (f *FieldList) Pos() token.Pos
```

##type File            
```golang
type File struct {
        Doc        *CommentGroup   // associated documentation; or nil
        Package    token.Pos       // position of "package" keyword
        Name       *Ident          // package name
        Decls      []Decl          // top-level declarations; or nil
        Scope      *Scope          // package scope (this file only)
        Imports    []*ImportSpec   // imports in this file
        Unresolved []*Ident        // unresolved identifiers in this file
        Comments   []*CommentGroup // list of all comments in the source file
}
```
A File node represents a Go source file.                       
The Comments list contains all comments in the source file in order of appearance,                       
	including the comments that are pointed to from other nodes via Doc and Comment fields.                      
表示一个GO源文件           
注释列表包含 在源文件里的所有注释的出现顺序,           
包含从其他节点通过Doc和Comment字段指向的注释           


##func MergePackageFiles           
```golang
func MergePackageFiles(pkg *Package, mode MergeMode) *File
```
MergePackageFiles creates a file AST by merging the ASTs of the files belonging to a package.            
The mode flags control merging behavior.           
创建一个文件 AST 通过合并一个包里的文件的AST          

##func (*File) End           
```golang
func (f *File) End() token.Pos
```

##func (*File) Pos           
```golang
func (f *File) Pos() token.Pos
```


##type Filter          
```golang
type Filter func(string) bool
```


##type ForStmt          
```golang
type ForStmt struct {
        For  token.Pos // position of "for" keyword
        Init Stmt      // initialization statement; or nil
        Cond Expr      // condition; or nil
        Post Stmt      // post iteration statement; or nil
        Body *BlockStmt
}
```
A ForStmt represents a for statement.          
表示一个 for声明          

##func (*ForStmt) End          
```golang
func (s *ForStmt) End() token.Pos
```

##func (*ForStmt) Pos          
```golang
func (s *ForStmt) Pos() token.Pos
```

##type FuncDecl          
```golang
type FuncDecl struct {
        Doc  *CommentGroup // associated documentation; or nil
        Recv *FieldList    // receiver (methods); or nil (functions)
        Name *Ident        // function/method name
        Type *FuncType     // function signature: parameters, results, and position of "func" keyword
        Body *BlockStmt    // function body; or nil (forward declaration)
}
```
A FuncDecl node represents a function declaration.
表示一个函数声明

##func (*FuncDecl) End          
```golang
func (d *FuncDecl) End() token.Pos
```

##func (*FuncDecl) Pos          
```golang
func (d *FuncDecl) Pos() token.Pos
```


##type FuncLit          
```golang
type FuncLit struct {
        Type *FuncType  // function type
        Body *BlockStmt // function body
}
```
A FuncLit node represents a function literal.          
表示一个字面函数          

##func (*FuncLit) End          
```golang
func (x *FuncLit) End() token.Pos
```

##func (*FuncLit) Pos          
```golang
func (x *FuncLit) Pos() token.Pos
```


##type FuncType          
```golang
type FuncType struct {
        Func    token.Pos  // position of "func" keyword (token.NoPos if there is no "func")
        Params  *FieldList // (incoming) parameters; non-nil
        Results *FieldList // (outgoing) results; or nil
}
```
A FuncType node represents a function type.          
表示一个函数类型          

##func (*FuncType) End         
```golang
func (x *FuncType) End() token.Pos
```

##func (*FuncType) Pos         
```golang
func (x *FuncType) Pos() token.Pos
```


##type GenDecl         
```golang
type GenDecl struct {
        Doc    *CommentGroup // associated documentation; or nil
        TokPos token.Pos     // position of Tok
        Tok    token.Token   // IMPORT, CONST, TYPE, VAR
        Lparen token.Pos     // position of '(', if any
        Specs  []Spec
        Rparen token.Pos // position of ')', if any
}
```
A GenDecl node (generic declaration node) represents an import, constant, type or variable declaration.          
A valid Lparen position (Lparen.Line > 0) indicates a parenthesized declaration.         
表示一个引入,常数,类型,或变量 声明.         
一个有效的Lparen位置(Lparen.Line > 0) 一个括号的声明。         

Relationship between Tok value and Specs element type:                  
Tok 值和Specs元素 之间的关系:         
```golang
token.IMPORT  *ImportSpec
token.CONST   *ValueSpec
token.TYPE    *TypeSpec
token.VAR     *ValueSpec
```

##func (*GenDecl) End         
```golang
func (d *GenDecl) End() token.Pos
```

##func (*GenDecl) Pos         
```golang
func (d *GenDecl) Pos() token.Pos
```


##type GoStmt         
```golang
type GoStmt struct {
        Go   token.Pos // position of "go" keyword
        Call *CallExpr
}
```
A GoStmt node represents a go statement.         
表示一个go声明         

##func (*GoStmt) End         
```golang
func (s *GoStmt) End() token.Pos
```

##func (*GoStmt) Pos         
```golang
func (s *GoStmt) Pos() token.Pos
```


##type Ident         
```golang
type Ident struct {
        NamePos token.Pos // identifier position
        Name    string    // identifier name
        Obj     *Object   // denoted object; or nil
}
```
An Ident node represents an identifier.         
表示一个标识符         

##func NewIdent                  
```golang
func NewIdent(name string) *Ident
```
NewIdent creates a new Ident without position. Useful for ASTs generated by code other than the Go parser.                  
创建一个没有位置(position)的新Ident, 通过代码 比其他GO解析器 生成AST更有用         

##func (*Ident) End         
```glolang
func (x *Ident) End() token.Pos
```

##func (*Ident) IsExported         
```golang
func (id *Ident) IsExported() bool
```
IsExported reports whether id is an exported Go symbol (that is, whether it begins with an uppercase letter).                           
报告id 是否是一个可导出的GO符号(也就是说,它是否是以大写字母开头的)                  


##func (*Ident) Pos         
```golang
func (x *Ident) Pos() token.Pos
```

##func (*Ident) String         
```golang
func (id *Ident) String() string
```


##type IfStmt         
```golang
type IfStmt struct {
        If   token.Pos // position of "if" keyword
        Init Stmt      // initialization statement; or nil
        Cond Expr      // condition
        Body *BlockStmt
        Else Stmt // else branch; or nil
}
```
An IfStmt node represents an if statement.         
表示一个if语句         

##func (*IfStmt) End         
```golang
func (s *IfStmt) End() token.Pos
```

##func (*IfStmt) Pos         
```golang
func (s *IfStmt) Pos() token.Pos
```

type ImportSpec

type ImportSpec struct {
        Doc     *CommentGroup // associated documentation; or nil
        Name    *Ident        // local package name (including "."); or nil
        Path    *BasicLit     // import path
        Comment *CommentGroup // line comments; or nil
        EndPos  token.Pos     // end of spec (overrides Path.Pos if nonzero)
}
An ImportSpec node represents a single package import.
表示一个  单个的引入包

func (*ImportSpec) End

func (s *ImportSpec) End() token.Pos


func (*ImportSpec) Pos

func (s *ImportSpec) Pos() token.Pos
Pos and End implementations for spec nodes.
实现spec节点

type Importer

type Importer func(imports map[string]*Object, path string) (pkg *Object, err error)

An Importer resolves import paths to package Objects. 
The imports map records the packages already imported, indexed by package id (canonical import path). 
An Importer must determine the canonical import path and check the map to see if it is already present in the imports map.
If so, the Importer can return the map entry. 
Otherwise, the Importer should load the package data for the given path into a new *Object (pkg), 
 	record pkg in the imports map, and then return pkg.





















