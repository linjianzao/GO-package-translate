Overview ▾

Package time provides functionality for measuring and displaying time.

The calendrical calculations always assume a Gregorian calendar.

time 包提供 测量和显示时间功能.
历法的计算通常假设公历。
 

Constants
```golang
const (
        ANSIC       = "Mon Jan _2 15:04:05 2006"
        UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
        RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
        RFC822      = "02 Jan 06 15:04 MST"
        RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
        RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
        RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
        RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
        RFC3339     = "2006-01-02T15:04:05Z07:00"
        RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
        Kitchen     = "3:04PM"
        // Handy time stamps.
        Stamp      = "Jan _2 15:04:05"
        StampMilli = "Jan _2 15:04:05.000"
        StampMicro = "Jan _2 15:04:05.000000"
        StampNano  = "Jan _2 15:04:05.000000000"
)
```
These are predefined layouts for use in Time.Format and Time.Parse. The reference time used in the layouts is:
这些预定义的设计 用于   Time.Format 和 Time.Parse. 在该设计中使用的基准时间是:

```golang
Mon Jan 2 15:04:05 MST 2006
```
which is Unix time 1136239445. Since MST is GMT-0700, the reference time can be thought of as
Unix时间是 1136239445. 由于MST是 GMT-0700,参考时间可以被认为是

```golang
01/02 03:04:05PM '06 -0700
```
To define your own format, write down what the reference time would look like formatted your way; 
see the values of constants like ANSIC, StampMicro or Kitchen for examples. 
The model is to demonstrate what the reference time looks like so that the Format and Parse methods can apply the same transformation to a general time value.

为了定义你自己的格式, 写下 参考时间将看起来像格式化你的方式;
为例子 查看常量值类似ANSIC,StampMicro 或 Kitchen.
模型的演示参考时间的样子 因此 Format和 Parse 方法可以 应用 同样的转换 到 时间值.

Within the format string, an underscore _ represents a space that may be replaced by a digit if the following number (a day) has two digits; 
for compatibility with fixed-width Unix time formats.
在格式字符串,一个下划线 表示一个 可以被一个数字替换的空间, 如果 下面的数有两个数字; 与固定宽度的Unix时间格式的兼容性。


A decimal point followed by one or more zeros represents a fractional second, printed to the given number of decimal places. 
A decimal point followed by one or more nines represents a fractional second, printed to the given number of decimal places, with trailing zeros removed. 
When parsing (only), the input may contain a fractional second field immediately after the seconds field, even if the layout does not signify its presence. 
In that case a decimal point followed by a maximal series of digits is parsed as a fractional second.

Numeric time zone offsets format as follows:

小数点后面跟着一个或多个零代表了一个分数 , 打印指定的小数位数。
小数点后面跟着一个或多个9代表了一个分数 ,打印指定的小数位数, 移除末尾的0.
当解析时, 输入可能包含一个分数的秒字段 在秒字段后, 即使如果设计并不意味着它的存在。
在这种情况下,一系列小数点后面跟着一个最大的数字解析为一个分数。


```golang
-0700  ±hhmm
-07:00 ±hh:mm
```
Replacing the sign in the format with a Z triggers the ISO 8601 behavior of printing Z instead of an offset for the UTC zone. Thus:


```golang
Z0700  Z or ±hhmm
Z07:00 Z or ±hh:mm
```


func After
```golang
func After(d Duration) <-chan Time
```
After waits for the duration to elapse and then sends the current time on the returned channel. It is equivalent to NewTimer(d).C.
After 等待 时间持续  然后 在返回从channel 上 发送当前时间.它相当于 NewTimer(d).C.

##▾ Example
```golang
Code:

    select {
    case m := <-c:
            handle(m)
    case <-time.After(5 * time.Minute):
            fmt.Println("timed out")
    }

```



func Sleep
```golang
func Sleep(d Duration)
```
Sleep pauses the current goroutine for at least the duration d. A negative or zero duration causes Sleep to return immediately.
Sleep 暂停 当前的 goroutine 至少持续d 时间. 一个负的或零 时间 会 让 Sleep立即返回

▾ Example
```golang
package main

import (
	"time"
)

func main() {
	time.Sleep(100 * time.Millisecond)
}
```


func Tick
```golang
func Tick(d Duration) <-chan Time
```
Tick is a convenience wrapper for NewTicker providing access to the ticking channel only. Useful for clients that have no need to shut down the ticker.
Tick 是一个方便的 封装  NewTicker 只提供访问  ticking channel.  对于客户端是有用的 所以不必关闭 ticker


