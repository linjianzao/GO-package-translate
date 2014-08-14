Package strings

Overview ▾

Package strings implements simple functions to manipulate strings.
strings包实现简单操作字符串功能.


func Contains
```golang
func Contains(s, substr string) bool
```
Contains returns true if substr is within s.
如果substr 包含在s里 返回true.

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Contains("seafood", "foo"))
	fmt.Println(strings.Contains("seafood", "bar"))
	fmt.Println(strings.Contains("seafood", ""))
	fmt.Println(strings.Contains("", ""))
}
```


func ContainsAny
```golang
func ContainsAny(s, chars string) bool
```
ContainsAny returns true if any Unicode code points in chars are within s.
ContainsAny 如果有任何的Unicode 字符chars 在 s里,返回true.

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ContainsAny("team", "i"))
	fmt.Println(strings.ContainsAny("failure", "u & i"))
	fmt.Println(strings.ContainsAny("foo", ""))
	fmt.Println(strings.ContainsAny("", ""))
}
```


func ContainsRune
```golang
func ContainsRune(s string, r rune) bool
```
ContainsRune returns true if the Unicode code point r is within s.
ContainsRune 如果 Unicode r 在s里, 返回true



func Count
```golang
func Count(s, sep string) int
```
Count counts the number of non-overlapping instances of sep in s.
Count 统计 在s里的 sep 的数量

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Count("cheese", "e"))
	fmt.Println(strings.Count("five", "")) // before & after each rune
}
```



func EqualFold
```golang
func EqualFold(s, t string) bool
```
EqualFold reports whether s and t, interpreted as UTF-8 strings, are equal under Unicode case-folding.
EqualFold 报告 s 和t , 是否是 相等的 UTF-8  Unicode 字符串.

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.EqualFold("Go", "go"))
}
```



func Fields
```golang
func Fields(s string) []string
```
Fields splits the string s around each instance of one or more consecutive white space characters, as defined by unicode.IsSpace, 
	returning an array of substrings of s or an empty list if s contains only white space.
Fields 根据左右的一个或多个 空白字符 分割字符串s, 空白字符由unicode.IsSpace定义，返回s的子串的数组 , 如果s只包含空白字符 返回空列表

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
}
```



func FieldsFunc
```golang
func FieldsFunc(s string, f func(rune) bool) []string
```
FieldsFunc splits the string s at each run of Unicode code points c satisfying f(c) and returns an array of slices of s. 
If all code points in s satisfy f(c) or the string is empty, an empty slice is returned.
FieldsFunc  以 满足f(c) 的每个Unicode编码 c 分割字符串s 并返回s的 数组slice.
如果在s里的所有编码字符都 满足  f(c) 或 字符串是空的, 返回空 slice.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	f := func(c rune) bool {
		return !unicode.IsLetter(c) && !unicode.IsNumber(c)
	}
	fmt.Printf("Fields are: %q", strings.FieldsFunc("  foo1;bar2,baz3...", f))
}
```


func HasPrefix
```golang
func HasPrefix(s, prefix string) bool
```
HasPrefix tests whether the string s begins with prefix.
HasPrefix 测试 字符串s 是否 以prefix 开始.



func HasSuffix
```golang
func HasSuffix(s, suffix string) bool
```
HasSuffix tests whether the string s ends with suffix.
HasSuffix 测试 字符串s 是否以suffix 结尾.



func Index
```golang
func Index(s, sep string) int
```
Index returns the index of the first instance of sep in s, or -1 if sep is not present in s.
Index 返回 sep在s中的 第一个实例 的索引, 如果 sep没有出现在s里, 返回-1.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Index("chicken", "ken"))
	fmt.Println(strings.Index("chicken", "dmr"))
}
```


func IndexAny
```golang
func IndexAny(s, chars string) int
```
IndexAny returns the index of the first instance of any Unicode code point from chars in s, or -1 if no Unicode code point from chars is present in s.
IndexAny 返回 在s 里 的第一个 Unicode 编码 实例 chars的索引, 如果s中出现的chars 不是Unicode 返回-1.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.IndexAny("chicken", "aeiouy"))
	fmt.Println(strings.IndexAny("crwth", "aeiouy"))
}
```


