Package big

Overview ▾

Package big implements multi-precision arithmetic (big numbers). 
The following numeric types are supported:
big实现多精度算术(大的数字)
支持以下的数字类型:

- Int	signed integers
- Rat	rational numbers


Methods are typically of the form:
方法通常是以下形式：

func (z *Int) Op(x, y *Int) *Int	(similar for *Rat)

and implement operations z = x Op y with the result as receiver; 
if it is one of the operands it may be overwritten (and its memory reused). 
To enable chaining of operations, the result is also returned. 
Methods returning a result other than *Int or *Rat take one of the operands as the receiver.
实现  z = x Op y 操作 计算的结果作为接收器;
如果它是一个操作数可能会被覆盖
为了使操作的链接，也传回的结果。
方法返回一个结果 比其他的 *Int  或 *Rat  取操作数作为接收器中的一个。



Constants

const MaxBase = 'z' - 'a' + 10 + 1 // = hexValue('z') + 1
MaxBase is the largest number base accepted for string conversions.
MaxBase 是接受 字符串转化的最大  基数


type Int

type Int struct {
        // contains filtered or unexported fields
}
An Int represents a signed multi-precision integer. 
The zero value for an Int represents the value 0.
一个Int 表示单个多精度整型.
Int的零值 是 0


func NewInt

func NewInt(x int64) *Int
NewInt allocates and returns a new Int set to x.
NewInt 分配并返回一个新的Int 设置x



func (*Int) Abs

func (z *Int) Abs(x *Int) *Int
Abs sets z to |x| (the absolute value of x) and returns z.
Abs 设置z 成  |x| (x的绝对值) 并返回z


func (*Int) Add

func (z *Int) Add(x, y *Int) *Int
Add sets z to the sum x+y and returns z.
Add 设置z成  x+y的和并 返回z


func (*Int) And

func (z *Int) And(x, y *Int) *Int
And sets z = x & y and returns z.
And 设置z= x & y  并返回z



func (*Int) AndNot

func (z *Int) AndNot(x, y *Int) *Int
AndNot sets z = x &^ y and returns z.
AndNot设置z = x &^ y 并返回z



func (*Int) Binomial

func (z *Int) Binomial(n, k int64) *Int
Binomial sets z to the binomial coefficient of (n, k) and returns z.
Binomial设置z为（N，K）的二项式系数和返回z。


func (*Int) Bit

func (x *Int) Bit(i int) uint
Bit returns the value of the i'th bit of x. 
That is, it returns (x>>i)&1. The bit index i must be >= 0.
Bit返回x的第i位的值。


func (*Int) BitLen

func (x *Int) BitLen() int
BitLen returns the length of the absolute value of x in bits. 
The bit length of 0 is 0.
返回x的位绝对值的长度。
0的位长度是0


func (*Int) Bits

func (x *Int) Bits() []Word
Bits provides raw (unchecked but fast) access to x by returning its absolute value as a little-endian Word slice. 
The result and x share the same underlying array. 
Bits is intended to support implementation of missing low-level Int functionality outside this package; 
it should be avoided otherwise.
Bits提供 以返回 它的一个小端Word slice 绝对值 访问x
返回值和x 共享同一个 底层数组.
Bits的目的是支持执行这个包之外缺少低层次的Int功能;
否则,应该避免。


func (*Int) Bytes

func (x *Int) Bytes() []byte
Bytes returns the absolute value of x as a big-endian byte slice.
以大端字节 slice 返回 x的绝对值


func (*Int) Cmp

func (x *Int) Cmp(y *Int) (r int)
Cmp compares x and y and returns:
Cmp比较x和y，并返回：

-1 if x <  y
 0 if x == y
+1 if x >  y




func (*Int) Div

func (z *Int) Div(x, y *Int) *Int
Div sets z to the quotient x/y for y != 0 and returns z. 
If y == 0, a division-by-zero run-time panic occurs. 
Div implements Euclidean division (unlike Go); 
see DivMod for more details.
Div 设置z 成x/y的商, y != 0 并返回z.
如果y==0, 运行的时候会遇到panic.
Div实现了辗转相除(不像GO);
更多详细信息 查看 DivMod



func (*Int) DivMod

