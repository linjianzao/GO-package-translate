包地址：http://golang.org/pkg/compress/bzip2/
```golang
Package bzip2 implements bzip2 decompression.
bzip2实现bzip2压缩解压
```
``golang
func NewReader(r io.Reader) io.Reader
  NewReader returns an io.Reader which decompresses bzip2 data from r.
  从bzip2的r返回一个io.Reader
```
 
```golang 
type StructuralError
  A StructuralError is returned when the bzip2 data is found to be syntactically invalid.
  bzip2 data 遇到语法错误时返回
  
    func (s StructuralError) Error() string
```