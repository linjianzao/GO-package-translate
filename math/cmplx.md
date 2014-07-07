Package cmplx

Overview ▾

Package cmplx provides basic constants and mathematical functions for complex numbers.
cmplx包提供基本的常量和数学函数复数。


func Abs

func Abs(x complex128) float64
Abs returns the absolute value (also called the modulus) of x.
返回x的绝对值


func Acos

func Acos(x complex128) complex128
Acos returns the inverse cosine of x.
Acos返回 x的反余弦。


func Acosh

func Acosh(x complex128) complex128
Acosh returns the inverse hyperbolic cosine of x.
返回x的反双曲余弦值。


func Asin

func Asin(x complex128) complex128
Asin returns the inverse sine of x.
Asin返回 x的反正弦。


func Asinh

func Asinh(x complex128) complex128
Asinh returns the inverse hyperbolic sine of x.
Asinh返回x的反双曲正弦值。


func Atan

func Atan(x complex128) complex128
Atan returns the inverse tangent of x.
Atan返回x的反正切值。


func Atanh

func Atanh(x complex128) complex128
Atanh returns the inverse hyperbolic tangent of x.
Atanh返回 x的反双曲正切值。


func Conj

func Conj(x complex128) complex128
Conj returns the complex conjugate of x.
Conj 返回x的复合值



func Cos

func Cos(x complex128) complex128
Cos returns the cosine of x.
Cos返回x的余弦值。



func Cosh

func Cosh(x complex128) complex128
Cosh returns the hyperbolic cosine of x.
Cosh返回x的双曲余弦值。


func Cot

func Cot(x complex128) complex128
Cot returns the cotangent of x.
Cot返回x的余切。


func Exp

func Exp(x complex128) complex128
Exp returns e**x, the base-e exponential of x.
返回返回e** X，基E X的指数。


func Inf

func Inf() complex128
Inf returns a complex infinity, complex(+Inf, +Inf).
Inf返回一个复合的无穷大，complex(+Inf, +Inf).


func IsInf

func IsInf(x complex128) bool
IsInf returns true if either real(x) or imag(x) is an infinity.
如果real(x) 或  imag(x) 是一个无穷大  IsInf返回true


func IsNaN

func IsNaN(x complex128) bool
IsNaN returns true if either real(x) or imag(x) is NaN and neither is an infinity.
如果real(x) 或  imag(x) 是 NaN  并且 不是一个无穷大, IsNaN 返回true



func Log

func Log(x complex128) complex128
Log returns the natural logarithm of x.
返回x的自然对数。


func Log10

func Log10(x complex128) complex128
Log10 returns the decimal logarithm of x.
返回x的十进制数。



func NaN

func NaN() complex128
NaN returns a complex “not-a-number” value.
返回一个复合的“不是一个数”值。


func Phase

func Phase(x complex128) float64
Phase returns the phase (also called the argument) of x. The returned value is in the range [-Pi, Pi].
Phase返回x的相位,返回值的范围在 [-Pi, Pi]


func Polar

func Polar(x complex128) (r, θ float64)
Polar returns the absolute value r and phase θ of x, such that x = r * e**θi. The phase is in the range [-Pi, Pi].
Polar 返回 r的绝对值 和  x的相位θ,这样  x = r * e**θi.相位的范围是 [-Pi, Pi].



func Pow

func Pow(x, y complex128) complex128
Pow returns x**y, the base-x exponential of y. 
For generalized compatibility with math.Pow:
Pow 返回 x**y,y的BASE-X的指数。
对于math.Pow广义兼容性：

Pow(0, ±0) returns 1+0i
Pow(0, c) for real(c)<0 returns Inf+0i if imag(c) is zero, otherwise Inf+Inf i.



func Rect

func Rect(r, θ float64) complex128
Rect returns the complex number x with polar coordinates r, θ.
Rect返回复合的数x与极坐标R，θ。


func Sin

func Sin(x complex128) complex128
Sin returns the sine of x.
返回x的正弦值。



func Sinh

func Sinh(x complex128) complex128
Sinh returns the hyperbolic sine of x.
返回x的双曲正弦值。



func Sqrt

func Sqrt(x complex128) complex128
Sqrt returns the square root of x. 
The result r is chosen so that real(r) ≥ 0 and imag(r) has the same sign as imag(x).
Sqrt 返回x的平方根
结果r 被选择 这样的real(r) ≥ 0 和 imag(r) 和 imag(x) 有一样的符号



func Tan

func Tan(x complex128) complex128
Tan returns the tangent of x.
Tan返回x的正切值。


func Tanh

func Tanh(x complex128) complex128
Tanh returns the hyperbolic tangent of x.
Tanh返回x的双曲正切值。

