func IndexByte
```golang
func IndexByte(s string, c byte) int
```
IndexByte returns the index of the first instance of c in s, or -1 if c is not present in s.
IndexByte 返回c在s中第一个实例的索引, 如果c没有出现在s中 返回-1.



func IndexFunc
```golang
func IndexFunc(s string, f func(rune) bool) int
```
IndexFunc returns the index into s of the first Unicode code point satisfying f(c), or -1 if none do.
IndexFunc 返回  在s中 第一个满足 f(c) 的Unicode 编码 的索引, 如果没有 返回-1.


▹ Example
```golang
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	f := func(c rune) bool {
		return unicode.Is(unicode.Han, c)
	}
	fmt.Println(strings.IndexFunc("Hello, 世界", f))
	fmt.Println(strings.IndexFunc("Hello, world", f))
}
```


func IndexRune
```golang
func IndexRune(s string, r rune) int
```
IndexRune returns the index of the first instance of the Unicode code point r, or -1 if rune is not present in s.
IndexRune 返回 r在 s中 的第一个 Unicode编码实例 索引,如果rune 没有出现在s中 返回-1.


▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.IndexRune("chicken", 'k'))
	fmt.Println(strings.IndexRune("chicken", 'd'))
}
```


func Join
```golang
func Join(a []string, sep string) string
```
Join concatenates the elements of a to create a single string. The separator string sep is placed between elements in the resulting string.
Join 连接 a的元素 来创建 一个字符串. 分离字符串sep 放在 结果字符串元素之间.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := []string{"foo", "bar", "baz"}
	fmt.Println(strings.Join(s, ", "))
}

```


func LastIndex
```golang
func LastIndex(s, sep string) int
```
LastIndex returns the index of the last instance of sep in s, or -1 if sep is not present in s.
LastIndex 返回在s 中的sep 的最后一个实例索引, 如果 sep没有出现在s,返回-1.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Index("go gopher", "go"))
	fmt.Println(strings.LastIndex("go gopher", "go"))
	fmt.Println(strings.LastIndex("go gopher", "rodent"))
}
```


func LastIndexAny
```golang
func LastIndexAny(s, chars string) int
```
LastIndexAny returns the index of the last instance of any Unicode code point from chars in s, or -1 if no Unicode code point from chars is present in s.
LastIndexAny 返回 在s中的 Unicode编码 chars 的最后一个实例的索引,如果 Unicode编码 chars 不存在s中 返回-1.



func LastIndexFunc
```golang
func LastIndexFunc(s string, f func(rune) bool) int
```
LastIndexFunc returns the index into s of the last Unicode code point satisfying f(c), or -1 if none do.
LastIndexFunc 返回 s 中 满足f(c) 的最后Unicode编码 索引, 如果没有 返回-1.


func Map
```golang
func Map(mapping func(rune) rune, s string) string
```
Map returns a copy of the string s with all its characters modified according to the mapping function. 
If mapping returns a negative value, the character is dropped from the string with no replacement.
Map 返回  所有 s字符串 通过 对应的 mapping函数修改的复制 .
如果 mapping 返回 负值,  该字符 从字符串删除 不会被替换.


▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	rot13 := func(r rune) rune {
		switch {
		case r >= 'A' && r <= 'Z':
			return 'A' + (r-'A'+13)%26
		case r >= 'a' && r <= 'z':
			return 'a' + (r-'a'+13)%26
		}
		return r
	}
	fmt.Println(strings.Map(rot13, "'Twas brillig and the slithy gopher..."))
}
```



func Repeat
```golang
func Repeat(s string, count int) string
```
Repeat returns a new string consisting of count copies of the string s.
Repeat 返回一个 复制 s count长度 的新字符串

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println("ba" + strings.Repeat("na", 2))
}
```



func Replace
```golang
func Replace(s, old, new string, n int) string
```
Replace returns a copy of the string s with the first n non-overlapping instances of old replaced by new. If n < 0, there is no limit on the number of replacements.
Replace返回  字符串s 用new 代替 old n次 的实例复制 . 如果 n < 0,不会限制替换数.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
	fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))
}
```


