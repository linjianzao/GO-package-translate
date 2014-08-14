包地址：http://golang.org/pkg/errors/

Overview

```golang
package errors_test

import (
    "fmt"
    "time"
)

// MyError is an error implementation that includes a time and message.
type MyError struct {
    When time.Time
    What string
}

func (e MyError) Error() string {
    return fmt.Sprintf("%v: %v", e.When, e.What)
}

func oops() error {
    return MyError{
        time.Date(1989, 3, 15, 22, 30, 0, 0, time.UTC),
        "the file system has gone away",
    }
}

func Example() {
    if err := oops(); err != nil {
        fmt.Println(err)
    }
    // Output: 1989-03-15 22:30:00 +0000 UTC: the file system has gone away
}

``

func New
```golang
func New(text string) error
	New returns an error that formats as the given text.
	使用给定的文本格式返回错误
```

▾ Example

Code:
```golang
err := errors.New("emit macho dwarf: elf header corrupted")
if err != nil {
    fmt.Print(err)
}
```
Output:
```golang
emit macho dwarf: elf header corrupted
```


▾ Example (Errorf)
```golang
The fmt package's Errorf function lets us use the package's formatting features to create descriptive error messages.
fmt包的Errorf函数让我们使用包的格式 创建描述性错误信息

Code:
```golang
const name, id = "bimmler", 17
err := fmt.Errorf("user %q (id %d) not found", name, id)
if err != nil {
    fmt.Print(err)
}
```
Output:
```golang
user "bimmler" (id 17) not found
```
```