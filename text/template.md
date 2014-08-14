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
  Fields也可以求变量值,包含链式: $x.Field1.Field2
    
    
    
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
    
- 数据的键名, 必须是一个map, 前面一个点,例如 .Key
  结果是map 根据 key 索引的 元素值.
  key 可以链式调用 并且 和 fields 结合成任意深度:  .Field1.Key1.Field2.Key2
  key必须是 数字字母标示符, 不像field 命名  他们不需要以大写字母字母开头.
  key也可以求变量值 ,包含在链式: $x.key1.key2
   
    
    
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
     
- 数据无参数方法名,前面一个点,例如  .Method
  

 
    
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
Arguments may evaluate to any type; if they are pointers the implementation automatically indirects to the base type when required. 
If an evaluation yields a function value, such as a function-valued field of a struct, the function is not invoked automatically, 
	but it can be used as a truth value for an if action and the like. 
To invoke it, use the call function, defined below.
Arguments 可以求任何类型的值; 如果他们是指针,当需要时 实现自动 indirects 基础类型.
一个函数值评价收益率,例如一个 struct  函数值 字段, 该函数不会自动调用. 但它可以作为真值如果行动等。
为了调用它 使用下列调用函数.


A pipeline is a possibly chained sequence of "commands". A command is a simple value (argument) or a function or method call, possibly with multiple arguments:
pipeline 是一个  可能的 "commands" 序列 链.  命令是一个简单的值(参数)或 一个函数 或方法 调用, 可能有多个参数:

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


##Pipelines

A pipeline may be "chained" by separating a sequence of commands with pipeline characters '|'. 
In a chained pipeline, the result of the each command is passed as the last argument of the following command. 
The output of the final command in the pipeline is the value of the pipeline.

The output of a command will be either one value or two values, the second of which has type error. 
If that second value is present and evaluates to non-nil, execution terminates and the error is returned to the caller of Execute.

pipeline 可能是  "chained"  通过pipeline 字符 '|' 分隔的命令序列
在一个链管道,  在每个命令的结果传递如下命令的最后一个参数。
管道里的最终命令输出是 管道的值.

一个命令的输出 将是 一个值或两个值, 第二个值是错误类型.
如果第二个值 现在和计算结果为非零 ,执行中断并且 返回调用 Execute的结果.


##Variables

A pipeline inside an action may initialize a variable to capture the result. The initialization has syntax
pipeline 一个action中可以初始化变量捕获结果.初始化有语法
```golang
$variable := pipeline
```
where $variable is the name of the variable. An action that declares a variable produces no output.
$variable 是 变量的名称. action声明一个变量 不输出.


If a "range" action initializes a variable, the variable is set to the successive elements of the iteration. Also, a "range" may declare two variables, separated by a comma:
如果一个 "range" 动作 初始化一个变量,该变量设置到 迭代 连续元素.  一个"range" 可能声明两个变量, 用变量分隔.
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


func HTMLEscape
```golang
func HTMLEscape(w io.Writer, b []byte)
```
HTMLEscape writes to w the escaped HTML equivalent of the plain text data b.
HTMLEscape 把转义的HTML纯文本b 写入w



func HTMLEscapeString
```golang
func HTMLEscapeString(s string) string
```
HTMLEscapeString returns the escaped HTML equivalent of the plain text data s.
HTMLEscapeString 返回 转义的HTML 纯文本数据s.



func HTMLEscaper
```golang
func HTMLEscaper(args ...interface{}) string
```
HTMLEscaper returns the escaped HTML equivalent of the textual representation of its arguments.
HTMLEscaper 返回 转义的 HTML  相当于 它的参数的文本表示.



func JSEscape
```golang
func JSEscape(w io.Writer, b []byte)
```
JSEscape writes to w the escaped JavaScript equivalent of the plain text data b.
JSEscape 把 转义的JavaScript 纯文本数据b 写入w.



func JSEscapeString
```golang
func JSEscapeString(s string) string
```
JSEscapeString returns the escaped JavaScript equivalent of the plain text data s.
JSEscapeString 返回 转义JavaScript 纯文本数据 s.



