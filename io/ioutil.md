Package ioutil

Overview ▾

Package ioutil implements some I/O utility functions.
ioutil包实现了一些 I/O 实用功能。


Variables

var Discard io.Writer = devNull(0)
Discard is an io.Writer on which all Write calls succeed without doing anything.
Discard 是一个 所有Write调用成功 但没有做任何事情的 io.Writer

func NopCloser

func NopCloser(r io.Reader) io.ReadCloser
NopCloser returns a ReadCloser with a no-op Close method wrapping the provided Reader r.
NopCloser 返回一个 ReadCloser ,没有操作 Close 方法 包装 提供的 Reader  r

func ReadAll

func ReadAll(r io.Reader) ([]byte, error)
ReadAll reads from r until an error or EOF and returns the data it read. 
A successful call returns err == nil, not err == EOF. 
Because ReadAll is defined to read from src until EOF, it does not treat an EOF from Read as an error to be reported.
ReadAll 从r读取 直到遇到 错误或EOF 并且返回读取的数据
成功的调用 返回  err == nil 而不是  err == EOF. 
因为 ReadAll默认从src读取 到 EOF,它不会把 EOF当成错误报告



func ReadDir

func ReadDir(dirname string) ([]os.FileInfo, error)
ReadDir reads the directory named by dirname and returns a list of sorted directory entries.
ReadDir 用dirname 读取目录名并返回排序的目录条目列表

func ReadFile

func ReadFile(filename string) ([]byte, error)
ReadFile reads the file named by filename and returns the contents. 
A successful call returns err == nil, not err == EOF. 
Because ReadFile reads the whole file, it does not treat an EOF from Read as an error to be reported.
ReadFile 以filename 读取命名文件 并返回内容
一个成功的调用返回 err == nil 而不是 err == EOF. 
因为ReadFile 读取整个文件,它不会把 EOF当成错误报告


func TempDir

func TempDir(dir, prefix string) (name string, err error)
TempDir creates a new temporary directory in the directory dir with a name beginning with prefix and returns the path of the new directory. 
If dir is the empty string, TempDir uses the default directory for temporary files (see os.TempDir). 
Multiple programs calling TempDir simultaneously will not choose the same directory. 
It is the caller's responsibility to remove the directory when no longer needed.
TempDir在目录 dir中 以名字为prefix  创建一个新的临时目录并且返回那个新目录的路径.
如果dir是空的字符串,TempDir 为临时文件  使用默认的目录(查看os.TempDir).
多个程序同时调用TempDir不会选择相同的目录.
当没有更长的需要时,会负责删除目录


func TempFile

func TempFile(dir, prefix string) (f *os.File, err error)
TempFile creates a new temporary file in the directory dir with a name beginning with prefix, opens the file for reading and writing, and returns the resulting *os.File. 
If dir is the empty string, TempFile uses the default directory for temporary files (see os.TempDir). 
Multiple programs calling TempFile simultaneously will not choose the same file. 
The caller can use f.Name() to find the pathname of the file. 
It is the caller's responsibility to remove the file when no longer needed.
TempFile在目录 dir中  以名字为prefix  创建一个新的临时文件,打开文件 读和写, 并返回 *os.File 结果.
如果dir是个空字符串,TempFile 为临时文件 使用默认的目录
多个程序同时调用TempFile不会选择相同的文件.
调用者可以用f.Name()找到文件的路径名.
当没有更长的需要时,会负责删除目录


func WriteFile

func WriteFile(filename string, data []byte, perm os.FileMode) error
WriteFile writes data to a file named by filename. 
If the file does not exist, WriteFile creates it with permissions perm; otherwise WriteFile truncates it before writing.
WriteFile用filename 写入数据到一个 命名文件.
如果文件不存在,WriteFile创建一个perm权限的文件.否则WriteFile会在写入之前截断.











