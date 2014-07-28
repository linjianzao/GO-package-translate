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
```golang
func Base(path string) string
```
Base returns the last element of path. 
Trailing path separators are removed before extracting the last element. 
If the path is empty, Base returns ".". If the path consists entirely of separators, Base returns a single separator.
Base返回path的最后一个元素
在获取最后一个元素之前 移除结尾的分隔符.
如果path是空的,Base 返回 "." . 如果path完全由分隔符组成  ,Base 返回单个分隔符.



func Clean
```golang
func Clean(path string) string
```
Clean returns the shortest path name equivalent to path by purely lexical processing. 
It applies the following rules iteratively until no further processing can be done:
Clean 返回 最短的path名称 等价于  path 通过纯粹的语法处理.
它遵循以下规则迭代直到没有进一步的处理可以做:
```golang
1. Replace multiple Separator elements with a single one.
2. Eliminate each . path name element (the current directory).
3. Eliminate each inner .. path name element (the parent directory)
   along with the non-.. element that precedes it.
4. Eliminate .. elements that begin a rooted path:
   that is, replace "/.." by "/" at the beginning of a path,
   assuming Separator is '/'.
  
1.替换多个分隔符元素成一个
2. 清除每个 .  路径名元素(代表当前目录)
3. 清除每个 .. 路径名元素(代表父目录) 和它前面的非 .. 元素。
4. 清除以root 路径 开始的..  : 也就是说在路径开始的时候用 "/" 替换 "/.."   
```   
   
The returned path ends in a slash only if it represents a root directory, such as "/" on Unix or `C:\` on Windows.

If the result of this process is an empty string, Clean returns the string ".".

See also Rob Pike, “Lexical File Names in Plan 9 or Getting Dot-Dot Right,” http://plan9.bell-labs.com/sys/doc/lexnames.html
只有当path表示一个根目录时,path的返回值是一个 斜杆,如 在Unix是"/"  或 在Windows上是`C:\` .
如果处理的结果是一个空字符串,Clean 返回字符串 ".".
见Rob Pike,"Plan 9的 词汇文件名 或右键获取",http://plan9.bell-labs.com/sys/doc/lexnames.html



func Dir
```golang
func Dir(path string) string
```
Dir returns all but the last element of path, typically the path's directory. 
After dropping the final element, the path is Cleaned and trailing slashes are removed. 
If the path is empty, Dir returns ".". If the path consists entirely of separators, Dir returns a single separator. 
The returned path does not end in a separator unless it is the root directory.
Dir 返回除path最后一个元素 的所有, 通常是path的目录.



func EvalSymlinks
```golang
func EvalSymlinks(path string) (string, error)
```
EvalSymlinks returns the path name after the evaluation of any symbolic links. 
If path is relative the result will be relative to the current directory, unless one of the components is an absolute symbolic link.
EvalSymlinks 返回 在任何符号连接之后的path名.
如果path是相对路径,结果将会相对当前目录, 除非 组件中有个是绝对路径符号链接.



func Ext

func Ext(path string) string
Ext returns the file name extension used by path. 
The extension is the suffix beginning at the final dot in the final element of path; it is empty if there is no dot.
Ext 返回path的 文件扩展名.
扩展是从最后一个点开始的 path的最后一个元素 后缀.如果没有点,它是空的



func FromSlash
```golang
func FromSlash(path string) string
```
FromSlash returns the result of replacing each slash ('/') character in path with a separator character. 
Multiple slashes are replaced by multiple separators.
FromSlash 返回 用分隔符替换 path里的每个 斜杠 ('/')字符 的结果
多个斜杠用多个分隔符替换.


func Glob
```golang
func Glob(pattern string) (matches []string, err error)
```
Glob returns the names of all files matching pattern or nil if there is no matching file. 
The syntax of patterns is the same as in Match. The pattern may describe hierarchical names such as /usr/*/bin/ed (assuming the Separator is '/').
Glob 返回所有匹配pattern 的文件名 或 如果没有匹配的文件返回nil.
patterns的语法 和 Match 里的一样.pattern可能描述 分级名,例如 /usr/*/bin/ed (假设分隔符是'/')



func HasPrefix
```golang
func HasPrefix(p, prefix string) bool
HasPrefix exists for historical compatibility and should not be used.
HasPrefix 为 历史遗留, 不应该被使用.
```


func IsAbs
```golang
func IsAbs(path string) bool
```
IsAbs returns true if the path is absolute.
如果path是绝对路径,IsAbs 返回true



func Join
```golang
func Join(elem ...string) string
```
Join joins any number of path elements into a single path, adding a Separator if necessary. 
The result is Cleaned, in particular all empty strings are ignored.
Join 加入任意数量的 路径元素到 path,如果需要的话 添加一个分隔符.
结果是被Cleaned 过的,特别是 所有空字符串被忽略



func Match
```golang
func Match(pattern, name string) (matched bool, err error)
```
Match returns true if name matches the shell file name pattern. The pattern syntax is:
如果name匹配 shell文件名pattern, Match 返回true. pattern 的 语法是:
```golang
pattern:
	{ term }
term:
	'*'         matches any sequence of non-Separator characters
	'?'         matches any single non-Separator character
	'[' [ '^' ] { character-range } ']'
	            character class (must be non-empty)
	c           matches character c (c != '*', '?', '\\', '[')
	'\\' c      matches character c

character-range:
	c           matches character c (c != '\\', '-', ']')
	'\\' c      matches character c
	lo '-' hi   matches character c for lo <= c <= hi
