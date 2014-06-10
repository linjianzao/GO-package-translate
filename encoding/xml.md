包地址：http://golang.org/pkg/encoding/xml/
```golang
Package xml implements a simple XML 1.0 parser that understands XML name spaces.
实现简单的XML1.0解析 理解 XML 命名空间
```

Constants
```golang
const (
    // A generic XML header suitable for use with the output of Marshal.  通用的xml头 适应输出解码
    // This is not automatically added to any output of this package,	作为一个方便,这个包不会自动添加任何输出
    // it is provided as a convenience.	
    Header = `<?xml version="1.0" encoding="UTF-8"?>` + "\n"
)
```

Variables
```golang
```golang
var HTMLAutoClose = htmlAutoClose
```
HTMLAutoClose is the set of HTML elements that should be considered to close automatically.
HTMLAutoClose 设置html元素并自动关闭

```golang
var HTMLEntity = htmlEntity
```
HTMLEntity is an entity map containing translations for the standard HTML entity characters.
将内容 映射成和标准HTML实体字符
```

func Escape
```golang
func Escape(w io.Writer, s []byte)
	Escape is like EscapeText but omits the error return value. It is provided for backwards compatibility with Go 1.0. 
	Code targeting Go 1.1 or later should use EscapeText.
	Escape类似EscapeText 但是 忽略了错误.它 向后兼容 GO 1.0
	GO1.1或者更高版本应该使用EscapeText
```

func EscapeText
```golang
func EscapeText(w io.Writer, s []byte) error
	EscapeText writes to w the properly escaped XML equivalent of the plain text data s.
	将s 正确转义成XML纯文本  写入w
```

func Marshal
```golang
```golang
func Marshal(v interface{}) ([]byte, error)
```
Marshal returns the XML encoding of v.
Marshal handles an array or slice by marshalling each of the elements. 
Marshal handles a pointer by marshalling the value it points at or, if the pointer is nil, by writing nothing. 
Marshal handles an interface value by marshalling the value it contains or, if the interface value is nil, by writing nothing. 
Marshal handles all other data by writing one or more XML elements containing the data.
返回XML编码v.
用marshalling处理数组或slice里的每个元素
用marshalling处理指针指向的值, 如果指针是nil,没有任何写入
用marshalling处理接口包含的值,如果接口是nil,没有任何写入
用writing处理所有其他的数据 ,一个或多个数据包含的XML元素


The name for the XML elements is taken from, in order of preference:
XML元素的取名, 按优先顺序:
```golang
- the tag on the XMLName field, if the data is a struct  如果数据是结构体,XMLName字段的标签
- the value of the XMLName field of type xml.Name  		XMLName字段是xml.Name类型的值
- the tag of the struct field used to obtain the data		使用结构体字段标签获取数据
- the name of the struct field used to obtain the data	使用结构体字段名称 获取数据
- the name of the marshalled type								marshalled类型的名称
```

The XML element for a struct contains marshalled elements for each of the exported fields of the struct, with these exceptions:
对于XML元素 结构体 包含的每个可导出字段 的 marshalled元素, 有这些异常:
```golang
- the XMLName field, described above, is omitted.	XMLName字段,上述,被忽略
- a field with tag "-" is omitted.	
字段标签是 "-"

- a field with tag "name,attr" becomes an attribute with the given name in the XML element.  
在XML元素中使用给定的名称让标签的"name,attr"变成一个属性

- a field with tag ",attr" becomes an attribute with the field name in the XML element. 	
在XML元素中使用给定的名称让标签的",attr"变成一个属性

- a field with tag ",chardata" is written as character data,not as an XML element.			
字段标签",chardata"写成字符数据, 而不是作为一个XML元素

- a field with tag ",innerxml" is written verbatim, not subject to the usual marshalling procedure.  
字段标签",innerxml"逐字写,而不是像通常marshalling

- a field with tag ",comment" is written as an XML comment, not subject to the usual marshalling procedure. It must not contain the "--" string within it.
字段标签",comment"写成XML的comment,而不是像通常marshalling. 必须不包含"--字符串"

