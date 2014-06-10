包地址：http://golang.org/pkg/encoding/json/
```golang
Package json implements encoding and decoding of JSON objects as defined in RFC 4627. 
The mapping between JSON objects and Go values is described in the documentation for the Marshal and Unmarshal functions.
See "JSON and Go" for an introduction to this package: http://golang.org/doc/articles/json_and_go.html
实现JOSN对象的编码和解码
JSON对象和GO值 的映射 在文档中的描述 是Marshal 和 Unmarshal函数
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
	HTMLEscape 追加JSON编码src 到dst中，内部的 <, >, &, U+2028 and U+2029 字节 修改成\u003c, \u003e, \u0026, \u2028, \u2029，所以JSON内部的HTML<script>标签是安全的
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

在GO1.1 以新的方式处理匿结构体字段。在1.1之前匿名结构体字段是被忽略的
在当前版本和更早的版本中强制忽略构造结构体字段的方法是：给JSON字段一个"_"标签
map值编码成JSON对象。map的key类型必须是字符串;JSON对象的key直接就是map的key
指针值编码成指针指向的值。nil指针编码成空JSON对象。
接口值编码成 接口包含的值。nil接口编码成空JSON对象。
channel、complex、函数值 不能用JSON编码。尝试用Marshal编码 会返回一个UnsupportedTypeError 错误
JSON不能表示循环数据结构，Marshal 不能处理他们。传递循环数据结构给Marshal会无限递归


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

func MarshalIndent
```golang
func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
	MarshalIndent is like Marshal but applies Indent to format the output.
	MarshalIndent 类似Marshal但是 输出格式会有缩进
```


func Unmarshal
```golang
func Unmarshal(data []byte, v interface{}) error
	Unmarshal parses the JSON-encoded data and stores the result in the value pointed to by v.
	
	Unmarshal uses the inverse of the encodings that Marshal uses, allocating maps, slices, and pointers as necessary, with the following additional rules:
	
	To unmarshal JSON into a pointer, Unmarshal first handles the case of the JSON being the JSON literal null. 
	In that case, Unmarshal sets the pointer to nil. Otherwise, Unmarshal unmarshals the JSON into the value pointed at by the pointer. 
	If the pointer is nil, Unmarshal allocates a new value for it to point to.
	
	To unmarshal JSON into a struct, Unmarshal matches incoming object keys to the keys used by Marshal (either the struct field name or its tag), 
		preferring an exact match but also accepting a case-insensitive match.
	
	To unmarshal JSON into an interface value, Unmarshal stores one of these in the interface value:
	
	Unmarshal解析JSON编码数组，并把结果存储在v
	Unmarshal使用 Marshal 的逆编码。如果需要的话会分配 maps 、slices、pointers， 以下是规则：
	对于指针：Unmarshal先处理字面量为空的情况。那种情况下，Unmarshal设置指针为nil。否则Unmarshal 解析指针指向的值。如果指针为nil，Unmarshal会分配一个新值
	
	对于结构体，Unmarshal 会匹配 Marshal使用的key来做为对象的key（无论结构字段名还是它的标签），宁愿一个完整的匹配 也不接受不区分大小写的匹配
	
	对于接口：Unmarshal 把他们存储在接口值里：
	
```golang
	bool, for JSON booleans
	float64, for JSON numbers
	string, for JSON strings
	[]interface{}, for JSON arrays
	map[string]interface{}, for JSON objects
	nil for JSON null
```
	If a JSON value is not appropriate for a given target type, or if a JSON number overflows the target type, 
		Unmarshal skips that field and completes the unmarshalling as best it can. If no more serious errors are encountered, 
		Unmarshal returns an UnmarshalTypeError describing the earliest such error.
	
	When unmarshaling quoted strings, invalid UTF-8 or invalid UTF-16 surrogate pairs are not treated as an error. 
	Instead, they are replaced by the Unicode replacement character U+FFFD.

	如果JSON值不是 给定的目标类型，或 如果json数字 溢出给定的目标，Unmarshal跳过那个字段，并且 如果可能的话以最好的方式解码。
	如果没有遇到更严重的问题，Unmarshal返回一个UnmarshalTypeError描述错误
	
	当解码遇到引号字符串、无效的 UTF-8或无效的UTF-16， 不会把他们当成错误， 而是使用Unicode字符U+FFFD代替

Code:
```golang
var jsonBlob = []byte(`[
    {"Name": "Platypus", "Order": "Monotremata"},
    {"Name": "Quoll",    "Order": "Dasyuromorphia"}
]`)
type Animal struct {
    Name  string
    Order string
}
var animals []Animal
err := json.Unmarshal(jsonBlob, &animals)
if err != nil {
    fmt.Println("error:", err)
}
fmt.Printf("%+v", animals)
```
Output:
```golang
[{Name:Platypus Order:Monotremata} {Name:Quoll Order:Dasyuromorphia}]
```
```


type Decoder
```golang
type Decoder struct {
    // contains filtered or unexported fields
}
A Decoder reads and decodes JSON objects from an input stream.
Decoder 从输入流中 读取和解码json对象


