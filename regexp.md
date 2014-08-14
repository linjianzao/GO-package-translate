Package regexp


Overview ▾

Package regexp implements regular expression search.
regexp包实现正则表达式搜索。

The syntax of the regular expressions accepted is the same general syntax used by Perl, Python, and other languages. 
More precisely, it is the syntax accepted by RE2 and described at http://code.google.com/p/re2/wiki/Syntax, except for \C. For an overview of the syntax, run
正则表达式的语法和 Perl, Python, 和其他语言的一般语法一样。
更确切地说，它的语法通过 RE2 接收并 描述在http://code.google.com/p/re2/wiki/Syntax，除了\ C。对于语法的概述，执行
```golang
godoc regexp/syntax
```

The regexp implementation provided by this package is guaranteed to run in time linear in the size of the input. 
(This is a property not guaranteed by most open source implementations of regular expressions.) For more information about this property, see
regexp提供这个包实现 确保在执行 时间线的输入大小。
（这是一个特性 不保证大部分 开原实现了正则表达式）更多关于这个特性的信息，查看
```golang
http://swtch.com/~rsc/regexp/regexp1.html
```
or any book about automata theory.
或任何有关自动机理论的书。


All characters are UTF-8-encoded code points.

There are 16 methods of Regexp that match a regular expression and identify the matched text. Their names are matched by this regular expression:
所有的字符都三UTF-8编码
Regexp有16个方法匹配一个正则表达式和标示匹配文本。他们的名称根据这些正则表达式匹配：

```golang
Find(All)?(String)?(Submatch)?(Index)?
```

If 'All' is present, the routine matches successive non-overlapping matches of the entire expression. 
	Empty matches abutting a preceding match are ignored. The return value is a slice containing the successive return values of the corresponding non-'All' routine. 
	These routines take an extra integer argument, n; if n >= 0, the function returns at most n matches/submatches.

If 'String' is present, the argument is a string; otherwise it is a slice of bytes; return values are adjusted as appropriate.

If 'Submatch' is present, the return value is a slice identifying the successive submatches of the expression. 
	Submatches are matches of parenthesized subexpressions (also known as capturing groups) within the regular expression, numbered from left to right in order of opening parenthesis. 
	Submatch 0 is the match of the entire expression, submatch 1 the match of the first parenthesized subexpression, and so on.

If 'Index' is present, matches and submatches are identified by byte index pairs within the input string: result[2*n:2*n+1] identifies the indexes of the nth submatch. 
	The pair for n==0 identifies the match of the entire expression. If 'Index' is not present, the match is identified by the text of the match/submatch. 
	If an index is negative, it means that subexpression did not match any string in the input.

如果  'All' ，整个表达式的事务匹配 不重复 连续的匹配。空匹配 紧靠的前一个匹配被忽略。返回值是一个slice 包含连续的返回值 对应的 非-'All' 事务。
这些程序采取额外的整数参数n;如果n >=0,函数返回最多为n的  matches/submatches。

如果'String'，参数是string; 否则 它是一个 bytes slicel;返回值适当调整。

如果 'Submatch'，



There is also a subset of the methods that can be applied to text read from a RuneReader:

```golang
MatchReader, FindReaderIndex, FindReaderSubmatchIndex
```

This set may grow. Note that regular expression matches may need to examine text beyond the text returned by a match, so the methods that match text from a RuneReader may read arbitrarily far into the input before returning.

(There are a few other methods that do not match this pattern.)


▹ Example

package main

import (
	"fmt"
	"regexp"
)

func main() {
	// Compile the expression once, usually at init time.
	// Use raw strings to avoid having to quote the backslashes.
	var validID = regexp.MustCompile(`^[a-z]+\[[0-9]+\]$`)

	fmt.Println(validID.MatchString("adam[23]"))
	fmt.Println(validID.MatchString("eve[7]"))
	fmt.Println(validID.MatchString("Job[48]"))
	fmt.Println(validID.MatchString("snakey"))
}
