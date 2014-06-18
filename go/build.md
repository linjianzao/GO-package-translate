包地址：http://golang.org/pkg/go/build/            
#Package build            

##Overview          
Package build gathers information about Go packages.          
build包云集了 和 GO包有关的信息          

##Go Path          

The Go path is a list of directory trees containing Go source code.           
It is consulted to resolve imports that cannot be found in the standard Go tree.           
The default path is the value of the GOPATH environment variable,           
	interpreted as a path list appropriate to the operating system           
	(on Unix, the variable is a colon-separated string; on Windows, a semicolon-separated string; on Plan 9, a list).          

Each directory listed in the Go path must have a prescribed structure:          

The src/ directory holds source code. The path below 'src' determines the import path or executable name.          

The pkg/ directory holds installed package objects.           
As in the Go tree, each target operating system and architecture pair has its own subdirectory of pkg (pkg/GOOS_GOARCH).          

If DIR is a directory listed in the Go path,           
	a package with source in DIR/src/foo/bar can be imported as "foo/bar" and has its compiled form installed to "DIR/pkg/GOOS_GOARCH/foo/bar.a"           
	(or, for gccgo, "DIR/pkg/gccgo/foo/libbar.a").          

The bin/ directory holds compiled commands.           
Each command is named for its source directory, but only using the final element, not the entire path.           
That is, the command with source in DIR/src/foo/quux is installed into DIR/bin/quux, not DIR/bin/foo/quux.           
The foo/ is stripped so that you can add DIR/bin to your PATH to get at the installed commands.          

Go路径是包含GO源码的目录树列表          
它参照以解决 在准备GO树中不能发现的引入          
默认值是GOPATH环境变量的值,          
解释为 该操作系统的路径列表(在Unix,变量是以冒号隔开的字符串;在Windows,是分号分隔的字符串,在Plan 9 是一个列表)          

GO路径中的每个目录必须有规定的结构:          
src/ 目录 放源码.src路径下决定导入路径或可执行文件的名称          

pkg/ 目录放安装包对象          
正如在GO树, 每个目标操作系统和架构都有他们自己的pkg子目录 (pkg/GOOS_GOARCH).          

如果DIR 是在GO路径列表的目录中,一个源码在DIR/src/foo/bar 里的包 可以 引入"foo/bar"          
	并且 编译成安装格式到"DIR/pkg/GOOS_GOARCH/foo/bar.a"(或者 对于gccgo,"DIR/pkg/gccgo/foo/libbar.a")          

bin/放编译的命令.          
每个命令以源目录命名,但是只使用最后的元素,而不是整个路径.          
也就是说 DIR/src/foo/quux里的源码 是安装到  DIR/bin/quux而不是DIR/bin/foo/quux.           
foo/ 会跳过 所有 你可以添加 DIR/bin 到你的 PATH 里 来获取安装命令          

Here's an example directory layout:          
这有个目录结构 例子:          
```golang
GOPATH=/home/user/gocode

/home/user/gocode/
    src/
        foo/
            bar/               (go code in package bar)
                x.go
            quux/              (go code in package main)
                y.go
    bin/
        quux                   (installed command)
    pkg/
        linux_amd64/
            foo/
                bar.a          (installed package object)
```


##Build Constraints 约束          

A build constraint is a line comment beginning with the directive +build that lists the conditions under which a file should be included in the package.           
Constraints may appear in any kind of source file (not just Go), but they must appear near the top of the file,           
	preceded only by blank lines and other line comments.          

To distinguish build constraints from package documentation, a series of build constraints must be followed by a blank line.          
一个build是一个行注释 以  指令+生成, 列出 下一个文件应该被包含在包里的条件 开始          
Constraints 可以出现在任何类型的源文件(不只是GO),但是他们必须出现在接近文件顶部附近,仅空白行和其他行注释在前面          

A build constraint is evaluated as the OR of space-separated options; each option evaluates as the AND of its comma-separated terms;          
and each term is an alphanumeric word or, preceded by !, its negation. That is, the build constraint:          
一个build约束 等价于空格分隔选项OR;  每个选项等价于 逗号分隔 的AND条件;并且每个条件的否定是一个字母数字 单词 or,以!在前面. 也就是说build约束:          
```golang
// +build linux,386 darwin,!cgo
```

