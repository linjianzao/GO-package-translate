Package os

Overview ▾

Package os provides a platform-independent interface to operating system functionality. 
The design is Unix-like, although the error handling is Go-like; failing calls return values of type error rather than error numbers. 
Often, more information is available within the error. 
For example, if a call that takes a file name fails, such as Open or Stat, 
	the error will include the failing file name when printed and will be of type *PathError, which may be unpacked for more information.

The os interface is intended to be uniform across all operating systems. Features not generally available appear in the system-specific package syscall.

Here is a simple example, opening a file and reading some of it.

os包提供一种与平台无关的接口 来操作 系统的功能.
该设计是类Unix,虽然错误处理是类Go;失败的调用返回的错误类型值，而不是错误的数字。
经常, 更多 信息 在错误里.
例如, 如果一个带文件名的调用失败, 如 Open 或 Stat, 错误 打印的时候将会包含 文件名 并  将会  *PathError类型,可用于解包的更多信息。
该os接口是 拟在所有操作系统上均匀。 一般不提供的功能出现在系统特定的软件包系统调用。
这有个简单的例子,打开一个文件 并读取一些.

```golang
file, err := os.Open("file.go") // For read access.
if err != nil {
	log.Fatal(err)
}
```

If the open fails, the error string will be self-explanatory, like
如果打开失败, 错误字符串将不言自明,如

```golang
open file.go: no such file or directory
```

The file's data can then be read into a slice of bytes. Read and Write take their byte counts from the length of the argument slice.
文件数据可以读取到一个 字节slice.Read 和 Write  从 参数slice 长度 获取他们的字节数。

```golang
data := make([]byte, 100)
count, err := file.Read(data)
if err != nil {
	log.Fatal(err)
}
fmt.Printf("read %d bytes: %q\n", count, data[:count])
```


Constants
```golang
const (
        O_RDONLY int = syscall.O_RDONLY // open the file read-only.  打开只读文件
        O_WRONLY int = syscall.O_WRONLY // open the file write-only. 打开只写文件
        O_RDWR   int = syscall.O_RDWR   // open the file read-write. 打开读写文件
        O_APPEND int = syscall.O_APPEND // append data to the file when writing. 写入时追加数据到文件
        O_CREATE int = syscall.O_CREAT  // create a new file if none exists. 如果不存在 创建一个新文件
        O_EXCL   int = syscall.O_EXCL   // used with O_CREATE, file must not exist 使用O_CREATE ,文件必须不存在
        O_SYNC   int = syscall.O_SYNC   // open for synchronous I/O. 打开同步I/O
        O_TRUNC  int = syscall.O_TRUNC  // if possible, truncate file when opened. 如果可能 打开后截短文件.
)
```
Flags to Open wrapping those of the underlying system. Not all flags may be implemented on a given system.
Flags Open 封装的系统底层. 在一个给定的系统上可能不会实现所有的 flags

```golang
const (
        SEEK_SET int = 0 // seek relative to the origin of the file
        SEEK_CUR int = 1 // seek relative to the current offset
        SEEK_END int = 2 // seek relative to the end
)
```
Seek whence values.
```golang
const (
        PathSeparator     = '/' // OS-specific path separator
        PathListSeparator = ':' // OS-specific path list separator
)
```

```golang
const DevNull = "/dev/null"
```

DevNull is the name of the operating system's “null device.” On Unix-like systems, it is "/dev/null"; on Windows, "NUL".
DevNull 是在 类Unix 操作系统的“null device.” 名字, 它是"/dev/null";  在Windows, "NUL".


Variables
```golang
var (
        ErrInvalid    = errors.New("invalid argument")
        ErrPermission = errors.New("permission denied")
        ErrExist      = errors.New("file already exists")
        ErrNotExist   = errors.New("file does not exist")
)
```
Portable analogs of some common system call errors.
一些常见的系统调用错误

```golang
var (
        Stdin  = NewFile(uintptr(syscall.Stdin), "/dev/stdin")
        Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
        Stderr = NewFile(uintptr(syscall.Stderr), "/dev/stderr")
)
```
Stdin, Stdout, and Stderr are open Files pointing to the standard input, standard output, and standard error file descriptors.
Stdin, Stdout, 和 Stderr 打开  Files指向 标准输入, 标准输出,和文件描述的标准错误.

```golang
var Args []string
```
Args hold the command-line arguments, starting with the program name.
Args 保留命令行参数, 以程序名开始.


func Chdir
```golang
func Chdir(dir string) error
```
Chdir changes the current working directory to the named directory. 
If there is an error, it will be of type *PathError.
Chdir 修改当前的工作目录 为指定的目录
如果这有错误,这将是 *PathError 类型.



