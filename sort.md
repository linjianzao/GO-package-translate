Package sort

Overview ▾

Package sort provides primitives for sorting slices and user-defined collections.
sort 包提供用于排序的slice和用户定义的集合的原语


▾ Example
```golang
package main

import (
	"fmt"
	"sort"
)

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%s: %d", p.Name, p.Age)
}

// ByAge implements sort.Interface for []Person based on
// the Age field.
type ByAge []Person

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

func main() {
	people := []Person{
		{"Bob", 31},
		{"John", 42},
		{"Michael", 17},
		{"Jenny", 26},
	}

	fmt.Println(people)
	sort.Sort(ByAge(people))
	fmt.Println(people)

}
```


▾ Example (SortKeys)
ExampleSortKeys demonstrates a technique for sorting a struct type using programmable sort criteria.
ExampleSortKeys 演示 使用可编程的排序条件 排序一个struct 技术.

```golang
package main

import (
	"fmt"
	"sort"
)

// A couple of type definitions to make the units clear.
type earthMass float64
type au float64

// A Planet defines the properties of a solar system object.
type Planet struct {
	name     string
	mass     earthMass
	distance au
}

// By is the type of a "less" function that defines the ordering of its Planet arguments.
type By func(p1, p2 *Planet) bool

// Sort is a method on the function type, By, that sorts the argument slice according to the function.
func (by By) Sort(planets []Planet) {
	ps := &planetSorter{
		planets: planets,
		by:      by, // The Sort method's receiver is the function (closure) that defines the sort order.
	}
	sort.Sort(ps)
}

// planetSorter joins a By function and a slice of Planets to be sorted.
type planetSorter struct {
	planets []Planet
	by      func(p1, p2 *Planet) bool // Closure used in the Less method.
}

// Len is part of sort.Interface.
func (s *planetSorter) Len() int {
	return len(s.planets)
}

// Swap is part of sort.Interface.
func (s *planetSorter) Swap(i, j int) {
	s.planets[i], s.planets[j] = s.planets[j], s.planets[i]
}

// Less is part of sort.Interface. It is implemented by calling the "by" closure in the sorter.
func (s *planetSorter) Less(i, j int) bool {
	return s.by(&s.planets[i], &s.planets[j])
}

var planets = []Planet{
	{"Mercury", 0.055, 0.4},
	{"Venus", 0.815, 0.7},
	{"Earth", 1.0, 1.0},
	{"Mars", 0.107, 1.5},
}

// ExampleSortKeys demonstrates a technique for sorting a struct type using programmable sort criteria.
func main() {
	// Closures that order the Planet structure.
	name := func(p1, p2 *Planet) bool {
		return p1.name < p2.name
	}
	mass := func(p1, p2 *Planet) bool {
		return p1.mass < p2.mass
	}
	distance := func(p1, p2 *Planet) bool {
		return p1.distance < p2.distance
	}
	decreasingDistance := func(p1, p2 *Planet) bool {
		return !distance(p1, p2)
	}

	// Sort the planets by the various criteria.
	By(name).Sort(planets)
	fmt.Println("By name:", planets)

	By(mass).Sort(planets)
	fmt.Println("By mass:", planets)

	By(distance).Sort(planets)
	fmt.Println("By distance:", planets)

	By(decreasingDistance).Sort(planets)
	fmt.Println("By decreasing distance:", planets)

}
```



▹ Example (SortMultiKeys)

ExampleMultiKeys demonstrates a technique for sorting a struct type using different sets of multiple fields in the comparison.
We chain together "Less" functions, each of which compares a single field.
ExampleMultiKeys演示 使用不同的 多字段集,排序一个struct类型  的技巧
我们 链  "Less" 函数 在一起, 每个比较单个字段.

