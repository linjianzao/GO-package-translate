Package sync

Overview ▾

Package sync provides basic synchronization primitives such as mutual exclusion locks. 
Other than the Once and WaitGroup types, most are intended for use by low-level library routines. Higher-level synchronization is better done via channels and communication.

Values containing the types defined in this package should not be copied.

sync 提供基本的同步原语 如互斥锁。除了 Once 和 WaitGroup 类型, 大部分 使用在低级的库例程.高级同步 通过 channel 和渠道做得更好.

包含在这个包中定义的类型的值不应该被复制。


type Cond
```golang
type Cond struct {
        // L is held while observing or changing the condition
        L Locker
        // contains filtered or unexported fields
}
```
Cond implements a condition variable, a rendezvous point for goroutines waiting for or announcing the occurrence of an event.

Each Cond has an associated Locker L (often a *Mutex or *RWMutex), which must be held when changing the condition and when calling the Wait method.

A Cond can be created as part of other structures. A Cond must not be copied after first use.
Cond实现 一个条件变量,交会点Go例程 等待 或  声明  事件的发生。

每个 Cond 对应 Locker L (经常 *Mutex or *RWMutex), 当修改条件或调用 Wait 方法时 必须保留.

Cond可被创建为其它结构的一部分。 Cond 在第一次使用后 不能被复制.



func NewCond
```golang
func NewCond(l Locker) *Cond
```
NewCond returns a new Cond with Locker l.
NewCond 根据  Locker l 返回一个新的 Cond



func (*Cond) Broadcast
```golang
func (c *Cond) Broadcast()
```
Broadcast wakes all goroutines waiting on c.

It is allowed but not required for the caller to hold c.L during the call.

Broadcast唤起所有 在c上 等待的例程.
它在调用的期间 允许 但不需要 调用者 保留 c.L.



func (*Cond) Signal
```golang
func (c *Cond) Signal()
```
Signal wakes one goroutine waiting on c, if there is any.

It is allowed but not required for the caller to hold c.L during the call.

Signal唤起在c上等待的一个例程, 如果有的话.

它在调用的期间 允许 但不需要 调用者 保留 c.L.



func (*Cond) Wait
```golang
func (c *Cond) Wait()
```
Wait atomically unlocks c.L and suspends execution of the calling goroutine. 
After later resuming execution, Wait locks c.L before returning. Unlike in other systems, Wait cannot return unless awoken by Broadcast or Signal.

Because c.L is not locked when Wait first resumes, the caller typically cannot assume that the condition is true when Wait returns. Instead, the caller should Wait in a loop:

Wait自动解锁 c.L 并挂起执行调用的例程.在之后恢复执行 ,Wait 在返回之前 锁住 c.L . 不像在其他操作系统, Wait不会返回 ,除非 通过 Broadcast 或 Signal 唤起.

因为当 Wait第一次恢复的时候c.L不会锁住, 当Wait返回时 调用者通常不能 假设 条件是true. 代替的, 调用者应该 在一个循环 Wait:

```golang
c.L.Lock()
for !condition() {
    c.Wait()
}
... make use of condition ...
c.L.Unlock()
```


type Locker
```golang
type Locker interface {
        Lock()
        Unlock()
}
```
A Locker represents an object that can be locked and unlocked.
Locker 表示 一个可以 锁和解锁的对象.



type Mutex
```golang
type Mutex struct {
        // contains filtered or unexported fields
}
```
A Mutex is a mutual exclusion lock. Mutexes can be created as part of other structures; the zero value for a Mutex is an unlocked mutex.
Mutex互斥.Mutexes 可被创建为其它结构的一部分; Mutex 的零值 时解锁互斥。



func (*Mutex) Lock
```golang
func (m *Mutex) Lock()
```
Lock locks m. If the lock is already in use, the calling goroutine blocks until the mutex is available.
Lock锁m. 如果该锁已经在使用, 阻塞调用的例程 直到 互斥  可用.



func (*Mutex) Unlock
```golang
func (m *Mutex) Unlock()
```
Unlock unlocks m. It is a run-time error if m is not locked on entry to Unlock.

A locked Mutex is not associated with a particular goroutine. It is allowed for one goroutine to lock a Mutex and then arrange for another goroutine to unlock it.

Unlock 解锁 m. 如果m没有锁定 再进入解锁,运行时会错误.

一个锁定的 Mutex 不是与特定的goroutine相关.它允许 一个例程 锁定一个Mutex 然后 安排其他例程 解锁它.




type Once
```golang
type Once struct {
        // contains filtered or unexported fields
}
```
Once is an object that will perform exactly one action.
Once 是执行只有一个动作的对象.

▹ Example

```golang
package main

import (
	"fmt"
	"sync"
)

func main() {
	var once sync.Once
	onceBody := func() {
		fmt.Println("Only once")
	}
	done := make(chan bool)
	for i := 0; i < 10; i++ {
		go func() {
			once.Do(onceBody)
			done <- true
		}()
	}
	for i := 0; i < 10; i++ {
		<-done
	}
}
```



