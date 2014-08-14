Package smtp

Overview ▾

Package smtp implements the Simple Mail Transfer Protocol as defined in RFC 5321. It also implements the following extensions:
smtp包实现在RFC 5321定义的 简单邮件传输协议.它也实现以下扩展

```golang
8BITMIME  RFC 1652
AUTH      RFC 2554
STARTTLS  RFC 3207
```
Additional extensions may be handled by clients.
其他扩展名可以由客户端来处理。

Example

```golang
  // Connect to the remote SMTP server.
    c, err := smtp.Dial("mail.example.com:25")
    if err != nil {
            log.Fatal(err)
    }

    // Set the sender and recipient first
    if err := c.Mail("sender@example.org"); err != nil {
            log.Fatal(err)
    }
    if err := c.Rcpt("recipient@example.net"); err != nil {
            log.Fatal(err)
    }

    // Send the email body.
    wc, err := c.Data()
    if err != nil {
            log.Fatal(err)
    }
    _, err = fmt.Fprintf(wc, "This is the email body")
    if err != nil {
            log.Fatal(err)
    }
    err = wc.Close()
    if err != nil {
            log.Fatal(err)
    }

    // Send the QUIT command and close the connection.
    err = c.Quit()
    if err != nil {
            log.Fatal(err)
    }
```


func SendMail
```golang
func SendMail(addr string, a Auth, from string, to []string, msg []byte) error
```
SendMail connects to the server at addr, switches to TLS if possible, authenticates with the optional mechanism a if possible, and then sends an email from address from, to addresses to, with message msg.
SendMail 连接服务器 addr,如果可能的话切换到TLS, 如果可能,验证通过可选的机制a, 然后 从地址 from 发送一个包含 msg信息的 邮件,到地址 to


type Auth
```golang
type Auth interface {
        // Start begins an authentication with a server.
        // It returns the name of the authentication protocol
        // and optionally data to include in the initial AUTH message
        // sent to the server. It can return proto == "" to indicate
        // that the authentication should be skipped.
        // If it returns a non-nil error, the SMTP client aborts
        // the authentication attempt and closes the connection.
        //Starti 以一个服务器验证开始.
           //它返回验证协议的名字和 发送 包含在 初始 AUTH 可选的数据 信息 到 服务器
           //它可能返回 proto == ""表明 该验证必须跳过
           //如果它返回一个非nil错误, SMTP客户端 中止验证尝试 并 关闭连接
        Start(server *ServerInfo) (proto string, toServer []byte, err error)


        // Next continues the authentication. The server has just sent
        // the fromServer data. If more is true, the server expects a
        // response, which Next should return as toServer; otherwise
        // Next should return toServer == nil.
        // If Next returns a non-nil error, the SMTP client aborts
        // the authentication attempt and closes the connection.
        //Next继续 验证. 服务器只发送 fromServer 数据.
           // 如果more是true,服务器预计 一个 响应, Next 返回到 toServer;
           //否则,Next 必须返回toServer == nil.
           //如果 Next 返回一个非nil错误,SMTP 客户端 中止验证尝试 并 关闭连接
        Next(fromServer []byte, more bool) (toServer []byte, err error)
}
```
Auth is implemented by an SMTP authentication mechanism.
Auth 实现 SMTP认证机制。



func CRAMMD5Auth
```golang
func CRAMMD5Auth(username, secret string) Auth
```
CRAMMD5Auth returns an Auth that implements the CRAM-MD5 authentication mechanism as defined in RFC 2195. 
The returned Auth uses the given username and secret to authenticate to the server using the challenge-response mechanism.
CRAMMD5Auth 返回一个 Auth , 实现了定义在RFC 2195的 CRAM-MD5 验证机制.
返回 Auth 使用给定的 username 和 使用challenge-response机制   secret 验证服务器



func PlainAuth
```golang
func PlainAuth(identity, username, password, host string) Auth
```
PlainAuth returns an Auth that implements the PLAIN authentication mechanism as defined in RFC 4616. 
The returned Auth uses the given username and password to authenticate on TLS connections to host and act as identity. 
Usually identity will be left blank to act as username.
PlainAuth 返回一个 Auth 实现定义在 RFC 4616里的 PLAIN 验证机制
返回 Auth 使用给定的 username 和 username 来验证 在TLS 连接上的主机和身份。
通常的身份将被留空作为用户名


Example
```golang
package main

import (
	"log"
	"net/smtp"
)

func main() {
	// Set up authentication information.
	auth := smtp.PlainAuth("", "user@example.com", "password", "mail.example.com")

	// Connect to the server, authenticate, set the sender and recipient,
	// and send the email all in one step.
	to := []string{"recipient@example.net"}
	msg := []byte("This is the email body.")
	err := smtp.SendMail("mail.example.com:25", auth, "sender@example.org", to, msg)
	if err != nil {
		log.Fatal(err)
	}
}
```


type Client
```golang
type Client struct {
        // Text is the textproto.Conn used by the Client. It is exported to allow for
        // clients to add extensions.
        Text *textproto.Conn
        // contains filtered or unexported fields
}
```
A Client represents a client connection to an SMTP server.
Client表示一个连接到 SMTP服务器的客户端.