func Chmod
```golang
func Chmod(name string, mode FileMode) error
```
Chmod changes the mode of the named file to mode. 
If the file is a symbolic link, it changes the mode of the link's target. 
If there is an error, it will be of type *PathError.
Chmod 修改命令文件的模式 为 mode
如果文件是一个符号链接. 它修改链接目标的模式.
如果这有错误,这将是 *PathError 类型.



func Chown
```golang
func Chown(name string, uid, gid int) error
```
Chown changes the numeric uid and gid of the named file. 
If the file is a symbolic link, it changes the uid and gid of the link's target. 
If there is an error, it will be of type *PathError.
Chown 修改指定文件的 uid 和 gid 号.
如果文件是一个符号链接. 它修改链接目标的uid和gid.
如果这有错误,这将是 *PathError 类型.



func Chtimes
```golang
func Chtimes(name string, atime time.Time, mtime time.Time) error
```
Chtimes changes the access and modification times of the named file, similar to the Unix utime() or utimes() functions.

The underlying filesystem may truncate or round the values to a less precise time unit. If there is an error, it will be of type *PathError.
Chtimes修改指定文件 访问和修改时间, 类似Unix utime() 或 utimes() 函数.
底层文件系统 可能截短 或 以一个不太精确的时间单位 接近值.如果有错误,这将是 *PathError 类型.



func Clearenv
```golang
func Clearenv()
```
Clearenv deletes all environment variables.
Clearenv 删除所有的环境变量



func Environ
```golang
func Environ() []string
```
Environ returns a copy of strings representing the environment, in the form "key=value".
Environ 返回一个复制字符串表示环境,形式是"key=value".



func Exit
```golang
func Exit(code int)
```
Exit causes the current program to exit with the given status code. 
Conventionally, code zero indicates success, non-zero an error. 
The program terminates immediately; deferred functions are not run.
Exit 用给定的状态码退出当前的程序.
通常,代码零表示成功,非零表示错误
该程序立即终止;延迟函数不会执行.



func Expand
```golang
func Expand(s string, mapping func(string) string) string
```
Expand replaces ${var} or $var in the string based on the mapping function. 
For example, os.ExpandEnv(s) is equivalent to os.Expand(s, os.Getenv).
Expand 替换 基于映射函数在字符串中的${var} 或 $var
例如,os.ExpandEnv(s) 等价于 os.Expand(s, os.Getenv).



func ExpandEnv
```golang
func ExpandEnv(s string) string
```
ExpandEnv replaces ${var} or $var in the string according to the values of the current environment variables. 
References to undefined variables are replaced by the empty string.
ExpandEnv 替换 当前环境变量的字符串值 ${var} or $var
引用 undefined变量 替换 成 空字符串


func Getegid
```golang
func Getegid() int
```
Getegid returns the numeric effective group id of the caller.
Getegid 返回调用者 有效的组id号



func Getenv
```golang
func Getenv(key string) string
```
Getenv retrieves the value of the environment variable named by the key. It returns the value, which will be empty if the variable is not present.
Getenv 根据key检索 环境变量的值.它返回该值,其中 如果 变量未找到 将是空的.



func Geteuid
```golang
func Geteuid() int
```
Geteuid returns the numeric effective user id of the caller.
Geteuid返回调用者有效的用户id号.



func Getgid
```golang
func Getgid() int
```
Getgid returns the numeric group id of the caller.
Getgid 返回 调用者的组id号.



func Getgroups
```golang
func Getgroups() ([]int, error)
```
Getgroups returns a list of the numeric ids of groups that the caller belongs to.
Getgroups 返回  调用者属于的组id号列表


func Getpagesize
```golang
func Getpagesize() int
```
Getpagesize returns the underlying system's memory page size.
Getpagesize 返回底层系统的内存页 大小.



func Getpid
```golang
func Getpid() int
```
Getpid returns the process id of the caller.
Getpid 返回调用者的进程id



func Getppid
```golang
func Getppid() int
```
Getppid returns the process id of the caller's parent.
Getppid 返回调用者的父进程id.



func Getuid
```golang
func Getuid() int
```
Getuid returns the numeric user id of the caller.
Getuid 返回调用者的用户id号


func Getwd
```golang
func Getwd() (dir string, err error)
```
Getwd returns a rooted path name corresponding to the current directory. 
If the current directory can be reached via multiple paths (due to symbolic links), Getwd may return any one of them.
Getwd 返回对应的一个 根路径名到当前目录
如果当前目录 可以 通过多条路径到达(符号链接),Getwd可能会返回他们中的一个.



func Hostname
```golang
func Hostname() (name string, err error)
```
Hostname returns the host name reported by the kernel.
Hostname 返回通过内核报告的主机名


