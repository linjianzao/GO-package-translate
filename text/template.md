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
	为了简化的if-else, else 方法 可能 包含其他if;
	和写成{{if pipeline}} T1 {{else}}{{if pipeline}} T0 {{end}}{{end}}的效果是完全一样的 



{{range pipeline}} T1 {{end}}
	The value of the pipeline must be an array, slice, map, or channel.
	If the value of the pipeline has length zero, nothing is output;
	otherwise, dot is set to the successive elements of the array,
	slice, or map and T1 is executed. If the value is a map and the
	keys are of basic type with a defined order ("comparable"), the
	elements will be visited in sorted key order.
	pipeline的值必须是array, slice, map, 或 channel.
	如果pipeline的值长度是零,不会输入任何东西;
	否则, 设置连续的array,slice, 或 map元素 点 和执行T1.如果值是map 并且 key的基础类型按定义的顺便("comparable"), 将会按顺序访问元素.
	
	
	
{{range pipeline}} T1 {{else}} T0 {{end}}
	The value of the pipeline must be an array, slice, map, or channel.
	If the value of the pipeline has length zero, dot is unaffected and
	T0 is executed; otherwise, dot is set to the successive elements
	of the array, slice, or map and T1 is executed.
	pipeline的值必须是array, slice, map, 或 channel.
	如果pipeline的值长度是零,点不会受影响并且 执行T0; 否则 设置连续的array,slice, 或 map元素 点,并执行T1
	
	

{{template "name"}}
	The template with the specified name is executed with nil data.
	不带数据的执行指定名称的模版
	

{{template "name" pipeline}}
	The template with the specified name is executed with dot set
	to the value of the pipeline.
	带pipeline 值 执行指定名称的模版


{{with pipeline}} T1 {{end}}
	If the value of the pipeline is empty, no output is generated;
	otherwise, dot is set to the value of the pipeline and T1 is
	executed.
	如果pipeline的值是空的, 不会生成输出;否则点 设置到pipeline的值 并执行  T1


{{with pipeline}} T1 {{else}} T0 {{end}}
	If the value of the pipeline is empty, dot is unaffected and T0
	is executed; otherwise, dot is set to the value of the pipeline
	and T1 is executed.
	如果pipeline的值是空的,点不会受影响 并执行T0; 否则 点 设置成pipeline的值并执行 T1

````


#Arguments

An argument is a simple value, denoted by one of the following.
参数是一个简单的值, 表示通过执行下列操作之一。
```golang
- A boolean, string, character, integer, floating-point, imaginary
  or complex constant in Go syntax. These behave like Go's untyped
  constants, although raw strings may not span newlines.

- GO语法里的boolean, string, character, integer, floating-point, imaginary
  or complex. 他们的行为像GO的 无类型的常量,虽然原始字符串可能无法跨越的新行。
  
  
- The keyword nil, representing an untyped Go nil.

- 关键字 nil 表示 无类型 GO nil


- The character '.' (period):
	.
  The result is the value of dot.

- 字符'.' 
  .
  结果是点的值。
  
  
- A variable name, which is a (possibly empty) alphanumeric string
  preceded by a dollar sign, such as
	$piOver2
  or
	$
  The result is the value of the variable.
  Variables are described below.
  
- 变量名是 一个 数字字母字符串 之前跟着美元符号例如$piOver2 或 $.
  结果是变量的值. 变量如下所述。
	
  
  
- The name of a field of the data, which must be a struct, preceded
  by a period, such as
	.Field
  The result is the value of the field. Field invocations may be
  chained:
    .Field1.Field2
  Fields can also be evaluated on variables, including chaining:
    $x.Field1.Field2
    
- 数据的字段名 必须是一个 struct 之前跟着点, 例如 .Field
  结果是字段的值. Field 可以链式调用: .Field1.Field2
  Fields也可以当成变量,包含链式: $x.Field1.Field2
    
    
    
- The name of a key of the data, which must be a map, preceded
  by a period, such as
	.Key
  The result is the map element value indexed by the key.
  Key invocations may be chained and combined with fields to any
  depth:
    .Field1.Key1.Field2.Key2
  Although the key must be an alphanumeric identifier, unlike with
  field names they do not need to start with an upper case letter.
  Keys can also be evaluated on variables, including chaining:
    $x.key1.key2
    
-   
    
    
- The name of a niladic method of the data, preceded by a period,
  such as
	.Method
  The result is the value of invoking the method with dot as the
  receiver, dot.Method(). Such a method must have one return value (of
  any type) or two return values, the second of which is an error.
  If it has two and the returned error is non-nil, execution terminates
  and an error is returned to the caller as the value of Execute.
  Method invocations may be chained and combined with fields and keys
  to any depth:
    .Field1.Key1.Method1.Field2.Key2.Method2
  Methods can also be evaluated on variables, including chaining:
    $x.Method1.Field
    

 
    
- The name of a niladic function, such as
	fun
  The result is the value of invoking the function, fun(). The return
  types and values behave as in methods. Functions and function
  names are described below.
- A parenthesized instance of one the above, for grouping. The result
  may be accessed by a field or map key invocation.
	print (.F1 arg1) (.F2 arg2)
	(.StructValuedMethod "arg").Field
	
	
```