- a field with a tag including the "omitempty" option is omitted if the field value is empty. 
The empty values are false, 0, any nil pointer or interface value, and any array, slice, map, or string of length zero.
字段标签包含"omitempty"选项,如果字段值是空的被忽略

- an anonymous struct field is handled as if the fields of its value were part of the outer struct.
 如果字段值是 外面结构体的一部分  ,匿名结构体字段已处理

``` 
If a field uses a tag "a>b>c", then the element c will be nested inside parent elements a and b. 
Fields that appear next to each other that name the same parent will be enclosed in one XML element.
See MarshalIndent for an example.
Marshal will return an error if asked to marshal a channel, function, or map.
如果字段使用标签 "a>b>c"  元素c会被父嵌套  a和b 里面.
字段 出现下一个 其他的同名父元素 将会被 归在同一个XML元素里
看MarshalIndent的例子
如果 解码 channel, function, 或map会返回错误
```


func MarshalIndent
``golang
```golang
func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
	MarshalIndent works like Marshal, 
	but each XML element begins on a new indented line that starts with prefix and is followed by one or more copies of indent according to the nesting depth.
	
```

Code:
```golang
type Address struct {
    City, State string
}
type Person struct {
    XMLName   xml.Name `xml:"person"`
    Id        int      `xml:"id,attr"`
    FirstName string   `xml:"name>first"`
    LastName  string   `xml:"name>last"`
    Age       int      `xml:"age"`
    Height    float32  `xml:"height,omitempty"`
    Married   bool
    Address
    Comment string `xml:",comment"`
}

v := &Person{Id: 13, FirstName: "John", LastName: "Doe", Age: 42}
v.Comment = " Need more details. "
v.Address = Address{"Hanga Roa", "Easter Island"}

output, err := xml.MarshalIndent(v, "  ", "    ")
if err != nil {
    fmt.Printf("error: %v\n", err)
}

os.Stdout.Write(output)
```
Output:
```golang
  <person id="13">
      <name>
          <first>John</first>
          <last>Doe</last>
      </name>
      <age>42</age>
      <Married>false</Married>
      <City>Hanga Roa</City>
      <State>Easter Island</State>
      <!-- Need more details. -->
  </person>
```
```


func Unmarshal
```golang
```golang
func Unmarshal(data []byte, v interface{}) error
```
Unmarshal parses the XML-encoded data and stores the result in the value pointed to by v, which must be an arbitrary struct, slice, or string. 
Well-formed data that does not fit into v is discarded.

Because Unmarshal uses the reflect package, it can only assign to exported (upper case) fields. 
Unmarshal uses a case-sensitive comparison to match XML element names to tag values and struct field names.

Unmarshal maps an XML element to a struct using the following rules. 
In the rules, the tag of a field refers to the value associated with the key 'xml' in the struct field's tag (see the example above).

Unmarshal解析XML编码数据 把结果存储在v里,必须是 struct, slice, 或 string. 其他格式的被丢弃
因为Unmarshal使用映射包,它只能用于可导出字段(大写)
Unmarshal 使用区分大小写 来匹配XML 元素名称 的tag值 和 结构体的字段名称
Unmarshal使用以下的规则来映射XML元素到结构体

