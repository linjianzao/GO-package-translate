Package utf8

Overview ▾

Package utf8 implements functions and constants to support text encoded in UTF-8. It includes functions to translate between runes and UTF-8 byte sequences.
utf8包 实现 函数和常量 来支持 UTF-8编码 文本.它包含函数在runes 和  UTF-8 字节序列 的转化.


Constants
```golang
const (
        RuneError = '\uFFFD'     // the "error" Rune or "Unicode replacement character"
        RuneSelf  = 0x80         // characters below Runeself are represented as themselves in a single byte.
        MaxRune   = '\U0010FFFF' // Maximum valid Unicode code point.
        UTFMax    = 4            // maximum number of bytes of a UTF-8 encoded Unicode character.
)
```
Numbers fundamental to the encoding.
Numbers编码的基础。



func DecodeLastRune
```golang
func DecodeLastRune(p []byte) (r rune, size int)
```
DecodeLastRune unpacks the last UTF-8 encoding in p and returns the rune and its width in bytes. 
If the encoding is invalid, it returns (RuneError, 1), an impossible result for correct UTF-8. 
An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of range, or is not the shortest possible UTF-8 encoding for the value. 
No other validation is performed.
DecodeLastRune 解压 p里的最后 UTF-8 编码  并返回 rune和它在字节的宽度.
 如果编码无效, 它返回  (RuneError, 1), 结果不可能是正确的 UTF-8. 
 如果它不是正确的UTF-8 编码无效,rune 超出编码范围, 或 不是最短的utf - 8编码的值。没有其他执行验证。

 ▾ Example
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	b := []byte("Hello, 世界")

	for len(b) > 0 {
		r, size := utf8.DecodeLastRune(b)
		fmt.Printf("%c %v\n", r, size)

		b = b[:len(b)-size]
	}
}
 ```
 
 
func DecodeLastRuneInString
```golang
func DecodeLastRuneInString(s string) (r rune, size int)
```
DecodeLastRuneInString is like DecodeLastRune but its input is a string. 
If the encoding is invalid, it returns (RuneError, 1), an impossible result for correct UTF-8. 
An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of range, or is not the shortest possible UTF-8 encoding for the value. 
No other validation is performed.
 
DecodeLastRuneInString 类似 DecodeLastRune 但是它的输入是字符串.
 如果编码无效, 它返回  (RuneError, 1), 结果不可能是正确的 UTF-8. 
 如果它不是正确的UTF-8 编码无效,rune 超出编码范围, 或 不是最短的utf - 8编码的值。没有其他执行验证。
 
 
 
 ▹ Example

 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	str := "Hello, 世界"

	for len(str) > 0 {
		r, size := utf8.DecodeLastRuneInString(str)
		fmt.Printf("%c %v\n", r, size)

		str = str[:len(str)-size]
	}
}
 
 ```
 
 
 
 
func DecodeRune
```golang
func DecodeRune(p []byte) (r rune, size int)
```
DecodeRune unpacks the first UTF-8 encoding in p and returns the rune and its width in bytes. 
If the encoding is invalid, it returns (RuneError, 1), an impossible result for correct UTF-8. 
An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of range, or is not the shortest possible UTF-8 encoding for the value. 
No other validation is performed.
DecodeRune 在p里的第一个 UTF-8  编码 并返回rune 和它在字节里的宽度.
如果编码无效, 它返回  (RuneError, 1), 结果不可能是正确的 UTF-8. 
 如果它不是正确的UTF-8 编码无效,rune 超出编码范围, 或 不是最短的utf - 8编码的值。没有其他执行验证。
 
 
 ▾ Example
 
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	b := []byte("Hello, 世界")

	for len(b) > 0 {
		r, size := utf8.DecodeRune(b)
		fmt.Printf("%c %v\n", r, size)

		b = b[size:]
	}
}
 
 ```
 
 
 
func DecodeRuneInString
```golang
func DecodeRuneInString(s string) (r rune, size int)
```
DecodeRuneInString is like DecodeRune but its input is a string. 
If the encoding is invalid, it returns (RuneError, 1), an impossible result for correct UTF-8. 
An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of range, or is not the shortest possible UTF-8 encoding for the value. 
No other validation is performed. 
 DecodeRuneInString 类似 DecodeRune 但它的输入是字符串.
 如果编码无效, 它返回  (RuneError, 1), 结果不可能是正确的 UTF-8. 
 如果它不是正确的UTF-8 编码无效,rune 超出编码范围, 或 不是最短的utf - 8编码的值。没有其他执行验证。
 
 
 ▾ Example

 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	str := "Hello, 世界"

	for len(str) > 0 {
		r, size := utf8.DecodeRuneInString(str)
		fmt.Printf("%c %v\n", r, size)

		str = str[size:]
	}
}
 ```
 
 