func IsExist
```golang
func IsExist(err error) bool
```
IsExist returns a boolean indicating whether the error is known to report that a file or directory already exists. 
It is satisfied by ErrExist as well as some syscall errors.
IsExist 返回一个 布尔值 说明 错误是否知道报告 文件或目录已经存在
它满足ErrExist  和一些系统调用错误一样.



func IsNotExist
```golang
func IsNotExist(err error) bool
```
IsNotExist returns a boolean indicating whether the error is known to report that a file or directory does not exist. 
It is satisfied by ErrNotExist as well as some syscall errors.
IsNotExist 返回一个 布尔值 说明 错误是否知道报告 文件或目录不存在
它满足ErrExist  和一些系统调用错误一样.



func IsPathSeparator
```golang
func IsPathSeparator(c uint8) bool
```
IsPathSeparator returns true if c is a directory separator character.
IsPathSeparator 如果c 是一个目录分隔符 返回true.



func IsPermission
```golang
func IsPermission(err error) bool
```
IsPermission returns a boolean indicating whether the error is known to report that permission is denied. 
It is satisfied by ErrPermission as well as some syscall errors.
IsPermission返回一个 布尔值 说明 错误是否知道报告 权限不足
它满足ErrPermission  和一些系统调用错误一样.



func Lchown
```golang
func Lchown(name string, uid, gid int) error
```
Lchown changes the numeric uid and gid of the named file. 
If the file is a symbolic link, it changes the uid and gid of the link itself. 
If there is an error, it will be of type *PathError.
Lchown 修改 指定文件的uid和gid号.
如果文件是一个符号链接,它修改链接本身的 uid和gid.
如果这有错误,这将是 *PathError 类型.



func Link
```golang
func Link(oldname, newname string) error
```
Link creates newname as a hard link to the oldname file. 
If there is an error, it will be of type *LinkError.
Link 创建 newname 成一个硬链接 到 oldname 文件.
如果这有错误,这将是 *LinkError 类型.



func Mkdir
```golang
func Mkdir(name string, perm FileMode) error
```
Mkdir creates a new directory with the specified name and permission bits.
If there is an error, it will be of type *PathError.
Mkdir 用指定的name和权限位 创建一个新的目录
如果这有错误,这将是 *PathError 类型.



func MkdirAll
```golang
func MkdirAll(path string, perm FileMode) error
```
MkdirAll creates a directory named path, along with any necessary parents, and returns nil, or else returns an error. 
The permission bits perm are used for all directories that MkdirAll creates. 
If path is already a directory, MkdirAll does nothing and returns nil.
MkdirAll创建一个 命令路径目录,以及任何必要的父目录, 并返回nil或 其他的返回错误.
权限位perm 用在所有MkdirAll创建的目录
如果path 已经存在一个目录,MkdirAll 不做任何事, 并返回nil



func NewSyscallError
```golang
func NewSyscallError(syscall string, err error) error
```
NewSyscallError returns, as an error, a new SyscallError with the given system call name and error details. As a convenience, if err is nil, NewSyscallError returns nil.
NewSyscallError 返回 一个错误, 一个新的 给定系统调用名称和 详细错误的SyscallError.为了方便,如果err是nil,NewSyscallError 返回nil.



func Readlink
```golang
func Readlink(name string) (string, error)
```
Readlink returns the destination of the named symbolic link. If there is an error, it will be of type *PathError.
Readlink 返回 指定符号链接的目的地.如果这有错误,这将是 *PathError 类型.



func Remove
```golang
func Remove(name string) error
```
Remove removes the named file or directory. If there is an error, it will be of type *PathError.
Remove 删除指定文件或目录,如果这有错误,这将是 *PathError 类型.



func RemoveAll
```golang
func RemoveAll(path string) error
```
RemoveAll removes path and any children it contains. 
It removes everything it can but returns the first error it encounters. 
If the path does not exist, RemoveAll returns nil (no error).
RemoveAll 删除path 和它包含的任何子路径
它删除任何 它可以删除的东西但是 返回遇到的第一个错误.
如果path 不存在,RemoveAll 返回nil(而不是error)



func Rename
```golang
func Rename(oldpath, newpath string) error
```
Rename renames (moves) a file. OS-specific restrictions might apply.
Rename 重命名(移动)一个文件. 适用于特定的 操作系统.



func SameFile
```golang
func SameFile(fi1, fi2 FileInfo) bool
```
SameFile reports whether fi1 and fi2 describe the same file. 
For example, on Unix this means that the device and inode fields of the two underlying structures are identical; 
on other systems the decision may be based on the path names. 
SameFile only applies to results returned by this package's Stat. 
It returns false in other cases.
SameFile 报告 fi1 和fi2 描述的是否是同一个文件.
例如 , 在Unix 这意味着 设备 和 索引节点 字段的两个底层 结构相同.
在其他的系统 可能是 在路径名的基础上决定的.
SameFile 只适用 通过这个包的 Stat 返回结果.
其他的情况 返回 false



