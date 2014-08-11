Package template

Overview ▾

Package template implements data-driven templates for generating textual output.

To generate HTML output, see package html/template, which has the same interface as this package but automatically secures HTML output against certain attacks.

Templates are executed by applying them to a data structure. 
Annotations in the template refer to elements of the data structure (typically a field of a struct or a key in a map) to control execution and derive values to be displayed. 
Execution of the template walks the structure and sets the cursor, represented by a period '.' and called "dot", to the value at the current location in the structure as execution proceeds.

The input text for a template is UTF-8-encoded text in any format. 
"Actions"--data evaluations or control structures--are delimited by "{{" and "}}"; all text outside actions is copied to the output unchanged. 
Actions may not span newlines, although comments can.

Once parsed, a template may be executed safely in parallel.

Here is a trivial example that prints "17 items are made of wool".

template 包 为生成文本输出 实现  数据驱动模板.

要生成HTML输出，查看 html/template, 和这个包有一样的接口但是 自动保证了应对某些攻击的HTML输出。

模板是通过应用它们的数据结构执行的。在模板中的注释是指该数据结构的元素(通常是struct的一个字段或 map 中的一个key) 以控制执行和要显示的派生值。
模版的执行 walks 结构体 和 设置游标, 通过一个句号  '.' 表示  和 调用"dot",以在结构执行进入的当前位置的值。

模版的输入值是任何格式的 UTF-8 编码文本.
"Actions"--数据  或控制结构 -- 用 "{{" 和 "}}" 分隔.  所有文字以外的行为将不变的被复制到输出。
操作可能无法跨越的新行，虽然注释可以。

一旦解析，模板可以安全地并行执行。

下面是一个简单的例子 打印 "17 items are made of wool".

```golang
type Inventory struct {
	Material string
	Count    uint
}
sweaters := Inventory{"wool", 17}
tmpl, err := template.New("test").Parse("{{.Count}} items are made of {{.Material}}")
if err != nil { panic(err) }
err = tmpl.Execute(os.Stdout, sweaters)
if err != nil { panic(err) }
```
More intricate examples appear below.
更复杂的例子在下面.


##Actions

Here is the list of actions. "Arguments" and "pipelines" are evaluations of data, defined in detail below.
这是行为的列表. "Arguments" 和  "pipelines" 是数据的评估,  在下文中详细定义。

```golang
{{/* a comment */}}
	A comment; discarded. May contain newlines.
	Comments do not nest and must start and end at the
	delimiters, as shown here.
	一个注释;丢弃. 可能包含换行符.注释不会嵌套 并且以分隔符开始和结束, 如这所示.
	

{{pipeline}}
	The default textual representation of the value of the pipeline
	is copied to the output.
	管道的值的默认文本表示 复制到 输出,


{{if pipeline}} T1 {{end}}
	If the value of the pipeline is empty, no output is generated;
	otherwise, T1 is executed.  The empty values are false, 0, any
	nil pointer or interface value, and any array, slice, map, or
	string of length zero.
	Dot is unaffected.
	如果pipeline的值是空的, 没有输出生成;
	否则执行  T1. 空值 是 false, 0 , 任何nil 指针或接口值 和任何array, slice, map 或长度为零的字符串.
	点不受影响。



{{if pipeline}} T1 {{else}} T0 {{end}}
	If the value of the pipeline is empty, T0 is executed;
	otherwise, T1 is executed.  Dot is unaffected.
	如果pipeline的值是空的,执行 T0 ;
	否则执行T1.点不受影响.



{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}
	To simplify the appearance of if-else chains, the else action
	of an if may include another if directly; the effect is exactly
	the same as writing
		{{if pipeline}} T1 {{else}}{{if pipeline}} T0 {{end}}{{end}}
	为了简化的if-else


{{range pipeline}} T1 {{end}}
	The value of the pipeline must be an array, slice, map, or channel.
	If the value of the pipeline has length zero, nothing is output;
	otherwise, dot is set to the successive elements of the array,
	slice, or map and T1 is executed. If the value is a map and the
	keys are of basic type with a defined order ("comparable"), the
	elements will be visited in sorted key order.

{{range pipeline}} T1 {{else}} T0 {{end}}
	The value of the pipeline must be an array, slice, map, or channel.
	If the value of the pipeline has length zero, dot is unaffected and
	T0 is executed; otherwise, dot is set to the successive elements
	of the array, slice, or map and T1 is executed.

{{template "name"}}
	The template with the specified name is executed with nil data.

{{template "name" pipeline}}
	The template with the specified name is executed with dot set
	to the value of the pipeline.

{{with pipeline}} T1 {{end}}
	If the value of the pipeline is empty, no output is generated;
	otherwise, dot is set to the value of the pipeline and T1 is
	executed.

{{with pipeline}} T1 {{else}} T0 {{end}}
	If the value of the pipeline is empty, dot is unaffected and T0
	is executed; otherwise, dot is set to the value of the pipeline
	and T1 is executed.


````













