import "archive/tar"

Package tar implements access to tar archives. It aims to cover most of the variations, including those produced by GNU and BSD tars.

实现访问tar归档文件。包含GNU和BSD的归档文件。
  
详情:

<http://www.freebsd.org/cgi/man.cgi?query=tar&sektion=5>
<http://www.gnu.org/software/tar/manual/html_node/Standard.html>
<http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html>


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



type Header
  type Header struct {
          Name       string    // name of header file entry 头文件名
          Mode       int64     // permission and mode bits 权限和模式位
          Uid        int       // user id of owner 用户id
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

func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)
    FileInfoHeader creates a partially-populated Header from fi. If fi describes a symlink, FileInfoHeader records link as the link target. If fi describes a directory, a slash is appended to the name.
    FileInfoHeader创建部分填充的头，如果是一个链接，返回链接对象，如果是一个目录，目录名加斜杠
    源码http://golang.org/src/pkg/archive/tar/common.go?s=4981:5046#L170
    
func (h *Header) FileInfo() os.FileInfo
  FileInfo returns an os.FileInfo for the Header.
  像os.FileInfo返回头信息



type Reader
  type Reader struct {
          // contains filtered or unexported fields 包涵过滤或取消导出字段
  }
  A Reader provides sequential access to the contents of a tar archive. A tar archive consists of a sequence of files. The Next method advances to the next file in the archive (including the first), and then it can be treated as an io.Reader to access the file's data.
  顺序访问tar归档包的目录。一个归档包由一系列文件组成。使用io.Reader的next方法访问下一个文件
  
func NewReader(r io.Reader) *Reader
  NewReader creates a new Reader reading from r.
  创建新的读取对象r

func (tr *Reader) Next() (*Header, error)
  Next advances to the next entry in the tar archive
  归档文件中读取下一个
  
func (tr *Reader) Read(b []byte) (n int, err error)
  Read reads from the current entry in the tar archive. It returns 0, io.EOF when it reaches the end of that entry, until Next is called to advance to the next entry.
  在包里读取一个确定的实体，当读取到文件末尾的时候返回0.
    
type Writer
  type Writer struct {
        // contains filtered or unexported fields 包涵过滤或取消导出字段
  }
  A Writer provides sequential writing of a tar archive in POSIX.1 format. A tar archive consists of a sequence of files. Call WriteHeader to begin a new file, and then call Write to supply that file's data, writing at most hdr.Size bytes in total.
  使用POSIX.1格式写入归档。一个归档文件，由一系列文件组成。调用WriteHeader开始写入文件，然后调用write写入数据，
  
func NewWriter(w io.Writer) *Writer
  NewWriter creates a new Writer writing to w.
  创建一个写入文件w
  
func (tw *Writer) Close() error
  Close closes the tar archive, flushing any unwritten data to the underlying writer.
  关闭归档，刷新任何写入的数据

func (tw *Writer) Flush() error
  Flush finishes writing the current file (optional).
  刷新文件
  
func (tw *Writer) Write(b []byte) (n int, err error)
  Write writes to the current entry in the tar archive. Write returns the error ErrWriteTooLong if more than hdr.Size bytes are written after WriteHeader.
  写入归档文件，当文件头大于hdr.Size，返回错误“写入文件太长”

func (tw *Writer) WriteHeader(hdr *Header) error
  WriteHeader writes hdr and prepares to accept the file's contents. WriteHeader calls Flush if it is not the first header. Calling after a Close will return ErrWriteAfterClose.
  编写访问目录，如果不是第一个标头调用刷新，在归档文件关闭的时候会返回错误“在关闭后写入”
