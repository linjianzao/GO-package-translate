Package filepath

Overview ▾

Package filepath implements utility routines for manipulating filename paths in a way compatible with the target operating system-defined file paths.
filepath 实现了实用例程来操作目标操作系统定义的文件路径的文件名兼容的路径的方式。

Constants
```golang
const (
        Separator     = os.PathSeparator
        ListSeparator = os.PathListSeparator
)
```

Variables
```golang
var ErrBadPattern = errors.New("syntax error in pattern")
```
ErrBadPattern indicates a globbing pattern was malformed.
ErrBadPattern 说明全局pattern的异常


```golang
var SkipDir = errors.New("skip this directory")
```
SkipDir is used as a return value from WalkFuncs to indicate that the directory named in the call is to be skipped. 
It is not returned as an error by any function.
SkipDir 用来 返回值  从WalkFuncs 说明 调用的指定目录 被跳过.
它不会返回任何函数的错误.


func Abs
```golang
func Abs(path string) (string, error)
```
Abs returns an absolute representation of path. 
If the path is not absolute it will be joined with the current working directory to turn it into an absolute path. 
The absolute path name for a given file is not guaranteed to be unique.
Abs 返回一个 绝对值 代表path.
如果path不是绝对路径 它将会加入当前的工作目录来转换成 绝对路径.
绝对路径的名字是给定的file,  所以不保证是唯一的.



func Base

func Base(path string) string
Base returns the last element of path. 
Trailing path separators are removed before extracting the last element. 
If the path is empty, Base returns ".". If the path consists entirely of separators, Base returns a single separator.