func (z *Int) DivMod(x, y, m *Int) (*Int, *Int)
DivMod sets z to the quotient x div y and m to the modulus x mod y and returns the pair (z, m) for y != 0. 
If y == 0, a division-by-zero run-time panic occurs.
DivMod implements Euclidean division and modulus (unlike Go):
DivMod设置z成x除y的商 并且m 是x模 y  返回 (z, m)对 ,y != 0.
如果y==0,运行的时候会遇到panic.
Div实现了辗转相除(不像GO);


q = x div y  such that
m = x - y*q  with 0 <= m < |q|

 
(See Raymond T. Boute, “The Euclidean definition of the functions div and mod”. 
	ACM Transactions on Programming Languages and Systems (TOPLAS), 14(2):127-144, New York, NY, USA, 4/1992. ACM press.) 
See QuoRem for T-division and modulus (like Go).



func (*Int) Exp

func (z *Int) Exp(x, y, m *Int) *Int
Exp sets z = x**y mod |m| (i.e. the sign of m is ignored), and returns z. 
If y <= 0, the result is 1 mod |m|; 
if m == nil or m == 0, z = x**y. 
See Knuth, volume 2, section 4.6.3.
Exp设置 z = x**y  模  |m| (即m的符号被忽略), 并且返回z.
如果y<=0,结果是1 模 |m|;
如果m == nil 或 m == 0, z = x**y.



func (*Int) Format

func (x *Int) Format(s fmt.State, ch rune)
Format is a support routine for fmt.Formatter. 
It accepts the formats 'b' (binary), 'o' (octal), 'd' (decimal), 'x' (lowercase hexadecimal), and 'X' (uppercase hexadecimal). 
Also supported are the full suite of package fmt's format verbs for integral types, 
	including '+', '-', and ' ' for sign control, '#' for leading zero in octal and for hexadecimal, 
	a leading "0x" or "0X" for "%#x" and "%#X" respectively, specification of minimum digits precision, 
	output field width, space or zero padding, and left or right justification.
Format 是fmt.Formatter支持例程。
它接收格式'b' (二进制), 'o' (八进制), 'd' (十进制), 'x' (小写十六进制), and 'X' (大写十六进制).
另外还支持全套fmt包的格式动词整型，
包含 '+', '-', 和 ' ' 符号控制i,'#' 八进制和十六进制前导零，
最小数字精度前导 "0x" 或 "0X" 分别为 "%#x" 和 "%#X" 
输出字段宽度，空格或零填充，和左或右对齐。



func (*Int) GCD

func (z *Int) GCD(x, y, a, b *Int) *Int
GCD sets z to the greatest common divisor of a and b, which both must be > 0, and returns z. 
If x and y are not nil, GCD sets x and y such that z = a*x + b*y. 
If either a or b is <= 0, GCD sets z = x = y = 0.
GCD设置z 成a和b的最大公约数，两个都必须 大于0  并且返回z.
如果x和y 都不是nil,GCD 设置x和y 就像  z = a*x + b*y. 
如果a或b 小于0 , GCD 设置  z = x = y = 0.



func (*Int) GobDecode

func (z *Int) GobDecode(buf []byte) error
GobDecode implements the gob.GobDecoder interface.
GobDecode实现 gob.GobDecoder 接口



func (*Int) GobEncode

func (x *Int) GobEncode() ([]byte, error)
GobEncode implements the gob.GobEncoder interface.
GobEncode实现 gob.GobEncoder接口



func (*Int) Int64

func (x *Int) Int64() int64
Int64 returns the int64 representation of x. 
If x cannot be represented in an int64, the result is undefined.
返回x的int64的表示。
如果x不能用int64来表示，结果是undefined。



func (*Int) Lsh

func (z *Int) Lsh(x *Int, n uint) *Int
Lsh sets z = x << n and returns z.
Lsh设置 z = x << n 并返回z



func (*Int) MarshalJSON

func (z *Int) MarshalJSON() ([]byte, error)
MarshalJSON implements the json.Marshaler interface.
MarshalJSON实现 json.Marshaler接口



func (*Int) MarshalText

func (z *Int) MarshalText() (text []byte, err error)
MarshalText implements the encoding.TextMarshaler interface
MarshalText实现 encoding.TextMarshaler接口



func (*Int) Mod

func (z *Int) Mod(x, y *Int) *Int
Mod sets z to the modulus x%y for y != 0 and returns z. 
If y == 0, a division-by-zero run-time panic occurs. 
Mod implements Euclidean modulus (unlike Go); 
see DivMod for more details.
Mod设置z 为 x%y ,y!=0 并返回z
如果y==0,运行的时候会遇到painc
Mod实现Euclidean模(不像GO)
更多详细信息 查看 DivMod




