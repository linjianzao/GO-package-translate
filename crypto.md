包地址：http://golang.org/pkg/crypto/

```golang
Package crypto collects common cryptographic constants.
常见加密项的集合
```

func RegisterHash
```golang
	func RegisterHash(h Hash, f func() hash.Hash)
		RegisterHash registers a function that returns a new instance of the given hash function. 
		This is intended to be called from the init function in packages that implement hash functions.
		注册一个函数，返回 给定的hash函数的新实例
		这意味着从包里的初始化函数调用 实现的hash函数
```

type Hash
```golang
type Hash uint
Hash identifies a cryptographic hash function that is implemented in another package.
标示在其他包里实现的hash加密函数

const (
    MD4       Hash = 1 + iota // import code.google.com/p/go.crypto/md4
    MD5                       // import crypto/md5
    SHA1                      // import crypto/sha1
    SHA224                    // import crypto/sha256
    SHA256                    // import crypto/sha256
    SHA384                    // import crypto/sha512
    SHA512                    // import crypto/sha512
    MD5SHA1                   // no implementation; MD5+SHA1 used for TLS RSA
    RIPEMD160                 // import code.google.com/p/go.crypto/ripemd160
)
```

func (Hash) Available
```golang
	func (h Hash) Available() bool
		Available reports whether the given hash function is linked into the binary.
		返回给定的hash函数是否链接到二进制
```

func (Hash) New
```golang
	func (h Hash) New() hash.Hash
		New returns a new hash.Hash calculating the given hash function. 
		New panics if the hash function is not linked into the binary.
		返回使用给定的hash函数计算的 hash.Hash
		如果hash函数没有链接到二进制会产生新的panics(恐慌)
```

func (Hash) Size
```golang
	func (h Hash) Size() int
		Size returns the length, in bytes, of a digest resulting from the given hash function. 
		It doesn't require that the hash function in question be linked into the program.
		
		使用给定的hash函数返回 以字节为单位 的 内容的长度 。
		如果hash函数链接到程序有问题 它不是必须的
```

type PrivateKey
```golang
	type PrivateKey interface{}
		PrivateKey represents a private key using an unspecified algorithm.
		使用未指定的算法 代表一个私钥
```

type PublicKey
```golang
	type PublicKey interface{}
		PublicKey represents a public key using an unspecified algorithm.
		使用未指定的算法 代表一个公钥
```

//这有很多子包就不翻了


