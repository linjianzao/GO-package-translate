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

返回的 os.File的 文件描述负和连接的不一样.
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


func (*IPConn) WriteTo

func (c *IPConn) WriteTo(b []byte, addr Addr) (int, error)
WriteTo implements the PacketConn WriteTo method.

func (*IPConn) WriteToIP

func (c *IPConn) WriteToIP(b []byte, addr *IPAddr) (int, error)
WriteToIP writes an IP packet to addr via c, copying the payload from b.

WriteToIP can be made to time out and return an error with Timeout() == true after a fixed time limit; see SetDeadline and SetWriteDeadline. On packet-oriented connections, write timeouts are rare.















