```golang
* If the struct has a field of type []byte or string with tag
   ",innerxml", Unmarshal accumulates the raw XML nested inside the
   element in that field.  The rest of the rules still apply.
如果结构体有一个字段是[]byte类型或 字符串 的标签是 ",innerxml".
Unmarshal 累加原始XML嵌套元素到那个字段.规则的其余部分仍然适用

* If the struct has a field named XMLName of type xml.Name,
   Unmarshal records the element name in that field.
如果结构体有字段名是 xml.Name类型的 XMLName.Unmarshal在那个字段记录元素名称

* If the XMLName field has an associated tag of the form
   "name" or "namespace-URL name", the XML element must have
   the given name (and, optionally, name space) or else Unmarshal
   returns an error.
如果XMLName字段有相关标签是 "name" 或者 "namespace-URL name",
XML元素必须给定名称(命名空间可选)或者 Unmarshal返回一个错误

* If the XML element has an attribute whose name matches a
   struct field name with an associated tag containing ",attr" or
   the explicit name in a struct field tag of the form "name,attr",
   Unmarshal records the attribute value in that field.
如果XML元素XML元素有一个属性 名字匹配一个 结构体字段名字 相关的tag 包含",attr"或
结构体字段里有 确切的tag名字 "name,attr" ,Unmarshal在那个字段记录属性值

* If the XML element contains character data, that data is
   accumulated in the first struct field that has tag ",chardata".
   The struct field may have type []byte or string.
   If there is no such field, the character data is discarded.
如果XML元素包含字符数据, 那个数据 第一个 结构体字段 有tag ",chardata".
结构体字段有 []byte 或者 string 类型.如果没有这个字段, 字符数据被丢弃

* If the XML element contains comments, they are accumulated in
   the first struct field that has tag ",comment".  The struct
   field may have type []byte or string.  If there is no such
   field, the comments are discarded.
如果XML元素包含comments, 第一个结构体字段有tag  ",comment".结构体字段也许会有[]byte 或者 string 类型.
如果没有这个字段,comments被丢弃

* If the XML element contains a sub-element whose name matches
   the prefix of a tag formatted as "a" or "a>b>c", unmarshal
   will descend into the XML structure looking for elements with the
   given names, and will map the innermost elements to that struct
   field. A tag starting with ">" is equivalent to one starting
   with the field name followed by ">".
如果XML元素包含一个子元素,名字匹配 的tag格式 的前缀有 "a" 或者 "a>b>c",
unmarshal将会 用给定的名字查看元素, 将会映射最里面的元素到 结构体字段.
tag以">"开始等价于 字段名后面跟">"

* If the XML element contains a sub-element whose name matches
   a struct field's XMLName tag and the struct field has no
   explicit name tag as per the previous rule, unmarshal maps
   the sub-element to that struct field.
如果XML元素包含子元素,名字匹配 结构体字段的XMLName tag , 结构体字段没有确切的名字 tag,按照以前的规则
unmarshal映射子元素到那个结构体字段

* If the XML element contains a sub-element whose name matches a
   field without any mode flags (",attr", ",chardata", etc), Unmarshal
   maps the sub-element to that struct field.
如果XML元素包含子元素, 名字没有匹配任何flags (",attr", ",chardata", 等),Unmarshal映射子元素到结构体字段

* If the XML element contains a sub-element that hasn't matched any
   of the above rules and the struct has a field with tag ",any",
   unmarshal maps the sub-element to that struct field.
如果XML元素包含一个子元素 没有匹配以上的任何规则, 结构体字段有tag ",any",Unmarshal映射子元素到结构体字段

* An anonymous struct field is handled as if the fields of its
   value were part of the outer struct.
 如果字段值是 外面结构体的一部分  ,匿名结构体字段已处理

* A struct field with tag "-" is never unmarshalled into.
结构体字段 tag "-"  从不会 反编组
```

Unmarshal maps an XML element to a string or []byte by saving the concatenation of that element's character data in the string or []byte. 
The saved []byte is never nil.

Unmarshal maps an attribute value to a string or []byte by saving the value in the string or slice.

Unmarshal maps an XML element to a slice by extending the length of the slice and mapping the element to the newly created value.

Unmarshal maps an XML element or attribute value to a bool by setting it to the boolean value represented by the string.

Unmarshal maps an XML element or attribute value to an integer or floating-point field by setting the field to the result of interpreting the string value in decimal. 
There is no check for overflow.

Unmarshal maps an XML element to an xml.Name by recording the element name.

Unmarshal maps an XML element to a pointer by setting the pointer to a freshly allocated value and then mapping the element to that value.

Unmarshal映射一个XML元素到 string或 []byte 保存级联元素的字符数据到 string或 []byte.保存的[]byte不会是nil
Unmarshal映射一个属性值到string 或 []byte, 保存到string 或 slice里
Unmarshal映射一个XML元素到slice,扩展slice 长度 和mapping 元素给 新创建的值.
Unmarshal映射一个XML元素或者属性值 bool,使用字符串表示布尔值
Unmarshal映射一个XML元素或者属性值 到整型或浮点型,设置字段 的结果为 十进制的字符串值,这边不会检查溢出
Unmarshal映射一个XML元素到 xml.Name  记录元素名
Unmarshal映射一个XML元素到指针,设置新申请的指针值,并映射元素到那个值


