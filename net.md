包地址：http://golang.org/pkg/net/

Package net provides a portable interface for network I/O, including TCP/IP, UDP, domain name resolution, and Unix domain sockets.
Although the package provides access to low-level networking primitives, most clients will need only the basic interface provided by the Dial, Listen, and Accept functions and the associated Conn and Listener interfaces. 
The crypto/tls package uses the same interfaces and similar Dial and Listen functions.
net包提供便捷的网络I/O接口，包含TCP/IP, UDP,域名解析，Unix 域 sockets
包提供对网络底层的访问，大部分客户端只需要提供Dial、Listen、接受函数和相关联的链接和监听的简单接口

The Dial function connects to a server:
Dial函数连接服务器：

conn, err := net.Dial("tcp", "google.com:80")
if err != nil {
	// handle error
}
fmt.Fprintf(conn, "GET / HTTP/1.0\r\n\r\n")
status, err := bufio.NewReader(conn).ReadString('\n')
// ...



The Listen function creates servers:
Listen函数创建服务：



ln, err := net.Listen("tcp", ":8080")
if err != nil {
	// handle error
}
for {
	conn, err := ln.Accept()
	if err != nil {
		// handle error
		continue
	}
	go handleConnection(conn)
}





Constants
    const (
        IPv4len = 4
        IPv6len = 16
    )
    IP address lengths (bytes).
    IP地址长度（字节）

Variables
     var (
        IPv4bcast     = IPv4(255, 255, 255, 255) // broadcast
        IPv4allsys    = IPv4(224, 0, 0, 1)       // all systems
        IPv4allrouter = IPv4(224, 0, 0, 2)       // all routers
        IPv4zero      = IPv4(0, 0, 0, 0)         // all zeros
    )
    
    