Arguments may evaluate to any type; if they are pointers the implementation automatically indirects to the base type when required. If an evaluation yields a function value, such as a function-valued field of a struct, the function is not invoked automatically, but it can be used as a truth value for an if action and the like. To invoke it, use the call function, defined below.

A pipeline is a possibly chained sequence of "commands". A command is a simple value (argument) or a function or method call, possibly with multiple arguments:



```golang
Argument
	The result is the value of evaluating the argument.
.Method [Argument...]
	The method can be alone or the last element of a chain but,
	unlike methods in the middle of a chain, it can take arguments.
	The result is the value of calling the method with the
	arguments:
		dot.Method(Argument1, etc.)
functionName [Argument...]
	The result is the value of calling the function associated
	with the name:
		function(Argument1, etc.)
	Functions and function names are described below.

```


Pipelines

A pipeline may be "chained" by separating a sequence of commands with pipeline characters '|'. In a chained pipeline, the result of the each command is passed as the last argument of the following command. The output of the final command in the pipeline is the value of the pipeline.

The output of a command will be either one value or two values, the second of which has type error. If that second value is present and evaluates to non-nil, execution terminates and the error is returned to the caller of Execute.



Variables

A pipeline inside an action may initialize a variable to capture the result. The initialization has syntax
```golang
$variable := pipeline
```
where $variable is the name of the variable. An action that declares a variable produces no output.

If a "range" action initializes a variable, the variable is set to the successive elements of the iteration. Also, a "range" may declare two variables, separated by a comma:

```golang
range $index, $element := pipeline
```

in which case $index and $element are set to the successive values of the array/slice index or map key and element, respectively. Note that if there is only one variable, it is assigned the element; this is opposite to the convention in Go range clauses.

A variable's scope extends to the "end" action of the control structure ("if", "with", or "range") in which it is declared, or to the end of the template if there is no such control structure. A template invocation does not inherit variables from the point of its invocation.

When execution begins, $ is set to the data argument passed to Execute, that is, to the starting value of dot.




Examples

Here are some example one-line templates demonstrating pipelines and variables. All produce the quoted word "output":
```golang
{{"\"output\""}}
	A string constant.
{{`"output"`}}
	A raw string constant.
{{printf "%q" "output"}}
	A function call.
{{"output" | printf "%q"}}
	A function call whose final argument comes from the previous
	command.
{{printf "%q" (print "out" "put")}}
	A parenthesized argument.
{{"put" | printf "%s%s" "out" | printf "%q"}}
	A more elaborate call.
{{"output" | printf "%s" | printf "%q"}}
	A longer chain.
{{with "output"}}{{printf "%q" .}}{{end}}
	A with action using dot.
{{with $x := "output" | printf "%q"}}{{$x}}{{end}}
	A with action that creates and uses a variable.
{{with $x := "output"}}{{printf "%q" $x}}{{end}}
	A with action that uses the variable in another action.
{{with $x := "output"}}{{$x | printf "%q"}}{{end}}
	The same, but pipelined.
```


Functions

During execution functions are found in two function maps: first in the template, then in the global function map. By default, no functions are defined in the template but the Funcs method can be used to add them.

Predefined global functions are named as follows.


