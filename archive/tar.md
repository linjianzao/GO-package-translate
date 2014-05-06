import "archive/tar"

Package tar implements access to tar archives. It aims to cover most of the variations, including those produced by GNU and BSD tars.

实现访问tar归档文件。包含GNU和BSD的归档文件。
  
详情:
```
http://www.freebsd.org/cgi/man.cgi?query=tar&sektion=5
http://www.gnu.org/software/tar/manual/html_node/Standard.html
http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html
```

  例子:
  
```golang
  // Create a buffer to write our archive to.
  buf := new(bytes.Buffer)

  // Create a new tar archive.
  tw := tar.NewWriter(buf)

  // Add some files to the archive.
  var files = []struct {
      Name, Body string
  }{
      {"readme.txt", "This archive contains some text files."},
      {"gopher.txt", "Gopher names:\nGeorge\nGeoffrey\nGonzo"},
      {"todo.txt", "Get animal handling licence."},
  }
  for _, file := range files {
      hdr := &tar.Header{
          Name: file.Name,
          Size: int64(len(file.Body)),
      }
      if err := tw.WriteHeader(hdr); err != nil {
          log.Fatalln(err)
      }
      if _, err := tw.Write([]byte(file.Body)); err != nil {
          log.Fatalln(err)
      }
  }
  // Make sure to check the error on Close.
  if err := tw.Close(); err != nil {
      log.Fatalln(err)
  }

  // Open the tar archive for reading.
  r := bytes.NewReader(buf.Bytes())
  tr := tar.NewReader(r)

  // Iterate through the files in the archive.
  for {
      hdr, err := tr.Next()
      if err == io.EOF {
          // end of tar archive
          break
      }
      if err != nil {
          log.Fatalln(err)
      }
      fmt.Printf("Contents of %s:\n", hdr.Name)
      if _, err := io.Copy(os.Stdout, tr); err != nil {
          log.Fatalln(err)
      }
      fmt.Println()
  }
```
  输出：
  <pre>
    Contents of readme.txt:
    This archive contains some text files.
    Contents of gopher.txt:
    Gopher names:
    George
    Geoffrey
    Gonzo
    Contents of todo.txt:
    Get animal handling licence.  
  </pre>


Constants
```golang
const (

    // Types
    TypeReg           = '0'    // regular file
    TypeRegA          = '\x00' // regular file
    TypeLink          = '1'    // hard link
    TypeSymlink       = '2'    // symbolic link
    TypeChar          = '3'    // character device node
    TypeBlock         = '4'    // block device node
    TypeDir           = '5'    // directory
    TypeFifo          = '6'    // fifo node
    TypeCont          = '7'    // reserved
    TypeXHeader       = 'x'    // extended header
    TypeXGlobalHeader = 'g'    // global extended header
    TypeGNULongName   = 'L'    // Next file has a long name
    TypeGNULongLink   = 'K'    // Next file symlinks to a file w/ a long name
)  
```

Variables
```golang
var (
    ErrWriteTooLong    = errors.New("archive/tar: write too long")
    ErrFieldTooLong    = errors.New("archive/tar: header field too long")
    ErrWriteAfterClose = errors.New("archive/tar: write after close")
)
var (
    ErrHeader = errors.New("archive/tar: invalid tar header")
)
```

```golang
type Header
  type Header struct {
          Name       string    // name of header file entry 头文件名
          Mode       int64     // permission and mode bits 权限和模式位
          Uid        int       // user id of owner linux下的Uid
          Gid        int       // group id of owner 用户组
          Size       int64     // length in bytes 字节长度
          ModTime    time.Time // modified time 修改时间
          Typeflag   byte      // type of header entry 头文件类型
          Linkname   string    // target name of link 链接地址
          Uname      string    // user name of owner 用户名
          Gname      string    // group name of owner 用户组名
          Devmajor   int64     // major number of character or block device 主要的字符或者块设备
          Devminor   int64     // minor number of character or block device 次要的字符或者块设备
          AccessTime time.Time // access time 访问时间
          ChangeTime time.Time // status change time 状态更改时间
  }
  A Header represents a single header in a tar archive. Some fields may not be populated.
  Header表示一个tar文档的头信息。 字段不用都填
```


