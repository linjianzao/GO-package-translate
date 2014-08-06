Package atomic

Overview ▾

Package atomic provides low-level atomic memory primitives useful for implementing synchronization algorithms.

These functions require great care to be used correctly. 
Except for special, low-level applications, synchronization is better done with channels or the facilities of the sync package. Share memory by communicating; 
don't communicate by sharing memory. 

atomic包提供低级 原子内存原语对实现同步算法非常有用。
这些功能需要非常谨慎正确使用。 除非特殊情况 ,低层次的应用， 同步 比 channel 或 sync包 更好.通过交流共享内存; 而不是通过共享内存交流.


The swap operation, implemented by the SwapT functions, is the atomic equivalent of:
交换操作通过SwapT函数实现,原子当量:

```golang
old = *addr
*addr = new
return old
```

The compare-and-swap operation, implemented by the CompareAndSwapT functions, is the atomic equivalent of:
比较并交换操作,通过 CompareAndSwapT 函数实现,原子当量:

```golang
if *addr == old {
	*addr = new
	return true
}
return false
```

The add operation, implemented by the AddT functions, is the atomic equivalent of:
添加 操作. 通过 AddT函数 实现, 原子当量:

```golang
*addr += delta
return *addr
```

The load and store operations, implemented by the LoadT and StoreT functions, are the atomic equivalents of "return *addr" and "*addr = val".
载入和存储操作,通过 LoadT 和 StoreT函数实现, 原子当量"return *addr" and "*addr = val".



func AddInt32
```golang
func AddInt32(addr *int32, delta int32) (new int32)
```
AddInt32 atomically adds delta to *addr and returns the new value.
AddInt32 原子 添加 delta 到 *addr  并返回新值.



func AddInt64
```golang
func AddInt64(addr *int64, delta int64) (new int64)
```
AddInt64 atomically adds delta to *addr and returns the new value.
AddInt64 原子 添加 delta  到 *addr  并返回新值. 



func AddUint32
```golang
func AddUint32(addr *uint32, delta uint32) (new uint32)
```
AddUint32 atomically adds delta to *addr and returns the new value. To subtract a signed positive constant value c from x, do AddUint32(&x, ^uint32(c-1)). 
In particular, to decrement x, do AddUint32(&x, ^uint32(0)).
AddUint32 原子 添加 delta  到 *addr  并返回新值. 为了从 x 减去一个 有符号的正常值 c,做 AddUint32(&x, ^uint32(c-1)). 特别是 递减x,  AddUint32(&x, ^uint32(0)).



func AddUint64
```golang
func AddUint64(addr *uint64, delta uint64) (new uint64)
```
AddUint64 atomically adds delta to *addr and returns the new value.
To subtract a signed positive constant value c from x, do AddUint64(&x, ^uint64(c-1)). In particular, to decrement x, do AddUint64(&x, ^uint64(0)).
AddUint64原子 添加 delta  到 *addr 并返回新值. 
为了从 x 减去一个 有符号的正常值 c,做AddUint64(&x, ^uint64(c-1)).特别是 递减x,  AddUint64(&x, ^uint64(0)).



func AddUintptr
```golang
func AddUintptr(addr *uintptr, delta uintptr) (new uintptr)
```
AddUintptr atomically adds delta to *addr and returns the new value.
AddUintptr 原子 添加 delta  到 *addr 并返回新值. 



func CompareAndSwapInt32
```golang
func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)
```
CompareAndSwapInt32 executes the compare-and-swap operation for an int32 value.
CompareAndSwapInt32 对int32值 执行 比较并交换.



func CompareAndSwapInt64
```golang
func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool)
```
CompareAndSwapInt64 executes the compare-and-swap operation for an int64 value.
CompareAndSwapInt64  对int64值 执行 比较并交换.



func CompareAndSwapPointer
```golang
func CompareAndSwapPointer(addr *unsafe.Pointer, old, new unsafe.Pointer) (swapped bool)
```
CompareAndSwapPointer executes the compare-and-swap operation for a unsafe.Pointer value.
CompareAndSwapPointer  对unsafe.Pointer值 执行 比较并交换.



func CompareAndSwapUint32
```golang
func CompareAndSwapUint32(addr *uint32, old, new uint32) (swapped bool)
```
CompareAndSwapUint32 executes the compare-and-swap operation for a uint32 value.
CompareAndSwapUint32 对uint32值 执行 比较并交换.



