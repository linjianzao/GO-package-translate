import "archive/zip"

Package zip provides support for reading and writing ZIP archives.
读取和写入 ZIP归档

See: http://www.pkware.com/documents/casestudies/APPNOTE.TXT

This package does not support disk spanning.
这个包不支持磁盘跨越

A note about ZIP64:
To be backwards compatible the FileHeader has both 32 and 64 bit Size fields. The 64 bit fields will always contain the correct value and for normal archives both fields will be the same. For files requiring the ZIP64 format the 32 bit fields will be 0xffffffff and the 64 bit fields must be used instead.
ZIP64备注:为了向后兼容的FileHeader里有32位和64位字段。64位包含的值和归档总是正确的。

Constants
```golang
const (
    Store   uint16 = 0
    Deflate uint16 = 8
)
Compression methods.
```
Variables
```golang
var (
    ErrFormat    = errors.New("zip: not a valid zip file")
    ErrAlgorithm = errors.New("zip: unsupported compression algorithm")
    ErrChecksum  = errors.New("zip: checksum error")
)
```

```golang
func RegisterCompressor

func RegisterCompressor(method uint16, comp Compressor)
RegisterCompressor registers custom compressors for a specified method ID. The common methods Store and Deflate are built in.
注册自定义压缩方法ID。这是内建方法
```

```golang
func RegisterDecompressor

func RegisterDecompressor(method uint16, d Decompressor)
RegisterDecompressor allows custom decompressors for a specified method ID.
允许自定义解压方法并指定ID
```

```golang
type Compressor

type Compressor func(io.Writer) (io.WriteCloser, error)
A Compressor returns a compressing writer, writing to the provided writer. On Close, any pending data should be flushed.
返回一个写入压缩，写入writer。Close的时候 任何写入的数据都被刷新
```

```golang
type Decompressor

type Decompressor func(io.Reader) io.ReadCloser
Decompressor is a function that wraps a Reader with a decompressing Reader. The decompressed ReadCloser is returned to callers who open files from within the archive. These callers are responsible for closing this reader when they're finished reading.
Decompressor 是一个包含解压的Reader的方法。解压后的ReadCloser返回给归档内打开的文件。这些调用当他们结束读取的时候会关闭
```

```golang
type File
  type File struct {
        FileHeader
        // contains filtered or unexported fields
  }
```

```golang
func (*File) DataOffset

func (f *File) DataOffset() (offset int64, err error)
  DataOffset returns the offset of the file's possibly-compressed data, relative to the beginning of the zip file.
  Most callers should instead use Open, which transparently decompresses data and verifies checksums.
  返回相对ZIP文件的开头位移可能的压缩数据。大部分解压数据和验证数据应该用这个代替Open
```

```golang
func (f *File) Open() (rc io.ReadCloser, err error)
    Open returns a ReadCloser that provides access to the File's contents. Multiple files may be read concurrently.
    返回可访问文件目录的对象，多个文件可以同时访问
```

```golang  
type FileHeader
  type FileHeader struct {
        // Name is the name of the file. 文件名称
        // It must be a relative path: it must not start with a drive 必须是一个真实存在的文件，必须处于未打开的状态
        // letter (e.g. C:) or leading slash, and only forward slashes  （就是相当于linux和windows目录开头）
        // are allowed.
        Name string

        CreatorVersion     uint16
        ReaderVersion      uint16
        Flags              uint16
        Method             uint16
        ModifiedTime       uint16 // MS-DOS time dos的时间
        ModifiedDate       uint16 // MS-DOS date dos的数据
        CRC32              uint32
        CompressedSize     uint32 // deprecated; use CompressedSize64 
        UncompressedSize   uint32 // deprecated; use UncompressedSize64
        CompressedSize64   uint64
        UncompressedSize64 uint64
        Extra              []byte
        ExternalAttrs      uint32 // Meaning depends on CreatorVersion
        Comment            string
  }
  FileHeader describes a file within a zip file. See the zip spec for details.
  FileHeader描述zip的文件信息
```

```golang
  func FileInfoHeader(fi os.FileInfo) (*FileHeader, error)
    FileInfoHeader creates a partially-populated FileHeader from an os.FileInfo.
    从一个os.FileInfo创建部分填充的FileHeader
```

```golang
  func (h *FileHeader) FileInfo() os.FileInfo
    FileInfo returns an os.FileInfo for the FileHeader.
    从FileHeader返回一个os.FileInfo
```

```golang
  func (h *FileHeader) ModTime() time.Time
    ModTime returns the modification time. The resolution is 2s.
    返回文件修改时间，间隔2S
```

```golang
  func (h *FileHeader) Mode() (mode os.FileMode)
    Mode returns the permission and mode bits for the FileHeader.
    返回FileHeader的权限和位模式
```    

```golang
  func (h *FileHeader) SetModTime(t time.Time)
    SetModTime sets the ModifiedTime and ModifiedDate fields to the given time. The resolution is 2s.
    使用给定的时间设置ModifiedTime和ModifiedDate字段。
```

```golang
  func (h *FileHeader) SetMode(mode os.FileMode)
    SetMode changes the permission and mode bits for the FileHeader.
    修改FileHeader的权限和位模式
```    

```golang    
type ReadCloser
  type ReadCloser struct {
        Reader
        // contains filtered or unexported fields
  }
```

  
  func OpenReader(name string) (*ReadCloser, error)
    OpenReader will open the Zip file specified by name and return a ReadCloser.
    根据文件名打开zip文件，返回一个类似句柄的东西
    
  func (rc *ReadCloser) Close() error
    Close closes the Zip file, rendering it unusable for I/O.
    关闭zip文件
    
type Reader
  type Reader struct {
        File    []*File
        Comment string
        // contains filtered or unexported fields
  }
  func NewReader(r io.ReaderAt, size int64) (*Reader, error)
    NewReader returns a new Reader reading from r, which is assumed to have the given size in bytes.
    假定有给定大小的字符，返回指针r，
    
type Writer
    func NewWriter(w io.Writer) *Writer
    func (w *Writer) Close() error
    func (w *Writer) Create(name string) (io.Writer, error)
    func (w *Writer) CreateHeader(fh *FileHeader) (io.Writer, error)