func Setenv
```golang
func Setenv(key, value string) error
```
Setenv sets the value of the environment variable named by the key. It returns an error, if any.
Setenv 根据key设置环境变量的变量值.如果有任何错误,会返回.



func Symlink
```golang
func Symlink(oldname, newname string) error
```
Symlink creates newname as a symbolic link to oldname. If there is an error, it will be of type *LinkError.
Symlink 创建  newname 符号链接到 oldname.如果这有错误,这将是 *LinkError 类型.


func TempDir
```golang
func TempDir() string
```
TempDir returns the default directory to use for temporary files.
TempDir 返回临时文件适用的默认目录.


func Truncate
```golang
func Truncate(name string, size int64) error
```
Truncate changes the size of the named file. 
If the file is a symbolic link, it changes the size of the link's target. 
If there is an error, it will be of type *PathError.
Truncate 修改指定文件的大小.
如果文件是一个符号链接, 它修改 链接目标的大小.
如果这有错误,这将是 *PathError 类型.


type File
```golang
type File struct {
        // contains filtered or unexported fields
}
```
File represents an open file descriptor.
File 表示一个打开的文件描述符


func Create
```golang
func Create(name string) (file *File, err error)
```
Create creates the named file mode 0666 (before umask), truncating it if it already exists. 
If successful, methods on the returned File can be used for I/O; 
the associated file descriptor has mode O_RDWR. 
If there is an error, it will be of type *PathError.
Create 创建指定文件模式 0666 (在umask 之前),如果已经存在截断它.
如果成功 返回的File 可以用在I/O.
相关的文件描述符的模式是O_RDWR。
如果这有错误,这将是 *PathError 类型.



func NewFile
```golang
func NewFile(fd uintptr, name string) *File
```
NewFile returns a new File with the given file descriptor and name.
NewFile用给定的文件描述符和name返回一个新的File.



func Open
```golang
func Open(name string) (file *File, err error)
```
Open opens the named file for reading. If successful, methods on the returned file can be used for reading; 
the associated file descriptor has mode O_RDONLY. 
If there is an error, it will be of type *PathError.
Open 打开用于读取指定的文件。如果成功,方法返回的file 可以用在 reading; 
有关的文件描述符有模式O_RDONLY.
如果这有错误,这将是 *PathError 类型.



func OpenFile
```golang
func OpenFile(name string, flag int, perm FileMode) (file *File, err error)
```
OpenFile is the generalized open call; most users will use Open or Create instead. 
It opens the named file with specified flag (O_RDONLY etc.) and perm, (0666 etc.) if applicable. 
If successful, methods on the returned File can be used for I/O. 
If there is an error, it will be of type *PathError.
OpenFile 是一般性打开调用;大部分使用者 用 Open 或 Create代替.
它以指定的flag(O_RDONLY 等.) 和 perm(0666 等.)打开指定的文件 (如果适用).
如果成功 方法返回的 File 可以用做 I/O. 
如果这有错误,这将是 *PathError 类型.



func Pipe
```golang
func Pipe() (r *File, w *File, err error)
```
Pipe returns a connected pair of Files; reads from r return bytes written to w. It returns the files and an error, if any.
Pipe 返回一个File 连接对; 从r 读取  返回写入w的字节. 如果遇到任何错误,它返回文件和 错误.



func (*File) Chdir
```golang
func (f *File) Chdir() error
```
Chdir changes the current working directory to the file, which must be a directory. 
If there is an error, it will be of type *PathError.
Chdir修改文件的当前工作目录, 必须是一个目录.
如果这有错误,这将是 *PathError 类型.



func (*File) Chmod
```golang
func (f *File) Chmod(mode FileMode) error
```
Chmod changes the mode of the file to mode. If there is an error, it will be of type *PathError.
Chmod 修改文件的模式 成 mode.如果这有错误,这将是 *PathError 类型.


func (*File) Chown
```golang
func (f *File) Chown(uid, gid int) error
```
Chown changes the numeric uid and gid of the named file. If there is an error, it will be of type *PathError.
Chown 修改 指定文件的 uid 和gid号.如果这有错误,这将是 *PathError 类型.



func (*File) Close
```golang
func (f *File) Close() error
```
Close closes the File, rendering it unusable for I/O. It returns an error, if any.
Close 关闭 File,而无法使用的I / O。 如果遇到任何错误,返回错误



