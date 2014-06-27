#Package crc32        
        
##Overview         
        
Package crc32 implements the 32-bit cyclic redundancy check, or CRC-32, checksum.         
See http://en.wikipedia.org/wiki/Cyclic_redundancy_check for information.        
crc32包实现 32位 循环冗余校验 或者 CRC-32 校验        
        
        
##Constants        
```golang
const (
        // Far and away the most common CRC-32 polynomial.
        // Used by ethernet (IEEE 802.3), v.42, fddi, gzip, zip, png, mpeg-2, ...
           //最常见的CRC-32 多项式
           //使用 ethernet (IEEE 802.3), v.42, fddi, gzip, zip, png, mpeg-2, ...
        IEEE = 0xedb88320

        // Castagnoli's polynomial, used in iSCSI.
        // Has better error detection characteristics than IEEE.
        // http://dx.doi.org/10.1109/26.231911
        //Castagnoli的多项式,使用 iSCSI
          //比IEEE有更好的错误检测特性
        Castagnoli = 0x82f63b78

        // Koopman's polynomial.
        // Also has better error detection characteristics than IEEE.
        // http://dx.doi.org/10.1109/DSN.2002.1028931
         //也是比IEEE有更好的错误检测特性
        Koopman = 0xeb31d82e
)
```
Predefined polynomials.        
预定义的多项式。        
```golang
const Size = 4
```
The size of a CRC-32 checksum in bytes.        
一个以字节为单位的CRC-32校验的大小。       

##Variables
```golang
var IEEETable = makeTable(IEEE)
```
IEEETable is the table for the IEEE polynomial.        
IEEETable 是为IEEE多项式的表        
        
        
##func Checksum        
```golang
func Checksum(data []byte, tab *Table) uint32
```
Checksum returns the CRC-32 checksum of data using the polynomial represented by the Table.        
返回CRC-32 校验的数据 ,使用 Table表示多项式        
        
        
##func ChecksumIEEE        
```golang
func ChecksumIEEE(data []byte) uint32
```
ChecksumIEEE returns the CRC-32 checksum of data using the IEEE polynomial.        
使用IEEE多项式返回 CRC-32校验数据        
        
        
##func New        
```golang
func New(tab *Table) hash.Hash32
```
New creates a new hash.Hash32 computing the CRC-32 checksum using the polynomial represented by the Table.        
创建一个新的 hash.Hash32计算 CRC-32 校验,使用 Table表示多项式        
        
        
##func NewIEEE        
```golang
func NewIEEE() hash.Hash32
```
NewIEEE creates a new hash.Hash32 computing the CRC-32 checksum using the IEEE polynomial.        
创建一个新的hash.Hash32   使用IEEE多项式 计算CRC-32 校验        
        
        
##func Update        
```golang
func Update(crc uint32, tab *Table, p []byte) uint32
```
Update returns the result of adding the bytes in p to the crc.        
返回 从p添加到crc的字节结果        
        
        
##type Table        
```golang
type Table [256]uint32
```
Table is a 256-word table representing the polynomial for efficient processing.        
Table 是一个 256词  表示 为更有效的处理多项式。        
        
        
##func MakeTable        
```golang
func MakeTable(poly uint32) *Table
```
MakeTable returns the Table constructed from the specified polynomial.        
从指定的多项式返回 Table结构        
