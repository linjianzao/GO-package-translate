#Package template            

#Overview ▾            

Package template (html/template) implements data-driven templates for generating HTML output safe against code injection.             
It provides the same interface as package text/template and should be used instead of text/template whenever the output is HTML.            

The documentation here focuses on the security features of the package.             
For information about how to program the templates themselves, see the documentation for text/template.            
template包 实现了数据驱动模版  生成输出HTML 安全 拒绝代码注入            
它在text/template提供同样的接口, 当输出HTML的时候  应该用来代替text/template             
这里的文档侧重于封装的安全功能。            
更多怎么编程 模版本身的有关信息 查看 text/template 文档            
            
##Introduction            
            
This package wraps package text/template so you can share its template API to parse and execute HTML templates safely.            
这个包含text/template包 所以你可以共享它的模版API 解析和运行HTML模版            
```golang
tmpl, err := template.New("name").Parse(...)
// Error checking elided
err = tmpl.Execute(out, data)
```
If successful, tmpl will now be injection-safe. Otherwise, err is an error defined in the docs for ErrorCode.            
            
HTML templates treat data values as plain text which should be encoded so they can be safely embedded in an HTML document.             
The escaping is contextual, so actions can appear within JavaScript, CSS, and URI contexts.            
            
The security model used by this package assumes that template authors are trusted, while Execute's data parameter is not. More details are provided below.            
如果成功 ,tmpl 现在将是 注入安全的.否则 在文档里会定义一个错误 ErrorCode            
HTML 模版 把数据值 当成 可以被编码的 纯文本 , 所以他们可以安全的嵌入一个HTML文档            
转义是上下文,所以转义可以出现在 JavaScript, CSS, 和 URI 里.            
使用这个包的安全模式 假定模版作者是可信的 , Execute 的数据的参数是不. 更多详细信息在下面.            
            
Example                        
```golang            
import "text/template"
...
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")
```
produces            
```golang
Hello, <script>alert('you have been pwned')</script>!
```
but the contextual autoescaping in html/template            
但是 上下文 自动 转义            
```golang
import "html/template"
...
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")
```
produces safe, escaped HTML output            
产出安全, 转义html输出            
```golang
Hello, &lt;script&gt;alert(&#39;you have been pwned&#39;)&lt;/script&gt;!
```
            
##Contexts            
            
This package understands HTML, CSS, JavaScript, and URIs. It adds sanitizing functions to each simple action pipeline, so given the excerpt            
这个包懂得 HTML, CSS, JavaScript, 和 URIs. 它给每一个简单的动作管道增加消毒函数.所以给出的片段            
```golang
<a href="/search?q={{.}}">{{.}}</a>
```
At parse time each {{.}} is overwritten to add escaping functions as necessary. In this case it becomes            
在解析时 需要的话 每个 {{.}} 覆盖 增加的转义函数.在这个例子 它变成            
```golang
<a href="/search?q={{. | urlquery}}">{{. | html}}</a>
```

##Errors            
            
See the documentation of ErrorCode for details.            
查看ErrorCode文档的详细信息            

##A fuller picture            
            
The rest of this package comment may be skipped on first reading;             
it includes details necessary to understand escaping contexts and error messages.             
Most users will not need to understand these details.            
这个包注释的其余部分可能 第一次读取 会被 跳过            
它包括必要的细节，了解转码上下文和错误消息。            
大多数用户并不需要了解这些细节。            
            
##Contexts            