```golang
and
	Returns the boolean AND of its arguments by returning the
	first empty argument or the last argument, that is,
	"and x y" behaves as "if x then y else x". All the
	arguments are evaluated.
call
	Returns the result of calling the first argument, which
	must be a function, with the remaining arguments as parameters.
	Thus "call .X.Y 1 2" is, in Go notation, dot.X.Y(1, 2) where
	Y is a func-valued field, map entry, or the like.
	The first argument must be the result of an evaluation
	that yields a value of function type (as distinct from
	a predefined function such as print). The function must
	return either one or two result values, the second of which
	is of type error. If the arguments don't match the function
	or the returned error value is non-nil, execution stops.
html
	Returns the escaped HTML equivalent of the textual
	representation of its arguments.
index
	Returns the result of indexing its first argument by the
	following arguments. Thus "index x 1 2 3" is, in Go syntax,
	x[1][2][3]. Each indexed item must be a map, slice, or array.
js
	Returns the escaped JavaScript equivalent of the textual
	representation of its arguments.
len
	Returns the integer length of its argument.
not
	Returns the boolean negation of its single argument.
or
	Returns the boolean OR of its arguments by returning the
	first non-empty argument or the last argument, that is,
	"or x y" behaves as "if x then x else y". All the
	arguments are evaluated.
print
	An alias for fmt.Sprint
printf
	An alias for fmt.Sprintf
println
	An alias for fmt.Sprintln
urlquery
	Returns the escaped value of the textual representation of
	its arguments in a form suitable for embedding in a URL query.

```


The boolean functions take any zero value to be false and a non-zero value to be true.

There is also a set of binary comparison operators defined as functions:



```golang
eq
	Returns the boolean truth of arg1 == arg2
ne
	Returns the boolean truth of arg1 != arg2
lt
	Returns the boolean truth of arg1 < arg2
le
	Returns the boolean truth of arg1 <= arg2
gt
	Returns the boolean truth of arg1 > arg2
ge
	Returns the boolean truth of arg1 >= arg2
```

For simpler multi-way equality tests, eq (only) accepts two or more arguments and compares the second and subsequent to the first, returning in effect

```golang
arg1==arg2 || arg1==arg3 || arg1==arg4 ...
```


(Unlike with || in Go, however, eq is a function call and all the arguments will be evaluated.)

The comparison functions work on basic types only (or named basic types, such as "type Celsius float32"). They implement the Go rules for comparison of values, except that size and exact type are ignored, so any integer value may be compared with any other integer value, any unsigned integer value may be compared with any other unsigned integer value, and so on. However, as usual, one may not compare an int with a float32 and so on.


##Associated templates

Each template is named by a string specified when it is created. Also, each template is associated with zero or more other templates that it may invoke by name; such associations are transitive and form a name space of templates.

A template may use a template invocation to instantiate another associated template; see the explanation of the "template" action above. The name must be that of a template associated with the template that contains the invocation.



##Nested template definitions

When parsing a template, another template may be defined and associated with the template being parsed. Template definitions must appear at the top level of the template, much like global variables in a Go program.

The syntax of such definitions is to surround each template declaration with a "define" and "end" action.

The define action names the template being created by providing a string constant. Here is a simple example:

```golang
`{{define "T1"}}ONE{{end}}
{{define "T2"}}TWO{{end}}
{{define "T3"}}{{template "T1"}} {{template "T2"}}{{end}}
{{template "T3"}}`
```

This defines two templates, T1 and T2, and a third T3 that invokes the other two when it is executed. Finally it invokes T3. If executed this template will produce the text

```golang
ONE TWO
```


By construction, a template may reside in only one association. If it's necessary to have a template addressable from multiple associations, the template definition must be parsed multiple times to create distinct *Template values, or must be copied with the Clone or AddParseTree method.

Parse may be called multiple times to assemble the various associated templates; see the ParseFiles and ParseGlob functions and methods for simple ways to parse related templates stored in files.

A template may be executed directly or through ExecuteTemplate, which executes an associated template identified by name. To invoke our example above, we might write,


```golang
err := tmpl.Execute(os.Stdout, "no data needed")
if err != nil {
	log.Fatalf("execution failed: %s", err)
}
```

or to invoke a particular template explicitly by name,

```golang
err := tmpl.ExecuteTemplate(os.Stdout, "T2", "no data needed")
if err != nil {
	log.Fatalf("execution failed: %s", err)
}
```

