func (*Int) ModInverse

func (z *Int) ModInverse(g, p *Int) *Int
ModInverse sets z to the multiplicative inverse of g in the group ℤ/pℤ (where p is a prime) and returns z.
设置z为g组中的乘法逆ℤ/ Pℤ(其中，p是一个素数),并返回z



func (*Int) Mul

func (z *Int) Mul(x, y *Int) *Int
Mul sets z to the product x*y and returns z.
Mul设置z为x*y , 并返回z



func (*Int) MulRange

func (z *Int) MulRange(a, b int64) *Int
MulRange sets z to the product of all integers in the range [a, b] inclusively and returns z. 
If a > b (empty range), the result is 1.
MulRange设置 z为 所有在 [a, b]范围里的整型（含） 并返回z



func (*Int) Neg

func (z *Int) Neg(x *Int) *Int
Neg sets z to -x and returns z.
Neg设置z 为-x 并返回z



func (*Int) Not

func (z *Int) Not(x *Int) *Int
Not sets z = ^x and returns z.
设置z = ^x 并返回z


func (*Int) Or

func (z *Int) Or(x, y *Int) *Int
Or sets z = x | y and returns z.
Or 设置 z = x | y 并返回z



func (*Int) ProbablyPrime

func (x *Int) ProbablyPrime(n int) bool
ProbablyPrime performs n Miller-Rabin tests to check whether x is prime. 
If it returns true, x is prime with probability 1 - 1/4^n. 
If it returns false, x is not prime.
ProbablyPrime执行 n Miller-Rabin测试来检查x是否 是素数.
如果返回true ,x是概率为1 - 1/4的n次方素数 
如果返回 false, x不是素数



func (*Int) Quo

func (z *Int) Quo(x, y *Int) *Int
Quo sets z to the quotient x/y for y != 0 and returns z. 
If y == 0, a division-by-zero run-time panic occurs. 
Quo implements truncated division (like Go);
see QuoRem for more details.
Quo设置z为 x/y的商 ,y!=0 并返回z.
如果y==0, 运行的 时候会遇到panic
Quo实现截断除法 (类似GO)
更多相信信息查看QuoRem



func (*Int) QuoRem

func (z *Int) QuoRem(x, y, r *Int) (*Int, *Int)
QuoRem sets z to the quotient x/y and r to the remainder x%y and returns the pair (z, r) for y != 0. 
If y == 0, a division-by-zero run-time panic occurs.
QuoRem implements T-division and modulus (like Go):

QuoRem设置z成 x/y的商 和 r成 x%y 剩余 , 返回  (z, r) 对, y != 0.
如果 y == 0, 运行时会遇到 panic
QuoRem实现 T-division 和模(类似GO):

q = x/y      with the result truncated to zero
r = x - y*q

(See Daan Leijen, “Division and Modulus for Computer Scientists”.) See DivMod for Euclidean division and modulus (unlike Go).



func (*Int) Rand

func (z *Int) Rand(rnd *rand.Rand, n *Int) *Int
Rand sets z to a pseudo-random number in [0, n) and returns z.
Rand设置z成[0, n)范围内的伪随机数 并返回z


func (*Int) Rem

func (z *Int) Rem(x, y *Int) *Int
Rem sets z to the remainder x%y for y != 0 and returns z. 
If y == 0, a division-by-zero run-time panic occurs. 
Rem implements truncated modulus (like Go); see QuoRem for more details.
Rem设置z成x%y剩余 , y != 0 并返回z.
如果y == 0,运行时会遇到 panic.
Rem 实现截断除法(like Go); 更多详细信息查看 QuoRem.



func (*Int) Rsh

func (z *Int) Rsh(x *Int, n uint) *Int
Rsh sets z = x >> n and returns z.
Rsh 设置 z = x >> n 并返回z



func (*Int) Scan

func (z *Int) Scan(s fmt.ScanState, ch rune) error
Scan is a support routine for fmt.Scanner; 
it sets z to the value of the scanned number. 
It accepts the formats 'b' (binary), 'o' (octal), 'd' (decimal), 'x' (lowercase hexadecimal), and 'X' (uppercase hexadecimal).
Format 是fmt.Scanner支持例程。
它设置z成 scanned数字值.
它接收格式'b' (二进制), 'o' (八进制), 'd' (十进制), 'x' (小写十六进制), and 'X' (大写十六进制).