Assuming {{.}} is `O'Reilly: How are <i>you</i>?`, the table below shows how {{.}} appears when used in the context to the left.            
假设{{.}} 是 `O'Reilly: How are <i>you</i>?`,下表会展示 当使用在在上下文中的左边  {{.}} 会怎么出现            
```golang
Context                          {{.}} After
{{.}}                            O'Reilly: How are &lt;i&gt;you&lt;/i&gt;?
<a title='{{.}}'>                O&#39;Reilly: How are you?
<a href="/{{.}}">                O&#39;Reilly: How are %3ci%3eyou%3c/i%3e?
<a href="?q={{.}}">              O&#39;Reilly%3a%20How%20are%3ci%3e...%3f
<a onx='f("{{.}}")'>             O\x27Reilly: How are \x3ci\x3eyou...?
<a onx='f({{.}})'>               "O\x27Reilly: How are \x3ci\x3eyou...?"
<a onx='pattern = /{{.}}/;'>     O\x27Reilly: How are \x3ci\x3eyou...\x3f
```

If used in an unsafe context, then the value might be filtered out:            
如果在不安全的环境中使用，则该值可能会被过滤掉：            

```golang
Context                          {{.}} After
<a href="{{.}}">                 #ZgotmplZ
```
since "O'Reilly:" is not an allowed protocol like "http:".            
If {{.}} is the innocuous word, `left`, then it can appear more widely,            
当"O'Reilly:" 没有允许的协议  类似"http:".            
如果{{.}}是无害的词`left` ,它可以出现更广泛，            
```golang
Context                              {{.}} After
{{.}}                                left
<a title='{{.}}'>                    left
<a href='{{.}}'>                     left
<a href='/{{.}}'>                    left
<a href='?dir={{.}}'>                left
<a style="border-{{.}}: 4px">        left
<a style="align: {{.}}">             left
<a style="background: '{{.}}'>       left
<a style="background: url('{{.}}')>  left
<style>p.{{.}} {color:red}</style>   left
```
Non-string values can be used in JavaScript contexts. If {{.}} is            
非字符串值可以用在JS里,如果 {{.}} 是            
```golang
struct{A,B string}{ "foo", "bar" }
```
            
in the escaped template            

```golang
<script>var pair = {{.}};</script>
```

then the template output is            
然后模版输出            

```golang
<script>var pair = {"A": "foo", "B": "bar"};</script>
```

See package json to understand how non-string content is marshalled for embedding in JavaScript contexts.            
查看 json包 了解 非字符串内容怎么编码 嵌入JS            


##Typed Strings

By default, this package assumes that all pipelines produce a plain text string.             
It adds escaping pipeline stages necessary to correctly and safely embed that plain text string in the appropriate context.            
            
When a data value is not plain text, you can make sure it is not over-escaped by marking it with its type.            
            
Types HTML, JS, URL, and others from content.go can carry safe content that is exempted from escaping.            
            
The template            
            
默认情况下，这个包假设所有的管道生产 纯文本字符串.它增加了转码管道必要的流水阶段, 在适当的范围内,正确和安全地嵌入纯文本字符串            
当一个数据值不是纯文本.你可以它没有过度转码它本身类型的标签            
HTML, JS, URL,和其他 content.go 可以带的安全内容 类型 豁免 过滤.            
模版            

```golang
Hello, {{.}}!
```
            
can be invoked with            
可以被调用                        
```golang
tmpl.Execute(out, HTML(`<b>World</b>`))
```
to produce            
产生            
```golang
Hello, <b>World</b>!
```
instead of the            
替代            
```golang
Hello, &lt;b&gt;World&lt;b&gt;!
```
that would have been produced if {{.}} was a regular string.            
如果{{.}} 是一个正则字符串将会产生那样的.            
            
            
##Security Model 安全模式            
                        
http://js-quasis-libraries-and-repl.googlecode.com/svn/trunk/safetemplate.html#problem_definition defines "safe" as used by this package.            
            
This package assumes that template authors are trusted, that Execute's data parameter is not, and seeks to preserve the properties below in the face of untrusted data:            
            
Structure Preservation Property: "... when a template author writes an HTML tag in a safe templating language,             
	the browser will interpret the corresponding portion of the output as a tag regardless of the values of untrusted data,             
	and similarly for other structures such as attribute boundaries and JS and CSS string boundaries."            
            
