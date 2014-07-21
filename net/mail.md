Package mail

import "net/mail"


Overview ▾

Package mail implements parsing of mail messages.

For the most part, this package follows the syntax as specified by RFC 5322. Notable divergences:

mail包实现 解析邮件信息.
在大多数情况下， 这个包   的语法在 RFC 5322 指定.值得注意的分歧：

```golang
* Obsolete address formats are not parsed, including addresses with
  embedded route information.
* Group addresses are not parsed.
* The full range of spacing (the CFWS syntax element) is not supported,
  such as breaking addresses across lines.
  
*  过时的地址格式是不会被解析，包括与地址嵌入式路由信息。 
*  组地址不会被解析
*  不支持全系列的间距（在CFWS语法元素）,比如跨线阻断地址。
  
```

  
Variables
```golang
var ErrHeaderNotPresent = errors.New("mail: header not in message")
```


func ParseAddressList
```golang
func ParseAddressList(list string) ([]*Address, error)
```
ParseAddressList parses the given string as a list of addresses.
ParseAddressList解析给定字符串成地址列表.


type Address
```golang
type Address struct {
        Name    string // Proper name; may be empty.
        Address string // user@domain
}
```
Address represents a single mail address. 
An address such as "Barry Gibbs <bg@example.com>" is represented as Address{Name: "Barry Gibbs", Address: "bg@example.com"}.
Address表示单个邮件地址.
地址 类似  "Barry Gibbs <bg@example.com>"  它表示 成  Address{Name: "Barry Gibbs", Address: "bg@example.com"}.



func ParseAddress
```golang
func ParseAddress(address string) (*Address, error)
```
Parses a single RFC 5322 address, e.g. "Barry Gibbs <bg@example.com>"
Parses 单个  RFC 5322 地址, 例如 "Barry Gibbs <bg@example.com>"



func (*Address) String
```golang
func (a *Address) String() string
```
String formats the address as a valid RFC 5322 address. 
If the address's name contains non-ASCII characters the name will be rendered according to RFC 2047.
String 格式话地址i成一个有效的 RFC 5322 地址.
如果 地址的 名字 包含 非ASCII 字符  该名字将会 根据RFC2047呈现。


type Header
```golang
type Header map[string][]string
```
A Header represents the key-value pairs in a mail message header.
Header 表示 邮件信息头里的 键-值对



func (Header) AddressList
```golang
func (h Header) AddressList(key string) ([]*Address, error)
```
AddressList parses the named header field as a list of addresses.
AddressList 解析 命名头 字段成 地址列表.



func (Header) Date
```golang
func (h Header) Date() (time.Time, error)
```
Date parses the Date header field.
Date 解析 Date 头 字段


func (Header) Get
```golang
func (h Header) Get(key string) string
```
Get gets the first value associated with the given key. If there are no values associated with the key, Get returns "".
Get 过去 和给定的key有关的第一个值. 如果 没有值对应的key ,Get 返回 " ".



type Message
```golang
type Message struct {
        Header Header
        Body   io.Reader
}
```
A Message represents a parsed mail message.
Message 表示一个解析的邮件信息.



func ReadMessage
```golang
func ReadMessage(r io.Reader) (msg *Message, err error)
```
ReadMessage reads a message from r. 
The headers are parsed, and the body of the message will be available for reading from r.
ReadMessage从r中读取一个信息.
头 解析, 可从r读取邮件的正文。
