This example uses a Decoder to decode a stream of distinct JSON values.
这个例子使用Decoder 解码不同的JSON值流
Code:
```golang
const jsonStream = `
    {"Name": "Ed", "Text": "Knock knock."}
    {"Name": "Sam", "Text": "Who's there?"}
    {"Name": "Ed", "Text": "Go fmt."}
    {"Name": "Sam", "Text": "Go fmt who?"}
    {"Name": "Ed", "Text": "Go fmt yourself!"}
`
type Message struct {
    Name, Text string
}
dec := json.NewDecoder(strings.NewReader(jsonStream))
for {
    var m Message
    if err := dec.Decode(&m); err == io.EOF {
        break
    } else if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s: %s\n", m.Name, m.Text)
}
```
Output:
```golang
Ed: Knock knock.
Sam: Who's there?
Ed: Go fmt.
Sam: Go fmt who?
Ed: Go fmt yourself!
```
```


func NewDecoder
```golang
func NewDecoder(r io.Reader) *Decoder
	NewDecoder returns a new decoder that reads from r.
	The decoder introduces its own buffering and may read data from r beyond the JSON values requested.
	从r中返回新的decoder
	decoder有它自己的缓冲区，并且可能会在json值需要之前读取数据
```

func (*Decoder) Buffered
```golang
func (dec *Decoder) Buffered() io.Reader
	Buffered returns a reader of the data remaining in the Decoder's buffer. The reader is valid until the next call to Decode.
	返回Decoder缓冲区剩余的数据reader。这个reader在下一次调用Decode之前都是有效的
```

func (*Decoder) Decode
```golang
func (dec *Decoder) Decode(v interface{}) error
	Decode reads the next JSON-encoded value from its input and stores it in the value pointed to by v.
	See the documentation for Unmarshal for details about the conversion of JSON into a Go value.
	Decode读取下一个输入的 json编码值并且把它存储在v里
	查看文档了解  把json转换成GO值的 相关的内容
```

func (*Decoder) UseNumber
```golang
func (dec *Decoder) UseNumber()
	UseNumber causes the Decoder to unmarshal a number into an interface{} as a Number instead of as a float64.
	UseNumber 把Decoder 的数字解码到interface{}里， 用来代替float64。
```

type Encoder
```golang
type Encoder struct {
    // contains filtered or unexported fields
}
An Encoder writes JSON objects to an output stream.
Encoder 写入JSON对象到输出流
```

func NewEncoder
```golang
func NewEncoder(w io.Writer) *Encoder
	NewEncoder returns a new encoder that writes to w.
	返回一个写入w 的新的encoder
```

func (*Encoder) Encode
```golang
func (enc *Encoder) Encode(v interface{}) error
	Encode writes the JSON encoding of v to the stream.
	See the documentation for Marshal for details about the conversion of Go values to JSON.
	把编码v写入JSON编码数据流
```

type InvalidUTF8Error
```golang
type InvalidUTF8Error struct {
    S string // the whole string value that caused the error
}
Before Go 1.2, an InvalidUTF8Error was returned by Marshal when attempting to encode a string value with invalid UTF-8 sequences. 
As of Go 1.2, Marshal instead coerces the string to valid UTF-8 by replacing invalid bytes with the Unicode replacement rune U+FFFD. 
This error is no longer generated but is kept for backwards compatibility with programs that might mention it.

GO1.2之前，当编码字符串遇到无效的UTF-8字符时，尝试Marshal编码一个字符串 会返回 InvalidUTF8Error 错误
到了GO1.2，Marshal用Unicode的 U+FFFD 来代替无效的字节。
这个错误不会再产生母，但是为了保持向后兼容，会在程序里提起.
```

func (*InvalidUTF8Error) Error
```golang
func (e *InvalidUTF8Error) Error() string
```

type InvalidUnmarshalError
```golang
type InvalidUnmarshalError struct {
    Type reflect.Type
}
An InvalidUnmarshalError describes an invalid argument passed to Unmarshal. (The argument to Unmarshal must be a non-nil pointer.)
给Unmarshal传递无效的参数时会产生 InvalidUnmarshalError错误.(Unmarshal的参数必须不为nil指针)
```

func (*InvalidUnmarshalError) Error
```golang
func (e *InvalidUnmarshalError) Error() string
```

type Marshaler
```golang
type Marshaler interface {
    MarshalJSON() ([]byte, error)
}
Marshaler is the interface implemented by objects that can marshal themselves into valid JSON.
Marshaler 是实现可以把他们自己编码成有效的JSON的 对象接口
```

type MarshalerError
```golang
type MarshalerError struct {
    Type reflect.Type
    Err  error
}
```

func (*MarshalerError) Error
```golang
func (e *MarshalerError) Error() string
```

type Number
```golang
type Number string
	A Number represents a JSON number literal.