func CompareAndSwapUint64
```golang
func CompareAndSwapUint64(addr *uint64, old, new uint64) (swapped bool)
```
CompareAndSwapUint64 executes the compare-and-swap operation for a uint64 value.
CompareAndSwapUint64 对uint64值 执行 比较并交换.



func CompareAndSwapUintptr
```golang
func CompareAndSwapUintptr(addr *uintptr, old, new uintptr) (swapped bool)
```
CompareAndSwapUintptr executes the compare-and-swap operation for a uintptr value.
CompareAndSwapUintptr 对uintptr值 执行 比较并交换.



func LoadInt32
```golang
func LoadInt32(addr *int32) (val int32)
```
LoadInt32 atomically loads *addr.
LoadInt32原子 载入 *addr.



func LoadInt64
```golang
func LoadInt64(addr *int64) (val int64)
```
LoadInt64 atomically loads *addr.
LoadInt64原子 载入 *addr.



func LoadPointer
```golang
func LoadPointer(addr *unsafe.Pointer) (val unsafe.Pointer)
```
LoadPointer atomically loads *addr.
LoadPointer原子 载入 *addr.



func LoadUint32
```golang
func LoadUint32(addr *uint32) (val uint32)
```
LoadUint32 atomically loads *addr.
LoadUint32原子 载入 *addr.



func LoadUint64
```golang
func LoadUint64(addr *uint64) (val uint64)
```
LoadUint64 atomically loads *addr.
LoadUint64原子 载入 *addr.



func LoadUintptr
```golang
func LoadUintptr(addr *uintptr) (val uintptr)
```
LoadUintptr atomically loads *addr.
LoadUintptr原子 载入 *addr.



func StoreInt32
```golang
func StoreInt32(addr *int32, val int32)
```
StoreInt32 atomically stores val into *addr.
StoreInt32 原子 存储 val 进  *addr.



func StoreInt64
```golang
func StoreInt64(addr *int64, val int64)
```
StoreInt64 atomically stores val into *addr.
StoreInt64 原子 存储 val 进  *addr.



func StorePointer
```golang
func StorePointer(addr *unsafe.Pointer, val unsafe.Pointer)
```
StorePointer atomically stores val into *addr.
StorePointer 原子 存储 val 进  *addr.


func StoreUint32
```golang
func StoreUint32(addr *uint32, val uint32)
```
StoreUint32 atomically stores val into *addr.
StoreUint32 原子 存储 val 进  *addr.



func StoreUint64
```golang
func StoreUint64(addr *uint64, val uint64)
```
StoreUint64 atomically stores val into *addr.
StoreUint64 原子 存储 val 进  *addr.



func StoreUintptr
```golang
func StoreUintptr(addr *uintptr, val uintptr)
```
StoreUintptr atomically stores val into *addr.
StoreUintptr原子 存储 val 进  *addr.



func SwapInt32
```golang
func SwapInt32(addr *int32, new int32) (old int32)
``
SwapInt32 atomically stores new into *addr and returns the previous *addr value.
StoreUintptr原子 存储 new 进  *addr 并返回 前一个 *addr 值.



func SwapInt64
```golang
func SwapInt64(addr *int64, new int64) (old int64)
```
SwapInt64 atomically stores new into *addr and returns the previous *addr value.
SwapInt64原子 存储 new 进  *addr 并返回 前一个 *addr 值.



func SwapPointer
```golang
func SwapPointer(addr *unsafe.Pointer, new unsafe.Pointer) (old unsafe.Pointer)
```
SwapPointer atomically stores new into *addr and returns the previous *addr value.
SwapPointer原子 存储 new 进  *addr 并返回 前一个 *addr 值.



func SwapUint32
```golang
func SwapUint32(addr *uint32, new uint32) (old uint32)
```
SwapUint32 atomically stores new into *addr and returns the previous *addr value.
SwapUint32原子 存储 new 进  *addr 并返回 前一个 *addr 值.



func SwapUint64
```golang
func SwapUint64(addr *uint64, new uint64) (old uint64)
```
SwapUint64 atomically stores new into *addr and returns the previous *addr value.
SwapUint64原子 存储 new 进  *addr 并返回 前一个 *addr 值.



func SwapUintptr
```golang
func SwapUintptr(addr *uintptr, new uintptr) (old uintptr)
```
SwapUintptr atomically stores new into *addr and returns the previous *addr value.
SwapUintptr 原子 存储 new 进  *addr 并返回 前一个 *addr 值.