▾ Example
```golang
Code:

    c := time.Tick(1 * time.Minute)
    for now := range c {
            fmt.Printf("%v %s\n", now, statusUpdate())
    }
```


type Duration
```golang
type Duration int64
```
A Duration represents the elapsed time between two instants as an int64 nanosecond count. 
The representation limits the largest representable duration to approximately 290 years.
Duration 表示 两个瞬间之间的运行时间 的int64纳秒数。
表示限制了最大可表示的持续时间大约290年。

```golang
const (
        Nanosecond  Duration = 1
        Microsecond          = 1000 * Nanosecond
        Millisecond          = 1000 * Microsecond
        Second               = 1000 * Millisecond
        Minute               = 60 * Second
        Hour                 = 60 * Minute
)
```
Common durations. There is no definition for units of Day or larger to avoid confusion across daylight savings time zone transitions.
共同的时间. 这没有定义Day的单位 或 更大的  为了避免混淆时区转换


To count the number of units in a Duration, divide:
计算Duration的单元的数量, 分:

```golang
second := time.Second
fmt.Print(int64(second/time.Millisecond)) // prints 1000
```
To convert an integer number of units to a Duration, multiply:
转换一个整型数 单位成Duration , 增加:

```golang
seconds := 10
fmt.Print(time.Duration(seconds)*time.Second) // prints 10s
```

▾ Example
```golang
Code:

    t0 := time.Now()
    expensiveCall()
    t1 := time.Now()
    fmt.Printf("The call took %v to run.\n", t1.Sub(t0))
```



func ParseDuration
```golang
func ParseDuration(s string) (Duration, error)
```
ParseDuration parses a duration string. 
A duration string is a possibly signed sequence of decimal numbers, each with optional fraction and a unit suffix, such as "300ms", "-1.5h" or "2h45m". 
Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h".

ParseDuration 解析一个duration字符串.
duration字符串 是一个 可能 签署的一序列小数,  每个有可选 分数 和一个 单位后缀, 例如 "300ms", "-1.5h" or "2h45m". 
有效的时间单位是"ns", "us" (or "µs"), "ms", "s", "m", "h".



func Since
```golang
func Since(t Time) Duration
```
Since returns the time elapsed since t. It is shorthand for time.Now().Sub(t).
Since 返回时间t. 它是time.Now().Sub(t) 的简写.



func (Duration) Hours
```golang
func (d Duration) Hours() float64
```
Hours returns the duration as a floating point number of hours.
Hours  返回时间的小时浮点数。



func (Duration) Minutes
```golang
func (d Duration) Minutes() float64
```
Minutes returns the duration as a floating point number of minutes.
Minutes  返回时间的分钟浮点数。


func (Duration) Nanoseconds
```golang
func (d Duration) Nanoseconds() int64
```
Nanoseconds returns the duration as an integer nanosecond count.
Nanoseconds 返回时间为整数的纳秒计数。


func (Duration) Seconds
```golang
func (d Duration) Seconds() float64
```
Seconds returns the duration as a floating point number of seconds.
Seconds返回时间的秒浮点数。


func (Duration) String
```golang
func (d Duration) String() string
```
String returns a string representing the duration in the form "72h3m0.5s". Leading zero units are omitted. 
As a special case, durations less than one second format use a smaller unit (milli-, micro-, or nanoseconds) to ensure that the leading digit is non-zero. 
The zero duration formats as 0, with no unit.

String返回一个字符串时间表示“72 h3m0.5s”。前导零单位 省略。
作为一个特例 ,时间不到一秒格式使用一个较小的单位(毫、微、纳秒),以确保前导的数字不是零。



type Location
```golang
type Location struct {
        // contains filtered or unexported fields
}
```
A Location maps time instants to the zone in use at that time. 
Typically, the Location represents the collection of time offsets in use in a geographical area, such as CEST and CET for central Europe.
Location 映射到当时的时区使用。
通常, Location 表示 在使用时间偏移的地理区域集合, 例如  欧洲中部 是CEST 和 CET. 


```golang
var Local *Location = &localLoc
```
Local represents the system's local time zone.
Local 表示系统的本地时区


```golang
var UTC *Location = &utcLoc
```
UTC represents Universal Coordinated Time (UTC).
UTC 表示 通用协调时间 (UTC).


func FixedZone
```golang
func FixedZone(name string, offset int) *Location
```
FixedZone returns a Location that always uses the given zone name and offset (seconds east of UTC).
FixedZone 返回一个 Location  通常使用 给定的时区name 和 offset(seconds east of UTC).



func LoadLocation
```golang
func LoadLocation(name string) (*Location, error)
```
LoadLocation returns the Location with the given name.