```golang
package main

import (
	"fmt"
	"sort"
)

// A Change is a record of source code changes, recording user, language, and delta size.
type Change struct {
	user     string
	language string
	lines    int
}

type lessFunc func(p1, p2 *Change) bool

// multiSorter implements the Sort interface, sorting the changes within.
type multiSorter struct {
	changes []Change
	less    []lessFunc
}

// Sort sorts the argument slice according to the less functions passed to OrderedBy.
func (ms *multiSorter) Sort(changes []Change) {
	ms.changes = changes
	sort.Sort(ms)
}

// OrderedBy returns a Sorter that sorts using the less functions, in order.
// Call its Sort method to sort the data.
func OrderedBy(less ...lessFunc) *multiSorter {
	return &multiSorter{
		less: less,
	}
}

// Len is part of sort.Interface.
func (ms *multiSorter) Len() int {
	return len(ms.changes)
}

// Swap is part of sort.Interface.
func (ms *multiSorter) Swap(i, j int) {
	ms.changes[i], ms.changes[j] = ms.changes[j], ms.changes[i]
}

// Less is part of sort.Interface. It is implemented by looping along the
// less functions until it finds a comparison that is either Less or
// !Less. Note that it can call the less functions twice per call. We
// could change the functions to return -1, 0, 1 and reduce the
// number of calls for greater efficiency: an exercise for the reader.
func (ms *multiSorter) Less(i, j int) bool {
	p, q := &ms.changes[i], &ms.changes[j]
	// Try all but the last comparison.
	var k int
	for k = 0; k < len(ms.less)-1; k++ {
		less := ms.less[k]
		switch {
		case less(p, q):
			// p < q, so we have a decision.
			return true
		case less(q, p):
			// p > q, so we have a decision.
			return false
		}
		// p == q; try the next comparison.
	}
	// All comparisons to here said "equal", so just return whatever
	// the final comparison reports.
	return ms.less[k](p, q)
}

var changes = []Change{
	{"gri", "Go", 100},
	{"ken", "C", 150},
	{"glenda", "Go", 200},
	{"rsc", "Go", 200},
	{"r", "Go", 100},
	{"ken", "Go", 200},
	{"dmr", "C", 100},
	{"r", "C", 150},
	{"gri", "Smalltalk", 80},
}

// ExampleMultiKeys demonstrates a technique for sorting a struct type using different
// sets of multiple fields in the comparison. We chain together "Less" functions, each of
// which compares a single field.
func main() {
	// Closures that order the Change structure.
	user := func(c1, c2 *Change) bool {
		return c1.user < c2.user
	}
	language := func(c1, c2 *Change) bool {
		return c1.language < c2.language
	}
	increasingLines := func(c1, c2 *Change) bool {
		return c1.lines < c2.lines
	}
	decreasingLines := func(c1, c2 *Change) bool {
		return c1.lines > c2.lines // Note: > orders downwards.
	}

	// Simple use: Sort by user.
	OrderedBy(user).Sort(changes)
	fmt.Println("By user:", changes)

	// More examples.
	OrderedBy(user, increasingLines).Sort(changes)
	fmt.Println("By user,<lines:", changes)

	OrderedBy(user, decreasingLines).Sort(changes)
	fmt.Println("By user,>lines:", changes)

	OrderedBy(language, increasingLines).Sort(changes)
	fmt.Println("By language,<lines:", changes)

	OrderedBy(language, increasingLines, user).Sort(changes)
	fmt.Println("By language,<lines,user:", changes)

}
```




▹ Example (SortWrapper)

```golang
package main

import (
	"fmt"
	"sort"
)

type Grams int

func (g Grams) String() string { return fmt.Sprintf("%dg", int(g)) }

type Organ struct {
	Name   string
	Weight Grams
}

type Organs []*Organ

func (s Organs) Len() int      { return len(s) }
func (s Organs) Swap(i, j int) { s[i], s[j] = s[j], s[i] }

// ByName implements sort.Interface by providing Less and using the Len and
// Swap methods of the embedded Organs value.
type ByName struct{ Organs }

func (s ByName) Less(i, j int) bool { return s.Organs[i].Name < s.Organs[j].Name }

// ByWeight implements sort.Interface by providing Less and using the Len and
// Swap methods of the embedded Organs value.
type ByWeight struct{ Organs }

func (s ByWeight) Less(i, j int) bool { return s.Organs[i].Weight < s.Organs[j].Weight }

func main() {
	s := []*Organ{
		{"brain", 1340},
		{"heart", 290},
		{"liver", 1494},
		{"pancreas", 131},
		{"prostate", 62},
		{"spleen", 162},
	}

	sort.Sort(ByWeight{s})
	fmt.Println("Organs by weight:")
	printOrgans(s)

	sort.Sort(ByName{s})
	fmt.Println("Organs by name:")
	printOrgans(s)

}

func printOrgans(s []*Organ) {
	for _, o := range s {
		fmt.Printf("%-8s (%v)\n", o.Name, o.Weight)
	}
}
```


func Float64s
```golang
func Float64s(a []float64)
```
Float64s sorts a slice of float64s in increasing order.
Float64s 以递增的顺序 排序 float64s slice.



func Float64sAreSorted
```golang
func Float64sAreSorted(a []float64) bool
```
Float64sAreSorted tests whether a slice of float64s is sorted in increasing order.
Float64sAreSorted 测试  一个float64s是否以递增的顺序排序



func Ints
```golang
func Ints(a []int)
```
Ints sorts a slice of ints in increasing order.
Ints 以递增的顺序排序ints slice


▾ Example
```golang
package main

import (
	"fmt"
	"sort"
)

func main() {
	s := []int{5, 2, 6, 3, 1, 4} // unsorted
	sort.Ints(s)
	fmt.Println(s)
}
```


