Package parse

Overview ▾

Package parse builds parse trees for templates as defined by text/template and html/template. 
Clients should use those packages to construct templates rather than this one, which provides shared internal data structures not intended for general use.
parse包 为模版 构建 定义在 text/template 和 html/template的 解析树 .
客户端应该使用这些包来构造模版  而不是这个包,  这规定不适用于一般用途的共享内部数据结构。


func IsEmptyTree
```golang
func IsEmptyTree(n Node) bool
```
IsEmptyTree reports whether this tree (node) is empty of everything but space.
IsEmptyTree 报告 这个树(节点) 是否 是空的 但是有空间.


func Parse
```golang
func Parse(name, text, leftDelim, rightDelim string, funcs ...map[string]interface{}) (treeSet map[string]*Tree, err error)
```
Parse returns a map from template name to parse.Tree, created by parsing the templates described in the argument string. 
The top-level template will be given the specified name. If an error is encountered, parsing stops and an empty map is returned with the error.
Parse 从 模版name 返回一个map 到parse.Tree ,通过  解析描述在参数字符串的模版 创建.
高级模版会指定name. 如果遇到错误, 解析停止 返回的map是空的并返回错误



type ActionNode
```golang
type ActionNode struct {
        NodeType
        Pos
        Line int       // The line number in the input (deprecated; kept for compatibility)
        Pipe *PipeNode // The pipeline in the action.
}
```
ActionNode holds an action (something bounded by delimiters). Control actions have their own nodes; 
ActionNode represents simple ones such as field evaluations and parenthesized pipelines.
ActionNode 持有一个 action(被分隔符为界). 控制动作有自己的节点;
ActionNode 表示简单的一个 例如 现场评估和括号的管道。



func (*ActionNode) Copy
```golang
func (a *ActionNode) Copy() Node
```


func (*ActionNode) String
```golang
func (a *ActionNode) String() string
```


type BoolNode
```golang
type BoolNode struct {
        NodeType
        Pos
        True bool // The value of the boolean constant.
}
```
BoolNode holds a boolean constant.
BoolNode 持有布尔常量。



func (*BoolNode) Copy
```golang
func (b *BoolNode) Copy() Node
```


func (*BoolNode) String
```golang
func (b *BoolNode) String() string
```



type BranchNode
```golang
type BranchNode struct {
        NodeType
        Pos
        Line     int       // The line number in the input (deprecated; kept for compatibility)
        Pipe     *PipeNode // The pipeline to be evaluated.
        List     *ListNode // What to execute if the value is non-empty.
        ElseList *ListNode // What to execute if the value is empty (nil if absent).
}
```
BranchNode is the common representation of if, range, and with.
BranchNode 是常见的if, range, 和 with.



func (*BranchNode) String
```golang
func (b *BranchNode) String() string
```


type ChainNode
```golang
type ChainNode struct {
        NodeType
        Pos
        Node  Node
        Field []string // The identifiers in lexical order.
}
```
ChainNode holds a term followed by a chain of field accesses (identifier starting with '.'). 
The names may be chained ('.x.y'). The periods are dropped from each ident.
ChainNode 持有长期 连锁领域的访问(标示符以'.'开始).
名称可以是链式的 ('.x.y'). 该周期是从每一个IDENT下降。


func (*ChainNode) Add
```golang
func (c *ChainNode) Add(field string)
```
Add adds the named field (which should start with a period) to the end of the chain.
Add添加字段名到 链的结尾.(必须以句号开始)



func (*ChainNode) Copy
```golang
func (c *ChainNode) Copy() Node
```


func (*ChainNode) String
```golang
func (c *ChainNode) String() string
```



type CommandNode
```golang
type CommandNode struct {
        NodeType
        Pos
        Args []Node // Arguments in lexical order: Identifier, field, or constant.
}
```
CommandNode holds a command (a pipeline inside an evaluating action).
CommandNode 持有一个命令



func (*CommandNode) Copy
```golang
func (c *CommandNode) Copy() Node
```



func (*CommandNode) String
```golang
func (c *CommandNode) String() string
```



type DotNode
```golang
type DotNode struct {
        Pos
}
```
DotNode holds the special identifier '.'.
DotNode 持有特殊的标示符'.'.



func (*DotNode) Copy
```golang
func (d *DotNode) Copy() Node
```


func (*DotNode) String
```golang
func (d *DotNode) String() string
```



func (*DotNode) Type
```golang
func (d *DotNode) Type() NodeType
```



type FieldNode
```golang
type FieldNode struct {
        NodeType
        Pos
        Ident []string // The identifiers in lexical order.
}
```
FieldNode holds a field (identifier starting with '.'). The names may be chained ('.x.y'). The period is dropped from each ident.
FieldNode 持有一个 字段(标示符以 '.' 开始). 名字可以是链式的('.x.y').该周期是从每一个IDENT下降。



func (*FieldNode) Copy
```golang
func (f *FieldNode) Copy() Node
```



func (*FieldNode) String
```golang
func (f *FieldNode) String() string
```



