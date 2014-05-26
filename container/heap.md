包地址：http://golang.org/pkg/container/heap/

```golang
Package heap provides heap operations for any type that implements heap.Interface. 
A heap is a tree with the property that each node is the minimum-valued node in its subtree.
提供任何能实现heap.Interface的类型的操作。堆是一个优先最低值子树节点的树
A heap is a common way to implement a priority queue. To build a priority queue, 
implement the Heap interface with the (negative) priority as the ordering for the Less method, 
so Push adds items while Pop removes the highest-priority item from the queue. 
The Examples include such an implementation; the file example_pq_test.go has the complete source. 
一个堆是一个实现优先队列的常用方法。建立一个优先队列，实现堆接口的顺序减。所以队列 push一个项会 删除最高优先的项。例子包含实现。
example_pq_test.go有所有的源码
```

```golang
This example inserts several ints into an IntHeap, checks the minimum, and removes them in order of priority.
这个例子是 插入一些整型数到 一个IntHeap，检查最小值，并按优先顺序删除他们
Code:

// This example demonstrates an integer heap built using the heap interface.
package heap_test

import (
    "container/heap"
    "fmt"
)


// An IntHeap is a min-heap of ints.
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    // Push and Pop use pointer receivers because they modify the slice's length,
    // not just its contents.
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

// This example inserts several ints into an IntHeap, checks the minimum,
// and removes them in order of priority.
func Example_intHeap() {
    h := &IntHeap{2, 1, 5}
    heap.Init(h)
    heap.Push(h, 3)
    fmt.Printf("minimum: %d\n", (*h)[0])
    for h.Len() > 0 {
        fmt.Printf("%d ", heap.Pop(h))
    }
    // Output:
    // minimum: 1
    // 1 2 3 5
}
```


```golang
This example inserts some items into a PriorityQueue, manipulates an item, and then removes the items in priority order.

Code:

// This example demonstrates a priority queue built using the heap interface.
package heap_test

import (
    "container/heap"
    "fmt"
)

// An Item is something we manage in a priority queue.
type Item struct {
    value    string // The value of the item; arbitrary.
    priority int    // The priority of the item in the queue.
    // The index is needed by update and is maintained by the heap.Interface methods.
    index int // The index of the item in the heap.
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    // We want Pop to give us the highest, not lowest, priority so we use greater than here.
    return pq[i].priority > pq[j].priority
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}

func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    item := x.(*Item)
    item.index = n
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    item.index = -1 // for safety
    *pq = old[0 : n-1]
    return item
}

// update modifies the priority and value of an Item in the queue.
func (pq *PriorityQueue) update(item *Item, value string, priority int) {
    heap.Remove(pq, item.index)
    item.value = value
    item.priority = priority
    heap.Push(pq, item)
}

// This example inserts some items into a PriorityQueue, manipulates an item,
// and then removes the items in priority order.
func Example_priorityQueue() {
    // Some items and their priorities.
    items := map[string]int{
        "banana": 3, "apple": 2, "pear": 4,
    }

    // Create a priority queue and put the items in it.
    pq := &PriorityQueue{}
    heap.Init(pq)
    for value, priority := range items {
        item := &Item{
            value:    value,
            priority: priority,
        }
        heap.Push(pq, item)
    }

    // Insert a new item and then modify its priority.
    item := &Item{
        value:    "orange",
        priority: 1,
    }
    heap.Push(pq, item)
    pq.update(item, item.value, 5)

    // Take the items out; they arrive in decreasing priority order.
    for pq.Len() > 0 {
        item := heap.Pop(pq).(*Item)
        fmt.Printf("%.2d:%s ", item.priority, item.value)
    }
    // Output:
    // 05:orange 04:pear 03:banana 02:apple
}
```

```golang
	func Fix(h Interface, i int)
		Fix reestablishes the heap ordering after the element at index i has changed its value. 
		Changing the value of the element at index i and then calling Fix is equivalent to, 
		but less expensive than, calling Remove(h, i) followed by a Push of the new value. 
		The complexity is O(log(n)) where n = h.Len().
		 在改变索引i的值后 重新建立堆排序
		 这样比调用Remove(h, i)然后push一个新值进去的开销小。
		当n=h.Len()的时候，复杂度是O(log(n))
```

```golang
func Init(h Interface)
   A heap must be initialized before any of the heap operations can be used. 
   Init is idempotent with respect to the heap invariants and may be called whenever the heap invariants may have been invalidated. 
   Its complexity is O(n) where n = h.Len(). 
   
   heap必须在任何使用堆操作之前初始化。
   
    时间复杂度 O(n)
```

```golang
func Pop(h Interface) interface{}
  Pop removes the minimum element (according to Less) from the heap and returns it. 
  The complexity is O(log(n)) where n = h.Len(). Same as Remove(h, 0). 
  Pop从heap删除最小元素（根据减）然后返回它。时间复杂度O(log(n))， n = h.Len()。和Remove(h, 0)一样。
```

```golang   
func Push(h Interface, x interface{})
  Push pushes the element x onto the heap. The complexity is O(log(n)) where n = h.Len(). 
  往堆里push元素x。复杂度是 O(log(n)) ， n = h.Len()。
```

```golang
func Remove(h Interface, i int) interface{}
   Remove removes the element at index i from the heap. The complexity is O(log(n)) where n = h.Len(). 
   根据索引i删除堆的元素。复杂是O(log(n)) ， n = h.Len(). 
```


type Interface
  type Interface interface {
          sort.Interface
          Push(x interface{}) // add x as element Len()
          Pop() interface{}   // remove and return element Len() - 1.
  }
  Any type that implements heap.
  Interface may be used as a min-heap with the following invariants (established after Init has been called or if the data is empty or sorted): 
  !h.Less(j, i) for 0 <= i < h.Len() and j = 2*i+1 or 2*i+2 and j < h.Len()
  
  Note that Push and Pop in this interface are for package heap's implementation to call. To add and remove things from the heap, use heap.Push and heap.Pop. 
  
  所有的type都有实现堆
  Interface可以被用来作为下列变量的一个最小堆():
  !h.Less(j, i) for 0 <= i < h.Len() and j = 2*i+1 or 2*i+2 and j < h.Len()
  
  备注:Push 和 Pop  是在heap 包实现调用的。从堆里添加和删除东西 使用  heap.Push 和 heap.Pop.
