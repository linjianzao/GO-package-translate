包地址：http://golang.org/pkg/encoding/asn1/
```golang
Package asn1 implements parsing of DER-encoded ASN.1 data structures, as defined in ITU-T Rec X.690.
See also “A Layman's Guide to a Subset of ASN.1, BER, and DER,” http://luca.ntop.org/Teaching/Appunti/asn1.html.

解析DER-encoded ASN.1 结构的数据
```

func Marshal
```golang
func Marshal(val interface{}) ([]byte, error)
	Marshal returns the ASN.1 encoding of val.
	返回val用 ASN.1编码后的结果
```

func Unmarshal
```golang
func Unmarshal(b []byte, val interface{}) (rest []byte, err error)
	Unmarshal parses the DER-encoded ASN.1 data structure b and uses the reflect package to fill in an arbitrary value pointed at by val. 
		Because Unmarshal uses the reflect package, the structs being written to must use upper case field names.
	
	An ASN.1 INTEGER can be written to an int, int32, int64, or *big.Int (from the math/big package). 
		If the encoded value does not fit in the Go type, Unmarshal returns a parse error.
	
	An ASN.1 BIT STRING can be written to a BitString.
	
	An ASN.1 OCTET STRING can be written to a []byte.
	
	An ASN.1 OBJECT IDENTIFIER can be written to an ObjectIdentifier.
	
	An ASN.1 ENUMERATED can be written to an Enumerated.
	
	An ASN.1 UTCTIME or GENERALIZEDTIME can be written to a time.Time.
	
	An ASN.1 PrintableString or IA5String can be written to a string.
	
	Any of the above ASN.1 values can be written to an interface{}. 
		The value stored in the interface has the corresponding Go type. For integers, that type is int64.
	
	An ASN.1 SEQUENCE OF x or SET OF x can be written to a slice if an x can be written to the slice's element type.
	
	An ASN.1 SEQUENCE or SET can be written to a struct if each of the elements in the sequence can be written to the corresponding element in the struct.
	
	The following tags on struct fields have special meaning to Unmarshal:
	```golang
	optional		marks the field as ASN.1 OPTIONAL
	[explicit] tag:x	specifies the ASN.1 tag number; implies ASN.1 CONTEXT SPECIFIC
	default:x		sets the default value for optional integer fields
	```
	If the type of the first field of a structure is RawContent then the raw ASN1 contents of the struct will be stored in it.
	Other ASN.1 types are not supported; if it encounters them, Unmarshal returns a parse error.
	
	
	Unmarshal 解析DER-encoded ASN.1结构的数据b 使用reflect包 填写val指定的任意值
	
	ASN.1 INTEGER 可以写入 int, int32, int64, or *big.Int(从math/big包)，如果编码的值不是GO的类型，Unmarshal返回解析错误
	
	ASN.1 BIT STRING可以写入BitString
	
	ASN.1 OCTET STRING可以写入[]byte
	
	ASN.1 OBJECT IDENTIFIER 可以写入ObjectIdentifier
	
	ASN.1 ENUMERATED可以写入Enumerated
	
	ASN.1 UTCTIME or GENERALIZEDTIME 可以写入time.Time
	
	ASN.1 PrintableString or IA5String可以写入string
	
	任何ASN.1 以上的值可以写入interface{}. 值存储到接口里对应相应的GO类型。比如integers ，对应类型int64
	
	如果x 可以写入slice的元素类型那 ASN.1 SEQUENCE OF x or SET OF x 可以写入slice
	
	如果序列中的每个元素可以写入到struct对应的元素 那 ASN.1 SEQUENCE or SET可以写入到struct
	
	对结构域下面的标签有特殊意义：
	```golang
		optional  让字段变成ASN.1 OPTIONAL
		[explicit] tag:x  指定ASN.1标签的数量;意味着ASN.1 CONTEXT SPECIFIC
		default:x  设置integer字段的默认值
	```
	
	如果结构体第一个字段的类型是RawContent，会存储在struct里的ASN1。其他的ASN.1类型不支持;
	如果遇到不支持的，Unmarshal会返回解析错误。
```

func UnmarshalWithParams
```golang
func UnmarshalWithParams(b []byte, val interface{}, params string) (rest []byte, err error)
	UnmarshalWithParams allows field parameters to be specified for the top-level element. 
	The form of the params is the same as the field tags.
	
	UnmarshalWithParams允许字段参数指定 顶级元素
	参数的形式和字段标签一样
```

type BitString
```golang
type BitString struct {
    Bytes     []byte // bits packed into bytes.
    BitLength int    // length in bits.
}
BitString is the structure to use when you want an ASN.1 BIT STRING type. 
A bit string is padded up to the nearest byte in memory and the number of valid bits is recorded. Padding bits will be zero.

当你需要一个ASN.1 BIT STRING类型结构时， 用BitString
bit字符串会填补内存最近的一个字节，可用的字节数会被记录。填补的 bits 将会时0
```

func (BitString) At
```golang
func (b BitString) At(i int) int
	At returns the bit at the given index. If the index is out of range it returns false.
	使用给定的索引返回位。如果索引在范围之外 返回false
```

func (BitString) RightAlign
```golang
func (b BitString) RightAlign() []byte
	RightAlign returns a slice where the padding bits are at the beginning. 
	The slice may share memory with the BitString.
	在填充bits开始的时候 返回slice
	slice可能和BitString共享内存
```

type Enumerated
```golang
type Enumerated int
	An Enumerated is represented as a plain int.
	表示一个素数
```

type Flag
```golang
type Flag bool
	A Flag accepts any data and is set to true if present.
	如果存在的话，接收任何数据并设置成true
```

type ObjectIdentifier
```golang
type ObjectIdentifier []int
	An ObjectIdentifier represents an ASN.1 OBJECT IDENTIFIER.
	表示一个ASN.1 OBJECT IDENTIFIER.
```

func (ObjectIdentifier) Equal
```golang
func (oi ObjectIdentifier) Equal(other ObjectIdentifier) bool
	Equal reports whether oi and other represent the same identifier.
	io和其他表示符是否一样
```

type RawContent
```golang
type RawContent []byte
	RawContent is used to signal that the undecoded, DER data needs to be preserved for a struct. 
	To use it, the first field of the struct must have this type. It's an error for any of the other fields to have this type.
	RawContent 时一个未解码的信号，DER数据需要被保存为一个结构。
	要使用它，结构的第一个字段必须时这个类型。如果其他任何字段有这个类型那会 错误。
```

type RawValue
```golang
type RawValue struct {
    Class, Tag int
    IsCompound bool
    Bytes      []byte
    FullBytes  []byte // includes the tag and length
}
A RawValue represents an undecoded ASN.1 object.
表示一个未解码的ASN.1对象
```

type StructuralError
```golang
type StructuralError struct {
    Msg string
}
A StructuralError suggests that the ASN.1 data is valid, but the Go type which is receiving it doesn't match.
StructuralError 意味着 ASN.1数据是有效的，但是GO类型不匹配
```

func (StructuralError) Error
```golang
	func (e StructuralError) Error() string
```

type SyntaxError
```golang
type SyntaxError struct {
    Msg string
}
A SyntaxError suggests that the ASN.1 data is invalid.
表示ASN.1 数据是无效的
```

func (SyntaxError) Error
```golang
func (e SyntaxError) Error() string
```