corresponds to the boolean formula:          
对应的布尔公式:          
```golang
(linux AND 386) OR (darwin AND (NOT cgo))
```

A file may have multiple build constraints. The overall constraint is the AND of the individual constraints. That is, the build constraints:          
一个文件 可能会有多个build约束.整体约束是个体约束的AND.也就是说, build约束:         
```golang 
// +build linux darwin
// +build 386
```
corresponds to the boolean formula:          
对应的布尔公式:          
```golang
(linux OR darwin) AND 386
```
During a particular build, the following words are satisfied:          
在一个特定的build , 满足下面的词:          
```golang
- the target operating system, as spelled by runtime.GOOS
- the target architecture, as spelled by runtime.GOARCH
- the compiler being used, either "gc" or "gccgo"
- "cgo", if ctxt.CgoEnabled is true
- "go1.1", from Go version 1.1 onward
- "go1.2", from Go version 1.2 onward
- any additional words listed in ctxt.BuildTags

-目标操作系统,由runtime.GOOS指定
-目标架构,由runtime.GOARCH指定
-编译器使用 "gc"或"gccgo"
-如果ctxt.CgoEnabled是true ,"cgo"
-"go1.1",从1.1版本开始
-"go1.2",从1.2版本开始
-在ctxt.BuildTags所列的任何附加的
```
If a file's name, after stripping the extension and a possible _test suffix, matches any of the following patterns:          
如果一个文件名, 剥离 扩展 和可能的_test后缀后, 匹配以下的任何情况:          
```golang
*_GOOS
*_GOARCH
*_GOOS_GOARCH
```

(example: source_windows_amd64.go) or the literals:          
(例子:source_windows_amd64.go)或者文字:          
```golang
GOOS
GOARCH
```

(example: windows.go) where GOOS and GOARCH represent any known operating system and architecture values respectively,           
	then the file is considered to have an implicit build constraint requiring those terms.          

To keep a file from being considered for the build:          
(例子: windows.go)其中GOOS和GOARCH分别表示任何已知的操作系统和体系架构值,那么该文件被认为是有 要求这些条件 隐性的build约束.          
为了保持一个文件被认为是构建:          
```golang
// +build ignore
```

(any other unsatisfied word will work as well, but “ignore” is conventional.)          

To build a file only when using cgo, and only on Linux and OS X:          
(任何其他未满足的 也可以正常的工作,但是惯例会“ignore”)          
只用cgo build一个文件 并且只在Linux和 OSX上:          
```golang
// +build linux,cgo darwin,cgo
```

Such a file is usually paired with another file implementing the default functionality for other systems, which in this case would carry the constraint:          
这样的文件通常 搭配其他的文件 实现 其他系统 默认的功能,在这种情况下将执行的约束          
```golang
// +build !linux,!darwin !cgo
```
Naming a file dns_windows.go will cause it to be included only when building the package for Windows; similarly,           
math_386.s will be included only when building the package for 32-bit x86.          
当为 Windows构建包 并命名为dns_windows.go 将会导致它可以只包含;同样的 为 32-bit x86 构建包,math_386.s将会只包含.          

##Variables          
```golang
var ToolDir = filepath.Join(runtime.GOROOT(), "pkg/tool/"+runtime.GOOS+"_"+runtime.GOARCH)
```
ToolDir is the directory containing build tools.          
包含build工具的目录          


##func ArchChar          
```golang
func ArchChar(goarch string) (string, error)
```
ArchChar returns the architecture character for the given goarch. For example, ArchChar("amd64") returns "6".          
返回给定GOARCH架构字符.比如:ArchChar("amd64") 返回6          


##func IsLocalImport          
```golang
func IsLocalImport(path string) bool
```
IsLocalImport reports whether the import path is a local import path, like ".", "..", "./foo", or "../foo".          
报告 引入路径是否是本地的引入路径, 比如".", "..", "./foo", or "../foo".          