```	
Match requires pattern to match all of name, not just a substring. The only possible returned error is ErrBadPattern, when pattern is malformed.

On Windows, escaping is disabled. Instead, '\\' is treated as path separator.
Match 需要pattern 匹配所有的name,而不是一个子串. 当pattern异常时 唯一可能返回的错误是ErrBadPattern.
在Windows 上, escaping 是不可用的. 替代的, '\\'  当成一个路径分隔符


func Rel
```golang
func Rel(basepath, targpath string) (string, error)
```
Rel returns a relative path that is lexically equivalent to targpath when joined to basepath with an intervening separator. 
That is, Join(basepath, Rel(basepath, targpath)) is equivalent to targpath itself. 
On success, the returned path will always be relative to basepath, even if basepath and targpath share no elements. 
An error is returned if targpath can't be made relative to basepath or if knowing the current working directory would be necessary to compute it.
Rel 当加入basepath和一个分隔符时. 返回一个相对路径, 那就是说 词汇相当于targpath.
就是说,Join(basepath, Rel(basepath, targpath))相当于 targpath 本身.
成功,返回的path通常是相对basepath, 即使 如果basepath 和 targpath 没有共享的元素.
如果targpath 不能相对 basepath 或  如果知道当前工作目录 将需要计算它,  返回一个error

▾ Example
```golang
package main

import (
	"fmt"
	"path/filepath"
)

func main() {
	paths := []string{
		"/a/b/c",
		"/b/c",
		"./b/c",
	}
	base := "/a"

	fmt.Println("On Unix:")
	for _, p := range paths {
		rel, err := filepath.Rel(base, p)
		fmt.Printf("%q: %q %v\n", p, rel, err)
	}

}
```


func Split
```golang
func Split(path string) (dir, file string)
```
Split splits path immediately following the final Separator, separating it into a directory and file name component. 
If there is no Separator in path, Split returns an empty dir and file set to path. The returned values have the property that path = dir+file.
Split 分割path 紧随以下最后的分隔符,分隔它成一个目录和文件名组件.
如果path里没有分隔符,Split 返回一个空dir和file 集 到path. 返回值有 path = dir+file 属性 .



func SplitList
```golang
func SplitList(path string) []string
```
SplitList splits a list of paths joined by the OS-specific ListSeparator, usually found in PATH or GOPATH environment variables. 
Unlike strings.Split, SplitList returns an empty slice when passed an empty string.
SplitList 分割path列表 加入 系统指定的列表分割符,经常 是在PATH或GOPATH的环境变量.
不同于 strings.Split, 当传入一个空字符串时,SplitList返回空slice.


▹ Example
```golang
package main

import (
	"fmt"
	"path/filepath"
)

func main() {
	fmt.Println("On Unix:", filepath.SplitList("/a/b/c:/usr/bin"))
}
```


func ToSlash
```golang
func ToSlash(path string) string
```
ToSlash returns the result of replacing each separator character in path with a slash ('/') character. Multiple separators are replaced by multiple slashes.
ToSlash 返回用斜杠('/') 字符 替换 path里的每个分隔符的结果. 多个分隔符 用多个斜杠代替.


func VolumeName
```golang
func VolumeName(path string) (v string)
```
VolumeName returns leading volume name. Given "C:\foo\bar" it returns "C:" under windows. 
Given "\\host\share\foo" it returns "\\host\share". On other platforms it returns "".
VolumeName 返回开头卷名. 在windows下,"C:\foo\bar" 会返回  "C:" . "\\host\share\foo" 会返回 "\\host\share".在其他平台 返回 " "



func Walk
```golang
func Walk(root string, walkFn WalkFunc) error
```
Walk walks the file tree rooted at root, calling walkFn for each file or directory in the tree, including root. 
All errors that arise visiting files and directories are filtered by walkFn. 
The files are walked in lexical order, which makes the output deterministic but means that for very large directories Walk can be inefficient. 
Walk does not follow symbolic links.
Walk 行 在root 的文件树, 为树里的每个文件或目录调用walkFn,包含root.
所有的错误都是出现在 通过walkFn 访问文件 和 目录被过滤.
文件是依词汇顺序走的,这使得输出确定性，但表示对于非常大的目录Walk可能是低效的。
Walk 不跟 符号链接.


type WalkFunc
```golang
type WalkFunc func(path string, info os.FileInfo, err error) error
```
WalkFunc is the type of the function called for each file or directory visited by Walk. 
The path argument contains the argument to Walk as a prefix; 
that is, if Walk is called with "dir", which is a directory containing the file "a", the walk function will be called with argument "dir/a". 
The info argument is the os.FileInfo for the named path.

If there was a problem walking to the file or directory named by path, 
	the incoming error will describe the problem and the function can decide how to handle that error (and Walk will not descend into that directory). 
If an error is returned, processing stops. 
The sole exception is that if path is a directory and the function returns the special value SkipDir, 
	the contents of the directory are skipped and processing continues as usual on the next file.

WalkFunc 是调用 通过 Walk访问每个文件或目录 的函数类型 

path参数包含 参数 到Walk 类似一个前缀;
就是说,如果Walk 调用  "dir", 目录包含 文件 "a",walk 文件将会以参数"dir/a" 调用.
该信息参数是为指定路径的os.FileInfo。

如果通过path 指定的文件或目录有问题, 引入的错误将会描述该问题 并且 该函数可以决定怎么处理那个错误.(并且Walk 不会进入那个目录)
如果返回一个错误,处理停止. 唯一的例外是 如果path是一个目录并且函数返回特殊值SkipDir,目录的内容会跳过并且 会接着 和平时一样处理下一个文件
