This example demonstrates unmarshaling an XML excerpt into a value with some preset fields. 
Note that the Phone field isn't modified and that the XML <Company> element is ignored. 
Also, the Groups field is assigned considering the element path provided in its tag.
这个例子演示解码一个预设置一些字段的XML片段到一个值
注:Phone字段不能修改,XML <Company> 元素被忽略
Groups字段分配到相应的元素
Code:
```golang
type Email struct {
    Where string `xml:"where,attr"`
    Addr  string
}
type Address struct {
    City, State string
}
type Result struct {
    XMLName xml.Name `xml:"Person"`
    Name    string   `xml:"FullName"`
    Phone   string
    Email   []Email
    Groups  []string `xml:"Group>Value"`
    Address
}
v := Result{Name: "none", Phone: "none"}

data := `
    <Person>
        <FullName>Grace R. Emlin</FullName>
        <Company>Example Inc.</Company>
        <Email where="home">
            <Addr>gre@example.com</Addr>
        </Email>
        <Email where='work'>
            <Addr>gre@work.com</Addr>
        </Email>
        <Group>
            <Value>Friends</Value>
            <Value>Squash</Value>
        </Group>
        <City>Hanga Roa</City>
        <State>Easter Island</State>
    </Person>
`
err := xml.Unmarshal([]byte(data), &v)
if err != nil {
    fmt.Printf("error: %v", err)
    return
}
fmt.Printf("XMLName: %#v\n", v.XMLName)
fmt.Printf("Name: %q\n", v.Name)
fmt.Printf("Phone: %q\n", v.Phone)
fmt.Printf("Email: %v\n", v.Email)
fmt.Printf("Groups: %v\n", v.Groups)
fmt.Printf("Address: %v\n", v.Address)
```
Output:
```golang
XMLName: xml.Name{Space:"", Local:"Person"}
Name: "Grace R. Emlin"
Phone: "none"
Email: [{home gre@example.com} {work gre@work.com}]
Groups: [Friends Squash]
Address: {Hanga Roa Easter Island}
```
```

type Attr
```golang
type Attr struct {
    Name  Name
    Value string
}
	An Attr represents an attribute in an XML element (Name=Value).
	表示XML元素里的一个属性
```

type CharData
```golang
type CharData []byte
	A CharData represents XML character data (raw text), in which XML escape sequences have been replaced by the characters they represent.
	表示XML字符数据(原始文本),其实 XML  escape 序列会 代替他们表示的字符
```

func (CharData) Copy
```golang
func (c CharData) Copy() CharData
```

type Comment
```golang
type Comment []byte
	A Comment represents an XML comment of the form <!--comment-->. The bytes do not include the <!-- and --> comment markers.
	表示一个XML comment 的格式 <!--comment-->.字节不包含<!-- and --> 注释标记。