func JSEscaper
```golang
func JSEscaper(args ...interface{}) string
```
JSEscaper returns the escaped JavaScript equivalent of the textual representation of its arguments.
JSEscaper  返回转义JavaScript 相当于 它的参数的文本表示.



func URLQueryEscaper
```golang
func URLQueryEscaper(args ...interface{}) string
```
URLQueryEscaper returns the escaped value of the textual representation of its arguments in a form suitable for embedding in a URL query.
URLQueryEscaper 返回 为嵌入在URL中的查询,转义它的参数纯文本值成 合适的形式.


type FuncMap
```golang
type FuncMap map[string]interface{}
```
FuncMap is the type of the map defining the mapping from names to functions. 
Each function must have either a single return value, or two return values of which the second has type error. 
In that case, if the second (error) return value evaluates to non-nil during execution, execution terminates and Execute returns that error.
FuncMap 是定义在 map里的 从 命名到函数的map类型.
每个函数必须 要么有一个返回值, 要么两个返回值 其中第二个是错误类型,
在这种情况下,如果第二个 返回值  在执行期间 是非nil, 执行中断 并返回那个错误.



type Template
```golang
type Template struct {
        *parse.Tree
        // contains filtered or unexported fields
}
```
Template is the representation of a parsed template. The *parse.Tree field is exported only for use by html/template and should be treated as unexported by all other clients.
Template 表示一个解析的模版.*parse.Tree字段 只使用 html/template输出, 并且 其他所有客户端应该 视为阻止导出.


▾ Example
```golang
package main

import (
	"log"
	"os"
	"text/template"
)

func main() {
	// Define a template.
	const letter = `
Dear {{.Name}},
{{if .Attended}}
It was a pleasure to see you at the wedding.{{else}}
It is a shame you couldn't make it to the wedding.{{end}}
{{with .Gift}}Thank you for the lovely {{.}}.
{{end}}
Best wishes,
Josie
`

	// Prepare some data to insert into the template.
	type Recipient struct {
		Name, Gift string
		Attended   bool
	}
	var recipients = []Recipient{
		{"Aunt Mildred", "bone china tea set", true},
		{"Uncle John", "moleskin pants", false},
		{"Cousin Rodney", "", false},
	}

	// Create a new template and parse the letter into it.
	t := template.Must(template.New("letter").Parse(letter))

	// Execute the template for each recipient.
	for _, r := range recipients {
		err := t.Execute(os.Stdout, r)
		if err != nil {
			log.Println("executing template:", err)
		}
	}

}
```


▹ Example (Func)
This example demonstrates a custom function to process template text. It installs the strings.Title function and uses it to Make Title Text Look Good In Our Template's Output.
```golang
package main

import (
	"log"
	"os"
	"strings"
	"text/template"
)

func main() {
	// First we create a FuncMap with which to register the function.
	funcMap := template.FuncMap{
		// The name "title" is what the function will be called in the template text.
		"title": strings.Title,
	}

	// A simple template definition to test our function.
	// We print the input text several ways:
	// - the original
	// - title-cased
	// - title-cased and then printed with %q
	// - printed with %q and then title-cased.
	const templateText = `
Input: {{printf "%q" .}}
Output 0: {{title .}}
Output 1: {{title . | printf "%q"}}
Output 2: {{printf "%q" . | title}}
`

	// Create a template, add the function map, and parse the text.
	tmpl, err := template.New("titleTest").Funcs(funcMap).Parse(templateText)
	if err != nil {
		log.Fatalf("parsing: %s", err)
	}

	// Run the template to verify the output.
	err = tmpl.Execute(os.Stdout, "the go programming language")
	if err != nil {
		log.Fatalf("execution: %s", err)
	}

}
```



▹ Example (Glob)

Here we demonstrate loading a set of templates from a directory.

Code:

```golang
// Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
            // T0.tmpl is a plain template file that just invokes T1.
            {"T0.tmpl", `T0 invokes T1: ({{template "T1"}})`},
            // T1.tmpl defines a template, T1 that invokes T2.
            {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
            // T2.tmpl defines a template T2.
            {"T2.tmpl", `{{define "T2"}}This is T2{{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // T0.tmpl is the first name matched, so it becomes the starting template,
    // the value returned by ParseGlob.
    tmpl := template.Must(template.ParseGlob(pattern))

    err := tmpl.Execute(os.Stdout, nil)
    if err != nil {
            log.Fatalf("template execution: %s", err)
    }
```

Output:
```golang
T0 invokes T1: (T1 invokes T2: (This is T2))
```


▹ Example (Helpers)
This example demonstrates one way to share some templates and use them in different contexts. In this variant we add multiple driver templates by hand to an existing bundle of templates.

Code:
```golang
// Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
            // T1.tmpl defines a template, T1 that invokes T2.
            {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
            // T2.tmpl defines a template T2.
            {"T2.tmpl", `{{define "T2"}}This is T2{{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // Load the helpers.
    templates := template.Must(template.ParseGlob(pattern))
    // Add one driver template to the bunch; we do this with an explicit template definition.
    _, err := templates.Parse("{{define `driver1`}}Driver 1 calls T1: ({{template `T1`}})\n{{end}}")
    if err != nil {
            log.Fatal("parsing driver1: ", err)
    }
    // Add another driver template.
    _, err = templates.Parse("{{define `driver2`}}Driver 2 calls T2: ({{template `T2`}})\n{{end}}")
    if err != nil {
            log.Fatal("parsing driver2: ", err)
    }
    // We load all the templates before execution. This package does not require
    // that behavior but html/template's escaping does, so it's a good habit.
    err = templates.ExecuteTemplate(os.Stdout, "driver1", nil)
    if err != nil {
            log.Fatalf("driver1 execution: %s", err)
    }
    err = templates.ExecuteTemplate(os.Stdout, "driver2", nil)
    if err != nil {
            log.Fatalf("driver2 execution: %s", err)
    }
```    

Output:

```golang
Driver 1 calls T1: (T1 invokes T2: (This is T2))
Driver 2 calls T2: (This is T2)
```


▹ Example (Share)

This example demonstrates how to use one group of driver templates with distinct sets of helper templates.

Code:

```golang
// Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
            // T0.tmpl is a plain template file that just invokes T1.
            {"T0.tmpl", "T0 ({{.}} version) invokes T1: ({{template `T1`}})\n"},
            // T1.tmpl defines a template, T1 that invokes T2. Note T2 is not defined
            {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // Load the drivers.
    drivers := template.Must(template.ParseGlob(pattern))

    // We must define an implementation of the T2 template. First we clone
    // the drivers, then add a definition of T2 to the template name space.

    // 1. Clone the helper set to create a new name space from which to run them.
    first, err := drivers.Clone()
    if err != nil {
            log.Fatal("cloning helpers: ", err)
    }
    // 2. Define T2, version A, and parse it.
    _, err = first.Parse("{{define `T2`}}T2, version A{{end}}")
    if err != nil {
            log.Fatal("parsing T2: ", err)
    }

    // Now repeat the whole thing, using a different version of T2.
    // 1. Clone the drivers.
    second, err := drivers.Clone()
    if err != nil {
            log.Fatal("cloning drivers: ", err)
    }
    // 2. Define T2, version B, and parse it.
    _, err = second.Parse("{{define `T2`}}T2, version B{{end}}")
    if err != nil {
            log.Fatal("parsing T2: ", err)
    }

    // Execute the templates in the reverse order to verify the
    // first is unaffected by the second.
    err = second.ExecuteTemplate(os.Stdout, "T0.tmpl", "second")
    if err != nil {
            log.Fatalf("second execution: %s", err)
    }
    err = first.ExecuteTemplate(os.Stdout, "T0.tmpl", "first")
    if err != nil {
            log.Fatalf("first: execution: %s", err)
    }
```
    
Output:

```golang
T0 (second version) invokes T1: (T1 invokes T2: (T2, version B))
T0 (first version) invokes T1: (T1 invokes T2: (T2, version A))
```


func Must
```golang
func Must(t *Template, err error) *Template
```
Must is a helper that wraps a call to a function returning (*Template, error) and panics if the error is non-nil. It is intended for use in variable initializations such as
Must 是一个助手 封装调用 一个函数 返回 (*Template, error)  并且 如果 error 不是 nil 会引发panic.它用于变量初始化

```golang
var t = template.Must(template.New("name").Parse("text"))
```


func New
```golang
func New(name string) *Template
```
New allocates a new template with the given name.
New 用给定的name 分配一个新的模版



func ParseFiles
```golang
func ParseFiles(filenames ...string) (*Template, error)
```
ParseFiles creates a new Template and parses the template definitions from the named files. 
The returned template's name will have the (base) name and (parsed) contents of the first file. 
There must be at least one file. If an error occurs, parsing stops and the returned *Template is nil.

ParseFiles 从指定的文件 创建一个新的 Template并解析模版定义
返回的模版名 将有(base)名称和(parsed) 第一个文件内容
这必须至少一个文件.如果 遇到错误,解析停止 并且 返回的 *Template 是nil.



func ParseGlob
```golang
func ParseGlob(pattern string) (*Template, error)
```
ParseGlob creates a new Template and parses the template definitions from the files identified by the pattern, which must match at least one file. 
The returned template will have the (base) name and (parsed) contents of the first file matched by the pattern. 
ParseGlob is equivalent to calling ParseFiles with the list of files matched by the pattern.

ParseGlob 从 pattern 文件标示符 创建一个新的 Template 并 解析模版定义,其中 必须至少一个文件.
通过pattern返回的模版名 将有(base)名称和(parsed) 第一个文件内容
ParseGlob相当于  调用 ParseFiles 与匹配的pattern 文件列表



func (*Template) AddParseTree
```golang
func (t *Template) AddParseTree(name string, tree *parse.Tree) (*Template, error)
```
AddParseTree creates a new template with the name and parse tree and associates it with t.
AddParseTree 根据name创建一个新的模版 并解析 tree 和对应的t.



func (*Template) Clone
```golang
func (t *Template) Clone() (*Template, error)
```
Clone returns a duplicate of the template, including all associated templates. 
The actual representation is not copied, but the name space of associated templates is, 
	so further calls to Parse in the copy will add templates to the copy but not to the original. 
Clone can be used to prepare common templates and use them with variant definitions for other templates by adding the variants after the clone is made.
Clone 返回复制的模版 , 包含所有关联的 模版.
实际的表示是不可复制的，但是相关的模板的命名空间是, 所以未来在复制的里 调用Parse将添加模版到复制的,而不是原来的.
Clone 可以用来准备 常见的模版 并 通过增加变种克隆作出后使用它们与其他模板变量的定义。



func (*Template) Delims
```golang
func (t *Template) Delims(left, right string) *Template
```
Delims sets the action delimiters to the specified strings, to be used in subsequent calls to Parse, ParseFiles, or ParseGlob. 
Nested template definitions will inherit the settings. An empty delimiter stands for the corresponding default: {{ or }}. 
The return value is the template, so calls can be chained.
Delims 用指定的字符串 设置 action 分隔符, 要用在子请求  调用  Parse, ParseFiles, 或 ParseGlob.
嵌套模板定义将继承的设置.任何分隔符标准 对应的默认 :  {{ or }}. 
返回值是 template , 所以可以链式调用



func (*Template) Execute
```golang
func (t *Template) Execute(wr io.Writer, data interface{}) (err error)
```
Execute applies a parsed template to the specified data object, and writes the output to wr. 
If an error occurs executing the template or writing its output, execution stops, but partial results may already have been written to the output writer. 
A template may be executed safely in parallel.
Execute 应用一个解析模版到值定的data对象, 并写 输出到 wr.
如果执行模版或写它的输出 遇到错误, 执行停止,但  部分结果可能已经写道 输出 writer.
模板可以并行安全地执行。



func (*Template) ExecuteTemplate
```golang
func (t *Template) ExecuteTemplate(wr io.Writer, name string, data interface{}) error
```
ExecuteTemplate applies the template associated with t that has the given name to the specified data object and writes the output to wr. 
If an error occurs executing the template or writing its output, execution stops, but partial results may already have been written to the output writer. 
A template may be executed safely in parallel.
ExecuteTemplate 应用 模版关联的t  给定的name到值定的data 对象 并写输出到wr.
如果执行模版或写它的输出 遇到错误, 执行停止,但  部分结果可能已经写道 输出 writer.
模板可以并行安全地执行。



func (*Template) Funcs
```golang
func (t *Template) Funcs(funcMap FuncMap) *Template
```
Funcs adds the elements of the argument map to the template's function map. It panics if a value in the map is not a function with appropriate return type. 
However, it is legal to overwrite elements of the map. The return value is the template, so calls can be chained.
Funcs 添加参数map 元素到模版的 map函数. 如果值在 map里 不是一个合适的返回类型函数, 它引发panic.
然而 它 规定 map 元素.返回值是模版 所以可以链式调用



func (*Template) Lookup
```golang
func (t *Template) Lookup(name string) *Template
```
Lookup returns the template with the given name that is associated with t, or nil if there is no such template.
Lookup 用给定的name 返回 关联的模版t , 如果没有这个模版返回nil




func (*Template) Name
```golang
func (t *Template) Name() string
```
Name returns the name of the template.
Name 返回模版名.



func (*Template) New
```golang
func (t *Template) New(name string) *Template
```
New allocates a new template associated with the given one and with the same delimiters. 
The association, which is transitive, allows one template to invoke another with a {{template}} action.
New 用给定的1和想用的分隔符 分配一个新的模版.
允许一个模版用 {{template}} 调用其他模版




func (*Template) Parse
```golang
func (t *Template) Parse(text string) (*Template, error)
```
Parse parses a string into a template. Nested template definitions will be associated with the top-level template t. 
Parse may be called multiple times to parse definitions of templates to associate with t. 
It is an error if a resulting template is non-empty (contains content other than template definitions) and would replace a non-empty template with the same name. 
(In multiple calls to Parse with the same receiver template, only one call can contain text other than space, comments, and template definitions.)
Parse 解析一个字符串 到模版.  嵌套模板定义将与顶层模板t关联。
Parse 可以多次调用来解析 和t有关的模版.
如果返回模版非空(包含其他模版定义内容)  并 用相同的名称替换一个非空模版, 是错误的.
(用相同的接收器多次调用Parse, 只有 一次调用可以包含其他 空格 注释 和模版定义文本)
 

func (*Template) ParseFiles
```golang
func (t *Template) ParseFiles(filenames ...string) (*Template, error)
```
ParseFiles parses the named files and associates the resulting templates with t. 
If an error occurs, parsing stops and the returned template is nil; otherwise it is t. There must be at least one file.
ParseFiles 解析 命名文件 并用t 关联结果模版.
如果遇到错误 ,解析停止并返回nil 模版.否则 它是t . 必须至少有一个文件.




func (*Template) ParseGlob
```golang
func (t *Template) ParseGlob(pattern string) (*Template, error)
```
ParseGlob parses the template definitions in the files identified by the pattern and associates the resulting templates with t. 
The pattern is processed by filepath.Glob and must match at least one file. 
ParseGlob is equivalent to calling t.ParseFiles with the list of files matched by the pattern.
ParseGlob 解析模版 通过pattern 定义在文件里的标示符 和t 关联的结果.
pattern 可能通过 filepath.Glob 并必须匹配多少一个文件.
ParseGlob 相当于 通过pattern 匹配文件列表 调用 t.ParseFiles.



func (*Template) Templates
```golang
func (t *Template) Templates() []*Template
```
Templates returns a slice of the templates associated with t, including t itself.
Templates 返回 和t关联的模版slice, 包含t 本身.





























