func IntsAreSorted
```golang
func IntsAreSorted(a []int) bool
```
IntsAreSorted tests whether a slice of ints is sorted in increasing order.
IntsAreSorted 测试 ints slice 是否以递增的顺序排序



func IsSorted
```golang
func IsSorted(data Interface) bool
```
IsSorted reports whether data is sorted.
IsSorted 报告 data是否排序.



func Search
```golang
func Search(n int, f func(int) bool) int
```
Search uses binary search to find and return the smallest index i in [0, n) at which f(i) is true, assuming that on the range [0, n), f(i) == true implies f(i+1) == true. 
That is, Search requires that f is false for some (possibly empty) prefix of the input range [0, n) and then true for the (possibly empty) remainder; 
Search returns the first true index. If there is no such index, Search returns n. (Note that the "not found" return value is not -1 as in, for instance, strings.Index). 
Search calls f(i) only for i in the range [0, n).

A common use of Search is to find the index i for a value x in a sorted, indexable data structure such as an array or slice. 
In this case, the argument f, typically a closure, captures the value to be searched for, and how the data structure is indexed and ordered.

For instance, given a slice data sorted in ascending order, the call Search(len(data), func(i int) bool { return data[i] >= 23 }) returns the smallest index i such that data[i] >= 23. 
If the caller wants to find whether 23 is in the slice, it must test data[i] == 23 separately.

Searching data sorted in descending order would use the <= operator instead of the >= operator.

Search使用二进制 搜索 来查找和返回最小的索引i [0, n) 其中 f(i)  是true,假设 在范围[0, n) 里,f(i) == true 意味着  f(i+1) == true. 
那就是说,Search需要  一些前缀 (可能是空的) 输入范围是 [0, n) f是 false ,其余的是true(可能是空);
Search返回第一个true的索引. 如果没有这个索引,Search 返回n.(注:"未发现" 返回值不是-1, 例如,strings.Index ).
Search 只 i在 范围[0, n) 调用 f(i).

Search的常见的用途 是 在排序里 查找 值x 的索引 i,可索引的数据结构 如 array 或 slice.
在这个情况下, 参数f, 通常是封闭的, 捕获被搜索的值, 数据结构怎么索引和排序.

例如,给一个 以升序排序的slice 数据, 调用Search(len(data), func(i int) bool { return data[i] >= 23 })  返回最小的索引i 如data[i] >= 23.
如果调用者想发现 23是否在slice里, 它必须分别测试data[i] == 23.

查询 以降序排序的数据将使用 <= 操作符 替代 >= 操作符



To complete the example above, the following code tries to find the value x in an integer slice data sorted in ascending order:
完成上面的例子, 下面的代码尝试 在一个升序排序的 整型slice 数据中查找 值x:
```golang
x := 23
i := sort.Search(len(data), func(i int) bool { return data[i] >= x })
if i < len(data) && data[i] == x {
	// x is present at data[i]
} else {
	// x is not present in data,
	// but i is the index where it would be inserted.
}
```

As a more whimsical example, this program guesses your number:
一个更异想天开的例子,这个程序猜测你的数:
```golang
func GuessingGame() {
	var s string
	fmt.Printf("Pick an integer from 0 to 100.\n")
	answer := sort.Search(100, func(i int) bool {
		fmt.Printf("Is your number <= %d? ", i)
		fmt.Scanf("%s", &s)
		return s != "" && s[0] == 'y'
	})
	fmt.Printf("Your number is %d.\n", answer)
}
```


func SearchFloat64s
```golang
func SearchFloat64s(a []float64, x float64) int
```
SearchFloat64s searches for x in a sorted slice of float64s and returns the index as specified by Search. 
The return value is the index to insert x if x is not present (it could be len(a)). The slice must be sorted in ascending order.
SearchFloat64s 在一个排序的float64s slice 中查找x并返回Search指定的索引.
返回值是x插入的索引 如果x不出现(可以是 len(a)).slice 必须以升序排序.



func SearchInts
```golang
func SearchInts(a []int, x int) int
SearchInts searches for x in a sorted slice of ints and returns the index as specified by Search. 
The return value is the index to insert x if x is not present (it could be len(a)). The slice must be sorted in ascending order.
```
SearchInts 在一个排序的ints slice 中查找x并返回Search指定的索引.
返回值是x插入的索引 如果x不出现(可以是 len(a)).slice 必须以升序排序.



func SearchStrings
```golang
func SearchStrings(a []string, x string) int
```
SearchStrings searches for x in a sorted slice of strings and returns the index as specified by Search. 
The return value is the index to insert x if x is not present (it could be len(a)). The slice must be sorted in ascending order.
SearchStrings 在一个排序的strings slice 中查找x并返回Search指定的索引.
返回值是x插入的索引 如果x不出现(可以是 len(a)).slice 必须以升序排序.



