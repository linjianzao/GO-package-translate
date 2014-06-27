#Package html        

##Overview ▾        
Package html provides functions for escaping and unescaping HTML text.        
html包提供 转码和 转义HTML文本 函数        
        
##func EscapeString        
```golang
func EscapeString(s string) string
```
EscapeString escapes special characters like "<" to become "&lt;".         
It escapes only five such characters: <, >, &, ' and ".         
UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.        
转码特殊字符 类似 "<" 变成"&lt;"        
它只转码5个字符 <, >, &, ' 和 ".        
        
        
##func UnescapeString        
```golang
func UnescapeString(s string) string
```
UnescapeString unescapes entities like "&lt;" to become "<".         
It unescapes a larger range of entities than EscapeString escapes.         
For example, "&aacute;" unescapes to "á", as does "&#225;" and "&xE1;".         
UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.        
转义实体 类似"&lt;" 变成"<"        
它转义 比 EscapeString 转码 更大范围的实体.        
例如 ,"&aacute;"  转义成"á",同样  "&#225;" 和 "&xE1;".        
UnescapeString(EscapeString(s)) == s , 但是这个转换并不总是对的        
