包地址：http://golang.org/pkg/debug/dwarf/
```golang
	Package dwarf provides access to DWARF debugging information loaded from executable files, 
	as defined in the DWARF 2.0 Standard at http://dwarfstd.org/doc/dwarf-2.0.0.pdf
	提供从执行文件中访问DWARF的debug信息
```

type AddrType
```golang
  type AddrType struct {
        BasicType
  }
  An AddrType represents a machine address type.
  AddrType表示一个机器地址类型
```

```golang
type ArrayType
  type ArrayType struct {
        CommonType
        Type          Type
        StrideBitSize int64 // if > 0, number of bits to hold each element
        Count         int64 // if == -1, an incomplete array, like char x[].
  }
  An ArrayType represents a fixed size array type.
  代表一个固定数组大小的类型
```

```golang
    func (t *ArrayType) Size() int64
```

```golang
    func (t *ArrayType) String() string
```


type Attr
```golang
  type Attr uint32
    An Attr identifies the attribute type in a DWARF Entry's Field.
    验证DWARF Entry's 的字段的属性类型
```

```golang
const (
    AttrSibling        Attr = 0x01
    AttrLocation       Attr = 0x02
    AttrName           Attr = 0x03
    AttrOrdering       Attr = 0x09
    AttrByteSize       Attr = 0x0B
    AttrBitOffset      Attr = 0x0C
    AttrBitSize        Attr = 0x0D
    AttrStmtList       Attr = 0x10
    AttrLowpc          Attr = 0x11
    AttrHighpc         Attr = 0x12
    AttrLanguage       Attr = 0x13
    AttrDiscr          Attr = 0x15
    AttrDiscrValue     Attr = 0x16
    AttrVisibility     Attr = 0x17
    AttrImport         Attr = 0x18
    AttrStringLength   Attr = 0x19
    AttrCommonRef      Attr = 0x1A
    AttrCompDir        Attr = 0x1B
    AttrConstValue     Attr = 0x1C
    AttrContainingType Attr = 0x1D
    AttrDefaultValue   Attr = 0x1E
    AttrInline         Attr = 0x20
    AttrIsOptional     Attr = 0x21
    AttrLowerBound     Attr = 0x22
    AttrProducer       Attr = 0x25
    AttrPrototyped     Attr = 0x27
    AttrReturnAddr     Attr = 0x2A
    AttrStartScope     Attr = 0x2C
    AttrStrideSize     Attr = 0x2E
    AttrUpperBound     Attr = 0x2F
    AttrAbstractOrigin Attr = 0x31
    AttrAccessibility  Attr = 0x32
    AttrAddrClass      Attr = 0x33
    AttrArtificial     Attr = 0x34
    AttrBaseTypes      Attr = 0x35
    AttrCalling        Attr = 0x36
    AttrCount          Attr = 0x37
    AttrDataMemberLoc  Attr = 0x38
    AttrDeclColumn     Attr = 0x39
    AttrDeclFile       Attr = 0x3A
    AttrDeclLine       Attr = 0x3B
    AttrDeclaration    Attr = 0x3C
    AttrDiscrList      Attr = 0x3D
    AttrEncoding       Attr = 0x3E
    AttrExternal       Attr = 0x3F
    AttrFrameBase      Attr = 0x40
    AttrFriend         Attr = 0x41
    AttrIdentifierCase Attr = 0x42
    AttrMacroInfo      Attr = 0x43
    AttrNamelistItem   Attr = 0x44
    AttrPriority       Attr = 0x45
    AttrSegment        Attr = 0x46
    AttrSpecification  Attr = 0x47
    AttrStaticLink     Attr = 0x48
    AttrType           Attr = 0x49
    AttrUseLocation    Attr = 0x4A
    AttrVarParam       Attr = 0x4B
    AttrVirtuality     Attr = 0x4C
    AttrVtableElemLoc  Attr = 0x4D
    AttrAllocated      Attr = 0x4E
    AttrAssociated     Attr = 0x4F
    AttrDataLocation   Attr = 0x50
    AttrStride         Attr = 0x51
    AttrEntrypc        Attr = 0x52
    AttrUseUTF8        Attr = 0x53
    AttrExtension      Attr = 0x54
    AttrRanges         Attr = 0x55
    AttrTrampoline     Attr = 0x56
    AttrCallColumn     Attr = 0x57
    AttrCallFile       Attr = 0x58
    AttrCallLine       Attr = 0x59
    AttrDescription    Attr = 0x5A
)
```

```golang
    func (a Attr) GoString() string
```

```golang
    func (a Attr) String() string
```

type BasicType
```golang
  type BasicType struct {
        CommonType
        BitSize   int64
        BitOffset int64
  }
  A BasicType holds fields common to all basic types.
  持有所有通用基本类型字段。
```

```golang
    func (b *BasicType) Basic() *BasicType
```