Example:
package main

import (
	"fmt"
	"log"
	"math/big"
)

func main() {
	// The Scan function is rarely used directly;
	// the fmt package recognizes it as an implementation of fmt.Scanner.
	i := new(big.Int)
	_, err := fmt.Sscan("18446744073709551617", i)
	if err != nil {
		log.Println("error scanning value:", err)
	} else {
		fmt.Println(i)
	}
}



func (*Int) Set

func (z *Int) Set(x *Int) *Int
Set sets z to x and returns z.
Set设置z成x 并返回z

func (*Int) SetBit

func (z *Int) SetBit(x *Int, i int, b uint) *Int
SetBit sets z to x, with x's i'th bit set to b (0 or 1). 
That is, if b is 1 SetBit sets z = x | (1 << i); 
if b is 0 SetBit sets z = x &^ (1 << i). 
If b is not 0 or 1, SetBit will panic.
SetBit设置z成x,其中x的第i个比特设置为b（0或1）
如果b是0, SetBit 设置 z = x &^ (1 << i).
如果是非0或1,SetBit会 panic



func (*Int) SetBits

func (z *Int) SetBits(abs []Word) *Int
SetBits provides raw (unchecked but fast) access to z by setting its value to abs, interpreted as a little-endian Word slice, and returning z. 
The result and abs share the same underlying array. 
SetBits is intended to support implementation of missing low-level Int functionality outside this package; 
it should be avoided otherwise.
Bits提供 以返回 它的一个小端Word slice 绝对值 访问x,并返回z.
结果和abs共享同一个底层数组.
Bits的目的是支持执行这个包之外缺少低层次的Int功能;
否则,应该避免.



func (*Int) SetBytes

func (z *Int) SetBytes(buf []byte) *Int
SetBytes interprets buf as the bytes of a big-endian unsigned integer, sets z to that value, and returns z.
SetBytesbuf解释为大端无符号整数的字节,设置z成那个值 并返回z.



func (*Int) SetInt64

func (z *Int) SetInt64(x int64) *Int
SetInt64 sets z to x and returns z.
SetInt64 设置z成x, 并返回z


func (*Int) SetString

func (z *Int) SetString(s string, base int) (*Int, bool)
SetString sets z to the value of s, interpreted in the given base, and returns z and a boolean indicating success. 
If SetString fails, the value of z is undefined but the returned value is nil.

The base argument must be 0 or a value from 2 through MaxBase. 
If the base is 0, the string prefix determines the actual conversion base.
A prefix of “0x” or “0X” selects base 16;
the “0” prefix selects base 8, and a “0b” or “0B” prefix selects base 2. 
Otherwise the selected base is 10.
SetString 设置z成s的值,解释给定的base, 并返回 z 和 一个布尔值，表示成功。
如果SetString 失败,z的值是undefined 但是返回的值是nil
base参数 必须是 0或 2 通过MaxBase的值
“0x”或“0X”前缀选择base为16;
“0”前缀 选择base8,“0b” 或 “0B”前缀选择 base 2.
否则选择base是10


▹ Example
package main

import (
	"fmt"
	"math/big"
)

func main() {
	i := new(big.Int)
	i.SetString("644", 8) // octal
	fmt.Println(i)
}



func (*Int) SetUint64

func (z *Int) SetUint64(x uint64) *Int
SetUint64 sets z to x and returns z.
SetUint64设置z成x 并返回z



func (*Int) Sign

func (x *Int) Sign() int
Sign returns:

-1 if x <  0
 0 if x == 0
+1 if x >  0


func (*Int) String

func (x *Int) String() string



func (*Int) Sub

func (z *Int) Sub(x, y *Int) *Int
Sub sets z to the difference x-y and returns z.
Sub设置z 为 x-y 并返回z.



func (*Int) Uint64

func (x *Int) Uint64() uint64
Uint64 returns the uint64 representation of x. If x cannot be represented in a uint64, the result is undefined.
返回x的 uint64表示.如果x不能 表示成uint64, 结果是undefined


func (*Int) UnmarshalJSON

func (z *Int) UnmarshalJSON(text []byte) error
UnmarshalJSON implements the json.Unmarshaler interface.
UnmarshalJSON 实现  json.Unmarshaler接口



