包地址：http://golang.org/pkg/encoding/pem/
```golang
Package pem implements the PEM data encoding, which originated in Privacy Enhanced Mail. 
The most common use of PEM encoding today is in TLS keys and certificates. See RFC 1421.
pem包实现PEM数据编码.它起源于增强保密邮件
今天用PEM编码更多的是 TLS keys 和证书
```

func Encode
```golang
func Encode(out io.Writer, b *Block) error
```

func EncodeToMemory
```golang
func EncodeToMemory(b *Block) []byte
```

type Block
```golang
type Block struct {
    Type    string            // The type, taken from the preamble (i.e. "RSA PRIVATE KEY").
    Headers map[string]string // Optional headers.
    Bytes   []byte            // The decoded bytes of the contents. Typically a DER encoded ASN.1 structure.
}
A Block represents a PEM encoded structure.
Block表示一个PEN编码结构

The encoded form is:
编码格式是:
```golang
-----BEGIN Type-----
Headers
base64-encoded Bytes
-----END Type-----
```
where Headers is a possibly empty sequence of Key: Value lines.
Headers有可能是空的序列key:值行
```

func Decode
```golang
func Decode(data []byte) (p *Block, rest []byte)
	Decode will find the next PEM formatted block (certificate, private key etc) in the input. 
	It returns that block and the remainder of the input. If no PEM data is found, p is nil and the whole of the input is returned in rest.
	在输入流查找下一个PEM 格式块
	它返回找到的块和其余的输入部分.如果 PEM数据没有找到p是 nil 并且 输入会完整的在rest里
```