func Dial
```golang
func Dial(addr string) (*Client, error)
```
Dial returns a new Client connected to an SMTP server at addr. The addr must include a port number.
Dial 返回一个新的 连接到 在 addr 的SMTP 服务器的Client. addr 必须包含端口号.



func NewClient
```golang
func NewClient(conn net.Conn, host string) (*Client, error)
```
NewClient returns a new Client using an existing connection and host as a server name to be used when authenticating.
NewClient 当验证的时候 使用已经存在的 连接和当成服务器名来使用的host, 返回一个新的Client.



func (*Client) Auth
```golang
func (c *Client) Auth(a Auth) error
```
Auth authenticates a client using the provided authentication mechanism. 
A failed authentication closes the connection. Only servers that advertise the AUTH extension support this function.
Auth 使用提供的验证机制 验证一个客户端.
验证失败 会关闭连接. 只有服务器 通知 AUTH 扩展 支持这个函数.


func (*Client) Close
```golang
func (c *Client) Close() error
```
Close closes the connection.
Close关闭连接.


func (*Client) Data
```golang
func (c *Client) Data() (io.WriteCloser, error)
```
Data issues a DATA command to the server and returns a writer that can be used to write the data. 
The caller should close the writer before calling any more methods on c. 
A call to Data must be preceded by one or more calls to Rcpt.
Data 发出 一个 DATA 指令到服务器 并 返回一个writer 那可以用来 写数据.
调用者 在调用任何 c上的 更多的方法时 应该 关闭writer.
调用 Data 必须 在前面加一个或多个调用 Rcpt


func (*Client) Extension
```golang
func (c *Client) Extension(ext string) (bool, string)
```
Extension reports whether an extension is support by the server. 
The extension name is case-insensitive. 
If the extension is supported, Extension also returns a string that contains any parameters the server specifies for the extension.
Extension 报告 扩展是否支持服务器.
扩展名 不区分大小写。
如果扩展支持,Extension 也返回包含服务器指定的为扩展的任何参数 的字符串



func (*Client) Hello
```golang
func (c *Client) Hello(localName string) error
```
Hello sends a HELO or EHLO to the server as the given host name. 
Calling this method is only necessary if the client needs control over the host name used. 
The client will introduce itself as "localhost" automatically otherwise. 
If Hello is called, it must be called before any of the other methods.
Hello 发送一个 HELO 或 EHLO 到指定的主机名的服务器.
如果客户端需要主机名使用的控制权, 只需要调用这个方法.
客户端将会自动介绍它自己为 "localhost" 
如果调用Hello,它必须在任何其他方法前调用.



func (*Client) Mail
```golang
func (c *Client) Mail(from string) error
```
Mail issues a MAIL command to the server using the provided email address. 
If the server supports the 8BITMIME extension, Mail adds the BODY=8BITMIME parameter. 
This initiates a mail transaction and is followed by one or more Rcpt calls.
Mail 使用提供的邮件地址 发出一个 MAIL 指令到 服务器.
如果服务器支持8BITMIME 扩展,Mail 添加  BODY=8BITMIME参数.
这将启动一个邮件 和 跟着 一个或多个 Rcpt调用.



func (*Client) Quit
```golang
func (c *Client) Quit() error
```
Quit sends the QUIT command and closes the connection to the server.
Quit 发送 QUIT指令 和关闭到服务器的连接.



func (*Client) Rcpt
```golang
func (c *Client) Rcpt(to string) error
```
Rcpt issues a RCPT command to the server using the provided email address. 
A call to Rcpt must be preceded by a call to Mail and may be followed by a Data call or another Rcpt call.
Rcpt  使用提供的邮件地址 发出 一个 RCPT 指令到服务器
调用 Rcpt 前必须 优先调用 Mail 并且 可能跟着 调用 Data或其他的Rcpt调用.



func (*Client) Reset
```golang
func (c *Client) Reset() error
```
Reset sends the RSET command to the server, aborting the current mail transaction.
Reset 发送RSET 指令到服务器, 中止当前的邮件事务.


func (*Client) StartTLS
```golang
func (c *Client) StartTLS(config *tls.Config) error
```
StartTLS sends the STARTTLS command and encrypts all further communication. 
Only servers that advertise the STARTTLS extension support this function.
StartTLS 发送 STARTTLS 命令和 加密所有进一步的通讯。
只有服务器宣传 STARTTLS 扩展支持这个函数.



func (*Client) Verify
```golang
func (c *Client) Verify(addr string) error
```
Verify checks the validity of an email address on the server. 
If Verify returns nil, the address is valid. 
A non-nil return does not necessarily indicate an invalid address. 
Many servers will not verify addresses for security reasons.
Verify 检查 服务器的邮件地址的 合法性.
如果 Verify 返回nil,地址是有效的.
返回 非nil 不一定是 一个无效的地址.
许多服务器 因为安全原因不会验证地址.


type ServerInfo
```golang
type ServerInfo struct {
        Name string   // SMTP server name
        TLS  bool     // using TLS, with valid certificate for Name
        Auth []string // advertised authentication mechanisms
}
```
ServerInfo records information about an SMTP server.
ServerInfo 记录和SMTP 服务器有关的信息.

















