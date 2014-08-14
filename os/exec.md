Package exec

Overview ▾

Package exec runs external commands. It wraps os.StartProcess to make it easier to remap stdin and stdout, connect I/O with pipes, and do other adjustments.
exec 执行外部命令。 它封装os.StartProcess 让它 更简单的 重映射标准输入 和标准输出, 用管道连接I/O, 并做其他的 调整.


Variables
```golang
var ErrNotFound = errors.New("executable file not found in $PATH")
```
ErrNotFound is the error resulting if a path search failed to find an executable file.
ErrNotFound是如果一个路径搜索没有找到一个可执行文件的错误结果.


func LookPath
```golang
func LookPath(file string) (string, error)
```
LookPath searches for an executable binary named file in the directories named by the PATH environment variable. 
If file contains a slash, it is tried directly and the PATH is not consulted. The result may be an absolute path or a path relative to the current directory.
LookPath根据PATH环境变脸 在指定的目录里 搜索指定的 二进制可执行文件.
如果file包含一个斜杆,它直接尝试 并  PATH 不查询. 结果可能是一个绝对路径或相对当前目录的路径


▹ Example
```golang
package main

import (
	"fmt"
	"log"
	"os/exec"
)

func main() {
	path, err := exec.LookPath("fortune")
	if err != nil {
		log.Fatal("installing fortune is in your future")
	}
	fmt.Printf("fortune is available at %s\n", path)
}
```


type Cmd
```golang
type Cmd struct {
        // Path is the path of the command to run.
        //
        // This is the only field that must be set to a non-zero
        // value. If Path is relative, it is evaluated relative
        // to Dir.
        //Path 是命令执行的路径
           //这是唯一 必须设置非零值字段.
   		   //如果Path是相对,它是相对 Dir.
        Path string


        // Args holds command line arguments, including the command as Args[0].
        // If the Args field is empty or nil, Run uses {Path}.
        //
        // In typical use, both Path and Args are set by calling Command.
        //Args保留命令行参数, 包含命令 Args[0].
           //如果 Args字段是空的或nil, Run 使用 {Path}.
           //在典型的应用, Path和 Args 都通过调用Command 设置.
        Args []string



        // Env specifies the environment of the process.
        // If Env is nil, Run uses the current process's environment.
        //Env指定 环境进程.
          // 如果Env是nil, Run 使用当前进程的环境
        Env []string



        // Dir specifies the working directory of the command.
        // If Dir is the empty string, Run runs the command in the
        // calling process's current directory.
		  //Dir 指定 命令的工作目录.
		   // 如果Dir 是空字符串,Run 在调用进程的当前目录的命令。
        Dir string


        // Stdin specifies the process's standard input. If Stdin is
        // nil, the process reads from the null device (os.DevNull).
        //Stdin 指定进程的标准输入.如果Stdin 是nil, 进程从 null设备读取(os.DevNull).
        Stdin io.Reader


        // Stdout and Stderr specify the process's standard output and error.
        //
        // If either is nil, Run connects the corresponding file descriptor
        // to the null device (os.DevNull).
        //
        // If Stdout and Stderr are the same writer, at most one
        // goroutine at a time will call Write.
        //Stdout 和 Stderr 指定进程的标准输出和错误.
        // 如果 是nil, Run 连接对应的 文件描述符到 null设备 (os.DevNull).
        //如果Stdout 和 Stderr 有同样的写. 在同一时间至少有一个goroutine 调用 Write.
        Stdout io.Writer
        Stderr io.Writer



        // ExtraFiles specifies additional open files to be inherited by the
        // new process. It does not include standard input, standard output, or
        // standard error. If non-nil, entry i becomes file descriptor 3+i.
        //
        // BUG: on OS X 10.6, child processes may sometimes inherit unwanted fds.
        // http://golang.org/issue/2603
        //ExtraFiles 指定由继承额外打开的文件.它不包含标准输入, 标准输出 或标准错误. 如果 非nil, 项i变成文件描述符3+i.
        // BUG: 在 OS X 10.6上, 子进程可能在某些时候继承不必要的fds.http://golang.org/issue/2603
        ExtraFiles []*os.File



        // SysProcAttr holds optional, operating system-specific attributes.
        // Run passes it to os.StartProcess as the os.ProcAttr's Sys field.
        //SysProcAttr保留可选的，操作系统特定的属性。
        //Run其传递给os.StartProcess作为os.ProcAttr Sys字段.
        SysProcAttr *syscall.SysProcAttr



        // Process is the underlying process, once started.
        //Process是底层进程,启动一次.
        Process *os.Process


        // ProcessState contains information about an exited process,
        // available after a call to Wait or Run.
        //ProcessState包含退出进程有关信息, 之后可调用 Wait 或 Run.
        ProcessState *os.ProcessState
        // contains filtered or unexported fields
}
```
Cmd represents an external command being prepared or run.
Cmd表示一个正在编制或运行外部命令。