```

func (Comment) Copy
```golang
func (c Comment) Copy() Comment
```

type Decoder
```golang
type Decoder struct {
    // Strict defaults to true, enforcing the requirements
    // of the XML specification.
    // If set to false, the parser allows input containing common
    // mistakes:
    //	* If an element is missing an end tag, the parser invents
    //	  end tags as necessary to keep the return values from Token
    //	  properly balanced.
    //	* In attribute values and character data, unknown or malformed
    //	  character entities (sequences beginning with &) are left alone.
    //
    // Setting:
    //
    //	d.Strict = false;
    //	d.AutoClose = HTMLAutoClose;
    //	d.Entity = HTMLEntity
    //
    // creates a parser that can handle typical HTML.
    //
    // Strict mode does not enforce the requirements of the XML name spaces TR.
    // In particular it does not reject name space tags using undefined prefixes.
    // Such tags are recorded with the unknown prefix as the name space URL.
    Strict bool

    // When Strict == false, AutoClose indicates a set of elements to
    // consider closed immediately after they are opened, regardless
    // of whether an end element is present.
    AutoClose []string

    // Entity can be used to map non-standard entity names to string replacements.
    // The parser behaves as if these standard mappings are present in the map,
    // regardless of the actual map content:
    //
    //	"lt": "<",
    //	"gt": ">",
    //	"amp": "&",
    //	"apos": "'",
    //	"quot": `"`,
    Entity map[string]string

    // CharsetReader, if non-nil, defines a function to generate
    // charset-conversion readers, converting from the provided
    // non-UTF-8 charset into UTF-8. If CharsetReader is nil or
    // returns an error, parsing stops with an error. One of the
    // the CharsetReader's result values must be non-nil.
    CharsetReader func(charset string, input io.Reader) (io.Reader, error)

    // DefaultSpace sets the default name space used for unadorned tags,
    // as if the entire XML stream were wrapped in an element containing
    // the attribute xmlns="DefaultSpace".
    DefaultSpace string
    // contains filtered or unexported fields
}
A Decoder represents an XML parser reading a particular input stream. The parser assumes that its input is encoded in UTF-8.
表示一个XML解析 读取特殊 输入流. parser 假设所有的输入都是UTF-8 编码
```

func NewDecoder
```golang
func NewDecoder(r io.Reader) *Decoder
	NewDecoder creates a new XML parser reading from r.
	从r中创建一个新的XML解析读取
```

func (*Decoder) Decode
```golang
func (d *Decoder) Decode(v interface{}) error
	Decode works like xml.Unmarshal, except it reads the decoder stream to find the start element.
	类似xml.Unmarshal, 除了 它读取decoder 流 查找开始 元素
```

func (*Decoder) DecodeElement
```golang
func (d *Decoder) DecodeElement(v interface{}, start *StartElement) error
	DecodeElement works like xml.Unmarshal except that it takes a pointer to the start XML element to decode into v. 
	It is useful when a client reads some raw XML tokens itself but also wants to defer to Unmarshal for some elements.
	
	类似xml.Unmarshal 除了它 需要一个指针 让 XML元素start解码到 v
	当客户端读取一些原始的XML tokens 同时也要推迟Unmarshal 一些元素的时候它很有用
```

func (*Decoder) RawToken
```golang
func (d *Decoder) RawToken() (Token, error)
	RawToken is like Token but does not verify that start and end elements match and does not translate name space prefixes to their corresponding URLs.
	类似Token 但是没有验证 开始和结束的元素匹配, 并且 没有命名空间前缀对应到他们的URLs
```

func (*Decoder) Skip
```golang
func (d *Decoder) Skip() error
	Skip reads tokens until it has consumed the end element matching the most recent start element already consumed. 
	It recurs if it encounters a start element, so it can be used to skip nested structures. 
	It returns nil if it finds an end element matching the start element; otherwise it returns an error describing the problem.
	读取tokens 直到最后的元素   匹配离开始元素最近的元素
	如果它遇到开始元素 会回溯,所以 用于跳过嵌套结构
	如果它在最后元素匹配 开始元素, 返回nil.否则返回程序的错误
```

func (*Decoder) Token
```golang
func (d *Decoder) Token() (t Token, err error)
Token returns the next XML token in the input stream. At the end of the input stream, Token returns nil, io.EOF.

Slices of bytes in the returned token data refer to the parser's internal buffer and remain valid only until the next call to Token. 
To acquire a copy of the bytes, call CopyToken or the token's Copy method.

Token expands self-closing elements such as <br/> into separate start and end elements returned by successive calls.

Token guarantees that the StartElement and EndElement tokens it returns are properly nested and matched: 
if Token encounters an unexpected end element, it will return an error.

Token implements XML name spaces as described by http://www.w3.org/TR/REC-xml-names/. 
Each of the Name structures contained in the Token has the Space set to the URL identifying its name space when known. 
If Token encounters an unrecognized name space prefix, it uses the prefix as the Space rather than report an error.

返回输入流的下一个XML token.在输入流的最后,Token 返回nil,io.EOF

字节的Slice在返回的token数据中 参考解析器的内部的缓冲区 并且保留有效值 直到下一次调用Token.
获取复制的字节,调用CopyToken 或者token的 Copy 方法

