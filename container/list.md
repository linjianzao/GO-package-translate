包地址：http://golang.org/pkg/container/list/

```golang
Overview 

Package list implements a doubly linked list.
实现双向链表

To iterate over a list (where l is a *List):

for e := l.Front(); e != nil; e = e.Next() {
	// do something with e.Value
}
```

```golang
	Code:

	// Create a new list and put some numbers in it. 创建一个新的 list ，放入一些数字
	l := list.New()
	e4 := l.PushBack(4)
	e1 := l.PushFront(1)
	l.InsertBefore(3, e4)
	l.InsertAfter(2, e1)
	
	// Iterate through list and and print its contents. 迭代list 打印出内容
	for e := l.Front(); e != nil; e = e.Next() {
	    fmt.Println(e.Value)
	}
	
	Output:
	
	1
	2
	3
	4
```


type Element
```golang
  type Element struct {
        // The value stored with this element.
        Value interface{}
        // contains filtered or unexported fields
  }
  Element is an element of a linked list. 
  元素链表
```

```golang 
    func (e *Element) Next() *Element
       Next returns the next list element or nil. 
       返回下一个链表元素或者nil
```

```golang    
    func (e *Element) Prev() *Element
      Prev returns the previous list element or nil. 
      返回上一个链表元素或者nil
```

type List
```golang
  type List struct {
        // contains filtered or unexported fields
  }
  List represents a doubly linked list. The zero value for List is an empty list ready to use. 
	双向链表。链表的零值是空链表。
```

```golang
    func New() *List
       New returns an initialized list. 
       返回一个初始化的链表
```

```golang    
    func (l *List) Back() *Element
      Back returns the last element of list l or nil. 
      	返回链表的最后一个元素或者nil
```

```golang    
    func (l *List) Front() *Element
      Front returns the first element of list l or nil 
      返回链表的第一个元素或者nil
```

```golang    
    func (l *List) Init() *List
      Init initializes or clears list l. 
        初始化或者清除链表
```

```golang    
    func (l *List) InsertAfter(v interface{}, mark *Element) *Element
      InsertAfter inserts a new element e with value v immediately after mark and returns e. 
      If mark is not an element of l, the list is not modified. 
      	在mark元素之后把值为v的e元素插入，然后返回e。如果mark不是l的元素，链表不会被修改
```

```golang
    func (l *List) InsertBefore(v interface{}, mark *Element) *Element
      InsertBefore inserts a new element e with value v immediately before mark and returns e. If mark is not an element of l, the list is not modified. 
        在mark元素之前把值为v的e元素插入，然后返回e。如果mark不是l的元素，链表不会被修改
```

```golang    
    func (l *List) Len() int
      Len returns the number of elements of list l. 
      	返回l的元素数量
```

```golang
    func (l *List) MoveToBack(e *Element)
      MoveToBack moves element e to the back of list l. If e is not an element of l, the list is not modified. 
      把元素e移动到链表l后面。如果e不是链表的元素 ，链表不会被修改
```

```golang    
    func (l *List) MoveToFront(e *Element)
      MoveToFront moves element e to the front of list l. 
      If e is not an element of l, the list is not modified. 
      把元素e移动到链表l前面。如果e不是链表的元素 ，链表不会被修改
```

```golang    
    func (l *List) PushBack(v interface{}) *Element
       PushBack inserts a new element e with value v at the back of list l and returns e.
         在l链表最后插入值为v的元素e ，返回e
```

```golang
    func (l *List) PushBackList(other *List)
       PushBackList inserts a copy of an other list at the back of list l. 
       The lists l and other may be the same. 
      	 插入从别的地方复制的链表到l后面。l和其他的可以是一样的链表
```

```golang    
    func (l *List) PushFront(v interface{}) *Element
      Pushfront inserts a new element e with value v at the front of list l and returns e.
       在l链表之前插入值为v的元素e ，返回e
```

```golang    
    func (l *List) PushFrontList(other *List)
      PushFrontList inserts a copy of an other list at the front of list l. The lists l and other may be the same. 
       插入从别的地方复制的链表到l前面。l和其他的都必须是一样的链表
```

```golang    
    func (l *List) Remove(e *Element) interface{}
      Remove removes e from l if e is an element of list l. It returns the element value e.Value. 
        如果e在l里面删除e元素。返回元素e.Value的值
```