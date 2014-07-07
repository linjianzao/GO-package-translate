Package math

Overview ▾
Package math provides basic constants and mathematical functions.
math包 提供 基本常量和数学函数。


Constants

const (
        E   = 2.71828182845904523536028747135266249775724709369995957496696763 // A001113
        Pi  = 3.14159265358979323846264338327950288419716939937510582097494459 // A000796
        Phi = 1.61803398874989484820458683436563811772030917980576286213544862 // A001622

        Sqrt2   = 1.41421356237309504880168872420969807856967187537694807317667974 // A002193
        SqrtE   = 1.64872127070012814684865078781416357165377610071014801157507931 // A019774
        SqrtPi  = 1.77245385090551602729816748334114518279754945612238712821380779 // A002161
        SqrtPhi = 1.27201964951406896425242246173749149171560804184009624861664038 // A139339

        Ln2    = 0.693147180559945309417232121458176568075500134360255254120680009 // A002162
        Log2E  = 1 / Ln2
        Ln10   = 2.30258509299404568401799145468436420760110148862877297603332790 // A002392
        Log10E = 1 / Ln10
)
Mathematical constants. Reference: http://oeis.org/Axxxxxx
数学常数

const (
        MaxFloat32             = 3.40282346638528859811704183484516925440e+38  // 2**127 * (2**24 - 1) / 2**23
        SmallestNonzeroFloat32 = 1.401298464324817070923729583289916131280e-45 // 1 / 2**(127 - 1 + 23)

        MaxFloat64             = 1.797693134862315708145274237317043567981e+308 // 2**1023 * (2**53 - 1) / 2**52
        SmallestNonzeroFloat64 = 4.940656458412465441765687928682213723651e-324 // 1 / 2**(1023 - 1 + 52)
)
Floating-point limit values.
Max is the largest finite value representable by the type. 
SmallestNonzero is the smallest positive, non-zero value representable by the type.
浮点型限制值.
Max是 该类型的 最大有限值
SmallestNonzero表示该类型的一个最小的正,非零值

const (
        MaxInt8   = 1<<7 - 1
        MinInt8   = -1 << 7
        MaxInt16  = 1<<15 - 1
        MinInt16  = -1 << 15
        MaxInt32  = 1<<31 - 1
        MinInt32  = -1 << 31
        MaxInt64  = 1<<63 - 1
        MinInt64  = -1 << 63
        MaxUint8  = 1<<8 - 1
        MaxUint16 = 1<<16 - 1
        MaxUint32 = 1<<32 - 1
        MaxUint64 = 1<<64 - 1
)
Integer limit values.
整数限制值。



func Abs

func Abs(x float64) float64
Abs returns the absolute value of x.
Abs返回x的绝对值

Special cases are:
特殊情况:

Abs(±Inf) = +Inf
Abs(NaN) = NaN




func Acos

func Acos(x float64) float64
Acos returns the arccosine, in radians, of x.
Acos返回反余弦值x

Special case is:
特殊情况:
Acos(x) = NaN if x < -1 or x > 1




func Acosh

func Acosh(x float64) float64
Acosh returns the inverse hyperbolic cosine of x.
Acosh 返回反双曲余弦值x。

Special cases are:
特殊情况:

Acosh(+Inf) = +Inf
Acosh(x) = NaN if x < 1
Acosh(NaN) = NaN




func Asin

func Asin(x float64) float64
Asin returns the arcsine, in radians, of x.
Asin返回x的反正弦

Special cases are:
特殊情况:

Asin(±0) = ±0
Asin(x) = NaN if x < -1 or x > 1



func Asinh

func Asinh(x float64) float64
Asinh returns the inverse hyperbolic sine of x.
Asinh 返回x的反双曲正弦值。

Special cases are:
特殊情况:

Asinh(±0) = ±0
Asinh(±Inf) = ±Inf
Asinh(NaN) = NaN


func Atan

func Atan(x float64) float64
Atan returns the arctangent, in radians, of x.
Atan返回反正切值的x

Special cases are:
特殊情况:

Atan(±0) = ±0
Atan(±Inf) = ±Pi/2
func Atan2

func Atan2(y, x float64) float64
Atan2 returns the arc tangent of y/x, using the signs of the two to determine the quadrant of the return value.
Atan2返回y/ x的反正切值，使用两个标志来确定返回值的象限。

Special cases are (in order):
特殊情况:

Atan2(y, NaN) = NaN
Atan2(NaN, x) = NaN
Atan2(+0, x>=0) = +0
Atan2(-0, x>=0) = -0
Atan2(+0, x<=-0) = +Pi
Atan2(-0, x<=-0) = -Pi
Atan2(y>0, 0) = +Pi/2
Atan2(y<0, 0) = -Pi/2
Atan2(+Inf, +Inf) = +Pi/4
Atan2(-Inf, +Inf) = -Pi/4
Atan2(+Inf, -Inf) = 3Pi/4
Atan2(-Inf, -Inf) = -3Pi/4
Atan2(y, +Inf) = 0
Atan2(y>0, -Inf) = +Pi
Atan2(y<0, -Inf) = -Pi
Atan2(+Inf, x) = +Pi/2
Atan2(-Inf, x) = -Pi/2


func Atanh

func Atanh(x float64) float64
Atanh returns the inverse hyperbolic tangent of x.
Atanh返回x的反双曲正切值。

Special cases are:
特殊情况:

Atanh(1) = +Inf
Atanh(±0) = ±0
Atanh(-1) = -Inf
Atanh(x) = NaN if x < -1 or x > 1
Atanh(NaN) = NaN


func Cbrt

func Cbrt(x float64) float64
Cbrt returns the cube root of x.
Cbrt返回x的立方根。

Special cases are:
特殊情况:

Cbrt(±0) = ±0
Cbrt(±Inf) = ±Inf
Cbrt(NaN) = NaN


func Ceil

func Ceil(x float64) float64
Ceil returns the least integer value greater than or equal to x.
返回大于所述至少一个整数值或等于x。

Special cases are:
特殊情况:

Ceil(±0) = ±0
Ceil(±Inf) = ±Inf
Ceil(NaN) = NaN


func Copysign

func Copysign(x, y float64) float64
Copysign returns a value with the magnitude of x and the sign of y.
Copysign返回一个值与x的绝对值和y的符号。



func Cos

func Cos(x float64) float64
Cos returns the cosine of the radian argument x.
COS返回弧度参数x的余弦值。

Special cases are:
特殊情况:

Cos(±Inf) = NaN
Cos(NaN) = NaN


func Cosh

func Cosh(x float64) float64
Cosh returns the hyperbolic cosine of x.
Cosh返回x的双曲余弦值。

Special cases are:
特殊情况:

Cosh(±0) = 1
Cosh(±Inf) = +Inf
Cosh(NaN) = NaN



func Dim

func Dim(x, y float64) float64
Dim returns the maximum of x-y or 0.
Dim返回的最大x-y或0。

Special cases are:
特殊情况:

Dim(+Inf, +Inf) = NaN
Dim(-Inf, -Inf) = NaN
Dim(x, NaN) = Dim(NaN, x) = NaN


func Erf

func Erf(x float64) float64
Erf returns the error function of x.
Erf返回x的误差函数。

Special cases are:
特殊情况:

Erf(+Inf) = 1
Erf(-Inf) = -1
Erf(NaN) = NaN



func Erfc

func Erfc(x float64) float64
Erfc returns the complementary error function of x.
Erfc返回x的互补误差函数。

Special cases are:
特殊情况:

Erfc(+Inf) = 0
Erfc(-Inf) = 2
Erfc(NaN) = NaN



func Exp

func Exp(x float64) float64
Exp returns e**x, the base-e exponential of x.
Exp返回e** X，基E X的指数。

Special cases are:
特殊情况:

Exp(+Inf) = +Inf
Exp(NaN) = NaN
Very large values overflow to 0 or +Inf. Very small values underflow to 1.


func Exp2

func Exp2(x float64) float64
Exp2 returns 2**x, the base-2 exponential of x.
Exp2 返回2**的x，基部2的指数的x。

Special cases are the same as Exp.
特殊情况与ExpExp一样



func Expm1

