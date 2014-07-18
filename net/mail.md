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
  
  
  
```

  
Variables

var ErrHeaderNotPresent = errors.New("mail: header not in message")
func ParseAddressList

func ParseAddressList(list string) ([]*Address, error)
ParseAddressList parses the given string as a list of addresses.

type Address

type Address struct {
        Name    string // Proper name; may be empty.
        Address string // user@domain
}
Address represents a single mail address. An address such as "Barry Gibbs <bg@example.com>" is represented as Address{Name: "Barry Gibbs", Address: "bg@example.com"}.

func ParseAddress

func ParseAddress(address string) (*Address, error)
Parses a single RFC 5322 address, e.g. "Barry Gibbs <bg@example.com>"

func (*Address) String

func (a *Address) String() string
String formats the address as a valid RFC 5322 address. If the address's name contains non-ASCII characters the name will be rendered according to RFC 2047.

type Header

type Header map[string][]string
A Header represents the key-value pairs in a mail message header.

func (Header) AddressList

func (h Header) AddressList(key string) ([]*Address, error)
AddressList parses the named header field as a list of addresses.

func (Header) Date

func (h Header) Date() (time.Time, error)
Date parses the Date header field.

func (Header) Get

func (h Header) Get(key string) string
Get gets the first value associated with the given key. If there are no values associated with the key, Get returns "".

type Message

type Message struct {
        Header Header
        Body   io.Reader
}
A Message represents a parsed mail message.

func ReadMessage

func ReadMessage(r io.Reader) (msg *Message, err error)
ReadMessage reads a message from r. The headers are parsed, and the body of the message will be available for reading from r.