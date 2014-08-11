Package tabwriter

Overview ▾

Package tabwriter implements a write filter (tabwriter.Writer) that translates tabbed columns in input into properly aligned text.

The package is using the Elastic Tabstops algorithm described at http://nickgravgaard.com/elastictabstops/index.html.

tabwriter包 实现 写过滤器(tabwriter.Writer)   转换标签栏输入成正确对齐文本。
该包 使用 描述在  http://nickgravgaard.com/elastictabstops/index.html 的 弹性制表位算法 

Constants
```golang
const (
        // Ignore html tags and treat entities (starting with '&'
        // and ending in ';') as single characters (width = 1).
           //忽略html 标签(以'&'开始 和以 ';'结束) 并 把HTML实体作为单个字符(width = 1).
        FilterHTML uint = 1 << iota

        // Strip Escape characters bracketing escaped text segments
        // instead of passing them through unchanged with the text.
        //Strip Escape 字符   过滤 文本段, 而不是  不变的传递他们.
        StripEscape

        // Force right-alignment of cell content.
        // Default is left-alignment.
           //强制单元格内容右对齐。
           //默认是左对齐
        AlignRight

        // Handle empty columns as if they were not present in
        // the input in the first place.
           // 如果他们没有出现在输入的首位, 处理空列.
        DiscardEmptyColumns

        // Always use tabs for indentation columns (i.e., padding of
        // leading empty cells on the left) independent of padchar.
          //请务必使用标签缩进列独立padchar的(例如, 先填充左边的空单元格)
        TabIndent

        // Print a vertical bar ('|') between columns (after formatting).
        // Discarded columns appear as zero-width columns ("||").
           //打印列之间的竖线  ('|') (在格式化后)
           //丢弃列显示为零宽度的列("||").
        Debug
)
```
Formatting can be controlled with these flags.
格式化 可以通过 这些 标志 控制.

```golang
const Escape = '\xff'
```

To escape a text segment, bracket it with Escape characters.
For instance, the tab in this string "Ignore this tab: \xff\t\xff" does not terminate a cell and constitutes a single character of width one for formatting purposes.

The value 0xff was chosen because it cannot appear in a valid UTF-8 sequence.

为了过滤一个文本段, 用Escape字符括起来. 例如,  在这个字符串的 制表符  "Ignore this tab: \xff\t\xff" 不会终止 一个单元格  并且构成了宽1用于格式化的单字符

选择 0xff值 因为它 不能出现 在一个有效的UTF-8 序列.



type Writer
```golang
type Writer struct {
        // contains filtered or unexported fields
}
```
A Writer is a filter that inserts padding around tab-delimited columns in its input to align them in the output.
Writer 是一个过滤器,   周围插入制表符分隔的列填充在它的输入来调节它们的输出。
The Writer treats incoming bytes as UTF-8 encoded text consisting of cells terminated by (horizontal or vertical) tabs or line breaks (newline or formfeed characters). 
Cells in adjacent lines constitute a column. The Writer inserts padding as needed to make all cells in a column have the same width, effectively aligning the columns.
It assumes that all characters have the same width except for tabs for which a tabwidth must be specified. 
Note that cells are tab-terminated, not tab-separated: trailing non-tab text at the end of a line does not form a column cell.

Writer 把传入的字符当成UTF-8 编码文本  单元格 由(水平或垂直)制表符或换行符(换行或换页字符)结束.
单元格在相邻的行 中 构成列. Writer  插入填充必要的, 使所有的单元格中的列具有相同的宽度，有效对齐列。
它假设所有的字符都由相同的宽 除了 tabwidth 指定的制表符.
注 该单元格 是 制表终止， 而不是 制表符分隔:  在一行的末尾尾随非标签文本不构成一列单元格。


The Writer assumes that all Unicode code points have the same width; this may not be true in some fonts.

Writer 假设 所有Unicode 编码 由相同宽度;  在一些字体中可能并非如此.

If DiscardEmptyColumns is set, empty columns that are terminated entirely by vertical (or "soft") tabs are discarded. 
Columns terminated by horizontal (or "hard") tabs are not affected by this flag.

如果 DiscardEmptyColumns 设置了,空列 完全由垂直制表符会丢弃.
列 以 水平 值表符 终止,不受此标志。

If a Writer is configured to filter HTML, HTML tags and entities are passed through. 
The widths of tags and entities are assumed to be zero (tags) and one (entities) for formatting purposes.

