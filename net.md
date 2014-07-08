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





func LookupTXT(name string) (txt []string, err error)
func SplitHostPort(hostport string) (host, port string, err error)
type Addr
type AddrError
    func (e *AddrError) Error() string
    func (e *AddrError) Temporary() bool
    func (e *AddrError) Timeout() bool
type Conn
    func Dial(network, address string) (Conn, error)
    func DialTimeout(network, address string, timeout time.Duration) (Conn, error)
    func FileConn(f *os.File) (c Conn, err error)
    func Pipe() (Conn, Conn)
type DNSConfigError
    func (e *DNSConfigError) Error() string
    func (e *DNSConfigError) Temporary() bool
    func (e *DNSConfigError) Timeout() bool
type DNSError
    func (e *DNSError) Error() string
    func (e *DNSError) Temporary() bool
    func (e *DNSError) Timeout() bool
type Dialer
    func (d *Dialer) Dial(network, address string) (Conn, error)
type Error
type Flags
    func (f Flags) String() string
type HardwareAddr
    func ParseMAC(s string) (hw HardwareAddr, err error)
    func (a HardwareAddr) String() string
type IP
    func IPv4(a, b, c, d byte) IP
    func ParseCIDR(s string) (IP, *IPNet, error)
    func ParseIP(s string) IP
    func (ip IP) DefaultMask() IPMask
    func (ip IP) Equal(x IP) bool
    func (ip IP) IsGlobalUnicast() bool
    func (ip IP) IsInterfaceLocalMulticast() bool
    func (ip IP) IsLinkLocalMulticast() bool
    func (ip IP) IsLinkLocalUnicast() bool
    func (ip IP) IsLoopback() bool
    func (ip IP) IsMulticast() bool
    func (ip IP) IsUnspecified() bool
    func (ip IP) Mask(mask IPMask) IP
    func (ip IP) String() string
    func (ip IP) To16() IP
    func (ip IP) To4() IP
type IPAddr
    func ResolveIPAddr(net, addr string) (*IPAddr, error)
    func (a *IPAddr) Network() string
    func (a *IPAddr) String() string
type IPConn
    func DialIP(netProto string, laddr, raddr *IPAddr) (*IPConn, error)
    func ListenIP(netProto string, laddr *IPAddr) (*IPConn, error)
    func (c *IPConn) Close() error
    func (c *IPConn) File() (f *os.File, err error)
    func (c *IPConn) LocalAddr() Addr
    func (c *IPConn) Read(b []byte) (int, error)
    func (c *IPConn) ReadFrom(b []byte) (int, Addr, error)
    func (c *IPConn) ReadFromIP(b []byte) (int, *IPAddr, error)
    func (c *IPConn) ReadMsgIP(b, oob []byte) (n, oobn, flags int, addr *IPAddr, err error)
    func (c *IPConn) RemoteAddr() Addr
    func (c *IPConn) SetDeadline(t time.Time) error
    func (c *IPConn) SetReadBuffer(bytes int) error
    func (c *IPConn) SetReadDeadline(t time.Time) error
    func (c *IPConn) SetWriteBuffer(bytes int) error
    func (c *IPConn) SetWriteDeadline(t time.Time) error
    func (c *IPConn) Write(b []byte) (int, error)
    func (c *IPConn) WriteMsgIP(b, oob []byte, addr *IPAddr) (n, oobn int, err error)
    func (c *IPConn) WriteTo(b []byte, addr Addr) (int, error)
    func (c *IPConn) WriteToIP(b []byte, addr *IPAddr) (int, error)
type IPMask
    func CIDRMask(ones, bits int) IPMask
    func IPv4Mask(a, b, c, d byte) IPMask
    func (m IPMask) Size() (ones, bits int)
    func (m IPMask) String() string
type IPNet
    func (n *IPNet) Contains(ip IP) bool
    func (n *IPNet) Network() string
    func (n *IPNet) String() string