type IdentifierNode
```golang
type IdentifierNode struct {
        NodeType
        Pos
        Ident string // The identifier's name.
}
```
IdentifierNode holds an identifier.
IdentifierNode 持有一个标示符



func NewIdentifier
```golang
func NewIdentifier(ident string) *IdentifierNode
```
NewIdentifier returns a new IdentifierNode with the given identifier name.
NewIdentifier 用给定的标示符名字返回一个新的 IdentifierNode



func (*IdentifierNode) Copy
```golang
func (i *IdentifierNode) Copy() Node
```


func (*IdentifierNode) SetPos
```golang
func (i *IdentifierNode) SetPos(pos Pos) *IdentifierNode
```
SetPos sets the position. NewIdentifier is a public method so we can't modify its signature. Chained for convenience. TODO: fix one day?
SetPos 设置位置.NewIdentifier 是一个公开的方法 所以我们不能修改它的签名.  链接方便. TODO: fix one day?



func (*IdentifierNode) String
```golang
func (i *IdentifierNode) String() string
```



type IfNode
```golang
type IfNode struct {
        BranchNode
}
```
IfNode represents an {{if}} action and its commands.
IfNode 表示一个 {{if}} action 和它的命令.


func (*IfNode) Copy
```golang
func (i *IfNode) Copy() Node
```



type ListNode
```golang
type ListNode struct {
        NodeType
        Pos
        Nodes []Node // The element nodes in lexical order.
}
```
ListNode holds a sequence of nodes.
ListNode持有一个节点序列



func (*ListNode) Copy
```golang
func (l *ListNode) Copy() Node
```



func (*ListNode) CopyList
```golang
func (l *ListNode) CopyList() *ListNode
```



func (*ListNode) String
```golang
func (l *ListNode) String() string
```



type NilNode
```golang
type NilNode struct {
        Pos
}
```
NilNode holds the special identifier 'nil' representing an untyped nil constant.
NilNode 持有特殊标示符  'nil' 代表非零常数。



func (*NilNode) Copy
```golang
func (n *NilNode) Copy() Node
```


func (*NilNode) String
```golang
func (n *NilNode) String() string
```



func (*NilNode) Type
```golang
func (n *NilNode) Type() NodeType
```



type Node
```golang
type Node interface {
        Type() NodeType
        String() string
        // Copy does a deep copy of the Node and all its components.
        // To avoid type assertions, some XxxNodes also have specialized
        // CopyXxx methods that return *XxxNode.
        Copy() Node
        Position() Pos // byte position of start of node in full original input string
        // contains filtered or unexported methods
}
```
A Node is an element in the parse tree. The interface is trivial. 
The interface contains an unexported method so that only types local to this package can satisfy it.
Node 是 在解析树的一个元素. 该接口是不重要的.
接口常量一个unexported 方法 所以只有这个包的本地类型可以满足它.



type NodeType
```golang
type NodeType int
```
NodeType identifies the type of a parse tree node.
NodeType 标识解析树节点的类型。

```golang
const (
        NodeText    NodeType = iota // Plain text.
        NodeAction                  // A non-control action such as a field evaluation.
        NodeBool                    // A boolean constant.
        NodeChain                   // A sequence of field accesses.
        NodeCommand                 // An element of a pipeline.
        NodeDot                     // The cursor, dot.

        NodeField      // A field or method name.
        NodeIdentifier // An identifier; always a function name.
        NodeIf         // An if action.
        NodeList       // A list of Nodes.
        NodeNil        // An untyped nil constant.
        NodeNumber     // A numerical constant.
        NodePipe       // A pipeline of commands.
        NodeRange      // A range action.
        NodeString     // A string constant.
        NodeTemplate   // A template invocation action.
        NodeVariable   // A $ variable.
        NodeWith       // A with action.
)
```



func (NodeType) Type
```golang
func (t NodeType) Type() NodeType
```
Type returns itself and provides an easy default implementation for embedding in a Node. Embedded in all non-trivial Nodes.
Type  返回它本身 和 提供一个简单的 默认实现 嵌套在Node里.嵌入在所有重要节点。



type NumberNode
```golang
type NumberNode struct {
        NodeType
        Pos
        IsInt      bool       // Number has an integral value.
        IsUint     bool       // Number has an unsigned integral value.
        IsFloat    bool       // Number has a floating-point value.
        IsComplex  bool       // Number is complex.
        Int64      int64      // The signed integer value.
        Uint64     uint64     // The unsigned integer value.
        Float64    float64    // The floating-point value.
        Complex128 complex128 // The complex value.
        Text       string     // The original textual representation from the input.
}
```
NumberNode holds a number: signed or unsigned integer, float, or complex. 
The value is parsed and stored under all the types that can represent the value. 
This simulates in a small amount of code the behavior of Go's ideal constants.
NumberNode 持有一个号码:符号或无符号 整型,浮点型,或 复合类型.
该值解析和存储所有它能表示的值 类型.
这个模拟在少量的代码的行为GO的理想的常量



