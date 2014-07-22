Package user

Overview ▾

Package user allows user account lookups by name or id.
user包 允许用户账户 按 name或id查找.


type UnknownUserError
```golang
type UnknownUserError string
```
UnknownUserError is returned by Lookup when a user cannot be found.
当一个用户不能发现时,UnknownUserError通过Lookup 返回



func (UnknownUserError) Error
```golang
func (e UnknownUserError) Error() string
```


type UnknownUserIdError
```golang
type UnknownUserIdError int
```
UnknownUserIdError is returned by LookupId when a user cannot be found.
当一个用户不能发现时,UnknownUserIdError通过LookupId 返回



func (UnknownUserIdError) Error
```golang
func (e UnknownUserIdError) Error() string
```


type User
```golang
type User struct {
        Uid      string // user id
        Gid      string // primary group id
        Username string
        Name     string
        HomeDir  string
}
```
User represents a user account.

On posix systems Uid and Gid contain a decimal number representing uid and gid. 
On windows Uid and Gid contain security identifier (SID) in a string format. 
On Plan 9, Uid, Gid, Username, and Name will be the contents of /dev/user.
User表示一个用户 账户.
在posix系统, Uid和Gid 包含一个 十进制数 表示的uid和 gid.
在windows, Uid和Gid包含 字符串格式的安全标识符(SID)
在Plan 9.Uid, Gid, Username, 和Name 将是/dev/user的内容.


func Current
```golang
func Current() (*User, error)
```
Current returns the current user.
Current 返回当前的用户


func Lookup
```golang
func Lookup(username string) (*User, error)
```
Lookup looks up a user by username. If the user cannot be found, the returned error is of type UnknownUserError.
Lookup 通过username 查找一个用户. 如果用户没有发现,返回错误是UnknownUserError类型


func LookupId
```golang
func LookupId(uid string) (*User, error)
```
LookupId looks up a user by userid. If the user cannot be found, the returned error is of type UnknownUserIdError.
LookupId 通过userid查找用户. 如果用户不能发现,返回的错误是UnknownUserIdError 类型.

