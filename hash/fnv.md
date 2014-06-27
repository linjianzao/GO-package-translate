#Package fnv        
##Overview 
        
Package fnv implements FNV-1 and FNV-1a, non-cryptographic hash functions created by Glenn Fowler, Landon Curt Noll, and Phong Vo.         
See http://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function.        
fnv包实现了FNV-1和FNV-1a,非加密哈希 函数  用 Glenn Fowler, Landon Curt Noll, 和 Phong Vo 创建         
                
##func New32        
```golang
func New32() hash.Hash32
```
New32 returns a new 32-bit FNV-1 hash.Hash.        
返回一个新的32位FNV-1 hash.Hash.        
        
        
##func New32a        
```golang
func New32a() hash.Hash32
```
New32a returns a new 32-bit FNV-1a hash.Hash.        
返回一个新的32位的FNV-1a hash.Hash.        
        
##func New64        
```golang
func New64() hash.Hash64        
```
New64 returns a new 64-bit FNV-1 hash.Hash.        
返回一个新的64位FNV-1 hash.Hash.        
        
##func New64a        
```golang
func New64a() hash.Hash64
        
New64a returns a new 64-bit FNV-1a hash.Hash.        
返回一个新的64位的FNV-1a hash.Hash.        