func (*Once) Do
```golang
func (o *Once) Do(f func())
```
Do calls the function f if and only if Do is being called for the first time for this instance of Once. In other words, given
当且仅当你被要求在第一时间 调用Once实例  Do 调用函数f.换句话说,给

```golang
var once Once
```

if once.Do(f) is called multiple times, only the first call will invoke f, even if f has a different value in each invocation. 
A new instance of Once is required for each function to execute.

Do is intended for initialization that must be run exactly once. 
Since f is niladic, it may be necessary to use a function literal to capture the arguments to a function to be invoked by Do:

如果 once.Do(f)  调用多次, 只有第一次 会调用f,即使每次调用f的是不同的值. 一个新的Once 实例 必须每次函数执行.

Do适用于初始化 必须 刚好执行一次. 因为f是译注， 它可能必须 使用函数文本来 捕捉函数的参数 让 Do 调用:

```golang
config.once.Do(func() { config.init(filename) })
```
Because no call to Do returns until the one call to f returns, if f causes Do to be called, it will deadlock.
因为没有调用 Do 返回 直到调用一次 让 f返回,如果f 导致 Do 被调用, 它会死锁。



type Pool
```golang
type Pool struct {

        // New optionally specifies a function to generate
        // a value when Get would otherwise return nil.
        // It may not be changed concurrently with calls to Get.
        //当Get时 New 选项 指定一个函数来生成一个值, 否则返回nil.
        //调用Get 不会同时修改
        New func() interface{}
        // contains filtered or unexported fields
}
```
A Pool is a set of temporary objects that may be individually saved and retrieved.

Any item stored in the Pool may be removed automatically at any time without notification. 
If the Pool holds the only reference when this happens, the item might be deallocated.

A Pool is safe for use by multiple goroutines simultaneously.

Pool's purpose is to cache allocated but unused items for later reuse, relieving pressure on the garbage collector. 
That is, it makes it easy to build efficient, thread-safe free lists. However, it is not suitable for all free lists.

An appropriate use of a Pool is to manage a group of temporary items silently shared among and potentially reused by concurrent independent clients of a package. 
Pool provides a way to amortize allocation overhead across many clients.

An example of good use of a Pool is in the fmt package, which maintains a dynamically-sized store of temporary output buffers. 
The store scales under load (when many goroutines are actively printing) and shrinks when quiescent.

On the other hand, a free list maintained as part of a short-lived object is not a suitable use for a Pool, since the overhead does not amortize well in that scenario.
It is more efficient to have such objects implement their own free list.
Pool是一个  可以单独地保存和检索 临时对象集。

任何存储在 Pool 里的项 可能在 任何时间 没有通知下 自动删除. 当它发生的时候如果Pool 只保留引用, 该项可能被释放.

Pool 多例程同时调用是安全的

Pool的目的 是用来缓存分配 以备 后面重用, 减轻对垃圾回收的压力。这就是说,它可以容易的高效的构建, 线程安全白名单.  然而它不适用所有的 列表.

适当的使用 Pool 来管理 一组 临时项之间的共享 和并发独立客户端通过潜在的重用一个包。

一个好的使用Pool例子 是在fmt 包里, 它保持了 一个动态储存临时输出 缓冲区 .
根据负载扩展(当许多例程 激活打印) 和收缩存储

另一方面，保持一个短暂对象的一部分空闲列表 是不适合使用Pool的, 因为在那种情况下 开销 缓冲不会良好.这是更有效的实现自己的空闲列表的对象。



func (*Pool) Get
```golang
func (p *Pool) Get() interface{}
```
Get selects an arbitrary item from the Pool, removes it from the Pool, and returns it to the caller. 
Get may choose to ignore the pool and treat it as empty. Callers should not assume any relation between values passed to Put and the values returned by Get.

If Get would otherwise return nil and p.New is non-nil, Get returns the result of calling p.New.

Get从 Pool 选择任意项, 从 Pool 移除它, 并 返回给调用者.Get可能选择忽略 pool  并把它当空. 调用者不应该假设 传给Put的值 和 返回给 Get的值 有任何关系.

如果 Get, 否则 返回nil 并且 p.New 是非nil,Get 返回 调用  p.New的结果.



func (*Pool) Put
```golang
func (p *Pool) Put(x interface{})
```
Put adds x to the pool.
Put 添加x 到pool



type RWMutex
```golang
type RWMutex struct {
        // contains filtered or unexported fields
}
```
An RWMutex is a reader/writer mutual exclusion lock. The lock can be held by an arbitrary number of readers or a single writer. 
RWMutexes can be created as part of other structures; the zero value for a RWMutex is an unlocked mutex.
RWMutex是一个  读写 互斥锁.  该锁 可以由 任意数量读取者 或 当个写入者 持有.
RWMutexes 可以创建成其他结构的一部分;RWMutex的零值 是 解锁互斥。