func (*Int) UnmarshalText

func (z *Int) UnmarshalText(text []byte) error
UnmarshalText implements the encoding.TextUnmarshaler interface
实现 json.Unmarshaler接口



func (*Int) Xor

func (z *Int) Xor(x, y *Int) *Int
Xor sets z = x ^ y and returns z.
Xor设置  z = x ^ y 并返回z




type Rat

type Rat struct {
        // contains filtered or unexported fields
}
A Rat represents a quotient a/b of arbitrary precision. 
The zero value for a Rat represents the value 0.
Rat 表示一个 任意精度的 a/b
Rat的零值 是0



func NewRat

func NewRat(a, b int64) *Rat
NewRat creates a new Rat with numerator a and denominator b.
NewRat创建一个新的Rat ,a是分子, b是分母.



func (*Rat) Abs

func (z *Rat) Abs(x *Rat) *Rat
Abs sets z to |x| (the absolute value of x) and returns z.
Abs设置z成 |x| (x的绝对值)  并返回z



func (*Rat) Add

func (z *Rat) Add(x, y *Rat) *Rat
Add sets z to the sum x+y and returns z.
Add 设置z成 x+y 并返回z



func (*Rat) Cmp

func (x *Rat) Cmp(y *Rat) int
Cmp compares x and y and returns:
Cmp比较x和y 并返回:

-1 if x <  y
 0 if x == y
+1 if x >  y



func (*Rat) Denom

func (x *Rat) Denom() *Int
Denom returns the denominator of x; 
it is always > 0. 
The result is a reference to x's denominator; 
it may change if a new value is assigned to x, and vice versa.
Denom返回x的分母.
它通常是 大于0.
其结果是一个变量x的分母;
如果一个新值被赋值给x, 它可能会改变 , 反之亦然。



func (*Rat) Float64

func (x *Rat) Float64() (f float64, exact bool)
Float64 returns the nearest float64 value for x and a bool indicating whether f represents x exactly. 
If the magnitude of x is too large to be represented by a float64, f is an infinity and exact is false. 
The sign of f always matches the sign of x, even if f == 0.
Float64返回 最接近x的float64值 和一个布尔值 , 指示f是否x完全地.
如果x 的大小 太大,无法通过的float64代表,f是一个无穷大，准确的说是false的。
f的符号总是匹配x的符号，即使如果f==0。



func (*Rat) FloatString

func (x *Rat) FloatString(prec int) string
FloatString returns a string representation of x in decimal form with prec digits of precision after the decimal point and the last digit rounded.
FloatString返回x的十进制形式小数点后的字符串表示形式与PREC位精度和最后一位四舍五入。


func (*Rat) GobDecode

func (z *Rat) GobDecode(buf []byte) error
GobDecode implements the gob.GobDecoder interface.
GobDecode 实现  gob.GobDecoder接口


func (*Rat) GobEncode

func (x *Rat) GobEncode() ([]byte, error)
GobEncode implements the gob.GobEncoder interface.
GobEncode 实现 gob.GobEncoder接口



func (*Rat) Inv

func (z *Rat) Inv(x *Rat) *Rat
Inv sets z to 1/x and returns z.
Inv设置z成1/x 并返回z


func (*Rat) IsInt

func (x *Rat) IsInt() bool
IsInt returns true if the denominator of x is 1.
如果x的分母是1返回true



func (*Rat) MarshalText

func (r *Rat) MarshalText() (text []byte, err error)
MarshalText implements the encoding.TextMarshaler interface
MarshalText实现encoding.TextMarshaler接口



func (*Rat) Mul

func (z *Rat) Mul(x, y *Rat) *Rat
Mul sets z to the product x*y and returns z.
设置z 成  x*y 并返回z



func (*Rat) Neg

func (z *Rat) Neg(x *Rat) *Rat
Neg sets z to -x and returns z.
设置z成-x 并返回z


func (*Rat) Num

func (x *Rat) Num() *Int
Num returns the numerator of x; 
it may be <= 0. 
The result is a reference to x's numerator; 
it may change if a new value is assigned to x, and vice versa. 
The sign of the numerator corresponds to the sign of x.
Num返回 x的分子
它可能是小于等于0的.
其结果是一个变量x的分子;
如果新值赋值成x 它可能会改变,反之亦然。
分子的符号对应于x的符号。