func Command
```golang
func Command(name string, arg ...string) *Cmd
```
Command returns the Cmd struct to execute the named program with the given arguments.

It sets only the Path and Args in the returned structure.

If name contains no path separators, Command uses LookPath to resolve the path to a complete name if possible. Otherwise it uses name directly.

The returned Cmd's Args field is constructed from the command name followed by the elements of arg, so arg should not include the command name itself.
For example, Command("echo", "hello")
Command 返回Cmd结构与给定的参数执行指定程序
它设置在返回的结构 的Path和 Args
如果name没有包含路径分隔符,Command 使用 LookPath 来解析 路径成一个完整的name ,如果可能的话.否则它直接使用name.
返回的Cmd 的Args 字段 是  从 命令name 跟着arg元素 构建的, 所以arg 不应该包含命令行name 本身.
比如, Command("echo", "hello")



▹ Example
```golang
package main

import (
	"bytes"
	"fmt"
	"log"
	"os/exec"
	"strings"
)

func main() {
	cmd := exec.Command("tr", "a-z", "A-Z")
	cmd.Stdin = strings.NewReader("some input")
	var out bytes.Buffer
	cmd.Stdout = &out
	err := cmd.Run()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("in all caps: %q\n", out.String())
}
```



func (*Cmd) CombinedOutput
```golang
func (c *Cmd) CombinedOutput() ([]byte, error)
```
CombinedOutput runs the command and returns its combined standard output and standard error.
CombinedOutput 执行命令 并返回它合成标准输出和标准错误。



func (*Cmd) Output
```golang
func (c *Cmd) Output() ([]byte, error)
```
Output runs the command and returns its standard output.
Output执行命令 并返回  它的标准输出.


▹ Example
```golang
package main

import (
	"fmt"
	"log"
	"os/exec"
)

func main() {
	out, err := exec.Command("date").Output()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("The date is %s\n", out)
}
```



func (*Cmd) Run
```golang
func (c *Cmd) Run() error
```
Run starts the specified command and waits for it to complete.

The returned error is nil if the command runs, has no problems copying stdin, stdout, and stderr, and exits with a zero exit status.

If the command fails to run or doesn't complete successfully, the error is of type *ExitError. Other error types may be returned for I/O problems.
Run启动指定的命令并等待它完成.
如果命令执行 返回error 是nil,  复制标输入 , 标准输出 和标准错误 并以零状态 退出 是没有问题的.
如果命令执行失败 或 没有成功 完成, 错误是*ExitError 类型. 其他错误类型可能为I / O问题被返回。



func (*Cmd) Start
```golang
func (c *Cmd) Start() error
```
Start starts the specified command but does not wait for it to complete.

The Wait method will return the exit code and release associated resources once the command exits.
Start启动指定的命令的但是不等待它完成.
Wait 方法将返回 退出码 和释放该命令退出有关的资源 一次.


▹ Example
```golang
package main

import (
	"log"
	"os/exec"
)

func main() {
	cmd := exec.Command("sleep", "5")
	err := cmd.Start()
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("Waiting for command to finish...")
	err = cmd.Wait()
	log.Printf("Command finished with error: %v", err)
}
```


func (*Cmd) StderrPipe
```golang
func (c *Cmd) StderrPipe() (io.ReadCloser, error)
```
StderrPipe returns a pipe that will be connected to the command's standard error when the command starts.