func Split
```golang
func Split(s, sep string) []string
```
Split slices s into all substrings separated by sep and returns a slice of the substrings between those separators. 
If sep is empty, Split splits after each UTF-8 sequence. It is equivalent to SplitN with a count of -1.
Split  通过sep 分离  slices s 所有子串 并返回  这些分隔符之间的子串 slice.
如果sep是空的,Split 分割每个 UTF-8序列. 它等价于 SplitN -1

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.Split("a,b,c", ","))
	fmt.Printf("%q\n", strings.Split("a man a plan a canal panama", "a "))
	fmt.Printf("%q\n", strings.Split(" xyz ", ""))
	fmt.Printf("%q\n", strings.Split("", "Bernardo O'Higgins"))
}
```


func SplitAfter
```golang
func SplitAfter(s, sep string) []string
```
SplitAfter slices s into all substrings after each instance of sep and returns a slice of those substrings. 
If sep is empty, SplitAfter splits after each UTF-8 sequence. It is equivalent to SplitAfterN with a count of -1.
SplitAfter  在每个sep实例后 切s成所有子串 并返回 他们子串 slice.
如果sep是空的,SplitAfter 在每个UTF-8序列后分割. 它等价于 SplitAfterN -1

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.SplitAfter("a,b,c", ","))
}
```



func SplitAfterN
```golang
func SplitAfterN(s, sep string, n int) []string
```
SplitAfterN slices s into substrings after each instance of sep and returns a slice of those substrings. 
If sep is empty, SplitAfterN splits after each UTF-8 sequence. The count determines the number of substrings to return:
SplitAfterN 在每个sep实例后 切s成所有子串 并返回 他们子串 slice.  
如果sep是空的,SplitAfterN在每个UTF-8序列后分割. 统计决定 返回的子串数:

```golang
n > 0: at most n substrings; the last substring will be the unsplit remainder.
n == 0: the result is nil (zero substrings)
n < 0: all substrings

n > 0: 最多n 子串. 最后的子串是剩余未分割的.
n == 0:返回值是nil
n < 0: 所有子串
```

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.SplitAfterN("a,b,c", ",", 2))
}
```


func SplitN
```golang
func SplitN(s, sep string, n int) []string
```
SplitN slices s into substrings separated by sep and returns a slice of the substrings between those separators. 
If sep is empty, SplitN splits after each UTF-8 sequence. The count determines the number of substrings to return:
SplitN 根据sep分离  s 成子串 并返回 他们分离器之间的 子串 slice.
如果sep是空的,SplitN 在每个UTF-8 序列 分割. 统计决定 返回的子串数:

```golang
n > 0: at most n substrings; the last substring will be the unsplit remainder.
n == 0: the result is nil (zero substrings)
n < 0: all substrings

n > 0: 最多n 子串. 最后的子串是剩余未分割的.
n == 0:返回值是nil
n < 0: 所有子串
```

▹ Example

```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.SplitN("a,b,c", ",", 2))
	z := strings.SplitN("a,b,c", ",", 0)
	fmt.Printf("%q (nil = %v)\n", z, z == nil)
}
```


func Title
```golang
func Title(s string) string
```
Title returns a copy of the string s with all Unicode letters that begin words mapped to their title case.

BUG: The rule Title uses for word boundaries does not handle Unicode punctuation properly.
Title 返回 字符串s 所有 Unicode 单词 开头映射为他们的词首字母大写。

BUG: 该Title规则用于单词边界 不会正确处理 Unicode 标点.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Title("her royal highness"))
}
```