##type Context          
```golang
type Context struct {
        GOARCH      string // target architecture 目标架构
        GOOS        string // target operating system 目标操作系统
        GOROOT      string // Go root 
        GOPATH      string // Go path
        CgoEnabled  bool   // whether cgo can be used CGO是否可用
        UseAllFiles bool   // use files regardless of +build lines, file names  不管使用的文件+生成线，文件名
        Compiler    string // compiler to assume when computing target paths 编译到系统目标路径

        // The build and release tags specify build constraints
        // that should be considered satisfied when processing +build lines.
        // Clients creating a new context may customize BuildTags, which
        // defaults to empty, but it is usually an error to customize ReleaseTags,
        // which defaults to the list of Go releases the current release is compatible with.
        // In addition to the BuildTags and ReleaseTags, build constraints
        // consider the values of GOARCH and GOOS as satisfied tags.
          //当处理 +build线时 必须满足  构建和发布指定build约束标签  
       //Clients创建一个新的上下文可以自定义的BuildTags,默认是空的,但是它通常是一个错误定制的ReleaseTags,默认为GO发布的 兼容当前版本的列表
         //除了BuildTags 和 ReleaseTags build约束 是满足GOARCH和GOOS的标签值
        BuildTags   []string
        ReleaseTags []string

        // The install suffix specifies a suffix to use in the name of the installation
        // directory. By default it is empty, but custom builds that need to keep
        // their outputs separate can set InstallSuffix to do so. For example, when
        // using the race detector, the go command uses InstallSuffix = "race", so
        // that on a Linux/386 system, packages are written to a directory named
        // "linux_386_race" instead of the usual "linux_386".
          //安装后缀指定使用安装目录的名称后缀.
          //默认情况下是空的,但是自定义的builds 需要保持输出分开 可以设置InstallSuffix 这样做
          //比如,使用探测器的比赛时，go命令使用InstallSuffix = "race",
          //因此在 Linux/386系统中,包写到目录名"linux_386_race"代替 通常的"linux_386"
        InstallSuffix string

        // JoinPath joins the sequence of path fragments into a single path.
        // If JoinPath is nil, Import uses filepath.Join.
           //连接路径片段的序列为单一路径。
           //如果JoinPath是nil,使用filepath.Join
        JoinPath func(elem ...string) string

        // SplitPathList splits the path list into a slice of individual paths.
        // If SplitPathList is nil, Import uses filepath.SplitList.
          //分割路径成 单一的路径 slice
          //如果SplitPathList是nil,使用filepath.SplitList.
        SplitPathList func(list string) []string

        // IsAbsPath reports whether path is an absolute path.
        // If IsAbsPath is nil, Import uses filepath.IsAbs.
          //报告路径是否是绝对路径
          //如果IsAbsPath是nil,使用filepath.IsAbs
        IsAbsPath func(path string) bool

        // IsDir reports whether the path names a directory.
        // If IsDir is nil, Import calls os.Stat and uses the result's IsDir method.
          //报告路径名字是否是个目录
          //如果IsDir是nil,调用 os.Stat 然后使用结果的 IsDir方法
        IsDir func(path string) bool

        // HasSubdir reports whether dir is a subdirectory of
        // (perhaps multiple levels below) root.
        // If so, HasSubdir sets rel to a slash-separated path that
        // can be joined to root to produce a path equivalent to dir.
        // If HasSubdir is nil, Import uses an implementation built on
        // filepath.EvalSymlinks.
          //报告dir 是否是 根的一个子目录(也许是多级下面)
          //如果是这样,HasSubdir设置rel到可连接的根,以产生等同于目录的 以斜线隔开的路径
          //如果HasSubdir是nil, 使用  建立在filepath.EvalSymlinks的实现
        HasSubdir func(root, dir string) (rel string, ok bool)

        // ReadDir returns a slice of os.FileInfo, sorted by Name,
        // describing the content of the named directory.
        // If ReadDir is nil, Import uses ioutil.ReadDir.
          //返回 一个os.FileInfo 的slice,以Name排序,描述指定目录的内容。
          //如果ReadDir是nil , 使用ioutil.ReadDir.
        ReadDir func(dir string) (fi []os.FileInfo, err error)

        // OpenFile opens a file (not a directory) for reading.
        // If OpenFile is nil, Import uses os.Open.
          //打开用于读取的文本(不是目录)
          //如果OpenFile是nil,使用os.Open.
        OpenFile func(path string) (r io.ReadCloser, err error)
}
```
A Context specifies the supporting context for a build.          
指定生成的配套环境。          
```golang
var Default Context = defaultContext()
```

