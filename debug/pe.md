包地址：http://golang.org/pkg/debug/pe/
```golang
Package pe implements access to PE (Microsoft Windows Portable Executable) files.
pe包实现访问 PE文件
```

Constants
```golang
  const (
        IMAGE_FILE_MACHINE_UNKNOWN   = 0x0
        IMAGE_FILE_MACHINE_AM33      = 0x1d3
        IMAGE_FILE_MACHINE_AMD64     = 0x8664
        IMAGE_FILE_MACHINE_ARM       = 0x1c0
        IMAGE_FILE_MACHINE_EBC       = 0xebc
        IMAGE_FILE_MACHINE_I386      = 0x14c
        IMAGE_FILE_MACHINE_IA64      = 0x200
        IMAGE_FILE_MACHINE_M32R      = 0x9041
        IMAGE_FILE_MACHINE_MIPS16    = 0x266
        IMAGE_FILE_MACHINE_MIPSFPU   = 0x366
        IMAGE_FILE_MACHINE_MIPSFPU16 = 0x466
        IMAGE_FILE_MACHINE_POWERPC   = 0x1f0
        IMAGE_FILE_MACHINE_POWERPCFP = 0x1f1
        IMAGE_FILE_MACHINE_R4000     = 0x166
        IMAGE_FILE_MACHINE_SH3       = 0x1a2
        IMAGE_FILE_MACHINE_SH3DSP    = 0x1a3
        IMAGE_FILE_MACHINE_SH4       = 0x1a6
        IMAGE_FILE_MACHINE_SH5       = 0x1a8
        IMAGE_FILE_MACHINE_THUMB     = 0x1c2
        IMAGE_FILE_MACHINE_WCEMIPSV2 = 0x169
  )
  const COFFSymbolSize = 18
```  
  
type COFFSymbol
```golang 
  type COFFSymbol struct {
        Name               [8]uint8
        Value              uint32
        SectionNumber      int16
        Type               uint16
        StorageClass       uint8
        NumberOfAuxSymbols uint8
  }
```
  
type File
```golang
  type File struct {
        FileHeader
        Sections []*Section
        Symbols  []*Symbol
        // contains filtered or unexported fields
  }
  A File represents an open PE file.
  代表打开一个PE文件
```

func NewFile
```golang  
  func NewFile(r io.ReaderAt) (*File, error)
    NewFile creates a new File for accessing a PE binary in an underlying reader.
    在底层的reader创建一个新文件接收PE的二进制文件
```

func Open  
```golang  
  func Open(name string) (*File, error)
    Open opens the named file using os.Open and prepares it for use as a PE binary.
    使用os.Open  打开一个命名文件并用PE binary准备。
```

func (*File) Close
```golang    
  func (f *File) Close() error
    Close closes the File. If the File was created using NewFile directly instead of Open, Close has no effect.
    关闭File。如果File使用NewFile直接创建用Open代替，Close没有影响
```

func (*File) DWARF
```golang    
  func (f *File) DWARF() (*dwarf.Data, error)
```  
  
func (*File) ImportedLibraries
```golang  
  func (f *File) ImportedLibraries() ([]string, error)
    ImportedLibraries returns the names of all libraries referred to by the binary f that are expected to be linked with the binary at dynamic link time.
    动态链接二进制时返回二进制f 中的所有 类库。
```

func (*File) ImportedSymbols
```golang  
  func (f *File) ImportedSymbols() ([]string, error)
    ImportedSymbols returns the names of all symbols referred to by the binary f that are expected to be satisfied by other libraries at dynamic load time. 
    It does not return weak symbols.
 ```   

func (*File) Section    
```golang
  func (f *File) Section(name string) *Section
    Section returns the first section with the given name, or nil if no such section exists.
    使用给定的名称返回第一节。如果节不存在返回nil
```
    
type FileHeader
```golang
  type FileHeader struct {
        Machine              uint16
        NumberOfSections     uint16
        TimeDateStamp        uint32
        PointerToSymbolTable uint32
        NumberOfSymbols      uint32
        SizeOfOptionalHeader uint16
        Characteristics      uint16
  }
```
  
type FormatError
```golang
  type FormatError struct {
        // contains filtered or unexported fields
  }
```

func (*FormatError) Error
```golang  
    func (e *FormatError) Error() string
```
    
type ImportDirectory
```golang  
  type ImportDirectory struct {
        OriginalFirstThunk uint32
        TimeDateStamp      uint32
        ForwarderChain     uint32
        Name               uint32
        FirstThunk         uint32
        // contains filtered or unexported fields
  }
```
  
type Section
```golang  
  type Section struct {
        SectionHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        io.ReaderAt
        // contains filtered or unexported fields
  }
```  

func (*Section) Data
```golang
  func (s *Section) Data() ([]byte, error)
    Data reads and returns the contents of the PE section.
    读取和返回PE节的内容
```

func (*Section) Open    
```golang
  func (s *Section) Open() io.ReadSeeker
    Open returns a new ReadSeeker reading the PE section.
    返回一个新的读取PE节的ReadSeeker
```
    
type SectionHeader
```golang
  type SectionHeader struct {
        Name                 string
        VirtualSize          uint32
        VirtualAddress       uint32
        Size                 uint32
        Offset               uint32
        PointerToRelocations uint32
        PointerToLineNumbers uint32
        NumberOfRelocations  uint16
        NumberOfLineNumbers  uint16
        Characteristics      uint32
  }
```

type SectionHeader32
```golang
  type SectionHeader32 struct {
        Name                 [8]uint8
        VirtualSize          uint32
        VirtualAddress       uint32
        SizeOfRawData        uint32
        PointerToRawData     uint32
        PointerToRelocations uint32
        PointerToLineNumbers uint32
        NumberOfRelocations  uint16
        NumberOfLineNumbers  uint16
        Characteristics      uint32
  }
```
  
type Symbol
```golang
  type Symbol struct {
        Name          string
        Value         uint32
        SectionNumber int16
        Type          uint16
        StorageClass  uint8
}
```