Wait will close the pipe after seeing the command exit, so most callers need not close the pipe themselves; 
however, an implication is that it is incorrect to call Wait before all reads from the pipe have completed. 
For the same reason, it is incorrect to use Run when using StderrPipe. See the StdoutPipe example for idiomatic usage.
StderrPipe 返回一个管道, 在命令开始的时候将连接到命令的标准错误.
Wait 将会在 命令退出后关闭管道, 所有 大部分调用者不必关闭管道本身;
然而. 从管道读取所有 完成之前 调用Wait是不正确的.
出于同样的原因, 当使用 StderrPipe 时它不正确的使用Run. 参见StdoutPipe的用法例子.



func (*Cmd) StdinPipe
```golang
func (c *Cmd) StdinPipe() (io.WriteCloser, error)
```
StdinPipe returns a pipe that will be connected to the command's standard input when the command starts. 
The pipe will be closed automatically after Wait sees the command exit.
A caller need only call Close to force the pipe to close sooner. 
For example, if the command being run will not exit until standard input is closed, the caller must close the pipe.
StdinPipe 返回一个管道 ,在命令开始的时候将连接到命令的标准错误.
在Wait 看到命令退出后 管道 将 自动关闭.
调用者只需要 调用Close 来强制让管道更快关闭.
例如, 如果命令执行 没有关闭 直到标准输入关闭, 调用者必须关闭该管道 


func (*Cmd) StdoutPipe
```golang
func (c *Cmd) StdoutPipe() (io.ReadCloser, error)
```
StdoutPipe returns a pipe that will be connected to the command's standard output when the command starts.

Wait will close the pipe after seeing the command exit, so most callers need not close the pipe themselves; 
however, an implication is that it is incorrect to call Wait before all reads from the pipe have completed. 
For the same reason, it is incorrect to call Run when using StdoutPipe. See the example for idiomatic usage.
StdoutPipe 返回一个管道, 在命令开始的时候将连接到命令的标准输出.
Wait 将会在 命令退出后关闭管道, 所有 大部分调用者不必关闭管道本身;
然而. 从管道读取所有 完成之前 调用Wait是不正确的.
出于同样的原因, 当使用 StdoutPipe 时它不正确的使用Run. 参见StdoutPipe的用法例子.


▹ Example
```golang
package main

import (
	"encoding/json"
	"fmt"
	"log"
	"os/exec"
)

func main() {
	cmd := exec.Command("echo", "-n", `{"Name": "Bob", "Age": 32}`)
	stdout, err := cmd.StdoutPipe()
	if err != nil {
		log.Fatal(err)
	}
	if err := cmd.Start(); err != nil {
		log.Fatal(err)
	}
	var person struct {
		Name string
		Age  int
	}
	if err := json.NewDecoder(stdout).Decode(&person); err != nil {
		log.Fatal(err)
	}
	if err := cmd.Wait(); err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s is %d years old\n", person.Name, person.Age)
}
```


func (*Cmd) Wait
```golang
func (c *Cmd) Wait() error
``
Wait waits for the command to exit. It must have been started by Start.

The returned error is nil if the command runs, has no problems copying stdin, stdout, and stderr, and exits with a zero exit status.

If the command fails to run or doesn't complete successfully, the error is of type *ExitError. Other error types may be returned for I/O problems.

Wait releases any resources associated with the Cmd.
Wait 等待命令退出.它必须是用 Start 启动的.
如果命令执行 返回的错误是nil,  复制标输入 , 标准输出 和标准错误 和以零状态 退出 是没有问题的.
如果命令执行失败 或 没有成功 完成, 错误是*ExitError 类型. 其他错误类型可能为I / O问题被返回。



type Error
```golang
type Error struct {
        Name string
        Err  error
}
```
Error records the name of a binary that failed to be executed and the reason it failed.
Error 记录 执行失败的二进制的名称 和它失败的原因.



func (*Error) Error
```golang
func (e *Error) Error() string
```


type ExitError
```golang
type ExitError struct {
        *os.ProcessState
}
```
An ExitError reports an unsuccessful exit by a command.
ExitError 报告不成功退出了命令。



func (*ExitError) Error
```golang
func (e *ExitError) Error() string
```







































