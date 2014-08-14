#Package crc64        
        
##Overview        
        
Package crc64 implements the 64-bit cyclic redundancy check, or CRC-64, checksum.         
See http://en.wikipedia.org/wiki/Cyclic_redundancy_check for information.        
crc64包实现 32位 循环冗余校验 或者 CRC-64 校验           
        
##Constants        
```golang
const (
        // The ISO polynomial, defined in ISO 3309 and used in HDLC.
        ISO = 0xD800000000000000

        // The ECMA polynomial, defined in ECMA 182.
        ECMA = 0xC96C5795D7870F42
)
```
Predefined polynomials.        
预定义的多项式。        
```golang
const Size = 8
```
The size of a CRC-64 checksum in bytes.        
一个以字节为单位的CRC-64校验以字节为单位的大小。        
        
##func Checksum        
```golang
func Checksum(data []byte, tab *Table) uint64
```
Checksum returns the CRC-64 checksum of data using the polynomial represented by the Table.        
返回CRC-64 校验的数据 ,使用 Table表示多项式        
        

##func New        
```golang
func New(tab *Table) hash.Hash64
```
New creates a new hash.Hash64 computing the CRC-64 checksum using the polynomial represented by the Table.        
创建一个新的 hash.Hash64计算 CRC-64 校验,使用 Table表示多项式        
        
##func Update        
```golang
func Update(crc uint64, tab *Table, p []byte) uint64
```
Update returns the result of adding the bytes in p to the crc.        
返回 从p添加到crc的字节结果        
        
##type Table        
```golang
type Table [256]uint64
```
Table is a 256-word table representing the polynomial for efficient processing.        
Table 是一个 256词  表示 为更有效的处理多项式        
        
##func MakeTable        
```golang
func MakeTable(poly uint64) *Table
```
MakeTable returns the Table constructed from the specified polynomial.        
从指定的多项式返回 Table结构               