func (*RWMutex) Lock
```golang
func (rw *RWMutex) Lock()
```
Lock locks rw for writing. If the lock is already locked for reading or writing, Lock blocks until the lock is available. 
To ensure that the lock eventually becomes available, a blocked Lock call excludes new readers from acquiring the lock.
Lock 锁rw 写入.如果 该锁已经由其他读或写 锁了,Lock 阻塞直到该锁可用.
为了确保锁最终变得可用, 被锁定的锁调用不包含 新的读 获取的锁.



func (*RWMutex) RLock
```golang
func (rw *RWMutex) RLock()
```
RLock locks rw for reading.
RLock 锁定 rw 读



func (*RWMutex) RLocker
```golang
func (rw *RWMutex) RLocker() Locker
```
RLocker returns a Locker interface that implements the Lock and Unlock methods by calling rw.RLock and rw.RUnlock.
RLocker 返回一个 Locker 接口 通过调用 rw.RLock 和 rw.RUnlock  实现Lock 和 Unlock方法.



func (*RWMutex) RUnlock
```golang
func (rw *RWMutex) RUnlock()
```
RUnlock undoes a single RLock call; it does not affect other simultaneous readers. It is a run-time error if rw is not locked for reading on entry to RUnlock.
RUnlock 撤消 一个RLock调用;它不会影响 其他 同时 读取者. 如果rw未锁定 ,执行时读取进入RUnlock会出错.



func (*RWMutex) Unlock
```golang
func (rw *RWMutex) Unlock()
```
Unlock unlocks rw for writing. It is a run-time error if rw is not locked for writing on entry to Unlock.

As with Mutexes, a locked RWMutex is not associated with a particular goroutine. 
One goroutine may RLock (Lock) an RWMutex and then arrange for another goroutine to RUnlock (Unlock) it.
Unlock 解锁 rw 写. 如果rw 没有锁定, 运行时进入Unlock 会出错.

与Mutexes , 锁定 RWMutex 不会与特定的例程 有关.一个例程 可能 RLock (Lock)  一个 RWMutex 然后 安排其他的 例程RUnlock (Unlock) 它.



type WaitGroup
```golang
type WaitGroup struct {
        // contains filtered or unexported fields
}
```
A WaitGroup waits for a collection of goroutines to finish. The main goroutine calls Add to set the number of goroutines to wait for. 
Then each of the goroutines runs and calls Done when finished. At the same time, Wait can be used to block until all goroutines have finished.
WaitGroup 等待 一个 例程 集结束. 主要例程 调用 Add 来设置等待的例程数.
当结束时每个例程执行和调用Done. 同时, Wait 阻塞 直到所有的 例程 结束.



▹ Example

This example fetches several URLs concurrently, using a WaitGroup to block until all the fetches are complete.
这个例子 同时获取多个URL, 使用 WaitGroup 阻塞 直到 所有 获取完成.
Code:
```golang
    var wg sync.WaitGroup
    var urls = []string{
            "http://www.golang.org/",
            "http://www.google.com/",
            "http://www.somestupidname.com/",
    }
    for _, url := range urls {
            // Increment the WaitGroup counter.
            wg.Add(1)
            // Launch a goroutine to fetch the URL.
            go func(url string) {
                    // Decrement the counter when the goroutine completes.
                    defer wg.Done()
                    // Fetch the URL.
                    http.Get(url)
            }(url)
    }
    // Wait for all HTTP fetches to complete.
    wg.Wait()

```
    
    
    
func (*WaitGroup) Add
```golang
func (wg *WaitGroup) Add(delta int)
```
Add adds delta, which may be negative, to the WaitGroup counter. If the counter becomes zero, all goroutines blocked on Wait are released. 
If the counter goes negative, Add panics.

Note that calls with positive delta must happen before the call to Wait, or else Wait may wait for too small a group. 
Typically this means the calls to Add should execute before the statement creating the goroutine or other event to be waited for. See the WaitGroup example.

Add添加 delta 到WaitGroup 计数器 ,可能是负的.  如果计数器变成0,阻塞Wait所有Go例程被释放。 如果计数器是负的,Add 引发panic

注: 以正 delta调用  必须 在 调用 Wait 之前,或 其他Wait等待太小的组.
通常这意味着 调用 Add 应该 在 声明 创建例程 或等待其他事件之前 执行.查看WaitGroup例子.


func (*WaitGroup) Done
```golang
func (wg *WaitGroup) Done()
```
Done decrements the WaitGroup counter.
Done递减WaitGroup计数器.



func (*WaitGroup) Wait
```golang
func (wg *WaitGroup) Wait()
```
Wait blocks until the WaitGroup counter is zero.    
Wait 阻塞直到WaitGroup计数器是零
    
    