func Sort
```golang
func Sort(data Interface)
```
Sort sorts data. It makes one call to data.Len to determine n, and O(n*log(n)) calls to data.Less and data.Swap. The sort is not guaranteed to be stable.
Sort 排序data. 它调用一次data.Len来确定n, O(n*log(n))  调用 data.Less 和 data.Swap.排序不保证是稳定的。



func Stable
```golang
func Stable(data Interface)
```
Stable sorts data while keeping the original order of equal elements.

It makes one call to data.Len to determine n, O(n*log(n)) calls to data.Less and O(n*log(n)*log(n)) calls to data.Swap.
Stable 保持 相等的元素的原始顺序 的data.

它调用一次data.Len来确定n, O(n*log(n)) 调用data.Less ,O(n*log(n)*log(n))调用data.Swap.



func Strings
```golang
func Strings(a []string)
```
Strings sorts a slice of strings in increasing order.
Strings 以递增的顺序的排序 strings



func StringsAreSorted
```golang
func StringsAreSorted(a []string) bool
```
StringsAreSorted tests whether a slice of strings is sorted in increasing order.
StringsAreSorted测试strings是否是 以递增的顺序排序 



type Float64Slice
```golang
type Float64Slice []float64
```
Float64Slice attaches the methods of Interface to []float64, sorting in increasing order.
Float64Slice 附加方法接口到 []float64,以递增的顺序排序 



func (Float64Slice) Len
```golang
func (p Float64Slice) Len() int
```



func (Float64Slice) Less
```golang
func (p Float64Slice) Less(i, j int) bool
```



func (Float64Slice) Search
```golang
func (p Float64Slice) Search(x float64) int
```
Search returns the result of applying SearchFloat64s to the receiver and x.
Search 返回 运行SearchFloat64s的结果到接收器和x.



func (Float64Slice) Sort
```golang
func (p Float64Slice) Sort()
```
Sort is a convenience method.
Sort是一个方便的方法。



func (Float64Slice) Swap
```golang
func (p Float64Slice) Swap(i, j int)
```



type IntSlice
```golang
type IntSlice []int
```
IntSlice attaches the methods of Interface to []int, sorting in increasing order.
IntSlice 附加方法接口到 []int,以递增的顺序排序 .



func (IntSlice) Len
```golang
func (p IntSlice) Len() int
```



func (IntSlice) Less
```golang
func (p IntSlice) Less(i, j int) bool
```



func (IntSlice) Search
```golang
func (p IntSlice) Search(x int) int
```
Search returns the result of applying SearchInts to the receiver and x.
Search 返回运行SearchInts的结果到接收器和x.



func (IntSlice) Sort
```golang
func (p IntSlice) Sort()
```
Sort is a convenience method.
Sort是一个方便的方法。



func (IntSlice) Swap
```golang
func (p IntSlice) Swap(i, j int)
```



type Interface
```golang
type Interface interface {
        // Len is the number of elements in the collection.
        Len() int
        // Less reports whether the element with
        // index i should sort before the element with index j.
        Less(i, j int) bool
        // Swap swaps the elements with indexes i and j.
        Swap(i, j int)
}
```
A type, typically a collection, that satisfies sort.Interface can be sorted by the routines in this package. 
The methods require that the elements of the collection be enumerated by an integer index.
type, 通常是一个集合, 满足sort.Interface 在这个包里 可以通过例程排序.
该方法需要集合元素通过整型索引枚举



func Reverse
```golang
func Reverse(data Interface) Interface
```
Reverse returns the reverse order for data.
Reverse 返回data相反的顺序


▾ Example
```golang
package main

import (
	"fmt"
	"sort"
)

func main() {
	s := []int{5, 2, 6, 3, 1, 4} // unsorted
	sort.Sort(sort.Reverse(sort.IntSlice(s)))
	fmt.Println(s)
}
```

type StringSlice
```golang
type StringSlice []string
```
StringSlice attaches the methods of Interface to []string, sorting in increasing order.
StringSlice 附加到方法接口  []string, 以递增顺序排序


func (StringSlice) Len
```golang
func (p StringSlice) Len() int
```



func (StringSlice) Less
```golang
func (p StringSlice) Less(i, j int) bool
```



func (StringSlice) Search
```golang
func (p StringSlice) Search(x string) int
```
Search returns the result of applying SearchStrings to the receiver and x.
Search 返回执行SearchStrings的结果到 接收器和x.



func (StringSlice) Sort
```golang
func (p StringSlice) Sort()
```
Sort is a convenience method.
Sort 是一个方便的方法.



func (StringSlice) Swap
```golang
func (p StringSlice) Swap(i, j int)
```