Code Effect Property: "... only code specified by the template author should run as a result of injecting the template output into a page and all code specified by the template author should run as a result of the same."            
            
Least Surprise Property: "A developer (or code reviewer) familiar with HTML, CSS, and JavaScript,             
	who knows that contextual autoescaping happens should be able to look at a {{.}} and correctly infer what sanitization happens."            
            
            
http://js-quasis-libraries-and-repl.googlecode.com/svn/trunk/safetemplate.html#problem_definition 定义了 这个包使用的 "安全"            
这个包假设模版作者都是可信的.该执行的数据参数不是，并力求在不受信任的数据面前保留以下属性：            
结构体保留属性:"... 当一个模版作者写入 HTML标签 到一个安全的模版语言,浏览器会解释作为标记输出的相应部分而不管不可信数据的值的，同样为其他结构，如属性的边界和JS和CSS字符串界限"            
代码影响属性:".. 仅由模板作者指定的代码应为注入模板输出到一个页面，通过模板作者指定的所有代码应该相同的结果运行的运行结果"            
至少惊异的属性:"一个开发者(或代码审查者)熟悉HTML, CSS, 和 JavaScript, 知道文本自动转码发生 应该 可以查看{{.}} 并 确定注入 怎么清除 "            


##func HTMLEscape            
```golang
func HTMLEscape(w io.Writer, b []byte)
```
HTMLEscape writes to w the escaped HTML equivalent of the plain text data b.            
写入转码的HTML到w 等价于写入纯文本数据b到w            
            
            
##func HTMLEscapeString            
```golang
func HTMLEscapeString(s string) string
```
HTMLEscapeString returns the escaped HTML equivalent of the plain text data s.            
返回转码的HTML 等价于纯文本数据s            


##func HTMLEscaper            
```golang
func HTMLEscaper(args ...interface{}) string
```
HTMLEscaper returns the escaped HTML equivalent of the textual representation of its arguments.            
返回转码 HTML 等价于 原文表示的参数            
            
##func JSEscape            
```golang
func JSEscape(w io.Writer, b []byte)
```
JSEscape writes to w the escaped JavaScript equivalent of the plain text data b.            
写入转码的JS到w  等价于写入纯文本数据b到w            
            
            
##func JSEscapeString            
```golang
func JSEscapeString(s string) string
```
JSEscapeString returns the escaped JavaScript equivalent of the plain text data s.            
返回转码的JS 等价于 纯文本数据s            

            
##func JSEscaper            
```golang
func JSEscaper(args ...interface{}) string
```
JSEscaper returns the escaped JavaScript equivalent of the textual representation of its arguments.            
返回转码的JS 等价于原文表示的参数            


##func URLQueryEscaper            
```golang
func URLQueryEscaper(args ...interface{}) string
```
URLQueryEscaper returns the escaped value of the textual representation of its arguments in a form suitable for embedding in a URL query.            
返回 在一个URL查询中嵌入的 一个 标单提交的原文表示的参数的 转码 的值            
            