func (*File) Fd
```golang
func (f *File) Fd() uintptr
```
Fd returns the integer Unix file descriptor referencing the open file.
返回引用打开的文件整数的Unix文件描述符。



func (*File) Name
```golang
func (f *File) Name() string
```
Name returns the name of the file as presented to Open.
Name 返回 打开的文件名


func (*File) Read
```golang
func (f *File) Read(b []byte) (n int, err error)
```
Read reads up to len(b) bytes from the File. 
It returns the number of bytes read and an error, if any. 
EOF is signaled by a zero count with err set to io.EOF.
Read 从File中 读取len(b) 字节.
它返回 读取的子节数和错误.
EOF 是一个零数 err设置到 io.EOF 的信号.



func (*File) ReadAt
```golang
func (f *File) ReadAt(b []byte, off int64) (n int, err error)
```
ReadAt reads len(b) bytes from the File starting at byte offset off. 
It returns the number of bytes read and the error, if any. 
ReadAt always returns a non-nil error when n < len(b). 
At end of file, that error is io.EOF.
ReadAt 从File的偏移量开始 读取len(b)字节
它返回读取的字节数 和错误.
当n < len(b)时ReadAt 通常返回一个非nil错误.
在文件的末尾, 错误是io.EOF.



func (*File) Readdir
```golang
func (f *File) Readdir(n int) (fi []FileInfo, err error)
```
Readdir reads the contents of the directory associated with file and returns a slice of up to n FileInfo values, as would be returned by Lstat, in directory order. 
Subsequent calls on the same file will yield further FileInfos.

If n > 0, Readdir returns at most n FileInfo structures. In this case, if Readdir returns an empty slice, it will return a non-nil error explaining why. 
At the end of a directory, the error is io.EOF.

If n <= 0, Readdir returns all the FileInfo from the directory in a single slice. 
In this case, if Readdir succeeds (reads all the way to the end of the directory), it returns the slice and a nil error. 
If it encounters an error before the end of the directory, Readdir returns the FileInfo read until that point and a non-nil error.

Readdir读取 文件有关的目录内容 并 返回 n FileInfo 值的切片, 将 通过Lstat返回, 按目录顺序.
子请求调用同样的文件 将 产生进一步的 FileInfos

如果n>0 ,Readdir 返回 最少 n FileInfo结构. 在这种情况下, 如果Readdir 返回一个空的slice ,它将返回一个非nil错误 解释为什么这样. 目录结尾 error 是io.EOF.
 
如果 n<=0,Readdir从目录 返回所有的FileInfo  到单个slice. 
在这种情况下,如果Readdir 成功(读取所有的目录到末尾), 它返回 slice 和一个nil error.
如果它在目录模为之前 遇到错误, Readdir 返回 FileInfo 读取的 直到那个点 和一个 非nil error.



func (*File) Readdirnames
```golang
func (f *File) Readdirnames(n int) (names []string, err error)
```
Readdirnames reads and returns a slice of names from the directory f.

If n > 0, Readdirnames returns at most n names. In this case, if Readdirnames returns an empty slice, it will return a non-nil error explaining why. 
At the end of a directory, the error is io.EOF.

If n <= 0, Readdirnames returns all the names from the directory in a single slice. 
In this case, if Readdirnames succeeds (reads all the way to the end of the directory), it returns the slice and a nil error. 
If it encounters an error before the end of the directory, Readdirnames returns the names read until that point and a non-nil error.

Readdirnames  从目录f 读取 和返回 names slice.

如果n > 0,Readdirnames 返回至少 n names. 在这种情况下,如果Readdirnames 返回一个空slice, 它将返回一个空 nil error 解释为什么.
在目录的结尾, error是 io.EOF.

如果n <= 0, Readdirnames 返回 从目录读取的所有names 到 单个slice.
在这种情况下, 如果Readdirnames 成功 (读取所有 直到 目录的结尾), 它返回slice 和一个nil error.
如果在目录结尾之前遇到错误, Readdirnames 返回读取到那个点  和 非nil error.



func (*File) Seek
```golang
func (f *File) Seek(offset int64, whence int) (ret int64, err error)
```
Seek sets the offset for the next Read or Write on file to offset, interpreted according to whence: 
	0 means relative to the origin of the file, 
	1 means relative to the current offset, 
	and 2 means relative to the end. 
It returns the new offset and an error, if any.
Seek 设置偏移下一个 Read 或 Write 的文件偏移量, 根据以下说明:
	0意味着 相对于文件的原点，
	1意味着 相对 当前的偏移量,
	2意味着相对结尾
它返回新的偏移 和 遇到的错误.	