Token 扩展 自闭的元素  例如 <br/>  分隔开始和结束的元素, 返回连续调用

Token 确保 StartElement 和EndElement tokens 它返回正确地嵌套和匹配:
如果Token 遇到未导出的最后元素,它会返回错误

Token实现XML命名空间, http://www.w3.org/TR/REC-xml-names/.
在Token中的每个Name结构体 包含 Space 设置URL 和确定的命名空间
如果Token 命名空间前缀未确认, 它使用Space前缀 而不是 报错
```


type Directive
```golang
type Directive []byte
	A Directive represents an XML directive of the form <!text>. The bytes do not include the <! and > markers.
	表示一个XML 形式的指令<!text>. 字节不包含 <! and >  标记
```

func (Directive) Copy
```golang
func (d Directive) Copy() Directive
```

type Encoder
```golang
```golang
type Encoder struct {
    // contains filtered or unexported fields
}
An Encoder writes XML data to an output stream.
写XML数据到输出流
```

Code:
```golang
type Address struct {
    City, State string
}
type Person struct {
    XMLName   xml.Name `xml:"person"`
    Id        int      `xml:"id,attr"`
    FirstName string   `xml:"name>first"`
    LastName  string   `xml:"name>last"`
    Age       int      `xml:"age"`
    Height    float32  `xml:"height,omitempty"`
    Married   bool
    Address
    Comment string `xml:",comment"`
}

v := &Person{Id: 13, FirstName: "John", LastName: "Doe", Age: 42}
v.Comment = " Need more details. "
v.Address = Address{"Hanga Roa", "Easter Island"}

enc := xml.NewEncoder(os.Stdout)
enc.Indent("  ", "    ")
if err := enc.Encode(v); err != nil {
    fmt.Printf("error: %v\n", err)
}
```
Output:
```golang
  <person id="13">
      <name>
          <first>John</first>
          <last>Doe</last>
      </name>
      <age>42</age>
      <Married>false</Married>
      <City>Hanga Roa</City>
      <State>Easter Island</State>
      <!-- Need more details. -->
  </person>
```
```


func NewEncoder
```golang
func NewEncoder(w io.Writer) *Encoder
	NewEncoder returns a new encoder that writes to w.
	返回一个新的写入 w的encoder  
```


func (*Encoder) Encode
```golang
func (enc *Encoder) Encode(v interface{}) error
	Encode writes the XML encoding of v to the stream.
	See the documentation for Marshal for details about the conversion of Go values to XML.
	Encode calls Flush before returning.
	写XML 编码v到 流
	查看文档 看Marshal 和 GO值转到XML 有关 的详细信息
	返回之前调用Flush
```

func (*Encoder) EncodeElement
```golang
func (enc *Encoder) EncodeElement(v interface{}, start StartElement) error
	EncodeElement writes the XML encoding of v to the stream, using start as the outermost tag in the encoding.
	See the documentation for Marshal for details about the conversion of Go values to XML.
	EncodeElement calls Flush before returning.
	写XML编码v 到流, 使用start作为编码最外面的标记
	查看文档 看Marshal 和 GO值转到XML 有关 的详细信息
	返回之前调用Flush