func (*Rat) Quo

func (z *Rat) Quo(x, y *Rat) *Rat
Quo sets z to the quotient x/y and returns z. If y == 0, a division-by-zero run-time panic occurs.
Quo设置z成  x/y 并返回z.如果y==0,运行时会遇到panic



func (*Rat) RatString

func (x *Rat) RatString() string
RatString returns a string representation of x in the form "a/b" if b != 1, and in the form "a" if b == 1.
RatString 返回x的形式的字符串表示" a/b" 如果 b!=1, 如果b==1 ,形式是  "a"


func (*Rat) Scan

func (z *Rat) Scan(s fmt.ScanState, ch rune) error
Scan is a support routine for fmt.Scanner. 
It accepts the formats 'e', 'E', 'f', 'F', 'g', 'G', and 'v'. 
All formats are equivalent.
Format 是fmt.Scanner支持例程。
它接受格式  'e', 'E', 'f', 'F', 'g', 'G', 和 'v'.
所有格式是等价的。

Example:
package main

import (
	"fmt"
	"log"
	"math/big"
)

func main() {
	// The Scan function is rarely used directly;
	// the fmt package recognizes it as an implementation of fmt.Scanner.
	r := new(big.Rat)
	_, err := fmt.Sscan("1.5000", r)
	if err != nil {
		log.Println("error scanning value:", err)
	} else {
		fmt.Println(r)
	}
}


func (*Rat) Set

func (z *Rat) Set(x *Rat) *Rat
Set sets z to x (by making a copy of x) and returns z.
Set设置z成x(通过复制x) 并返回z (备注 这样修改x 就不会连着修改z了)



func (*Rat) SetFloat64

func (z *Rat) SetFloat64(f float64) *Rat
SetFloat64 sets z to exactly f and returns z. If f is not finite, SetFloat returns nil.
SetFloat64 设置z成f 并返回z. 如果f不是有限的, SetFloat 返回nil.



func (*Rat) SetFrac

func (z *Rat) SetFrac(a, b *Int) *Rat
SetFrac sets z to a/b and returns z.
SetFrac 设置z 成  a/b 并返回z


func (*Rat) SetFrac64

func (z *Rat) SetFrac64(a, b int64) *Rat
SetFrac64 sets z to a/b and returns z.
SetFrac64设置z成a/b 并返回z


func (*Rat) SetInt

func (z *Rat) SetInt(x *Int) *Rat
SetInt sets z to x (by making a copy of x) and returns z.
SetInt 设置z成x(通过复制一个x) 并返回z



func (*Rat) SetInt64

func (z *Rat) SetInt64(x int64) *Rat
SetInt64 sets z to x and returns z.
SetInt64 设置z成x 并返回z


func (*Rat) SetString

func (z *Rat) SetString(s string) (*Rat, bool)
SetString sets z to the value of s and returns z and a boolean indicating success.
s can be given as a fraction "a/b" or as a floating-point number optionally followed by an exponent. 
If the operation failed, the value of z is undefined but the returned value is nil.
SetString设置z成s的值, 并 返回z和 一个布尔值，表示成功。
s可以表示成一个分数"a/b" 或 任选随后的指数的一个浮点数.
如果操作失败，z的值是不确定的，但返回的值是零。

▹ Example
package main

import (
	"fmt"
	"math/big"
)

func main() {
	r := new(big.Rat)
	r.SetString("355/113")
	fmt.Println(r.FloatString(3))
}



func (*Rat) Sign

func (x *Rat) Sign() int
Sign returns:

-1 if x <  0
 0 if x == 0
+1 if x >  0


func (*Rat) String

func (x *Rat) String() string
String returns a string representation of x in the form "a/b" (even if b == 1).
String返回x的形式的字符串表示“a/b”(即使如果b==1)


func (*Rat) Sub

func (z *Rat) Sub(x, y *Rat) *Rat
Sub sets z to the difference x-y and returns z.
Sub设置z成 x-y 并返回z


func (*Rat) UnmarshalText

func (r *Rat) UnmarshalText(text []byte) error
UnmarshalText implements the encoding.TextUnmarshaler interface
UnmarshalText实现 encoding.TextUnmarshaler 接口


type Word

type Word uintptr
A Word represents a single digit of a multi-precision unsigned integer.
表示一个多精度无符号整数的个位数。












