##type CSS            
```golang
type CSS string
```
CSS encapsulates known safe content that matches any of:            
CSS的封装 匹配任何的安全内容：            
```golang
1. The CSS3 stylesheet production, such as `p { color: purple }`.
2. The CSS3 rule production, such as `a[href=~"https:"].foo#bar`.
3. CSS3 declaration productions, such as `color: red; margin: 2px`.
4. The CSS3 value production, such as `rgba(0, 0, 255, 127)`.
```
See http://www.w3.org/TR/css3-syntax/#parsing and https://web.archive.org/web/20090211114933/http://w3.org/TR/css3-syntax#style            
            
##type Error            
```golang
type Error struct {
        // ErrorCode describes the kind of error.描述了 各种错误
        ErrorCode ErrorCode
        
        // Name is the name of the template in which the error was encountered.遇到错误的模版的名称
        Name string
        
        // Line is the line number of the error in the template source or 0. 错误在源模版中的行数 或者0
        Line int
        
        // Description is a human-readable description of the problem. 描述 人类可读的问题
        Description string
}
```
Error describes a problem encountered during template Escaping.            
描述模版在转义过程遇到的问题            
            
##func (*Error) Error            
```golang
func (e *Error) Error() string
```
            
##type ErrorCode            
```golang
type ErrorCode int
```
ErrorCode is a code for a kind of error.            
一个号码表示一种错误            
```golang
const (
        // OK indicates the lack of an error. 表示缺乏一个错误。
        OK ErrorCode = iota

        // ErrAmbigContext: "... appears in an ambiguous URL context" 出现在一个模棱两可的URL上下文
        // Example:
        //   <a href="
        //      {{if .C}}
        //        /path/
        //      {{else}}
        //        /search?q=
        //      {{end}}
        //      {{.X}}
          //   ">
        // Discussion:
        //   {{.X}} is in an ambiguous URL context since, depending on {{.C}},
        //  it may be either a URL suffix or a query parameter.
        //   Moving {{.X}} into the condition removes the ambiguity:
        //   <a href="{{if .C}}/path/{{.X}}{{else}}/search?q={{.X}}">
        
        ErrAmbigContext

        // ErrBadHTML: "expected space, attr name, or end of tag, but got ...",
        //   "... in unquoted attr", "... in attribute name"
        // Example:
        //   <a href = /search?q=foo>
        //   <href=foo>
        //   <form na<e=...>
        //   <option selected<
        // Discussion:
        //   This is often due to a typo in an HTML element, but some runes
        //   are banned in tag names, attribute names, and unquoted attribute
        //   values because they can tickle parser ambiguities.
        //   Quoting all attributes is the best policy.
        ErrBadHTML

        // ErrBranchEnd: "{{if}} branches end in different contexts"
        // Example:
        //   {{if .C}}<a href="{{end}}{{.X}}
        // Discussion:
        //   Package html/template statically examines each path through an
        //   {{if}}, {{range}}, or {{with}} to escape any following pipelines.
        //   The example is ambiguous since {{.X}} might be an HTML text node,
        //   or a URL prefix in an HTML attribute. The context of {{.X}} is
        //   used to figure out how to escape it, but that context depends on
        //   the run-time value of {{.C}} which is not statically known.
        //
        //   The problem is usually something like missing quotes or angle
        //   brackets, or can be avoided by refactoring to put the two contexts
        //   into different branches of an if, range or with. If the problem
        //   is in a {{range}} over a collection that should never be empty,
        //   adding a dummy {{else}} can help.
        ErrBranchEnd

        // ErrEndContext: "... ends in a non-text context: ..."
        // Examples:
        //   <div
        //   <div title="no close quote>
        //   <script>f()
        // Discussion:
        //   Executed templates should produce a DocumentFragment of HTML.
        //   Templates that end without closing tags will trigger this error.
        //   Templates that should not be used in an HTML context or that
        //   produce incomplete Fragments should not be executed directly.
        //
        //   {{define "main"}} <script>{{template "helper"}}</script> {{end}}
        //   {{define "helper"}} document.write(' <div title=" ') {{end}}
        //
        //   "helper" does not produce a valid document fragment, so should
        //   not be Executed directly.
        ErrEndContext

        // ErrNoSuchTemplate: "no such template ..."
        // Examples:
        //   {{define "main"}}<div {{template "attrs"}}>{{end}}
        //   {{define "attrs"}}href="{{.URL}}"{{end}}
        // Discussion:
        //   Package html/template looks through template calls to compute the
        //   context.
        //   Here the {{.URL}} in "attrs" must be treated as a URL when called
        //   from "main", but you will get this error if "attrs" is not defined
        //   when "main" is parsed.
        ErrNoSuchTemplate

        // ErrOutputContext: "cannot compute output context for template ..."
        // Examples:
        //   {{define "t"}}{{if .T}}{{template "t" .T}}{{end}}{{.H}}",{{end}}
        // Discussion:
        //   A recursive template does not end in the same context in which it
        //   starts, and a reliable output context cannot be computed.
        //   Look for typos in the named template.
        //   If the template should not be called in the named start context,
        //   look for calls to that template in unexpected contexts.
        //   Maybe refactor recursive templates to not be recursive.
        ErrOutputContext

        // ErrPartialCharset: "unfinished JS regexp charset in ..."
        // Example:
        //     <script>var pattern = /foo[{{.Chars}}]/</script>
        // Discussion:
        //   Package html/template does not support interpolation into regular
        //   expression literal character sets.
        ErrPartialCharset

        // ErrPartialEscape: "unfinished escape sequence in ..."
        // Example:
        //   <script>alert("\{{.X}}")</script>
        // Discussion:
        //   Package html/template does not support actions following a
        //   backslash.
        //   This is usually an error and there are better solutions; for
        //   example
        //     <script>alert("{{.X}}")</script>
        //   should work, and if {{.X}} is a partial escape sequence such as
        //   "xA0", mark the whole sequence as safe content: JSStr(`\xA0`)
        ErrPartialEscape

        // ErrRangeLoopReentry: "on range loop re-entry: ..."
        // Example:
        //   <script>var x = [{{range .}}'{{.}},{{end}}]</script>
        // Discussion:
        //   If an iteration through a range would cause it to end in a
        //   different context than an earlier pass, there is no single context.
        //   In the example, there is missing a quote, so it is not clear
        //   whether {{.}} is meant to be inside a JS string or in a JS value
        //   context.  The second iteration would produce something like
        //
        //     <script>var x = ['firstValue,'secondValue]</script>
        ErrRangeLoopReentry

        // ErrSlashAmbig: '/' could start a division or regexp.
        // Example:
        //   <script>
        //     {{if .C}}var x = 1{{end}}
        //     /-{{.N}}/i.test(x) ? doThis : doThat();
        //   </script>
        // Discussion:
        //   The example above could produce `var x = 1/-2/i.test(s)...`
        //   in which the first '/' is a mathematical division operator or it
        //   could produce `/-2/i.test(s)` in which the first '/' starts a
        //   regexp literal.
        //   Look for missing semicolons inside branches, and maybe add
        //   parentheses to make it clear which interpretation you intend.
        ErrSlashAmbig
)
```
We define codes for each error that manifests while escaping templates, but escaped templates may also fail at runtime.            
我们为模版转码 遇到的每个错误定义了号码 清单,但是 转义模版 也可能在运行时出错            
       
Output: "ZgotmplZ" Example:            
```golang
<img src="{{.X}}">
where {{.X}} evaluates to `javascript:...`
```
Discussion:            
```golang
"ZgotmplZ" is a special value that indicates that unsafe content reached a
CSS or URL context at runtime. The output of the example will be
  <img src="#ZgotmplZ">