func (*File) Stat
```golang
func (f *File) Stat() (fi FileInfo, err error)
```
Stat returns the FileInfo structure describing file. If there is an error, it will be of type *PathError.
Stat 返回 FileInfo 结构 描述文件. 如果这有错误,这将是 *PathError 类型.


func (*File) Sync
```golang
func (f *File) Sync() (err error)
```
Sync commits the current contents of the file to stable storage. 
Typically, this means flushing the file system's in-memory copy of recently written data to disk.
Sync 该文件的当前内容提交到稳定存储器。
通常, 这意味着刷新文件系统的内存复制 最新的写入数据到磁盘.



func (*File) Truncate
```golang
func (f *File) Truncate(size int64) error
```
Truncate changes the size of the file. It does not change the I/O offset. 
If there is an error, it will be of type *PathError.
Truncate修改 文件 的大小. 它不会修改 I/O 偏移量.
如果这有错误,这将是 *PathError 类型.



func (*File) Write
```golang
func (f *File) Write(b []byte) (n int, err error)
```
Write writes len(b) bytes to the File. It returns the number of bytes written and an error, if any. 
Write returns a non-nil error when n != len(b).
Write 写len(b)字节到File. 它返回写入的字节数和任何遇到的错误.
当 n != len(b)  Write 返回一个 非nil error.


func (*File) WriteAt
```golang
func (f *File) WriteAt(b []byte, off int64) (n int, err error)
```
WriteAt writes len(b) bytes to the File starting at byte offset off. 
It returns the number of bytes written and an error, if any. WriteAt returns a non-nil error when n != len(b).
WriteAt  从字节 偏移量开始 写  len(b) 字节  到 File .
它返回写入的字节数和 遇到的错误.  当n != len(b) 时 WriteAt 返回一个非nil error.


func (*File) WriteString
```golang
func (f *File) WriteString(s string) (ret int, err error)
```
WriteString is like Write, but writes the contents of string s rather than a slice of bytes.
WriteString类似Write, 但是 写入的字符串s的内容 而不是字节 slice.


type FileInfo
```golang
type FileInfo interface {
        Name() string       // base name of the file
        Size() int64        // length in bytes for regular files; system-dependent for others
        Mode() FileMode     // file mode bits
        ModTime() time.Time // modification time
        IsDir() bool        // abbreviation for Mode().IsDir()
        Sys() interface{}   // underlying data source (can return nil)
}
```
A FileInfo describes a file and is returned by Stat and Lstat.
FileInfo 描述 一个文件和 通常 Stat 和 Lstat返回.


func Lstat
```golang
func Lstat(name string) (fi FileInfo, err error)
```
Lstat returns a FileInfo describing the named file. 
If the file is a symbolic link, the returned FileInfo describes the symbolic link. 
Lstat makes no attempt to follow the link. If there is an error, it will be of type *PathError.
Lstat 返回 一个 指定文件的 FileInfo 描述.
如果文件是一个符号连接,返回 FileInfo描述该符号连接.
Lstat没有尝试跟随链接。  如果这有错误,这将是 *PathError 类型.



func Stat
```golang
func Stat(name string) (fi FileInfo, err error)
```
Stat returns a FileInfo describing the named file. 
If there is an error, it will be of type *PathError.
Stat返回 一个 指定文件的 FileInfo 描述.
 如果这有错误,这将是 *PathError 类型.


type FileMode
```golang
type FileMode uint32
```
A FileMode represents a file's mode and permission bits. 
The bits have the same definition on all systems, so that information about files can be moved from one system to another portably. 
Not all bits apply to all systems. The only required bit is ModeDir for directories.
FileMode 表示i一个文件 模式 和权限位
位在所有的系统定义都是一样的, 所以 那个 文件有关的信息 可以从一个系统移植到其他.
不是所有位适用于所有系统。 唯一需要位 是 ModeDir 目录.

```golang
const (
        // The single letters are the abbreviations
        // used by the String method's formatting.
           //单字母缩写.
           //使用String 方法的格式.
        ModeDir        FileMode = 1 << (32 - 1 - iota) // d: is a directory  目录
        ModeAppend                                     // a: append-only 只追加
        ModeExclusive                                  // l: exclusive use  专用
        ModeTemporary                                  // T: temporary file (not backed up) 临时文件(不备份)
        ModeSymlink                                    // L: symbolic link 符号链接
        ModeDevice                                     // D: device file  设备文件
        ModeNamedPipe                                  // p: named pipe (FIFO)  命名管道
        ModeSocket                                     // S: Unix domain socket  Unix域 socket
        ModeSetuid                                     // u: setuid
        ModeSetgid                                     // g: setgid
        ModeCharDevice                                 // c: Unix character device, when ModeDevice is set
        ModeSticky                                     // t: sticky

        // Mask for the type bits. For regular files, none will be set.
        ModeType = ModeDir | ModeSymlink | ModeNamedPipe | ModeSocket | ModeDevice

        ModePerm FileMode = 0777 // permission bits
)
```
The defined file mode bits are the most significant bits of the FileMode. 
The nine least-significant bits are the standard Unix rwxrwxrwx permissions. 
The values of these bits should be considered part of the public API and may be used in wire protocols or disk representations: 
	they must not be changed, although new bits might be added.
