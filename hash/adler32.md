#Package adler32                   
          
##Overview ▾          
          
Package adler32 implements the Adler-32 checksum.          
It is defined in RFC 1950:          
adler32包实现了Adler-32验证          
          
```golang
Adler-32 is composed of two sums accumulated per byte: s1 is
the sum of all bytes, s2 is the sum of all s1 values. Both sums
are done modulo 65521. s1 is initialized to 1, s2 to zero.  The
Adler-32 checksum is stored as s2*65536 + s1 in most-
significant-byte first (network) order.

Adler-32是由两个字节 组成的:s1累加所有字节,s2累加所有s1的值.
对两个的总数模65521. s1初始是1, s2是零.
Adler-32验证 存储 s2*65536 + s1 在 最显著字节的第一个（网络）的顺序。
```

##Constants          
```golang
const Size = 4
```
The size of an Adler-32 checksum in bytes.          
          
          
##func Checksum          
```golang
func Checksum(data []byte) uint32
```
Checksum returns the Adler-32 checksum of data.          
返回Adler-32校验数据          
          
          
##func New          
```golang
func New() hash.Hash32
```
New returns a new hash.Hash32 computing the Adler-32 checksum.          
返回一个新的 hash.Hash32 计算Adler-32校验          