#Package hash          

##Overview
Package hash provides interfaces for hash functions.          
hash包提供哈希函数接口          
          
          
##type Hash          
```golang
type Hash interface {
        // Write (via the embedded io.Writer interface) adds more data to the running hash.
        // It never returns an error.
           //写入更多的数据到运行的哈希里(通过嵌入 io.Writer 接口)
           //不会返回错误
        io.Writer

        // Sum appends the current hash to b and returns the resulting slice.
        // It does not change the underlying hash state.
          //追加确定的hash到b 并且返回 结果slice
          //不会改变底层hash状态
        Sum(b []byte) []byte

        // Reset resets the Hash to its initial state.
          // 重置Hash 到初始状态
        Reset()

        // Size returns the number of bytes Sum will return.
        Size() int

        // BlockSize returns the hash's underlying block size.
        // The Write method must be able to accept any amount
        // of data, but it may operate more efficiently if all writes
        // are a multiple of the block size.
          //返回哈希底层块大小 ,Write方法必须可以接收任何数量的数据, 
          //但是也可以更高效, 如果所有的写操作是块大小的倍数。
        BlockSize() int
}
```
Hash is the common interface implemented by all hash functions.          
Hash是实现所有哈希函数的公共接口          
          
          
##type Hash32          
```golang
type Hash32 interface {
        Hash
        Sum32() uint32
}
```
Hash32 is the common interface implemented by all 32-bit hash functions.          
实现所有32位哈希函数的公共接口          
          
          
##type Hash64          
```golang
type Hash64 interface {
        Hash
        Sum64() uint64
}
```
Hash64 is the common interface implemented by all 64-bit hash functions.          
实现所有64位哈希函数的公共接口          