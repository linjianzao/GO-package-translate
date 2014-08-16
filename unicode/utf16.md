Package utf16

Package utf16 implements encoding and decoding of UTF-16 sequences.


func Decode
```golang
func Decode(s []uint16) []rune
```
Decode returns the Unicode code point sequence represented by the UTF-16 encoding s.
Decode 返回  UTF-16 编码s 的 Unicode代码点表示


func DecodeRune
```golang
func DecodeRune(r1, r2 rune) rune
```
DecodeRune returns the UTF-16 decoding of a surrogate pair. If the pair is not a valid UTF-16 surrogate pair, DecodeRune returns the Unicode replacement code point U+FFFD.
DecodeRune 返回 UTF-16 代理对的解码。


func Encode
```golang
func Encode(s []rune) []uint16
```
Encode returns the UTF-16 encoding of the Unicode code point sequence s.
Encode 返回 Unicode 代码点序列s 的UTF-16 编码



func EncodeRune
```golang
func EncodeRune(r rune) (r1, r2 rune)
```
EncodeRune returns the UTF-16 surrogate pair r1, r2 for the given rune. 
If the rune is not a valid Unicode code point or does not need encoding, EncodeRune returns U+FFFD, U+FFFD.
EncodeRune  返回 给定rune 的UTF-16 替代对 r1, r2 



func IsSurrogate
```golang
func IsSurrogate(r rune) bool
```
IsSurrogate returns true if the specified Unicode code point can appear in a surrogate pair.
IsSurrogate 如果指定的Unicode 能出现在一对代理返回  true.
