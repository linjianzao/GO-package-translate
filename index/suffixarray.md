Package suffixarray

Overview ▾

Package suffixarray implements substring search in logarithmic time using an in-memory suffix array.
实现了用一个内存中的后缀数组中的对数时间字符串搜索。

Example use:

// create index for some data
index := suffixarray.New(data)

// lookup byte slice s
offsets1 := index.Lookup(s, -1) // the list of all indices where s occurs in data
offsets2 := index.Lookup(s, 3)  // the list of at most 3 indices where s occurs in data



type Index

type Index struct {
        // contains filtered or unexported fields
}
Index implements a suffix array for fast substring search.
实现一个后缀数组的最快子字符串搜索


func New

func New(data []byte) *Index
New creates a new Index for data. Index creation time is O(N*log(N)) for N = len(data).
创建一个新的 索引数据.索引创建的 时间复杂度 是 O(N*log(N)) ,N = len(data).


func (*Index) Bytes

func (x *Index) Bytes() []byte
Bytes returns the data over which the index was created. It must not be modified.
返回索引建立在其上的数据.它必须没有被修改

func (*Index) FindAllIndex

func (x *Index) FindAllIndex(r *regexp.Regexp, n int) (result [][]int)
FindAllIndex returns a sorted list of non-overlapping matches of the regular expression r, where a match is a pair of indices specifying the matched slice of x.Bytes(). 
If n < 0, all matches are returned in successive order. 
Otherwise, at most n matches are returned and they may not be successive. 
The result is nil if there are no matches, or if n == 0.
返回正则表达式r的非重叠匹配的排序列表，其中一个匹配是一对索引指定x.Bytes的匹配片（）
如果n<0, 所有的匹配会连续的返回
否则,最多n匹配被退回，他们可能不是连续的。
如果没有匹配或者n==0 结果是 nil


func (*Index) Lookup

func (x *Index) Lookup(s []byte, n int) (result []int)
Lookup returns an unsorted list of at most n indices where the byte string s occurs in the indexed data. 
If n < 0, all occurrences are returned. The result is nil if s is empty, s is not found, or n == 0. 
Lookup time is O(log(N)*len(s) + len(result)) where N is the size of the indexed data.
返回在那里的字节字符串s中出现的索引数据最n个索引的无序列表。
如果 n<0,所有遇到的都会返回.如果s是空的 ,s 没有发现 或 n==0  结果是nil
Lookup的时间复杂度是O(log(N)*len(s) + len(result)), N是 索引数据的大小


func (*Index) Read

func (x *Index) Read(r io.Reader) error
Read reads the index from r into x; x must not be nil.
从r中读取索引到 x. x必须不为 nil

func (*Index) Write

func (x *Index) Write(w io.Writer) error
Write writes the index x to w.
写索引x到 w