func ToLower
```golang
func ToLower(s string) string
```
ToLower returns a copy of the string s with all Unicode letters mapped to their lower case.
ToLower 返回 字符串s 所有 Unicode 字母 的小写  复制.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToLower("Gopher"))
}
```


func ToLowerSpecial
```golang
func ToLowerSpecial(_case unicode.SpecialCase, s string) string
```
ToLowerSpecial returns a copy of the string s with all Unicode letters mapped to their lower case, giving priority to the special casing rules.
ToLowerSpecial返回 字符串s 所有 Unicode 字母 的小写  复制. 特殊的大小写规则优先。



func ToTitle
```golang
func ToTitle(s string) string
```
ToTitle returns a copy of the string s with all Unicode letters mapped to their title case.
ToTitle  返回 字符串s 所有 Unicode 字母 的首字母大写  复制.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToTitle("loud noises"))
	fmt.Println(strings.ToTitle("хлеб"))
}
```


func ToTitleSpecial
```golang
func ToTitleSpecial(_case unicode.SpecialCase, s string) string
```
ToTitleSpecial returns a copy of the string s with all Unicode letters mapped to their title case, giving priority to the special casing rules.
ToTitleSpecial  返回 字符串s 所有 Unicode 字母 的首字母大写  复制.特殊的大小写规则优先。



func ToUpper
```golang
func ToUpper(s string) string
```
ToUpper returns a copy of the string s with all Unicode letters mapped to their upper case.
ToUpper  返回 字符串s 所有 Unicode 字母 的大写字母  复制

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToUpper("Gopher"))
}
```



func ToUpperSpecial
```golang
func ToUpperSpecial(_case unicode.SpecialCase, s string) string
```
ToUpperSpecial returns a copy of the string s with all Unicode letters mapped to their upper case, giving priority to the special casing rules.
ToUpperSpecial 返回 字符串s 所有 Unicode 字母 的大写字母  复制 .特殊的大小写规则优先。



func Trim
```golang
func Trim(s string, cutset string) string
```
Trim returns a slice of the string s with all leading and trailing Unicode code points contained in cutset removed.
Trim  返回 字符串s  所有 开头和结尾 Unicode  包含 的cutset.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("[%q]", strings.Trim(" !!! Achtung! Achtung! !!! ", "! "))
}
```


func TrimFunc
```golang
func TrimFunc(s string, f func(rune) bool) string
```
TrimFunc returns a slice of the string s with all leading and trailing Unicode code points c satisfying f(c) removed.
TrimFunc 返回 字符串s  所有 开头和结尾 Unicode  包含 的满足 f(c) 的 c .



func TrimLeft
```golang
func TrimLeft(s string, cutset string) string
```
TrimLeft returns a slice of the string s with all leading Unicode code points contained in cutset removed.
TrimLeft返回 字符串s  所有 开头 Unicode  包含 的cutset.



func TrimLeftFunc
```golang
func TrimLeftFunc(s string, f func(rune) bool) string
```
TrimLeftFunc returns a slice of the string s with all leading Unicode code points c satisfying f(c) removed.
TrimLeftFunc 返回 字符串s  所有 开头Unicode  包含 的满足 f(c) 的 c .



func TrimPrefix
```golang
func TrimPrefix(s, prefix string) string
```
TrimPrefix returns s without the provided leading prefix string. If s doesn't start with prefix, s is returned unchanged.
TrimPrefix 返回 不包含 开头prefix字符串的s. 如果s没有以prefix 开始, s 不变


▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	var s = "Goodbye,, world!"
	s = strings.TrimPrefix(s, "Goodbye,")
	s = strings.TrimPrefix(s, "Howdy,")
	fmt.Print("Hello" + s)
}
```


func TrimRight
```golang
func TrimRight(s string, cutset string) string
```
TrimRight returns a slice of the string s, with all trailing Unicode code points contained in cutset removed.
TrimRight返回 字符串s  所有 结尾 Unicode  包含 的cutset.



func TrimRightFunc
```golang
func TrimRightFunc(s string, f func(rune) bool) string
```
TrimRightFunc returns a slice of the string s with all trailing Unicode code points c satisfying f(c) removed.
TrimRightFunc 返回 字符串s  所有 结尾 Unicode  包含 的满足 f(c) 的 c .


func TrimSpace
```golang
func TrimSpace(s string) string
```
TrimSpace returns a slice of the string s, with all leading and trailing white space removed, as defined by Unicode.
TrimSpace 返回 字符串s 所有开头和结尾的 根据Unicode定义的空格.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.TrimSpace(" \t\n a lone gopher \n\t\r\n"))
}
```