If the data comes from a trusted source, use content types to exempt it
from filtering: URL(`javascript:...`).

"ZgotmplZ"是一个指定的值, 指明 不安全的内容运行时的每个CSS 或 URL.输出例子是
 <img src="#ZgotmplZ">
如果数据来源可信, 使用内容类型，以其从过滤 豁免: URL(`javascript:...`).

```


##type FuncMap            
```golang
type FuncMap map[string]interface{}
```
FuncMap is the type of the map defining the mapping from names to functions.             
Each function must have either a single return value, or two return values of which the second has type error.             
In that case, if the second (error) argument evaluates to non-nil during execution, execution terminates and Execute returns that error.             
FuncMap has the same base type as FuncMap in "text/template", copied here so clients need not import "text/template".            
FuncMap是从名称到定义函数的映射类型。            
每个函数必须有单独的返回值或两个返回值 第二个值是错误类型            
在这种情况下,如果执行的期间 第二个参数 是非nil,执行中断 并返回那个错误            
FuncMap在"text/template"中 有相同的基础类型,这里复制的 所以 客户端不用引入  "text/template".            
            
            
##type HTML            
```golang
type HTML string
```
HTML encapsulates a known safe HTML document fragment.             
It should not be used for HTML from a third-party, or HTML with unclosed tags or comments.             
The outputs of a sound HTML sanitizer and a template escaped by this package are fine for use with HTML.            
HTML封装了一个已知的安全的HTML文档片段。            
它不应该被用于从HTML第三方，或与HTML标签未封闭或注释。            
使用 HTML 一个 健全的HTML过滤和一个模版转义 在这个包是好的            

            
##type HTMLAttr            
```golang
type HTMLAttr string
```
HTMLAttr encapsulates an HTML attribute from a trusted source, for example, ` dir="ltr"`.            
从可信的源中 封装HTML 属性, 比如  ` dir="ltr"`.            
            
            
##type JS            
```golang
type JS string
```
JS encapsulates a known safe EcmaScript5 Expression, for example, `(x + y * z())`.             
Template authors are responsible for ensuring that typed expressions do not break the intended precedence and that there is no statement/expression ambiguity as when passing an expression like "{ foo: bar() }\n['foo']()",             
which is both a valid Expression and a valid Program with a very different meaning.            
封装已知安全的EcmaScript5表达式, 比如`(x + y * z())`.            
当传递表达式 类似 "{ foo: bar() }\n['foo']()"  模版作者确保键入的表达式不打破预期优先级并且 没有声明/表达歧义            
一个非常不同的含义 即有效的表达式 又有效的程序            
            
##type JSStr            
```golang
type JSStr string
```
JSStr encapsulates a sequence of characters meant to be embedded between quotes in a JavaScript expression.             
The string must match a series of StringCharacters:            
封装 引号之间的一序列字符 嵌入到JS表达式里            
字符串必须是相匹配的一系列字符串字符:            
```golang
StringCharacter :: SourceCharacter but not `\` or LineTerminator
                 | EscapeSequence
```                 
Note that LineContinuations are not allowed. JSStr("foo\\nbar") is fine, but JSStr("foo\\\nbar") is not.            
注:LineContinuations是不允许的.JSStr("foo\\nbar")可以 但是JSStr("foo\\\nbar") 不行            
            
            
##type Template            
```golang
type Template struct {

        // The underlying template's parse tree, updated to be HTML-safe.
           //底层模版解析树 ,更新成HTML安全
        Tree *parse.Tree
        // contains filtered or unexported fields
           //包含过滤或 未导出字段
}
```
Template is a specialized Template from "text/template" that produces a safe HTML document fragment.            
Template是一个"text/template" 专门生产 安全的HTML文档片段            
            
