包地址：http://golang.org/pkg/encoding/json/
```golang
Package json implements encoding and decoding of JSON objects as defined in RFC 4627. 
The mapping between JSON objects and Go values is described in the documentation for the Marshal and Unmarshal functions.
See "JSON and Go" for an introduction to this package: http://golang.org/doc/articles/json_and_go.html
实现JOSN对象的编码和解码
JSON对象和GO值在文档中的描述 是Marshal 和 Unmarshal函数
```

func Compact
```golang
func Compact(dst *bytes.Buffer, src []byte) error
	Compact appends to dst the JSON-encoded src with insignificant space characters elided.
	Compact 追加JSON编码src到 dst,省略无关紧要的空格字符
```

func HTMLEscape
```golang
func HTMLEscape(dst *bytes.Buffer, src []byte)
	HTMLEscape appends to dst the JSON-encoded src with <, >, &, U+2028 and U+2029 characters inside 
		string literals changed to \u003c, \u003e, \u0026, \u2028, \u2029 so that the JSON will be safe to embed inside HTML <script> tags.
	For historical reasons, web browsers don't honor standard HTML escaping within <script> tags, so an alternative JSON encoding must be used.
	HTMLEscape 追加JSON编码src 到dst中， <, >, &, U+2028 and U+2029 字节 修改成\u003c, \u003e, \u0026, \u2028, \u2029，所以JSON内部的HTML<script>标签是安全的
	历史原因，web浏览器没有标准的过滤 里面的<script> 标签，所以必须用JSON代替
```

func Indent
```golang
func Indent(dst *bytes.Buffer, src []byte, prefix, indent string) error
	Indent appends to dst an indented form of the JSON-encoded src. 
	Each element in a JSON object or array begins on a new, 
		indented line beginning with prefix followed by one or more copies of indent according to the indentation nesting. 
	The data appended to dst has no trailing newline, to make it easier to embed inside other formatted JSON data.
	Indent 追加缩进格式的src JSON编码到 dst
	JSON对象里的每个元素或数组从新的开始，缩进行以一个或多个嵌套缩进开始
	数据追加到dst 没有换行符，让它能更方便的嵌入其他 JSON数据格式
```


func Marshal
```golang
```golang
func Marshal(v interface{}) ([]byte, error)
```
Marshal returns the JSON encoding of v.

Marshal traverses the value v recursively. 
If an encountered value implements the Marshaler interface and is not a nil pointer, Marshal calls its MarshalJSON method to produce JSON. 
The nil pointer exception is not strictly necessary but mimics a similar, necessary exception in the behavior of UnmarshalJSON.

Otherwise, Marshal uses the following type-dependent default encodings:

Boolean values encode as JSON booleans.

Floating point, integer, and Number values encode as JSON numbers.

String values encode as JSON strings. InvalidUTF8Error will be returned if an invalid UTF-8 sequence is encountered. 
	The angle brackets "<" and ">" are escaped to "\u003c" and "\u003e" to keep some browsers from misinterpreting JSON output as HTML.

Array and slice values encode as JSON arrays, except that []byte encodes as a base64-encoded string, and a nil slice encodes as the null JSON object.

Struct values encode as JSON objects. Each exported struct field becomes a member of the object unless

Marshal返回v的JSON编码
递归遍历v的值
如果值Marshaler接口，而且不是一个空指针，Marshal调用MarshalJSON方法 产生json
空指针不是必要的
否则，Marshal使用类型相关的默认编码：
编码值用JSON 布尔值
浮点值，整型 和数字值 用JSON 数字
字符串值编码用 JSON字符串。如果遇到无效的UTF-8序列会返回 InvalidUTF8Error错误
	尖括号"<" 和 ">"  过滤成 "\u003c" 和 "\u003e"，让一些浏览器保持 输出JSON成HTML
数组和slice编码成JSON 数组，除了[]byte 编码成base64字符串，nil slice编码成空JSON对象
结构体编码成JSON对象。结构体的每个字段变成对象成员

```golang
- the field's tag is "-", or
- the field is empty and its tag specifies the "omitempty" option.
```

The empty values are false, 0, any nil pointer or interface value, and any array, slice, map, or string of length zero. 
The object's default key string is the struct field name but can be specified in the struct field's tag value. 
The "json" key in the struct field's tag value is the key name, followed by an optional comma and options. Examples:
空值是false，0 任何空指针或接口值，任何长度为0的 数组 ,slice, map，字符串 
对象的默认键字符串是结构体的字段名， 可以指定 结构体里 字段的标签值
结构体里的 "json"键 字段标签值 是键的名称，可选逗号和可选参数。例子：

```golang
// Field is ignored by this package. Field被忽略
Field int `json:"-"`