func TrimSuffix
```golang
func TrimSuffix(s, suffix string) string
```
TrimSuffix returns s without the provided trailing suffix string. If s doesn't end with suffix, s is returned unchanged.
TrimSuffix 返回结尾不包含 suffix字符串的 s. 如果s 没有以suffix 结尾, s 不变.

▾ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	var s = "Hello, goodbye, etc!"
	s = strings.TrimSuffix(s, "goodbye, etc!")
	s = strings.TrimSuffix(s, "planet")
	fmt.Print(s, "world!")
}
```


type Reader
```golang
type Reader struct {
        // contains filtered or unexported fields
}
```
A Reader implements the io.Reader, io.ReaderAt, io.Seeker, io.WriterTo, io.ByteScanner, and io.RuneScanner interfaces by reading from a string.
Reader 根据 从a 字符串读取 实现 io.Reader, io.ReaderAt, io.Seeker, io.WriterTo, io.ByteScanner, and io.RuneScanner 接口.


func NewReader
```golang
func NewReader(s string) *Reader
```
NewReader returns a new Reader reading from s. It is similar to bytes.NewBufferString but more efficient and read-only.
NewReader s 中 返回一个新的 Reader. 它类似 bytes.NewBufferString 但更有效 并且 只读。


func (*Reader) Len
```golang
func (r *Reader) Len() int
```
Len returns the number of bytes of the unread portion of the string.
Len 返回该字符串 未读部分 的数量


func (*Reader) Read
```golang
func (r *Reader) Read(b []byte) (n int, err error)
```


func (*Reader) ReadAt
```golang
func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)
```


func (*Reader) ReadByte
```golang
func (r *Reader) ReadByte() (b byte, err error)
```


func (*Reader) ReadRune
```golang
func (r *Reader) ReadRune() (ch rune, size int, err error)
```


func (*Reader) Seek
```golang
func (r *Reader) Seek(offset int64, whence int) (int64, error)
```
Seek implements the io.Seeker interface.
Seek 实现 io.Seeker接口


func (*Reader) UnreadByte
```golang
func (r *Reader) UnreadByte() error
```


func (*Reader) UnreadRune
```golang
func (r *Reader) UnreadRune() error
```


func (*Reader) WriteTo
```golang
func (r *Reader) WriteTo(w io.Writer) (n int64, err error)
```
WriteTo implements the io.WriterTo interface.
WriteTo 实现 io.WriterTo 接口


type Replacer
```golang
type Replacer struct {
        // contains filtered or unexported fields
}
```
A Replacer replaces a list of strings with replacements.
Replacer 替换字符串列表。


func NewReplacer
```golang
func NewReplacer(oldnew ...string) *Replacer
```
NewReplacer returns a new Replacer from a list of old, new string pairs. Replacements are performed in order, without overlapping matches.
NewReplacer 从旧新 字符串对列表 返回一个新 Replacer. 替代 是为了进行 不重复的匹配.

▹ Example
```golang
package main

import (
	"fmt"
	"strings"
)

func main() {
	r := strings.NewReplacer("<", "&lt;", ">", "&gt;")
	fmt.Println(r.Replace("This is <b>HTML</b>!"))
}
```


func (*Replacer) Replace
```golang
func (r *Replacer) Replace(s string) string
```
Replace returns a copy of s with all replacements performed.
Replace返回s的进行更换所有的副本。


func (*Replacer) WriteString
```golang
func (r *Replacer) WriteString(w io.Writer, s string) (n int, err error)
```
WriteString writes s to w with all replacements performed.
WriteString 写 全部替换完成的s 到w 