Default is the default Context for builds.           
It uses the GOARCH, GOOS, GOROOT, and GOPATH environment variables if set, or else the compiled code's GOARCH, GOOS, and GOROOT.          
默认的Context          
如果设置了它使用 GOARCH, GOOS, GOROOT, 和 GOPATH  环境变量 , 否则编译代码的GOARCH, GOOS, 和 GOROOT.          


##func (*Context) Import          
```golang
func (ctxt *Context) Import(path string, srcDir string, mode ImportMode) (*Package, error)
```
Import returns details about the Go package named by the import path, interpreting local import paths relative to the srcDir directory.           
If the path is a local import path naming a package that can be imported using a standard import path, the returned package will set p.ImportPath to that path.          
使用path 返回GO 包名 有关的详细信息,本地路径相对于srcDir目录          
如果路径是一个本地包的引入路径名, 可以 使用标准的导入路径导入一个包, 返回 包 会设置p.ImportPath 成 那个路径          

In the directory containing the package, .go, .c, .h, and .s files are considered part of the package except for:          
在包含包的目录,  .go, .c, .h, 和 .s文件包含包的一部分,除了:          
```golang
- .go files in package documentation
- files starting with _ or . (likely editor temporary files)
- files with build constraints not satisfied by the context
- 包里的.go文件文档 以_或.开始(可能是编辑器的临时文件)与构建约束文件上下文不对应
```

If an error occurs, Import returns a non-nil error and a non-nil *Package containing partial information.          
如果遇到错误,返回非nil错误和一个 包含部分信息的非nil包          


##func (*Context) ImportDir          
```golang
func (ctxt *Context) ImportDir(dir string, mode ImportMode) (*Package, error)
```
ImportDir is like Import but processes the Go package found in the named directory.          
类似Import 但是处理在指定路径发现的包          


##func (*Context) MatchFile          
```golang
func (ctxt *Context) MatchFile(dir, name string) (match bool, err error)
```
MatchFile reports whether the file with the given name in the given directory matches the context and would be included in a Package created by ImportDir of that directory.          
MatchFile considers the name of the file and may use ctxt.OpenFile to read some or all of the file's content.          
报告 指定的目录里 指定的名称 文将是否匹配,将被包含在Package  用ImportDir创建那个目录          
MatchFile 查看文件的名字 并 可以使用 ctxt.OpenFile  读取一些 或全部的 文件内容          


##func (*Context) SrcDirs          
```golang
func (ctxt *Context) SrcDirs() []string
```
SrcDirs returns a list of package source root directories.           
It draws from the current Go root and Go path but omits directories that do not exist.          
返回包源根目录的列表.          
它提取当前的GO根目录和 GO路径  忽略不存在的目录          

##type ImportMode          
```golang
type ImportMode uint
```
An ImportMode controls the behavior of the Import method.          
控制导入方法的行为          
```golang
const (
        // If FindOnly is set, Import stops after locating the directory
        // that should contain the sources for a package.  It does not
        // read any files in the directory.
          //如果设置了FindOnly,Import在查找该目录有包含 包的源 后停止.
          //它不会读取目录里的任何文件
          
        FindOnly ImportMode = 1 << iota

        // If AllowBinary is set, Import can be satisfied by a compiled
        // package object without corresponding sources.
          //如果设置了AllowBinary,可以通过编译的软件包即使对象没有满足相应的资源。
        AllowBinary
)
```


##type NoGoError          
```golang
type NoGoError struct {
        Dir string
}
```
NoGoError is the error used by Import to describe a directory containing no buildable Go source files.                     
(It may still contain test files, files hidden by build tags, and so on.)                    
这是个用来描述Import  目录不包含可构建的包源文件的错误.(它可能会包含测试文件,文件用build tag隐藏 , 等等)                    


