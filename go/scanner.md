包地址:http://golang.org/pkg/go/scanner/               
#Package scanner

##Overview ▾

Package scanner implements a scanner for Go source text. 
It takes a []byte as source which can then be tokenized through repeated calls to the Scan method.
实现一个GO源文本扫描器
它需要一个[]byte 为源,  然后可以通过重复调用Scan方法进行标记化

func PrintError

func PrintError(w io.Writer, err error)
PrintError is a utility function that prints a list of errors to w, one error per line, if the err parameter is an ErrorList.
Otherwise it prints the err string.
PrintError 是一个打印错误的列表到w效用函数,每行一个错误,如果错误是一个ErrorList.
否则它打印err 字符串


type Error

type Error struct {
        Pos token.Position
        Msg string
}
In an ErrorList, an error is represented by an *Error. 
The position Pos, if valid, points to the beginning of the offending token, and the error condition is described by Msg.
在一个ErrorList,一个错误表示 *Error.
位置Pos, 如果有效,指向违规的token开始,并且Msg 描述 错误条件



func (Error) Error

func (e Error) Error() string
Error implements the error interface.
实现error接口



type ErrorHandler

type ErrorHandler func(pos token.Position, msg string)
An ErrorHandler may be provided to Scanner.Init. 
If a syntax error is encountered and a handler was installed, the handler is called with a position and an error message. 
The position points to the beginning of the offending token.
ErrorHandler 可以被提供给Scanner.Init。
如果遇到语法错误 并且处理程序被安装,该处理程序被调用时的位置和错误信息
位置指向违规token开始的地方



type ErrorList

type ErrorList []*Error
ErrorList is a list of *Errors. 
The zero value for an ErrorList is an empty ErrorList ready to use.
一个*Error列表
ErrorList的零值是一个空的 准备被使用的ErrorList


func (*ErrorList) Add

func (p *ErrorList) Add(pos token.Position, msg string)
Add adds an Error with given position and error message to an ErrorList.
使用给定的位置和错误信息 添加一个Error到ErrorList

func (ErrorList) Err

func (p ErrorList) Err() error
Err returns an error equivalent to this error list. If the list is empty, Err returns nil.
返回一个等价于这个错误列表的错误.如果 列表是空的 Err返回nil


func (ErrorList) Error

func (p ErrorList) Error() string
An ErrorList implements the error interface.
实现一个错误接口

func (ErrorList) Len

func (p ErrorList) Len() int
ErrorList implements the sort Interface.
实现一个排序接口

func (ErrorList) Less

func (p ErrorList) Less(i, j int) bool


func (*ErrorList) RemoveMultiples

func (p *ErrorList) RemoveMultiples()
RemoveMultiples sorts an ErrorList and removes all but the first error per line.
排序一个ErrorList 并且删除每行第一个错误。

func (*ErrorList) Reset

func (p *ErrorList) Reset()
Reset resets an ErrorList to no errors.
重值一个ErrorList 成没有错误

func (ErrorList) Sort

func (p ErrorList) Sort()
Sort sorts an ErrorList. *Error entries are sorted by position, other errors are sorted by error message, and before any *Error entry.
在任何*Error之前 排序一个ErrorList.整个*Error 根据位置排序,其他错误用错误信息排序. 

func (ErrorList) Swap

func (p ErrorList) Swap(i, j int)
type Mode

type Mode uint
A mode value is a set of flags (or 0). They control scanner behavior.
mode的值是一个flag集(或者0).他们控制scanner行为

const (
        ScanComments Mode = 1 << iota // return comments as COMMENT tokens

)
type Scanner

type Scanner struct {

        // public state - ok to modify
        
        ErrorCount int // number of errors encountered
        
        // contains filtered or unexported fields
}
A Scanner holds the scanner's internal state while processing a given text. 
It can be allocated as part of another data structure but must be initialized via Init before use.
使用给定的文本处理 扫描器内部的状态

func (*Scanner) Init

func (s *Scanner) Init(file *token.File, src []byte, err ErrorHandler, mode Mode)
Init prepares the scanner s to tokenize the text src by setting the scanner at the beginning of src. 
The scanner uses the file set file for position information and it adds line information for each line. 
It is ok to re-use the same file when re-scanning the same file as line information which is already present is ignored. 
Init causes a panic if the file size does not match the src size.



Calls to Scan will invoke the error handler err if they encounter a syntax error and err is not nil. Also, for each error encountered, the Scanner field ErrorCount is incremented by one. The mode parameter determines how comments are handled.

Note that Init may call err if there is an error in the first character of the file.

func (*Scanner) Scan

func (s *Scanner) Scan() (pos token.Pos, tok token.Token, lit string)
Scan scans the next token and returns the token position, the token, and its literal string if applicable. The source end is indicated by token.EOF.

If the returned token is a literal (token.IDENT, token.INT, token.FLOAT, token.IMAG, token.CHAR, token.STRING) or token.COMMENT, the literal string has the corresponding value.

If the returned token is a keyword, the literal string is the keyword.

If the returned token is token.SEMICOLON, the corresponding literal string is ";" if the semicolon was present in the source, and "\n" if the semicolon was inserted because of a newline or at EOF.

If the returned token is token.ILLEGAL, the literal string is the offending character.

In all other cases, Scan returns an empty literal string.

For more tolerant parsing, Scan will return a valid token if possible even if a syntax error was encountered. Thus, even if the resulting token sequence contains no illegal tokens, a client may not assume that no error occurred. Instead it must check the scanner's ErrorCount or the number of calls of the error handler, if there was one installed.

Scan adds line information to the file added to the file set with Init. Token positions are relative to that file and thus relative to the file set.



Example
package main

import (
	"fmt"
	"go/scanner"
	"go/token"
)

func main() {
	// src is the input that we want to tokenize.
	src := []byte("cos(x) + 1i*sin(x) // Euler")

	// Initialize the scanner.
	var s scanner.Scanner
	fset := token.NewFileSet()                      // positions are relative to fset
	file := fset.AddFile("", fset.Base(), len(src)) // register input "file"
	s.Init(file, src, nil /* no error handler */, scanner.ScanComments)

	// Repeated calls to Scan yield the token sequence found in the input.
	for {
		pos, tok, lit := s.Scan()
		if tok == token.EOF {
			break
		}
		fmt.Printf("%s\t%s\t%q\n", fset.Position(pos), tok, lit)
	}

}
