##func Must            
```golang
func Must(t *Template, err error) *Template
```
Must is a helper that wraps a call to a function returning (*Template, error) and panics if the error is non-nil.             
It is intended for use in variable initializations such as            
Must是一个发现一个调用函数返回和 如果 错误 非nil panic 助手            
它在变量初始化时使用 例如:            
var t = template.Must(template.New("name").Parse("html"))            
            
            
##func New            
```golang
func New(name string) *Template
```
New allocates a new HTML template with the given name.            
用给定的name 申请一个新的HTML模版            
            
            
##func ParseFiles            
```golang
func ParseFiles(filenames ...string) (*Template, error)
```
ParseFiles creates a new Template and parses the template definitions from the named files.             
The returned template's name will have the (base) name and (parsed) contents of the first file.             
There must be at least one file. If an error occurs, parsing stops and the returned *Template is nil.            
创建一个新的模版 并且 从命名文件中解析模版定义            
返回的模版名字 将有(基础)名字 和 第一个文件的(解析)内容            
必须至少一个文件. 如果遇到错误,解析停止 返回*Template 是 nil            

            
##func ParseGlob            
```golang
func ParseGlob(pattern string) (*Template, error)
```
ParseGlob creates a new Template and parses the template definitions from the files identified by the pattern, which must match at least one file.             
The returned template will have the (base) name and (parsed) contents of the first file matched by the pattern.             
ParseGlob is equivalent to calling ParseFiles with the list of files matched by the pattern.            
根据pattern 从文件中 创建一个新的模版和解析 模版定义, 匹配的文件必须至少有一个文件            
返回的模版名字 将有(基础)名字 和 第一个文件的(解析)内容            
ParseGlob等价于 调用ParseFiles 根据pattern匹配 文件列表            
            
            
##func (*Template) AddParseTree            
```golang
func (t *Template) AddParseTree(name string, tree *parse.Tree) (*Template, error)
```
AddParseTree creates a new template with the name and parse tree and associates it with t.            
It returns an error if t has already been executed.            
使用name和parse 树 创建一个相应的新的模版             
如果t已经在执行了 会返回错误            
            