```

func (*Encoder) EncodeToken
```golang
func (enc *Encoder) EncodeToken(t Token) error
	EncodeToken writes the given XML token to the stream. 
	It returns an error if StartElement and EndElement tokens are not properly matched.

	EncodeToken does not call Flush, 
	because usually it is part of a larger operation such as Encode or EncodeElement (or a custom Marshaler's MarshalXML invoked during those), 
	and those will call Flush when finished.

	Callers that create an Encoder and then invoke EncodeToken directly, without using Encode or EncodeElement, 
	need to call Flush when finished to ensure that the XML is written to the underlying writer.
	
	把给定的XML token 写入流
	如果StartElement和EndElement tokens 没有正确匹配 返回错误
	
	EncodeToken不会调用Flush
	因为通常它是大数据操作的一部分 例如 Encode 或 EncodeElement(或者自定义的Marshaler的MarshalXML在这些调用),然后结束的时候会调用Flush
	
	调用者创建一个Encoder 然后直接调用EncodeToken, 没有使用Encode 或 EncodeElement, 当XML写入到底层的writer 结束时 必须调用Flush
```

func (*Encoder) Flush
```golang
func (enc *Encoder) Flush() error
	Flush flushes any buffered XML to the underlying writer. See the EncodeToken documentation for details about when it is necessary.
	书信任何缓冲区的XML到底层writer.
```

func (*Encoder) Indent
```golang
func (enc *Encoder) Indent(prefix, indent string)
	Indent sets the encoder to generate XML in which each element begins on a new indented line that starts with 
		prefix and is followed by one or more copies of indent according to the nesting depth.
	
	设置encoder 生成 XML 其中 每个元素 以 新的 indented 开始   以前缀和  跟着的 一个或多个 复制 对应的嵌套深度。
```

type EndElement
```golang
type EndElement struct {
    Name Name
}
An EndElement represents an XML end element.
表示一个XML 结束元素
```

type Marshaler
```golang
type Marshaler interface {
    MarshalXML(e *Encoder, start StartElement) error
}
Marshaler is the interface implemented by objects that can marshal themselves into valid XML elements.

MarshalXML encodes the receiver as zero or more XML elements. 
By convention, arrays or slices are typically encoded as a sequence of elements, one per entry.
Using start as the element tag is not required, but doing so will enable Unmarshal to match the XML elements to the correct struct field. 
One common implementation strategy is to construct a separate value with a layout corresponding to the desired XML and then to encode it using e.EncodeElement. 
Another common strategy is to use repeated calls to e.EncodeToken to generate the XML output one token at a time. 
The sequence of encoded tokens must make up zero or more valid XML elements.

Marshaler实现 可以 marshal他们本身 到 有效的XML元素的对象接口

MarshalXML编码接收者的一个或多个XML元素
按照惯例,数组和slice 通常编码元素序列 的每一个条目.
使用start tag元素不是必须的,但是这样做 将会让Unmarshal 匹配XML元素到正确的结构体 字段.
一个常见的实现方法是  构建一个单独的值 对应所需的XML布局 然后用e.EncodeElement编码
其他尝试的方法是 调用 e.EncodeToken生成XML 输出 一个token 一次
tokens 的编码序列必须 0或者多个有效的 XML元素组成

```

type MarshalerAttr
```golang
type MarshalerAttr interface {
    MarshalXMLAttr(name Name) (Attr, error)
}
	MarshalerAttr is the interface implemented by objects that can marshal themselves into valid XML attributes.
	MarshalXMLAttr returns an XML attribute with the encoded value of the receiver. 
	Using name as the attribute name is not required, but doing so will enable Unmarshal to match the attribute to the correct struct field. 
	If MarshalXMLAttr returns the zero attribute Attr{}, no attribute will be generated in the output. 
	MarshalXMLAttr is used only for struct fields with the "attr" option in the field tag.
	
	MarshalerAttr 是实现 可以解码自己 成有效的XML属性 的接口
	MarshalXMLAttr 从接收者的编码值 中返回XML属性
	属性名不是必须的,但是那样做能让Unmarshal 匹配属性到正确的结构体字段
```

type Name
```golang
type Name struct {
    Space, Local string
}
A Name represents an XML name (Local) annotated with a name space identifier (Space).
In tokens returned by Decoder.Token, the Space identifier is given as a canonical URL, not the short prefix used in the document being parsed.
表示一个XML 命名空间  注释命名
在tokens 中 用Decoder.Token返回 , Space标示是一个典型的URL,在文档 解析中没有短前缀
```

type ProcInst
```golang
type ProcInst struct {
    Target string
    Inst   []byte
}
A ProcInst represents an XML processing instruction of the form <?target inst?>
表示一个XML处理指令的形式 <?target inst?>
```

func (ProcInst) Copy
```golang
func (p ProcInst) Copy() ProcInst
```

type StartElement
```golang
type StartElement struct {
    Name Name
    Attr []Attr
}
A StartElement represents an XML start element.
表示一个XML开始元素
```

func (StartElement) Copy
```golang
func (e StartElement) Copy() StartElement
```

func (StartElement) End
```golang
func (e StartElement) End() EndElement
	End returns the corresponding XML end element.
	返回对应的XML结束元素
```

type SyntaxError
```golang
type SyntaxError struct {
    Msg  string
    Line int
}
A SyntaxError represents a syntax error in the XML input stream.
表示XML输入流的 语法错误
```

func (*SyntaxError) Error
```golang
	func (e *SyntaxError) Error() string
```

type TagPathError
```golang
type TagPathError struct {
    Struct       reflect.Type
    Field1, Tag1 string
    Field2, Tag2 string
}
A TagPathError represents an error in the unmarshalling process caused by the use of field tags with conflicting paths.
一个TagPathError代表因使用字段标识与路径冲突的解组过程中出现错误。
```

func (*TagPathError) Error
```golang
	func (e *TagPathError) Error() string
```

type Token
```golang
type Token interface{}
A Token is an interface holding one of the token types: StartElement, EndElement, CharData, Comment, ProcInst, or Directive.
Token 是 一个token类型的接口 : StartElement, EndElement, CharData, Comment, ProcInst, or Directive.
```

func CopyToken
```golang
func CopyToken(t Token) Token
	CopyToken returns a copy of a Token.
	返回一个复制的Token
```

type UnmarshalError
```golang
type UnmarshalError string
An UnmarshalError represents an error in the unmarshalling process.
解码处理过程中遇到的错误
```

func (UnmarshalError) Error
```golang
func (e UnmarshalError) Error() string
```

type Unmarshaler
```golang
type Unmarshaler interface {
    UnmarshalXML(d *Decoder, start StartElement) error
}
Unmarshaler is the interface implemented by objects that can unmarshal an XML element description of themselves.
UnmarshalXML decodes a single XML element beginning with the given start element. 
If it returns an error, the outer call to Unmarshal stops and returns that error. 
UnmarshalXML must consume exactly one XML element. 
One common implementation strategy is to unmarshal into a separate value with a layout matching the expected XML using d.DecodeElement, 
	and then to copy the data from that value into the receiver. 
Another common strategy is to use d.Token to process the XML object one token at a time. UnmarshalXML may not use d.RawToken.

Unmarshaler是实现 可以 解码XML元素说明的 接口对象
UnmarshalXML以start元素编码单个XML元素
如果返回错误,调用Unmarshal 停止并 返回错误.
UnmarshalXML必须消耗一个XML元素
一种常见的实现方式是 使用d.DecodeElement匹配确切的XML 布局  到一个单独的值,从那个值中复制数据到接收者
其他常见的方式是使用 d.Token 处理XML对象一次.UnmarshalXML也许不会用d.RawToken
```


type UnmarshalerAttr
```golang
type UnmarshalerAttr interface {
    UnmarshalXMLAttr(attr Attr) error
}
UnmarshalerAttr is the interface implemented by objects that can unmarshal an XML attribute description of themselves.

UnmarshalXMLAttr decodes a single XML attribute. If it returns an error, the outer call to Unmarshal stops and returns that error. 
UnmarshalXMLAttr is used only for struct fields with the "attr" option in the field tag.

实现解析XML属性说明的接口对象
UnmarshalXMLAttr解码单个XML属性.如果返回错误, 调用Unmarshal停止并返回错误
UnmarshalXMLAttr 在字段的tag 中 使用 "attr"选项
```

type UnsupportedTypeError
```golang
type UnsupportedTypeError struct {
    Type reflect.Type
}
A MarshalXMLError is returned when Marshal encounters a type that cannot be converted into XML.
MarshalXMLError返回Marshal遇到的 不能把类型转成XML的错误
```

func (*UnsupportedTypeError) Error
```golang
func (e *UnsupportedTypeError) Error() string
```


Bugs
```golang
☞ Mapping between XML elements and data structures is inherently flawed: 
		an XML element is an order-dependent collection of anonymous values, while a data structure is an order-independent collection of named values. 
		See package json for a textual representation more suitable to data structures.
在XML元素和结构体之间映射是天生有缺陷的:
		一个XML元素是一个顺序相关的集合 匿名值, 当一个结构体数据 是一个顺序集合的名称值.
```