默认的文件模式位 是最显著FileMode 位.
这九个最不显著位 是 标准 Unix rwxrwxrwx 权限.
这些位的值应被视为公共API的一部分 并 可在有线协议或磁盘陈述中使用：他们不能改变，但新的位可能会增加。



func (FileMode) IsDir
```golang
func (m FileMode) IsDir() bool
```
IsDir reports whether m describes a directory. That is, it tests for the ModeDir bit being set in m.
IsDir报告 m 是否 描述了一个目录.就是说, 它测试 被m设置 的ModeDir位。



func (FileMode) IsRegular
```golang
func (m FileMode) IsRegular() bool
```
IsRegular reports whether m describes a regular file. That is, it tests that no mode type bits are set.
IsRegular 报告 是否m 描述一个普通文件. 就是说 , 它测试  没有 模式类型为被设置.



func (FileMode) Perm
```golang
func (m FileMode) Perm() FileMode
```
Perm returns the Unix permission bits in m.
Perm 返回在m里的 Unix 权限位.


func (FileMode) String
```golang
func (m FileMode) String() string
```


type LinkError
```golang
type LinkError struct {
        Op  string
        Old string
        New string
        Err error
}
```
LinkError records an error during a link or symlink or rename system call and the paths that caused it.
LinkError 记录 在连接或 符号 或重命名 系统调用期间遇到的错误  和引起它的路径


func (*LinkError) Error
```golang
func (e *LinkError) Error() string
```


type PathError
```golang
type PathError struct {
        Op   string
        Path string
        Err  error
}
```
PathError records an error and the operation and file path that caused it.
PathError 记录错误和 操作 和 引起它的文件路径.



func (*PathError) Error
```golang
func (e *PathError) Error() string
```



type ProcAttr
```golang
type ProcAttr struct {
        // If Dir is non-empty, the child changes into the directory before
        // creating the process.
           // 如果 Dir 非空,子 在创建进程之前 修改到目录
        Dir string
        
        
        // If Env is non-nil, it gives the environment variables for the
        // new process in the form returned by Environ.
        // If it is nil, the result of Environ will be used.
           // 如果Env 是非nil, 它通过 Environ 返回  为新的进程 给定环境变量
           //如果它是nil, 将使用 Environ 结果.
        Env []string
        
        
        // Files specifies the open files inherited by the new process.  The
        // first three entries correspond to standard input, standard output, and
        // standard error.  An implementation may support additional entries,
        // depending on the underlying operating system.  A nil entry corresponds
        // to that file being closed when the process starts.
        //Files 指定继承的新进程中打开文件.
           // 开始的三个项 对应 标准输入, 标准输出, 和标准错误.
           //一个实现可能支持额外的项目，依赖 底层操系统.
           //当进程开始的时候, 一个nil 输入 对应 被关闭的文件
        Files []*File


        // Operating system-specific process creation attributes.
        // Note that setting this field means that your program
        // may not execute properly or even compile on some
        // operating systems.
           //特定于操作系统的进程创建的属性。
           //请注意，设置这个字段意味着你的程序 可能在一些操作系统无法正确执行，甚至是编译.
        Sys *syscall.SysProcAttr
}
```
ProcAttr holds the attributes that will be applied to a new process started by StartProcess.
ProcAttr 保留 属性, 到 用StartProcess 开始的 新的进程



type Process
```golang
type Process struct {
        Pid int
        // contains filtered or unexported fields
}
```
Process stores the information about a process created by StartProcess.
Process 储存由StartProcess创建一个进程的信息。


func FindProcess
```golang
func FindProcess(pid int) (p *Process, err error)
```
FindProcess looks for a running process by its pid. 
The Process it returns can be used to obtain information about the underlying operating system process.
FindProcess由它的pid查找正在运行的进程。
它返回该进程可以用于获得关于底层操作系统的进程的信息。


func StartProcess
```golang
func StartProcess(name string, argv []string, attr *ProcAttr) (*Process, error)
```
StartProcess starts a new process with the program, arguments and attributes specified by name, argv and attr.

StartProcess is a low-level interface. The os/exec package provides higher-level interfaces.

If there is an error, it will be of type *PathError.