##func (*Template) Clone            
```golang
func (t *Template) Clone() (*Template, error)
```
Clone returns a duplicate of the template, including all associated templates.             
The actual representation is not copied, but the name space of associated templates is,             
so further calls to Parse in the copy will add templates to the copy but not to the original.             
Clone can be used to prepare common templates and use them with variant definitions for other templates by adding the variants after the clone is made.            
            
It returns an error if t has already been executed.            
Clone返回重复的模版,包含所有相应的模版            
实际上没有复制,但 命名空间对应的模版是,  所以 在复制时 进一步调用Parse 将会添加模版到复制,但是不是原始的.            
Clone可以用来准备常用模板 并且用它们与其他模板变量的定义中加入变异克隆作出后            
            
            
##func (*Template) Delims            
```golang
func (t *Template) Delims(left, right string) *Template
```
Delims sets the action delimiters to the specified strings, to be used in subsequent calls to Parse, ParseFiles, or ParseGlob.             
Nested template definitions will inherit the settings. An empty delimiter stands for the corresponding default: {{ or }}.             
The return value is the template, so calls can be chained.            
设置指定的字符串为分隔符,随后给Parse, ParseFiles, or ParseGlob调用.            
嵌套模板定义将继承这些设置。一个空的分隔符 默认对应 : {{ or }}.             
返回值是模版 所以调用可以被链接。            
            
            
##func (*Template) Execute            
```golang
func (t *Template) Execute(wr io.Writer, data interface{}) error
```
Execute applies a parsed template to the specified data object, writing the output to wr.             
If an error occurs executing the template or writing its output, execution stops, but partial results may already have been written to the output writer.             
A template may be executed safely in parallel.            
Execute适用 解析一个模版到指定的数据对象 ,把输出写到wr            
如果执行模版或者写输出遇到错误, 执行停止,但是部分结果可能会写到 输出 writer            
模板可以安全地并行执行。            

            
##func (*Template) ExecuteTemplate            
```golang
func (t *Template) ExecuteTemplate(wr io.Writer, name string, data interface{}) error
```
ExecuteTemplate applies the template associated with t that has the given name to the specified data object and writes the output to wr.             
If an error occurs executing the template or writing its output, execution stops, but partial results may already have been written to the output writer.             
A template may be executed safely in parallel.            
ExecuteTemplate适用 和t有关的模版  给定的名字 指定数据对象 并且把输出 写入 wr            
如果执行模版或者写输出遇到错误, 执行停止,但是部分结果可能会写到 输出 writer            
模板可以安全地并行执行。            
            
            
##func (*Template) Funcs            
```golang
func (t *Template) Funcs(funcMap FuncMap) *Template
```
Funcs adds the elements of the argument map to the template's function map.             
It panics if a value in the map is not a function with appropriate return type.             
However, it is legal to overwrite elements of the map. The return value is the template, so calls can be chained.            
Funcs添加参数元素 到 模版的 函数 map            
如果map里的值没有适当的返回类型, 它会 panics.            
然而它合法的覆盖map元素.返回值是template所以调用可以被链接。            
            
