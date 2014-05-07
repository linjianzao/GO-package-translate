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

```golang
  func OpenReader(name string) (*ReadCloser, error)
    OpenReader will open the Zip file specified by name and return a ReadCloser.
    打开指定名称的zip文件，返回ReadCloser
```

```golang
  func (rc *ReadCloser) Close() error
    Close closes the Zip file, rendering it unusable for I/O.
    关闭zip文件
```

```golang
type Reader
  type Reader struct {
        File    []*File
        Comment string
        // contains filtered or unexported fields
  }
```
```golang
//例子
// Open a zip archive for reading.
r, err := zip.OpenReader("testdata/readme.zip")
if err != nil {
    log.Fatal(err)
}
defer r.Close()

// Iterate through the files in the archive,
// printing some of their contents.
for _, f := range r.File {
    fmt.Printf("Contents of %s:\n", f.Name)
    rc, err := f.Open()
    if err != nil {
        log.Fatal(err)
    }
    _, err = io.CopyN(os.Stdout, rc, 68)
    if err != nil {
        log.Fatal(err)
    }
    rc.Close()
    fmt.Println()
}
```
Output:
```golang
Contents of README:
This is the source code repository for the Go programming language.
```

```golang
  func NewReader(r io.ReaderAt, size int64) (*Reader, error)
    NewReader returns a new Reader reading from r, which is assumed to have the given size in bytes.
    返回给定size的新Reader
```

```golang    
type Writer
  type Writer struct {
    // contains filtered or unexported fields
  }
  Writer implements a zip file writer.
```

```golang
// Create a buffer to write our archive to.
buf := new(bytes.Buffer)

// Create a new zip archive.
w := zip.NewWriter(buf)

// Add some files to the archive.
var files = []struct {
    Name, Body string
}{
    {"readme.txt", "This archive contains some text files."},
    {"gopher.txt", "Gopher names:\nGeorge\nGeoffrey\nGonzo"},
    {"todo.txt", "Get animal handling licence.\nWrite more examples."},
}
for _, file := range files {
    f, err := w.Create(file.Name)
    if err != nil {
        log.Fatal(err)
    }
    _, err = f.Write([]byte(file.Body))
    if err != nil {
        log.Fatal(err)
    }
}

// Make sure to check the error on Close.
err := w.Close()
if err != nil {
    log.Fatal(err)
}
```

```golang
func NewWriter(w io.Writer) *Writer
NewWriter returns a new Writer writing a zip file to w.
返回一个新的Writer
```

```golang
func (w *Writer) Close() error
Close finishes writing the zip file by writing the central directory. It does not (and can not) close the underlying writer.
结束写入ZIP文件。它不会（也不能）关闭相关的writer
```

```golang
func (w *Writer) Create(name string) (io.Writer, error)
Create adds a file to the zip file using the provided name. It returns a Writer to which the file contents should be written. The name must be a relative path: it must not start with a drive letter (e.g. C:) or leading slash, and only forward slashes are allowed. The file's contents must be written to the io.Writer before the next call to Create, CreateHeader, or Close.
使用给定的name添加一个ZIP文件。文件可写入的情况下返回Writer。name必须是相对路径：不能以字母(e.g. C:) 或者前斜杠开头，只能用正斜杠。文件内容必须在 Create、CreateHeader 或 Close 前写入io.Writer
```

```golang
func (w *Writer) CreateHeader(fh *FileHeader) (io.Writer, error)
CreateHeader adds a file to the zip file using the provided FileHeader for the file metadata. It returns a Writer to which the file contents should be written. The file's contents must be written to the io.Writer before the next call to Create, CreateHeader, or Close.
使用FileHeader的文件元数据 添加一个ZIP文件。文件可写入的情况下返回Writer.文件内容必须在 Create、CreateHeader 或 Close 前写入io.Writer
```