StartProcess 按 指定的name, argv 和 attr 开启一个新的进程 
StartProcess是一个低级的接口.os/exec包提供 高级接口.
如果这有错误,这将是 *PathError 类型.


func (*Process) Kill
```golang
func (p *Process) Kill() error
```
Kill causes the Process to exit immediately.
Kill 使进程立即退出


func (*Process) Release
```golang
func (p *Process) Release() error
```
Release releases any resources associated with the Process p, rendering it unusable in the future. 
Release only needs to be called if Wait is not.
Release 释放任何与Process p关联的资源, 未来无法使用.
Release 如果Wait 没有, 只需要调用.


func (*Process) Signal
```golang
func (p *Process) Signal(sig Signal) error
```
Signal sends a signal to the Process. Sending Interrupt on Windows is not implemented.
Signal将信号发送到进程 .在Windows上发送中断未实现。


func (*Process) Wait
```golang
func (p *Process) Wait() (*ProcessState, error)
```
Wait waits for the Process to exit, and then returns a ProcessState describing its status and an error, if any. 
Wait releases any resources associated with the Process. 
On most operating systems, the Process must be a child of the current process or an error will be returned.
Wait等待Process退出, 并返回 描述它的状态的ProcessState 和遇到的任何错误.
Wait 释放 和Process 有关的任何资源.
在大部分的操作系统上,Process必须是当前进程的子进程或将返回错误.


type ProcessState
```golang
type ProcessState struct {
        // contains filtered or unexported fields
}
```
ProcessState stores information about a process, as reported by Wait.
ProcessState 存储 进程有关的信息, 通过Wait 报告.


func (*ProcessState) Exited
```golang
func (p *ProcessState) Exited() bool
```
Exited reports whether the program has exited.
Exited 报告 程序是否退出



func (*ProcessState) Pid
```golang
func (p *ProcessState) Pid() int
```
Pid returns the process id of the exited process.
Pid返回退出进程的进程ID。


func (*ProcessState) String
```golang
func (p *ProcessState) String() string
```



func (*ProcessState) Success
```golang
func (p *ProcessState) Success() bool
```
Success reports whether the program exited successfully, such as with exit status 0 on Unix.
Success报告程序退出是否成功,例如 在Unix 退出状态 0


func (*ProcessState) Sys
```golang
func (p *ProcessState) Sys() interface{}
```
Sys returns system-dependent exit information about the process. 
Convert it to the appropriate underlying type, such as syscall.WaitStatus on Unix, to access its contents.
Sys 返回  关于该进程依赖于系统的退出的信息。
将其转换为相应的基础类型,如 Unix 上的 syscall.WaitStatus, 访问它的内容.



func (*ProcessState) SysUsage
```golang
func (p *ProcessState) SysUsage() interface{}
```
SysUsage returns system-dependent resource usage information about the exited process. 
Convert it to the appropriate underlying type, such as *syscall.Rusage on Unix, to access its contents. 
(On Unix, *syscall.Rusage matches struct rusage as defined in the getrusage(2) manual page.)
SysUsage返回  返回有关已退出进程依赖于系统的资源使用情况的信息。
将其转换为相应的基础类型,如 Unix 上的 *syscall.Rusage, 访问它的内容.
(在Unix上, *syscall.Rusage  使用getrusage（2）手册页中定义匹配结构rusage。)



func (*ProcessState) SystemTime
```golang
func (p *ProcessState) SystemTime() time.Duration
```
SystemTime returns the system CPU time of the exited process and its children.
SystemTime 返回退出进程及其子进程的系统CPU时间。



func (*ProcessState) UserTime
```golang
func (p *ProcessState) UserTime() time.Duration
```
UserTime returns the user CPU time of the exited process and its children.
UserTime 返回退出进程及其子进程的用户CPU时间。


type Signal
```golang
type Signal interface {
        String() string
        Signal() // to distinguish from other Stringers
}
```
A Signal represents an operating system signal. 
The usual underlying implementation is operating system-dependent: on Unix it is syscall.Signal.
Signal 表示一个操作系统信号.
通常的底层实现是依赖于操作系统: 在Unix 上它是 syscall.Signal. 

```golang
var (
        Interrupt Signal = syscall.SIGINT
        Kill      Signal = syscall.SIGKILL
)
```
The only signal values guaranteed to be present on all systems are Interrupt (send the process an interrupt) and Kill (force the process to exit).
保证所有系统上都有唯一的信号值是中断(发送进程中断)和Kill(强制退出进程)



type SyscallError
```golang
type SyscallError struct {
        Syscall string
        Err     error
}
```
SyscallError records an error from a specific system call.
SyscallError从指定的系统调用 记录错误.


func (*SyscallError) Error
```golang
func (e *SyscallError) Error() string
```






