##func (*Template) Lookup            
```golang
func (t *Template) Lookup(name string) *Template
```
Lookup returns the template with the given name that is associated with t, or nil if there is no such template.            
使用给定的name返回和t有关的 模版, 或 如果没有这样的模版是nil            
            
            
##func (*Template) Name            
```golang
func (t *Template) Name() string
```
Name returns the name of the template.            
返回模版的名称            
            
##func (*Template) New            
```golang
func (t *Template) New(name string) *Template
```
New allocates a new HTML template associated with the given one and with the same delimiters.             
The association, which is transitive, allows one template to invoke another with a {{template}} action.            
创建一个和给定名字 有关并且有相同分隔符的模版            
关系是过渡的,允许一个模版 使用  {{template}} 调用其他的            
            
##func (*Template) Parse            
```golang
func (t *Template) Parse(src string) (*Template, error)
```
Parse parses a string into a template.             
Nested template definitions will be associated with the top-level template t.             
Parse may be called multiple times to parse definitions of templates to associate with t.             
It is an error if a resulting template is non-empty (contains content other than template definitions) and would replace a non-empty template with the same name.             
(In multiple calls to Parse with the same receiver template, only one call can contain text other than space, comments, and template definitions.)            
解析一个字符串到一个模版            
嵌套模板定义将与顶层模板t关联。            
Parse可能会调用多次来解析和模版定义有关的t            
如果结果模版是非空的(包含内容意外的模版定义)并且 用同样的名字代替非空模版.            
(在多个调用Parse 有相同的模版接收者, 只有 一个调用能包含文本以外的 空白, 注释 和模版定义)            
            
##func (*Template) ParseFiles            
```golang
func (t *Template) ParseFiles(filenames ...string) (*Template, error)
```
ParseFiles parses the named files and associates the resulting templates with t.             
If an error occurs, parsing stops and the returned template is nil;             
otherwise it is t. There must be at least one file.            
解析命名文件 并且 返回和模版有关的 t            
如果遇到错误,解析停止 并且 返回的模版是nil            
返回它是t.至少必须有一个文件.            
            
##func (*Template) ParseGlob            
```golang
func (t *Template) ParseGlob(pattern string) (*Template, error)
```
ParseGlob parses the template definitions in the files identified by the pattern and associates the resulting templates with t.             
The pattern is processed by filepath.Glob and must match at least one file.             
ParseGlob is equivalent to calling t.ParseFiles with the list of files matched by the pattern.            
根据pattern 解析所确定文件中的 模版定义并且 返回模版相关 t            
pattern使用filepath.Glob 处理 并且 必须至少匹配一个文件.            
ParseGlob 等价于 根据pattern   调用t.ParseFiles匹配文件列表            
            
##func (*Template) Templates            
```golang
func (t *Template) Templates() []*Template
```
Templates returns a slice of the templates associated with t, including t itself.            
返回和t有关的模版切片包含t本身            
            
##type URL            
```golang
type URL string
```
URL encapsulates a known safe URL or URL substring (see RFC 3986).             
A URL like `javascript:checkThatFormNotEditedBeforeLeavingPage()` from a trusted source should go in the page,             
but by default dynamic `javascript:` URLs are filtered out since they are a frequently exploited injection vector.            
封装一个安全的 URL 或URL子串            
一个URL 类似`javascript:checkThatFormNotEditedBeforeLeavingPage()`  从一个可信的源 到页面,            
但是 默认的动态 `javascript:` URL 过滤掉  因为它们是一个经常利用注射载体。            