If the name is "" or "UTC", LoadLocation returns UTC. If the name is "Local", LoadLocation returns Local.

Otherwise, the name is taken to be a location name corresponding to a file in the IANA Time Zone database, such as "America/New_York".

The time zone database needed by LoadLocation may not be present on all systems, especially non-Unix systems. 
LoadLocation looks in the directory or uncompressed zip file named by the ZONEINFO environment variable, if any, then looks in known installation locations on Unix systems, and finally looks in $GOROOT/lib/time/zoneinfo.zip.

LoadLocation 用给定的name返回 Location.

如果 name 是 "" 或 "UTC",LoadLocation 返回 UTC. 如果name是  "Local", LoadLocation 返回 Local.

否则 name  采用  IANA Time Zone 数据库里  name位置 对应的文件,例如 "America/New_York".

所需的时区数据库LoadLocation可能不会出现在所有的系统, 特别是非unix系统。
LoadLocation 用ZONEINFO环境变量查看目录 或未解压zip文件, 如果有, 查看已知在Unix系统上安装位置, 最终在 $GOROOT/lib/time/zoneinfo.zip.



func (*Location) String
```golang
func (l *Location) String() string
```
String returns a descriptive name for the time zone information, corresponding to the argument to LoadLocation.
String返回一个描述性的名称时区信息 ,对应LoadLocation的参数。



type Month
```golang
type Month int
```
A Month specifies a month of the year (January = 1, ...).
Month 指定年的 月份 (January = 1, ...).

```golang
const (
        January Month = 1 + iota
        February
        March
        April
        May
        June
        July
        August
        September
        October
        November
        December
)
```

▹ Example

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	_, month, day := time.Now().Date()
	if month == time.November &amp;&amp; day == 10 {
		fmt.Println("Happy Go day!")
	}
}
```



func (Month) String
```golang
func (m Month) String() string
```
String returns the English name of the month ("January", "February", ...).
String 返回月份的英语名("January", "February", ...).



type ParseError
```golang
type ParseError struct {
        Layout     string
        Value      string
        LayoutElem string
        ValueElem  string
        Message    string
}
```
ParseError describes a problem parsing a time string.
ParseError 描述 解析时间字符串的问题.



func (*ParseError) Error
```golang
func (e *ParseError) Error() string
```
Error returns the string representation of a ParseError.
Error 返回 ParseError的字符串表示.



type Ticker
```golang
type Ticker struct {
        C <-chan Time // The channel on which the ticks are delivered.
        // contains filtered or unexported fields
}
```
A Ticker holds a channel that delivers `ticks' of a clock at intervals.
Ticker 有一个channel 提供一个时钟的 时间间隔 `ticks'



func NewTicker
```golang
func NewTicker(d Duration) *Ticker
```
NewTicker returns a new Ticker containing a channel that will send the time with a period specified by the duration argument. 
It adjusts the intervals or drops ticks to make up for slow receivers. The duration d must be greater than zero; 
if not, NewTicker will panic. Stop the ticker to release associated resources.
NewTicker 返回一个新的Ticker 包含 一个 channel 将会和一个句号一起 发送  根据 时间参数指定 的时间
调整时间间隔或drops ticks ,以弥补缓慢的接收器。 时间d 必须大于0
如果没有 ,NewTicker 将会引发panic. 停止 ticker释放相关资源。



func (*Ticker) Stop
```golang
func (t *Ticker) Stop()
```
Stop turns off a ticker. After Stop, no more ticks will be sent. Stop does not close the channel, to prevent a read from the channel succeeding incorrectly.
Stop 关闭一个 ticker. Stop之后, 不会在发送 ticks . Stop 不会关闭 channel ,为了防止从通道读取成功不正确。



type Time
```golang
type Time struct {
        // contains filtered or unexported fields
}
```
A Time represents an instant in time with nanosecond precision.

Programs using times should typically store and pass them as values, not pointers. 
That is, time variables and struct fields should be of type time.Time, not *time.Time. A Time value can be used by multiple goroutines simultaneously.

Time instants can be compared using the Before, After, and Equal methods. The Sub method subtracts two instants, producing a Duration. 
The Add method adds a Time and a Duration, producing a Time.

The zero value of type Time is January 1, year 1, 00:00:00.000000000 UTC. 
As this time is unlikely to come up in practice, the IsZero method gives a simple way of detecting a time that has not been initialized explicitly.

Each Time has associated with it a Location, consulted when computing the presentation form of the time, such as in the Format, Hour, and Year methods. 
The methods Local, UTC, and In return a Time with a specific location. Changing the location in this way changes only the presentation; 
it does not change the instant in time being denoted and therefore does not affect the computations described in earlier paragraphs.

Time表示一个 瞬间 的时间与纳秒精度。
程序使用时间应该通常存储和传递这些值,不是指针。
那就是说,时间变量 和结构体字段 应该是time.Time类型,而不是  *time.Time.   Time 值可以同时用于多个goroutines.

时间可以 使用 Before, After, 和 Equal 方法对比. Sub 方法 对两个instants 相减 ,产生一个 Duration.
Add方法 添加一个Time 和一个Duration ,产生一个 Time.

Time类型的零值是 January 1, year 1, 00:00:00.000000000 UTC. 
这个时间是不可能用于实际的,IsZero 方法 给了一个简单的 检测一段时间,没有显式初始化 的方法。

每个Time 关联一个 Location, 当计算时间的表现形式时咨询, 例如 Format, Hour,和Year方法.
Local, UTC, 和 In方法 用指定的location返回一个 Time.  以这种方式改变位置变化只表示;
它不改变目前的时间表示,因此不影响前面段落中描述的计算。



func Date
```golang
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```
Date returns the Time corresponding to
Date 返回Time对应的 
```golang
yyyy-mm-dd hh:mm:ss + nsec nanoseconds
```

in the appropriate zone for that time in the given location.

The month, day, hour, min, sec, and nsec values may be outside their usual ranges and will be normalized during the conversion. For example, October 32 converts to November 1.

A daylight savings time transition skips or repeats times. 
For example, in the United States, March 13, 2011 2:15am never occurred, while November 6, 2011 1:15am occurred twice. 
In such cases, the choice of time zone, and therefore the time, is not well-defined. 
Date returns a time that is correct in one of the two zones involved in the transition, but it does not guarantee which.

Date panics if loc is nil.

在适当的区域在给定的位置。
month, day, hour, min, sec, and nsec 值 可能超过他们的使用范围, 并期间将归一化的转换.例如 October 32  转化为November 1.

daylight savings time 过渡跳过或重复多次时间。
例如在US 永远不会遇到March 13, 2011 2:15am ,但遇到 November 6, 2011 1:15am 两次.
这这种情况下,时区的选择 是不明确的.
Date返回  正确的 在两个时区之间转化的时间, 但是不能保证.

如果loc是nil ,Date 引发panic.


▹ Example

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Date(2009, time.November, 10, 23, 0, 0, 0, time.UTC)
	fmt.Printf("Go launched at %s\n", t.Local())
}

```