func (*NumberNode) Copy
```golang
func (n *NumberNode) Copy() Node
```


func (*NumberNode) String
```golang
func (n *NumberNode) String() string
```


type PipeNode
```golang
type PipeNode struct {
        NodeType
        Pos
        Line int             // The line number in the input (deprecated; kept for compatibility)
        Decl []*VariableNode // Variable declarations in lexical order.
        Cmds []*CommandNode  // The commands in lexical order.
}
```
PipeNode holds a pipeline with optional declaration
PipeNode 持有一个 可选声明管道



func (*PipeNode) Copy
```golang
func (p *PipeNode) Copy() Node
```


func (*PipeNode) CopyPipe
```golang
func (p *PipeNode) CopyPipe() *PipeNode
```


func (*PipeNode) String
```golang
func (p *PipeNode) String() string
```


type Pos
```golang
type Pos int
```
Pos represents a byte position in the original input text from which this template was parsed.
Pos 表示从这个模版解析的源输入文本的一个字节位置   


func (Pos) Position
```golang
func (p Pos) Position() Pos
```



type RangeNode
```golang
type RangeNode struct {
        BranchNode
}
```
RangeNode represents a {{range}} action and its commands.
RangeNode 表示一个  {{range}} action 和它的命令



func (*RangeNode) Copy
```golang
func (r *RangeNode) Copy() Node
```



type StringNode
```golang
type StringNode struct {
        NodeType
        Pos
        Quoted string // The original text of the string, with quotes.
        Text   string // The string, after quote processing.
}
```
StringNode holds a string constant. The value has been "unquoted".
StringNode 持有一个字符串常量. 该值"unquoted".



func (*StringNode) Copy
```golang
func (s *StringNode) Copy() Node
```



func (*StringNode) String
```golang
func (s *StringNode) String() string
```


type TemplateNode
```golang
type TemplateNode struct {
        NodeType
        Pos
        Line int       // The line number in the input (deprecated; kept for compatibility)
        Name string    // The name of the template (unquoted).
        Pipe *PipeNode // The command to evaluate as dot for the template.
}
```
TemplateNode represents a {{template}} action.
TemplateNode 表示一个 {{template}} action.



func (*TemplateNode) Copy
```golang
func (t *TemplateNode) Copy() Node
```


func (*TemplateNode) String
```golang
func (t *TemplateNode) String() string
```


type TextNode
```golang
type TextNode struct {
        NodeType
        Pos
        Text []byte // The text; may span newlines.
}
```
TextNode holds plain text.
TextNode 持有纯文本。



func (*TextNode) Copy
```golang
func (t *TextNode) Copy() Node
```


func (*TextNode) String
```golang
func (t *TextNode) String() string
```



type Tree
```golang
type Tree struct {
        Name      string    // name of the template represented by the tree.
        ParseName string    // name of the top-level template during parsing, for error messages.
        Root      *ListNode // top-level root of the tree.
        // contains filtered or unexported fields
}
```
Tree is the representation of a single parsed template.
Tree 表示单个解析模版,



func New
```golang
func New(name string, funcs ...map[string]interface{}) *Tree
```
New allocates a new parse tree with the given name.
New 用给定的name分配一个新的解析树



func (*Tree) Copy
```golang
func (t *Tree) Copy() *Tree
```
Copy returns a copy of the Tree. Any parsing state is discarded.
Copy 返回 复制Tree.  任何解析状态都不可用



func (*Tree) ErrorContext
```golang
func (t *Tree) ErrorContext(n Node) (location, context string)
```
ErrorContext returns a textual representation of the location of the node in the input text.
ErrorContext 返回一个文本表示 输入文本里 节点的位置.



func (*Tree) Parse
```golang
func (t *Tree) Parse(text, leftDelim, rightDelim string, treeSet map[string]*Tree, funcs ...map[string]interface{}) (tree *Tree, err error)
```
Parse parses the template definition string to construct a representation of the template for execution. 
If either action delimiter string is empty, the default ("{{" or "}}") is used. Embedded template definitions are added to the treeSet map.
Parse 解析模版定义字符串 为执行构造一个模版表示. 如果action 分隔符字符串是空的, 默认使用("{{" or "}}") .嵌入模版定义 添加到 treeSet map.



type VariableNode
```golang
type VariableNode struct {
        NodeType
        Pos
        Ident []string // Variable name and fields in lexical order.
}
```
VariableNode holds a list of variable names, possibly with chained field accesses. The dollar sign is part of the (first) name.
VariableNode 持有一个变量名列表, 可能 链式 访问. 美元符号是(第一个)名字的一部分



func (*VariableNode) Copy
```golang
func (v *VariableNode) Copy() Node
```



func (*VariableNode) String
```golang
func (v *VariableNode) String() string
```


type WithNode
```golang
type WithNode struct {
        BranchNode
}
```
WithNode represents a {{with}} action and its commands.
WithNode 表示一个{{with}} action 和它的命令




func (*WithNode) Copy
```golang
func (w *WithNode) Copy() Node
```