```golang
func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)
    FileInfoHeader creates a partially-populated Header from fi. If fi describes a symlink, FileInfoHeader records link as the link target. If fi describes a directory, a slash is appended to the name. Because os.FileInfo's Name method returns only the base name of the file it describes, it may be necessary to modify the Name field of the returned header to provide the full path name of the file.
    FileInfoHeader创建部分填充的头，如果是一个链接，返回链接对象，如果是一个目录，目录名加斜杠。因为os.FileInfo的 Name方法 返回最基本的文件名，要修改头的Name字段必须使用完整的路径名称
```

```golang
func (h *Header) FileInfo() os.FileInfo
  FileInfo returns an os.FileInfo for the Header.
  返回一个os.FileInfo
```

```golang
type Reader
  type Reader struct {
          // contains filtered or unexported fields 包涵过滤或取消导出字段
  }
  A Reader provides sequential access to the contents of a tar archive. A tar archive consists of a sequence of files. The Next method advances to the next file in the archive (including the first), and then it can be treated as an io.Reader to access the file's data.
  顺序访问tar归档包的目录。一个归档包由一系列文件组成。Next方法访问归档文件中的下一个文件（包含第一个文件）然后它可以像io.Reader一样访问文件的数据
```

```golang
func NewReader(r io.Reader) *Reader
  NewReader creates a new Reader reading from r.
  从r中创建新的Reader 对象
```

```golang
func (tr *Reader) Next() (*Header, error)
  Next advances to the next entry in the tar archive
  归档文件中下一个条目。注：跳到tar文件的下一个条目
```

```golang
func (tr *Reader) Read(b []byte) (n int, err error)
  Read reads from the current entry in the tar archive. It returns 0, io.EOF when it reaches the end of that entry, until Next is called to advance to the next entry.
  从tar归档中读取确定的条目。直到调用Next读取到文档末尾的时候返回0.
 ```

```golang   
type Writer
  type Writer struct {
        // contains filtered or unexported fields 包涵过滤或取消导出字段
  }
  A Writer provides sequential writing of a tar archive in POSIX.1 format. A tar archive consists of a sequence of files. Call WriteHeader to begin a new file, and then call Write to supply that file's data, writing at most hdr.Size bytes in total.
  使用POSIX1格式顺序写入归档。一个归档文件，由一系列文件组成。调用WriteHeader开始一个新文件，然后调用write写入数据，总共写入hdr.Size长度。
```

```golang 
func NewWriter(w io.Writer) *Writer
  NewWriter creates a new Writer writing to w.
  创建一个新的Writer对象w。
```

```golang
func (tw *Writer) Close() error
  Close closes the tar archive, flushing any unwritten data to the underlying writer.
  关闭归档，刷新任何未写入的数据到底层writer
```

```golang
func (tw *Writer) Flush() error
  Flush finishes writing the current file (optional).
  刷新写入的文件
```

```golang
func (tw *Writer) Write(b []byte) (n int, err error)
  Write writes to the current entry in the tar archive. Write returns the error ErrWriteTooLong if more than hdr.Size bytes are written after WriteHeader.
  写入归档文件，当文件头大于hdr.Size，返回错误“写入文件太长”
```

```golang
func (tw *Writer) WriteHeader(hdr *Header) error
  WriteHeader writes hdr and prepares to accept the file's contents. WriteHeader calls Flush if it is not the first header. Calling after a Close will return ErrWriteAfterClose.
  写入hdr 准备访问文件的内容，如果不是第一个头就调用Flush，在归档文件返回 ErrWriteAfterClose之后调用Close方法
```