Well-known IPv4 addresses
IPv4地址
        
   var (
        IPv6zero                   = IP{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
        IPv6unspecified            = IP{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
        IPv6loopback               = IP{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1}
        IPv6interfacelocalallnodes = IP{0xff, 0x01, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
        IPv6linklocalallnodes      = IP{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
        IPv6linklocalallrouters    = IP{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x02}
    )
        Well-known IPv6 addresses
        IPv6地址

var (
        ErrWriteToConnected = errors.New("use of WriteTo with pre-connected connection")
)



func InterfaceAddrs
    
func InterfaceAddrs() ([]Addr, error)
InterfaceAddrs returns a list of the system's network interface addresses.
返回系统网络接口地址列表
   


func Interfaces
        
func Interfaces() ([]Interface, error)
Interfaces returns a list of the system's network interfaces.
返回系统网络接口



func JoinHostPort
    
func JoinHostPort(host, port string) string
JoinHostPort combines host and port into a network address of the form "host:port" or, if host contains a colon or a percent sign, "[host]:port".
根据 "host:port"  格式合并 host 和port 网络地址，如果主机包含冒号或者百分号 格式就是 "[host]:port"。
	
	
	
func LookupAddr
func LookupAddr(addr string) (name []string, err error)
LookupAddr performs a reverse lookup for the given address, returning a list of names mapping to that address.
对于给定的地址进行反向查找， 返回那个地址的名称映射。



func LookupCNAME

func LookupCNAME(name string) (cname string, err error)
LookupCNAME returns the canonical DNS host for the given name. 
Callers that do not care about the canonical name can call LookupHost or LookupIP directly; 
both take care of resolving the canonical name as part of the lookup.
返回规范的DNS主机名称。调用者不关心规范的名称能直接让LookupHost 或者LookupIP调用。 都在乎解析查找规范名称的一部分。
    
    

func LookupHost

func LookupHost(host string) (addrs []string, err error)
LookupHost looks up the given host using the local resolver. 
It returns an array of that host's addresses.
LookupHost使用本地解析器 查询给定的host.
它返回那个host的地址 数组



func LookupIP

func LookupIP(host string) (addrs []IP, err error)
LookupIP looks up host using the local resolver. 
It returns an array of that host's IPv4 and IPv6 addresses.
LookupIP用本地解析器 查询host.
它返回 host的IPv4和IPv6地址数组



func LookupMX

func LookupMX(name string) (mx []*MX, err error)
LookupMX returns the DNS MX records for the given domain name sorted by preference.
LookupMX  返回用给定的 域名排序优先 的 DNS MX 记录.



func LookupNS

func LookupNS(name string) (ns []*NS, err error)
LookupNS returns the DNS NS records for the given domain name.
LookupNS 返回 给定域名name的DNS NS记录



func LookupPort

func LookupPort(network, service string) (port int, err error)
LookupPort looks up the port for the given network and service.
LookupPort 用给定的network 和 service 查找端口



func LookupSRV

func LookupSRV(service, proto, name string) (cname string, addrs []*SRV, err error)
LookupSRV tries to resolve an SRV query of the given service, protocol, and domain name. 
The proto is "tcp" or "udp". The returned records are sorted by priority and randomized by weight within a priority.

LookupSRV constructs the DNS name to look up following RFC 2782. 
That is, it looks up _service._proto.name. 
To accommodate services publishing SRV records under non-standard names, if both service and proto are empty strings, LookupSRV looks up name directly.
LookupSRV用给定的 service, protocol, 和 域名 name 尝试解析一个SRV查询
协议是"tcp" 或 "udp".返回的记录以优先级排列 并且优先级内按权重随机。
LookupSRV 构建DNS name 来查找,遵循RFC2782。
就是说, 它查找 _service._proto.name.
为了容纳服务在 非标准的公共SRV记录, 如果service和proto都是空字符串,LookupSRV直接查询 name



func LookupTXT

func LookupTXT(name string) (txt []string, err error)
LookupTXT returns the DNS TXT records for the given domain name.
LookupTXT用给定的 域名name 返回 DNS TXT记录



func SplitHostPort

func SplitHostPort(hostport string) (host, port string, err error)
SplitHostPort splits a network address of the form "host:port", "[host]:port" or "[ipv6-host%zone]:port" into host or ipv6-host%zone and port. 
A literal address or host name for IPv6 must be enclosed in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80".
SplitHostPort 拆分 "host:port","[host]:port" or "[ipv6-host%zone]:port" 成host 或ipv6-host%zone 成 port.
IPv6的一个文字地址或 host name必须 封闭在在方括号中,例如 "[::1]:80",  "[ipv6-host]:http" 或 "[ipv6-host%zone]:80".
 

type Addr

type Addr interface {
        Network() string // name of the network
        String() string  // string form of address
}
Addr represents a network end point address.
Addr表示一个 一个网络终端地址。



type AddrError

type AddrError struct {
        Err  string
        Addr string
}



func (*AddrError) Error

func (e *AddrError) Error() string



func (*AddrError) Temporary

func (e *AddrError) Temporary() bool



func (*AddrError) Timeout

func (e *AddrError) Timeout() bool



type Conn

type Conn interface {
        // Read reads data from the connection.
        // Read can be made to time out and return a Error with Timeout() == true
        // after a fixed time limit; see SetDeadline and SetReadDeadline.
        //Read 从连接中读取数据
        //Read 可超时 并且 返回一个Timeout() == true Error  在固定时间超过后.
           //查看 SetDeadline 和 SetReadDeadline.
        Read(b []byte) (n int, err error)

        // Write writes data to the connection.
        // Write can be made to time out and return a Error with Timeout() == true
        // after a fixed time limit; see SetDeadline and SetWriteDeadline.
        //Write 写数据到 连接
          //可超时 并且 返回一个Timeout() == true Error  在固定时间超过后.
        Write(b []byte) (n int, err error)


        // Close closes the connection.
        // Any blocked Read or Write operations will be unblocked and return errors.
        //Close关闭连接
           //任何Read 或 Write操作都会被阻塞 并 返回错误.
        Close() error


        // LocalAddr returns the local network address.
        //LocalAddr返回本地的网络地址
        LocalAddr() Addr


        // RemoteAddr returns the remote network address.
        //RemoteAddr 返回
        RemoteAddr() Addr远程网络地址。


        // SetDeadline sets the read and write deadlines associated
        // with the connection. It is equivalent to calling both
        // SetReadDeadline and SetWriteDeadline.
        //
        // A deadline is an absolute time after which I/O operations
        // fail with a timeout (see type Error) instead of
        // blocking. The deadline applies to all future I/O, not just
        // the immediately following call to Read or Write.
        //
        // An idle timeout can be implemented by repeatedly extending
        // the deadline after successful Read or Write calls.
        //
        // A zero value for t means I/O operations will not time out.
        
        //SetDeadline 设置 读和写关联的连接的期限.它等价于调用了SetReadDeadline and SetWriteDeadline两个.
          //最后期限是一个 因时间超时导致 I/O操作失败而不是因阻塞 的绝对时间
          //最后期限适用所有的I/O, 而不是随后调用的Read 或 Write.
          //一个空闲超时 可以在Read 或 Write 调用成功后 ,一再延迟后实现.
          //为t的零值 I/O 表示 将不会超时
        SetDeadline(t time.Time) error


        // SetReadDeadline sets the deadline for future Read calls.
        // A zero value for t means Read will not time out.
        
        //SetReadDeadline 设置 为未来的最后期间 Read 调用 
           //为t的零值 I/O 表示Read 将不会超时
        SetReadDeadline(t time.Time) error

        
        
        // SetWriteDeadline sets the deadline for future Write calls.
        // Even if write times out, it may return n > 0, indicating that
        // some of the data was successfully written.
        // A zero value for t means Write will not time out.
        //SetWriteDeadline 设置 为未来的最后期间 Write 调用 
          //即使如果写超时,它可能会返回 n>0,这表明一些数据写入成功
          //为t的零值 I/O 表示Write将不会超时
        
        SetWriteDeadline(t time.Time) error
}
Conn is a generic stream-oriented network connection.
Multiple goroutines may invoke methods on a Conn simultaneously.
Conn是一个通用的面向流的网络连接。
多个goroutines 可能同时调用一个Conn



func Dial

func Dial(network, address string) (Conn, error)
Dial connects to the address on the named network.
Known networks are "tcp", "tcp4" (IPv4-only), "tcp6" (IPv6-only), "udp", "udp4" (IPv4-only), "udp6" (IPv6-only), "ip", "ip4" (IPv4-only), "ip6" (IPv6-only), "unix", "unixgram" and "unixpacket".
For TCP and UDP networks, addresses have the form host:port. 
If host is a literal IPv6 address or host name, it must be enclosed in square brackets as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80". 
The functions JoinHostPort and SplitHostPort manipulate addresses in this form.
Dial连接 命名network 的地址.
已知的网络是 "tcp", "tcp4" (IPv4-only), "tcp6" (IPv6-only), "udp", "udp4" (IPv4-only), "udp6" (IPv6-only), "ip", "ip4" (IPv4-only), "ip6" (IPv6-only), "unix", "unixgram" and "unixpacket".
对于TCP 和 UDP 网络,地址的形式是 host:port. 
如果host是一个IPv6地址文字或host名,它必须被括在方括号"[::1]:80", "[ipv6-host]:http" 或 "[ipv6-host%zone]:80".
JoinHostPort和 SplitHostPort 函数是在这种形式操作的地址。

Examples:

Dial("tcp", "12.34.56.78:80")
Dial("tcp", "google.com:http")
Dial("tcp", "[2001:db8::1]:http")
Dial("tcp", "[fe80::1%lo0]:80")
For IP networks, the network must be "ip", "ip4" or "ip6" followed by a colon and a protocol number or name and the addr must be a literal IP address.
对于IP网络, 网络必须是 "ip", "ip4" 或 "ip6" 后面跟一个冒号 和 协议号或者 name 和 addr 必须是一个IP地址.

Examples:

Dial("ip4:1", "127.0.0.1")
Dial("ip6:ospf", "::1")
For Unix networks, the address must be a file system path.
对于Unix网络, 地址必须是一个文件系统路径



func DialTimeout

func DialTimeout(network, address string, timeout time.Duration) (Conn, error)
DialTimeout acts like Dial but takes a timeout. 
The timeout includes name resolution, if required.
DialTimeout 类似Dial 但是 有一个超时时间.
如果需要timeout 包含 name 解析.


func FileConn

func FileConn(f *os.File) (c Conn, err error)
FileConn returns a copy of the network connection corresponding to the open file f. 
It is the caller's responsibility to close f when finished. 
Closing c does not affect f, and closing f does not affect c.
FileConn 返回 网络连接对应的 打开的文件f 的复制
当结束时 调用者负责关闭f. 关闭c不会影响到f,关闭f也不会影响到c.


func Pipe

func Pipe() (Conn, Conn)
Pipe creates a synchronous, in-memory, full duplex network connection; 
both ends implement the Conn interface. 
Reads on one end are matched with writes on the other, copying data directly between the two; 
there is no internal buffering.
Pipe创建一个同步的, 内存的, 双工的 网络连接;
两端都实现Conn接口.
在一端读取要与在其他写入匹配,直接在两端复制数据.
没有内部缓冲区



type DNSConfigError

type DNSConfigError struct {
        Err error
}
DNSConfigError represents an error reading the machine's DNS configuration.
DNSConfigError 表示读取机器的DNS配置 错误


func (*DNSConfigError) Error

func (e *DNSConfigError) Error() string



func (*DNSConfigError) Temporary

func (e *DNSConfigError) Temporary() bool



func (*DNSConfigError) Timeout

func (e *DNSConfigError) Timeout() bool




type DNSError

type DNSError struct {
        Err       string // description of the error
        Name      string // name looked for
        Server    string // server used
        IsTimeout bool
}
DNSError represents a DNS lookup error.
DNSError表示一个DNS查询错误.



func (*DNSError) Error

func (e *DNSError) Error() string



func (*DNSError) Temporary

func (e *DNSError) Temporary() bool



func (*DNSError) Timeout

func (e *DNSError) Timeout() bool




type Dialer

type Dialer struct {
        // Timeout is the maximum amount of time a dial will wait for
        // a connect to complete. If Deadline is also set, it may fail
        // earlier.
        //
        // The default is no timeout.
        //
        // With or without a timeout, the operating system may impose
        // its own earlier timeout. For instance, TCP timeouts are
        // often around 3 minutes.
        
         //超时是最大 时间数 dial 将等待一个完整的连接.
         //如果Deadline 也设置, 它会更早的失败
         //默认是没有超时的.
         //带或不带 一个超时,操作系统可能会强制它更早的超时.例如,TCP的超时经常3分钟左右.
        Timeout time.Duration



        // Deadline is the absolute point in time after which dials
        // will fail. If Timeout is set, it may fail earlier.
        // Zero means no deadline, or dependent on the operating system
        // as with the Timeout option.
          //
          
        Deadline time.Time



        // LocalAddr is the local address to use when dialing an
        // address. The address must be of a compatible type for the
        // network being dialed.
        // If nil, a local address is automatically chosen.
        LocalAddr Addr



        // DualStack allows a single dial to attempt to establish
        // multiple IPv4 and IPv6 connections and to return the first
        // established connection when the network is "tcp" and the
        // destination is a host name that has multiple address family
        // DNS records.
        DualStack bool



        // KeepAlive specifies the keep-alive period for an active
        // network connection.
        // If zero, keep-alives are not enabled. Network protocols
        // that do not support keep-alives ignore this field.
        KeepAlive time.Duration
}
A Dialer contains options for connecting to an address.

The zero value for each field is equivalent to dialing without that option. 
Dialing with the zero value of Dialer is therefore equivalent to just calling the Dial function.
 一个Dialer 包含连接地址的选项.
每个字段的零值相当于dialing 没有那个选项.
带零值的Dialing 的 Dialer等价于只调用Dial函数.


func (*Dialer) Dial

func (d *Dialer) Dial(network, address string) (Conn, error)
Dial connects to the address on the named network.

See func Dial for a description of the network and address parameters.
Dial 连接到地址的命名network.
查看Dial函数 的network 和 address 参数描述



type Error

type Error interface {
        error
        Timeout() bool   // Is the error a timeout?
        Temporary() bool // Is the error temporary?
}
An Error represents a network error.
Error表示一个network错误.



type Flags

type Flags uint
const (
        FlagUp           Flags = 1 << iota // interface is up
        FlagBroadcast                      // interface supports broadcast access capability
        FlagLoopback                       // interface is a loopback interface
        FlagPointToPoint                   // interface belongs to a point-to-point link
        FlagMulticast                      // interface supports multicast access capability
)



func (Flags) String

func (f Flags) String() string



type HardwareAddr

type HardwareAddr []byte
A HardwareAddr represents a physical hardware address.
HardwareAddr表示一个物理硬件地址。



func ParseMAC

func ParseMAC(s string) (hw HardwareAddr, err error)
ParseMAC parses s as an IEEE 802 MAC-48, EUI-48, or EUI-64 using one of the following formats:
ParseMAC解析s 成 802 MAC-48, EUI-48, or EUI-64  使用以下的格式中的一种:


01:23:45:67:89:ab
01:23:45:67:89:ab:cd:ef
01-23-45-67-89-ab
01-23-45-67-89-ab-cd-ef
0123.4567.89ab
0123.4567.89ab.cdef



func (HardwareAddr) String

func (a HardwareAddr) String() string


type IP

type IP []byte
An IP is a single IP address, a slice of bytes. 
Functions in this package accept either 4-byte (IPv4) or 16-byte (IPv6) slices as input.

Note that in this documentation, referring to an IP address as an IPv4 address or an IPv6 address is a semantic property of the address, 
	not just the length of the byte slice: a 16-byte slice can still be an IPv4 address.
IP是单个IP地址, 一个字节 slice.
在这个包里的函数 接收 4字节(IPv4) 或 16字节 (IPv6)  slice的输入.
注意 在这个文档, 对于 IP地址  IPv4 或 IPv6 是一个地址的语义属性，不只是slice字节 长度: 一个16字节 slice 可以保留 一个 IPv4地址.




func IPv4

func IPv4(a, b, c, d byte) IP
IPv4 returns the IP address (in 16-byte form) of the IPv4 address a.b.c.d.
IPv4 返回 IPv4地址 a.b.c.d 的IP地址.(16字节形式)


func ParseCIDR

func ParseCIDR(s string) (IP, *IPNet, error)
ParseCIDR parses s as a CIDR notation IP address and mask, like "192.168.100.1/24" or "2001:DB8::/48", as defined in RFC 4632 and RFC 4291.

It returns the IP address and the network implied by the IP and mask. For example, ParseCIDR("192.168.100.1/16") returns the IP address 192.168.100.1 and the network 192.168.0.0/16.
ParseCIDR解析s 成 定义在RFC 4632 和 RFC 4291中的  一个CIDR 表示法的IP地址和mask,类似 "192.168.100.1/24" 或 "2001:DB8::/48",



func ParseIP

func ParseIP(s string) IP
ParseIP parses s as an IP address, returning the result. 
The string s can be in dotted decimal ("74.125.19.99") or IPv6 ("2001:4860:0:2001::68") form. 
If s is not a valid textual representation of an IP address, ParseIP returns nil.
ParseIP解析s成一个IP地址 返回结果.
字符串s可以是 点 十进制("74.125.19.99")  或 IPv6 ("2001:4860:0:2001::68")  形式.
如果s不是一个有效的IP地址表示,ParseIP 返回nil




func (IP) DefaultMask

func (ip IP) DefaultMask() IPMask
DefaultMask returns the default IP mask for the IP address ip. 
Only IPv4 addresses have default masks; DefaultMask returns nil if ip is not a valid IPv4 address.
DefaultMask 返回 IP地址的默认 IP mask.
只有 IPv4 有默认的 masks.如果ip不是一个有效的 IPv4地址, 返回nil



func (IP) Equal

func (ip IP) Equal(x IP) bool
Equal returns true if ip and x are the same IP address. 
An IPv4 address and that same address in IPv6 form are considered to be equal.
Equal如果ip 和x是同一个IP地址, 返回true
一个 IPv4地址和它同样地址的IPv6形式 是会相等的.




func (IP) IsGlobalUnicast

func (ip IP) IsGlobalUnicast() bool
IsGlobalUnicast returns true if ip is a global unicast address.
如果ip是一个全局的单播地址,返回true.



func (IP) IsInterfaceLocalMulticast

func (ip IP) IsInterfaceLocalMulticast() bool
IsInterfaceLinkLocalMulticast returns true if ip is an interface-local multicast address.
如果ip是一个接口本地多播地址, 返回true.



func (IP) IsLinkLocalMulticast

func (ip IP) IsLinkLocalMulticast() bool
IsLinkLocalMulticast returns true if ip is a link-local multicast address.
IsLinkLocalMulticast 如果 ip是一个 本地连接多播地址, 返回true


func (IP) IsLinkLocalUnicast

func (ip IP) IsLinkLocalUnicast() bool
IsLinkLocalUnicast returns true if ip is a link-local unicast address.
如果 ip是一个 本地连接单播地址, 返回true


func (IP) IsLoopback

func (ip IP) IsLoopback() bool
IsLoopback returns true if ip is a loopback address.
如果ip 是一个环回地址,返回true



func (IP) IsMulticast

func (ip IP) IsMulticast() bool
IsMulticast returns true if ip is a multicast address.
如果ip是一个多播地址 返回true



func (IP) IsUnspecified

func (ip IP) IsUnspecified() bool
IsUnspecified returns true if ip is an unspecified address.
如果ip是一个未指定地址 返回true



func (IP) MarshalText

func (ip IP) MarshalText() ([]byte, error)
MarshalText implements the encoding.TextMarshaler interface. The encoding is the same as returned by String.
MarshalText 实现encoding.TextMarshaler 接口.  编码和返回的String一样



func (IP) Mask

func (ip IP) Mask(mask IPMask) IP
Mask returns the result of masking the IP address ip with mask.
Mask返回 IP地址的ip和mask 的 结果



func (IP) String

func (ip IP) String() string
String returns the string form of the IP address ip. 
If the address is an IPv4 address, the string representation is dotted decimal ("74.125.19.99"). 
Otherwise the representation is IPv6 ("2001:4860:0:2001::68").
String返回IP地址ip 的字符串形式.
如果地址是IPv4地址,字符串表示点 十进制 ("74.125.19.99"). 
否则表示 是 IPv6 ("2001:4860:0:2001::68").



func (IP) To16

func (ip IP) To16() IP
To16 converts the IP address ip to a 16-byte representation. 
If ip is not an IP address (it is the wrong length), To16 returns nil.
To16转化IP地址ip成一个 16位表示.
如果ip不是一个IP地址,To16返回nil.


func (IP) To4

func (ip IP) To4() IP
To4 converts the IPv4 address ip to a 4-byte representation. 
If ip is not an IPv4 address, To4 returns nil.
To4转化IPv4地址ip成一个 4位表示.
如果ip不是一个IPv4地址,To4返回nil.



func (*IP) UnmarshalText

func (ip *IP) UnmarshalText(text []byte) error
UnmarshalText implements the encoding.TextUnmarshaler interface. 
The IP address is expected in a form accepted by ParseIP.
UnmarshalText 实现 encoding.TextUnmarshaler接口.
IP地址 是预计由ParseIP接受的一种形式。



type IPAddr

type IPAddr struct {
        IP   IP
        Zone string // IPv6 scoped addressing zone
}
IPAddr represents the address of an IP end point.
IPAddr表示IP终端地址



func ResolveIPAddr

func ResolveIPAddr(net, addr string) (*IPAddr, error)
ResolveIPAddr parses addr as an IP address of the form "host" or "ipv6-host%zone" and resolves the domain name on the network net, which must be "ip", "ip4" or "ip6".
ResolveIPAddr解析 addr 成IP地址形式  "host" 或 "ipv6-host%zone" 并且解析网络上的域名,必须是 "ip", "ip4" 或 "ip6".


func (*IPAddr) Network

func (a *IPAddr) Network() string
Network returns the address's network name, "ip".
Network返回地址的网络名称 ,"ip".


func (*IPAddr) String

func (a *IPAddr) String() string



type IPConn

type IPConn struct {
        // contains filtered or unexported fields
}
IPConn is the implementation of the Conn and PacketConn interfaces for IP network connections.
IPConn实现 IP 网络连接  Conn和 PacketConn接口


func DialIP

func DialIP(netProto string, laddr, raddr *IPAddr) (*IPConn, error)
DialIP connects to the remote address raddr on the network protocol netProto, which must be "ip", "ip4", or "ip6" followed by a colon and a protocol number or name.
DialIP连接 网络协议netProto 的 远程地址 raddr ,必须是"ip", "ip4", or "ip6" 后面跟着一个冒号 或一个协议号 或 名字


func ListenIP

func ListenIP(netProto string, laddr *IPAddr) (*IPConn, error)
ListenIP listens for incoming IP packets addressed to the local address laddr. 
The returned connection's ReadFrom and WriteTo methods can be used to receive and send IP packets with per-packet addressing.
ListenIP 监听 本地地址laddr传入的IP数据包.
返回的连接ReadFrom 和WriteTo 方法 可以用来接收和发送IP每个数据包的寻址数据。



func (*IPConn) Close

func (c *IPConn) Close() error
Close closes the connection.
关闭连接



func (*IPConn) File

func (c *IPConn) File() (f *os.File, err error)
File sets the underlying os.File to blocking mode and returns a copy. 
It is the caller's responsibility to close f when finished. Closing c does not affect f, and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.
File设置底层os.File 成阻塞模式 并返回一个复制.
结束的时候调用者负责关闭f.关闭c不影响f , 关闭f不影响c.

返回的 os.File的 文件描述符和连接的不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果。



func (*IPConn) LocalAddr

func (c *IPConn) LocalAddr() Addr
LocalAddr returns the local network address.
LocalAddr返回 本地网络地址


func (*IPConn) Read

func (c *IPConn) Read(b []byte) (int, error)
Read implements the Conn Read method.
实现Conn Read 方法


func (*IPConn) ReadFrom

func (c *IPConn) ReadFrom(b []byte) (int, Addr, error)
ReadFrom implements the PacketConn ReadFrom method.
ReadFrom实现PacketConn 的ReadFrom 方法



func (*IPConn) ReadFromIP

func (c *IPConn) ReadFromIP(b []byte) (int, *IPAddr, error)
ReadFromIP reads an IP packet from c, copying the payload into b. 
It returns the number of bytes copied into b and the return address that was on the packet.

ReadFromIP can be made to time out and return an error with Timeout() == true after a fixed time limit; see SetDeadline and SetReadDeadline.
ReadFromIP 从c中读取IP数据包, 复制有效载荷到b.
它返回复制到b的字节数,和 数据包里的地址.
ReadFromIP可以导致超时 并 在固定时间限制之后返回一个错误 Timeout() == true



func (*IPConn) ReadMsgIP

func (c *IPConn) ReadMsgIP(b, oob []byte) (n, oobn, flags int, addr *IPAddr, err error)
ReadMsgIP reads a packet from c, copying the payload into b and the associated out-of-band data into oob. 
It returns the number of bytes copied into b, the number of bytes copied into oob, the flags that were set on the packet and the source address of the packet.
ReadMsgIP从c中读取数据包, 复制有效载荷到b 和 对应的 数据到 oob.
它返回复制到b的字节数,和 复制到oob的字节数,数据包里设置的标示 和数据包的源地址



func (*IPConn) RemoteAddr

func (c *IPConn) RemoteAddr() Addr
RemoteAddr returns the remote network address.
RemoteAddr返回 远程网络地址


func (*IPConn) SetDeadline

func (c *IPConn) SetDeadline(t time.Time) error
SetDeadline implements the Conn SetDeadline method.
SetDeadline实现Conn SetDeadline 方法.



func (*IPConn) SetReadBuffer

func (c *IPConn) SetReadBuffer(bytes int) error
SetReadBuffer sets the size of the operating system's receive buffer associated with the connection.
SetReadBuffer 设置操作系统的接收缓冲关联的连接的大小



func (*IPConn) SetReadDeadline

func (c *IPConn) SetReadDeadline(t time.Time) error
SetReadDeadline implements the Conn SetReadDeadline method.
SetReadDeadline 实现Conn SetReadDeadline  方法



func (*IPConn) SetWriteBuffer

func (c *IPConn) SetWriteBuffer(bytes int) error
SetWriteBuffer sets the size of the operating system's transmit buffer associated with the connection.
SetWriteBuffer 设置操作系统的发送缓冲关联的连接的大小



func (*IPConn) SetWriteDeadline

func (c *IPConn) SetWriteDeadline(t time.Time) error
SetWriteDeadline implements the Conn SetWriteDeadline method.
SetWriteDeadline实现Conn SetWriteDeadline  方法


func (*IPConn) Write

func (c *IPConn) Write(b []byte) (int, error)
Write implements the Conn Write method.
Write实现Conn Write方法


func (*IPConn) WriteMsgIP

func (c *IPConn) WriteMsgIP(b, oob []byte, addr *IPAddr) (n, oobn int, err error)
WriteMsgIP writes a packet to addr via c, copying the payload from b and the associated out-of-band data from oob. 
It returns the number of payload and out-of-band bytes written.
WriteMsgIP 通过c写 包数据到addr,从b复制有效载荷 和从oob 复制有关 数据.
返回有效有效载荷数 和 写入字节



func (*IPConn) WriteTo

func (c *IPConn) WriteTo(b []byte, addr Addr) (int, error)
WriteTo implements the PacketConn WriteTo method.
WriteTo实现PacketConn 的 WriteTo方法



func (*IPConn) WriteToIP

func (c *IPConn) WriteToIP(b []byte, addr *IPAddr) (int, error)
WriteToIP writes an IP packet to addr via c, copying the payload from b.
WriteToIP can be made to time out and return an error with Timeout() == true after a fixed time limit; 
see SetDeadline and SetWriteDeadline. On packet-oriented connections, write timeouts are rare.
WriteToIP 从b复制有效载荷,通过 c 写入一个 IP数据包到 addr.
WriteToIP 可超时 并且 在固定时间限制后 返回一个  Timeout() == true 错误.
查看 SetDeadline 和 SetWriteDeadline. 在面向数据包的连接， 写超时是罕见的。



type IPMask

type IPMask []byte
An IP mask is an IP address.
一个IP掩码是一个IP地址。



func CIDRMask

func CIDRMask(ones, bits int) IPMask
CIDRMask returns an IPMask consisting of `ones' 1 bits followed by 0s up to a total length of `bits' bits. 
For a mask of this form, CIDRMask is the inverse of IPMask.Size.
CIDRMask 返回一个 由 IPMask组成的`ones'  1位后面跟着 0s 到总长度 `bits' 位.
对于一个这种形式的掩码,CIDRMask 是IPMask.Size的逆.



func IPv4Mask

func IPv4Mask(a, b, c, d byte) IPMask
IPv4Mask returns the IP mask (in 4-byte form) of the IPv4 mask a.b.c.d.
IPv4Mask 返回 IP掩码(4字节的形式)的 IPv4   a.b.c.d.



func (IPMask) Size

func (m IPMask) Size() (ones, bits int)
Size returns the number of leading ones and total bits in the mask. 
If the mask is not in the canonical form--ones followed by zeros--then Size returns 0, 0.
Size 返回 ones 数 和 掩码里的 所有 位.



func (IPMask) String

func (m IPMask) String() string
String returns the hexadecimal form of m, with no punctuation.
String 返回十进制形式的m,没有标点符号。



type IPNet

type IPNet struct {
        IP   IP     // network number
        Mask IPMask // network mask
}
An IPNet represents an IP network.
IPNet 表示一个 IP 网络.



func (*IPNet) Contains

func (n *IPNet) Contains(ip IP) bool
Contains reports whether the network includes ip.
Contains 报告 网络是否包含 ip.



func (*IPNet) Network

func (n *IPNet) Network() string
Network returns the address's network name, "ip+net".
Network 返回地址的网络名称 ,"ip+net".



func (*IPNet) String

func (n *IPNet) String() string
String returns the CIDR notation of n like "192.168.100.1/24" or "2001:DB8::/48" as defined in RFC 4632 and RFC 4291. 
If the mask is not in the canonical form, it returns the string which consists of an IP address, followed by a slash character and a mask expressed as hexadecimal form with no punctuation like "192.168.100.1/c000ff00".
String 返回 定义在 RFC 4632 和 RFC 4291的  CIDR的符号 n 类似  "192.168.100.1/24" 或"2001:DB8::/48" 
如果掩码 没有在规范形式里,它返回 由IP地址 的字符串, 跟着一个 斜线字符和一个 没有标点符号的十进制形式的掩码  如:"192.168.100.1/c000ff00".



type Interface

type Interface struct {
        Index        int          // positive integer that starts at one, zero is never used
        MTU          int          // maximum transmission unit
        Name         string       // e.g., "en0", "lo0", "eth0.100"
        HardwareAddr HardwareAddr // IEEE MAC-48, EUI-48 and EUI-64 form
        Flags        Flags        // e.g., FlagUp, FlagLoopback, FlagMulticast
}
Interface represents a mapping between network interface name and index. 
It also represents network interface facility information.
Interface 表示一个网络接口名称和索引 的映射.
它也表示 网络接口设施信息.



func InterfaceByIndex

func InterfaceByIndex(index int) (*Interface, error)
InterfaceByIndex returns the interface specified by index.
InterfaceByIndex 返回由index 指定的接口



func InterfaceByName

func InterfaceByName(name string) (*Interface, error)
InterfaceByName returns the interface specified by name.
返回由name 指定的接口



func (*Interface) Addrs

func (ifi *Interface) Addrs() ([]Addr, error)
Addrs returns interface addresses for a specific interface.
Addrs 为一个指定的接口返回接口地址



func (*Interface) MulticastAddrs

func (ifi *Interface) MulticastAddrs() ([]Addr, error)
MulticastAddrs returns multicast, joined group addresses for a specific interface.
MulticastAddrs 返回多播, 为一个指定的接口加入组地址



type InvalidAddrError

type InvalidAddrError string




func (InvalidAddrError) Error

func (e InvalidAddrError) Error() string




func (InvalidAddrError) Temporary

func (e InvalidAddrError) Temporary() bool




func (InvalidAddrError) Timeout

func (e InvalidAddrError) Timeout() bool




type Listener

type Listener interface {
        // Accept waits for and returns the next connection to the listener.
        Accept() (c Conn, err error)

        // Close closes the listener.
        // Any blocked Accept operations will be unblocked and return errors.
        Close() error

        // Addr returns the listener's network address.
        Addr() Addr
}
A Listener is a generic network listener for stream-oriented protocols.

Multiple goroutines may invoke methods on a Listener simultaneously.
Listener 是一个通用的网络监听面向流的协议。
Multiple goroutines 可能会同时调用一个 Listener



Example:
package main

import (
	"io"
	"log"
	"net"
)

func main() {
	// Listen on TCP port 2000 on all interfaces.
	l, err := net.Listen("tcp", ":2000")
	if err != nil {
		log.Fatal(err)
	}
	defer l.Close()
	for {
		// Wait for a connection.
		conn, err := l.Accept()
		if err != nil {
			log.Fatal(err)
		}
		// Handle the connection in a new goroutine.
		// The loop then returns to accepting, so that
		// multiple connections may be served concurrently.
		go func(c net.Conn) {
			// Echo all incoming data.
			io.Copy(c, c)
			// Shut down the connection.
			c.Close()
		}(conn)
	}
}



func FileListener

func FileListener(f *os.File) (l Listener, err error)
FileListener returns a copy of the network listener corresponding to the open file f. 
It is the caller's responsibility to close l when finished. 
Closing l does not affect f, and closing f does not affect l.
FileListener返回一个 网络 监听对应的 打开的文件f 的复制
当结束时它的调用者负责关闭l
关闭l不影响f ,关闭f不影响l



func Listen

func Listen(net, laddr string) (Listener, error)
Listen announces on the local network address laddr. 
The network net must be a stream-oriented network: "tcp", "tcp4", "tcp6", "unix" or "unixpacket". 
See Dial for the syntax of laddr.
Listen声明在本地网络地址  laddr
网络 net 必须是一个面向流的网络 : "tcp", "tcp4", "tcp6", "unix" or "unixpacket". 
查看Dial 中的 laddr语法




type MX

type MX struct {
        Host string
        Pref uint16
}
An MX represents a single DNS MX record.
MX 表示 单个DNS MX  记录



type NS

type NS struct {
        Host string
}
An NS represents a single DNS NS record.
表示单个 DNS NS  记录



type OpError

type OpError struct {
        // Op is the operation which caused the error, such as
        // "read" or "write".
        //Op 是能引起错误的 操作,例如 "read" 或 "write".
        Op string


        // Net is the network type on which this error occurred,
        // such as "tcp" or "udp6".
        //Net 是发生此错误的网络类型,例如 "tcp" 或 "udp6".
        Net string


        // Addr is the network address on which this error occurred.
        //Addr 是在发生此错误的网络地址
        Addr Addr


        // Err is the error that occurred during the operation.
        //Err 这个错误遇到的 的 操作期间
        Err error
}
OpError is the error type usually returned by functions in the net package. 
It describes the operation, network type, and address of an error.
OpError 是 通常 经过  net包里的函数返回的错误类型.
它描述操作,网络类型 和错误地址.


func (*OpError) Error

func (e *OpError) Error() string



func (*OpError) Temporary

func (e *OpError) Temporary() bool



func (*OpError) Timeout

func (e *OpError) Timeout() bool



type PacketConn

type PacketConn interface {
        // ReadFrom reads a packet from the connection,
        // copying the payload into b.  It returns the number of
        // bytes copied into b and the return address that
        // was on the packet.
        // ReadFrom can be made to time out and return
        // an error with Timeout() == true after a fixed time limit;
        // see SetDeadline and SetReadDeadline.
        
        //ReadFrom 从连接读取数据包,复制有效载荷到b.
           //它返回复制到b的字节数并且返回数据包的那个地址.
        //ReadFrom 会超时 并且在固定的时间限制后 返回 一个Timeout() == true 错误
           //查看SetDeadline and SetReadDeadline.
        
        ReadFrom(b []byte) (n int, addr Addr, err error)



        // WriteTo writes a packet with payload b to addr.
        // WriteTo can be made to time out and return
        // an error with Timeout() == true after a fixed time limit;
        // see SetDeadline and SetWriteDeadline.
        // On packet-oriented connections, write timeouts are rare.
        
        //WriteTo 写数据包载荷 b 到 addr.
        //WriteTo会超时 并且在固定的时间限制后 返回 一个Timeout() == true 错误
           // 查看 SetDeadline and SetWriteDeadline.
           //在面向数据包的连接中, 写超时是罕见的.
        WriteTo(b []byte, addr Addr) (n int, err error)



        // Close closes the connection.
        // Any blocked ReadFrom or WriteTo operations will be unblocked and return errors.
        //Close 关闭连接.
           //任何阻塞ReadFrom或 WriteTo 操作的 将会 不阻塞  并返回错误.
        Close() error



        // LocalAddr returns the local network address.
        //LocalAddr 返回本地网络地址
        LocalAddr() Addr



        // SetDeadline sets the read and write deadlines associated
        // with the connection.
        //SetDeadline 设置与该连接相关联的读取和写入的最后期限。
        SetDeadline(t time.Time) error



        // SetReadDeadline sets the deadline for future Read calls.
        // If the deadline is reached, Read will fail with a timeout
        // (see type Error) instead of blocking.
        // A zero value for t means Read will not time out.
        //SetReadDeadline 为未来的Read调用 设置 最后期限
          //如果 最后期限到了,Read 会失败 并 一个超过会代替阻塞.
          //零值的t 意味着Read 不会超时
        SetReadDeadline(t time.Time) error

        // SetWriteDeadline sets the deadline for future Write calls.
        // If the deadline is reached, Write will fail with a timeout
        // (see type Error) instead of blocking.
        // A zero value for t means Write will not time out.
        // Even if write times out, it may return n > 0, indicating that
        // some of the data was successfully written.
        //SetWriteDeadline 为 未来的Write 调用 设置最后期限
           //如果最后期限到了,Write会失败 并 一个超过会代替阻塞.
          //零值的t 意味着Write 不会超时
          //即使如果写超时了, 它可能会返回 n > 0,这意味者 有一些数据写入成功
        SetWriteDeadline(t time.Time) error
}
PacketConn is a generic packet-oriented network connection.

Multiple goroutines may invoke methods on a PacketConn simultaneously.
PacketConn通常是面向数据包的网络连接.
多个 goroutines 可能会同时调用一个PacketConn



func FilePacketConn

func FilePacketConn(f *os.File) (c PacketConn, err error)
FilePacketConn returns a copy of the packet network connection corresponding to the open file f. 
It is the caller's responsibility to close f when finished. 
Closing c does not affect f, and closing f does not affect c.
FilePacketConn  返回 网络连接对应的 打开的文件f 的复制
当结束时它的调用者负责关闭f.
关闭c不会影响f, 关闭f 不会影响c



func ListenPacket

func ListenPacket(net, laddr string) (PacketConn, error)
ListenPacket announces on the local network address laddr. 
The network net must be a packet-oriented network: "udp", "udp4", "udp6", "ip", "ip4", "ip6" or "unixgram". 
See Dial for the syntax of laddr.
ListenPacket 声明在本地网络地址上的 laddr
网络必须是面向数据包的网络:"udp", "udp4", "udp6", "ip", "ip4", "ip6" or "unixgram".
查看Dial 里的laddr 语法.



type ParseError

type ParseError struct {
        Type string
        Text string
}
A ParseError represents a malformed text string and the type of string that was expected.
一个ParseError 表示一个 异常文本字符串 和字符串的预期的类型。



func (*ParseError) Error

func (e *ParseError) Error() string



type SRV

type SRV struct {
        Target   string
        Port     uint16
        Priority uint16
        Weight   uint16
}
An SRV represents a single DNS SRV record.
一个SRV 表示 单个的DNS SRV 记录.



type TCPAddr

type TCPAddr struct {
        IP   IP
        Port int
        Zone string // IPv6 scoped addressing zone
}
TCPAddr represents the address of a TCP end point.
TCPAddr 表示 TCP终端的地址.



func ResolveTCPAddr

func ResolveTCPAddr(net, addr string) (*TCPAddr, error)
ResolveTCPAddr parses addr as a TCP address of the form "host:port" or "[ipv6-host%zone]:port" and resolves a pair of domain name and port name on the network net, which must be "tcp", "tcp4" or "tcp6".
A literal address or host name for IPv6 must be enclosed in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80".
ResolveTCPAddr 解析一个addr 成一个TCP地址形式host:port"或 "[ipv6-host%zone]:port" 并 解析网络net上的域名和端口对 , 必须是"tcp", "tcp4" or "tcp6".
一个文本地址 或 host 的IPv6 必须括在方括号,"[::1]:80", "[ipv6-host]:http" 或 "[ipv6-host%zone]:80".



func (*TCPAddr) Network

func (a *TCPAddr) Network() string
Network returns the address's network name, "tcp".
Network 返回 地址的网络名称, "tcp".



func (*TCPAddr) String

func (a *TCPAddr) String() string



type TCPConn

type TCPConn struct {
        // contains filtered or unexported fields
}
TCPConn is an implementation of the Conn interface for TCP network connections.
TCPConn 是一个 实现了 Conn 接口 的TCP 网络连接



func DialTCP

func DialTCP(net string, laddr, raddr *TCPAddr) (*TCPConn, error)
DialTCP connects to the remote address raddr on the network net, which must be "tcp", "tcp4", or "tcp6". 
If laddr is not nil, it is used as the local address for the connection.
DialTCP 连接网络net上 的连接远程地址raddr, 必须是"tcp", "tcp4", 或 "tcp6".
如果 laddr 是 非nil , 它使用本地地址来连接




func (*TCPConn) Close

func (c *TCPConn) Close() error
Close closes the connection.
Close 关闭连接



func (*TCPConn) CloseRead

func (c *TCPConn) CloseRead() error
CloseRead shuts down the reading side of the TCP connection. 
Most callers should just use Close.
CloseRead 关闭读取端的TCP连接.
大部分的调用只能用 Close



func (*TCPConn) CloseWrite

func (c *TCPConn) CloseWrite() error
CloseWrite shuts down the writing side of the TCP connection. Most callers should just use Close.
CloseWrite 关闭写入端的TCP连接.
大部分的调用只能用 Close



func (*TCPConn) File

func (c *TCPConn) File() (f *os.File, err error)
File sets the underlying os.File to blocking mode and returns a copy. 
It is the caller's responsibility to close f when finished. Closing c does not affect f, and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.
File设置底层os.File 成阻塞模式 并返回一个复制.
结束的时候调用者负责关闭f.关闭c不影响f , 关闭f不影响c.

返回的 os.File的 文件描述符和连接的不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果。


func (*TCPConn) LocalAddr

func (c *TCPConn) LocalAddr() Addr
LocalAddr returns the local network address.
LocalAddr 返回本地网络地址



func (*TCPConn) Read

func (c *TCPConn) Read(b []byte) (int, error)
Read implements the Conn Read method.
Read 实现 Conn Read 方法.



func (*TCPConn) ReadFrom

func (c *TCPConn) ReadFrom(r io.Reader) (int64, error)
ReadFrom implements the io.ReaderFrom ReadFrom method.
ReadFrom 实现io.ReaderFrom ReadFrom 方法



func (*TCPConn) RemoteAddr

func (c *TCPConn) RemoteAddr() Addr
RemoteAddr returns the remote network address.
RemoteAddr 返回远程 网络地址



func (*TCPConn) SetDeadline

func (c *TCPConn) SetDeadline(t time.Time) error
SetDeadline implements the Conn SetDeadline method.
SetDeadline 实现Conn SetDeadlin 方法



func (*TCPConn) SetKeepAlive

func (c *TCPConn) SetKeepAlive(keepalive bool) error
SetKeepAlive sets whether the operating system should send keepalive messages on the connection.
SetKeepAlive 设置 操作系统 是否 应该保持 发送信息的连接.



func (*TCPConn) SetKeepAlivePeriod

func (c *TCPConn) SetKeepAlivePeriod(d time.Duration) error
SetKeepAlivePeriod sets period between keep alives.
SetKeepAlivePeriod设置保持有效指示之间的时期。



func (*TCPConn) SetLinger

func (c *TCPConn) SetLinger(sec int) error
SetLinger sets the behavior of Close on a connection which still has data waiting to be sent or to be acknowledged.

If sec < 0 (the default), the operating system finishes sending the data in the background.

If sec == 0, the operating system discards any unsent or unacknowledged data.

If sec > 0, the data is sent in the background as with sec < 0. On some operating systems after sec seconds have elapsed any remaining unsent data may be discarded.
SetLinger设置连接 Close 的行为 , 保持有数据等待发送或进行确认
如果 sec < 0(默认),操作系统在后台结束发送数据.
如果sec == 0,操作系统 丢弃任何 未发送或未确认数据.
如果 sec > 0, 数据在后台发送 类似sec < 0. 在一些操作系统,在sec 秒后 任何剩余的未发送的数据可能会被丢弃.



func (*TCPConn) SetNoDelay

func (c *TCPConn) SetNoDelay(noDelay bool) error
SetNoDelay controls whether the operating system should delay packet transmission in hopes of sending fewer packets (Nagle's algorithm). 
The default is true (no delay), meaning that data is sent as soon as possible after a Write.
SetNoDelay 控制 是否 操作系统 在发送较小数据包的时候应该 延迟发送数据包(内格尔算法).
默认是true(不延迟),意味着数据在写入后 尽可能快的发送.



func (*TCPConn) SetReadBuffer

func (c *TCPConn) SetReadBuffer(bytes int) error
SetReadBuffer sets the size of the operating system's receive buffer associated with the connection.
SetReadBuffer 设置操作系统的与该连接相关的接收缓冲器的大小。



func (*TCPConn) SetReadDeadline

func (c *TCPConn) SetReadDeadline(t time.Time) error
SetReadDeadline implements the Conn SetReadDeadline method.
SetReadDeadline实现Conn SetReadDeadline 方法



func (*TCPConn) SetWriteBuffer

func (c *TCPConn) SetWriteBuffer(bytes int) error
SetWriteBuffer sets the size of the operating system's transmit buffer associated with the connection.
SetWriteBuffer 设置与该连接相关联的操作系统的发送缓冲区的大小。



func (*TCPConn) SetWriteDeadline

func (c *TCPConn) SetWriteDeadline(t time.Time) error
SetWriteDeadline implements the Conn SetWriteDeadline method.
SetWriteDeadline实现Conn SetWriteDeadline  方法



func (*TCPConn) Write

func (c *TCPConn) Write(b []byte) (int, error)
Write implements the Conn Write method.
Write 实现Conn Write  方法



type TCPListener

type TCPListener struct {
        // contains filtered or unexported fields
}
TCPListener is a TCP network listener. 
Clients should typically use variables of type Listener instead of assuming TCP.
TCPListener 是一个TCP网络监听.
客户端通常应该使用Listener 变量类型 而不是TCP.



func ListenTCP

func ListenTCP(net string, laddr *TCPAddr) (*TCPListener, error)
ListenTCP announces on the TCP address laddr and returns a TCP listener.
Net must be "tcp", "tcp4", or "tcp6". If laddr has a port of 0, ListenTCP will choose an available port. 
The caller can use the Addr method of TCPListener to retrieve the chosen address.
ListenTCP 声明TCP 地址laddr 并且返回 一个TCP 监听.
Net 必须是 "tcp", "tcp4", or "tcp6".  如果laddr的端口是0, ListenTCP 将会选择一个可用的端口.
调用者 可以使用 TCPListener的Addr方法检索所选择的地址。



func (*TCPListener) Accept

func (l *TCPListener) Accept() (Conn, error)
Accept implements the Accept method in the Listener interface; 
it waits for the next call and returns a generic Conn.
Accept 实现Listener接口里的Accept 方法.
它等待下一个调用 并返回通用的Conn



func (*TCPListener) AcceptTCP

func (l *TCPListener) AcceptTCP() (*TCPConn, error)
AcceptTCP accepts the next incoming call and returns the new connection.
AcceptTCP 接收 下一个调用 并返回 新的连接.



func (*TCPListener) Addr

func (l *TCPListener) Addr() Addr
Addr returns the listener's network address, a *TCPAddr.
Addr返回监听的网络地址,一个 *TCPAddr.



func (*TCPListener) Close

func (l *TCPListener) Close() error
Close stops listening on the TCP address. Already Accepted connections are not closed.
Close 停止监听TCP地址.已经 Accepted 连接不会关闭.



func (*TCPListener) File

func (l *TCPListener) File() (f *os.File, err error)
File returns a copy of the underlying os.File, set to blocking mode. 
It is the caller's responsibility to close f when finished. 
Closing l does not affect f, and closing f does not affect l.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.
File返回底层os.File复制,设置为阻塞模式。
当结束的时候它的调用者负责关闭f.
关闭l不会影响f , 关闭f 不会影响l
返回的os.File 的文件 描述符 和 连接的 不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果。


func (*TCPListener) SetDeadline

func (l *TCPListener) SetDeadline(t time.Time) error
SetDeadline sets the deadline associated with the listener. 
A zero time value disables the deadline.
SetDeadline 设置和监听有关的 最后期限
零时间值禁用的最后期限。



type UDPAddr

type UDPAddr struct {
        IP   IP
        Port int
        Zone string // IPv6 scoped addressing zone
}
UDPAddr represents the address of a UDP end point.
UDPAddr 表示 UDP终端的地址



func ResolveUDPAddr

func ResolveUDPAddr(net, addr string) (*UDPAddr, error)
ResolveUDPAddr parses addr as a UDP address of the form "host:port" or "[ipv6-host%zone]:port" and resolves a pair of domain name and port name on the network net, which must be "udp", "udp4" or "udp6". 
A literal address or host name for IPv6 must be enclosed in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80".
ResolveUDPAddr 解析 addr 成 UDP 形式的地址"host:port"或"[ipv6-host%zone]:port" 并 解析网络 net 上的 域名和端口对,必须是"udp", "udp4" or "udp6". 
IPv6的一个文字地址或 host name必须 封闭在在方括号中,例如 "[::1]:80",  "[ipv6-host]:http" 或 "[ipv6-host%zone]:80".



func (*UDPAddr) Network

func (a *UDPAddr) Network() string
Network returns the address's network name, "udp".
Network返回地址的网络名称，"UDP"。



func (*UDPAddr) String

func (a *UDPAddr) String() string



type UDPConn

type UDPConn struct {
        // contains filtered or unexported fields
}
UDPConn is the implementation of the Conn and PacketConn interfaces for UDP network connections.
UDPConn 是实现Conn 和 PacketConn 接口 的UDP 网络连接.



func DialUDP

func DialUDP(net string, laddr, raddr *UDPAddr) (*UDPConn, error)
DialUDP connects to the remote address raddr on the network net, which must be "udp", "udp4", or "udp6". 
If laddr is not nil, it is used as the local address for the connection.
DialUDP 连接 网络net上的远程地址 raddr , 必须 是"udp", "udp4", or "udp6".
如果 laddr 是 非nil,它被用作本地地址进行连接。



func ListenMulticastUDP

func ListenMulticastUDP(net string, ifi *Interface, gaddr *UDPAddr) (*UDPConn, error)
ListenMulticastUDP listens for incoming multicast UDP packets addressed to the group address gaddr on ifi, which specifies the interface to join. 
ListenMulticastUDP uses default multicast interface if ifi is nil.
ListenMulticastUDP 监听 在ifi 上  传入多播UDP 数据包 地址 到组地址 gaddr,它指定加入的接口。
ListenMulticastUDP 如果ifi 是nil 使用默认的 多播接口.



func ListenUDP

func ListenUDP(net string, laddr *UDPAddr) (*UDPConn, error)
ListenUDP listens for incoming UDP packets addressed to the local address laddr. 
Net must be "udp", "udp4", or "udp6". If laddr has a port of 0, ListenUDP will choose an available port. 
The LocalAddr method of the returned UDPConn can be used to discover the port. 
The returned connection's ReadFrom and WriteTo methods can be used to receive and send UDP packets with per-packet addressing.
ListenUDP 侦听传入的UDP数据包寻址到本地地址LADDR。
Net必须是"udp", "udp4", 或 "udp6". 如果  laddr 有一个0端口,ListenUDP 会选择可用的端口.
返回的UDPConn 的LocalAddr 方法 可以用来覆盖端口.
返回的连接的ReadFrom 和WriteTo 方法 的每个数据包的寻址 可以用来接收和发送UDP 数据包




func (*UDPConn) Close

func (c *UDPConn) Close() error
Close closes the connection.
关闭连接



func (*UDPConn) File

func (c *UDPConn) File() (f *os.File, err error)
File sets the underlying os.File to blocking mode and returns a copy. 
It is the caller's responsibility to close f when finished. 
Closing c does not affect f, and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.
File设置底层os.File 成阻塞模式 并返回一个复制.
结束的时候调用者负责关闭f.关闭c不影响f , 关闭f不影响c.
返回的 os.File的 文件描述符和连接的不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果。



func (*UDPConn) LocalAddr

func (c *UDPConn) LocalAddr() Addr
LocalAddr returns the local network address.
LocalAddr 返回本地网络地址



func (*UDPConn) Read

func (c *UDPConn) Read(b []byte) (int, error)
Read implements the Conn Read method.
Read 实现Conn的 Read 方法



func (*UDPConn) ReadFrom

func (c *UDPConn) ReadFrom(b []byte) (int, Addr, error)
ReadFrom implements the PacketConn ReadFrom method.
ReadFrom 实现PacketConn 的ReadFrom 方法



func (*UDPConn) ReadFromUDP

func (c *UDPConn) ReadFromUDP(b []byte) (n int, addr *UDPAddr, err error)
ReadFromUDP reads a UDP packet from c, copying the payload into b. 
It returns the number of bytes copied into b and the return address that was on the packet.

ReadFromUDP can be made to time out and return an error with Timeout() == true after a fixed time limit; 
see SetDeadline and SetReadDeadline.
ReadFromUDP 从c中读取UDP数据包, 复制有效载荷到b.
它返回复制到b的字节数,和 数据包里的地址.
ReadFromUDP可以导致超时 并 在固定时间限制之后返回一个错误 Timeout() == true
查看SetDeadline 和 SetReadDeadline.




func (*UDPConn) ReadMsgUDP

func (c *UDPConn) ReadMsgUDP(b, oob []byte) (n, oobn, flags int, addr *UDPAddr, err error)
ReadMsgUDP reads a packet from c, copying the payload into b and the associated out-of-band data into oob. 
It returns the number of bytes copied into b, the number of bytes copied into oob, the flags that were set on the packet and the source address of the packet.
ReadMsgUDP从c中读取数据包, 复制有效载荷到b 和 对应的 数据到 oob.
它返回复制到b的字节数,和 复制到oob的字节数,数据包里设置的标示 和数据包的源地址



func (*UDPConn) RemoteAddr

func (c *UDPConn) RemoteAddr() Addr
RemoteAddr returns the remote network address.
RemoteAddr返回远程网络地址



func (*UDPConn) SetDeadline

func (c *UDPConn) SetDeadline(t time.Time) error
SetDeadline implements the Conn SetDeadline method.
SetDeadline 实现Conn 的SetDeadline方法



func (*UDPConn) SetReadBuffer

func (c *UDPConn) SetReadBuffer(bytes int) error
SetReadBuffer sets the size of the operating system's receive buffer associated with the connection.
SetReadBuffer设置操作系统的接收缓冲关联的连接的大小



func (*UDPConn) SetReadDeadline

func (c *UDPConn) SetReadDeadline(t time.Time) error
SetReadDeadline implements the Conn SetReadDeadline method.
SetReadDeadline 实现 SetReadDeadline  方法



func (*UDPConn) SetWriteBuffer

func (c *UDPConn) SetWriteBuffer(bytes int) error
SetWriteBuffer sets the size of the operating system's transmit buffer associated with the connection.
SetWriteBuffer设置与该连接相关联的操作系统的发送缓冲区的大小。



func (*UDPConn) SetWriteDeadline

func (c *UDPConn) SetWriteDeadline(t time.Time) error
SetWriteDeadline implements the Conn SetWriteDeadline method.
SetWriteDeadline实现Conn的SetWriteDeadline 方法



func (*UDPConn) Write

func (c *UDPConn) Write(b []byte) (int, error)
Write implements the Conn Write method.
Write 实现 Conn 的Write方法



func (*UDPConn) WriteMsgUDP

func (c *UDPConn) WriteMsgUDP(b, oob []byte, addr *UDPAddr) (n, oobn int, err error)
WriteMsgUDP writes a packet to addr via c, copying the payload from b and the associated out-of-band data from oob. 
It returns the number of payload and out-of-band bytes written.
WriteMsgUDP 通过c写 包数据到addr,从b复制有效载荷 和从oob 复制有关 数据.
返回有效有效载荷数 和 写入字节



func (*UDPConn) WriteTo

func (c *UDPConn) WriteTo(b []byte, addr Addr) (int, error)
WriteTo implements the PacketConn WriteTo method.
WriteTo实现PacketConn 的WriteTo方法



func (*UDPConn) WriteToUDP

func (c *UDPConn) WriteToUDP(b []byte, addr *UDPAddr) (int, error)
WriteToUDP writes a UDP packet to addr via c, copying the payload from b.

WriteToUDP can be made to time out and return an error with Timeout() == true after a fixed time limit;
see SetDeadline and SetWriteDeadline. On packet-oriented connections, write timeouts are rare.
WriteToUDP 从b复制有效载荷,通过 c 写入一个 UDP数据包到 addr .
WriteToUDP 可超时 并且 在固定时间限制后 返回一个  Timeout() == true 错误.
查看 SetDeadline 和 SetWriteDeadline. 在面向数据包的连接， 写超时是罕见的。



type UnixAddr

type UnixAddr struct {
        Name string
        Net  string
}
UnixAddr represents the address of a Unix domain socket end point.
UnixAddr表示一个 Unix 域 socket 终端 的地址



func ResolveUnixAddr

func ResolveUnixAddr(net, addr string) (*UnixAddr, error)
ResolveUnixAddr parses addr as a Unix domain socket address. 
The string net gives the network name, "unix", "unixgram" or "unixpacket".
ResolveUnixAddr 解析 addr 成一个Unix 域 socket 地址.
net 字符串 提供 网络名"unix", "unixgram" 或 "unixpacket".



func (*UnixAddr) Network

func (a *UnixAddr) Network() string
Network returns the address's network name, "unix", "unixgram" or "unixpacket".
Network 返回地址的网络名 , "unix", "unixgram" or "unixpacket".



func (*UnixAddr) String

func (a *UnixAddr) String() string



type UnixConn

type UnixConn struct {
        // contains filtered or unexported fields
}
UnixConn is an implementation of the Conn interface for connections to Unix domain sockets.
UnixConn 是一个实现了 连接 Unix域sockets 的Conn 接口



func DialUnix

func DialUnix(net string, laddr, raddr *UnixAddr) (*UnixConn, error)
DialUnix connects to the remote address raddr on the network net, which must be "unix", "unixgram" or "unixpacket". 
If laddr is not nil, it is used as the local address for the connection.
DialUnix 连接网络net上 的连接远程地址raddr, 必须是"tcp", "tcp4", 或 "tcp6".
如果 laddr 是 非nil , 它使用本地地址来连接



func ListenUnixgram

func ListenUnixgram(net string, laddr *UnixAddr) (*UnixConn, error)
ListenUnixgram listens for incoming Unix datagram packets addressed to the local address laddr. 
The network net must be "unixgram". 
The returned connection's ReadFrom and WriteTo methods can be used to receive and send packets with per-packet addressing.
ListenUnixgram 监听本地地址laddr传入的Unix数据报包地址
网络net必须是"unixgram". 
返回的 连接的 ReadFrom 和  WriteTo 方法 的每个数据包寻址 可以被用来接收和发送数据包



func (*UnixConn) Close

func (c *UnixConn) Close() error
Close closes the connection.
关闭连接



func (*UnixConn) CloseRead

func (c *UnixConn) CloseRead() error
CloseRead shuts down the reading side of the Unix domain connection. Most callers should just use Close.
CloseRead 关闭读取的Unix 域 连接 端.大部分的调用者应该用 Close



func (*UnixConn) CloseWrite

func (c *UnixConn) CloseWrite() error
CloseWrite shuts down the writing side of the Unix domain connection. Most callers should just use Close.
CloseWrite 关闭写入的Unix 域 连接 端.大部分的调用者应该用 Close



func (*UnixConn) File

func (c *UnixConn) File() (f *os.File, err error)
File sets the underlying os.File to blocking mode and returns a copy. 
It is the caller's responsibility to close f when finished. 
Closing c does not affect f, and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.
File设置底层os.File 成阻塞模式 并返回一个复制.
结束的时候调用者负责关闭f.关闭c不影响f , 关闭f不影响c.

返回的 os.File的 文件描述符和连接的不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果。



func (*UnixConn) LocalAddr

func (c *UnixConn) LocalAddr() Addr
LocalAddr returns the local network address.
LocalAddr 返回 本地网络地址



func (*UnixConn) Read

func (c *UnixConn) Read(b []byte) (int, error)
Read implements the Conn Read method.
Read 实现Conn Read  方法



func (*UnixConn) ReadFrom

func (c *UnixConn) ReadFrom(b []byte) (int, Addr, error)
ReadFrom implements the PacketConn ReadFrom method.
ReadFrom 实现PacketConn的 ReadFrom 方法



func (*UnixConn) ReadFromUnix

func (c *UnixConn) ReadFromUnix(b []byte) (n int, addr *UnixAddr, err error)
ReadFromUnix reads a packet from c, copying the payload into b. It returns the number of bytes copied into b and the source address of the packet.

ReadFromUnix can be made to time out and return an error with Timeout() == true after a fixed time limit; see SetDeadline and SetReadDeadline.
ReadFromUnix从c中读取数据包, 复制有效载荷到b . 它返回复制到b的字节数 和数据



func (*UnixConn) ReadMsgUnix

func (c *UnixConn) ReadMsgUnix(b, oob []byte) (n, oobn, flags int, addr *UnixAddr, err error)
ReadMsgUnix reads a packet from c, copying the payload into b and the associated out-of-band data into oob. 
It returns the number of bytes copied into b, the number of bytes copied into oob, the flags that were set on the packet, and the source address of the packet.
ReadMsgUnix 从c读取一个数据包, 复制有效载荷 到b 和对应的 out-of-band  数据到  oob.
返回复制到b的字节数, 复制到oob的字节数,flags 设置在数据包里,并且源地址在数据包里



func (*UnixConn) RemoteAddr

func (c *UnixConn) RemoteAddr() Addr
RemoteAddr returns the remote network address.
RemoteAddr 返回 远程网络地址



func (*UnixConn) SetDeadline

func (c *UnixConn) SetDeadline(t time.Time) error
SetDeadline implements the Conn SetDeadline method.
SetDeadline 设置 Conn 的 SetDeadline  method


func (*UnixConn) SetReadBuffer

func (c *UnixConn) SetReadBuffer(bytes int) error
SetReadBuffer sets the size of the operating system's receive buffer associated with the connection.
SetReadBuffer 设置  操作系统和该连接有关的接收缓冲区 的大小


func (*UnixConn) SetReadDeadline

func (c *UnixConn) SetReadDeadline(t time.Time) error
SetReadDeadline implements the Conn SetReadDeadline method.
SetReadDeadline 实现 Conn  的SetReadDeadline 方法



func (*UnixConn) SetWriteBuffer

func (c *UnixConn) SetWriteBuffer(bytes int) error
SetWriteBuffer sets the size of the operating system's transmit buffer associated with the connection.
SetWriteBuffer 设置  操作系统和该连接有关的发送缓冲区 的大小



func (*UnixConn) SetWriteDeadline

func (c *UnixConn) SetWriteDeadline(t time.Time) error
SetWriteDeadline implements the Conn SetWriteDeadline method.
SetWriteDeadline 实现 Conn的SetWriteDeadline 方法



func (*UnixConn) Write

func (c *UnixConn) Write(b []byte) (int, error)
Write implements the Conn Write method.
Write 实现 Conn 的Write 方法



func (*UnixConn) WriteMsgUnix

func (c *UnixConn) WriteMsgUnix(b, oob []byte, addr *UnixAddr) (n, oobn int, err error)
WriteMsgUnix writes a packet to addr via c, copying the payload from b and the associated out-of-band data from oob. 
It returns the number of payload and out-of-band bytes written.
WriteMsgUnix  通过c 写一个数据包到 addr, 从b复制有效载荷 和 从 oob  复制 对应的 out-of-band  数据



func (*UnixConn) WriteTo

func (c *UnixConn) WriteTo(b []byte, addr Addr) (n int, err error)
WriteTo implements the PacketConn WriteTo method.
WriteTo 实现PacketConn 的 WriteTo 方法



func (*UnixConn) WriteToUnix

func (c *UnixConn) WriteToUnix(b []byte, addr *UnixAddr) (n int, err error)
WriteToUnix writes a packet to addr via c, copying the payload from b.
WriteToUnix can be made to time out and return an error with Timeout() == true after a fixed time limit; 
see SetDeadline and SetWriteDeadline. On packet-oriented connections, write timeouts are rare.

WriteToUnix 通过c 写一个数据包到 addr,从b复制有效载荷.
WriteToUnix可以导致超时 并 在固定时间限制之后返回一个错误 Timeout() == true
查看  SetDeadline 和 SetWriteDeadline.在面向数据包的连接，写超时是罕见的。


type UnixListener

type UnixListener struct {
        // contains filtered or unexported fields
}
UnixListener is a Unix domain socket listener. 
Clients should typically use variables of type Listener instead of assuming Unix domain sockets.
UnixListener 是一个Unix 域 socket 监听器.
客户端通常应该使用类型的变量 Listener



func ListenUnix

func ListenUnix(net string, laddr *UnixAddr) (*UnixListener, error)
ListenUnix announces on the Unix domain socket laddr and returns a Unix listener. 
The network net must be "unix" or "unixpacket".
ListenUnix 声明  在 Unix 域上的 socket  laddr 并且返回一个 Unix  listener
网络 net 必须是  "unix" 或 "unixpacket".



func (*UnixListener) Accept

func (l *UnixListener) Accept() (c Conn, err error)
Accept implements the Accept method in the Listener interface; it waits for the next call and returns a generic Conn.
Accept实现 Listener接口里的 Accept 方法; 它等待下一次调用 并返回一个通用的Conn. 



func (*UnixListener) AcceptUnix

func (l *UnixListener) AcceptUnix() (*UnixConn, error)
AcceptUnix accepts the next incoming call and returns the new connection.
AcceptUnix 接收下一个调用 并返回新的连接.



func (*UnixListener) Addr

func (l *UnixListener) Addr() Addr
Addr returns the listener's network address.
Addr 返回 listener 的网络地址



func (*UnixListener) Close

func (l *UnixListener) Close() error
Close stops listening on the Unix address. 
Already accepted connections are not closed.
Close 停止监听Unix 地址
已经接受的连接不会关闭。



func (*UnixListener) File

func (l *UnixListener) File() (f *os.File, err error)
File returns a copy of the underlying os.File, set to blocking mode. 
It is the caller's responsibility to close f when finished. 
Closing l does not affect f, and closing f does not affect l.

The returned os.File's file descriptor is different from the connection's. 
Attempting to change properties of the original using this duplicate may or may not have the desired effect.

File返回底层os.File复制,设置为阻塞模式。
当结束的时候它的调用者负责关闭f.
关闭l不会影响f , 关闭f 不会影响l
返回的os.File 的文件 描述符 和 连接的 不一样.
尝试使用这种重复改变原来的属性可能会或可能不会产生预期的效果




func (*UnixListener) SetDeadline

func (l *UnixListener) SetDeadline(t time.Time) (err error)
SetDeadline sets the deadline associated with the listener. A zero time value disables the deadline.
SetDeadline 设置  deadline有关的listener .零值的时间值 deadline 不可用



type UnknownNetworkError

type UnknownNetworkError string



func (UnknownNetworkError) Error

func (e UnknownNetworkError) Error() string



func (UnknownNetworkError) Temporary

func (e UnknownNetworkError) Temporary() bool



func (UnknownNetworkError) Timeout

func (e UnknownNetworkError) Timeout() bool



