```golang    
    func (t *BasicType) String() string
```
    
type BoolType
```golang
  type BoolType struct {
        BasicType
  }
  A BoolType represents a boolean type.
  代表一个布尔类型
```

type CharType
```golang
  type CharType struct {
        BasicType
  }
  A CharType represents a signed character type.
  代表单个字符类型
```
  
type CommonType
```golang
  type CommonType struct {
        ByteSize int64  // size of value of this type, in bytes
        Name     string // name that can be used to refer to type
  }
  A CommonType holds fields common to multiple types. 
  If a field is not known or not applicable for a given type, the zero value is used.
  CommonType 多种类型的共同字段。如果字段未知或者么有给定类型 ， 就是零值。
```

func (*CommonType) Common
```golang  
    func (c *CommonType) Common() *CommonType
```

func (*CommonType) Size
```golang
    func (c *CommonType) Size() int64
```

type ComplexType
```golang
  type ComplexType struct {
        BasicType
  }
  A ComplexType represents a complex floating point type.
  代表一个复合浮点指针类型
```

type Data
```golang
  type Data struct {
        // contains filtered or unexported fields
  }
  Data represents the DWARF debugging information loaded from an executable file (for example, an ELF or Mach-O executable).
  代表 从执行的文件载入的 DWARF debugging 信息。
```

func New
```golang  
  func New(abbrev, aranges, frame, info, line, pubnames, ranges, str []byte) (*Data, error)
    New returns a new Data object initialized from the given parameters. Rather than calling this function directly, 
    	clients should typically use the DWARF method of the File type of the appropriate package debug/elf, debug/macho, or debug/pe.
    The []byte arguments are the data from the corresponding debug section in the object file; 
    	for example, for an ELF object, abbrev is the contents of the ".debug_abbrev" section.
     
     用给定的参数初始化一个新的Data对象。相当于直接调用这个函数，客户端通常 在文件类型适合的  debug/elf, debug/macho, or debug/pe 包 使用DWARF 方法
    []byte参数相应的debug 在对象文件中的数据。
    	例如，对于一个ELF对象，abbrev 是.debug_abbrev 章节的目录
```

func (*Data) Reader
```golang
  func (d *Data) Reader() *Reader
    Reader returns a new Reader for Data. 
    	The reader is positioned at byte offset 0 in the DWARF “info” section.
    从Data 返回一个新的Reader。在DWARF的 info 章节，reader 定位字节偏移0
```

func (*Data) Type
```golang
  func (d *Data) Type(off Offset) (Type, error)
```
  
type DecodeError
```golang
  type DecodeError struct {
        Name   string
        Offset Offset
        Err    string
```  

```golang
	func (e DecodeError) Error() string
```
	
type DotDotDotType
```golang
  type DotDotDotType struct {
        CommonType
  }
  A DotDotDotType represents the variadic ... function parameter.
  代表可变函数参数。
```  

```golang
    func (t *DotDotDotType) String() string
```

type Entry
```golang
  type Entry struct {
        Offset   Offset // offset of Entry in DWARF info
        Tag      Tag    // tag (kind of Entry)
        Children bool   // whether Entry is followed by children
        Field    []Field
  }
  An entry is a sequence of attribute/value pairs.
  是一对 属性/值 序列
```

func (*Entry) Val
```golang  
  func (e *Entry) Val(a Attr) interface{}
    Val returns the value associated with attribute Attr in Entry, or nil if there is no such attribute.
    A common idiom is to merge the check for nil return with the check that the value has the expected dynamic type, as in:
    v, ok := e.Val(AttrSibling).(int64);
    返回Entry里关联的属性Attr的值，如果没有属性是nil
    一个共同的 惯用语法 合并 nil检查 返回检查值是否是动态类型，在：  
```

type EnumType
```golang
  type EnumType struct {
        CommonType
        EnumName string
        Val      []*EnumValue
  }
  An EnumType represents an enumerated type. 
  The only indication of its native integer type is its ByteSize (inside CommonType).
  代表一个枚举类型。其原生整型类型的唯一标示是ByteSize(包含在CommonType里)。
```

```golang    
  func (t *EnumType) String() string
```  
  
type EnumValue
```golang
  type EnumValue struct {
        Name string
        Val  int64
  }
  An EnumValue represents a single enumeration value.
  代表单个枚举值。
```
  
type Field
```golang
  type Field struct {
        Attr Attr
        Val  interface{}
  }
  A Field is a single attribute/value pair in an Entry.
  在Entry 里单个属性/值 对
```
  
type FloatType
```golang
  type FloatType struct {
        BasicType
  }
  A FloatType represents a floating point type.
  代表一个浮点类型
```

type FuncType
```golang
  type FuncType struct {
        CommonType
        ReturnType Type
        ParamType  []Type
  }
  A FuncType represents a function type.
  代表函数类型
``` 

