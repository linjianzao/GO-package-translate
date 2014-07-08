Package mime

Overview ▾

Package mime implements parts of the MIME spec.
mime包实现 部分MIME规范


func AddExtensionType

func AddExtensionType(ext, typ string) error
AddExtensionType sets the MIME type associated with the extension ext to typ. 
The extension should begin with a leading dot, as in ".html".
AddExtensionType设置 MIME 类型 关联扩展 ext 到 typ



func FormatMediaType

func FormatMediaType(t string, param map[string]string) string
FormatMediaType serializes mediatype t and the parameters param as a media type conforming to RFC 2045 and RFC 2616. 
The type and parameter names are written in lower-case. 
When any of the arguments result in a standard violation then FormatMediaType returns the empty string.
FormatMediaType 序列化 mediatype  t和参数 param 成 一个符合RFC 2045 和 RFC 2616   的media类型
类型和参数名都是 小写的.
当有任何参数结果违反 标准, FormatMediaType 返回空字符串



func ParseMediaType

func ParseMediaType(v string) (mediatype string, params map[string]string, err error)
ParseMediaType parses a media type value and any optional parameters, per RFC 1521. 
Media types are the values in Content-Type and Content-Disposition headers (RFC 2183). 
On success, ParseMediaType returns the media type converted to lowercase and trimmed of white space and a non-nil map. 
The returned map, params, maps from the lowercase attribute to the attribute value with its case preserved.
ParseMediaType 根据RFC 1521  解析一个media 类型的值 和任何参数选项.
Media类型是 Content-Type 和 Content-Disposition 头的值 (RFC 2183). 
成功的话,ParseMediaType 返回 media 类型 转化成小写字母 并且 修剪空白  和一个非nil map
返回的map,params , maps 从小写属性的属性值与它的情况下保存。




func TypeByExtension

func TypeByExtension(ext string) string
TypeByExtension returns the MIME type associated with the file extension ext. 
The extension ext should begin with a leading dot, as in ".html". When ext has no associated type, TypeByExtension returns "".
The built-in table is small but on unix it is augmented by the local system's mime.types file(s) if available under one or more of these names:
TypeByExtension 返回  MIME类型 关联的 文件扩展ext.
扩展ext 必须以前导点开始,例如 ".html". 当ext 没有关联类型时,TypeByExtension返回" ".
内建表很小 但是在 unix 上它 根据本地系统的mime.types file(s) 增强,   如果变量底下一个或更多这样的名字

/etc/mime.types
/etc/apache2/mime.types
/etc/apache/mime.types

Windows system mime types are extracted from registry.
Text types have the charset parameter set to "utf-8" by default.
Windows系统 的mime 类型 从注册表提取.
文本类型字符参数默认设置成 "utf-8"