type Interface
    func InterfaceByIndex(index int) (*Interface, error)
    func InterfaceByName(name string) (*Interface, error)
    func (ifi *Interface) Addrs() ([]Addr, error)
    func (ifi *Interface) MulticastAddrs() ([]Addr, error)
type InvalidAddrError
    func (e InvalidAddrError) Error() string
    func (e InvalidAddrError) Temporary() bool
    func (e InvalidAddrError) Timeout() bool
type Listener
    func FileListener(f *os.File) (l Listener, err error)
    func Listen(net, laddr string) (Listener, error)
type MX
type NS
type OpError
    func (e *OpError) Error() string
    func (e *OpError) Temporary() bool
    func (e *OpError) Timeout() bool
type PacketConn
    func FilePacketConn(f *os.File) (c PacketConn, err error)
    func ListenPacket(net, laddr string) (PacketConn, error)
type ParseError
    func (e *ParseError) Error() string
type SRV
type TCPAddr
    func ResolveTCPAddr(net, addr string) (*TCPAddr, error)
    func (a *TCPAddr) Network() string
    func (a *TCPAddr) String() string
type TCPConn
    func DialTCP(net string, laddr, raddr *TCPAddr) (*TCPConn, error)
    func (c *TCPConn) Close() error
    func (c *TCPConn) CloseRead() error
    func (c *TCPConn) CloseWrite() error
    func (c *TCPConn) File() (f *os.File, err error)
    func (c *TCPConn) LocalAddr() Addr
    func (c *TCPConn) Read(b []byte) (int, error)
    func (c *TCPConn) ReadFrom(r io.Reader) (int64, error)
    func (c *TCPConn) RemoteAddr() Addr
    func (c *TCPConn) SetDeadline(t time.Time) error
    func (c *TCPConn) SetKeepAlive(keepalive bool) error
    func (c *TCPConn) SetLinger(sec int) error
    func (c *TCPConn) SetNoDelay(noDelay bool) error
    func (c *TCPConn) SetReadBuffer(bytes int) error
    func (c *TCPConn) SetReadDeadline(t time.Time) error
    func (c *TCPConn) SetWriteBuffer(bytes int) error
    func (c *TCPConn) SetWriteDeadline(t time.Time) error
    func (c *TCPConn) Write(b []byte) (int, error)
type TCPListener
    func ListenTCP(net string, laddr *TCPAddr) (*TCPListener, error)
    func (l *TCPListener) Accept() (Conn, error)
    func (l *TCPListener) AcceptTCP() (*TCPConn, error)
    func (l *TCPListener) Addr() Addr
    func (l *TCPListener) Close() error
    func (l *TCPListener) File() (f *os.File, err error)
    func (l *TCPListener) SetDeadline(t time.Time) error
type UDPAddr
    func ResolveUDPAddr(net, addr string) (*UDPAddr, error)
    func (a *UDPAddr) Network() string
    func (a *UDPAddr) String() string
type UDPConn
    func DialUDP(net string, laddr, raddr *UDPAddr) (*UDPConn, error)
    func ListenMulticastUDP(net string, ifi *Interface, gaddr *UDPAddr) (*UDPConn, error)
    func ListenUDP(net string, laddr *UDPAddr) (*UDPConn, error)
    func (c *UDPConn) Close() error
    func (c *UDPConn) File() (f *os.File, err error)
    func (c *UDPConn) LocalAddr() Addr
    func (c *UDPConn) Read(b []byte) (int, error)
    func (c *UDPConn) ReadFrom(b []byte) (int, Addr, error)
    func (c *UDPConn) ReadFromUDP(b []byte) (n int, addr *UDPAddr, err error)
    func (c *UDPConn) ReadMsgUDP(b, oob []byte) (n, oobn, flags int, addr *UDPAddr, err error)
    func (c *UDPConn) RemoteAddr() Addr
    func (c *UDPConn) SetDeadline(t time.Time) error
    func (c *UDPConn) SetReadBuffer(bytes int) error
    func (c *UDPConn) SetReadDeadline(t time.Time) error
    func (c *UDPConn) SetWriteBuffer(bytes int) error
    func (c *UDPConn) SetWriteDeadline(t time.Time) error
    func (c *UDPConn) Write(b []byte) (int, error)
    func (c *UDPConn) WriteMsgUDP(b, oob []byte, addr *UDPAddr) (n, oobn int, err error)
    func (c *UDPConn) WriteTo(b []byte, addr Addr) (int, error)
    func (c *UDPConn) WriteToUDP(b []byte, addr *UDPAddr) (int, error)
