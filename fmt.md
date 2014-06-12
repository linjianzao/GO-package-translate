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
同样的,不用指定  运算对象的大小(int8, int64)     

The width and precision control formatting and are in units of Unicode code points.     
(This differs from C's printf where the units are numbers of bytes.) Either or both of the flags may be replaced with the character '*',     
causing their values to be obtained from the next operand, which must be of type int.    
宽度和精度控制Unicode字符格式.(和C的printf 不一样的地方是 字节的数量单位) 任一 或 所有的flag 也可以 使用 '*'字符代替, 下一个运算对象获取的值 肯定是int类型     

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
对于每个Printf之类的函数,也有不带任何格式的Print函数, 等价于对每个运算对象使用%v      
其他的Println变种 在空白的操作数之间追加一个换行符      
不管verb,如果操作数是个接口值,使用内部具体的值,而不是接口本身,因此:      
```golang
	var i interface{} = 23
	fmt.Printf("%v\n", i)
```
will print 23.      
If an operand implements interface Formatter, that interface can be used for fine control of formatting.      
If the format (which is implicitly %v for Println etc.) is valid for a string (%s %q %v %x %X), the following two rules also apply:      
1. If an operand implements the error interface, the Error method will be used to convert the object to a string,       
	which will then be formatted as required by the verb (if any).      
2. If an operand implements method String() string, that method will be used to convert the object to a string,       
	which will then be formatted as required by the verb (if any).      

将会打印23      
如果运算对象实现Formatter接口,那个接口可以被精确的控制格式    
如果格式(Println隐式地使用%v)是有效的字符串(%s %q %v %x %X),那会有下面的两个规则:    
1.如果操作数实现了错误接口,Error方法转换对象成字符串,然后被格式化成所需的verb(若有的话)    
2.如果操作数实现了字符串 的String()方法,那个方法转换对象成字符串,然后被格式化成所需的verb(若有的话)    

To avoid recursion in cases such as      
为了避免递归等情况    
```golang
	type X string
	func (x X) String() string { return Sprintf("<%s>", x) }
```

convert the value before recurring:    
在递归之前转换值    
```golang
	func (x X) String() string { return Sprintf("<%s>", string(x)) }
```

Explicit argument indexes:    

In Printf, Sprintf, and Fprintf, the default behavior is for each formatting verb to format successive arguments passed in the call.     
However, the notation [n] immediately before the verb indicates that the nth one-indexed argument is to be formatted instead.     
The same notation before a '*' for a width or precision selects the argument index holding the value.     
After processing a bracketed expression [n], arguments n+1, n+2, etc. will be processed unless otherwise directed.    

显式参数索引:    
在 Printf, Sprintf, 和 Fprintf,默认对调用时的每个verb 连续传递的参数 格式化        
然而,符号 [n] 表示 第几个参数被格式化      
同样的符合 在'*'之前 宽度或精度选择参数索引值。     
括号后的表达示 [n], 参数 n+1, n+2, 等 除非有其他的说明 不然会被处理    

For example,    
比如:
```golang
	fmt.Sprintf("%[2]d %[1]d\n", 11, 22)
```

will yield "22, 11", while        
将产生 "22, 11",而    
```golang
	fmt.Sprintf("%[3]*.[2]*[1]f", 12.0, 2, 6),
```

equivalent to
等价于
```golang
	fmt.Sprintf("%6.2f", 12.0),
```

will yield " 12.00". Because an explicit index affects subsequent verbs,      
	this notation can be used to print the same values multiple times by resetting the index for the first argument to be repeated:        
结果是" 12.00".因为一个显示的值数影响后面的verbs,这个符号可以 根据第一个参数 重置索引 打印多次同一个值    
```golang
	fmt.Sprintf("%d %d %#[1]x %#x", 16, 17)
```
will yield "16 17 0x10 0x11".    
结果是"16 17 0x10 0x11".    

Format errors:     
If an invalid argument is given for a verb, such as providing a string to %d,      
	the generated string will contain a description of the problem, as in these examples:       
格式化错误:     
如果verb的参数是一个无效值,比如字符串 对应到 %d, 生成的字符串将会包含错误描述, 像下面这些例子    
```golang
	Wrong type or unknown verb: %!verb(type=value)  错误的类型或未知的verb
		Printf("%d", hi):          %!d(string=hi)
		
	Too many arguments: %!(EXTRA type=value)  参数太多
		Printf("hi", "guys"):      hi%!(EXTRA string=guys)
		
	Too few arguments: %!verb(MISSING)  参数太少
		Printf("hi%d"):            hi %!d(MISSING)
		
		
	Non-int for width or precision: %!(BADWIDTH) or %!(BADPREC)  非int的宽度或精度
		Printf("%*s", 4.5, "hi"):  %!(BADWIDTH)hi
		Printf("%.*s", 4.5, "hi"): %!(BADPREC)hi
		
	Invalid or invalid use of argument index: %!(BADINDEX)  无效 或者 无效使用 参数索引
		Printf("%*[2]d", 7):       %!d(BADINDEX)
		Printf("%.[2]d", 7):       %!d(BADINDEX)
```

All errors begin with the string "%!" followed sometimes by a single character (the verb) and end with a parenthesized description.    
If an Error or String method triggers a panic when called by a print routine, the fmt package reformats the error message from the panic,     
	decorating it with an indication that it came through the fmt package.	
For example, if a String method calls panic("bad"), the resulting formatted message will look like  
所有的错误都以字符串 "%!" 开始 ,有时候跟着单个字符 然后结束 加上括弧    
如果Error或String方法在调用 打印 程序 触发 panic , fmt包重新定义 panic的错误信息格式,美化 然后抛出 fmt包    
例如 如果  String方法调用 panic("bad"), 结果的信息将会是下面这个样子    
```golang
	%!s(PANIC=bad)
```
The %!s just shows the print verb in use when the failure occurred.       
 %!s 只是显示打印verb 在失败的时候有调用    
 


##Scanning     
An analogous set of functions scans formatted text to yield values.     
Scan, Scanf and Scanln read from os.Stdin;    
Fscan, Fscanf and Fscanln read from a specified io.Reader;    
Sscan, Sscanf and Sscanln read from an argument string.    
Scanln, Fscanln and Sscanln stop scanning at a newline and require that the items be followed by one;     
Scanf, Fscanf and Sscanf require newlines in the input to match newlines in the format;    
the other routines treat newlines as spaces.     

类似scans函数集 格式化文本 来产生值        
Scan, Scanf 和 Scanln从 os.Stdin读取     
Fscan, Fscanf 和 Fscanln 从指定的io.Reader读取     
Sscan, Sscanf and Sscanln 从字符串参数中读取     
Scanln, Fscanln 和 Sscanln 遇到换行符会停止扫描 并且 之后需要跟随一个项     
Scanf, Fscanf 和 Sscanf   需要输入匹配的格式 换行符来 换行     
其他的把换行符 当空格     


Scanf, Fscanf, and Sscanf parse the arguments according to a format string, analogous to that of Printf.      
For example, %x will scan an integer as a hexadecimal number, and %v will scan the default representation format for the value.     

Scanf, Fscanf, 和 Sscanf 根据字符串格式解析参数 ,类似Printf     
比如 %x 会将整型 扫描成 16进制数字,  %v  把值 扫描成 默认格式     

The formats behave analogously to those of Printf with the following exceptions:     
类似Printf 有以下 异常:
```golang
%p is not implemented
%T is not implemented
%e %E %f %F %g %G are all equivalent and scan any floating point or complex value
%s and %v on strings scan a space-delimited token
Flags # and + are not implemented.

%p未实现
%T未实现
%e %E %f %F %g %G 都等价 并且扫描 任何浮点型 或复合值
%s 和 %v 遇到字符串 扫描一个空格分隔的标记
 # 和 + 未实现

```

The familiar base-setting prefixes 0 (octal) and 0x (hexadecimal) are accepted when scanning integers without a format or with the %v verb.     
熟悉的基础设置是 以0(8进制) 和以0x(16进制) 开头, 当扫描 整型没有格式或者%v verb     

Width is interpreted in the input text (%5s means at most five runes of input will be read to scan a string)      
	but there is no syntax for scanning with a precision (no %5.2f, just %5f).     
宽输入文本解释(%5s意味着 输入最多有5条规则 将会读到  扫描的字符串) 但是没有精度的语法(没有 %5.2f, 只有 %5f)     

When scanning with a format, all non-empty runs of space characters (except newline) are equivalent to a single space in both the format and the input.      
With that proviso, text in the format string must match the input text;      
scanning stops if it does not, with the return value of the function indicating the number of arguments scanned.     
当扫描一个格式,  所有非空的 空格字符(除了换行符) 等价于单个的空格 不官是在输入还是格式     
根据这些条件,文本在字符串格式化 必须 匹配输入文本     
如果扫描参数数量的 功能说明返回值 没有 那停止扫描     

In all the scanning functions, a carriage return followed immediately by a newline is treated as a plain newline (\r\n means the same as \n).     
在所有的扫描函数.  一个回车跟着换行符被看成是普通的换行符(\r\n 和\n是一样的).     

In all the scanning functions, if an operand implements method Scan (that is, it implements the Scanner interface)      
	that method will be used to scan the text for that operand.      
Also, if the number of arguments scanned is less than the number of arguments provided, an error is returned.     
在所有的扫描函数,如果运算对象实现了Scan方法(就是实现了Scanner接口) 那个方法 将被用于那个操作数扫描文本     
如果 参数扫描的数量少于 参数的数量 返回错误.     

All arguments to be scanned must be either pointers to basic types or implementations of the Scanner interface.     
所有参数扫描 必须指向基本类型或实现Scanner 接口     

Note: Fscan etc. can read one character (rune) past the input they return, which means that a loop calling a scan routine may skip some of the input.      
This is usually a problem only when there is no space between input values.      
If the reader provided to Fscan implements ReadRune, that method will be used to read characters.     
If the reader also implements UnreadRune, that method will be used to save the character and successive calls will not lose data.      
To attach ReadRune and UnreadRune methods to a reader without that capability, use bufio.NewReader.     

注:Fscan等 . 可以通过输入返回,读取一个字符, 意味着 循环调用一个扫描程序 可能会跳过某些输入     
这通常在 两个 输入之间没有空格的错误     
如果reader提供Fscan 实现ReadRune,那个方法会用于读取字符     
如果reader 也实现了UnreadRune,那意味着方法会保存字符 并且连续调用会丢失数据     
使用bufio.NewReader,附属在 ReadRune 和 UnreadRune 的reader 没有这种方法     



##func Errorf     
```golang
func Errorf(format string, a ...interface{}) error
```
Errorf formats according to a format specifier and returns the string as a value that satisfies error.     
根据指定的格式 格式化并 返回错误的 字符串值     


##func Fprint     
```golang
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
```
Fprint formats using the default formats for its operands and writes to w.      
Spaces are added between operands when neither is a string.      
It returns the number of bytes written and any write error encountered.     
使用默认的格式 格式化 运算对象 并写入w     
运算对象之间加入间隔     
返回写入的字节数, 和任何遇到的错误     


func Fprintf     
```golang
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
```
Fprintf formats according to a format specifier and writes to w.      
It returns the number of bytes written and any write error encountered.     
根据指定的格式格式化 并写入w     
返回写入的字节数 和任何遇到的错误     


func Fprintln     
```golang
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```
Fprintln formats using the default formats for its operands and writes to w.      
Spaces are always added between operands and a newline is appended.      
It returns the number of bytes written and any write error encountered.     
使用默认的格式 格式化运算对象 并 写入w     
通常在运算对象和换行符之间加入间隔     
返回写入的字节数 和任何遇到的错误        


func Fscan        
```golang
func Fscan(r io.Reader, a ...interface{}) (n int, err error)
```
Fscan scans text read from r, storing successive space-separated values into successive arguments.         
Newlines count as space. It returns the number of items successfully scanned.         
If that is less than the number of arguments, err will report why.        
Fscan 从r中读取扫描文本, 存储连续的空格到 连续的参数        
换行符 算做 空格. 它返回 扫描成功的项的数量        
如果少于参数数量 会返回为什么会错误        


func Fscanf        
```golang
func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error)
```
Fscanf scans text read from r, storing successive space-separated values into successive arguments as determined by the format.         
It returns the number of items successfully parsed.        
Fscanf从r中读取扫描文本,存储连续的空格到 连续的参数        
它返回成功解析的项目的数量        


func Fscanln        
```golang
func Fscanln(r io.Reader, a ...interface{}) (n int, err error)
```
Fscanln is similar to Fscan, but stops scanning at a newline and after the final item there must be a newline or EOF.        
Fscanln 类似Fscan  但停止扫描换行和最后一个项目之后，必须有一个换行符或EOF。        


func Print        
```golang
func Print(a ...interface{}) (n int, err error)
```
Print formats using the default formats for its operands and writes to standard output.         
Spaces are added between operands when neither is a string.         
It returns the number of bytes written and any write error encountered.        
使用默认的格式  格式化运算对象 并 写入标准输出        
运算对象之间加入间隔并加入换行符        
返回写入的字节数 和任何写入遇到的错误          


func Printf        
```golang
func Printf(format string, a ...interface{}) (n int, err error)
```
Printf formats according to a format specifier and writes to standard output.         
It returns the number of bytes written and any write error encountered.        
使用根据指定的格式  格式化运算对象 并 写入标准输出        
返回写入的字节数 和任何写入遇到的错误        


func Println
```golang
func Println(a ...interface{}) (n int, err error)
```
Println formats using the default formats for its operands and writes to standard output.         
Spaces are always added between operands and a newline is appended.         
It returns the number of bytes written and any write error encountered.        
使用默认的格式  格式化运算对象 并 写入标准输出        
运算对象之间加入间隔并加入换行符        
返回写入的字节数 和任何写入遇到的错误        















func Scan
```golang
func Scan(a ...interface{}) (n int, err error)
```
Scan scans text read from standard input, storing successive space-separated values into successive arguments. 
Newlines count as space. It returns the number of items successfully scanned. 
If that is less than the number of arguments, err will report why.
从标准输入读取扫描文本.存储连续的空格值到连续的参数
换行符算做间隔.返回扫墓成功项的数量 
如果少于参数的数量 ,报告为什么会错误


func Scanf
```golang
func Scanf(format string, a ...interface{}) (n int, err error)
```
Scanf scans text read from standard input, storing successive space-separated values into successive arguments as determined by the format. 
It returns the number of items successfully scanned.
Fscanf从r中读取扫描文本,存储连续的空格到 连续的参数 
返回扫描成功项的数量


func Scanln
```golang
func Scanln(a ...interface{}) (n int, err error)
```
Scanln is similar to Scan, but stops scanning at a newline and after the final item there must be a newline or EOF.
类似Scan 但停止扫描换行和最后一个项目之后，必须有一个换行符或EOF。   


func Sprint
```golang
func Sprint(a ...interface{}) string
```
Sprint formats using the default formats for its operands and returns the resulting string. 
Spaces are added between operands when neither is a string.
使用默认的格式 格式化运算对象 返回结果字符串
运算对象之间加入间隔	


func Sprintf
```golang
func Sprintf(format string, a ...interface{}) string
```
Sprintf formats according to a format specifier and returns the resulting string.
根据指定的格式 格式化 并返回结果字符串


func Sprintln
```golang
func Sprintln(a ...interface{}) string
```
Sprintln formats using the default formats for its operands and returns the resulting string. 
Spaces are always added between operands and a newline is appended.
使用默认的格式 格式化运算对象 返回结果字符串
运算对象之间加入间隔并追加换行符


func Sscan
```golang
func Sscan(str string, a ...interface{}) (n int, err error)
```
Sscan scans the argument string, storing successive space-separated values into successive arguments.
Newlines count as space. It returns the number of items successfully scanned. 
If that is less than the number of arguments, err will report why.
从扫描参数字符串.存储连续的空格值到连续的参数
换行符算做间隔.返回扫墓成功项的数量 
如果少于参数的数量 ,报告为什么会错误


func Sscanf
```golang
func Sscanf(str string, format string, a ...interface{}) (n int, err error)
```
Sscanf scans the argument string, storing successive space-separated values into successive arguments as determined by the format.
It returns the number of items successfully parsed.
从扫描参数字符串.存储连续的空格值到连续的参数
返回解析成功项的数量


func Sscanln
```golang
func Sscanln(str string, a ...interface{}) (n int, err error)
```
Sscanln is similar to Sscan, but stops scanning at a newline and after the final item there must be a newline or EOF.
类似Sscan  但停止扫描换行和最后一个项目之后，必须有一个换行符或EOF。



type Formatter
```golang
type Formatter interface {
    Format(f State, c rune)
}
```
Formatter is the interface implemented by values with a custom formatter. 
The implementation of Format may call Sprint(f) or Fprint(f) etc. to generate its output.
通过使用自定义值 实现接口
实施Format 可以调用 Sprint(f) 或 Fprint(f)等. 以产生其输出

type GoStringer
```golang
type GoStringer interface {
    GoString() string
}
```
GoStringer is implemented by any value that has a GoString method, which defines the Go syntax for that value.
The GoString method is used to print values passed as an operand to a %#v format.
是任何有GoString方法 的值 ,为那个值定义GO语法
打印 %#v 格式的运算对象值


##type ScanState
```golang
type ScanState interface {
    // ReadRune reads the next rune (Unicode code point) from the input.
    // If invoked during Scanln, Fscanln, or Sscanln, ReadRune() will
    // return EOF after returning the first '\n' or when reading beyond
    // the specified width.
    // ReadRune从输入里读取下一个rune(Unicode)
     //如果在Scanln, Fscanln, 或 Sscanln 期间调用,ReadRune()在返回第一个 '\n'或者读取到指定的宽度后 将会返回EOF
    ReadRune() (r rune, size int, err error)
    
    
    // UnreadRune causes the next call to ReadRune to return the same rune.
    //UnreadRune 调用ReadRune 返回相同的rune
    UnreadRune() error
    
    
    // SkipSpace skips space in the input. Newlines are treated as space
    // unless the scan operation is Scanln, Fscanln or Sscanln, in which case
    // a newline is treated as EOF.
    //SkipSpace  跳过输入的间隔.换行符被当成间隔 除非扫描操作是Scanln, Fscanln 或 Sscanln, 这种情况下 换行符被当成 EOF
    SkipSpace()
    
    // Token skips space in the input if skipSpace is true, then returns the
    // run of Unicode code points c satisfying f(c).  If f is nil,
    // !unicode.IsSpace(c) is used; that is, the token will hold non-space
    // characters.  Newlines are treated as space unless the scan operation
    // is Scanln, Fscanln or Sscanln, in which case a newline is treated as
    // EOF.  The returned slice points to shared data that may be overwritten
    // by the next call to Token, a call to a Scan function using the ScanState
    // as input, or when the calling Scan method returns.
     //如果skipSpace是true,Token跳过输入的间隔,然后返回执行的 满足f(c)的Unicode c.
     //如果f是nil,使用!unicode.IsSpace(c);那样,token将会保留非间隔字符.
     //换行符被当作间隔, 除非扫描操作是Scanln, Fscanln 或 Sscanln, 这种情况下 换行符被当成 EOF
     //返回的slice指向 共享数据,可能会在下一次调用Token的时候被重写. 
     //调用Scan函数使用ScanState作为输入,或者 当Scan调用返回.
    Token(skipSpace bool, f func(rune) bool) (token []byte, err error)
    

    // Width returns the value of the width option and whether it has been set.
    // The unit is Unicode code points.
     // 返回值的宽的选项 无论是否设置
   //unit是Unicode
    Width() (wid int, ok bool)
    
    
    // Because ReadRune is implemented by the interface, Read should never be
    // called by the scanning routines and a valid implementation of
    // ScanState may choose always to return an error from Read.
     //因为ReadRune 根据接口实现,Read应该从不用扫描程序调用并且 一个 有效的ScanState实现 通常会选择从Read返回错误
    Read(buf []byte) (n int, err error)
}
```
ScanState represents the scanner state passed to custom scanners. 
Scanners may do rune-at-a-time scanning or ask the ScanState to discover the next space-delimited token.
表示自定义的scanners
Scanners  可以rune 一次扫描 或者访问ScanState 寻找下一个分隔 token


##type Scanner
```golang
type Scanner interface {
    Scan(state ScanState, verb rune) error
}
```
Scanner is implemented by any value that has a Scan method, 
	which scans the input for the representation of a value and stores the result in the receiver, which must be a pointer to be useful. 
The Scan method is called for any argument to Scan, Scanf, or Scanln that implements it.
实现任何有Scan方法的值, 扫描输入的值 并且 在接收方存储结构,  必须是有效的指针
Scan 方法 能被任何实现 Scan, Scanf, or Scanln 的参数调用

##type State
```golang
type State interface {
    // Write is the function to call to emit formatted output to be printed.
    //Write 是发送格式化输出打印的方法
    Write(b []byte) (ret int, err error)
    
    
    // Width returns the value of the width option and whether it has been set.
     //返回值的宽的选项 无论是否设置
    Width() (wid int, ok bool)
    
    
    // Precision returns the value of the precision option and whether it has been set.
    ///返回值的精度的选项 无论是否设置
    Precision() (prec int, ok bool)

    // Flag reports whether the flag c, a character, has been set.
    //报告 flag c , a 字符 是否设置
    Flag(c int) bool
}
```
State represents the printer state passed to custom formatters. 
It provides access to the io.Writer interface plus information about the flags and options for the operand's format specifier.
表示printer到自定义格式的状态
提供访问io.Writer的接口 对运算对象以指定的格式 添加有关flag 的信息和选项


##type Stringer
```golang
type Stringer interface {
    String() string
}
```
Stringer is implemented by any value that has a String method, which defines the “native” format for that value. 
The String method is used to print values passed as an operand to any format that accepts a string or to an unformatted printer such as Print.
是任何有String方法 的值 ,为那个值定义了“native”格式
String方法用来通过任何格式访问 字符串 或 未格式化 的 运算对象, 类似Print













