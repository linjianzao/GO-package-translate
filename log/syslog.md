#Package syslog

Overview ▾

Package syslog provides a simple interface to the system log service. 
It can send messages to the syslog daemon using UNIX domain sockets, UDP or TCP.
Only one call to Dial is necessary. 
On write failures, the syslog client will attempt to reconnect to the server and write again.
syslog包提供一个简单的 系统日志服务接口
它可以用 UNIX 域sockets , UDP 或TCP来发送信息到syslog 守护进程
只有一个调用 Dial 是必须的
写入失败的时候,syslog 端 将会尝试重连服务 并且 重新写入


func NewLogger

func NewLogger(p Priority, logFlag int) (*log.Logger, error)
NewLogger creates a log.Logger whose output is written to the system log service with the specified priority. 
The logFlag argument is the flag set passed through to log.New to create the Logger.
NewLogger创建一个 指定优先 输出写入到系统日志服务 的  log.Logger
logFlag参数是一个设置 通过 log.New 创建Logger 的标示



type Priority

type Priority int
The Priority is a combination of the syslog facility and severity. 
For example, LOG_ALERT | LOG_FTP sends an alert severity message from the FTP facility. 
The default severity is LOG_EMERG; the default facility is LOG_KERN.
Priority是一个系统日志工具和 严重程度的组合.
比如,LOG_ALERT | LOG_FTP 发送一个 从FTP工具 发送一个 警报严重性信息.
默认的严重性是 LOG_EMERG；默认的工具是 LOG_KERN.


const (

        // From /usr/include/sys/syslog.h.
        // These are the same on Linux, BSD, and OS X.
        LOG_EMERG Priority = iota
        LOG_ALERT
        LOG_CRIT
        LOG_ERR
        LOG_WARNING
        LOG_NOTICE
        LOG_INFO
        LOG_DEBUG
)
const (

        // From /usr/include/sys/syslog.h.
        // These are the same up to LOG_FTP on Linux, BSD, and OS X.
        LOG_KERN Priority = iota << 3
        LOG_USER
        LOG_MAIL
        LOG_DAEMON
        LOG_AUTH
        LOG_SYSLOG
        LOG_LPR
        LOG_NEWS
        LOG_UUCP
        LOG_CRON
        LOG_AUTHPRIV
        LOG_FTP

        LOG_LOCAL0
        LOG_LOCAL1
        LOG_LOCAL2
        LOG_LOCAL3
        LOG_LOCAL4
        LOG_LOCAL5
        LOG_LOCAL6
        LOG_LOCAL7
)



type Writer

type Writer struct {
        // contains filtered or unexported fields
}
A Writer is a connection to a syslog server.
一个Writer 是一个 系统日志服务器连接.


func Dial

func Dial(network, raddr string, priority Priority, tag string) (*Writer, error)
Dial establishes a connection to a log daemon by connecting to address raddr on the specified network. 
Each write to the returned writer sends a log message with the given facility, severity and tag.
If network is empty, Dial will connect to the local syslog server.
Dial建立一个  在指定的network上通过连接一个raddr地址 的日志守护进程连接.
每次写返回writer  用给定的工具 严重性和 tag 发送一个日志信息.
如果network 是空的, Dial 将连接到本地系统日志服务器.


func New

func New(priority Priority, tag string) (w *Writer, err error)
New establishes a new connection to the system log daemon. 
Each write to the returned writer sends a log message with the given priority and prefix.
New 建立一个 新的 系统日志守护进程 连接.
每次写返回writer  用给定的优先级和前缀发送一个日志信息.


func (*Writer) Alert

func (w *Writer) Alert(m string) (err error)
Alert logs a message with severity LOG_ALERT, ignoring the severity passed to New.
Alert 以严重程度LOG_ALERT 记录一个信息,忽略传递给New的严重程度。


func (*Writer) Close

func (w *Writer) Close() error
Close closes a connection to the syslog daemon.
Close关闭一个连接的系统日志 守护进程


func (*Writer) Crit

func (w *Writer) Crit(m string) (err error)
Crit logs a message with severity LOG_CRIT, ignoring the severity passed to New.
Crit以严重程度LOG_CRIT记录一个信息,忽略传递给New的严重程度。


func (*Writer) Debug

func (w *Writer) Debug(m string) (err error)
Debug logs a message with severity LOG_DEBUG, ignoring the severity passed to New.
Debug以严重程度LOG_DEBUG记录一个信息,忽略传递给New的严重程度。


func (*Writer) Emerg

func (w *Writer) Emerg(m string) (err error)
Emerg logs a message with severity LOG_EMERG, ignoring the severity passed to New.
Emerg以严重程度LOG_EMERG记录一个信息, 忽略传递给New的严重程度。


func (*Writer) Err

func (w *Writer) Err(m string) (err error)
Err logs a message with severity LOG_ERR, ignoring the severity passed to New.
Err以严重程度LOG_ERR记录一个信息, 忽略传递给New的严重程度。


func (*Writer) Info

func (w *Writer) Info(m string) (err error)
Info logs a message with severity LOG_INFO, ignoring the severity passed to New.
Info以严重程度LOG_INFO记录一个信息, 忽略传递给New的严重程度。


func (*Writer) Notice

func (w *Writer) Notice(m string) (err error)
Notice logs a message with severity LOG_NOTICE, ignoring the severity passed to New.
Notice以严重程度LOG_NOTICE记录一个信息, 忽略传递给New的严重程度。



func (*Writer) Warning

func (w *Writer) Warning(m string) (err error)
Warning logs a message with severity LOG_WARNING, ignoring the severity passed to New.
Warning以严重程度LOG_WARNING记录一个信息, 忽略传递给New的严重程度。



func (*Writer) Write

func (w *Writer) Write(b []byte) (int, error)
Write sends a log message to the syslog daemon.
Write发送一个日志信息到系统日志守护进程