func Expm1(x float64) float64
Expm1 returns e**x - 1, the base-e exponential of x minus 1. 
It is more accurate than Exp(x) - 1 when x is near zero.
返回e**× - 1，x的基e指数减1。
它比Exp(x）的更准确 - 1，当x接近零。

Special cases are:
特殊情况:

Expm1(+Inf) = +Inf
Expm1(-Inf) = -1
Expm1(NaN) = NaN
Very large values overflow to -1 or +Inf.
非常大的值溢出到-1或+Inf



func Float32bits

func Float32bits(f float32) uint32
Float32bits returns the IEEE 754 binary representation of f.
Float32bits返回f的IEEE 754二进制表示。


func Float32frombits

func Float32frombits(b uint32) float32
Float32frombits returns the floating point number corresponding to the IEEE 754 binary representation b.
Float32frombits返回对应于IEEE 754的二进制表示B中的浮点数。


func Float64bits

func Float64bits(f float64) uint64
Float64bits returns the IEEE 754 binary representation of f.
Float64bits返回f的IEEE 754二进制表示。


func Float64frombits

func Float64frombits(b uint64) float64
Float64frombits returns the floating point number corresponding the IEEE 754 binary representation b.
Float64frombits返回相应的IEEE 754二进制表示B中的浮点数。



func Floor

func Floor(x float64) float64
Floor returns the greatest integer value less than or equal to x.
Floor返回的最大整数的值小于或等于x。

Special cases are:
特殊情况:

Floor(±0) = ±0
Floor(±Inf) = ±Inf
Floor(NaN) = NaN


func Frexp

func Frexp(f float64) (frac float64, exp int)
Frexp breaks f into a normalized fraction and an integral power of two.
It returns frac and exp satisfying f == frac × 2**exp, with the absolute value of frac in the interval [½, 1).
Frexp 分解f成 正常化的分数和为2的整数幂
它返回 frac 和 exp 满足 f == frac × 2**exp, 在区间 [½, 1) 中的 frac的绝对值.

Special cases are:
特殊情况:

Frexp(±0) = ±0, 0
Frexp(±Inf) = ±Inf, 0
Frexp(NaN) = NaN, 0


func Gamma

func Gamma(x float64) float64
Gamma returns the Gamma function of x.
Gamma返回x的Gamma函数

Special cases are:
特殊情况:

Gamma(+Inf) = +Inf
Gamma(+0) = +Inf
Gamma(-0) = -Inf
Gamma(x) = NaN for integer x < 0
Gamma(-Inf) = NaN
Gamma(NaN) = NaN


func Hypot

func Hypot(p, q float64) float64
Hypot returns Sqrt(p*p + q*q), taking care to avoid unnecessary overflow and underflow.
Hypot返回 Sqrt(p*p + q*q),同时注意避免不必要的溢出和下溢。

Special cases are:
特殊情况:

Hypot(±Inf, q) = +Inf
Hypot(p, ±Inf) = +Inf
Hypot(NaN, q) = NaN
Hypot(p, NaN) = NaN


func Ilogb

func Ilogb(x float64) int
Ilogb returns the binary exponent of x as an integer.
Ilogb返回x的二进制指数为整数。

Special cases are:
特殊情况:

Ilogb(±Inf) = MaxInt32
Ilogb(0) = MinInt32
Ilogb(NaN) = MaxInt32


func Inf

func Inf(sign int) float64
Inf returns positive infinity if sign >= 0, negative infinity if sign < 0.
Inf 返回 如果 sign >= 0 是 无穷大, 如果 sign < 0 无穷小.


func IsInf

func IsInf(f float64, sign int) bool
IsInf reports whether f is an infinity, according to sign. 
If sign > 0, IsInf reports whether f is positive infinity.
If sign < 0, IsInf reports whether f is negative infinity. 
If sign == 0, IsInf reports whether f is either infinity.
IsInf 根据sign 报告f是否是无穷大.


func IsNaN

func IsNaN(f float64) (is bool)
IsNaN reports whether f is an IEEE 754 “not-a-number” value.
IsNaN 报告f是否是IEEE754“不是一个数”值。



func J0

func J0(x float64) float64
J0 returns the order-zero Bessel function of the first kind.
返回第一种 0阶的 Bessel 函数

Special cases are:
特殊情况:

J0(±Inf) = 0
J0(0) = 1
J0(NaN) = NaN



func J1

func J1(x float64) float64
J1 returns the order-one Bessel function of the first kind.
返回第一种 1阶的 Bessel 函数

Special cases are:
特殊情况:

J1(±Inf) = 0
J1(NaN) = NaN


func Jn

func Jn(n int, x float64) float64
Jn returns the order-n Bessel function of the first kind.
返回第一种 N阶的 Bessel 函数

Special cases are:
特殊情况:

Jn(n, ±Inf) = 0
Jn(n, NaN) = NaN



func Ldexp

func Ldexp(frac float64, exp int) float64
Ldexp is the inverse of Frexp. It returns frac × 2**exp.
Ldexp是Frexp的倒数。它返回 frac × 2**exp.

Special cases are:
特殊情况:

Ldexp(±0, exp) = ±0
Ldexp(±Inf, exp) = ±Inf
Ldexp(NaN, exp) = NaN



func Lgamma

func Lgamma(x float64) (lgamma float64, sign int)
Lgamma returns the natural logarithm and sign (-1 or +1) of Gamma(x).
Lgamma返回自然对数  Gamma(x)的sign (-1 or +1) 

Special cases are:
特殊情况:

Lgamma(+Inf) = +Inf
Lgamma(0) = +Inf
Lgamma(-integer) = +Inf
Lgamma(-Inf) = -Inf
Lgamma(NaN) = NaN



func Log

func Log(x float64) float64
Log returns the natural logarithm of x.
Log返回x的自然对数。

Special cases are:
特殊情况:

Log(+Inf) = +Inf
Log(0) = -Inf
Log(x < 0) = NaN
Log(NaN) = NaN



func Log10

func Log10(x float64) float64
Log10 returns the decimal logarithm of x. The special cases are the same as for Log.
Log10返回x的十进制数.特殊情况 和Log一样



func Log1p

func Log1p(x float64) float64
Log1p returns the natural logarithm of 1 plus its argument x. 
It is more accurate than Log(1 + x) when x is near zero.
返回1的自然对数加上它的参数x
当x接近零的时候 它比 Log(1 + x) 更准确

Special cases are:
特殊情况:

Log1p(+Inf) = +Inf
Log1p(±0) = ±0
Log1p(-1) = -Inf
Log1p(x < -1) = NaN
Log1p(NaN) = NaN



func Log2

func Log2(x float64) float64
Log2 returns the binary logarithm of x. The special cases are the same as for Log.
Log2返回x的二进制数。特殊情况 和Log一样



func Logb

func Logb(x float64) float64
Logb returns the binary exponent of x.
Logb返回x的二进制指数。

Special cases are:
特殊情况:

Logb(±Inf) = +Inf
Logb(0) = -Inf
Logb(NaN) = NaN



func Max

func Max(x, y float64) float64
Max returns the larger of x or y.
返回x和y中的最大值

Special cases are:
特殊情况:

Max(x, +Inf) = Max(+Inf, x) = +Inf
Max(x, NaN) = Max(NaN, x) = NaN
Max(+0, ±0) = Max(±0, +0) = +0
Max(-0, -0) = -0



func Min

func Min(x, y float64) float64
Min returns the smaller of x or y.
返回x和y中的最小值

Special cases are:
特殊情况:

Min(x, -Inf) = Min(-Inf, x) = -Inf
Min(x, NaN) = Min(NaN, x) = NaN
Min(-0, ±0) = Min(±0, -0) = -0



func Mod

func Mod(x, y float64) float64
Mod returns the floating-point remainder of x/y. 
The magnitude of the result is less than y and its sign agrees with that of x.
Mod 返回 x/y剩余的浮点值(就是数学中的模)
结果的幅值小于y ,符号取决于x

Special cases are:
特殊情况:

Mod(±Inf, y) = NaN
Mod(NaN, y) = NaN
Mod(x, 0) = NaN
Mod(x, ±Inf) = x
Mod(x, NaN) = NaN



func Modf

func Modf(f float64) (int float64, frac float64)
Modf returns integer and fractional floating-point numbers that sum to f. 
Both values have the same sign as f.
返回整数和小数的浮点数字，总和为f。
这两个值都是和f 相同的符号。

Special cases are:
特殊情况:

Modf(±Inf) = ±Inf, NaN
Modf(NaN) = NaN, NaN



func NaN

func NaN() float64
NaN returns an IEEE 754 “not-a-number” value.
NaN返回一个IEEE 754“不是一个数”值。


func Nextafter

func Nextafter(x, y float64) (r float64)
Nextafter returns the next representable value after x towards y. 
If x == y, then x is returned.
返回下一个 在x指向y 后 表示的值.
如果 x == y, 返回x

Special cases are:
特殊情况:

Nextafter(NaN, y) = NaN
Nextafter(x, NaN) = NaN


func Pow

func Pow(x, y float64) float64
Pow returns x**y, the base-x exponential of y.
Pow 返回x**y,y的基X的指数。

Special cases are (in order):
特殊情况:

Pow(x, ±0) = 1 for any x
Pow(1, y) = 1 for any y
Pow(x, 1) = x for any x
Pow(NaN, y) = NaN
Pow(x, NaN) = NaN
Pow(±0, y) = ±Inf for y an odd integer < 0
Pow(±0, -Inf) = +Inf
Pow(±0, +Inf) = +0
Pow(±0, y) = +Inf for finite y < 0 and not an odd integer
Pow(±0, y) = ±0 for y an odd integer > 0
Pow(±0, y) = +0 for finite y > 0 and not an odd integer
Pow(-1, ±Inf) = 1
Pow(x, +Inf) = +Inf for |x| > 1
Pow(x, -Inf) = +0 for |x| > 1
Pow(x, +Inf) = +0 for |x| < 1
Pow(x, -Inf) = +Inf for |x| < 1
Pow(+Inf, y) = +Inf for y > 0
Pow(+Inf, y) = +0 for y < 0
Pow(-Inf, y) = Pow(-0, -y)
Pow(x, y) = NaN for finite x < 0 and finite non-integer y


func Pow10

func Pow10(e int) float64
Pow10 returns 10**e, the base-10 exponential of e.
返回 10**e,e的基10指数。

Special cases are:
特殊情况:

Pow10(e) = +Inf for e > 309
Pow10(e) = 0 for e < -324


func Remainder

func Remainder(x, y float64) float64
Remainder returns the IEEE 754 floating-point remainder of x/y.
返回 x/y 剩余的 IEEE 754浮点型

Special cases are:
特殊情况:

Remainder(±Inf, y) = NaN
Remainder(NaN, y) = NaN
Remainder(x, 0) = NaN
Remainder(x, ±Inf) = x
Remainder(x, NaN) = NaN


func Signbit

func Signbit(x float64) bool
Signbit returns true if x is negative or negative zero.
如果x是负或者负0 返回true


func Sin

func Sin(x float64) float64
Sin returns the sine of the radian argument x.
返回弧度参数x的正弦值。

Special cases are:
特殊情况:

Sin(±0) = ±0
Sin(±Inf) = NaN
Sin(NaN) = NaN



func Sincos

func Sincos(x float64) (sin, cos float64)
Sincos returns Sin(x), Cos(x).
返回 Sin(x), Cos(x).

Special cases are:
特殊情况:

Sincos(±0) = ±0, 1
Sincos(±Inf) = NaN, NaN
Sincos(NaN) = NaN, NaN


func Sinh

func Sinh(x float64) float64
Sinh returns the hyperbolic sine of x.
返回x的双曲正弦值。

Special cases are:
特殊情况:

Sinh(±0) = ±0
Sinh(±Inf) = ±Inf
Sinh(NaN) = NaN


func Sqrt

func Sqrt(x float64) float64
Sqrt returns the square root of x.
返回x的平方根。

Special cases are:
特殊情况:

Sqrt(+Inf) = +Inf
Sqrt(±0) = ±0
Sqrt(x < 0) = NaN
Sqrt(NaN) = NaN


func Tan

func Tan(x float64) float64
Tan returns the tangent of the radian argument x.
返回弧度参数x的正切值。

Special cases are:
特殊情况:

Tan(±0) = ±0
Tan(±Inf) = NaN
Tan(NaN) = NaN


func Tanh

func Tanh(x float64) float64
Tanh returns the hyperbolic tangent of x.
返回x的双曲正切值。

Special cases are:
特殊情况:

Tanh(±0) = ±0
Tanh(±Inf) = ±1
Tanh(NaN) = NaN



func Trunc

func Trunc(x float64) float64
Trunc returns the integer value of x.
返回x的整数值。

Special cases are:
特殊情况:

Trunc(±0) = ±0
Trunc(±Inf) = ±Inf
Trunc(NaN) = NaN


func Y0

func Y0(x float64) float64
Y0 returns the order-zero Bessel function of the second kind.
Y0返回 返回第二种 0阶的 Bessel 函数

Special cases are:
特殊情况:

Y0(+Inf) = 0
Y0(0) = -Inf
Y0(x < 0) = NaN
Y0(NaN) = NaN



func Y1

func Y1(x float64) float64
Y1 returns the order-one Bessel function of the second kind.
Y1返回 返回第二种 1阶的 Bessel 函数

Special cases are:
特殊情况:

Y1(+Inf) = 0
Y1(0) = -Inf
Y1(x < 0) = NaN
Y1(NaN) = NaN




func Yn

func Yn(n int, x float64) float64
Yn returns the order-n Bessel function of the second kind.
Yn返回 返回第二种n阶的 Bessel 函数

Special cases are:
特殊情况:

Yn(n, +Inf) = 0
Yn(n > 0, 0) = -Inf
Yn(n < 0, 0) = +Inf if n is odd, -Inf if n is even
Y1(n, x < 0) = NaN
Y1(n, NaN) = NaN