如果 Writer配置来过滤HTML, HTML 标签和实体被通过。
为了格式化 假设 标签和实体 的宽带 是 零(标签)  和  1 (实体)


A segment of text may be escaped by bracketing it with Escape characters. The tabwriter passes escaped text segments through unchanged. 
In particular, it does not interpret any tabs or line breaks within the segment. 
If the StripEscape flag is set, the Escape characters are stripped from the output; otherwise they are passed through as well. 
For the purpose of formatting, the width of the escaped text is always computed excluding the Escape characters.

The formfeed character ('\f') acts like a newline but it also terminates all columns in the current line (effectively calling Flush). 
Cells in the next line start new columns. Unless found inside an HTML tag or inside an escaped text segment, formfeed characters appear as newlines in the output.

The Writer must buffer input internally, because proper spacing of one line may depend on the cells in future lines. Clients must call Flush when done calling Write.

文本段可能 通过 包含它的 转义 字符 过滤.  tabwriter 传递 转义文本段 是 不变的.
特别是, 它不会 转化 文本段里的任何 制表符 或 换行符.
如果StripEscape  设置了,Escape 字符 从输出 跳出;  否则他们会被传递.
为了格式化的目的, 过滤文本子宽度 通常 计算 包括 Escape 字符.

换页字符 ('\f') 类似 换行符 它也终止于当前行的所有列. 
下一行单元格开始新列 . 除非里面的HTML标记或转义文本段内发现，换页字符显示为输出换行符。

Writer必须在内部缓冲器输入，因为 一行适当的间隔可以 依赖 单元格里未来的行




func NewWriter
```golang
func NewWriter(output io.Writer, minwidth, tabwidth, padding int, padchar byte, flags uint) *Writer
```
NewWriter allocates and initializes a new tabwriter.Writer. The parameters are the same as for the Init function.
NewWriter 分配和 初始化 新的 tabwriter.Writer. 参数和Init 一样.



func (*Writer) Flush
```golang
func (b *Writer) Flush() (err error)
```
Flush should be called after the last call to Write to ensure that any data buffered in the Writer is written to output. 
Any incomplete escape sequence at the end is considered complete for formatting purposes.
Flush 应该在  最后调用 Write 后 调用, 确保Writer里缓冲区的任何数据 写入输入,
在结束时,任何不完整的转义序列被认为是完整的格式化



func (*Writer) Init
```golang
func (b *Writer) Init(output io.Writer, minwidth, tabwidth, padding int, padchar byte, flags uint) *Writer
```
A Writer must be initialized with a call to Init. The first parameter (output) specifies the filter output. The remaining parameters control the formatting:
Writer必须调用 Init 初始化. 第一个参数 指定  过滤输出. 剩下的参数控制格式:

```golang
minwidth	minimal cell width including any padding
tabwidth	width of tab characters (equivalent number of spaces)
padding		padding added to a cell before computing its width
padchar		ASCII char used for padding
		if padchar == '\t', the Writer will assume that the
		width of a '\t' in the formatted output is tabwidth,
		and cells are left-aligned independent of align_left
		(for correct-looking results, tabwidth must correspond
		to the tab width in the viewer displaying the result)
flags		formatting control

minwidth  任何填充的最小的单元格的宽度.
tabwidth  制表符的宽度(相当于空格数)
padding   
padchar
flags
```

▹ Example
```golang
package main

import (
	"fmt"
	"os"
	"text/tabwriter"
)

func main() {
	w := new(tabwriter.Writer)

	// Format in tab-separated columns with a tab stop of 8.
	w.Init(os.Stdout, 0, 8, 0, '\t', 0)
	fmt.Fprintln(w, "a\tb\tc\td\t.")
	fmt.Fprintln(w, "123\t12345\t1234567\t123456789\t.")
	fmt.Fprintln(w)
	w.Flush()

	// Format right-aligned in space-separated columns of minimal width 5
	// and at least one blank of padding (so wider column entries do not
	// touch each other).
	w.Init(os.Stdout, 5, 0, 1, ' ', tabwriter.AlignRight)
	fmt.Fprintln(w, "a\tb\tc\td\t.")
	fmt.Fprintln(w, "123\t12345\t1234567\t123456789\t.")
	fmt.Fprintln(w)
	w.Flush()

}
```


func (*Writer) Write
```golang
func (b *Writer) Write(buf []byte) (n int, err error)
```
Write writes buf to the writer b. The only errors returned are ones encountered while writing to the underlying output stream.
Write 写 buf 到 b.  只有在遭遇 写入底层输出流遇到错误 才会返回 错误.