// Field appears in JSON as key "myName".   Field根据键myName 出现在JSON
Field int `json:"myName"`

// Field appears in JSON as key "myName" and
// the field is omitted from the object if its value is empty, 如果对象值是空的 字段被省略
// as defined above.
Field int `json:"myName,omitempty"`

// Field appears in JSON as key "Field" (the default), but
// the field is skipped if empty. 字段如果是空的 被忽略
// Note the leading comma.
Field int `json:",omitempty"`
```

The "string" option signals that a field is stored as JSON inside a JSON-encoded string. 
It applies only to fields of string, floating point, or integer types. 
This extra level of encoding is sometimes used when communicating with JavaScript programs:
可以在字段里 把JSON 存储在JSON编码字符串
它只适用字符串 、浮点型、整型字段
这个特性有时候用在和JS程序通信：

```golang
Int64String int64 `json:",string"`
```


The key name will be used if it's a non-empty string consisting of only Unicode letters, digits, dollar signs, percent signs, hyphens, underscores and slashes.
如果非用字符串并且只包含Unicode字母、数字、美元符号、百分号、连字符、下划线和 斜线    键名 会被使用，

Anonymous struct fields are usually marshaled as if their inner exported fields were fields in the outer struct, 
	subject to the usual Go visibility rules amended as described in the next paragraph. 
An anonymous struct field with a name given in its JSON tag is treated as having that name, rather than being anonymous.
匿名结构体字段   如果他们内嵌字段在结构体之外  通常使用marshaled ,在接下来的段落 围绕者 GO的可视规则
一个匿名结构体会在它的JSON标签有它的名字，而不是匿名的

The Go visibility rules for struct fields are amended for JSON when deciding which field to marshal or unmarshal. 
If there are multiple fields at the same level, and that level is the least nested 
	(and would therefore be the nesting level selected by the usual Go rules), the following extra rules apply:
GO的结构字段的可视字段 当字段的 marshal或 unmarshal 决定修改JSON
如果有多个字段在同一级，最低级的嵌套(因此，并不会因一般的规则选择嵌套级别) 以下附加规则适用：

1) Of those fields, if any are JSON-tagged, only tagged fields are considered, even if there are multiple untagged fields that would otherwise conflict. 
2) If there is exactly one field (tagged or not according to the first rule), that is selected. 
3) Otherwise there are multiple fields, and all are ignored; no error occurs.

Handling of anonymous struct fields is new in Go 1.1. Prior to Go 1.1, anonymous struct fields were ignored. 
To force ignoring of an anonymous struct field in both current and earlier versions, give the field a JSON tag of "-".

Map values encode as JSON objects. The map's key type must be string; the object keys are used directly as map keys.

Pointer values encode as the value pointed to. A nil pointer encodes as the null JSON object.

Interface values encode as the value contained in the interface. A nil interface value encodes as the null JSON object.

Channel, complex, and function values cannot be encoded in JSON. Attempting to encode such a value causes Marshal to return an UnsupportedTypeError.

JSON cannot represent cyclic data structures and Marshal does not handle them. Passing cyclic structures to Marshal will result in an infinite recursion.

1)这些字段，如果任何JSON标记，只有标记字段被认为是，即使如果有多个无标记字段，否则会冲突
2)如果有一个字段恰好是（标记或不对应第一条规则）， 会被选择
3)否则多个字段会被忽略，不会产生错误


Code:
```golang
type ColorGroup struct {
    ID     int
    Name   string
    Colors []string
}
group := ColorGroup{
    ID:     1,
    Name:   "Reds",
    Colors: []string{"Crimson", "Red", "Ruby", "Maroon"},
}
b, err := json.Marshal(group)
if err != nil {
    fmt.Println("error:", err)
}
os.Stdout.Write(b)
```
Output:
```golang
{"ID":1,"Name":"Reds","Colors":["Crimson","Red","Ruby","Maroon"]}
```
```