func Now
```golang
func Now() Time
```
Now returns the current local time.
Now 返回当前本地时间。



func Parse
```golang
func Parse(layout, value string) (Time, error)
```
Parse parses a formatted string and returns the time value it represents. The layout defines the format by showing how the reference time,
Parse 解析一个格式化的字符串 并返回它表示的时间值. 通过展示布局定义格式参考时间,

```golang
Mon Jan 2 15:04:05 -0700 MST 2006
```

would be interpreted if it were the value; it serves as an example of the input format. 
The same interpretation will then be made to the input string. 
Predefined layouts ANSIC, UnixDate, RFC3339 and others describe standard and convenient representations of the reference time. 
For more information about the formats and the definition of the reference time, see the documentation for ANSIC and the other constants defined by this package.
如果它是一个值 会解释; 它是输入格式的一个例子。
相同的解释将输入字符串。
预定义的布局 ANSIC, UnixDate, RFC3339  和其他描述标准 和 方便的参考时间表示。
更多信息的格式和引用的定义, 查看这个包定义的ANSIC 文档和 其他常量.


Elements omitted from the value are assumed to be zero or, when zero is impossible, one, so parsing "3:04pm" returns the time corresponding to Jan 1, year 0, 15:04:00 UTC 
(note that because the year is 0, this time is before the zero Time). 
Years must be in the range 0000..9999. The day of the week is checked for syntax but it is otherwise ignored.
元素省略 值假定是零或  当可能是零时, 1  所以解析  "3:04pm" 返回时间对应的 Jan 1, year 0, 15:04:00 UTC(注:因为 年是0, 这个时间在 零Time 之前).
年必须在 0000..9999之间.一周的天 检查语法但另有忽略。


In the absence of a time zone indicator, Parse returns a time in UTC.
在缺乏一个时区指标,Parse 返回 UTC 中的一个时间.

When parsing a time with a zone offset like -0700, if the offset corresponds to a time zone used by the current location (Local), then Parse uses that location and zone in the returned time. 
Otherwise it records the time as being in a fabricated location with time fixed at the given zone offset.
当解析一个时间  如-0700, 如果 偏移对应一个当前本地时区,Parse 在返回的时间 使用那个location and zone

