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
			值表示的GO语法
		
	%T	a Go-syntax representation of the type of the value
		值类型的GO语法
	
	%%	a literal percent sign; consumes no value
		一个文本百分号;不消耗值
```
	