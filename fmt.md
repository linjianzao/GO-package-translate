#Package fmt
fmt包

##Overview
Package fmt implements formatted I/O with functions analogous to C's printf and scanf.    
The format 'verbs' are derived from C's but are simpler.    
fmt包实现了类似C的printf 和 scanf 的格式化I/O. 'verbs'格式 来源于C 但是更简单
	
##Printing
The verbs:    
General:
```golang
	%v	the value in a default format.  when printing structs, the plus flag (%+v) adds field names
		%v默认格式.  打印结构体时, 增加字段名  加号flag（％+V）
		
	%#v	a Go-syntax representation of the value
			GO语法表示的值
		
	%T	a Go-syntax representation of the type of the value
		GO语法表示的值的类型
	
	%%	a literal percent sign; consumes no value
		一个文本百分号;不消耗值
```

Boolean:
```golang
	%t	the word true or false 
		表示 true 或者false
```

Integer:
```golang
	%b	base 2
	
	%c	the character represented by the corresponding Unicode code point
		表示对应的Unicode 字符
	
	%d	base 10
		
	%o	base 8
		
	%q	a single-quoted character literal safely escaped with Go syntax.
		根据GO语法 过滤的单引号字符
	
	%x	base 16, with lower-case letters for a-f
		base 16,a-f的小写字母
	
	%X	base 16, with upper-case letters for A-F
		base 16, A-F的大写字母
	
	%U	Unicode format: U+1234; same as "U+%04X"
		Unicode格式: U+1234;类似"U+%04X"
```

Floating-point and complex constituents:
```golang
	%b	decimalless scientific notation with exponent a power of two,
		in the manner of strconv.FormatFloat with the 'b' format,
		e.g. -123456p-78
		十进制科学计数法和二值数幂,在strconv.FormatFloat 中的方式是 'b',例如: -123456p-78
		
	%e	scientific notation, 例如: -1234.456e+78
		科学计数法,e.g. -1234.456e+78
		
	%E	scientific notation, 例如: -1234.456E+78
		科学计数法,e.g. -1234.456E+78
	
	%f	decimal point but no exponent, 例如: 123.456
		小数点 但是无指数
		
	%g	whichever of %e or %f produces more compact output
		%e 或 %f 更紧凑的输出 
	
	%G	whichever of %E or %f produces more compact output
		 %E 或 %f更紧凑的输出 
```

String and slice of bytes:
```golang
	%s	the uninterpreted bytes of the string or slice
		string或slice 连续的字符
	
	%q	a double-quoted string safely escaped with Go syntax
		GO语法中 双引号安全的 字符串
	
	%x	base 16, lower-case, two characters per byte
		base 16,小写字母, 每个字节两个字符
	
	%X	base 16, upper-case, two characters per byte
		base 16, 大写字母, 每个字节两个字符
```

Pointer:
```golang
	%p	base 16 notation, with leading 0x
		base 16 符号,以0x开始
```

There is no 'u' flag. Integers are printed unsigned if they have unsigned type.     
Similarly, there is no need to specify the size of the operand (int8, int64).    
没有 'u' falg.如果他们是无符号类型,整型打印无符号     
同样的,不用指定 操作数的大小(int8, int64)     

The width and precision control formatting and are in units of Unicode code points.     
(This differs from C's printf where the units are numbers of bytes.) Either or both of the flags may be replaced with the character '*',     
causing their values to be obtained from the next operand, which must be of type int.    
宽度和精度控制Unicode字符格式.(和C的printf 不一样的地方是 字节的数量单位) 任一 或 所有的flag 也可以 使用 '*'字符代替, 下一个操作数获取的值 肯定是int类型     

For numeric values, width sets the minimum width of the field and precision sets the number of places after the decimal,     
	if appropriate, except that for %g/%G it sets the total number of digits.     
For example, given 123.45 the format %6.2f prints 123.45 while %.4g prints 123.5.     
The default precision for %e and %f is 6; for %g it is the smallest number of digits necessary to identify the value uniquely.    
对于数字值,width设子和最小的字段宽度 和 precision设置小数点后的位数     
如果可以,除了 %g/%G  设置总位数     
比如: 123.45  用%6.2f格式化 成  123.45 而%.4g 是 123.5.     
%e 和 %f 默认精度是6 .对于%g 它是最小的位数必须唯一标识值。     

For most values, width is the minimum number of characters to output, padding the formatted form with spaces if necessary.     
For strings, precision is the maximum number of characters to output, truncating if necessary.    
对于大多数的值,width是输出的最小字符数,如果需要 用空格 格式化.     
对于字符串,precision是输出的最大字符数,如果需要截断.     

Other flags:

```golang
+	always print a sign for numeric values;
	guarantee ASCII-only output for %q (%+q)
	通常数字值会打印除一个标示
	确保 %q (%+q)只输出ASCII
		
-	pad with spaces on the right rather than the left (left-justify the field)
	放在右边而不是左边(左对齐字段)

#	alternate format: add leading 0 for octal (%#o), 0x for hex (%#x);
	0X for hex (%#X); suppress 0x for %p (%#p);
	for %q, print a raw (backquoted) string if strconv.CanBackquote
	returns true;
	write e.g. U+0078 'x' if the character is printable for %U (%#U).
	替代格式: 八进制以0开头(%#o),十六进制以0x开头(%#x);0X也是16进制(%#X);%p (%#p) 压缩16进制.
	对于%q,如果 strconv.CanBackquote返回true 打印一个原始(反引号)字符串
	如果字符打印 %U (%#U) 写 U+0078 'x'
	
' '	(space) leave a space for elided sign in numbers (% d);
	put spaces between bytes printing strings or slices in hex (% x, % X)
		留下一个省略空间 ,在16进制 把 strings 或 slices  放入空间中
	
0	pad with leading zeros rather than spaces;
	for numbers, this moves the padding after the sign
	以0开头而不是空格.对于数字 该成加在符号后面
```

Flags are ignored by verbs that do not expect them.      
For example there is no alternate decimal format, so %#d and %d behave identically.     

For each Printf-like function, there is also a Print function that takes no format and is equivalent to saying %v for every operand.      
Another variant Println inserts blanks between operands and appends a newline.     

Regardless of the verb, if an operand is an interface value, the internal concrete value is used, not the interface itself. Thus:     

Flags除了他们都被忽略
比如 没有替代10进制格式, 所有 so %#d 和 %d 是相同的
对于每个Printf之类的函数,也有不带任何格式的Print函数, 等价于对每个操作数使用%v
其他的Println变种 在空白的操作数之间追加一个换行符
无论是verb,
