When parsing a time with a zone abbreviation like MST, if the zone abbreviation has a defined offset in the current location, then that offset is used. 
The zone abbreviation "UTC" is recognized as UTC regardless of location. 
If the zone abbreviation is unknown, Parse records the time as being in a fabricated location with the given zone abbreviation and a zero offset. 
This choice means that such a time can be parse and reformatted with the same layout losslessly, but the exact instant used in the representation will differ by the actual zone offset. 
To avoid such problems, prefer time layouts that use a numeric zone offset, or use ParseInLocation.
当解析一个时间 用区缩写 如 MST, 如果 区缩写 在当前的 location  有一个定义的偏移, 使用那个offset.
区缩写"UTC" 主要指UTC而不考虑它们的位置。
如果区缩写 是未知的, Parse  用给定的 区缩写和区偏移 在 fabricated location 记录时间.
这个选择意味着  一个时间可以解析 并且重新格式化losslessly相同的格局, 但具体的即时表示将通过实际的不同区域中使用的偏移量。
为了避免这些问题,喜欢时间布局,使用数字区偏移,或使用ParseInLocation。


▹ Example
```golang
Code:

// longForm shows by example how the reference time would be represented in
    // the desired layout.
    const longForm = "Jan 2, 2006 at 3:04pm (MST)"
    t, _ := time.Parse(longForm, "Feb 3, 2013 at 7:54pm (PST)")
    fmt.Println(t)

    // shortForm is another way the reference time would be represented
    // in the desired layout; it has no time zone present.
    // Note: without explicit zone, returns time in UTC.
    const shortForm = "2006-Jan-02"
    t, _ = time.Parse(shortForm, "2013-Feb-03")
    fmt.Println(t)
```
    
Output:

```golang
2013-02-03 19:54:00 -0800 PST
2013-02-03 00:00:00 +0000 UTC
```



func ParseInLocation
```golang
func ParseInLocation(layout, value string, loc *Location) (Time, error)
```
ParseInLocation is like Parse but differs in two important ways. 
First, in the absence of time zone information, Parse interprets a time as UTC;
ParseInLocation interprets the time as in the given location. 
Second, when given a zone offset or abbreviation, Parse tries to match it against the Local location; 
ParseInLocation uses the given location.
ParseInLocation 类似 Parse 但是在两个重要方面不一样.
首先  在缺乏时区信息, Parse 解释为UTC时间;
ParseInLocation 在给定的location 解释时间.
其次,当给定 一个 区偏移或 缩写时, Parse 尝试匹配 它 对Local的位置;
ParseInLocation使用给定的位置。

▹ Example
```golang
Code:

loc, _ := time.LoadLocation("Europe/Berlin")

    const longForm = "Jan 2, 2006 at 3:04pm (MST)"
    t, _ := time.ParseInLocation(longForm, "Jul 9, 2012 at 5:02am (CEST)", loc)
    fmt.Println(t)

    // Note: without explicit zone, returns time in given location.
    const shortForm = "2006-Jan-02"
    t, _ = time.ParseInLocation(shortForm, "2012-Jul-09", loc)
    fmt.Println(t)
```
    
Output:

```golang
2012-07-09 05:02:00 +0200 CEST
2012-07-09 00:00:00 +0200 CEST
```



func Unix
```golang
func Unix(sec int64, nsec int64) Time
```
Unix returns the local Time corresponding to the given Unix time, sec seconds and nsec nanoseconds since January 1, 1970 UTC. 
It is valid to pass nsec outside the range [0, 999999999].
Unix 返回本地Time 对应的 指定的 Unix时间,sec秒 和nsec纳秒从 January 1, 1970 UTC 开始.
它 超过nsec 范围外 是有效的.



func (Time) Add
```golang
func (t Time) Add(d Duration) Time
```
Add returns the time t+d.
Add 返回时间t+d.



func (Time) AddDate
```golang
func (t Time) AddDate(years int, months int, days int) Time
```
AddDate returns the time corresponding to adding the given number of years, months, and days to t. 
For example, AddDate(-1, 2, 3) applied to January 1, 2011 returns March 4, 2010.

AddDate normalizes its result in the same way that Date does, so, for example, adding one month to October 31 yields December 1, the normalized form for November 31.

AddDate返回时间对应的添加 给定的 years, months, and days数 到t.
例如,AddDate(-1, 2, 3)应用到  January 1, 2011  返回  March 4, 2010.

AddDate 规范化Date其结果在相同的方式,因此, 如 添加1个月到October 31  December 1,November 31的标准化形式。




func (Time) After
```golang
func (t Time) After(u Time) bool
```
After reports whether the time instant t is after u.
After 报告时间 t 是否在u后面