##func (*NoGoError) Error          
```golang
func (e *NoGoError) Error() string
```


##type Package          
```golang
type Package struct {
        Dir         string   // directory containing package sources
        							  //目录包含的源
        
        Name        string   // package name
        							  //包名
        
        Doc         string   // documentation synopsis
        							   //文档简介
        
        ImportPath  string   // import path of package ("" if unknown)
        							   //导入包的路径(未知的情况下是 " ")
        
        Root        string   // root of Go tree where this package lives
        							   //GO树的根,包的生命
        
        SrcRoot     string   // package source root directory ("" if unknown)
        							   //包源的根目录
        
        PkgRoot     string   // package install root directory ("" if unknown)
        							   //包安装的根目录
        
        BinDir      string   // command install directory ("" if unknown)
        							   //命令的安装目录
        
        Goroot      bool     // package found in Go root
        							  //GOroot里的包
        
        PkgObj      string   // installed .a file
        							   //安装成.a文件
        							   
        AllTags     []string // tags that can influence file selection in this directory
        							   //tags可以影响这个目录中的文件选择
        							   
        ConflictDir string   // this directory shadows Dir in $GOPATH
        							   

        // Source files
        GoFiles        []string // .go source files (excluding CgoFiles, TestGoFiles, XTestGoFiles)
        								   //.go 源文件
        								   
        CgoFiles       []string // .go source files that import "C"
        								   // 导入"C"的 .go 源文件
        
        IgnoredGoFiles []string // .go source files ignored for this build
        								  // .go源文件忽略此构建
        
        CFiles         []string // .c source files
        								  // .c源文件
        
        CXXFiles       []string // .cc, .cpp and .cxx source files
        								  // .cc, .cpp 和 .cxx源文件 
        
        HFiles         []string // .h, .hh, .hpp and .hxx source files
        								   //.h, .hh, .hpp 和 .hxx 源文件
        								    
        SFiles         []string // .s source files
        								  //.s 源文件
        
        SwigFiles      []string // .swig files
        								  //.swig文件			
        
        SwigCXXFiles   []string // .swigcxx files
										  // .swigcxx文件
        
        SysoFiles      []string // .syso system object files to add to archive
        								  //.syso 系统的目标文件添加到归档

        // Cgo directives
        CgoCFLAGS    []string // Cgo CFLAGS directives
        CgoCPPFLAGS  []string // Cgo CPPFLAGS directives
        CgoCXXFLAGS  []string // Cgo CXXFLAGS directives
        CgoLDFLAGS   []string // Cgo LDFLAGS directives
        CgoPkgConfig []string // Cgo pkg-config directives

        // Dependency information
        Imports   []string                    // imports from GoFiles, CgoFiles
        ImportPos map[string][]token.Position // line information for Imports

        // Test information
        TestGoFiles    []string                    // _test.go files in package
        TestImports    []string                    // imports from TestGoFiles
        TestImportPos  map[string][]token.Position // line information for TestImports
        XTestGoFiles   []string                    // _test.go files outside package
        XTestImports   []string                    // imports from XTestGoFiles
        XTestImportPos map[string][]token.Position // line information for XTestImports
}
```
A Package describes the Go package found in a directory.          
描述在目录里发现的GO包          


##func Import          
```golang
func Import(path, srcDir string, mode ImportMode) (*Package, error)
```
Import is shorthand for Default.Import.          
Import是Default.Import的速记          



##func ImportDir          
```golang
func ImportDir(dir string, mode ImportMode) (*Package, error)
```
ImportDir is shorthand for Default.ImportDir.          
ImportDir是Default.ImportDir的速记          


##func (*Package) IsCommand          
```golang
func (p *Package) IsCommand() bool
```
IsCommand reports whether the package is considered a command to be installed (not just a library).           
Packages named "main" are treated as commands.          
报告程序包是否被认为是安装的命令(不只是个类库)          
包名为"main" 也被当成命令          

























                