type UnixAddr
    func ResolveUnixAddr(net, addr string) (*UnixAddr, error)
    func (a *UnixAddr) Network() string
    func (a *UnixAddr) String() string
type UnixConn
    func DialUnix(net string, laddr, raddr *UnixAddr) (*UnixConn, error)
    func ListenUnixgram(net string, laddr *UnixAddr) (*UnixConn, error)
    func (c *UnixConn) Close() error
    func (c *UnixConn) CloseRead() error
    func (c *UnixConn) CloseWrite() error
    func (c *UnixConn) File() (f *os.File, err error)
    func (c *UnixConn) LocalAddr() Addr
    func (c *UnixConn) Read(b []byte) (int, error)
    func (c *UnixConn) ReadFrom(b []byte) (int, Addr, error)
    func (c *UnixConn) ReadFromUnix(b []byte) (n int, addr *UnixAddr, err error)
    func (c *UnixConn) ReadMsgUnix(b, oob []byte) (n, oobn, flags int, addr *UnixAddr, err error)
    func (c *UnixConn) RemoteAddr() Addr
    func (c *UnixConn) SetDeadline(t time.Time) error
    func (c *UnixConn) SetReadBuffer(bytes int) error
    func (c *UnixConn) SetReadDeadline(t time.Time) error
    func (c *UnixConn) SetWriteBuffer(bytes int) error
    func (c *UnixConn) SetWriteDeadline(t time.Time) error
    func (c *UnixConn) Write(b []byte) (int, error)
    func (c *UnixConn) WriteMsgUnix(b, oob []byte, addr *UnixAddr) (n, oobn int, err error)
    func (c *UnixConn) WriteTo(b []byte, addr Addr) (n int, err error)
    func (c *UnixConn) WriteToUnix(b []byte, addr *UnixAddr) (n int, err error)
type UnixListener
    func ListenUnix(net string, laddr *UnixAddr) (*UnixListener, error)
    func (l *UnixListener) Accept() (c Conn, err error)
    func (l *UnixListener) AcceptUnix() (*UnixConn, error)
    func (l *UnixListener) Addr() Addr
    func (l *UnixListener) Close() error
    func (l *UnixListener) File() (f *os.File, err error)
    func (l *UnixListener) SetDeadline(t time.Time) (err error)
type UnknownNetworkError
    func (e UnknownNetworkError) Error() string
    func (e UnknownNetworkError) Temporary() bool
    func (e UnknownNetworkError) Timeout() bool
Examples

Listener
Package files

cgo_linux.go cgo_unix.go dial.go dnsclient.go dnsclient_unix.go dnsconfig_unix.go dnsmsg.go fd_poll_runtime.go fd_unix.go file_unix.go hosts.go interface.go interface_linux.go ip.go iprawsock.go iprawsock_posix.go ipsock.go ipsock_posix.go lookup.go lookup_unix.go mac.go net.go parse.go pipe.go port.go port_unix.go sendfile_linux.go sock_cloexec.go sock_linux.go sock_posix.go sock_unix.go sockopt_linux.go sockopt_posix.go sockoptip_linux.go sockoptip_posix.go tcpsock.go tcpsock_posix.go udpsock.go udpsock_posix.go unixsock.go unixsock_posix.go