表示一个json数字 字面量
```

func (Number) Float64
```golang
func (n Number) Float64() (float64, error)
	Float64 returns the number as a float64.
	返回float64 的数字
```

func (Number) Int64
```golang
func (n Number) Int64() (int64, error)
	Int64 returns the number as an int64.
	返回int64数字
```

func (Number) String
```golang
func (n Number) String() string
String returns the literal text of the number.
返回数字的文本
```

type RawMessage
```golang
type RawMessage []byte
RawMessage is a raw encoded JSON object. It implements Marshaler and Unmarshaler and can be used to delay JSON decoding or precompute a JSON encoding.
RawMessage是一个原始的JSON编码对象.它实现 延迟 或预处理  Marshaler和Unmarshaler JSON对象

This example uses RawMessage to delay parsing part of a JSON message.
这是个使用RawMessage 延迟解析部分JSON信息. 
Code:
```golang
type Color struct {
    Space string
    Point json.RawMessage // delay parsing until we know the color space
}
type RGB struct {
    R uint8
    G uint8
    B uint8
}
type YCbCr struct {
    Y  uint8
    Cb int8
    Cr int8
}

var j = []byte(`[
    {"Space": "YCbCr", "Point": {"Y": 255, "Cb": 0, "Cr": -10}},
    {"Space": "RGB",   "Point": {"R": 98, "G": 218, "B": 255}}
]`)
var colors []Color
err := json.Unmarshal(j, &colors)
if err != nil {
    log.Fatalln("error:", err)
}

for _, c := range colors {
    var dst interface{}
    switch c.Space {
    case "RGB":
        dst = new(RGB)
    case "YCbCr":
        dst = new(YCbCr)
    }
    err := json.Unmarshal(c.Point, dst)
    if err != nil {
        log.Fatalln("error:", err)
    }
    fmt.Println(c.Space, dst)
}
```golang
Output:
```golang
YCbCr &{255 0 -10}
RGB &{98 218 255}
```
```

func (*RawMessage) MarshalJSON
```golang
func (m *RawMessage) MarshalJSON() ([]byte, error)
	MarshalJSON returns *m as the JSON encoding of m.
	返回 *m
```

func (*RawMessage) UnmarshalJSON
```golang
func (m *RawMessage) UnmarshalJSON(data []byte) error
	UnmarshalJSON sets *m to a copy of data.
	UnmarshalJSON 设置*m 复制数据
```

type SyntaxError
```golang
type SyntaxError struct {
    Offset int64 // error occurred after reading Offset bytes
    // contains filtered or unexported fields
}
A SyntaxError is a description of a JSON syntax error.
描述 JSON语法错误
```

func (*SyntaxError) Error
```golang
func (e *SyntaxError) Error() string
```

type UnmarshalFieldError
```golang
type UnmarshalFieldError struct {
    Key   string
    Type  reflect.Type
    Field reflect.StructField
}
An UnmarshalFieldError describes a JSON object key that led to an unexported (and therefore unwritable) struct field. (No longer used; kept for compatibility.)
描述json对象key导致不导出结构体字段的错误;(这个不再使用,只是为了保持兼任性)
```

func (*UnmarshalFieldError) Error
```golang
func (e *UnmarshalFieldError) Error() string
```

type UnmarshalTypeError
```golang
type UnmarshalTypeError struct {
    Value string       // description of JSON value - "bool", "array", "number -5"
    Type  reflect.Type // type of Go value it could not be assigned to
}
An UnmarshalTypeError describes a JSON value that was not appropriate for a value of a specific Go type.
UnmarshalTypeError 描述json值不适用指定的 GO类型 的错误
```

func (*UnmarshalTypeError) Error
```golang
func (e *UnmarshalTypeError) Error() string
```

type Unmarshaler
```golang
type Unmarshaler interface {
    UnmarshalJSON([]byte) error
}
Unmarshaler is the interface implemented by objects that can unmarshal a JSON description of themselves. 
The input can be assumed to be a valid encoding of a JSON value. 
UnmarshalJSON must copy the JSON data if it wishes to retain the data after returning.
Unmarshaler 实现可以解码JSON说明本身的 接口对象
这个输入假定是一个有效 JSON值
UnmarshalJSON 如果希望在返回后保留数据, 必须复制JSON数据
```

type UnsupportedTypeError
```golang
type UnsupportedTypeError struct {
    Type reflect.Type
}
An UnsupportedTypeError is returned by Marshal when attempting to encode an unsupported value type.
尝试编码不支持的值类型会返回UnsupportedTypeError错误
```

func (*UnsupportedTypeError) Error
```golang
func (e *UnsupportedTypeError) Error() string
```

type UnsupportedValueError
```golang
type UnsupportedValueError struct {
    Value reflect.Value
    Str   string
}
```

func (*UnsupportedValueError) Error
```golang
func (e *UnsupportedValueError) Error() string
```