```golang
    func (t *FuncType) String() string
```    
    
type IntType
```golang
  type IntType struct {
        BasicType
  }
  An IntType represents a signed integer type.
  代表单个整型类型
```

type Offset
```golang
  type Offset uint32
  An Offset represents the location of an Entry within the DWARF info. (See Reader.Seek.)
  代表Entry位置的DWARF信息
```

type PtrType
```golang
  type PtrType struct {
        CommonType
        Type Type
  }
  A PtrType represents a pointer type.
  代表一个指针类型
```  

```golang
    func (t *PtrType) String() string
```    

type QualType
```golang
  type QualType struct {
        CommonType
        Qual string
        Type Type
  }
  A QualType represents a type that has the C/C++ "const", "restrict", or "volatile" qualifier.
  表示 一个 有 C/C++   "const", "restrict", or "volatile" 类型
```  

```golang
    func (t *QualType) Size() int64
```

```golang    
    func (t *QualType) String() string
```    
    
type Reader
```golang
  type Reader struct {
        // contains filtered or unexported fields
  }
  A Reader allows reading Entry structures from a DWARF “info” section. 
  The Entry structures are arranged in a tree. 
  The Reader's Next function return successive entries from a pre-order traversal of the tree. 
  If an entry has children, its Children field will be true, and the children follow, terminated by an Entry with Tag 0.
  允许从一个DWARF info 章节读取Entry结构。
  Entry结构是树形的。
  Reader 的Next函数 从树的前序遍历返回连续的条目。
  如果条目是子类，Children field 将是true， children 在Entry Tag 0 时终止。
```

```golang  
  func (r *Reader) Next() (*Entry, error)
    Next reads the next entry from the encoded entry stream. 
    It returns nil, nil when it reaches the end of the section. 
    It returns an error if the current offset is invalid or the data at the offset cannot be decoded as a valid Entry.
     从输入的编码流读取下一个输入。接收到最后的章节后 返回  nil,nil
     当然遇到偏移无效或者偏移的数据无法解析成有效的 Entry，返回错误
``` 

```golang
  func (r *Reader) Seek(off Offset)
    Seek positions the Reader at offset off in the encoded entry stream. Offset 0 can be used to denote the first entry.
    查看Reader在输入编码流偏移的位置。第一个输入的偏移0。
```

```golang    
  func (r *Reader) SkipChildren()
    SkipChildren skips over the child entries associated with the last Entry returned by Next. 
    If that Entry did not have children or Next has not been called, SkipChildren is a no-op.
    跳过最后输入关联的子条目 通过Next返回。
    如果Entry没有子条目或者Next 没有被调用，SkipChildren无操作
```
    
type StructField
```golang
  type StructField struct {
        Name       string
        Type       Type
        ByteOffset int64
        ByteSize   int64
        BitOffset  int64 // within the ByteSize bytes at ByteOffset
        BitSize    int64 // zero if not a bit field
  }
  A StructField represents a field in a struct, union, or C++ class type.
  代表一个结构、关联或者 C++类  类型的字段
```
  
type StructType
```golang
  type StructType struct {
        CommonType
        StructName string
        Kind       string // "struct", "union", or "class".
        Field      []*StructField
        Incomplete bool // if true, struct, union, class is declared but not defined
  }
  A StructType represents a struct, union, or C++ class type.
  代表一个结构、关联或者 C++类  类型
```

```golang  
    func (t *StructType) Defn() string
```   

```golang
    func (t *StructType) String() string
```    
    
type Tag
```golang
  type Tag uint32
  A Tag is the classification (the type) of an Entry.
  是一个输入的分类。
```

```golang  
    func (t Tag) GoString() string
```

```golang    
    func (t Tag) String() string
```    
    
type Type
```golang
  type Type interface {
        Common() *CommonType
        String() string
        Size() int64
  }
  A Type conventionally represents a pointer to any of the specific Type structures (CharType, StructType, etc.).
  代表任何特定类型的结构的指针
```

type TypedefType
```golang
  type TypedefType struct {
        CommonType
        Type Type
  }
  A TypedefType represents a named type.
  代表一个命名类型。
```

```golang
    func (t *TypedefType) Size() int64
```

```golang    
    func (t *TypedefType) String() string 
```   

type UcharType
```golang
  type UcharType struct {
        BasicType
  }
  A UcharType represents an unsigned character type.
  表示一个无符号的字符类型
```

type UintType
```golang
  type UintType struct {
        BasicType
  }
  A UintType represents an unsigned integer type.
  代表无符号整型
```
  
type VoidType
```golang
  type VoidType struct {
        CommonType
  }
  
  A VoidType represents the C void type.
  代表c void 类型
```

```golang    
    func (t *VoidType) String() string
```

Package files
buf.go const.go entry.go open.go type.go unit.go