func EncodeRune
```golang
func EncodeRune(p []byte, r rune) int
```
EncodeRune writes into p (which must be large enough) the UTF-8 encoding of the rune. It returns the number of bytes written.
EncodeRune 把UTF-8 编码rune 写入p(必须够大). 它返回写入的字节数.

 
 ▹ Example
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	r := '世'
	buf := make([]byte, 3)

	n := utf8.EncodeRune(buf, r)

	fmt.Println(buf)
	fmt.Println(n)
}
 
 ```
 
 
 
 
func FullRune
```golang
func FullRune(p []byte) bool
```
FullRune reports whether the bytes in p begin with a full UTF-8 encoding of a rune. 
An invalid encoding is considered a full Rune since it will convert as a width-1 error rune.
FullRune 报告 在p的 字节 以完整的 UTF-8 编码rune 开头.
无效的编码 被认为是一个完整的Rune,因为它将转换为width-1错误rune.
 
 
 ▹ Example
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	buf := []byte{228, 184, 150} // 世
	fmt.Println(utf8.FullRune(buf))
	fmt.Println(utf8.FullRune(buf[:2]))
}
 ```
 
 
 
func FullRuneInString
```golang
func FullRuneInString(s string) bool
```
FullRuneInString is like FullRune but its input is a string.
FullRuneInString 类似 FullRune 但输入是 字符串
 
 
 ▹ Example
 
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	str := "世"
	fmt.Println(utf8.FullRuneInString(str))
	fmt.Println(utf8.FullRuneInString(str[:2]))
}
 
 ```
 
 
 
func RuneCount
```golang
func RuneCount(p []byte) int
```
RuneCount returns the number of runes in p. Erroneous and short encodings are treated as single runes of width 1 byte.
RuneCount 返回p里的rune 数.错误和短编码被视为单rune宽度1个字节。
 
▾ Example
```golang
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	buf := []byte("Hello, 世界")
	fmt.Println("bytes =", len(buf))
	fmt.Println("runes =", utf8.RuneCount(buf))
}
```
 
 
 
func RuneCountInString
```golang
func RuneCountInString(s string) (n int)
```
RuneCountInString is like RuneCount but its input is a string. 
RuneCountInString 类似 RuneCount 但它的输入是字符串
 
▹ Example
```golang
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	str := "Hello, 世界"
	fmt.Println("bytes =", len(str))
	fmt.Println("runes =", utf8.RuneCountInString(str))
}

```
 
 
 
 
func RuneLen
```golang
func RuneLen(r rune) int
```
RuneLen returns the number of bytes required to encode the rune. It returns -1 if the rune is not a valid value to encode in UTF-8.
RuneLen 返回编码rune需要的字节数. 如果 rune 不是一个有效的UTF-8 它返回-1
 
▹ Example 
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	fmt.Println(utf8.RuneLen('a'))
	fmt.Println(utf8.RuneLen('界'))
}
 ```
 
 
 
func RuneStart
```golang
func RuneStart(b byte) bool
```
RuneStart reports whether the byte could be the first byte of an encoded rune. Second and subsequent bytes always have the top two bits set to 10. 
RuneStart 报告 字节是否可以第一个字节编码的rune.第二和子请求字节通常前两位设置为10。
 
▹ Example
```golang
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	buf := []byte("a界")
	fmt.Println(utf8.RuneStart(buf[0]))
	fmt.Println(utf8.RuneStart(buf[1]))
	fmt.Println(utf8.RuneStart(buf[2]))
}
``` 
 
 
func Valid
```golang
func Valid(p []byte) bool
```
Valid reports whether p consists entirely of valid UTF-8-encoded runes. 
Valid 报告p 是否完全由有效的utf - 8编码的rune。
 
▹ Example
```golang
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	valid := []byte("Hello, 世界")
	invalid := []byte{0xff, 0xfe, 0xfd}

	fmt.Println(utf8.Valid(valid))
	fmt.Println(utf8.Valid(invalid))
}

``` 
 
 
 
func ValidRune
```golang
func ValidRune(r rune) bool
```
ValidRune reports whether r can be legally encoded as UTF-8. Code points that are out of range or a surrogate half are illegal.
ValidRune 报告r 是否 是合法的 UTF-8 编码. 代码点的范围或替代一半是非法的。

▹ Example
 
```golang
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	valid := 'a'
	invalid := rune(0xfffffff)

	fmt.Println(utf8.ValidRune(valid))
	fmt.Println(utf8.ValidRune(invalid))
}

``` 
 
 
 
func ValidString
```golang
func ValidString(s string) bool
```
ValidString reports whether s consists entirely of valid UTF-8-encoded runes.
ValidString 包含 s完全由有效的utf - 8编码的rune。
 
 
▾ Example 
 
 ```golang
 package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	valid := "Hello, 世界"
	invalid := string([]byte{0xff, 0xfe, 0xfd})

	fmt.Println(utf8.ValidString(valid))
	fmt.Println(utf8.ValidString(invalid))
}
 ```
 
 