func (Time) Before
```golang
func (t Time) Before(u Time) bool
````
Before reports whether the time instant t is before u.
Before 报告时间 t 是否在u前面



func (Time) Clock
```golang
func (t Time) Clock() (hour, min, sec int)
```
Clock returns the hour, minute, and second within the day specified by t.
Clock 返回 在t里的 小时 , 分钟 和秒



func (Time) Date
```golang
func (t Time) Date() (year int, month Month, day int)
```
Date returns the year, month, and day in which t occurs.
Date 返回在t 里的 年, 月, 日



func (Time) Day
```golang
func (t Time) Day() int
```
Day returns the day of the month specified by t.
Day 返回指定t的日。



func (Time) Equal
```golang
func (t Time) Equal(u Time) bool
```
Equal reports whether t and u represent the same time instant. 
Two times can be equal even if they are in different locations. 
For example, 6:00 +0200 CEST and 4:00 UTC are Equal. 
This comparison is different from using t == u, which also compares the locations.
Equal 报告 t和u 是否表示 相同的时间.
两次时间即使如果地区不一样也可以相等.
例如 6:00 +0200 CEST 和 4:00 UTC相等
这个比较和使用 t == u 不一样, 它也可以比较地区.


func (Time) Format
```golang
func (t Time) Format(layout string) string
```
Format returns a textual representation of the time value formatted according to layout, which defines the format by showing how the reference time,
Format 返回一个 时间值 根据layout 格式化的  纯文本表示, 其中定义了 格式 通过 展示参考时间,

```golang
Mon Jan 2 15:04:05 -0700 MST 2006
```

would be displayed if it were the value; it serves as an example of the desired output. 
The same display rules will then be applied to the time value. 
Predefined layouts ANSIC, UnixDate, RFC3339 and others describe standard and convenient representations of the reference time. 
For more information about the formats and the definition of the reference time, see the documentation for ANSIC and the other constants defined by this package.
如果它是值, 将会显示.它是所需的输出的一个示例。
时间值适用相同的显示规则.
预定义的布局 ANSIC, UnixDate, RFC3339  和其他描述标准 和 方便的参考时间表示。
更多信息的格式和引用的定义, 查看这个包定义的ANSIC 文档和 其他常量.



▹ Example
```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	// layout shows by example how the reference time should be represented.
	const layout = "Jan 2, 2006 at 3:04pm (MST)"
	t := time.Date(2009, time.November, 10, 15, 0, 0, 0, time.Local)
	fmt.Println(t.Format(layout))
	fmt.Println(t.UTC().Format(layout))
}
```



func (*Time) GobDecode
```golang
func (t *Time) GobDecode(data []byte) error
```
GobDecode implements the gob.GobDecoder interface.
GobDecode实现 GobDecode接口



func (Time) GobEncode
```golang
func (t Time) GobEncode() ([]byte, error)
```
GobEncode implements the gob.GobEncoder interface.
GobEncode 实现 gob.GobEncoder 接口



func (Time) Hour
```golang
func (t Time) Hour() int
```
Hour returns the hour within the day specified by t, in the range [0, 23].
Hour 返回t的小时, 范围在 [0, 23].



func (Time) ISOWeek
```golang
func (t Time) ISOWeek() (year, week int)
```
ISOWeek returns the ISO 8601 year and week number in which t occurs. 
Week ranges from 1 to 53. Jan 01 to Jan 03 of year n might belong to week 52 or 53 of year n-1, and Dec 29 to Dec 31 might belong to week 1 of year n+1.
ISOWeek 返回t的  ISO 8601 年和周数. 周的范围 从1 到53. 每年的1月1日到1月3日,n可能属于52 星期或 53 年n - 1 ,12月29日至12月31日可能属于n + 1 年 1周。



func (Time) In
```golang
func (t Time) In(loc *Location) Time
```
In returns t with the location information set to loc.
In panics if loc is nil.
在返回t 位置信息设置到loc 。如果loc是nil 会引发panic



func (Time) IsZero
```golang
func (t Time) IsZero() bool
```
IsZero reports whether t represents the zero time instant, January 1, year 1, 00:00:00 UTC.
IsZero 报告 t 是否表示零时间, January 1, year 1, 00:00:00 UTC.



func (Time) Local
```golang
func (t Time) Local() Time
```
Local returns t with the location set to local time.
Local 返回当地时间t的位置设置。



func (Time) Location
```golang
func (t Time) Location() *Location
```
Location returns the time zone information associated with t.
Location 返回t 对应的时区信息.



func (Time) MarshalBinary
```golang
func (t Time) MarshalBinary() ([]byte, error)
```
MarshalBinary implements the encoding.BinaryMarshaler interface.
MarshalBinary实现 encoding.BinaryMarshaler 接口



func (Time) MarshalJSON
```golang
func (t Time) MarshalJSON() ([]byte, error)
```
MarshalJSON implements the json.Marshaler interface. The time is a quoted string in RFC 3339 format, with sub-second precision added if present.
MarshalJSON 实现json.Marshaler 接口.  时间引号字符串在 RFC 3339格式,  如果现在添加了次秒级精度。



func (Time) MarshalText
```golang
func (t Time) MarshalText() ([]byte, error)
```
MarshalText implements the encoding.TextMarshaler interface. The time is formatted in RFC 3339 format, with sub-second precision added if present.
MarshalText 实现 encoding.TextMarshaler  接口.  实现 是用  RFC 3339 格式 格式化的,如果现在添加了次秒级精度。



func (Time) Minute
```golang
func (t Time) Minute() int
```
Minute returns the minute offset within the hour specified by t, in the range [0, 59].
Minute 返回指定t 里的 分钟偏移,范围 [0, 59].



func (Time) Month
```golang
func (t Time) Month() Month
```
Month returns the month of the year specified by t.
Month 返回指定t的月份.



func (Time) Nanosecond
```golang
func (t Time) Nanosecond() int
```
Nanosecond returns the nanosecond offset within the second specified by t, in the range [0, 999999999].
Nanosecond 返回指定t的 纳秒偏移,范围  [0, 999999999].



func (Time) Round
```golang
func (t Time) Round(d Duration) Time
```
Round returns the result of rounding t to the nearest multiple of d (since the zero time). 
The rounding behavior for halfway values is to round up. If d <= 0, Round returns t unchanged.
Round 返回 舍入t 最接近的d 的结果
中间值舍入行为是圆的。 如果d <= 0, Round 返回的t不会修改


▹ Example
```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Date(0, 0, 0, 12, 15, 30, 918273645, time.UTC)
	round := []time.Duration{
		time.Nanosecond,
		time.Microsecond,
		time.Millisecond,
		time.Second,
		2 * time.Second,
		time.Minute,
		10 * time.Minute,
		time.Hour,
	}

	for _, d := range round {
		fmt.Printf("t.Round(%6s) = %s\n", d, t.Round(d).Format("15:04:05.999999999"))
	}
}
```



func (Time) Second
```golang
func (t Time) Second() int
```
Second returns the second offset within the minute specified by t, in the range [0, 59].
Second 返回t的秒 , 范围  [0, 59].



func (Time) String
```golang
func (t Time) String() string
```
String returns the time formatted using the format string
String 适用字符串格式化 时间
```golang
"2006-01-02 15:04:05.999999999 -0700 MST"
```


func (Time) Sub
```golang
func (t Time) Sub(u Time) Duration
```
Sub returns the duration t-u. 
If the result exceeds the maximum (or minimum) value that can be stored in a Duration, the maximum (or minimum) duration will be returned. 
To compute t-d for a duration d, use t.Add(-d).
Sub 返回 时间 t-u. 
如果结果 超过 存储在 Duration里的最大值,  最大值时间将会返回,.
为了计算 t-d 的时间d, 使用t.Add(-d).



func (Time) Truncate
```golang
func (t Time) Truncate(d Duration) Time
```
Truncate returns the result of rounding t down to a multiple of d (since the zero time). If d <= 0, Truncate returns t unchanged.	
Truncate 返回结果的舍入t的d(自零时间)。 如果d<=0, Truncate 返回的t不会修改.



▹ Example
```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	t, _ := time.Parse("2006 Jan 02 15:04:05", "2012 Dec 07 12:15:30.918273645")
	trunc := []time.Duration{
		time.Nanosecond,
		time.Microsecond,
		time.Millisecond,
		time.Second,
		2 * time.Second,
		time.Minute,
		10 * time.Minute,
		time.Hour,
	}

	for _, d := range trunc {
		fmt.Printf("t.Truncate(%6s) = %s\n", d, t.Truncate(d).Format("15:04:05.999999999"))
	}

}
```



func (Time) UTC
```golang
func (t Time) UTC() Time
```
UTC returns t with the location set to UTC.
UTC 返回 地区设置为UTC的t 



func (Time) Unix
```golang
func (t Time) Unix() int64
```
Unix returns t as a Unix time, the number of seconds elapsed since January 1, 1970 UTC.
Unix 返回一个 Unix时间t,  秒数从  January 1, 1970 UTC.开始.



func (Time) UnixNano
```golang
func (t Time) UnixNano() int64
```
UnixNano returns t as a Unix time, the number of nanoseconds elapsed since January 1, 1970 UTC. 
The result is undefined if the Unix time in nanoseconds cannot be represented by an int64. Note that this means the result of calling UnixNano on the zero Time is undefined.
UnixNano 返回一个Unix 时间t. 纳秒数从  January 1, 1970 UTC 开始.
如果Unix 在纳秒里不能用 int64 表示, 结果是 undefined. 注: 这样意味着 在零Time 上调用 UnixNano 结果是 undefined



func (*Time) UnmarshalBinary
```golang
func (t *Time) UnmarshalBinary(data []byte) error
```
UnmarshalBinary implements the encoding.BinaryUnmarshaler interface.
UnmarshalBinary 实现 encoding.BinaryUnmarshaler 接口


func (*Time) UnmarshalJSON
```golang
func (t *Time) UnmarshalJSON(data []byte) (err error)
```
UnmarshalJSON implements the json.Unmarshaler interface. The time is expected to be a quoted string in RFC 3339 format.
UnmarshalJSON 实现  json.Unmarshaler 接口. 时间预计将在RFC 3339格式。 



func (*Time) UnmarshalText
```golang
func (t *Time) UnmarshalText(data []byte) (err error)
```
UnmarshalText implements the encoding.TextUnmarshaler interface. The time is expected to be in RFC 3339 format.
UnmarshalText 实现 encoding.TextUnmarshaler 接口.时间预计将在RFC 3339格式。 



func (Time) Weekday
```golang
func (t Time) Weekday() Weekday
```
Weekday returns the day of the week specified by t.
Weekday 返回t的周



func (Time) Year
```golang
func (t Time) Year() int
```
Year returns the year in which t occurs.
Year 返回t 的 年.



func (Time) YearDay
```golang
func (t Time) YearDay() int
```
YearDay returns the day of the year specified by t, in the range [1,365] for non-leap years, and [1,366] in leap years.
YearDay  返回t的年 , 范围 不是 闰年 [1,365], 闰年[1,366]



func (Time) Zone
```golang
func (t Time) Zone() (name string, offset int)
```
Zone computes the time zone in effect at time t, returning the abbreviated name of the zone (such as "CET") and its offset in seconds east of UTC.
Zone 计算 时间t 的时区. 返回时区缩写 (例如"CET") 和UTC东 秒偏移



type Timer
```golang
type Timer struct {
        C <-chan Time
        // contains filtered or unexported fields
}
```
The Timer type represents a single event. When the Timer expires, the current time will be sent on C, unless the Timer was created by AfterFunc.
Timer类型表示单个事件. 当 Timer 过期,  当前时间将会发送C,除非Timer是由AfterFunc创建的。


func AfterFunc
```golang
func AfterFunc(d Duration, f func()) *Timer
```
AfterFunc waits for the duration to elapse and then calls f in its own goroutine. It returns a Timer that can be used to cancel the call using its Stop method.
AfterFunc 等待 时间过去, 然后在它的goroutine 调用f. 它返回 一个Timer  可以用来 放弃调用  使用它的Stop方法.



func NewTimer
```golang
func NewTimer(d Duration) *Timer
```
NewTimer creates a new Timer that will send the current time on its channel after at least duration d.
NewTimer 创建一个新的 Timer 将发送当前时间 channel 在至少d时间后.



func (*Timer) Reset
```golang
func (t *Timer) Reset(d Duration) bool
```
Reset changes the timer to expire after duration d. It returns true if the timer had been active, false if the timer had expired or been stopped.
Reset 在时间d 之后修改timer过期. 如果timer 被激活 它返回true.如果 timer 过期或已经停止, 返回false.


func (*Timer) Stop
```golang
func (t *Timer) Stop() bool
```
Stop prevents the Timer from firing.
It returns true if the call stops the timer, false if the timer has already expired or been stopped. 
Stop does not close the channel, to prevent a read from the channel succeeding incorrectly.
Stop防止Timer射击。
如果调用停止timer 返回true. 如果timer已经过期 或已经停止 返回false.
Stop 不会关闭 channel,为了防止从通道读取成功不正确。


type Weekday
```golang
type Weekday int
```
A Weekday specifies a day of the week (Sunday = 0, ...).
Weekday 指定 周 (Sunday = 0, ...).

```golang
const (
        Sunday Weekday = iota
        Monday
        Tuesday
        Wednesday
        Thursday
        Friday
        Saturday
)
```

func (Weekday) String
```golang
func (d Weekday) String() string
```
String returns the English name of the day ("Sunday", "Monday", ...).
String 返回日期的英文名 ("Sunday", "Monday", ...).


















































