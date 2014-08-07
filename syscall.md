Package syscall

Overview ▾

Package syscall contains an interface to the low-level operating system primitives. 
The details vary depending on the underlying system, and by default, godoc will display the syscall documentation for the current system. 
If you want godoc to display syscall documentation for another system, set $GOOS and $GOARCH to the desired system. 
For example, if you want to view documentation for freebsd/arm on linux/amd64, set $GOOS to freebsd and $GOARCH to arm. 
The primary use of syscall is inside other packages that provide a more portable interface to the system, such as "os", "time" and "net". 
Use those packages rather than this one if you can. For details of the functions and data types in this package consult the manuals for the appropriate operating system. 
These calls return err == nil to indicate success; otherwise err is an operating system error describing the failure. On most systems, that error has type syscall.Errno.

syscall 包含一个接口 到 低级 操作系统原函数.
细节取决于底层系统,默认的,godoc 将会显示当前系统的syscall文档.
如果你想godoc 显示别的系统的syscall文档,设置$GOOS 和 $GOARCH 成 期望的系统.
例如. 如果你像查看 linux/amd64 的 freebsd/arm 文档,  设置 $GOOS 成 freebsd 和  $GOARCH  成 arm.
syscall最主要的使用是  让其他包 提供一个更便携接口到系统中，例如  "os", "time" 和 "net".
如果您可以使用这些软件包，而不是这一个. 这个包里 详细的函数 和数据类型 查看 适合的操作系统 对应的参考手册.
这些调用返回err == nil，表示成功;否则 err 是一个描述失败的操作系统错误.在大部分系统, 该错误类型是  syscall.Errno.


Constants
```golang
const (
        AF_UNSPEC = iota
        AF_UNIX
        AF_INET
        AF_INET6
)
```

```golang
const (
        SHUT_RD = iota
        SHUT_WR
        SHUT_RDWR
)
```

```golang
const (
        SOCK_STREAM = 1 + iota
        SOCK_DGRAM
        SOCK_RAW
        SOCK_SEQPACKET
)
```

```golang
const (
        IPPROTO_IP   = 0
        IPPROTO_IPV4 = 4
        IPPROTO_IPV6 = 0x29
        IPPROTO_TCP  = 6
        IPPROTO_UDP  = 0x11
)
```

```golang
const (
        SOL_SOCKET
        SO_TYPE
        NET_RT_IFLIST
        IFNAMSIZ
        IFF_UP
        IFF_BROADCAST
        IFF_LOOPBACK
        IFF_POINTOPOINT
        IFF_MULTICAST
        IPV6_V6ONLY
        SOMAXCONN
        F_DUPFD_CLOEXEC
        SO_BROADCAST
        SO_REUSEADDR
        SO_REUSEPORT
        SO_RCVBUF
        SO_SNDBUF
        SO_KEEPALIVE
        SO_LINGER
        SO_ERROR
        IP_PORTRANGE
        IP_PORTRANGE_DEFAULT
        IP_PORTRANGE_LOW
        IP_PORTRANGE_HIGH
        IP_MULTICAST_IF
        IP_MULTICAST_LOOP
        IP_ADD_MEMBERSHIP
        IPV6_PORTRANGE
        IPV6_PORTRANGE_DEFAULT
        IPV6_PORTRANGE_LOW
        IPV6_PORTRANGE_HIGH
        IPV6_MULTICAST_IF
        IPV6_MULTICAST_LOOP
        IPV6_JOIN_GROUP
        TCP_NODELAY
        TCP_KEEPINTVL
        TCP_KEEPIDLE

        SYS_FCNTL = 500 // unsupported
)
```
Misc constants expected by package net but not supported.
Misc 但不支持预期的net包常量.


```golang
const (
        SIGCHLD
        SIGINT
        SIGKILL
        SIGTRAP
        SIGQUIT
)
```

```golang
const (
        Stdin  = 0
        Stdout = 1
        Stderr = 2
)
```

```golang
const (
        O_RDONLY  = 0
        O_WRONLY  = 1
        O_RDWR    = 2
        O_ACCMODE = 3

        O_CREAT    = 0100
        O_CREATE   = O_CREAT // for ken
        O_TRUNC    = 01000
        O_APPEND   = 02000
        O_EXCL     = 0200
        O_NONBLOCK = 04000
        O_NDELAY   = O_NONBLOCK
        O_SYNC     = 010000
        O_FSYNC    = O_SYNC
        O_ASYNC    = 020000

        O_CLOEXEC = 0

        FD_CLOEXEC = 1
)
```
native_client/src/trusted/service_runtime/include/sys/fcntl.h

```golang
const (
        F_DUPFD   = 0
        F_GETFD   = 1
        F_SETFD   = 2
        F_GETFL   = 3
        F_SETFL   = 4
        F_GETOWN  = 5
        F_SETOWN  = 6
        F_GETLK   = 7
        F_SETLK   = 8
        F_SETLKW  = 9
        F_RGETLK  = 10
        F_RSETLK  = 11
        F_CNVT    = 12
        F_RSETLKW = 13

        F_RDLCK   = 1
        F_WRLCK   = 2
        F_UNLCK   = 3
        F_UNLKSYS = 4
)
```
native_client/src/trusted/service_runtime/include/sys/fcntl.h

```golang
const (
        S_IFMT        = 0000370000
        S_IFSHM_SYSV  = 0000300000
        S_IFSEMA      = 0000270000
        S_IFCOND      = 0000260000
        S_IFMUTEX     = 0000250000
        S_IFSHM       = 0000240000
        S_IFBOUNDSOCK = 0000230000
        S_IFSOCKADDR  = 0000220000
        S_IFDSOCK     = 0000210000

        S_IFSOCK = 0000140000
        S_IFLNK  = 0000120000
        S_IFREG  = 0000100000
        S_IFBLK  = 0000060000
        S_IFDIR  = 0000040000
        S_IFCHR  = 0000020000
        S_IFIFO  = 0000010000

        S_UNSUP = 0000370000

        S_ISUID = 0004000
        S_ISGID = 0002000
        S_ISVTX = 0001000

        S_IREAD  = 0400
        S_IWRITE = 0200
        S_IEXEC  = 0100

        S_IRWXU = 0700
        S_IRUSR = 0400
        S_IWUSR = 0200
        S_IXUSR = 0100

        S_IRWXG = 070
        S_IRGRP = 040
        S_IWGRP = 020
        S_IXGRP = 010

        S_IRWXO = 07
        S_IROTH = 04
        S_IWOTH = 02
        S_IXOTH = 01
)
```
native_client/src/trusted/service_runtime/include/bits/stat.h


```golang
const (
        AF_ALG                           = 0x26
        AF_APPLETALK                     = 0x5
        AF_ASH                           = 0x12
        AF_ATMPVC                        = 0x8
        AF_ATMSVC                        = 0x14
        AF_AX25                          = 0x3
        AF_BLUETOOTH                     = 0x1f
        AF_BRIDGE                        = 0x7
        AF_CAIF                          = 0x25
        AF_CAN                           = 0x1d
        AF_DECnet                        = 0xc
        AF_ECONET                        = 0x13
        AF_FILE                          = 0x1
        AF_IEEE802154                    = 0x24
        AF_INET                          = 0x2
        AF_INET6                         = 0xa
        AF_IPX                           = 0x4
        AF_IRDA                          = 0x17
        AF_ISDN                          = 0x22
        AF_IUCV                          = 0x20
        AF_KEY                           = 0xf
        AF_LLC                           = 0x1a
        AF_LOCAL                         = 0x1
        AF_MAX                           = 0x27
        AF_NETBEUI                       = 0xd
        AF_NETLINK                       = 0x10
        AF_NETROM                        = 0x6
        AF_PACKET                        = 0x11
        AF_PHONET                        = 0x23
        AF_PPPOX                         = 0x18
        AF_RDS                           = 0x15
        AF_ROSE                          = 0xb
        AF_ROUTE                         = 0x10
        AF_RXRPC                         = 0x21
        AF_SECURITY                      = 0xe
        AF_SNA                           = 0x16
        AF_TIPC                          = 0x1e
        AF_UNIX                          = 0x1
        AF_UNSPEC                        = 0x0
        AF_WANPIPE                       = 0x19
        AF_X25                           = 0x9
        ARPHRD_ADAPT                     = 0x108
        ARPHRD_APPLETLK                  = 0x8
        ARPHRD_ARCNET                    = 0x7
        ARPHRD_ASH                       = 0x30d
        ARPHRD_ATM                       = 0x13
        ARPHRD_AX25                      = 0x3
        ARPHRD_BIF                       = 0x307
        ARPHRD_CHAOS                     = 0x5
        ARPHRD_CISCO                     = 0x201
        ARPHRD_CSLIP                     = 0x101
        ARPHRD_CSLIP6                    = 0x103
        ARPHRD_DDCMP                     = 0x205
        ARPHRD_DLCI                      = 0xf
        ARPHRD_ECONET                    = 0x30e
        ARPHRD_EETHER                    = 0x2
        ARPHRD_ETHER                     = 0x1
        ARPHRD_EUI64                     = 0x1b
        ARPHRD_FCAL                      = 0x311
        ARPHRD_FCFABRIC                  = 0x313
        ARPHRD_FCPL                      = 0x312
        ARPHRD_FCPP                      = 0x310
        ARPHRD_FDDI                      = 0x306
        ARPHRD_FRAD                      = 0x302
        ARPHRD_HDLC                      = 0x201
        ARPHRD_HIPPI                     = 0x30c
        ARPHRD_HWX25                     = 0x110
        ARPHRD_IEEE1394                  = 0x18
        ARPHRD_IEEE802                   = 0x6
        ARPHRD_IEEE80211                 = 0x321
        ARPHRD_IEEE80211_PRISM           = 0x322
        ARPHRD_IEEE80211_RADIOTAP        = 0x323
        ARPHRD_IEEE802154                = 0x324
        ARPHRD_IEEE802154_PHY            = 0x325
        ARPHRD_IEEE802_TR                = 0x320
        ARPHRD_INFINIBAND                = 0x20
        ARPHRD_IPDDP                     = 0x309
        ARPHRD_IPGRE                     = 0x30a
        ARPHRD_IRDA                      = 0x30f
        ARPHRD_LAPB                      = 0x204
        ARPHRD_LOCALTLK                  = 0x305
        ARPHRD_LOOPBACK                  = 0x304
        ARPHRD_METRICOM                  = 0x17
        ARPHRD_NETROM                    = 0x0
        ARPHRD_NONE                      = 0xfffe
        ARPHRD_PIMREG                    = 0x30b
        ARPHRD_PPP                       = 0x200
        ARPHRD_PRONET                    = 0x4
        ARPHRD_RAWHDLC                   = 0x206
        ARPHRD_ROSE                      = 0x10e
        ARPHRD_RSRVD                     = 0x104
        ARPHRD_SIT                       = 0x308
        ARPHRD_SKIP                      = 0x303
        ARPHRD_SLIP                      = 0x100
        ARPHRD_SLIP6                     = 0x102
        ARPHRD_TUNNEL                    = 0x300
        ARPHRD_TUNNEL6                   = 0x301
        ARPHRD_VOID                      = 0xffff
        ARPHRD_X25                       = 0x10f
        BPF_A                            = 0x10
        BPF_ABS                          = 0x20
        BPF_ADD                          = 0x0
        BPF_ALU                          = 0x4
        BPF_AND                          = 0x50
        BPF_B                            = 0x10
        BPF_DIV                          = 0x30
        BPF_H                            = 0x8
        BPF_IMM                          = 0x0
        BPF_IND                          = 0x40
        BPF_JA                           = 0x0
        BPF_JEQ                          = 0x10
        BPF_JGE                          = 0x30
        BPF_JGT                          = 0x20
        BPF_JMP                          = 0x5
        BPF_JSET                         = 0x40
        BPF_K                            = 0x0
        BPF_LD                           = 0x0
        BPF_LDX                          = 0x1
        BPF_LEN                          = 0x80
        BPF_LSH                          = 0x60
        BPF_MAJOR_VERSION                = 0x1
        BPF_MAXINSNS                     = 0x1000
        BPF_MEM                          = 0x60
        BPF_MEMWORDS                     = 0x10
        BPF_MINOR_VERSION                = 0x1
        BPF_MISC                         = 0x7
        BPF_MSH                          = 0xa0
        BPF_MUL                          = 0x20
        BPF_NEG                          = 0x80
        BPF_OR                           = 0x40
        BPF_RET                          = 0x6
        BPF_RSH                          = 0x70
        BPF_ST                           = 0x2
        BPF_STX                          = 0x3
        BPF_SUB                          = 0x10
        BPF_TAX                          = 0x0
        BPF_TXA                          = 0x80
        BPF_W                            = 0x0
        BPF_X                            = 0x8
        CLONE_CHILD_CLEARTID             = 0x200000
        CLONE_CHILD_SETTID               = 0x1000000
        CLONE_DETACHED                   = 0x400000
        CLONE_FILES                      = 0x400
        CLONE_FS                         = 0x200
        CLONE_IO                         = 0x80000000
        CLONE_NEWIPC                     = 0x8000000
        CLONE_NEWNET                     = 0x40000000
        CLONE_NEWNS                      = 0x20000
        CLONE_NEWPID                     = 0x20000000
        CLONE_NEWUSER                    = 0x10000000
        CLONE_NEWUTS                     = 0x4000000
        CLONE_PARENT                     = 0x8000
        CLONE_PARENT_SETTID              = 0x100000
        CLONE_PTRACE                     = 0x2000
        CLONE_SETTLS                     = 0x80000
        CLONE_SIGHAND                    = 0x800
        CLONE_SYSVSEM                    = 0x40000
        CLONE_THREAD                     = 0x10000
        CLONE_UNTRACED                   = 0x800000
        CLONE_VFORK                      = 0x4000
        CLONE_VM                         = 0x100
        DT_BLK                           = 0x6
        DT_CHR                           = 0x2
        DT_DIR                           = 0x4
        DT_FIFO                          = 0x1
        DT_LNK                           = 0xa
        DT_REG                           = 0x8
        DT_SOCK                          = 0xc
        DT_UNKNOWN                       = 0x0
        DT_WHT                           = 0xe
        EPOLLERR                         = 0x8
        EPOLLET                          = -0x80000000
        EPOLLHUP                         = 0x10
        EPOLLIN                          = 0x1
        EPOLLMSG                         = 0x400
        EPOLLONESHOT                     = 0x40000000
        EPOLLOUT                         = 0x4
        EPOLLPRI                         = 0x2
        EPOLLRDBAND                      = 0x80
        EPOLLRDHUP                       = 0x2000
        EPOLLRDNORM                      = 0x40
        EPOLLWRBAND                      = 0x200
        EPOLLWRNORM                      = 0x100
        EPOLL_CLOEXEC                    = 0x80000
        EPOLL_CTL_ADD                    = 0x1
        EPOLL_CTL_DEL                    = 0x2
        EPOLL_CTL_MOD                    = 0x3
        EPOLL_NONBLOCK                   = 0x800
        ETH_P_1588                       = 0x88f7
        ETH_P_8021Q                      = 0x8100
        ETH_P_802_2                      = 0x4
        ETH_P_802_3                      = 0x1
        ETH_P_AARP                       = 0x80f3
        ETH_P_ALL                        = 0x3
        ETH_P_AOE                        = 0x88a2
        ETH_P_ARCNET                     = 0x1a
        ETH_P_ARP                        = 0x806
        ETH_P_ATALK                      = 0x809b
        ETH_P_ATMFATE                    = 0x8884
        ETH_P_ATMMPOA                    = 0x884c
        ETH_P_AX25                       = 0x2
        ETH_P_BPQ                        = 0x8ff
        ETH_P_CAIF                       = 0xf7
        ETH_P_CAN                        = 0xc
        ETH_P_CONTROL                    = 0x16
        ETH_P_CUST                       = 0x6006
        ETH_P_DDCMP                      = 0x6
        ETH_P_DEC                        = 0x6000
        ETH_P_DIAG                       = 0x6005
        ETH_P_DNA_DL                     = 0x6001
        ETH_P_DNA_RC                     = 0x6002
        ETH_P_DNA_RT                     = 0x6003
        ETH_P_DSA                        = 0x1b
        ETH_P_ECONET                     = 0x18
        ETH_P_EDSA                       = 0xdada
        ETH_P_FCOE                       = 0x8906
        ETH_P_FIP                        = 0x8914
        ETH_P_HDLC                       = 0x19
        ETH_P_IEEE802154                 = 0xf6
        ETH_P_IEEEPUP                    = 0xa00
        ETH_P_IEEEPUPAT                  = 0xa01
        ETH_P_IP                         = 0x800
        ETH_P_IPV6                       = 0x86dd
        ETH_P_IPX                        = 0x8137
        ETH_P_IRDA                       = 0x17
        ETH_P_LAT                        = 0x6004
        ETH_P_LINK_CTL                   = 0x886c
        ETH_P_LOCALTALK                  = 0x9
        ETH_P_LOOP                       = 0x60
        ETH_P_MOBITEX                    = 0x15
        ETH_P_MPLS_MC                    = 0x8848
        ETH_P_MPLS_UC                    = 0x8847
        ETH_P_PAE                        = 0x888e
        ETH_P_PAUSE                      = 0x8808
        ETH_P_PHONET                     = 0xf5
        ETH_P_PPPTALK                    = 0x10
        ETH_P_PPP_DISC                   = 0x8863
        ETH_P_PPP_MP                     = 0x8
        ETH_P_PPP_SES                    = 0x8864
        ETH_P_PUP                        = 0x200
        ETH_P_PUPAT                      = 0x201
        ETH_P_RARP                       = 0x8035
        ETH_P_SCA                        = 0x6007
        ETH_P_SLOW                       = 0x8809
        ETH_P_SNAP                       = 0x5
        ETH_P_TEB                        = 0x6558
        ETH_P_TIPC                       = 0x88ca
        ETH_P_TRAILER                    = 0x1c
        ETH_P_TR_802_2                   = 0x11
        ETH_P_WAN_PPP                    = 0x7
        ETH_P_WCCP                       = 0x883e
        ETH_P_X25                        = 0x805
        FD_CLOEXEC                       = 0x1
        FD_SETSIZE                       = 0x400
        F_DUPFD                          = 0x0
        F_DUPFD_CLOEXEC                  = 0x406
        F_EXLCK                          = 0x4
        F_GETFD                          = 0x1
        F_GETFL                          = 0x3
        F_GETLEASE                       = 0x401
        F_GETLK                          = 0x5
        F_GETLK64                        = 0x5
        F_GETOWN                         = 0x9
        F_GETOWN_EX                      = 0x10
        F_GETPIPE_SZ                     = 0x408
        F_GETSIG                         = 0xb
        F_LOCK                           = 0x1
        F_NOTIFY                         = 0x402
        F_OK                             = 0x0
        F_RDLCK                          = 0x0
        F_SETFD                          = 0x2
        F_SETFL                          = 0x4
        F_SETLEASE                       = 0x400
        F_SETLK                          = 0x6
        F_SETLK64                        = 0x6
        F_SETLKW                         = 0x7
        F_SETLKW64                       = 0x7
        F_SETOWN                         = 0x8
        F_SETOWN_EX                      = 0xf
        F_SETPIPE_SZ                     = 0x407
        F_SETSIG                         = 0xa
        F_SHLCK                          = 0x8
        F_TEST                           = 0x3
        F_TLOCK                          = 0x2
        F_ULOCK                          = 0x0
        F_UNLCK                          = 0x2
        F_WRLCK                          = 0x1
        ICMPV6_FILTER                    = 0x1
        IFA_F_DADFAILED                  = 0x8
        IFA_F_DEPRECATED                 = 0x20
        IFA_F_HOMEADDRESS                = 0x10
        IFA_F_NODAD                      = 0x2
        IFA_F_OPTIMISTIC                 = 0x4
        IFA_F_PERMANENT                  = 0x80
        IFA_F_SECONDARY                  = 0x1
        IFA_F_TEMPORARY                  = 0x1
        IFA_F_TENTATIVE                  = 0x40
        IFA_MAX                          = 0x7
        IFF_ALLMULTI                     = 0x200
        IFF_AUTOMEDIA                    = 0x4000
        IFF_BROADCAST                    = 0x2
        IFF_DEBUG                        = 0x4
        IFF_DYNAMIC                      = 0x8000
        IFF_LOOPBACK                     = 0x8
        IFF_MASTER                       = 0x400
        IFF_MULTICAST                    = 0x1000
        IFF_NOARP                        = 0x80
        IFF_NOTRAILERS                   = 0x20
        IFF_NO_PI                        = 0x1000
        IFF_ONE_QUEUE                    = 0x2000
        IFF_POINTOPOINT                  = 0x10
        IFF_PORTSEL                      = 0x2000
        IFF_PROMISC                      = 0x100
        IFF_RUNNING                      = 0x40
        IFF_SLAVE                        = 0x800
        IFF_TAP                          = 0x2
        IFF_TUN                          = 0x1
        IFF_TUN_EXCL                     = 0x8000
        IFF_UP                           = 0x1
        IFF_VNET_HDR                     = 0x4000
        IFNAMSIZ                         = 0x10
        IN_ACCESS                        = 0x1
        IN_ALL_EVENTS                    = 0xfff
        IN_ATTRIB                        = 0x4
        IN_CLASSA_HOST                   = 0xffffff
        IN_CLASSA_MAX                    = 0x80
        IN_CLASSA_NET                    = 0xff000000
        IN_CLASSA_NSHIFT                 = 0x18
        IN_CLASSB_HOST                   = 0xffff
        IN_CLASSB_MAX                    = 0x10000
        IN_CLASSB_NET                    = 0xffff0000
        IN_CLASSB_NSHIFT                 = 0x10
        IN_CLASSC_HOST                   = 0xff
        IN_CLASSC_NET                    = 0xffffff00
        IN_CLASSC_NSHIFT                 = 0x8
        IN_CLOEXEC                       = 0x80000
        IN_CLOSE                         = 0x18
        IN_CLOSE_NOWRITE                 = 0x10
        IN_CLOSE_WRITE                   = 0x8
        IN_CREATE                        = 0x100
        IN_DELETE                        = 0x200
        IN_DELETE_SELF                   = 0x400
        IN_DONT_FOLLOW                   = 0x2000000
        IN_EXCL_UNLINK                   = 0x4000000
        IN_IGNORED                       = 0x8000
        IN_ISDIR                         = 0x40000000
        IN_LOOPBACKNET                   = 0x7f
        IN_MASK_ADD                      = 0x20000000
        IN_MODIFY                        = 0x2
        IN_MOVE                          = 0xc0
        IN_MOVED_FROM                    = 0x40
        IN_MOVED_TO                      = 0x80
        IN_MOVE_SELF                     = 0x800
        IN_NONBLOCK                      = 0x800
        IN_ONESHOT                       = 0x80000000
        IN_ONLYDIR                       = 0x1000000
        IN_OPEN                          = 0x20
        IN_Q_OVERFLOW                    = 0x4000
        IN_UNMOUNT                       = 0x2000
        IPPROTO_AH                       = 0x33
        IPPROTO_COMP                     = 0x6c
        IPPROTO_DCCP                     = 0x21
        IPPROTO_DSTOPTS                  = 0x3c
        IPPROTO_EGP                      = 0x8
        IPPROTO_ENCAP                    = 0x62
        IPPROTO_ESP                      = 0x32
        IPPROTO_FRAGMENT                 = 0x2c
        IPPROTO_GRE                      = 0x2f
        IPPROTO_HOPOPTS                  = 0x0
        IPPROTO_ICMP                     = 0x1
        IPPROTO_ICMPV6                   = 0x3a
        IPPROTO_IDP                      = 0x16
        IPPROTO_IGMP                     = 0x2
        IPPROTO_IP                       = 0x0
        IPPROTO_IPIP                     = 0x4
        IPPROTO_IPV6                     = 0x29
        IPPROTO_MTP                      = 0x5c
        IPPROTO_NONE                     = 0x3b
        IPPROTO_PIM                      = 0x67
        IPPROTO_PUP                      = 0xc
        IPPROTO_RAW                      = 0xff
        IPPROTO_ROUTING                  = 0x2b
        IPPROTO_RSVP                     = 0x2e
        IPPROTO_SCTP                     = 0x84
        IPPROTO_TCP                      = 0x6
        IPPROTO_TP                       = 0x1d
        IPPROTO_UDP                      = 0x11
        IPPROTO_UDPLITE                  = 0x88
        IPV6_2292DSTOPTS                 = 0x4
        IPV6_2292HOPLIMIT                = 0x8
        IPV6_2292HOPOPTS                 = 0x3
        IPV6_2292PKTINFO                 = 0x2
        IPV6_2292PKTOPTIONS              = 0x6
        IPV6_2292RTHDR                   = 0x5
        IPV6_ADDRFORM                    = 0x1
        IPV6_ADD_MEMBERSHIP              = 0x14
        IPV6_AUTHHDR                     = 0xa
        IPV6_CHECKSUM                    = 0x7
        IPV6_DROP_MEMBERSHIP             = 0x15
        IPV6_DSTOPTS                     = 0x3b
        IPV6_HOPLIMIT                    = 0x34
        IPV6_HOPOPTS                     = 0x36
        IPV6_IPSEC_POLICY                = 0x22
        IPV6_JOIN_ANYCAST                = 0x1b
        IPV6_JOIN_GROUP                  = 0x14
        IPV6_LEAVE_ANYCAST               = 0x1c
        IPV6_LEAVE_GROUP                 = 0x15
        IPV6_MTU                         = 0x18
        IPV6_MTU_DISCOVER                = 0x17
        IPV6_MULTICAST_HOPS              = 0x12
        IPV6_MULTICAST_IF                = 0x11
        IPV6_MULTICAST_LOOP              = 0x13
        IPV6_NEXTHOP                     = 0x9
        IPV6_PKTINFO                     = 0x32
        IPV6_PMTUDISC_DO                 = 0x2
        IPV6_PMTUDISC_DONT               = 0x0
        IPV6_PMTUDISC_PROBE              = 0x3
        IPV6_PMTUDISC_WANT               = 0x1
        IPV6_RECVDSTOPTS                 = 0x3a
        IPV6_RECVERR                     = 0x19
        IPV6_RECVHOPLIMIT                = 0x33
        IPV6_RECVHOPOPTS                 = 0x35
        IPV6_RECVPKTINFO                 = 0x31
        IPV6_RECVRTHDR                   = 0x38
        IPV6_RECVTCLASS                  = 0x42
        IPV6_ROUTER_ALERT                = 0x16
        IPV6_RTHDR                       = 0x39
        IPV6_RTHDRDSTOPTS                = 0x37
        IPV6_RTHDR_LOOSE                 = 0x0
        IPV6_RTHDR_STRICT                = 0x1
        IPV6_RTHDR_TYPE_0                = 0x0
        IPV6_RXDSTOPTS                   = 0x3b
        IPV6_RXHOPOPTS                   = 0x36
        IPV6_TCLASS                      = 0x43
        IPV6_UNICAST_HOPS                = 0x10
        IPV6_V6ONLY                      = 0x1a
        IPV6_XFRM_POLICY                 = 0x23
        IP_ADD_MEMBERSHIP                = 0x23
        IP_ADD_SOURCE_MEMBERSHIP         = 0x27
        IP_BLOCK_SOURCE                  = 0x26
        IP_DEFAULT_MULTICAST_LOOP        = 0x1
        IP_DEFAULT_MULTICAST_TTL         = 0x1
        IP_DF                            = 0x4000
        IP_DROP_MEMBERSHIP               = 0x24
        IP_DROP_SOURCE_MEMBERSHIP        = 0x28
        IP_FREEBIND                      = 0xf
        IP_HDRINCL                       = 0x3
        IP_IPSEC_POLICY                  = 0x10
        IP_MAXPACKET                     = 0xffff
        IP_MAX_MEMBERSHIPS               = 0x14
        IP_MF                            = 0x2000
        IP_MINTTL                        = 0x15
        IP_MSFILTER                      = 0x29
        IP_MSS                           = 0x240
        IP_MTU                           = 0xe
        IP_MTU_DISCOVER                  = 0xa
        IP_MULTICAST_IF                  = 0x20
        IP_MULTICAST_LOOP                = 0x22
        IP_MULTICAST_TTL                 = 0x21
        IP_OFFMASK                       = 0x1fff
        IP_OPTIONS                       = 0x4
        IP_ORIGDSTADDR                   = 0x14
        IP_PASSSEC                       = 0x12
        IP_PKTINFO                       = 0x8
        IP_PKTOPTIONS                    = 0x9
        IP_PMTUDISC                      = 0xa
        IP_PMTUDISC_DO                   = 0x2
        IP_PMTUDISC_DONT                 = 0x0
        IP_PMTUDISC_PROBE                = 0x3
        IP_PMTUDISC_WANT                 = 0x1
        IP_RECVERR                       = 0xb
        IP_RECVOPTS                      = 0x6
        IP_RECVORIGDSTADDR               = 0x14
        IP_RECVRETOPTS                   = 0x7
        IP_RECVTOS                       = 0xd
        IP_RECVTTL                       = 0xc
        IP_RETOPTS                       = 0x7
        IP_RF                            = 0x8000
        IP_ROUTER_ALERT                  = 0x5
        IP_TOS                           = 0x1
        IP_TRANSPARENT                   = 0x13
        IP_TTL                           = 0x2
        IP_UNBLOCK_SOURCE                = 0x25
        IP_XFRM_POLICY                   = 0x11
        LINUX_REBOOT_CMD_CAD_OFF         = 0x0
        LINUX_REBOOT_CMD_CAD_ON          = 0x89abcdef
        LINUX_REBOOT_CMD_HALT            = 0xcdef0123
        LINUX_REBOOT_CMD_KEXEC           = 0x45584543
        LINUX_REBOOT_CMD_POWER_OFF       = 0x4321fedc
        LINUX_REBOOT_CMD_RESTART         = 0x1234567
        LINUX_REBOOT_CMD_RESTART2        = 0xa1b2c3d4
        LINUX_REBOOT_CMD_SW_SUSPEND      = 0xd000fce2
        LINUX_REBOOT_MAGIC1              = 0xfee1dead
        LINUX_REBOOT_MAGIC2              = 0x28121969
        LOCK_EX                          = 0x2
        LOCK_NB                          = 0x4
        LOCK_SH                          = 0x1
        LOCK_UN                          = 0x8
        MADV_DOFORK                      = 0xb
        MADV_DONTFORK                    = 0xa
        MADV_DONTNEED                    = 0x4
        MADV_HUGEPAGE                    = 0xe
        MADV_HWPOISON                    = 0x64
        MADV_MERGEABLE                   = 0xc
        MADV_NOHUGEPAGE                  = 0xf
        MADV_NORMAL                      = 0x0
        MADV_RANDOM                      = 0x1
        MADV_REMOVE                      = 0x9
        MADV_SEQUENTIAL                  = 0x2
        MADV_UNMERGEABLE                 = 0xd
        MADV_WILLNEED                    = 0x3
        MAP_32BIT                        = 0x40
        MAP_ANON                         = 0x20
        MAP_ANONYMOUS                    = 0x20
        MAP_DENYWRITE                    = 0x800
        MAP_EXECUTABLE                   = 0x1000
        MAP_FILE                         = 0x0
        MAP_FIXED                        = 0x10
        MAP_GROWSDOWN                    = 0x100
        MAP_HUGETLB                      = 0x40000
        MAP_LOCKED                       = 0x2000
        MAP_NONBLOCK                     = 0x10000
        MAP_NORESERVE                    = 0x4000
        MAP_POPULATE                     = 0x8000
        MAP_PRIVATE                      = 0x2
        MAP_SHARED                       = 0x1
        MAP_STACK                        = 0x20000
        MAP_TYPE                         = 0xf
        MCL_CURRENT                      = 0x1
        MCL_FUTURE                       = 0x2
        MNT_DETACH                       = 0x2
        MNT_EXPIRE                       = 0x4
        MNT_FORCE                        = 0x1
        MSG_CMSG_CLOEXEC                 = 0x40000000
        MSG_CONFIRM                      = 0x800
        MSG_CTRUNC                       = 0x8
        MSG_DONTROUTE                    = 0x4
        MSG_DONTWAIT                     = 0x40
        MSG_EOR                          = 0x80
        MSG_ERRQUEUE                     = 0x2000
        MSG_FASTOPEN                     = 0x20000000
        MSG_FIN                          = 0x200
        MSG_MORE                         = 0x8000
        MSG_NOSIGNAL                     = 0x4000
        MSG_OOB                          = 0x1
        MSG_PEEK                         = 0x2
        MSG_PROXY                        = 0x10
        MSG_RST                          = 0x1000
        MSG_SYN                          = 0x400
        MSG_TRUNC                        = 0x20
        MSG_TRYHARD                      = 0x4
        MSG_WAITALL                      = 0x100
        MSG_WAITFORONE                   = 0x10000
        MS_ACTIVE                        = 0x40000000
        MS_ASYNC                         = 0x1
        MS_BIND                          = 0x1000
        MS_DIRSYNC                       = 0x80
        MS_INVALIDATE                    = 0x2
        MS_I_VERSION                     = 0x800000
        MS_KERNMOUNT                     = 0x400000
        MS_MANDLOCK                      = 0x40
        MS_MGC_MSK                       = 0xffff0000
        MS_MGC_VAL                       = 0xc0ed0000
        MS_MOVE                          = 0x2000
        MS_NOATIME                       = 0x400
        MS_NODEV                         = 0x4
        MS_NODIRATIME                    = 0x800
        MS_NOEXEC                        = 0x8
        MS_NOSUID                        = 0x2
        MS_NOUSER                        = -0x80000000
        MS_POSIXACL                      = 0x10000
        MS_PRIVATE                       = 0x40000
        MS_RDONLY                        = 0x1
        MS_REC                           = 0x4000
        MS_RELATIME                      = 0x200000
        MS_REMOUNT                       = 0x20
        MS_RMT_MASK                      = 0x800051
        MS_SHARED                        = 0x100000
        MS_SILENT                        = 0x8000
        MS_SLAVE                         = 0x80000
        MS_STRICTATIME                   = 0x1000000
        MS_SYNC                          = 0x4
        MS_SYNCHRONOUS                   = 0x10
        MS_UNBINDABLE                    = 0x20000
        NAME_MAX                         = 0xff
        NETLINK_ADD_MEMBERSHIP           = 0x1
        NETLINK_AUDIT                    = 0x9
        NETLINK_BROADCAST_ERROR          = 0x4
        NETLINK_CONNECTOR                = 0xb
        NETLINK_DNRTMSG                  = 0xe
        NETLINK_DROP_MEMBERSHIP          = 0x2
        NETLINK_ECRYPTFS                 = 0x13
        NETLINK_FIB_LOOKUP               = 0xa
        NETLINK_FIREWALL                 = 0x3
        NETLINK_GENERIC                  = 0x10
        NETLINK_INET_DIAG                = 0x4
        NETLINK_IP6_FW                   = 0xd
        NETLINK_ISCSI                    = 0x8
        NETLINK_KOBJECT_UEVENT           = 0xf
        NETLINK_NETFILTER                = 0xc
        NETLINK_NFLOG                    = 0x5
        NETLINK_NO_ENOBUFS               = 0x5
        NETLINK_PKTINFO                  = 0x3
        NETLINK_ROUTE                    = 0x0
        NETLINK_SCSITRANSPORT            = 0x12
        NETLINK_SELINUX                  = 0x7
        NETLINK_UNUSED                   = 0x1
        NETLINK_USERSOCK                 = 0x2
        NETLINK_XFRM                     = 0x6
        NLA_ALIGNTO                      = 0x4
        NLA_F_NESTED                     = 0x8000
        NLA_F_NET_BYTEORDER              = 0x4000
        NLA_HDRLEN                       = 0x4
        NLMSG_ALIGNTO                    = 0x4
        NLMSG_DONE                       = 0x3
        NLMSG_ERROR                      = 0x2
        NLMSG_HDRLEN                     = 0x10
        NLMSG_MIN_TYPE                   = 0x10
        NLMSG_NOOP                       = 0x1
        NLMSG_OVERRUN                    = 0x4
        NLM_F_ACK                        = 0x4
        NLM_F_APPEND                     = 0x800
        NLM_F_ATOMIC                     = 0x400
        NLM_F_CREATE                     = 0x400
        NLM_F_DUMP                       = 0x300
        NLM_F_ECHO                       = 0x8
        NLM_F_EXCL                       = 0x200
        NLM_F_MATCH                      = 0x200
        NLM_F_MULTI                      = 0x2
        NLM_F_REPLACE                    = 0x100
        NLM_F_REQUEST                    = 0x1
        NLM_F_ROOT                       = 0x100
        O_ACCMODE                        = 0x3
        O_APPEND                         = 0x400
        O_ASYNC                          = 0x2000
        O_CLOEXEC                        = 0x80000
        O_CREAT                          = 0x40
        O_DIRECT                         = 0x4000
        O_DIRECTORY                      = 0x10000
        O_DSYNC                          = 0x1000
        O_EXCL                           = 0x80
        O_FSYNC                          = 0x101000
        O_LARGEFILE                      = 0x0
        O_NDELAY                         = 0x800
        O_NOATIME                        = 0x40000
        O_NOCTTY                         = 0x100
        O_NOFOLLOW                       = 0x20000
        O_NONBLOCK                       = 0x800
        O_RDONLY                         = 0x0
        O_RDWR                           = 0x2
        O_RSYNC                          = 0x101000
        O_SYNC                           = 0x101000
        O_TRUNC                          = 0x200
        O_WRONLY                         = 0x1
        PACKET_ADD_MEMBERSHIP            = 0x1
        PACKET_BROADCAST                 = 0x1
        PACKET_DROP_MEMBERSHIP           = 0x2
        PACKET_FASTROUTE                 = 0x6
        PACKET_HOST                      = 0x0
        PACKET_LOOPBACK                  = 0x5
        PACKET_MR_ALLMULTI               = 0x2
        PACKET_MR_MULTICAST              = 0x0
        PACKET_MR_PROMISC                = 0x1
        PACKET_MULTICAST                 = 0x2
        PACKET_OTHERHOST                 = 0x3
        PACKET_OUTGOING                  = 0x4
        PACKET_RECV_OUTPUT               = 0x3
        PACKET_RX_RING                   = 0x5
        PACKET_STATISTICS                = 0x6
        PRIO_PGRP                        = 0x1
        PRIO_PROCESS                     = 0x0
        PRIO_USER                        = 0x2
        PROT_EXEC                        = 0x4
        PROT_GROWSDOWN                   = 0x1000000
        PROT_GROWSUP                     = 0x2000000
        PROT_NONE                        = 0x0
        PROT_READ                        = 0x1
        PROT_WRITE                       = 0x2
        PR_CAPBSET_DROP                  = 0x18
        PR_CAPBSET_READ                  = 0x17
        PR_ENDIAN_BIG                    = 0x0
        PR_ENDIAN_LITTLE                 = 0x1
        PR_ENDIAN_PPC_LITTLE             = 0x2
        PR_FPEMU_NOPRINT                 = 0x1
        PR_FPEMU_SIGFPE                  = 0x2
        PR_FP_EXC_ASYNC                  = 0x2
        PR_FP_EXC_DISABLED               = 0x0
        PR_FP_EXC_DIV                    = 0x10000
        PR_FP_EXC_INV                    = 0x100000
        PR_FP_EXC_NONRECOV               = 0x1
        PR_FP_EXC_OVF                    = 0x20000
        PR_FP_EXC_PRECISE                = 0x3
        PR_FP_EXC_RES                    = 0x80000
        PR_FP_EXC_SW_ENABLE              = 0x80
        PR_FP_EXC_UND                    = 0x40000
        PR_GET_DUMPABLE                  = 0x3
        PR_GET_ENDIAN                    = 0x13
        PR_GET_FPEMU                     = 0x9
        PR_GET_FPEXC                     = 0xb
        PR_GET_KEEPCAPS                  = 0x7
        PR_GET_NAME                      = 0x10
        PR_GET_PDEATHSIG                 = 0x2
        PR_GET_SECCOMP                   = 0x15
        PR_GET_SECUREBITS                = 0x1b
        PR_GET_TIMERSLACK                = 0x1e
        PR_GET_TIMING                    = 0xd
        PR_GET_TSC                       = 0x19
        PR_GET_UNALIGN                   = 0x5
        PR_MCE_KILL                      = 0x21
        PR_MCE_KILL_CLEAR                = 0x0
        PR_MCE_KILL_DEFAULT              = 0x2
        PR_MCE_KILL_EARLY                = 0x1
        PR_MCE_KILL_GET                  = 0x22
        PR_MCE_KILL_LATE                 = 0x0
        PR_MCE_KILL_SET                  = 0x1
        PR_SET_DUMPABLE                  = 0x4
        PR_SET_ENDIAN                    = 0x14
        PR_SET_FPEMU                     = 0xa
        PR_SET_FPEXC                     = 0xc
        PR_SET_KEEPCAPS                  = 0x8
        PR_SET_NAME                      = 0xf
        PR_SET_PDEATHSIG                 = 0x1
        PR_SET_PTRACER                   = 0x59616d61
        PR_SET_SECCOMP                   = 0x16
        PR_SET_SECUREBITS                = 0x1c
        PR_SET_TIMERSLACK                = 0x1d
        PR_SET_TIMING                    = 0xe
        PR_SET_TSC                       = 0x1a
        PR_SET_UNALIGN                   = 0x6
        PR_TASK_PERF_EVENTS_DISABLE      = 0x1f
        PR_TASK_PERF_EVENTS_ENABLE       = 0x20
        PR_TIMING_STATISTICAL            = 0x0
        PR_TIMING_TIMESTAMP              = 0x1
        PR_TSC_ENABLE                    = 0x1
        PR_TSC_SIGSEGV                   = 0x2
        PR_UNALIGN_NOPRINT               = 0x1
        PR_UNALIGN_SIGBUS                = 0x2
        PTRACE_ARCH_PRCTL                = 0x1e
        PTRACE_ATTACH                    = 0x10
        PTRACE_CONT                      = 0x7
        PTRACE_DETACH                    = 0x11
        PTRACE_EVENT_CLONE               = 0x3
        PTRACE_EVENT_EXEC                = 0x4
        PTRACE_EVENT_EXIT                = 0x6
        PTRACE_EVENT_FORK                = 0x1
        PTRACE_EVENT_VFORK               = 0x2
        PTRACE_EVENT_VFORK_DONE          = 0x5
        PTRACE_GETEVENTMSG               = 0x4201
        PTRACE_GETFPREGS                 = 0xe
        PTRACE_GETFPXREGS                = 0x12
        PTRACE_GETREGS                   = 0xc
        PTRACE_GETREGSET                 = 0x4204
        PTRACE_GETSIGINFO                = 0x4202
        PTRACE_GET_THREAD_AREA           = 0x19
        PTRACE_KILL                      = 0x8
        PTRACE_OLDSETOPTIONS             = 0x15
        PTRACE_O_MASK                    = 0x7f
        PTRACE_O_TRACECLONE              = 0x8
        PTRACE_O_TRACEEXEC               = 0x10
        PTRACE_O_TRACEEXIT               = 0x40
        PTRACE_O_TRACEFORK               = 0x2
        PTRACE_O_TRACESYSGOOD            = 0x1
        PTRACE_O_TRACEVFORK              = 0x4
        PTRACE_O_TRACEVFORKDONE          = 0x20
        PTRACE_PEEKDATA                  = 0x2
        PTRACE_PEEKTEXT                  = 0x1
        PTRACE_PEEKUSR                   = 0x3
        PTRACE_POKEDATA                  = 0x5
        PTRACE_POKETEXT                  = 0x4
        PTRACE_POKEUSR                   = 0x6
        PTRACE_SETFPREGS                 = 0xf
        PTRACE_SETFPXREGS                = 0x13
        PTRACE_SETOPTIONS                = 0x4200
        PTRACE_SETREGS                   = 0xd
        PTRACE_SETREGSET                 = 0x4205
        PTRACE_SETSIGINFO                = 0x4203
        PTRACE_SET_THREAD_AREA           = 0x1a
        PTRACE_SINGLEBLOCK               = 0x21
        PTRACE_SINGLESTEP                = 0x9
        PTRACE_SYSCALL                   = 0x18
        PTRACE_SYSEMU                    = 0x1f
        PTRACE_SYSEMU_SINGLESTEP         = 0x20
        PTRACE_TRACEME                   = 0x0
        RLIMIT_AS                        = 0x9
        RLIMIT_CORE                      = 0x4
        RLIMIT_CPU                       = 0x0
        RLIMIT_DATA                      = 0x2
        RLIMIT_FSIZE                     = 0x1
        RLIMIT_NOFILE                    = 0x7
        RLIMIT_STACK                     = 0x3
        RLIM_INFINITY                    = -0x1
        RTAX_ADVMSS                      = 0x8
        RTAX_CWND                        = 0x7
        RTAX_FEATURES                    = 0xc
        RTAX_FEATURE_ALLFRAG             = 0x8
        RTAX_FEATURE_ECN                 = 0x1
        RTAX_FEATURE_SACK                = 0x2
        RTAX_FEATURE_TIMESTAMP           = 0x4
        RTAX_HOPLIMIT                    = 0xa
        RTAX_INITCWND                    = 0xb
        RTAX_INITRWND                    = 0xe
        RTAX_LOCK                        = 0x1
        RTAX_MAX                         = 0xe
        RTAX_MTU                         = 0x2
        RTAX_REORDERING                  = 0x9
        RTAX_RTO_MIN                     = 0xd
        RTAX_RTT                         = 0x4
        RTAX_RTTVAR                      = 0x5
        RTAX_SSTHRESH                    = 0x6
        RTAX_UNSPEC                      = 0x0
        RTAX_WINDOW                      = 0x3
        RTA_ALIGNTO                      = 0x4
        RTA_MAX                          = 0x10
        RTCF_DIRECTSRC                   = 0x4000000
        RTCF_DOREDIRECT                  = 0x1000000
        RTCF_LOG                         = 0x2000000
        RTCF_MASQ                        = 0x400000
        RTCF_NAT                         = 0x800000
        RTCF_VALVE                       = 0x200000
        RTF_ADDRCLASSMASK                = 0xf8000000
        RTF_ADDRCONF                     = 0x40000
        RTF_ALLONLINK                    = 0x20000
        RTF_BROADCAST                    = 0x10000000
        RTF_CACHE                        = 0x1000000
        RTF_DEFAULT                      = 0x10000
        RTF_DYNAMIC                      = 0x10
        RTF_FLOW                         = 0x2000000
        RTF_GATEWAY                      = 0x2
        RTF_HOST                         = 0x4
        RTF_INTERFACE                    = 0x40000000
        RTF_IRTT                         = 0x100
        RTF_LINKRT                       = 0x100000
        RTF_LOCAL                        = 0x80000000
        RTF_MODIFIED                     = 0x20
        RTF_MSS                          = 0x40
        RTF_MTU                          = 0x40
        RTF_MULTICAST                    = 0x20000000
        RTF_NAT                          = 0x8000000
        RTF_NOFORWARD                    = 0x1000
        RTF_NONEXTHOP                    = 0x200000
        RTF_NOPMTUDISC                   = 0x4000
        RTF_POLICY                       = 0x4000000
        RTF_REINSTATE                    = 0x8
        RTF_REJECT                       = 0x200
        RTF_STATIC                       = 0x400
        RTF_THROW                        = 0x2000
        RTF_UP                           = 0x1
        RTF_WINDOW                       = 0x80
        RTF_XRESOLVE                     = 0x800
        RTM_BASE                         = 0x10
        RTM_DELACTION                    = 0x31
        RTM_DELADDR                      = 0x15
        RTM_DELADDRLABEL                 = 0x49
        RTM_DELLINK                      = 0x11
        RTM_DELNEIGH                     = 0x1d
        RTM_DELQDISC                     = 0x25
        RTM_DELROUTE                     = 0x19
        RTM_DELRULE                      = 0x21
        RTM_DELTCLASS                    = 0x29
        RTM_DELTFILTER                   = 0x2d
        RTM_F_CLONED                     = 0x200
        RTM_F_EQUALIZE                   = 0x400
        RTM_F_NOTIFY                     = 0x100
        RTM_F_PREFIX                     = 0x800
        RTM_GETACTION                    = 0x32
        RTM_GETADDR                      = 0x16
        RTM_GETADDRLABEL                 = 0x4a
        RTM_GETANYCAST                   = 0x3e
        RTM_GETDCB                       = 0x4e
        RTM_GETLINK                      = 0x12
        RTM_GETMULTICAST                 = 0x3a
        RTM_GETNEIGH                     = 0x1e
        RTM_GETNEIGHTBL                  = 0x42
        RTM_GETQDISC                     = 0x26
        RTM_GETROUTE                     = 0x1a
        RTM_GETRULE                      = 0x22
        RTM_GETTCLASS                    = 0x2a
        RTM_GETTFILTER                   = 0x2e
        RTM_MAX                          = 0x4f
        RTM_NEWACTION                    = 0x30
        RTM_NEWADDR                      = 0x14
        RTM_NEWADDRLABEL                 = 0x48
        RTM_NEWLINK                      = 0x10
        RTM_NEWNDUSEROPT                 = 0x44
        RTM_NEWNEIGH                     = 0x1c
        RTM_NEWNEIGHTBL                  = 0x40
        RTM_NEWPREFIX                    = 0x34
        RTM_NEWQDISC                     = 0x24
        RTM_NEWROUTE                     = 0x18
        RTM_NEWRULE                      = 0x20
        RTM_NEWTCLASS                    = 0x28
        RTM_NEWTFILTER                   = 0x2c
        RTM_NR_FAMILIES                  = 0x10
        RTM_NR_MSGTYPES                  = 0x40
        RTM_SETDCB                       = 0x4f
        RTM_SETLINK                      = 0x13
        RTM_SETNEIGHTBL                  = 0x43
        RTNH_ALIGNTO                     = 0x4
        RTNH_F_DEAD                      = 0x1
        RTNH_F_ONLINK                    = 0x4
        RTNH_F_PERVASIVE                 = 0x2
        RTN_MAX                          = 0xb
        RTPROT_BIRD                      = 0xc
        RTPROT_BOOT                      = 0x3
        RTPROT_DHCP                      = 0x10
        RTPROT_DNROUTED                  = 0xd
        RTPROT_GATED                     = 0x8
        RTPROT_KERNEL                    = 0x2
        RTPROT_MRT                       = 0xa
        RTPROT_NTK                       = 0xf
        RTPROT_RA                        = 0x9
        RTPROT_REDIRECT                  = 0x1
        RTPROT_STATIC                    = 0x4
        RTPROT_UNSPEC                    = 0x0
        RTPROT_XORP                      = 0xe
        RTPROT_ZEBRA                     = 0xb
        RT_CLASS_DEFAULT                 = 0xfd
        RT_CLASS_LOCAL                   = 0xff
        RT_CLASS_MAIN                    = 0xfe
        RT_CLASS_MAX                     = 0xff
        RT_CLASS_UNSPEC                  = 0x0
        RUSAGE_CHILDREN                  = -0x1
        RUSAGE_SELF                      = 0x0
        RUSAGE_THREAD                    = 0x1
        SCM_CREDENTIALS                  = 0x2
        SCM_RIGHTS                       = 0x1
        SCM_TIMESTAMP                    = 0x1d
        SCM_TIMESTAMPING                 = 0x25
        SCM_TIMESTAMPNS                  = 0x23
        SHUT_RD                          = 0x0
        SHUT_RDWR                        = 0x2
        SHUT_WR                          = 0x1
        SIOCADDDLCI                      = 0x8980
        SIOCADDMULTI                     = 0x8931
        SIOCADDRT                        = 0x890b
        SIOCATMARK                       = 0x8905
        SIOCDARP                         = 0x8953
        SIOCDELDLCI                      = 0x8981
        SIOCDELMULTI                     = 0x8932
        SIOCDELRT                        = 0x890c
        SIOCDEVPRIVATE                   = 0x89f0
        SIOCDIFADDR                      = 0x8936
        SIOCDRARP                        = 0x8960
        SIOCGARP                         = 0x8954
        SIOCGIFADDR                      = 0x8915
        SIOCGIFBR                        = 0x8940
        SIOCGIFBRDADDR                   = 0x8919
        SIOCGIFCONF                      = 0x8912
        SIOCGIFCOUNT                     = 0x8938
        SIOCGIFDSTADDR                   = 0x8917
        SIOCGIFENCAP                     = 0x8925
        SIOCGIFFLAGS                     = 0x8913
        SIOCGIFHWADDR                    = 0x8927
        SIOCGIFINDEX                     = 0x8933
        SIOCGIFMAP                       = 0x8970
        SIOCGIFMEM                       = 0x891f
        SIOCGIFMETRIC                    = 0x891d
        SIOCGIFMTU                       = 0x8921
        SIOCGIFNAME                      = 0x8910
        SIOCGIFNETMASK                   = 0x891b
        SIOCGIFPFLAGS                    = 0x8935
        SIOCGIFSLAVE                     = 0x8929
        SIOCGIFTXQLEN                    = 0x8942
        SIOCGPGRP                        = 0x8904
        SIOCGRARP                        = 0x8961
        SIOCGSTAMP                       = 0x8906
        SIOCGSTAMPNS                     = 0x8907
        SIOCPROTOPRIVATE                 = 0x89e0
        SIOCRTMSG                        = 0x890d
        SIOCSARP                         = 0x8955
        SIOCSIFADDR                      = 0x8916
        SIOCSIFBR                        = 0x8941
        SIOCSIFBRDADDR                   = 0x891a
        SIOCSIFDSTADDR                   = 0x8918
        SIOCSIFENCAP                     = 0x8926
        SIOCSIFFLAGS                     = 0x8914
        SIOCSIFHWADDR                    = 0x8924
        SIOCSIFHWBROADCAST               = 0x8937
        SIOCSIFLINK                      = 0x8911
        SIOCSIFMAP                       = 0x8971
        SIOCSIFMEM                       = 0x8920
        SIOCSIFMETRIC                    = 0x891e
        SIOCSIFMTU                       = 0x8922
        SIOCSIFNAME                      = 0x8923
        SIOCSIFNETMASK                   = 0x891c
        SIOCSIFPFLAGS                    = 0x8934
        SIOCSIFSLAVE                     = 0x8930
        SIOCSIFTXQLEN                    = 0x8943
        SIOCSPGRP                        = 0x8902
        SIOCSRARP                        = 0x8962
        SOCK_CLOEXEC                     = 0x80000
        SOCK_DCCP                        = 0x6
        SOCK_DGRAM                       = 0x2
        SOCK_NONBLOCK                    = 0x800
        SOCK_PACKET                      = 0xa
        SOCK_RAW                         = 0x3
        SOCK_RDM                         = 0x4
        SOCK_SEQPACKET                   = 0x5
        SOCK_STREAM                      = 0x1
        SOL_AAL                          = 0x109
        SOL_ATM                          = 0x108
        SOL_DECNET                       = 0x105
        SOL_ICMPV6                       = 0x3a
        SOL_IP                           = 0x0
        SOL_IPV6                         = 0x29
        SOL_IRDA                         = 0x10a
        SOL_PACKET                       = 0x107
        SOL_RAW                          = 0xff
        SOL_SOCKET                       = 0x1
        SOL_TCP                          = 0x6
        SOL_X25                          = 0x106
        SOMAXCONN                        = 0x80
        SO_ACCEPTCONN                    = 0x1e
        SO_ATTACH_FILTER                 = 0x1a
        SO_BINDTODEVICE                  = 0x19
        SO_BROADCAST                     = 0x6
        SO_BSDCOMPAT                     = 0xe
        SO_DEBUG                         = 0x1
        SO_DETACH_FILTER                 = 0x1b
        SO_DOMAIN                        = 0x27
        SO_DONTROUTE                     = 0x5
        SO_ERROR                         = 0x4
        SO_KEEPALIVE                     = 0x9
        SO_LINGER                        = 0xd
        SO_MARK                          = 0x24
        SO_NO_CHECK                      = 0xb
        SO_OOBINLINE                     = 0xa
        SO_PASSCRED                      = 0x10
        SO_PASSSEC                       = 0x22
        SO_PEERCRED                      = 0x11
        SO_PEERNAME                      = 0x1c
        SO_PEERSEC                       = 0x1f
        SO_PRIORITY                      = 0xc
        SO_PROTOCOL                      = 0x26
        SO_RCVBUF                        = 0x8
        SO_RCVBUFFORCE                   = 0x21
        SO_RCVLOWAT                      = 0x12
        SO_RCVTIMEO                      = 0x14
        SO_REUSEADDR                     = 0x2
        SO_RXQ_OVFL                      = 0x28
        SO_SECURITY_AUTHENTICATION       = 0x16
        SO_SECURITY_ENCRYPTION_NETWORK   = 0x18
        SO_SECURITY_ENCRYPTION_TRANSPORT = 0x17
        SO_SNDBUF                        = 0x7
        SO_SNDBUFFORCE                   = 0x20
        SO_SNDLOWAT                      = 0x13
        SO_SNDTIMEO                      = 0x15
        SO_TIMESTAMP                     = 0x1d
        SO_TIMESTAMPING                  = 0x25
        SO_TIMESTAMPNS                   = 0x23
        SO_TYPE                          = 0x3
        S_BLKSIZE                        = 0x200
        S_IEXEC                          = 0x40
        S_IFBLK                          = 0x6000
        S_IFCHR                          = 0x2000
        S_IFDIR                          = 0x4000
        S_IFIFO                          = 0x1000
        S_IFLNK                          = 0xa000
        S_IFMT                           = 0xf000
        S_IFREG                          = 0x8000
        S_IFSOCK                         = 0xc000
        S_IREAD                          = 0x100
        S_IRGRP                          = 0x20
        S_IROTH                          = 0x4
        S_IRUSR                          = 0x100
        S_IRWXG                          = 0x38
        S_IRWXO                          = 0x7
        S_IRWXU                          = 0x1c0
        S_ISGID                          = 0x400
        S_ISUID                          = 0x800
        S_ISVTX                          = 0x200
        S_IWGRP                          = 0x10
        S_IWOTH                          = 0x2
        S_IWRITE                         = 0x80
        S_IWUSR                          = 0x80
        S_IXGRP                          = 0x8
        S_IXOTH                          = 0x1
        S_IXUSR                          = 0x40
        TCIFLUSH                         = 0x0
        TCIOFLUSH                        = 0x2
        TCOFLUSH                         = 0x1
        TCP_CONGESTION                   = 0xd
        TCP_CORK                         = 0x3
        TCP_DEFER_ACCEPT                 = 0x9
        TCP_INFO                         = 0xb
        TCP_KEEPCNT                      = 0x6
        TCP_KEEPIDLE                     = 0x4
        TCP_KEEPINTVL                    = 0x5
        TCP_LINGER2                      = 0x8
        TCP_MAXSEG                       = 0x2
        TCP_MAXWIN                       = 0xffff
        TCP_MAX_WINSHIFT                 = 0xe
        TCP_MD5SIG                       = 0xe
        TCP_MD5SIG_MAXKEYLEN             = 0x50
        TCP_MSS                          = 0x200
        TCP_NODELAY                      = 0x1
        TCP_QUICKACK                     = 0xc
        TCP_SYNCNT                       = 0x7
        TCP_WINDOW_CLAMP                 = 0xa
        TIOCCBRK                         = 0x5428
        TIOCCONS                         = 0x541d
        TIOCEXCL                         = 0x540c
        TIOCGDEV                         = 0x80045432
        TIOCGETD                         = 0x5424
        TIOCGICOUNT                      = 0x545d
        TIOCGLCKTRMIOS                   = 0x5456
        TIOCGPGRP                        = 0x540f
        TIOCGPTN                         = 0x80045430
        TIOCGRS485                       = 0x542e
        TIOCGSERIAL                      = 0x541e
        TIOCGSID                         = 0x5429
        TIOCGSOFTCAR                     = 0x5419
        TIOCGWINSZ                       = 0x5413
        TIOCINQ                          = 0x541b
        TIOCLINUX                        = 0x541c
        TIOCMBIC                         = 0x5417
        TIOCMBIS                         = 0x5416
        TIOCMGET                         = 0x5415
        TIOCMIWAIT                       = 0x545c
        TIOCMSET                         = 0x5418
        TIOCM_CAR                        = 0x40
        TIOCM_CD                         = 0x40
        TIOCM_CTS                        = 0x20
        TIOCM_DSR                        = 0x100
        TIOCM_DTR                        = 0x2
        TIOCM_LE                         = 0x1
        TIOCM_RI                         = 0x80
        TIOCM_RNG                        = 0x80
        TIOCM_RTS                        = 0x4
        TIOCM_SR                         = 0x10
        TIOCM_ST                         = 0x8
        TIOCNOTTY                        = 0x5422
        TIOCNXCL                         = 0x540d
        TIOCOUTQ                         = 0x5411
        TIOCPKT                          = 0x5420
        TIOCPKT_DATA                     = 0x0
        TIOCPKT_DOSTOP                   = 0x20
        TIOCPKT_FLUSHREAD                = 0x1
        TIOCPKT_FLUSHWRITE               = 0x2
        TIOCPKT_IOCTL                    = 0x40
        TIOCPKT_NOSTOP                   = 0x10
        TIOCPKT_START                    = 0x8
        TIOCPKT_STOP                     = 0x4
        TIOCSBRK                         = 0x5427
        TIOCSCTTY                        = 0x540e
        TIOCSERCONFIG                    = 0x5453
        TIOCSERGETLSR                    = 0x5459
        TIOCSERGETMULTI                  = 0x545a
        TIOCSERGSTRUCT                   = 0x5458
        TIOCSERGWILD                     = 0x5454
        TIOCSERSETMULTI                  = 0x545b
        TIOCSERSWILD                     = 0x5455
        TIOCSER_TEMT                     = 0x1
        TIOCSETD                         = 0x5423
        TIOCSIG                          = 0x40045436
        TIOCSLCKTRMIOS                   = 0x5457
        TIOCSPGRP                        = 0x5410
        TIOCSPTLCK                       = 0x40045431
        TIOCSRS485                       = 0x542f
        TIOCSSERIAL                      = 0x541f
        TIOCSSOFTCAR                     = 0x541a
        TIOCSTI                          = 0x5412
        TIOCSWINSZ                       = 0x5414
        TUNATTACHFILTER                  = 0x401054d5
        TUNDETACHFILTER                  = 0x401054d6
        TUNGETFEATURES                   = 0x800454cf
        TUNGETIFF                        = 0x800454d2
        TUNGETSNDBUF                     = 0x800454d3
        TUNGETVNETHDRSZ                  = 0x800454d7
        TUNSETDEBUG                      = 0x400454c9
        TUNSETGROUP                      = 0x400454ce
        TUNSETIFF                        = 0x400454ca
        TUNSETLINK                       = 0x400454cd
        TUNSETNOCSUM                     = 0x400454c8
        TUNSETOFFLOAD                    = 0x400454d0
        TUNSETOWNER                      = 0x400454cc
        TUNSETPERSIST                    = 0x400454cb
        TUNSETSNDBUF                     = 0x400454d4
        TUNSETTXFILTER                   = 0x400454d1
        TUNSETVNETHDRSZ                  = 0x400454d8
        WALL                             = 0x40000000
        WCLONE                           = 0x80000000
        WCONTINUED                       = 0x8
        WEXITED                          = 0x4
        WNOHANG                          = 0x1
        WNOTHREAD                        = 0x20000000
        WNOWAIT                          = 0x1000000
        WORDSIZE                         = 0x40
        WSTOPPED                         = 0x2
        WUNTRACED                        = 0x2
)
```

```golang
const (
        E2BIG           = Errno(0x7)
        EACCES          = Errno(0xd)
        EADDRINUSE      = Errno(0x62)
        EADDRNOTAVAIL   = Errno(0x63)
        EADV            = Errno(0x44)
        EAFNOSUPPORT    = Errno(0x61)
        EAGAIN          = Errno(0xb)
        EALREADY        = Errno(0x72)
        EBADE           = Errno(0x34)
        EBADF           = Errno(0x9)
        EBADFD          = Errno(0x4d)
        EBADMSG         = Errno(0x4a)
        EBADR           = Errno(0x35)
        EBADRQC         = Errno(0x38)
        EBADSLT         = Errno(0x39)
        EBFONT          = Errno(0x3b)
        EBUSY           = Errno(0x10)
        ECANCELED       = Errno(0x7d)
        ECHILD          = Errno(0xa)
        ECHRNG          = Errno(0x2c)
        ECOMM           = Errno(0x46)
        ECONNABORTED    = Errno(0x67)
        ECONNREFUSED    = Errno(0x6f)
        ECONNRESET      = Errno(0x68)
        EDEADLK         = Errno(0x23)
        EDEADLOCK       = Errno(0x23)
        EDESTADDRREQ    = Errno(0x59)
        EDOM            = Errno(0x21)
        EDOTDOT         = Errno(0x49)
        EDQUOT          = Errno(0x7a)
        EEXIST          = Errno(0x11)
        EFAULT          = Errno(0xe)
        EFBIG           = Errno(0x1b)
        EHOSTDOWN       = Errno(0x70)
        EHOSTUNREACH    = Errno(0x71)
        EIDRM           = Errno(0x2b)
        EILSEQ          = Errno(0x54)
        EINPROGRESS     = Errno(0x73)
        EINTR           = Errno(0x4)
        EINVAL          = Errno(0x16)
        EIO             = Errno(0x5)
        EISCONN         = Errno(0x6a)
        EISDIR          = Errno(0x15)
        EISNAM          = Errno(0x78)
        EKEYEXPIRED     = Errno(0x7f)
        EKEYREJECTED    = Errno(0x81)
        EKEYREVOKED     = Errno(0x80)
        EL2HLT          = Errno(0x33)
        EL2NSYNC        = Errno(0x2d)
        EL3HLT          = Errno(0x2e)
        EL3RST          = Errno(0x2f)
        ELIBACC         = Errno(0x4f)
        ELIBBAD         = Errno(0x50)
        ELIBEXEC        = Errno(0x53)
        ELIBMAX         = Errno(0x52)
        ELIBSCN         = Errno(0x51)
        ELNRNG          = Errno(0x30)
        ELOOP           = Errno(0x28)
        EMEDIUMTYPE     = Errno(0x7c)
        EMFILE          = Errno(0x18)
        EMLINK          = Errno(0x1f)
        EMSGSIZE        = Errno(0x5a)
        EMULTIHOP       = Errno(0x48)
        ENAMETOOLONG    = Errno(0x24)
        ENAVAIL         = Errno(0x77)
        ENETDOWN        = Errno(0x64)
        ENETRESET       = Errno(0x66)
        ENETUNREACH     = Errno(0x65)
        ENFILE          = Errno(0x17)
        ENOANO          = Errno(0x37)
        ENOBUFS         = Errno(0x69)
        ENOCSI          = Errno(0x32)
        ENODATA         = Errno(0x3d)
        ENODEV          = Errno(0x13)
        ENOENT          = Errno(0x2)
        ENOEXEC         = Errno(0x8)
        ENOKEY          = Errno(0x7e)
        ENOLCK          = Errno(0x25)
        ENOLINK         = Errno(0x43)
        ENOMEDIUM       = Errno(0x7b)
        ENOMEM          = Errno(0xc)
        ENOMSG          = Errno(0x2a)
        ENONET          = Errno(0x40)
        ENOPKG          = Errno(0x41)
        ENOPROTOOPT     = Errno(0x5c)
        ENOSPC          = Errno(0x1c)
        ENOSR           = Errno(0x3f)
        ENOSTR          = Errno(0x3c)
        ENOSYS          = Errno(0x26)
        ENOTBLK         = Errno(0xf)
        ENOTCONN        = Errno(0x6b)
        ENOTDIR         = Errno(0x14)
        ENOTEMPTY       = Errno(0x27)
        ENOTNAM         = Errno(0x76)
        ENOTRECOVERABLE = Errno(0x83)
        ENOTSOCK        = Errno(0x58)
        ENOTSUP         = Errno(0x5f)
        ENOTTY          = Errno(0x19)
        ENOTUNIQ        = Errno(0x4c)
        ENXIO           = Errno(0x6)
        EOPNOTSUPP      = Errno(0x5f)
        EOVERFLOW       = Errno(0x4b)
        EOWNERDEAD      = Errno(0x82)
        EPERM           = Errno(0x1)
        EPFNOSUPPORT    = Errno(0x60)
        EPIPE           = Errno(0x20)
        EPROTO          = Errno(0x47)
        EPROTONOSUPPORT = Errno(0x5d)
        EPROTOTYPE      = Errno(0x5b)
        ERANGE          = Errno(0x22)
        EREMCHG         = Errno(0x4e)
        EREMOTE         = Errno(0x42)
        EREMOTEIO       = Errno(0x79)
        ERESTART        = Errno(0x55)
        ERFKILL         = Errno(0x84)
        EROFS           = Errno(0x1e)
        ESHUTDOWN       = Errno(0x6c)
        ESOCKTNOSUPPORT = Errno(0x5e)
        ESPIPE          = Errno(0x1d)
        ESRCH           = Errno(0x3)
        ESRMNT          = Errno(0x45)
        ESTALE          = Errno(0x74)
        ESTRPIPE        = Errno(0x56)
        ETIME           = Errno(0x3e)
        ETIMEDOUT       = Errno(0x6e)
        ETOOMANYREFS    = Errno(0x6d)
        ETXTBSY         = Errno(0x1a)
        EUCLEAN         = Errno(0x75)
        EUNATCH         = Errno(0x31)
        EUSERS          = Errno(0x57)
        EWOULDBLOCK     = Errno(0xb)
        EXDEV           = Errno(0x12)
        EXFULL          = Errno(0x36)
)
```
Errors


```golang
const (
        SIGABRT   = Signal(0x6)
        SIGALRM   = Signal(0xe)
        SIGBUS    = Signal(0x7)
        SIGCHLD   = Signal(0x11)
        SIGCLD    = Signal(0x11)
        SIGCONT   = Signal(0x12)
        SIGFPE    = Signal(0x8)
        SIGHUP    = Signal(0x1)
        SIGILL    = Signal(0x4)
        SIGINT    = Signal(0x2)
        SIGIO     = Signal(0x1d)
        SIGIOT    = Signal(0x6)
        SIGKILL   = Signal(0x9)
        SIGPIPE   = Signal(0xd)
        SIGPOLL   = Signal(0x1d)
        SIGPROF   = Signal(0x1b)
        SIGPWR    = Signal(0x1e)
        SIGQUIT   = Signal(0x3)
        SIGSEGV   = Signal(0xb)
        SIGSTKFLT = Signal(0x10)
        SIGSTOP   = Signal(0x13)
        SIGSYS    = Signal(0x1f)
        SIGTERM   = Signal(0xf)
        SIGTRAP   = Signal(0x5)
        SIGTSTP   = Signal(0x14)
        SIGTTIN   = Signal(0x15)
        SIGTTOU   = Signal(0x16)
        SIGUNUSED = Signal(0x1f)
        SIGURG    = Signal(0x17)
        SIGUSR1   = Signal(0xa)
        SIGUSR2   = Signal(0xc)
        SIGVTALRM = Signal(0x1a)
        SIGWINCH  = Signal(0x1c)
        SIGXCPU   = Signal(0x18)
        SIGXFSZ   = Signal(0x19)
)
```
Signals


```golang
const (
        AF_802                        = 0x12
        AF_APPLETALK                  = 0x10
        AF_CCITT                      = 0xa
        AF_CHAOS                      = 0x5
        AF_DATAKIT                    = 0x9
        AF_DECnet                     = 0xc
        AF_DLI                        = 0xd
        AF_ECMA                       = 0x8
        AF_FILE                       = 0x1
        AF_GOSIP                      = 0x16
        AF_HYLINK                     = 0xf
        AF_IMPLINK                    = 0x3
        AF_INET                       = 0x2
        AF_INET6                      = 0x1a
        AF_INET_OFFLOAD               = 0x1e
        AF_IPX                        = 0x17
        AF_KEY                        = 0x1b
        AF_LAT                        = 0xe
        AF_LINK                       = 0x19
        AF_LOCAL                      = 0x1
        AF_MAX                        = 0x20
        AF_NBS                        = 0x7
        AF_NCA                        = 0x1c
        AF_NIT                        = 0x11
        AF_NS                         = 0x6
        AF_OSI                        = 0x13
        AF_OSINET                     = 0x15
        AF_PACKET                     = 0x20
        AF_POLICY                     = 0x1d
        AF_PUP                        = 0x4
        AF_ROUTE                      = 0x18
        AF_SNA                        = 0xb
        AF_TRILL                      = 0x1f
        AF_UNIX                       = 0x1
        AF_UNSPEC                     = 0x0
        AF_X25                        = 0x14
        ARPHRD_ARCNET                 = 0x7
        ARPHRD_ATM                    = 0x10
        ARPHRD_AX25                   = 0x3
        ARPHRD_CHAOS                  = 0x5
        ARPHRD_EETHER                 = 0x2
        ARPHRD_ETHER                  = 0x1
        ARPHRD_FC                     = 0x12
        ARPHRD_FRAME                  = 0xf
        ARPHRD_HDLC                   = 0x11
        ARPHRD_IB                     = 0x20
        ARPHRD_IEEE802                = 0x6
        ARPHRD_IPATM                  = 0x13
        ARPHRD_METRICOM               = 0x17
        ARPHRD_TUNNEL                 = 0x1f
        B0                            = 0x0
        B110                          = 0x3
        B115200                       = 0x12
        B1200                         = 0x9
        B134                          = 0x4
        B150                          = 0x5
        B153600                       = 0x13
        B1800                         = 0xa
        B19200                        = 0xe
        B200                          = 0x6
        B230400                       = 0x14
        B2400                         = 0xb
        B300                          = 0x7
        B307200                       = 0x15
        B38400                        = 0xf
        B460800                       = 0x16
        B4800                         = 0xc
        B50                           = 0x1
        B57600                        = 0x10
        B600                          = 0x8
        B75                           = 0x2
        B76800                        = 0x11
        B921600                       = 0x17
        B9600                         = 0xd
        BIOCFLUSH                     = 0x20004268
        BIOCGBLEN                     = 0x40044266
        BIOCGDLT                      = 0x4004426a
        BIOCGDLTLIST                  = -0x3fefbd89
        BIOCGDLTLIST32                = -0x3ff7bd89
        BIOCGETIF                     = 0x4020426b
        BIOCGETLIF                    = 0x4078426b
        BIOCGHDRCMPLT                 = 0x40044274
        BIOCGRTIMEOUT                 = 0x4010427b
        BIOCGRTIMEOUT32               = 0x4008427b
        BIOCGSEESENT                  = 0x40044278
        BIOCGSTATS                    = 0x4080426f
        BIOCGSTATSOLD                 = 0x4008426f
        BIOCIMMEDIATE                 = -0x7ffbbd90
        BIOCPROMISC                   = 0x20004269
        BIOCSBLEN                     = -0x3ffbbd9a
        BIOCSDLT                      = -0x7ffbbd8a
        BIOCSETF                      = -0x7fefbd99
        BIOCSETF32                    = -0x7ff7bd99
        BIOCSETIF                     = -0x7fdfbd94
        BIOCSETLIF                    = -0x7f87bd94
        BIOCSHDRCMPLT                 = -0x7ffbbd8b
        BIOCSRTIMEOUT                 = -0x7fefbd86
        BIOCSRTIMEOUT32               = -0x7ff7bd86
        BIOCSSEESENT                  = -0x7ffbbd87
        BIOCSTCPF                     = -0x7fefbd8e
        BIOCSUDPF                     = -0x7fefbd8d
        BIOCVERSION                   = 0x40044271
        BPF_A                         = 0x10
        BPF_ABS                       = 0x20
        BPF_ADD                       = 0x0
        BPF_ALIGNMENT                 = 0x4
        BPF_ALU                       = 0x4
        BPF_AND                       = 0x50
        BPF_B                         = 0x10
        BPF_DFLTBUFSIZE               = 0x100000
        BPF_DIV                       = 0x30
        BPF_H                         = 0x8
        BPF_IMM                       = 0x0
        BPF_IND                       = 0x40
        BPF_JA                        = 0x0
        BPF_JEQ                       = 0x10
        BPF_JGE                       = 0x30
        BPF_JGT                       = 0x20
        BPF_JMP                       = 0x5
        BPF_JSET                      = 0x40
        BPF_K                         = 0x0
        BPF_LD                        = 0x0
        BPF_LDX                       = 0x1
        BPF_LEN                       = 0x80
        BPF_LSH                       = 0x60
        BPF_MAJOR_VERSION             = 0x1
        BPF_MAXBUFSIZE                = 0x1000000
        BPF_MAXINSNS                  = 0x200
        BPF_MEM                       = 0x60
        BPF_MEMWORDS                  = 0x10
        BPF_MINBUFSIZE                = 0x20
        BPF_MINOR_VERSION             = 0x1
        BPF_MISC                      = 0x7
        BPF_MSH                       = 0xa0
        BPF_MUL                       = 0x20
        BPF_NEG                       = 0x80
        BPF_OR                        = 0x40
        BPF_RELEASE                   = 0x30bb6
        BPF_RET                       = 0x6
        BPF_RSH                       = 0x70
        BPF_ST                        = 0x2
        BPF_STX                       = 0x3
        BPF_SUB                       = 0x10
        BPF_TAX                       = 0x0
        BPF_TXA                       = 0x80
        BPF_W                         = 0x0
        BPF_X                         = 0x8
        BRKINT                        = 0x2
        CFLUSH                        = 0xf
        CLOCAL                        = 0x800
        CREAD                         = 0x80
        CS5                           = 0x0
        CS6                           = 0x10
        CS7                           = 0x20
        CS8                           = 0x30
        CSIZE                         = 0x30
        CSTART                        = 0x11
        CSTOP                         = 0x13
        CSTOPB                        = 0x40
        CSUSP                         = 0x1a
        CSWTCH                        = 0x1a
        DLT_AIRONET_HEADER            = 0x78
        DLT_APPLE_IP_OVER_IEEE1394    = 0x8a
        DLT_ARCNET                    = 0x7
        DLT_ARCNET_LINUX              = 0x81
        DLT_ATM_CLIP                  = 0x13
        DLT_ATM_RFC1483               = 0xb
        DLT_AURORA                    = 0x7e
        DLT_AX25                      = 0x3
        DLT_BACNET_MS_TP              = 0xa5
        DLT_CHAOS                     = 0x5
        DLT_CISCO_IOS                 = 0x76
        DLT_C_HDLC                    = 0x68
        DLT_DOCSIS                    = 0x8f
        DLT_ECONET                    = 0x73
        DLT_EN10MB                    = 0x1
        DLT_EN3MB                     = 0x2
        DLT_ENC                       = 0x6d
        DLT_ERF_ETH                   = 0xaf
        DLT_ERF_POS                   = 0xb0
        DLT_FDDI                      = 0xa
        DLT_FRELAY                    = 0x6b
        DLT_GCOM_SERIAL               = 0xad
        DLT_GCOM_T1E1                 = 0xac
        DLT_GPF_F                     = 0xab
        DLT_GPF_T                     = 0xaa
        DLT_GPRS_LLC                  = 0xa9
        DLT_HDLC                      = 0x10
        DLT_HHDLC                     = 0x79
        DLT_HIPPI                     = 0xf
        DLT_IBM_SN                    = 0x92
        DLT_IBM_SP                    = 0x91
        DLT_IEEE802                   = 0x6
        DLT_IEEE802_11                = 0x69
        DLT_IEEE802_11_RADIO          = 0x7f
        DLT_IEEE802_11_RADIO_AVS      = 0xa3
        DLT_IPNET                     = 0xe2
        DLT_IPOIB                     = 0xa2
        DLT_IP_OVER_FC                = 0x7a
        DLT_JUNIPER_ATM1              = 0x89
        DLT_JUNIPER_ATM2              = 0x87
        DLT_JUNIPER_CHDLC             = 0xb5
        DLT_JUNIPER_ES                = 0x84
        DLT_JUNIPER_ETHER             = 0xb2
        DLT_JUNIPER_FRELAY            = 0xb4
        DLT_JUNIPER_GGSN              = 0x85
        DLT_JUNIPER_MFR               = 0x86
        DLT_JUNIPER_MLFR              = 0x83
        DLT_JUNIPER_MLPPP             = 0x82
        DLT_JUNIPER_MONITOR           = 0xa4
        DLT_JUNIPER_PIC_PEER          = 0xae
        DLT_JUNIPER_PPP               = 0xb3
        DLT_JUNIPER_PPPOE             = 0xa7
        DLT_JUNIPER_PPPOE_ATM         = 0xa8
        DLT_JUNIPER_SERVICES          = 0x88
        DLT_LINUX_IRDA                = 0x90
        DLT_LINUX_LAPD                = 0xb1
        DLT_LINUX_SLL                 = 0x71
        DLT_LOOP                      = 0x6c
        DLT_LTALK                     = 0x72
        DLT_MTP2                      = 0x8c
        DLT_MTP2_WITH_PHDR            = 0x8b
        DLT_MTP3                      = 0x8d
        DLT_NULL                      = 0x0
        DLT_PCI_EXP                   = 0x7d
        DLT_PFLOG                     = 0x75
        DLT_PFSYNC                    = 0x12
        DLT_PPP                       = 0x9
        DLT_PPP_BSDOS                 = 0xe
        DLT_PPP_PPPD                  = 0xa6
        DLT_PRISM_HEADER              = 0x77
        DLT_PRONET                    = 0x4
        DLT_RAW                       = 0xc
        DLT_RAWAF_MASK                = 0x2240000
        DLT_RIO                       = 0x7c
        DLT_SCCP                      = 0x8e
        DLT_SLIP                      = 0x8
        DLT_SLIP_BSDOS                = 0xd
        DLT_SUNATM                    = 0x7b
        DLT_SYMANTEC_FIREWALL         = 0x63
        DLT_TZSP                      = 0x80
        ECHO                          = 0x8
        ECHOCTL                       = 0x200
        ECHOE                         = 0x10
        ECHOK                         = 0x20
        ECHOKE                        = 0x800
        ECHONL                        = 0x40
        ECHOPRT                       = 0x400
        EMPTY_SET                     = 0x0
        EMT_CPCOVF                    = 0x1
        EQUALITY_CHECK                = 0x0
        EXTA                          = 0xe
        EXTB                          = 0xf
        FD_CLOEXEC                    = 0x1
        FD_NFDBITS                    = 0x40
        FD_SETSIZE                    = 0x10000
        FLUSHALL                      = 0x1
        FLUSHDATA                     = 0x0
        FLUSHO                        = 0x2000
        F_ALLOCSP                     = 0xa
        F_ALLOCSP64                   = 0xa
        F_BADFD                       = 0x2e
        F_BLKSIZE                     = 0x13
        F_BLOCKS                      = 0x12
        F_CHKFL                       = 0x8
        F_COMPAT                      = 0x8
        F_DUP2FD                      = 0x9
        F_DUP2FD_CLOEXEC              = 0x24
        F_DUPFD                       = 0x0
        F_DUPFD_CLOEXEC               = 0x25
        F_FREESP                      = 0xb
        F_FREESP64                    = 0xb
        F_GETFD                       = 0x1
        F_GETFL                       = 0x3
        F_GETLK                       = 0xe
        F_GETLK64                     = 0xe
        F_GETOWN                      = 0x17
        F_GETXFL                      = 0x2d
        F_HASREMOTELOCKS              = 0x1a
        F_ISSTREAM                    = 0xd
        F_MANDDNY                     = 0x10
        F_MDACC                       = 0x20
        F_NODNY                       = 0x0
        F_NPRIV                       = 0x10
        F_PRIV                        = 0xf
        F_QUOTACTL                    = 0x11
        F_RDACC                       = 0x1
        F_RDDNY                       = 0x1
        F_RDLCK                       = 0x1
        F_REVOKE                      = 0x19
        F_RMACC                       = 0x4
        F_RMDNY                       = 0x4
        F_RWACC                       = 0x3
        F_RWDNY                       = 0x3
        F_SETFD                       = 0x2
        F_SETFL                       = 0x4
        F_SETLK                       = 0x6
        F_SETLK64                     = 0x6
        F_SETLK64_NBMAND              = 0x2a
        F_SETLKW                      = 0x7
        F_SETLKW64                    = 0x7
        F_SETLK_NBMAND                = 0x2a
        F_SETOWN                      = 0x18
        F_SHARE                       = 0x28
        F_SHARE_NBMAND                = 0x2b
        F_UNLCK                       = 0x3
        F_UNLKSYS                     = 0x4
        F_UNSHARE                     = 0x29
        F_WRACC                       = 0x2
        F_WRDNY                       = 0x2
        F_WRLCK                       = 0x2
        HUPCL                         = 0x400
        ICANON                        = 0x2
        ICRNL                         = 0x100
        IEXTEN                        = 0x8000
        IFF_ADDRCONF                  = 0x80000
        IFF_ALLMULTI                  = 0x200
        IFF_ANYCAST                   = 0x400000
        IFF_BROADCAST                 = 0x2
        IFF_CANTCHANGE                = 0x7f203003b5a
        IFF_COS_ENABLED               = 0x200000000
        IFF_DEBUG                     = 0x4
        IFF_DEPRECATED                = 0x40000
        IFF_DHCPRUNNING               = 0x4000
        IFF_DUPLICATE                 = 0x4000000000
        IFF_FAILED                    = 0x10000000
        IFF_FIXEDMTU                  = 0x1000000000
        IFF_INACTIVE                  = 0x40000000
        IFF_INTELLIGENT               = 0x400
        IFF_IPMP                      = 0x8000000000
        IFF_IPMP_CANTCHANGE           = 0x10000000
        IFF_IPMP_INVALID              = 0x1ec200080
        IFF_IPV4                      = 0x1000000
        IFF_IPV6                      = 0x2000000
        IFF_L3PROTECT                 = 0x40000000000
        IFF_LOOPBACK                  = 0x8
        IFF_MULTICAST                 = 0x800
        IFF_MULTI_BCAST               = 0x1000
        IFF_NOACCEPT                  = 0x4000000
        IFF_NOARP                     = 0x80
        IFF_NOFAILOVER                = 0x8000000
        IFF_NOLINKLOCAL               = 0x20000000000
        IFF_NOLOCAL                   = 0x20000
        IFF_NONUD                     = 0x200000
        IFF_NORTEXCH                  = 0x800000
        IFF_NOTRAILERS                = 0x20
        IFF_NOXMIT                    = 0x10000
        IFF_OFFLINE                   = 0x80000000
        IFF_POINTOPOINT               = 0x10
        IFF_PREFERRED                 = 0x400000000
        IFF_PRIVATE                   = 0x8000
        IFF_PROMISC                   = 0x100
        IFF_ROUTER                    = 0x100000
        IFF_RUNNING                   = 0x40
        IFF_STANDBY                   = 0x20000000
        IFF_TEMPORARY                 = 0x800000000
        IFF_UNNUMBERED                = 0x2000
        IFF_UP                        = 0x1
        IFF_VIRTUAL                   = 0x2000000000
        IFF_VRRP                      = 0x10000000000
        IFF_XRESOLV                   = 0x100000000
        IFNAMSIZ                      = 0x10
        IFT_1822                      = 0x2
        IFT_6TO4                      = 0xca
        IFT_AAL5                      = 0x31
        IFT_ARCNET                    = 0x23
        IFT_ARCNETPLUS                = 0x24
        IFT_ATM                       = 0x25
        IFT_CEPT                      = 0x13
        IFT_DS3                       = 0x1e
        IFT_EON                       = 0x19
        IFT_ETHER                     = 0x6
        IFT_FDDI                      = 0xf
        IFT_FRELAY                    = 0x20
        IFT_FRELAYDCE                 = 0x2c
        IFT_HDH1822                   = 0x3
        IFT_HIPPI                     = 0x2f
        IFT_HSSI                      = 0x2e
        IFT_HY                        = 0xe
        IFT_IB                        = 0xc7
        IFT_IPV4                      = 0xc8
        IFT_IPV6                      = 0xc9
        IFT_ISDNBASIC                 = 0x14
        IFT_ISDNPRIMARY               = 0x15
        IFT_ISO88022LLC               = 0x29
        IFT_ISO88023                  = 0x7
        IFT_ISO88024                  = 0x8
        IFT_ISO88025                  = 0x9
        IFT_ISO88026                  = 0xa
        IFT_LAPB                      = 0x10
        IFT_LOCALTALK                 = 0x2a
        IFT_LOOP                      = 0x18
        IFT_MIOX25                    = 0x26
        IFT_MODEM                     = 0x30
        IFT_NSIP                      = 0x1b
        IFT_OTHER                     = 0x1
        IFT_P10                       = 0xc
        IFT_P80                       = 0xd
        IFT_PARA                      = 0x22
        IFT_PPP                       = 0x17
        IFT_PROPMUX                   = 0x36
        IFT_PROPVIRTUAL               = 0x35
        IFT_PTPSERIAL                 = 0x16
        IFT_RS232                     = 0x21
        IFT_SDLC                      = 0x11
        IFT_SIP                       = 0x1f
        IFT_SLIP                      = 0x1c
        IFT_SMDSDXI                   = 0x2b
        IFT_SMDSICIP                  = 0x34
        IFT_SONET                     = 0x27
        IFT_SONETPATH                 = 0x32
        IFT_SONETVT                   = 0x33
        IFT_STARLAN                   = 0xb
        IFT_T1                        = 0x12
        IFT_ULTRA                     = 0x1d
        IFT_V35                       = 0x2d
        IFT_X25                       = 0x5
        IFT_X25DDN                    = 0x4
        IFT_X25PLE                    = 0x28
        IFT_XETHER                    = 0x1a
        IGNBRK                        = 0x1
        IGNCR                         = 0x80
        IGNPAR                        = 0x4
        IMAXBEL                       = 0x2000
        INLCR                         = 0x40
        INPCK                         = 0x10
        IN_AUTOCONF_MASK              = 0xffff0000
        IN_AUTOCONF_NET               = 0xa9fe0000
        IN_CLASSA_HOST                = 0xffffff
        IN_CLASSA_MAX                 = 0x80
        IN_CLASSA_NET                 = 0xff000000
        IN_CLASSA_NSHIFT              = 0x18
        IN_CLASSB_HOST                = 0xffff
        IN_CLASSB_MAX                 = 0x10000
        IN_CLASSB_NET                 = 0xffff0000
        IN_CLASSB_NSHIFT              = 0x10
        IN_CLASSC_HOST                = 0xff
        IN_CLASSC_NET                 = 0xffffff00
        IN_CLASSC_NSHIFT              = 0x8
        IN_CLASSD_HOST                = 0xfffffff
        IN_CLASSD_NET                 = 0xf0000000
        IN_CLASSD_NSHIFT              = 0x1c
        IN_CLASSE_NET                 = 0xffffffff
        IN_LOOPBACKNET                = 0x7f
        IN_PRIVATE12_MASK             = 0xfff00000
        IN_PRIVATE12_NET              = 0xac100000
        IN_PRIVATE16_MASK             = 0xffff0000
        IN_PRIVATE16_NET              = 0xc0a80000
        IN_PRIVATE8_MASK              = 0xff000000
        IN_PRIVATE8_NET               = 0xa000000
        IPPROTO_AH                    = 0x33
        IPPROTO_DSTOPTS               = 0x3c
        IPPROTO_EGP                   = 0x8
        IPPROTO_ENCAP                 = 0x4
        IPPROTO_EON                   = 0x50
        IPPROTO_ESP                   = 0x32
        IPPROTO_FRAGMENT              = 0x2c
        IPPROTO_GGP                   = 0x3
        IPPROTO_HELLO                 = 0x3f
        IPPROTO_HOPOPTS               = 0x0
        IPPROTO_ICMP                  = 0x1
        IPPROTO_ICMPV6                = 0x3a
        IPPROTO_IDP                   = 0x16
        IPPROTO_IGMP                  = 0x2
        IPPROTO_IP                    = 0x0
        IPPROTO_IPV6                  = 0x29
        IPPROTO_MAX                   = 0x100
        IPPROTO_ND                    = 0x4d
        IPPROTO_NONE                  = 0x3b
        IPPROTO_OSPF                  = 0x59
        IPPROTO_PIM                   = 0x67
        IPPROTO_PUP                   = 0xc
        IPPROTO_RAW                   = 0xff
        IPPROTO_ROUTING               = 0x2b
        IPPROTO_RSVP                  = 0x2e
        IPPROTO_SCTP                  = 0x84
        IPPROTO_TCP                   = 0x6
        IPPROTO_UDP                   = 0x11
        IPV6_ADD_MEMBERSHIP           = 0x9
        IPV6_BOUND_IF                 = 0x41
        IPV6_CHECKSUM                 = 0x18
        IPV6_DONTFRAG                 = 0x21
        IPV6_DROP_MEMBERSHIP          = 0xa
        IPV6_DSTOPTS                  = 0xf
        IPV6_FLOWINFO_FLOWLABEL       = 0xffff0f00
        IPV6_FLOWINFO_TCLASS          = 0xf00f
        IPV6_HOPLIMIT                 = 0xc
        IPV6_HOPOPTS                  = 0xe
        IPV6_JOIN_GROUP               = 0x9
        IPV6_LEAVE_GROUP              = 0xa
        IPV6_MULTICAST_HOPS           = 0x7
        IPV6_MULTICAST_IF             = 0x6
        IPV6_MULTICAST_LOOP           = 0x8
        IPV6_NEXTHOP                  = 0xd
        IPV6_PAD1_OPT                 = 0x0
        IPV6_PATHMTU                  = 0x25
        IPV6_PKTINFO                  = 0xb
        IPV6_PREFER_SRC_CGA           = 0x20
        IPV6_PREFER_SRC_CGADEFAULT    = 0x10
        IPV6_PREFER_SRC_CGAMASK       = 0x30
        IPV6_PREFER_SRC_COA           = 0x2
        IPV6_PREFER_SRC_DEFAULT       = 0x15
        IPV6_PREFER_SRC_HOME          = 0x1
        IPV6_PREFER_SRC_MASK          = 0x3f
        IPV6_PREFER_SRC_MIPDEFAULT    = 0x1
        IPV6_PREFER_SRC_MIPMASK       = 0x3
        IPV6_PREFER_SRC_NONCGA        = 0x10
        IPV6_PREFER_SRC_PUBLIC        = 0x4
        IPV6_PREFER_SRC_TMP           = 0x8
        IPV6_PREFER_SRC_TMPDEFAULT    = 0x4
        IPV6_PREFER_SRC_TMPMASK       = 0xc
        IPV6_RECVDSTOPTS              = 0x28
        IPV6_RECVHOPLIMIT             = 0x13
        IPV6_RECVHOPOPTS              = 0x14
        IPV6_RECVPATHMTU              = 0x24
        IPV6_RECVPKTINFO              = 0x12
        IPV6_RECVRTHDR                = 0x16
        IPV6_RECVRTHDRDSTOPTS         = 0x17
        IPV6_RECVTCLASS               = 0x19
        IPV6_RTHDR                    = 0x10
        IPV6_RTHDRDSTOPTS             = 0x11
        IPV6_RTHDR_TYPE_0             = 0x0
        IPV6_SEC_OPT                  = 0x22
        IPV6_SRC_PREFERENCES          = 0x23
        IPV6_TCLASS                   = 0x26
        IPV6_UNICAST_HOPS             = 0x5
        IPV6_UNSPEC_SRC               = 0x42
        IPV6_USE_MIN_MTU              = 0x20
        IPV6_V6ONLY                   = 0x27
        IP_ADD_MEMBERSHIP             = 0x13
        IP_ADD_SOURCE_MEMBERSHIP      = 0x17
        IP_BLOCK_SOURCE               = 0x15
        IP_BOUND_IF                   = 0x41
        IP_BROADCAST                  = 0x106
        IP_BROADCAST_TTL              = 0x43
        IP_DEFAULT_MULTICAST_LOOP     = 0x1
        IP_DEFAULT_MULTICAST_TTL      = 0x1
        IP_DF                         = 0x4000
        IP_DHCPINIT_IF                = 0x45
        IP_DONTFRAG                   = 0x1b
        IP_DONTROUTE                  = 0x105
        IP_DROP_MEMBERSHIP            = 0x14
        IP_DROP_SOURCE_MEMBERSHIP     = 0x18
        IP_HDRINCL                    = 0x2
        IP_MAXPACKET                  = 0xffff
        IP_MF                         = 0x2000
        IP_MSS                        = 0x240
        IP_MULTICAST_IF               = 0x10
        IP_MULTICAST_LOOP             = 0x12
        IP_MULTICAST_TTL              = 0x11
        IP_NEXTHOP                    = 0x19
        IP_OPTIONS                    = 0x1
        IP_PKTINFO                    = 0x1a
        IP_RECVDSTADDR                = 0x7
        IP_RECVIF                     = 0x9
        IP_RECVOPTS                   = 0x5
        IP_RECVPKTINFO                = 0x1a
        IP_RECVRETOPTS                = 0x6
        IP_RECVSLLA                   = 0xa
        IP_RECVTTL                    = 0xb
        IP_RETOPTS                    = 0x8
        IP_REUSEADDR                  = 0x104
        IP_SEC_OPT                    = 0x22
        IP_TOS                        = 0x3
        IP_TTL                        = 0x4
        IP_UNBLOCK_SOURCE             = 0x16
        IP_UNSPEC_SRC                 = 0x42
        ISIG                          = 0x1
        ISTRIP                        = 0x20
        IXANY                         = 0x800
        IXOFF                         = 0x1000
        IXON                          = 0x400
        MADV_ACCESS_DEFAULT           = 0x6
        MADV_ACCESS_LWP               = 0x7
        MADV_ACCESS_MANY              = 0x8
        MADV_DONTNEED                 = 0x4
        MADV_FREE                     = 0x5
        MADV_NORMAL                   = 0x0
        MADV_RANDOM                   = 0x1
        MADV_SEQUENTIAL               = 0x2
        MADV_WILLNEED                 = 0x3
        MAP_32BIT                     = 0x80
        MAP_ALIGN                     = 0x200
        MAP_ANON                      = 0x100
        MAP_ANONYMOUS                 = 0x100
        MAP_FIXED                     = 0x10
        MAP_INITDATA                  = 0x800
        MAP_NORESERVE                 = 0x40
        MAP_PRIVATE                   = 0x2
        MAP_RENAME                    = 0x20
        MAP_SHARED                    = 0x1
        MAP_TEXT                      = 0x400
        MAP_TYPE                      = 0xf
        MCL_CURRENT                   = 0x1
        MCL_FUTURE                    = 0x2
        MSG_CTRUNC                    = 0x10
        MSG_DONTROUTE                 = 0x4
        MSG_DONTWAIT                  = 0x80
        MSG_DUPCTRL                   = 0x800
        MSG_EOR                       = 0x8
        MSG_MAXIOVLEN                 = 0x10
        MSG_NOTIFICATION              = 0x100
        MSG_OOB                       = 0x1
        MSG_PEEK                      = 0x2
        MSG_TRUNC                     = 0x20
        MSG_WAITALL                   = 0x40
        MSG_XPG4_2                    = 0x8000
        MS_ASYNC                      = 0x1
        MS_INVALIDATE                 = 0x2
        MS_OLDSYNC                    = 0x0
        MS_SYNC                       = 0x4
        M_FLUSH                       = 0x86
        NOFLSH                        = 0x80
        OCRNL                         = 0x8
        OFDEL                         = 0x80
        OFILL                         = 0x40
        ONLCR                         = 0x4
        ONLRET                        = 0x20
        ONOCR                         = 0x10
        OPENFAIL                      = -0x1
        OPOST                         = 0x1
        O_ACCMODE                     = 0x600003
        O_APPEND                      = 0x8
        O_CLOEXEC                     = 0x800000
        O_CREAT                       = 0x100
        O_DSYNC                       = 0x40
        O_EXCL                        = 0x400
        O_EXEC                        = 0x400000
        O_LARGEFILE                   = 0x2000
        O_NDELAY                      = 0x4
        O_NOCTTY                      = 0x800
        O_NOFOLLOW                    = 0x20000
        O_NOLINKS                     = 0x40000
        O_NONBLOCK                    = 0x80
        O_RDONLY                      = 0x0
        O_RDWR                        = 0x2
        O_RSYNC                       = 0x8000
        O_SEARCH                      = 0x200000
        O_SIOCGIFCONF                 = -0x3ff796ec
        O_SIOCGLIFCONF                = -0x3fef9688
        O_SYNC                        = 0x10
        O_TRUNC                       = 0x200
        O_WRONLY                      = 0x1
        O_XATTR                       = 0x4000
        PARENB                        = 0x100
        PAREXT                        = 0x100000
        PARMRK                        = 0x8
        PARODD                        = 0x200
        PENDIN                        = 0x4000
        PRIO_PGRP                     = 0x1
        PRIO_PROCESS                  = 0x0
        PRIO_USER                     = 0x2
        PROT_EXEC                     = 0x4
        PROT_NONE                     = 0x0
        PROT_READ                     = 0x1
        PROT_WRITE                    = 0x2
        RLIMIT_AS                     = 0x6
        RLIMIT_CORE                   = 0x4
        RLIMIT_CPU                    = 0x0
        RLIMIT_DATA                   = 0x2
        RLIMIT_FSIZE                  = 0x1
        RLIMIT_NOFILE                 = 0x5
        RLIMIT_STACK                  = 0x3
        RLIM_INFINITY                 = -0x3
        RTAX_AUTHOR                   = 0x6
        RTAX_BRD                      = 0x7
        RTAX_DST                      = 0x0
        RTAX_GATEWAY                  = 0x1
        RTAX_GENMASK                  = 0x3
        RTAX_IFA                      = 0x5
        RTAX_IFP                      = 0x4
        RTAX_MAX                      = 0x9
        RTAX_NETMASK                  = 0x2
        RTAX_SRC                      = 0x8
        RTA_AUTHOR                    = 0x40
        RTA_BRD                       = 0x80
        RTA_DST                       = 0x1
        RTA_GATEWAY                   = 0x2
        RTA_GENMASK                   = 0x8
        RTA_IFA                       = 0x20
        RTA_IFP                       = 0x10
        RTA_NETMASK                   = 0x4
        RTA_NUMBITS                   = 0x9
        RTA_SRC                       = 0x100
        RTF_BLACKHOLE                 = 0x1000
        RTF_CLONING                   = 0x100
        RTF_DONE                      = 0x40
        RTF_DYNAMIC                   = 0x10
        RTF_GATEWAY                   = 0x2
        RTF_HOST                      = 0x4
        RTF_INDIRECT                  = 0x40000
        RTF_KERNEL                    = 0x80000
        RTF_LLINFO                    = 0x400
        RTF_MASK                      = 0x80
        RTF_MODIFIED                  = 0x20
        RTF_MULTIRT                   = 0x10000
        RTF_PRIVATE                   = 0x2000
        RTF_PROTO1                    = 0x8000
        RTF_PROTO2                    = 0x4000
        RTF_REJECT                    = 0x8
        RTF_SETSRC                    = 0x20000
        RTF_STATIC                    = 0x800
        RTF_UP                        = 0x1
        RTF_XRESOLVE                  = 0x200
        RTF_ZONE                      = 0x100000
        RTM_ADD                       = 0x1
        RTM_CHANGE                    = 0x3
        RTM_CHGADDR                   = 0xf
        RTM_DELADDR                   = 0xd
        RTM_DELETE                    = 0x2
        RTM_FREEADDR                  = 0x10
        RTM_GET                       = 0x4
        RTM_IFINFO                    = 0xe
        RTM_LOCK                      = 0x8
        RTM_LOSING                    = 0x5
        RTM_MISS                      = 0x7
        RTM_NEWADDR                   = 0xc
        RTM_OLDADD                    = 0x9
        RTM_OLDDEL                    = 0xa
        RTM_REDIRECT                  = 0x6
        RTM_RESOLVE                   = 0xb
        RTM_VERSION                   = 0x3
        RTV_EXPIRE                    = 0x4
        RTV_HOPCOUNT                  = 0x2
        RTV_MTU                       = 0x1
        RTV_RPIPE                     = 0x8
        RTV_RTT                       = 0x40
        RTV_RTTVAR                    = 0x80
        RTV_SPIPE                     = 0x10
        RTV_SSTHRESH                  = 0x20
        RT_AWARE                      = 0x1
        RUSAGE_CHILDREN               = -0x1
        RUSAGE_SELF                   = 0x0
        SCM_RIGHTS                    = 0x1010
        SCM_TIMESTAMP                 = 0x1013
        SCM_UCRED                     = 0x1012
        SHUT_RD                       = 0x0
        SHUT_RDWR                     = 0x2
        SHUT_WR                       = 0x1
        SIG2STR_MAX                   = 0x20
        SIOCADDMULTI                  = -0x7fdf96cf
        SIOCADDRT                     = -0x7fcf8df6
        SIOCATMARK                    = 0x40047307
        SIOCDARP                      = -0x7fdb96e0
        SIOCDELMULTI                  = -0x7fdf96ce
        SIOCDELRT                     = -0x7fcf8df5
        SIOCDIPSECONFIG               = -0x7ffb9669
        SIOCDXARP                     = -0x7fff9658
        SIOCFIPSECONFIG               = -0x7ffb966b
        SIOCGARP                      = -0x3fdb96e1
        SIOCGDSTINFO                  = -0x3fff965c
        SIOCGENADDR                   = -0x3fdf96ab
        SIOCGENPSTATS                 = -0x3fdf96c7
        SIOCGETLSGCNT                 = -0x3fef8deb
        SIOCGETNAME                   = 0x40107334
        SIOCGETPEER                   = 0x40107335
        SIOCGETPROP                   = -0x3fff8f44
        SIOCGETSGCNT                  = -0x3feb8deb
        SIOCGETSYNC                   = -0x3fdf96d3
        SIOCGETVIFCNT                 = -0x3feb8dec
        SIOCGHIWAT                    = 0x40047301
        SIOCGIFADDR                   = -0x3fdf96f3
        SIOCGIFBRDADDR                = -0x3fdf96e9
        SIOCGIFCONF                   = -0x3ff796a4
        SIOCGIFDSTADDR                = -0x3fdf96f1
        SIOCGIFFLAGS                  = -0x3fdf96ef
        SIOCGIFHWADDR                 = -0x3fdf9647
        SIOCGIFINDEX                  = -0x3fdf96a6
        SIOCGIFMEM                    = -0x3fdf96ed
        SIOCGIFMETRIC                 = -0x3fdf96e5
        SIOCGIFMTU                    = -0x3fdf96ea
        SIOCGIFMUXID                  = -0x3fdf96a8
        SIOCGIFNETMASK                = -0x3fdf96e7
        SIOCGIFNUM                    = 0x40046957
        SIOCGIP6ADDRPOLICY            = -0x3fff965e
        SIOCGIPMSFILTER               = -0x3ffb964c
        SIOCGLIFADDR                  = -0x3f87968f
        SIOCGLIFBINDING               = -0x3f879666
        SIOCGLIFBRDADDR               = -0x3f879685
        SIOCGLIFCONF                  = -0x3fef965b
        SIOCGLIFDADSTATE              = -0x3f879642
        SIOCGLIFDSTADDR               = -0x3f87968d
        SIOCGLIFFLAGS                 = -0x3f87968b
        SIOCGLIFGROUPINFO             = -0x3f4b9663
        SIOCGLIFGROUPNAME             = -0x3f879664
        SIOCGLIFHWADDR                = -0x3f879640
        SIOCGLIFINDEX                 = -0x3f87967b
        SIOCGLIFLNKINFO               = -0x3f879674
        SIOCGLIFMETRIC                = -0x3f879681
        SIOCGLIFMTU                   = -0x3f879686
        SIOCGLIFMUXID                 = -0x3f87967d
        SIOCGLIFNETMASK               = -0x3f879683
        SIOCGLIFNUM                   = -0x3ff3967e
        SIOCGLIFSRCOF                 = -0x3fef964f
        SIOCGLIFSUBNET                = -0x3f879676
        SIOCGLIFTOKEN                 = -0x3f879678
        SIOCGLIFUSESRC                = -0x3f879651
        SIOCGLIFZONE                  = -0x3f879656
        SIOCGLOWAT                    = 0x40047303
        SIOCGMSFILTER                 = -0x3ffb964e
        SIOCGPGRP                     = 0x40047309
        SIOCGSTAMP                    = -0x3fef9646
        SIOCGXARP                     = -0x3fff9659
        SIOCIFDETACH                  = -0x7fdf96c8
        SIOCILB                       = -0x3ffb9645
        SIOCLIFADDIF                  = -0x3f879691
        SIOCLIFDELND                  = -0x7f879673
        SIOCLIFGETND                  = -0x3f879672
        SIOCLIFREMOVEIF               = -0x7f879692
        SIOCLIFSETND                  = -0x7f879671
        SIOCLIPSECONFIG               = -0x7ffb9668
        SIOCLOWER                     = -0x7fdf96d7
        SIOCSARP                      = -0x7fdb96e2
        SIOCSCTPGOPT                  = -0x3fef9653
        SIOCSCTPPEELOFF               = -0x3ffb9652
        SIOCSCTPSOPT                  = -0x7fef9654
        SIOCSENABLESDP                = -0x3ffb9649
        SIOCSETPROP                   = -0x7ffb8f43
        SIOCSETSYNC                   = -0x7fdf96d4
        SIOCSHIWAT                    = -0x7ffb8d00
        SIOCSIFADDR                   = -0x7fdf96f4
        SIOCSIFBRDADDR                = -0x7fdf96e8
        SIOCSIFDSTADDR                = -0x7fdf96f2
        SIOCSIFFLAGS                  = -0x7fdf96f0
        SIOCSIFINDEX                  = -0x7fdf96a5
        SIOCSIFMEM                    = -0x7fdf96ee
        SIOCSIFMETRIC                 = -0x7fdf96e4
        SIOCSIFMTU                    = -0x7fdf96eb
        SIOCSIFMUXID                  = -0x7fdf96a7
        SIOCSIFNAME                   = -0x7fdf96b7
        SIOCSIFNETMASK                = -0x7fdf96e6
        SIOCSIP6ADDRPOLICY            = -0x7fff965d
        SIOCSIPMSFILTER               = -0x7ffb964b
        SIOCSIPSECONFIG               = -0x7ffb966a
        SIOCSLGETREQ                  = -0x3fdf96b9
        SIOCSLIFADDR                  = -0x7f879690
        SIOCSLIFBRDADDR               = -0x7f879684
        SIOCSLIFDSTADDR               = -0x7f87968e
        SIOCSLIFFLAGS                 = -0x7f87968c
        SIOCSLIFGROUPNAME             = -0x7f879665
        SIOCSLIFINDEX                 = -0x7f87967a
        SIOCSLIFLNKINFO               = -0x7f879675
        SIOCSLIFMETRIC                = -0x7f879680
        SIOCSLIFMTU                   = -0x7f879687
        SIOCSLIFMUXID                 = -0x7f87967c
        SIOCSLIFNAME                  = -0x3f87967f
        SIOCSLIFNETMASK               = -0x7f879682
        SIOCSLIFPREFIX                = -0x3f879641
        SIOCSLIFSUBNET                = -0x7f879677
        SIOCSLIFTOKEN                 = -0x7f879679
        SIOCSLIFUSESRC                = -0x7f879650
        SIOCSLIFZONE                  = -0x7f879655
        SIOCSLOWAT                    = -0x7ffb8cfe
        SIOCSLSTAT                    = -0x7fdf96b8
        SIOCSMSFILTER                 = -0x7ffb964d
        SIOCSPGRP                     = -0x7ffb8cf8
        SIOCSPROMISC                  = -0x7ffb96d0
        SIOCSQPTR                     = -0x3ffb9648
        SIOCSSDSTATS                  = -0x3fdf96d2
        SIOCSSESTATS                  = -0x3fdf96d1
        SIOCSXARP                     = -0x7fff965a
        SIOCTMYADDR                   = -0x3ff79670
        SIOCTMYSITE                   = -0x3ff7966e
        SIOCTONLINK                   = -0x3ff7966f
        SIOCUPPER                     = -0x7fdf96d8
        SIOCX25RCV                    = -0x3fdf96c4
        SIOCX25TBL                    = -0x3fdf96c3
        SIOCX25XMT                    = -0x3fdf96c5
        SIOCXPROTO                    = 0x20007337
        SOCK_CLOEXEC                  = 0x80000
        SOCK_DGRAM                    = 0x1
        SOCK_NDELAY                   = 0x200000
        SOCK_NONBLOCK                 = 0x100000
        SOCK_RAW                      = 0x4
        SOCK_RDM                      = 0x5
        SOCK_SEQPACKET                = 0x6
        SOCK_STREAM                   = 0x2
        SOCK_TYPE_MASK                = 0xffff
        SOL_FILTER                    = 0xfffc
        SOL_PACKET                    = 0xfffd
        SOL_ROUTE                     = 0xfffe
        SOL_SOCKET                    = 0xffff
        SOMAXCONN                     = 0x80
        SO_ACCEPTCONN                 = 0x2
        SO_ALL                        = 0x3f
        SO_ALLZONES                   = 0x1014
        SO_ANON_MLP                   = 0x100a
        SO_ATTACH_FILTER              = 0x40000001
        SO_BAND                       = 0x4000
        SO_BROADCAST                  = 0x20
        SO_COPYOPT                    = 0x80000
        SO_DEBUG                      = 0x1
        SO_DELIM                      = 0x8000
        SO_DETACH_FILTER              = 0x40000002
        SO_DGRAM_ERRIND               = 0x200
        SO_DOMAIN                     = 0x100c
        SO_DONTLINGER                 = -0x81
        SO_DONTROUTE                  = 0x10
        SO_ERROPT                     = 0x40000
        SO_ERROR                      = 0x1007
        SO_EXCLBIND                   = 0x1015
        SO_HIWAT                      = 0x10
        SO_ISNTTY                     = 0x800
        SO_ISTTY                      = 0x400
        SO_KEEPALIVE                  = 0x8
        SO_LINGER                     = 0x80
        SO_LOWAT                      = 0x20
        SO_MAC_EXEMPT                 = 0x100b
        SO_MAC_IMPLICIT               = 0x1016
        SO_MAXBLK                     = 0x100000
        SO_MAXPSZ                     = 0x8
        SO_MINPSZ                     = 0x4
        SO_MREADOFF                   = 0x80
        SO_MREADON                    = 0x40
        SO_NDELOFF                    = 0x200
        SO_NDELON                     = 0x100
        SO_NODELIM                    = 0x10000
        SO_OOBINLINE                  = 0x100
        SO_PROTOTYPE                  = 0x1009
        SO_RCVBUF                     = 0x1002
        SO_RCVLOWAT                   = 0x1004
        SO_RCVPSH                     = 0x100d
        SO_RCVTIMEO                   = 0x1006
        SO_READOPT                    = 0x1
        SO_RECVUCRED                  = 0x400
        SO_REUSEADDR                  = 0x4
        SO_SECATTR                    = 0x1011
        SO_SNDBUF                     = 0x1001
        SO_SNDLOWAT                   = 0x1003
        SO_SNDTIMEO                   = 0x1005
        SO_STRHOLD                    = 0x20000
        SO_TAIL                       = 0x200000
        SO_TIMESTAMP                  = 0x1013
        SO_TONSTOP                    = 0x2000
        SO_TOSTOP                     = 0x1000
        SO_TYPE                       = 0x1008
        SO_USELOOPBACK                = 0x40
        SO_VRRP                       = 0x1017
        SO_WROFF                      = 0x2
        TCFLSH                        = 0x5407
        TCIFLUSH                      = 0x0
        TCIOFLUSH                     = 0x2
        TCOFLUSH                      = 0x1
        TCP_ABORT_THRESHOLD           = 0x11
        TCP_ANONPRIVBIND              = 0x20
        TCP_CONN_ABORT_THRESHOLD      = 0x13
        TCP_CONN_NOTIFY_THRESHOLD     = 0x12
        TCP_CORK                      = 0x18
        TCP_EXCLBIND                  = 0x21
        TCP_INIT_CWND                 = 0x15
        TCP_KEEPALIVE                 = 0x8
        TCP_KEEPALIVE_ABORT_THRESHOLD = 0x17
        TCP_KEEPALIVE_THRESHOLD       = 0x16
        TCP_KEEPCNT                   = 0x23
        TCP_KEEPIDLE                  = 0x22
        TCP_KEEPINTVL                 = 0x24
        TCP_LINGER2                   = 0x1c
        TCP_MAXSEG                    = 0x2
        TCP_MSS                       = 0x218
        TCP_NODELAY                   = 0x1
        TCP_NOTIFY_THRESHOLD          = 0x10
        TCP_RECVDSTADDR               = 0x14
        TCP_RTO_INITIAL               = 0x19
        TCP_RTO_MAX                   = 0x1b
        TCP_RTO_MIN                   = 0x1a
        TCSAFLUSH                     = 0x5410
        TIOC                          = 0x5400
        TIOCCBRK                      = 0x747a
        TIOCCDTR                      = 0x7478
        TIOCCILOOP                    = 0x746c
        TIOCEXCL                      = 0x740d
        TIOCFLUSH                     = 0x7410
        TIOCGETC                      = 0x7412
        TIOCGETD                      = 0x7400
        TIOCGETP                      = 0x7408
        TIOCGLTC                      = 0x7474
        TIOCGPGRP                     = 0x7414
        TIOCGPPS                      = 0x547d
        TIOCGPPSEV                    = 0x547f
        TIOCGSID                      = 0x7416
        TIOCGSOFTCAR                  = 0x5469
        TIOCGWINSZ                    = 0x5468
        TIOCHPCL                      = 0x7402
        TIOCKBOF                      = 0x5409
        TIOCKBON                      = 0x5408
        TIOCLBIC                      = 0x747e
        TIOCLBIS                      = 0x747f
        TIOCLGET                      = 0x747c
        TIOCLSET                      = 0x747d
        TIOCMBIC                      = 0x741c
        TIOCMBIS                      = 0x741b
        TIOCMGET                      = 0x741d
        TIOCMSET                      = 0x741a
        TIOCM_CAR                     = 0x40
        TIOCM_CD                      = 0x40
        TIOCM_CTS                     = 0x20
        TIOCM_DSR                     = 0x100
        TIOCM_DTR                     = 0x2
        TIOCM_LE                      = 0x1
        TIOCM_RI                      = 0x80
        TIOCM_RNG                     = 0x80
        TIOCM_RTS                     = 0x4
        TIOCM_SR                      = 0x10
        TIOCM_ST                      = 0x8
        TIOCNOTTY                     = 0x7471
        TIOCNXCL                      = 0x740e
        TIOCOUTQ                      = 0x7473
        TIOCREMOTE                    = 0x741e
        TIOCSBRK                      = 0x747b
        TIOCSCTTY                     = 0x7484
        TIOCSDTR                      = 0x7479
        TIOCSETC                      = 0x7411
        TIOCSETD                      = 0x7401
        TIOCSETN                      = 0x740a
        TIOCSETP                      = 0x7409
        TIOCSIGNAL                    = 0x741f
        TIOCSILOOP                    = 0x746d
        TIOCSLTC                      = 0x7475
        TIOCSPGRP                     = 0x7415
        TIOCSPPS                      = 0x547e
        TIOCSSOFTCAR                  = 0x546a
        TIOCSTART                     = 0x746e
        TIOCSTI                       = 0x7417
        TIOCSTOP                      = 0x746f
        TIOCSWINSZ                    = 0x5467
        TOSTOP                        = 0x100
        VCEOF                         = 0x8
        VCEOL                         = 0x9
        VDISCARD                      = 0xd
        VDSUSP                        = 0xb
        VEOF                          = 0x4
        VEOL                          = 0x5
        VEOL2                         = 0x6
        VERASE                        = 0x2
        VINTR                         = 0x0
        VKILL                         = 0x3
        VLNEXT                        = 0xf
        VMIN                          = 0x4
        VQUIT                         = 0x1
        VREPRINT                      = 0xc
        VSTART                        = 0x8
        VSTOP                         = 0x9
        VSUSP                         = 0xa
        VSWTCH                        = 0x7
        VT0                           = 0x0
        VT1                           = 0x4000
        VTDLY                         = 0x4000
        VTIME                         = 0x5
        VWERASE                       = 0xe
        WCONTFLG                      = 0xffff
        WCONTINUED                    = 0x8
        WCOREFLG                      = 0x80
        WEXITED                       = 0x1
        WNOHANG                       = 0x40
        WNOWAIT                       = 0x80
        WOPTMASK                      = 0xcf
        WRAP                          = 0x20000
        WSIGMASK                      = 0x7f
        WSTOPFLG                      = 0x7f
        WSTOPPED                      = 0x4
        WTRAPPED                      = 0x2
        WUNTRACED                     = 0x4
)
```



```golang
const (
        E2BIG           = Errno(0x7)
        EACCES          = Errno(0xd)
        EADDRINUSE      = Errno(0x7d)
        EADDRNOTAVAIL   = Errno(0x7e)
        EADV            = Errno(0x44)
        EAFNOSUPPORT    = Errno(0x7c)
        EAGAIN          = Errno(0xb)
        EALREADY        = Errno(0x95)
        EBADE           = Errno(0x32)
        EBADF           = Errno(0x9)
        EBADFD          = Errno(0x51)
        EBADMSG         = Errno(0x4d)
        EBADR           = Errno(0x33)
        EBADRQC         = Errno(0x36)
        EBADSLT         = Errno(0x37)
        EBFONT          = Errno(0x39)
        EBUSY           = Errno(0x10)
        ECANCELED       = Errno(0x2f)
        ECHILD          = Errno(0xa)
        ECHRNG          = Errno(0x25)
        ECOMM           = Errno(0x46)
        ECONNABORTED    = Errno(0x82)
        ECONNREFUSED    = Errno(0x92)
        ECONNRESET      = Errno(0x83)
        EDEADLK         = Errno(0x2d)
        EDEADLOCK       = Errno(0x38)
        EDESTADDRREQ    = Errno(0x60)
        EDOM            = Errno(0x21)
        EDQUOT          = Errno(0x31)
        EEXIST          = Errno(0x11)
        EFAULT          = Errno(0xe)
        EFBIG           = Errno(0x1b)
        EHOSTDOWN       = Errno(0x93)
        EHOSTUNREACH    = Errno(0x94)
        EIDRM           = Errno(0x24)
        EILSEQ          = Errno(0x58)
        EINPROGRESS     = Errno(0x96)
        EINTR           = Errno(0x4)
        EINVAL          = Errno(0x16)
        EIO             = Errno(0x5)
        EISCONN         = Errno(0x85)
        EISDIR          = Errno(0x15)
        EL2HLT          = Errno(0x2c)
        EL2NSYNC        = Errno(0x26)
        EL3HLT          = Errno(0x27)
        EL3RST          = Errno(0x28)
        ELIBACC         = Errno(0x53)
        ELIBBAD         = Errno(0x54)
        ELIBEXEC        = Errno(0x57)
        ELIBMAX         = Errno(0x56)
        ELIBSCN         = Errno(0x55)
        ELNRNG          = Errno(0x29)
        ELOCKUNMAPPED   = Errno(0x48)
        ELOOP           = Errno(0x5a)
        EMFILE          = Errno(0x18)
        EMLINK          = Errno(0x1f)
        EMSGSIZE        = Errno(0x61)
        EMULTIHOP       = Errno(0x4a)
        ENAMETOOLONG    = Errno(0x4e)
        ENETDOWN        = Errno(0x7f)
        ENETRESET       = Errno(0x81)
        ENETUNREACH     = Errno(0x80)
        ENFILE          = Errno(0x17)
        ENOANO          = Errno(0x35)
        ENOBUFS         = Errno(0x84)
        ENOCSI          = Errno(0x2b)
        ENODATA         = Errno(0x3d)
        ENODEV          = Errno(0x13)
        ENOENT          = Errno(0x2)
        ENOEXEC         = Errno(0x8)
        ENOLCK          = Errno(0x2e)
        ENOLINK         = Errno(0x43)
        ENOMEM          = Errno(0xc)
        ENOMSG          = Errno(0x23)
        ENONET          = Errno(0x40)
        ENOPKG          = Errno(0x41)
        ENOPROTOOPT     = Errno(0x63)
        ENOSPC          = Errno(0x1c)
        ENOSR           = Errno(0x3f)
        ENOSTR          = Errno(0x3c)
        ENOSYS          = Errno(0x59)
        ENOTACTIVE      = Errno(0x49)
        ENOTBLK         = Errno(0xf)
        ENOTCONN        = Errno(0x86)
        ENOTDIR         = Errno(0x14)
        ENOTEMPTY       = Errno(0x5d)
        ENOTRECOVERABLE = Errno(0x3b)
        ENOTSOCK        = Errno(0x5f)
        ENOTSUP         = Errno(0x30)
        ENOTTY          = Errno(0x19)
        ENOTUNIQ        = Errno(0x50)
        ENXIO           = Errno(0x6)
        EOPNOTSUPP      = Errno(0x7a)
        EOVERFLOW       = Errno(0x4f)
        EOWNERDEAD      = Errno(0x3a)
        EPERM           = Errno(0x1)
        EPFNOSUPPORT    = Errno(0x7b)
        EPIPE           = Errno(0x20)
        EPROTO          = Errno(0x47)
        EPROTONOSUPPORT = Errno(0x78)
        EPROTOTYPE      = Errno(0x62)
        ERANGE          = Errno(0x22)
        EREMCHG         = Errno(0x52)
        EREMOTE         = Errno(0x42)
        ERESTART        = Errno(0x5b)
        EROFS           = Errno(0x1e)
        ESHUTDOWN       = Errno(0x8f)
        ESOCKTNOSUPPORT = Errno(0x79)
        ESPIPE          = Errno(0x1d)
        ESRCH           = Errno(0x3)
        ESRMNT          = Errno(0x45)
        ESTALE          = Errno(0x97)
        ESTRPIPE        = Errno(0x5c)
        ETIME           = Errno(0x3e)
        ETIMEDOUT       = Errno(0x91)
        ETOOMANYREFS    = Errno(0x90)
        ETXTBSY         = Errno(0x1a)
        EUNATCH         = Errno(0x2a)
        EUSERS          = Errno(0x5e)
        EWOULDBLOCK     = Errno(0xb)
        EXDEV           = Errno(0x12)
        EXFULL          = Errno(0x34)
)
```
Errors


```golang
const (
        SIGABRT    = Signal(0x6)
        SIGALRM    = Signal(0xe)
        SIGBUS     = Signal(0xa)
        SIGCANCEL  = Signal(0x24)
        SIGCHLD    = Signal(0x12)
        SIGCLD     = Signal(0x12)
        SIGCONT    = Signal(0x19)
        SIGEMT     = Signal(0x7)
        SIGFPE     = Signal(0x8)
        SIGFREEZE  = Signal(0x22)
        SIGHUP     = Signal(0x1)
        SIGILL     = Signal(0x4)
        SIGINT     = Signal(0x2)
        SIGIO      = Signal(0x16)
        SIGIOT     = Signal(0x6)
        SIGJVM1    = Signal(0x27)
        SIGJVM2    = Signal(0x28)
        SIGKILL    = Signal(0x9)
        SIGLOST    = Signal(0x25)
        SIGLWP     = Signal(0x21)
        SIGPIPE    = Signal(0xd)
        SIGPOLL    = Signal(0x16)
        SIGPROF    = Signal(0x1d)
        SIGPWR     = Signal(0x13)
        SIGQUIT    = Signal(0x3)
        SIGSEGV    = Signal(0xb)
        SIGSTOP    = Signal(0x17)
        SIGSYS     = Signal(0xc)
        SIGTERM    = Signal(0xf)
        SIGTHAW    = Signal(0x23)
        SIGTRAP    = Signal(0x5)
        SIGTSTP    = Signal(0x18)
        SIGTTIN    = Signal(0x1a)
        SIGTTOU    = Signal(0x1b)
        SIGURG     = Signal(0x15)
        SIGUSR1    = Signal(0x10)
        SIGUSR2    = Signal(0x11)
        SIGVTALRM  = Signal(0x1c)
        SIGWAITING = Signal(0x20)
        SIGWINCH   = Signal(0x14)
        SIGXCPU    = Signal(0x1e)
        SIGXFSZ    = Signal(0x1f)
        SIGXRES    = Signal(0x26)
)
```
Signals


```golang
const (
        SYS_READ                   = 0
        SYS_WRITE                  = 1
        SYS_OPEN                   = 2
        SYS_CLOSE                  = 3
        SYS_STAT                   = 4
        SYS_FSTAT                  = 5
        SYS_LSTAT                  = 6
        SYS_POLL                   = 7
        SYS_LSEEK                  = 8
        SYS_MMAP                   = 9
        SYS_MPROTECT               = 10
        SYS_MUNMAP                 = 11
        SYS_BRK                    = 12
        SYS_RT_SIGACTION           = 13
        SYS_RT_SIGPROCMASK         = 14
        SYS_RT_SIGRETURN           = 15
        SYS_IOCTL                  = 16
        SYS_PREAD64                = 17
        SYS_PWRITE64               = 18
        SYS_READV                  = 19
        SYS_WRITEV                 = 20
        SYS_ACCESS                 = 21
        SYS_PIPE                   = 22
        SYS_SELECT                 = 23
        SYS_SCHED_YIELD            = 24
        SYS_MREMAP                 = 25
        SYS_MSYNC                  = 26
        SYS_MINCORE                = 27
        SYS_MADVISE                = 28
        SYS_SHMGET                 = 29
        SYS_SHMAT                  = 30
        SYS_SHMCTL                 = 31
        SYS_DUP                    = 32
        SYS_DUP2                   = 33
        SYS_PAUSE                  = 34
        SYS_NANOSLEEP              = 35
        SYS_GETITIMER              = 36
        SYS_ALARM                  = 37
        SYS_SETITIMER              = 38
        SYS_GETPID                 = 39
        SYS_SENDFILE               = 40
        SYS_SOCKET                 = 41
        SYS_CONNECT                = 42
        SYS_ACCEPT                 = 43
        SYS_SENDTO                 = 44
        SYS_RECVFROM               = 45
        SYS_SENDMSG                = 46
        SYS_RECVMSG                = 47
        SYS_SHUTDOWN               = 48
        SYS_BIND                   = 49
        SYS_LISTEN                 = 50
        SYS_GETSOCKNAME            = 51
        SYS_GETPEERNAME            = 52
        SYS_SOCKETPAIR             = 53
        SYS_SETSOCKOPT             = 54
        SYS_GETSOCKOPT             = 55
        SYS_CLONE                  = 56
        SYS_FORK                   = 57
        SYS_VFORK                  = 58
        SYS_EXECVE                 = 59
        SYS_EXIT                   = 60
        SYS_WAIT4                  = 61
        SYS_KILL                   = 62
        SYS_UNAME                  = 63
        SYS_SEMGET                 = 64
        SYS_SEMOP                  = 65
        SYS_SEMCTL                 = 66
        SYS_SHMDT                  = 67
        SYS_MSGGET                 = 68
        SYS_MSGSND                 = 69
        SYS_MSGRCV                 = 70
        SYS_MSGCTL                 = 71
        SYS_FCNTL                  = 72
        SYS_FLOCK                  = 73
        SYS_FSYNC                  = 74
        SYS_FDATASYNC              = 75
        SYS_TRUNCATE               = 76
        SYS_FTRUNCATE              = 77
        SYS_GETDENTS               = 78
        SYS_GETCWD                 = 79
        SYS_CHDIR                  = 80
        SYS_FCHDIR                 = 81
        SYS_RENAME                 = 82
        SYS_MKDIR                  = 83
        SYS_RMDIR                  = 84
        SYS_CREAT                  = 85
        SYS_LINK                   = 86
        SYS_UNLINK                 = 87
        SYS_SYMLINK                = 88
        SYS_READLINK               = 89
        SYS_CHMOD                  = 90
        SYS_FCHMOD                 = 91
        SYS_CHOWN                  = 92
        SYS_FCHOWN                 = 93
        SYS_LCHOWN                 = 94
        SYS_UMASK                  = 95
        SYS_GETTIMEOFDAY           = 96
        SYS_GETRLIMIT              = 97
        SYS_GETRUSAGE              = 98
        SYS_SYSINFO                = 99
        SYS_TIMES                  = 100
        SYS_PTRACE                 = 101
        SYS_GETUID                 = 102
        SYS_SYSLOG                 = 103
        SYS_GETGID                 = 104
        SYS_SETUID                 = 105
        SYS_SETGID                 = 106
        SYS_GETEUID                = 107
        SYS_GETEGID                = 108
        SYS_SETPGID                = 109
        SYS_GETPPID                = 110
        SYS_GETPGRP                = 111
        SYS_SETSID                 = 112
        SYS_SETREUID               = 113
        SYS_SETREGID               = 114
        SYS_GETGROUPS              = 115
        SYS_SETGROUPS              = 116
        SYS_SETRESUID              = 117
        SYS_GETRESUID              = 118
        SYS_SETRESGID              = 119
        SYS_GETRESGID              = 120
        SYS_GETPGID                = 121
        SYS_SETFSUID               = 122
        SYS_SETFSGID               = 123
        SYS_GETSID                 = 124
        SYS_CAPGET                 = 125
        SYS_CAPSET                 = 126
        SYS_RT_SIGPENDING          = 127
        SYS_RT_SIGTIMEDWAIT        = 128
        SYS_RT_SIGQUEUEINFO        = 129
        SYS_RT_SIGSUSPEND          = 130
        SYS_SIGALTSTACK            = 131
        SYS_UTIME                  = 132
        SYS_MKNOD                  = 133
        SYS_USELIB                 = 134
        SYS_PERSONALITY            = 135
        SYS_USTAT                  = 136
        SYS_STATFS                 = 137
        SYS_FSTATFS                = 138
        SYS_SYSFS                  = 139
        SYS_GETPRIORITY            = 140
        SYS_SETPRIORITY            = 141
        SYS_SCHED_SETPARAM         = 142
        SYS_SCHED_GETPARAM         = 143
        SYS_SCHED_SETSCHEDULER     = 144
        SYS_SCHED_GETSCHEDULER     = 145
        SYS_SCHED_GET_PRIORITY_MAX = 146
        SYS_SCHED_GET_PRIORITY_MIN = 147
        SYS_SCHED_RR_GET_INTERVAL  = 148
        SYS_MLOCK                  = 149
        SYS_MUNLOCK                = 150
        SYS_MLOCKALL               = 151
        SYS_MUNLOCKALL             = 152
        SYS_VHANGUP                = 153
        SYS_MODIFY_LDT             = 154
        SYS_PIVOT_ROOT             = 155
        SYS__SYSCTL                = 156
        SYS_PRCTL                  = 157
        SYS_ARCH_PRCTL             = 158
        SYS_ADJTIMEX               = 159
        SYS_SETRLIMIT              = 160
        SYS_CHROOT                 = 161
        SYS_SYNC                   = 162
        SYS_ACCT                   = 163
        SYS_SETTIMEOFDAY           = 164
        SYS_MOUNT                  = 165
        SYS_UMOUNT2                = 166
        SYS_SWAPON                 = 167
        SYS_SWAPOFF                = 168
        SYS_REBOOT                 = 169
        SYS_SETHOSTNAME            = 170
        SYS_SETDOMAINNAME          = 171
        SYS_IOPL                   = 172
        SYS_IOPERM                 = 173
        SYS_CREATE_MODULE          = 174
        SYS_INIT_MODULE            = 175
        SYS_DELETE_MODULE          = 176
        SYS_GET_KERNEL_SYMS        = 177
        SYS_QUERY_MODULE           = 178
        SYS_QUOTACTL               = 179
        SYS_NFSSERVCTL             = 180
        SYS_GETPMSG                = 181
        SYS_PUTPMSG                = 182
        SYS_AFS_SYSCALL            = 183
        SYS_TUXCALL                = 184
        SYS_SECURITY               = 185
        SYS_GETTID                 = 186
        SYS_READAHEAD              = 187
        SYS_SETXATTR               = 188
        SYS_LSETXATTR              = 189
        SYS_FSETXATTR              = 190
        SYS_GETXATTR               = 191
        SYS_LGETXATTR              = 192
        SYS_FGETXATTR              = 193
        SYS_LISTXATTR              = 194
        SYS_LLISTXATTR             = 195
        SYS_FLISTXATTR             = 196
        SYS_REMOVEXATTR            = 197
        SYS_LREMOVEXATTR           = 198
        SYS_FREMOVEXATTR           = 199
        SYS_TKILL                  = 200
        SYS_TIME                   = 201
        SYS_FUTEX                  = 202
        SYS_SCHED_SETAFFINITY      = 203
        SYS_SCHED_GETAFFINITY      = 204
        SYS_SET_THREAD_AREA        = 205
        SYS_IO_SETUP               = 206
        SYS_IO_DESTROY             = 207
        SYS_IO_GETEVENTS           = 208
        SYS_IO_SUBMIT              = 209
        SYS_IO_CANCEL              = 210
        SYS_GET_THREAD_AREA        = 211
        SYS_LOOKUP_DCOOKIE         = 212
        SYS_EPOLL_CREATE           = 213
        SYS_EPOLL_CTL_OLD          = 214
        SYS_EPOLL_WAIT_OLD         = 215
        SYS_REMAP_FILE_PAGES       = 216
        SYS_GETDENTS64             = 217
        SYS_SET_TID_ADDRESS        = 218
        SYS_RESTART_SYSCALL        = 219
        SYS_SEMTIMEDOP             = 220
        SYS_FADVISE64              = 221
        SYS_TIMER_CREATE           = 222
        SYS_TIMER_SETTIME          = 223
        SYS_TIMER_GETTIME          = 224
        SYS_TIMER_GETOVERRUN       = 225
        SYS_TIMER_DELETE           = 226
        SYS_CLOCK_SETTIME          = 227
        SYS_CLOCK_GETTIME          = 228
        SYS_CLOCK_GETRES           = 229
        SYS_CLOCK_NANOSLEEP        = 230
        SYS_EXIT_GROUP             = 231
        SYS_EPOLL_WAIT             = 232
        SYS_EPOLL_CTL              = 233
        SYS_TGKILL                 = 234
        SYS_UTIMES                 = 235
        SYS_VSERVER                = 236
        SYS_MBIND                  = 237
        SYS_SET_MEMPOLICY          = 238
        SYS_GET_MEMPOLICY          = 239
        SYS_MQ_OPEN                = 240
        SYS_MQ_UNLINK              = 241
        SYS_MQ_TIMEDSEND           = 242
        SYS_MQ_TIMEDRECEIVE        = 243
        SYS_MQ_NOTIFY              = 244
        SYS_MQ_GETSETATTR          = 245
        SYS_KEXEC_LOAD             = 246
        SYS_WAITID                 = 247
        SYS_ADD_KEY                = 248
        SYS_REQUEST_KEY            = 249
        SYS_KEYCTL                 = 250
        SYS_IOPRIO_SET             = 251
        SYS_IOPRIO_GET             = 252
        SYS_INOTIFY_INIT           = 253
        SYS_INOTIFY_ADD_WATCH      = 254
        SYS_INOTIFY_RM_WATCH       = 255
        SYS_MIGRATE_PAGES          = 256
        SYS_OPENAT                 = 257
        SYS_MKDIRAT                = 258
        SYS_MKNODAT                = 259
        SYS_FCHOWNAT               = 260
        SYS_FUTIMESAT              = 261
        SYS_NEWFSTATAT             = 262
        SYS_UNLINKAT               = 263
        SYS_RENAMEAT               = 264
        SYS_LINKAT                 = 265
        SYS_SYMLINKAT              = 266
        SYS_READLINKAT             = 267
        SYS_FCHMODAT               = 268
        SYS_FACCESSAT              = 269
        SYS_PSELECT6               = 270
        SYS_PPOLL                  = 271
        SYS_UNSHARE                = 272
        SYS_SET_ROBUST_LIST        = 273
        SYS_GET_ROBUST_LIST        = 274
        SYS_SPLICE                 = 275
        SYS_TEE                    = 276
        SYS_SYNC_FILE_RANGE        = 277
        SYS_VMSPLICE               = 278
        SYS_MOVE_PAGES             = 279
        SYS_UTIMENSAT              = 280
        SYS_EPOLL_PWAIT            = 281
        SYS_SIGNALFD               = 282
        SYS_TIMERFD_CREATE         = 283
        SYS_EVENTFD                = 284
        SYS_FALLOCATE              = 285
        SYS_TIMERFD_SETTIME        = 286
        SYS_TIMERFD_GETTIME        = 287
        SYS_ACCEPT4                = 288
        SYS_SIGNALFD4              = 289
        SYS_EVENTFD2               = 290
        SYS_EPOLL_CREATE1          = 291
        SYS_DUP3                   = 292
        SYS_PIPE2                  = 293
        SYS_INOTIFY_INIT1          = 294
        SYS_PREADV                 = 295
        SYS_PWRITEV                = 296
        SYS_RT_TGSIGQUEUEINFO      = 297
        SYS_PERF_EVENT_OPEN        = 298
        SYS_RECVMMSG               = 299
        SYS_FANOTIFY_INIT          = 300
        SYS_FANOTIFY_MARK          = 301
        SYS_PRLIMIT64              = 302
)
```


```golang
const (
        SYS_EXECVE = 59
        SYS_FCNTL  = 62
)
```


TODO(aram): remove these before Go 1.3.
TODO(aram): GO1.3之前删除它们。


```golang
const (
        SizeofSockaddrInet4     = 0x10
        SizeofSockaddrInet6     = 0x1c
        SizeofSockaddrAny       = 0x70
        SizeofSockaddrUnix      = 0x6e
        SizeofSockaddrLinklayer = 0x14
        SizeofSockaddrNetlink   = 0xc
        SizeofLinger            = 0x8
        SizeofIPMreq            = 0x8
        SizeofIPMreqn           = 0xc
        SizeofIPv6Mreq          = 0x14
        SizeofMsghdr            = 0x38
        SizeofCmsghdr           = 0x10
        SizeofInet4Pktinfo      = 0xc
        SizeofInet6Pktinfo      = 0x14
        SizeofIPv6MTUInfo       = 0x20
        SizeofICMPv6Filter      = 0x20
        SizeofUcred             = 0xc
        SizeofTCPInfo           = 0x68
)
```


```golang
const (
        IFA_UNSPEC          = 0x0
        IFA_ADDRESS         = 0x1
        IFA_LOCAL           = 0x2
        IFA_LABEL           = 0x3
        IFA_BROADCAST       = 0x4
        IFA_ANYCAST         = 0x5
        IFA_CACHEINFO       = 0x6
        IFA_MULTICAST       = 0x7
        IFLA_UNSPEC         = 0x0
        IFLA_ADDRESS        = 0x1
        IFLA_BROADCAST      = 0x2
        IFLA_IFNAME         = 0x3
        IFLA_MTU            = 0x4
        IFLA_LINK           = 0x5
        IFLA_QDISC          = 0x6
        IFLA_STATS          = 0x7
        IFLA_COST           = 0x8
        IFLA_PRIORITY       = 0x9
        IFLA_MASTER         = 0xa
        IFLA_WIRELESS       = 0xb
        IFLA_PROTINFO       = 0xc
        IFLA_TXQLEN         = 0xd
        IFLA_MAP            = 0xe
        IFLA_WEIGHT         = 0xf
        IFLA_OPERSTATE      = 0x10
        IFLA_LINKMODE       = 0x11
        IFLA_LINKINFO       = 0x12
        IFLA_NET_NS_PID     = 0x13
        IFLA_IFALIAS        = 0x14
        IFLA_MAX            = 0x1d
        RT_SCOPE_UNIVERSE   = 0x0
        RT_SCOPE_SITE       = 0xc8
        RT_SCOPE_LINK       = 0xfd
        RT_SCOPE_HOST       = 0xfe
        RT_SCOPE_NOWHERE    = 0xff
        RT_TABLE_UNSPEC     = 0x0
        RT_TABLE_COMPAT     = 0xfc
        RT_TABLE_DEFAULT    = 0xfd
        RT_TABLE_MAIN       = 0xfe
        RT_TABLE_LOCAL      = 0xff
        RT_TABLE_MAX        = 0xffffffff
        RTA_UNSPEC          = 0x0
        RTA_DST             = 0x1
        RTA_SRC             = 0x2
        RTA_IIF             = 0x3
        RTA_OIF             = 0x4
        RTA_GATEWAY         = 0x5
        RTA_PRIORITY        = 0x6
        RTA_PREFSRC         = 0x7
        RTA_METRICS         = 0x8
        RTA_MULTIPATH       = 0x9
        RTA_FLOW            = 0xb
        RTA_CACHEINFO       = 0xc
        RTA_TABLE           = 0xf
        RTN_UNSPEC          = 0x0
        RTN_UNICAST         = 0x1
        RTN_LOCAL           = 0x2
        RTN_BROADCAST       = 0x3
        RTN_ANYCAST         = 0x4
        RTN_MULTICAST       = 0x5
        RTN_BLACKHOLE       = 0x6
        RTN_UNREACHABLE     = 0x7
        RTN_PROHIBIT        = 0x8
        RTN_THROW           = 0x9
        RTN_NAT             = 0xa
        RTN_XRESOLVE        = 0xb
        RTNLGRP_NONE        = 0x0
        RTNLGRP_LINK        = 0x1
        RTNLGRP_NOTIFY      = 0x2
        RTNLGRP_NEIGH       = 0x3
        RTNLGRP_TC          = 0x4
        RTNLGRP_IPV4_IFADDR = 0x5
        RTNLGRP_IPV4_MROUTE = 0x6
        RTNLGRP_IPV4_ROUTE  = 0x7
        RTNLGRP_IPV4_RULE   = 0x8
        RTNLGRP_IPV6_IFADDR = 0x9
        RTNLGRP_IPV6_MROUTE = 0xa
        RTNLGRP_IPV6_ROUTE  = 0xb
        RTNLGRP_IPV6_IFINFO = 0xc
        RTNLGRP_IPV6_PREFIX = 0x12
        RTNLGRP_IPV6_RULE   = 0x13
        RTNLGRP_ND_USEROPT  = 0x14
        SizeofNlMsghdr      = 0x10
        SizeofNlMsgerr      = 0x14
        SizeofRtGenmsg      = 0x1
        SizeofNlAttr        = 0x4
        SizeofRtAttr        = 0x4
        SizeofIfInfomsg     = 0x10
        SizeofIfAddrmsg     = 0x8
        SizeofRtMsg         = 0xc
        SizeofRtNexthop     = 0x8
)
```


```golang
const (
        SizeofSockFilter = 0x8
        SizeofSockFprog  = 0x10
)
```


```golang
const (
        VINTR    = 0x0
        VQUIT    = 0x1
        VERASE   = 0x2
        VKILL    = 0x3
        VEOF     = 0x4
        VTIME    = 0x5
        VMIN     = 0x6
        VSWTC    = 0x7
        VSTART   = 0x8
        VSTOP    = 0x9
        VSUSP    = 0xa
        VEOL     = 0xb
        VREPRINT = 0xc
        VDISCARD = 0xd
        VWERASE  = 0xe
        VLNEXT   = 0xf
        VEOL2    = 0x10
        IGNBRK   = 0x1
        BRKINT   = 0x2
        IGNPAR   = 0x4
        PARMRK   = 0x8
        INPCK    = 0x10
        ISTRIP   = 0x20
        INLCR    = 0x40
        IGNCR    = 0x80
        ICRNL    = 0x100
        IUCLC    = 0x200
        IXON     = 0x400
        IXANY    = 0x800
        IXOFF    = 0x1000
        IMAXBEL  = 0x2000
        IUTF8    = 0x4000
        OPOST    = 0x1
        OLCUC    = 0x2
        ONLCR    = 0x4
        OCRNL    = 0x8
        ONOCR    = 0x10
        ONLRET   = 0x20
        OFILL    = 0x40
        OFDEL    = 0x80
        B0       = 0x0
        B50      = 0x1
        B75      = 0x2
        B110     = 0x3
        B134     = 0x4
        B150     = 0x5
        B200     = 0x6
        B300     = 0x7
        B600     = 0x8
        B1200    = 0x9
        B1800    = 0xa
        B2400    = 0xb
        B4800    = 0xc
        B9600    = 0xd
        B19200   = 0xe
        B38400   = 0xf
        CSIZE    = 0x30
        CS5      = 0x0
        CS6      = 0x10
        CS7      = 0x20
        CS8      = 0x30
        CSTOPB   = 0x40
        CREAD    = 0x80
        PARENB   = 0x100
        PARODD   = 0x200
        HUPCL    = 0x400
        CLOCAL   = 0x800
        B57600   = 0x1001
        B115200  = 0x1002
        B230400  = 0x1003
        B460800  = 0x1004
        B500000  = 0x1005
        B576000  = 0x1006
        B921600  = 0x1007
        B1000000 = 0x1008
        B1152000 = 0x1009
        B1500000 = 0x100a
        B2000000 = 0x100b
        B2500000 = 0x100c
        B3000000 = 0x100d
        B3500000 = 0x100e
        B4000000 = 0x100f
        ISIG     = 0x1
        ICANON   = 0x2
        XCASE    = 0x4
        ECHO     = 0x8
        ECHOE    = 0x10
        ECHOK    = 0x20
        ECHONL   = 0x40
        NOFLSH   = 0x80
        TOSTOP   = 0x100
        ECHOCTL  = 0x200
        ECHOPRT  = 0x400
        ECHOKE   = 0x800
        FLUSHO   = 0x1000
        PENDIN   = 0x4000
        IEXTEN   = 0x8000
        TCGETS   = 0x5401
        TCSETS   = 0x5402
)
```


```golang
const (
        S_IFMT   = 0xf000
        S_IFIFO  = 0x1000
        S_IFCHR  = 0x2000
        S_IFDIR  = 0x4000
        S_IFBLK  = 0x6000
        S_IFREG  = 0x8000
        S_IFLNK  = 0xa000
        S_IFSOCK = 0xc000
        S_ISUID  = 0x800
        S_ISGID  = 0x400
        S_ISVTX  = 0x200
        S_IRUSR  = 0x100
        S_IWUSR  = 0x80
        S_IXUSR  = 0x40
)
```

```golang
const (
        SizeofSockaddrInet4    = 0x10
        SizeofSockaddrInet6    = 0x20
        SizeofSockaddrAny      = 0xfc
        SizeofSockaddrUnix     = 0x6e
        SizeofSockaddrDatalink = 0xfc
        SizeofLinger           = 0x8
        SizeofIPMreq           = 0x8
        SizeofIPv6Mreq         = 0x14
        SizeofMsghdr           = 0x30
        SizeofCmsghdr          = 0xc
        SizeofInet6Pktinfo     = 0x14
        SizeofIPv6MTUInfo      = 0x24
        SizeofICMPv6Filter     = 0x20
)
```


```golang
const (
        SizeofIfMsghdr  = 0x54
        SizeofIfData    = 0x44
        SizeofIfaMsghdr = 0x14
        SizeofRtMsghdr  = 0x4c
        SizeofRtMetrics = 0x28
)
```


```golang
const (
        SizeofBpfVersion = 0x4
        SizeofBpfStat    = 0x80
        SizeofBpfProgram = 0x10
        SizeofBpfInsn    = 0x8
        SizeofBpfHdr     = 0x14
)
```

```golang
const ImplementsGetwd = true
```

```golang
const ImplementsGetwd = false
```

```golang
const ImplementsGetwd = false
```


The const provides a compile-time constant so clients can adjust to whether there is a working Getwd and avoid even linking this function into the binary. See ../os/getwd.go.
该常量提供

```golang
const PathMax = 256
```

```golang
const (
        PathMax = 0x1000
)
```

```golang
const SizeofInotifyEvent = 0x10
```


Variables
```golang
var (
        Stdin  = 0
        Stdout = 1
        Stderr = 2
)
```

```golang
var ForkLock sync.RWMutex
```

```golang
var ForkLock sync.RWMutex
```

```golang
var SocketDisableIPv6 bool
```

```golang
var SocketDisableIPv6 bool
```
For testing: clients can set this flag to force creation of IPv6 sockets to return EAFNOSUPPORT.
测试: 客户端可以设置这个标示 强制 建立 IPv6 sockets 来 返回 EAFNOSUPPORT



func Accept
```golang
func Accept(fd int) (nfd int, sa Sockaddr, err error)
```


func Accept4
```golang
func Accept4(fd int, flags int) (nfd int, sa Sockaddr, err error)
```


func Access
```golang
func Access(path string, mode uint32) (err error)
```



func Acct
```golang
func Acct(path string) (err error)
```


func Adjtime
```golang
func Adjtime(delta *Timeval, olddelta *Timeval) (err error)
```


func Adjtimex
```golang
func Adjtimex(buf *Timex) (state int, err error)
```


func AttachLsf
```golang
func AttachLsf(fd int, i []SockFilter) error
```


func Bind
```golang
func Bind(fd int, sa Sockaddr) (err error)
```


func BindToDevice
```golang
func BindToDevice(fd int, device string) (err error)
```
BindToDevice binds the socket associated with fd to device.
BindToDevice 绑定 socket 对应的fd 到设备



func BytePtrFromString
```golang
func BytePtrFromString(s string) (*byte, error)
```
BytePtrFromString returns a pointer to a NUL-terminated array of bytes containing the text of s. If s contains a NUL byte at any location, it returns (nil, EINVAL).
BytePtrFromString 返回  包含文本s 以NULL结尾的字节数组 指针. 如果s 任何位置包含一个 NUL, 它返回(nil, EINVAL).



func ByteSliceFromString
```golang
func ByteSliceFromString(s string) ([]byte, error)
```
ByteSliceFromString returns a NUL-terminated slice of bytes containing the text of s. If s contains a NUL byte at any location, it returns (nil, EINVAL).
ByteSliceFromString 返回  包含文本s 以NULL结尾的字节slice 指针. 如果s 任何位置包含一个 NUL, 它返回(nil, EINVAL).



func Chdir
```golang
func Chdir(path string) (err error)
```


func Chmod
```golang
func Chmod(path string, mode uint32) (err error)
```


func Chown
```golang
func Chown(path string, uid int, gid int) (err error)
```


func Chroot
```golang
func Chroot(path string) (err error)
```


func Clearenv
```golang
func Clearenv()
```


func Close
```golang
func Close(fd int) (err error)
```


func CloseOnExec
```golang
func CloseOnExec(fd int)
```


func CmsgLen
```golang
func CmsgLen(datalen int) int
```
CmsgLen returns the value to store in the Len field of the Cmsghdr structure, taking into account any necessary alignment.
CmsgLen 返回 存储 在 Cmsghdr结构体 Len字段 的值, 考虑到任何必要的调整。



func CmsgSpace
```golang
func CmsgSpace(datalen int) int
```
CmsgSpace returns the number of bytes an ancillary element with payload of the passed data length occupies.
CmsgSpace 返回的字节的传送数据长度的有效负载的辅助元件占用的数量。



func Connect
```golang
func Connect(fd int, sa Sockaddr) (err error)
```


func Creat
```golang
func Creat(path string, mode uint32) (fd int, err error)
```


func DetachLsf
```golang
func DetachLsf(fd int) error
```


func Dup
```golang
func Dup(fd int) (nfd int, err error)
```


func Dup2
```golang
func Dup2(oldfd int, newfd int) (err error)
```


func Dup3
```golang
func Dup3(oldfd int, newfd int, flags int) (err error)
```


func Environ
```golang
func Environ() []string
```


func EpollCreate
```golang
func EpollCreate(size int) (fd int, err error)
```


func EpollCreate1
```golang
func EpollCreate1(flag int) (fd int, err error)
```


func EpollCtl
```golang
func EpollCtl(epfd int, op int, fd int, event *EpollEvent) (err error)
```


func EpollWait
```golang
func EpollWait(epfd int, events []EpollEvent, msec int) (n int, err error)
```


func Exec
```golang
func Exec(argv0 string, argv []string, envv []string) (err error)
```
Ordinary exec.


func Exit
```golang
func Exit(code int)
```


func Faccessat
```golang
func Faccessat(dirfd int, path string, mode uint32, flags int) (err error)
```


func Fallocate
```golang
func Fallocate(fd int, mode uint32, off int64, len int64) (err error)
```


func Fchdir
```golang
func Fchdir(fd int) (err error)
```


func Fchmod
```golang
func Fchmod(fd int, mode uint32) (err error)
``


func Fchmodat
```golang
func Fchmodat(dirfd int, path string, mode uint32, flags int) (err error)
```


func Fchown
```golang
func Fchown(fd int, uid int, gid int) (err error)
```



func Fchownat
```golang
func Fchownat(dirfd int, path string, uid int, gid int, flags int) (err error)
```


func FcntlFlock
```golang
func FcntlFlock(fd uintptr, cmd int, lk *Flock_t) error
```
FcntlFlock performs a fcntl syscall for the F_GETLK, F_SETLK or F_SETLKW command.
FcntlFlock 为 F_GETLK, F_SETLK 或  F_SETLKW 命令  执行一个 fcntl 系统调用 .



func Fdatasync
```golang
func Fdatasync(fd int) (err error)
```


func Flock
```golang
func Flock(fd int, how int) (err error)
```


func ForkExec
```golang
func ForkExec(argv0 string, argv []string, attr *ProcAttr) (pid int, err error)
```
Combination of fork and exec, careful to be thread safe.
Combination fork 和执行 , 小心线程安全.


func Fpathconf
```golang
func Fpathconf(fd int, name int) (val int, err error)
```


func Fstat
```golang
func Fstat(fd int, stat *Stat_t) (err error)
```


func Fstatfs
```golang
func Fstatfs(fd int, buf *Statfs_t) (err error)
```


func Fsync
```golang
func Fsync(fd int) (err error)
```


func Ftruncate
```golang
func Ftruncate(fd int, length int64) (err error)
```


func Futimes
```golang
func Futimes(fd int, tv []Timeval) (err error)
```



func Futimesat
```golang
func Futimesat(dirfd int, path string, tv []Timeval) (err error)
```


func Getcwd
```golang
func Getcwd(buf []byte) (n int, err error)
```



func Getdents
```golang
func Getdents(fd int, buf []byte, basep *uintptr) (n int, err error)
```



func Getegid
```golang
func Getegid() (egid int)
```


func Getenv
```golang
func Getenv(key string) (value string, found bool)
```


func Geteuid
```golang
func Geteuid() (euid int)
```



func Getgid
```golang
func Getgid() (gid int)
```


func Getgroups
```golang
func Getgroups() (gids []int, err error)
```


func Gethostname
```golang
func Gethostname() (name string, err error)
```


func Getpagesize
```golang
func Getpagesize() int
```


func Getpgid
```golang
func Getpgid(pid int) (pgid int, err error)
```


func Getpgrp
```golang
func Getpgrp() (pid int)
```


func Getpid
```golang
func Getpid() (pid int)
```


func Getppid
```golang
func Getppid() (ppid int)
```


func Getpriority
```golang
func Getpriority(which int, who int) (n int, err error)
```


func Getrlimit
```golang
func Getrlimit(which int, lim *Rlimit) (err error)
```


func Getrusage
```golang
func Getrusage(who int, rusage *Rusage) (err error)
```


func GetsockoptInet4Addr
```golang
func GetsockoptInet4Addr(fd, level, opt int) (value [4]byte, err error)
```


func GetsockoptInt
```golang
func GetsockoptInt(fd, level, opt int) (value int, err error)
```


func Gettid
```golang
func Gettid() (tid int)
```


func Gettimeofday
```golang
func Gettimeofday(tv *Timeval) (err error)
```


func Getuid
```golang
func Getuid() (uid int)
```


func Getwd
```golang
func Getwd() (string, error)
```


func Getxattr
```golang
func Getxattr(path string, attr string, dest []byte) (sz int, err error)
```


func InotifyAddWatch
```golang
func InotifyAddWatch(fd int, pathname string, mask uint32) (watchdesc int, err error)
```


func InotifyInit
```golang
func InotifyInit() (fd int, err error)
``


func InotifyInit1
```golang
func InotifyInit1(flags int) (fd int, err error)
```


func InotifyRmWatch
```golang
func InotifyRmWatch(fd int, watchdesc uint32) (success int, err error)
```



func Ioperm
```golang
func Ioperm(from int, num int, on int) (err error)
```


func Iopl
```golang
func Iopl(level int) (err error)
```


func Kill
```golang
func Kill(pid int, signum Signal) (err error)
```


func Klogctl
```golang
func Klogctl(typ int, buf []byte) (n int, err error)
```


func Lchown
```golang
func Lchown(path string, uid int, gid int) (err error)
```


func Link
```golang
func Link(path string, link string) (err error)
```


func Listen
```golang
func Listen(s int, backlog int) (err error)
```



func Listxattr
```golang
func Listxattr(path string, dest []byte) (sz int, err error)
```


func LsfSocket
```golang
func LsfSocket(ifindex, proto int) (int, error)
```


func Lstat
```golang
func Lstat(path string, stat *Stat_t) (err error)
```


func Madvise
```golang
func Madvise(b []byte, advice int) (err error)
```


func Mkdir
```golang
func Mkdir(path string, mode uint32) (err error)
```


func Mkdirat
```golang
func Mkdirat(dirfd int, path string, mode uint32) (err error)
```


func Mkfifo
```golang
func Mkfifo(path string, mode uint32) (err error)
```



func Mknod
```golang
func Mknod(path string, mode uint32, dev int) (err error)
```


func Mknodat
```golang
func Mknodat(dirfd int, path string, mode uint32, dev int) (err error)
```



func Mlock
```golang
func Mlock(b []byte) (err error)
```



func Mlockall
```golang
func Mlockall(flags int) (err error)
```



func Mmap
```golang
func Mmap(fd int, offset int64, length int, prot int, flags int) (data []byte, err error)
```


func Mount
```golang
func Mount(source string, target string, fstype string, flags uintptr, data string) (err error)
```


func Mprotect
```golang
func Mprotect(b []byte, prot int) (err error)
```


func Munlock
```golang
func Munlock(b []byte) (err error)
```


func Munlockall
```golang
func Munlockall() (err error)
```


func Munmap
```golang
func Munmap(b []byte) (err error)
```


func Nanosleep
```golang
func Nanosleep(time *Timespec, leftover *Timespec) (err error)
```



func NetlinkRIB
```golang
func NetlinkRIB(proto, family int) ([]byte, error)
```
NetlinkRIB returns routing information base, as known as RIB, which consists of network facility information, states and parameters.
NetlinkRIB 返回 路由信息库, 被称为 RIB,它包括网络设施信息的 状态和参数。



func Open
```golang
func Open(path string, mode int, perm uint32) (fd int, err error)
```


func Openat
```golang
func Openat(dirfd int, path string, flags int, mode uint32) (fd int, err error)
```


func ParseDirent
```golang
func ParseDirent(buf []byte, max int, names []string) (consumed int, count int, newnames []string)
```
ParseDirent parses up to max directory entries in buf, appending the names to names. 
It returns the number bytes consumed from buf, the number of entries added to names, and the new names slice.
ParseDirent 解析 buf里最长的条目,追加 names 到 names.
它 返回 从buf 消耗的字节数,  添加到 names 的项的 数量, 和新names  slice



func ParseNetlinkMessage
```golang
func ParseNetlinkMessage(b []byte) ([]NetlinkMessage, error)
```
ParseNetlinkMessage parses b as an array of netlink messages and returns the slice containing the NetlinkMessage structures.
ParseNetlinkMessage 解析b 成 网络链路信息 数组  并 返回 包含NetlinkMessage 结构体的 slice.



func ParseNetlinkRouteAttr
```golang
func ParseNetlinkRouteAttr(m *NetlinkMessage) ([]NetlinkRouteAttr, error)
```
ParseNetlinkRouteAttr parses m's payload as an array of netlink route attributes and returns the slice containing the NetlinkRouteAttr structures.
ParseNetlinkRouteAttr 解析m的 有效载荷 成网络链路的路由属性 数组,  并返回 包含 NetlinkRouteAttr 结构体的slice



func ParseRoutingMessage
```golang
func ParseRoutingMessage(b []byte) ([]RoutingMessage, error)
```


func ParseRoutingSockaddr
```golang
func ParseRoutingSockaddr(msg RoutingMessage) ([]Sockaddr, error)
```



func ParseSocketControlMessage
```golang
func ParseSocketControlMessage(b []byte) ([]SocketControlMessage, error)
```
ParseSocketControlMessage parses b as an array of socket control messages.
ParseSocketControlMessage 解析b 成 socket 控制信息 的数组.


func ParseUnixRights
```golang
func ParseUnixRights(m *SocketControlMessage) ([]int, error)
```
ParseUnixRights decodes a socket control message that contains an integer array of open file descriptors from another process.
ParseUnixRights 解码 socket控制消息 包含 从其他进程 打开的文件描述符 整型数组



func Pathconf
```golang
func Pathconf(path string, name int) (val int, err error)
```



func Pause
```golang
func Pause() (err error)
```



func Pipe
```golang
func Pipe(p []int) (err error)
```



func Pipe2
```golang
func Pipe2(p []int, flags int) (err error)
```



func PivotRoot
```golang
func PivotRoot(newroot string, putold string) (err error)
```



func Pread
```golang
func Pread(fd int, p []byte, offset int64) (n int, err error)
```



func PtraceAttach
```golang
func PtraceAttach(pid int) (err error)
```


func PtraceCont
```golang
func PtraceCont(pid int, signal int) (err error)
```



func PtraceDetach
```golang
func PtraceDetach(pid int) (err error)
```


func PtraceGetEventMsg
```golang
func PtraceGetEventMsg(pid int) (msg uint, err error)
```



func PtraceGetRegs
```golang
func PtraceGetRegs(pid int, regsout *PtraceRegs) (err error)
```


func PtracePeekData
```golang
func PtracePeekData(pid int, addr uintptr, out []byte) (count int, err error)
```


func PtracePeekText
```golang
func PtracePeekText(pid int, addr uintptr, out []byte) (count int, err error)
```


func PtracePokeData
```golang
func PtracePokeData(pid int, addr uintptr, data []byte) (count int, err error)
```


func PtracePokeText
```golang
func PtracePokeText(pid int, addr uintptr, data []byte) (count int, err error)
```


func PtraceSetOptions
```golang
func PtraceSetOptions(pid int, options int) (err error)
```


func PtraceSetRegs
```golang
func PtraceSetRegs(pid int, regs *PtraceRegs) (err error)
```


func PtraceSingleStep
```golang
func PtraceSingleStep(pid int) (err error)
```


func PtraceSyscall
```golang
func PtraceSyscall(pid int, signal int) (err error)
```


func Pwrite
```golang
func Pwrite(fd int, p []byte, offset int64) (n int, err error)
```


func RawSyscall
```golang
func RawSyscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)
```


func RawSyscall6
```golang
func RawSyscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)
```


func Read
```golang
func Read(fd int, p []byte) (n int, err error)
```


func ReadDirent
```golang
func ReadDirent(fd int, buf []byte) (n int, err error)
```


func Readlink
```golang
func Readlink(path string, buf []byte) (n int, err error)
```


func Reboot
```golang
func Reboot(cmd int) (err error)
```


func Recvfrom
```golang
func Recvfrom(fd int, p []byte, flags int) (n int, from Sockaddr, err error)
```


func Recvmsg
```golang
func Recvmsg(fd int, p, oob []byte, flags int) (n, oobn int, recvflags int, from Sockaddr, err error)
```


func Removexattr
```golang
func Removexattr(path string, attr string) (err error)
```


func Rename
```golang
func Rename(from string, to string) (err error)
```


func Renameat
```golang
func Renameat(olddirfd int, oldpath string, newdirfd int, newpath string) (err error)
```


func Rmdir
```golang
func Rmdir(path string) (err error)
```


func RouteRIB
```golang
func RouteRIB(facility, param int) ([]byte, error)
```


func Seek
```golang
func Seek(fd int, offset int64, whence int) (newoffset int64, err error)
```


func Select
```golang
func Select(nfd int, r *FdSet, w *FdSet, e *FdSet, timeout *Timeval) (n int, err error)
```


func Sendfile
```golang
func Sendfile(outfd int, infd int, offset *int64, count int) (written int, err error)
```


func Sendmsg
```golang
func Sendmsg(fd int, p, oob []byte, to Sockaddr, flags int) (err error)
```


func SendmsgN
```golang
func SendmsgN(fd int, p, oob []byte, to Sockaddr, flags int) (n int, err error)
```


func Sendto
```golang
func Sendto(fd int, p []byte, flags int, to Sockaddr) (err error)
```


func SetLsfPromisc
```golang
func SetLsfPromisc(name string, m bool) error
```


func SetNonblock
```golang
func SetNonblock(fd int, nonblocking bool) error
```


func SetReadDeadline
```golang
func SetReadDeadline(fd int, t int64) error
```


func SetWriteDeadline
```golang
func SetWriteDeadline(fd int, t int64) error
```


func Setdomainname
```golang
func Setdomainname(p []byte) (err error)
```


func Setegid
```golang
func Setegid(egid int) (err error)
```


func Setenv
```golang
func Setenv(key, value string) error
```


func Seteuid
```golang
func Seteuid(euid int) (err error)
```


func Setfsgid
```golang
func Setfsgid(gid int) (err error)
```


func Setfsuid
```golang
func Setfsuid(uid int) (err error)
```


func Setgid
```golang
func Setgid(gid int) (err error)
```


func Setgroups
```golang
func Setgroups(gids []int) (err error)
```


func Sethostname
```golang
func Sethostname(p []byte) (err error)
```


func Setpgid
```golang
func Setpgid(pid int, pgid int) (err error)
```


func Setpriority
```golang
func Setpriority(which int, who int, prio int) (err error)
```


func Setregid
```golang
func Setregid(rgid int, egid int) (err error)
```


func Setresgid
```golang
func Setresgid(rgid int, egid int, sgid int) (err error)
```


func Setresuid
```golang
func Setresuid(ruid int, euid int, suid int) (err error)
```


func Setreuid
```golang
func Setreuid(ruid int, euid int) (err error)
```


func Setrlimit
```golang
func Setrlimit(which int, lim *Rlimit) (err error)
```


func Setsid
```golang
func Setsid() (pid int, err error)
```


func SetsockoptByte
```golang
func SetsockoptByte(fd, level, opt int, value byte) (err error)
```


func SetsockoptICMPv6Filter
```golang
func SetsockoptICMPv6Filter(fd, level, opt int, filter *ICMPv6Filter) error
```


func SetsockoptIPMreq
```golang
func SetsockoptIPMreq(fd, level, opt int, mreq *IPMreq) (err error)
```



func SetsockoptIPMreqn
```golang
func SetsockoptIPMreqn(fd, level, opt int, mreq *IPMreqn) (err error)
```


func SetsockoptIPv6Mreq
```golang
func SetsockoptIPv6Mreq(fd, level, opt int, mreq *IPv6Mreq) (err error)
```



func SetsockoptInet4Addr
```golang
func SetsockoptInet4Addr(fd, level, opt int, value [4]byte) (err error)
```



func SetsockoptInt
```golang
func SetsockoptInt(fd, level, opt int, value int) (err error)
```


func SetsockoptLinger
```golang
func SetsockoptLinger(fd, level, opt int, l *Linger) (err error)
```


func SetsockoptString
```golang
func SetsockoptString(fd, level, opt int, s string) (err error)
```



func SetsockoptTimeval
```golang
func SetsockoptTimeval(fd, level, opt int, tv *Timeval) (err error)
```



func Settimeofday
```golang
func Settimeofday(tv *Timeval) (err error)
```


func Setuid
```golang
func Setuid(uid int) (err error)
```


func Setxattr
```golang
func Setxattr(path string, attr string, data []byte, flags int) (err error)
```



func Shutdown
```golang
func Shutdown(s int, how int) (err error)
```


func SlicePtrFromStrings
```golang
func SlicePtrFromStrings(ss []string) ([]*byte, error)
```
SlicePtrFromStrings converts a slice of strings to a slice of pointers to NUL-terminated byte slices. If any string contains a NUL byte, it returns (nil, EINVAL).
SlicePtrFromStrings 转换 string slice 成   以NUL结尾的字节slice 的指针 slice .如果 任何字符串 包含一个 nul 字节,它返回 (nil, EINVAL).



func Socket
```golang
func Socket(domain, typ, proto int) (fd int, err error)
```


func Socketpair
```golang
func Socketpair(domain, typ, proto int) (fd [2]int, err error)
```


func Splice
```golang
func Splice(rfd int, roff *int64, wfd int, woff *int64, len int, flags int) (n int64, err error)
```


func StartProcess
```golang
func StartProcess(argv0 string, argv []string, attr *ProcAttr) (pid int, handle uintptr, err error)
```
StartProcess wraps ForkExec for package os.
StartProcess 为os包 封装ForkExec



func Stat
```golang
func Stat(path string, stat *Stat_t) (err error)
```


func Statfs
```golang
func Statfs(path string, buf *Statfs_t) (err error)
```



func StopIO
```golang
func StopIO(fd int) error
```


func StringBytePtr
```golang
func StringBytePtr(s string) *byte
```
StringBytePtr is deprecated. Use BytePtrFromString instead. If s contains a NUL byte this function panics instead of returning an error.
StringBytePtr 是不推荐的 .使用 BytePtrFromString 代替. 如果s包含一个 NUL 字节  这个函数用panic  代替返回错误.



func StringByteSlice
```golang
func StringByteSlice(s string) []byte.
```
StringByteSlice is deprecated. Use ByteSliceFromString instead. If s contains a NUL byte this function panics instead of returning an error.
StringByteSlice是不推荐的. 使用 ByteSliceFromString 代替.如果s包含一个 NUL 字节  这个函数用panic  代替返回错误.



func StringSlicePtr
```golang
func StringSlicePtr(ss []string) []*byte
```
StringSlicePtr is deprecated. Use SlicePtrFromStrings instead. If any string contains a NUL byte this function panics instead of returning an error.
StringSlicePtr是不推荐的. 使用 SlicePtrFromStrings 代替.如果s包含一个 NUL 字节  这个函数用panic  代替返回错误.



func Symlink
```golang
func Symlink(path string, link string) (err error)
```



func Sync
```golang
func Sync() (err error)
```


func SyncFileRange
```golang
func SyncFileRange(fd int, off int64, n int64, flags int) (err error)
```


func Syscall
```golang
func Syscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)
```


func Syscall6
```golang
func Syscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)
```


func Sysctl
```golang
func Sysctl(key string) (string, error)
```


func SysctlUint32
```golang
func SysctlUint32(name string) (value uint32, err error)
```


func Sysinfo
```golang
func Sysinfo(info *Sysinfo_t) (err error)
```


func Tee
```golang
func Tee(rfd int, wfd int, len int, flags int) (n int64, err error)
```


func Tgkill
```golang
func Tgkill(tgid int, tid int, sig Signal) (err error)
```


func Times
```golang
func Times(tms *Tms) (ticks uintptr, err error)
```


func TimespecToNsec
```golang
func TimespecToNsec(ts Timespec) int64
```


func TimevalToNsec
```golang
func TimevalToNsec(tv Timeval) int64
```


func Truncate
```golang
func Truncate(path string, length int64) (err error)
```


func Umask
```golang
func Umask(newmask int) (oldmask int)
```


func Uname
```golang
func Uname(buf *Utsname) (err error)
```


func UnixCredentials
```golang
func UnixCredentials(ucred *Ucred) []byte
```
UnixCredentials encodes credentials into a socket control message for sending to another process. This can be used for authentication.
UnixCredentials 编码 证书到 socket 控制信息,  发送给其他进程. 这可以用来验证.



func UnixRights
```golang
func UnixRights(fds ...int) []byte
```
UnixRights encodes a set of open file descriptors into a socket control message for sending to another process.
UnixRights 编码一个 打开文件描述符 集 到 socket控制信息 来发送给其他进程



func Unlink
```golang
func Unlink(path string) (err error)
```


func Unlinkat
```golang
func Unlinkat(dirfd int, path string) (err error)
```


func Unmount
```golang
func Unmount(target string, flags int) (err error)
```


func Unshare
```golang
func Unshare(flags int) (err error)
```



func Ustat
```golang
func Ustat(dev int, ubuf *Ustat_t) (err error)
```


func Utime
```golang
func Utime(path string, buf *Utimbuf) (err error)
```


func Utimes
```golang
func Utimes(path string, times *[2]Timeval) (err error)
```


func UtimesNano
```golang
func UtimesNano(path string, ts []Timespec) (err error)
```


func Wait4
```golang
func Wait4(pid int, wstatus *WaitStatus, options int, rusage *Rusage) (wpid int, err error)
```


func Write
```golang
func Write(fd int, p []byte) (n int, err error)
```


type BpfHdr
```golang
type BpfHdr struct {
        Tstamp    BpfTimeval
        Caplen    uint32
        Datalen   uint32
        Hdrlen    uint16
        Pad_cgo_0 [2]byte
}
```


type BpfInsn
```golang
type BpfInsn struct {
        Code uint16
        Jt   uint8
        Jf   uint8
        K    uint32
}
```


type BpfProgram
```golang
type BpfProgram struct {
        Len       uint32
        Pad_cgo_0 [4]byte
        Insns     *BpfInsn
}
```



type BpfStat
```golang
type BpfStat struct {
        Recv    uint64
        Drop    uint64
        Capt    uint64
        Padding [13]uint64
}
```



type BpfTimeval
```golang
type BpfTimeval struct {
        Sec  int32
        Usec int32
}
```



type BpfVersion
```golang
type BpfVersion struct {
        Major uint16
        Minor uint16
}
```



type Cmsghdr
```golang
type Cmsghdr struct {
        Len   uint32
        Level int32
        Type  int32
}
```



func (*Cmsghdr) SetLen
```golang
func (cmsg *Cmsghdr) SetLen(length int)
```



type Credential
```golang
type Credential struct {
        Uid    uint32   // User ID.
        Gid    uint32   // Group ID.
        Groups []uint32 // Supplementary group IDs.
}
```
Credential holds user and group identities to be assumed by a child process started by StartProcess.
Credential 保留用户和组 标示  可以假定  子进程 是用 StartProcess 启动的.



type Dirent
```golang
type Dirent struct {
        Ino       uint64
        Off       int64
        Reclen    uint16
        Name      [1]int8
        Pad_cgo_0 [5]byte
}
```



type EpollEvent
```golang
type EpollEvent struct {
        Events uint32
        Fd     int32
        Pad    int32
}
```


type Errno
```golang
type Errno uintptr
```
An Errno is an unsigned number describing an error condition. It implements the error interface. 
The zero Errno is by convention a non-error, so code to convert from Errno to error should use:
Errno 是一个无符号数字 描述错误条件.它实现了错误接口. 零Errno 是一个 非错误, 所以 代码 转化 Errno 成 错误 应该使用:

```golang
err = nil
if errno != 0 {
	err = errno
}
```


```golang
const (
        // native_client/src/trusted/service_runtime/include/sys/errno.h
        // The errors are mainly copied from Linux.
        EPERM           Errno = 1       /* Operation not permitted */
        ENOENT          Errno = 2       /* No such file or directory */
        ESRCH           Errno = 3       /* No such process */
        EINTR           Errno = 4       /* Interrupted system call */
        EIO             Errno = 5       /* I/O error */
        ENXIO           Errno = 6       /* No such device or address */
        E2BIG           Errno = 7       /* Argument list too long */
        ENOEXEC         Errno = 8       /* Exec format error */
        EBADF           Errno = 9       /* Bad file number */
        ECHILD          Errno = 10      /* No child processes */
        EAGAIN          Errno = 11      /* Try again */
        ENOMEM          Errno = 12      /* Out of memory */
        EACCES          Errno = 13      /* Permission denied */
        EFAULT          Errno = 14      /* Bad address */
        EBUSY           Errno = 16      /* Device or resource busy */
        EEXIST          Errno = 17      /* File exists */
        EXDEV           Errno = 18      /* Cross-device link */
        ENODEV          Errno = 19      /* No such device */
        ENOTDIR         Errno = 20      /* Not a directory */
        EISDIR          Errno = 21      /* Is a directory */
        EINVAL          Errno = 22      /* Invalid argument */
        ENFILE          Errno = 23      /* File table overflow */
        EMFILE          Errno = 24      /* Too many open files */
        ENOTTY          Errno = 25      /* Not a typewriter */
        EFBIG           Errno = 27      /* File too large */
        ENOSPC          Errno = 28      /* No space left on device */
        ESPIPE          Errno = 29      /* Illegal seek */
        EROFS           Errno = 30      /* Read-only file system */
        EMLINK          Errno = 31      /* Too many links */
        EPIPE           Errno = 32      /* Broken pipe */
        ENAMETOOLONG    Errno = 36      /* File name too long */
        ENOSYS          Errno = 38      /* Function not implemented */
        EDQUOT          Errno = 122     /* Quota exceeded */
        EDOM            Errno = 33      /* Math arg out of domain of func */
        ERANGE          Errno = 34      /* Math result not representable */
        EDEADLK         Errno = 35      /* Deadlock condition */
        ENOLCK          Errno = 37      /* No record locks available */
        ENOTEMPTY       Errno = 39      /* Directory not empty */
        ELOOP           Errno = 40      /* Too many symbolic links */
        ENOMSG          Errno = 42      /* No message of desired type */
        EIDRM           Errno = 43      /* Identifier removed */
        ECHRNG          Errno = 44      /* Channel number out of range */
        EL2NSYNC        Errno = 45      /* Level 2 not synchronized */
        EL3HLT          Errno = 46      /* Level 3 halted */
        EL3RST          Errno = 47      /* Level 3 reset */
        ELNRNG          Errno = 48      /* Link number out of range */
        EUNATCH         Errno = 49      /* Protocol driver not attached */
        ENOCSI          Errno = 50      /* No CSI structure available */
        EL2HLT          Errno = 51      /* Level 2 halted */
        EBADE           Errno = 52      /* Invalid exchange */
        EBADR           Errno = 53      /* Invalid request descriptor */
        EXFULL          Errno = 54      /* Exchange full */
        ENOANO          Errno = 55      /* No anode */
        EBADRQC         Errno = 56      /* Invalid request code */
        EBADSLT         Errno = 57      /* Invalid slot */
        EDEADLOCK       Errno = EDEADLK /* File locking deadlock error */
        EBFONT          Errno = 59      /* Bad font file fmt */
        ENOSTR          Errno = 60      /* Device not a stream */
        ENODATA         Errno = 61      /* No data (for no delay io) */
        ETIME           Errno = 62      /* Timer expired */
        ENOSR           Errno = 63      /* Out of streams resources */
        ENONET          Errno = 64      /* Machine is not on the network */
        ENOPKG          Errno = 65      /* Package not installed */
        EREMOTE         Errno = 66      /* The object is remote */
        ENOLINK         Errno = 67      /* The link has been severed */
        EADV            Errno = 68      /* Advertise error */
        ESRMNT          Errno = 69      /* Srmount error */
        ECOMM           Errno = 70      /* Communication error on send */
        EPROTO          Errno = 71      /* Protocol error */
        EMULTIHOP       Errno = 72      /* Multihop attempted */
        EDOTDOT         Errno = 73      /* Cross mount point (not really error) */
        EBADMSG         Errno = 74      /* Trying to read unreadable message */
        EOVERFLOW       Errno = 75      /* Value too large for defined data type */
        ENOTUNIQ        Errno = 76      /* Given log. name not unique */
        EBADFD          Errno = 77      /* f.d. invalid for this operation */
        EREMCHG         Errno = 78      /* Remote address changed */
        ELIBACC         Errno = 79      /* Can't access a needed shared lib */
        ELIBBAD         Errno = 80      /* Accessing a corrupted shared lib */
        ELIBSCN         Errno = 81      /* .lib section in a.out corrupted */
        ELIBMAX         Errno = 82      /* Attempting to link in too many libs */
        ELIBEXEC        Errno = 83      /* Attempting to exec a shared library */
        EILSEQ          Errno = 84
        EUSERS          Errno = 87
        ENOTSOCK        Errno = 88  /* Socket operation on non-socket */
        EDESTADDRREQ    Errno = 89  /* Destination address required */
        EMSGSIZE        Errno = 90  /* Message too long */
        EPROTOTYPE      Errno = 91  /* Protocol wrong type for socket */
        ENOPROTOOPT     Errno = 92  /* Protocol not available */
        EPROTONOSUPPORT Errno = 93  /* Unknown protocol */
        ESOCKTNOSUPPORT Errno = 94  /* Socket type not supported */
        EOPNOTSUPP      Errno = 95  /* Operation not supported on transport endpoint */
        EPFNOSUPPORT    Errno = 96  /* Protocol family not supported */
        EAFNOSUPPORT    Errno = 97  /* Address family not supported by protocol family */
        EADDRINUSE      Errno = 98  /* Address already in use */
        EADDRNOTAVAIL   Errno = 99  /* Address not available */
        ENETDOWN        Errno = 100 /* Network interface is not configured */
        ENETUNREACH     Errno = 101 /* Network is unreachable */
        ENETRESET       Errno = 102
        ECONNABORTED    Errno = 103 /* Connection aborted */
        ECONNRESET      Errno = 104 /* Connection reset by peer */
        ENOBUFS         Errno = 105 /* No buffer space available */
        EISCONN         Errno = 106 /* Socket is already connected */
        ENOTCONN        Errno = 107 /* Socket is not connected */
        ESHUTDOWN       Errno = 108 /* Can't send after socket shutdown */
        ETOOMANYREFS    Errno = 109
        ETIMEDOUT       Errno = 110 /* Connection timed out */
        ECONNREFUSED    Errno = 111 /* Connection refused */
        EHOSTDOWN       Errno = 112 /* Host is down */
        EHOSTUNREACH    Errno = 113 /* Host is unreachable */
        EALREADY        Errno = 114 /* Socket already connected */
        EINPROGRESS     Errno = 115 /* Connection already in progress */
        ESTALE          Errno = 116
        ENOTSUP         Errno = EOPNOTSUPP /* Not supported */
        ENOMEDIUM       Errno = 123        /* No medium (in tape drive) */
        ECANCELED       Errno = 125        /* Operation canceled. */
        ELBIN           Errno = 2048       /* Inode is remote (not really error) */
        EFTYPE          Errno = 2049       /* Inappropriate file type or format */
        ENMFILE         Errno = 2050       /* No more files */
        EPROCLIM        Errno = 2051
        ENOSHARE        Errno = 2052   /* No such host or network path */
        ECASECLASH      Errno = 2053   /* Filename exists with different case */
        EWOULDBLOCK     Errno = EAGAIN /* Operation would block */
)
```
TODO: Auto-generate some day. (Hard-coded in binaries so not likely to change.)
TODO:自动生成某一天.(是二进制硬编码所以不可能改变。)



func (Errno) Error
```golang
func (e Errno) Error() string
```


func (Errno) Temporary
```golang
func (e Errno) Temporary() bool
```


func (Errno) Timeout
```golang
func (e Errno) Timeout() bool
```



type FdSet
```golang
type FdSet struct {
        Bits [1024]int64
}
```



type Flock_t
```golang
type Flock_t struct {
        Type      int16
        Whence    int16
        Pad_cgo_0 [4]byte
        Start     int64
        Len       int64
        Sysid     int32
        Pid       int32
        Pad       [4]int64
}
```



type Fsid
```golang
type Fsid struct {
        X__val [2]int32
}
```


type ICMPv6Filter
```golang
type ICMPv6Filter struct {
        X__icmp6_filt [8]uint32
}
```



func GetsockoptICMPv6Filter
```golang
func GetsockoptICMPv6Filter(fd, level, opt int) (*ICMPv6Filter, error)
```


type IPMreq
```golang
type IPMreq struct {
        Multiaddr [4]byte /* in_addr */
        Interface [4]byte /* in_addr */
}
```



func GetsockoptIPMreq
```golang
func GetsockoptIPMreq(fd, level, opt int) (*IPMreq, error)
```


type IPMreqn
```golang
type IPMreqn struct {
        Multiaddr [4]byte /* in_addr */
        Address   [4]byte /* in_addr */
        Ifindex   int32
}
```



func GetsockoptIPMreqn
```golang
func GetsockoptIPMreqn(fd, level, opt int) (*IPMreqn, error)
```



type IPv6MTUInfo
```golang
type IPv6MTUInfo struct {
        Addr RawSockaddrInet6
        Mtu  uint32
}
```



func GetsockoptIPv6MTUInfo
```golang
func GetsockoptIPv6MTUInfo(fd, level, opt int) (*IPv6MTUInfo, error)
```



type IPv6Mreq
```golang
type IPv6Mreq struct {
        Multiaddr [16]byte /* in6_addr */
        Interface uint32
}
```


func GetsockoptIPv6Mreq
```golang
func GetsockoptIPv6Mreq(fd, level, opt int) (*IPv6Mreq, error)
```


type IfAddrmsg
```golang
type IfAddrmsg struct {
        Family    uint8
        Prefixlen uint8
        Flags     uint8
        Scope     uint8
        Index     uint32
}
```


type IfData
```golang
type IfData struct {
        Type       uint8
        Addrlen    uint8
        Hdrlen     uint8
        Pad_cgo_0  [1]byte
        Mtu        uint32
        Metric     uint32
        Baudrate   uint32
        Ipackets   uint32
        Ierrors    uint32
        Opackets   uint32
        Oerrors    uint32
        Collisions uint32
        Ibytes     uint32
        Obytes     uint32
        Imcasts    uint32
        Omcasts    uint32
        Iqdrops    uint32
        Noproto    uint32
        Lastchange Timeval32
}
```


type IfInfomsg
```golang
type IfInfomsg struct {
        Family     uint8
        X__ifi_pad uint8
        Type       uint16
        Index      int32
        Flags      uint32
        Change     uint32
}
```


type IfMsghdr
```golang
type IfMsghdr struct {
        Msglen    uint16
        Version   uint8
        Type      uint8
        Addrs     int32
        Flags     int32
        Index     uint16
        Pad_cgo_0 [2]byte
        Data      IfData
}
```



type IfaMsghdr
```golang
type IfaMsghdr struct {
        Msglen    uint16
        Version   uint8
        Type      uint8
        Addrs     int32
        Flags     int32
        Index     uint16
        Pad_cgo_0 [2]byte
        Metric    int32
}
```



type Inet4Pktinfo
```golang
type Inet4Pktinfo struct {
        Ifindex  int32
        Spec_dst [4]byte /* in_addr */
        Addr     [4]byte /* in_addr */
}
```


type Inet6Pktinfo
```golang
type Inet6Pktinfo struct {
        Addr    [16]byte /* in6_addr */
        Ifindex uint32
}
```


type InotifyEvent
```golang
type InotifyEvent struct {
        Wd     int32
        Mask   uint32
        Cookie uint32
        Len    uint32
        Name   [0]uint8
}
```


type Iovec
```golang
type Iovec struct {
        Base *int8
        Len  uint64
}
```


func (*Iovec) SetLen
```golang
func (iov *Iovec) SetLen(length int)
```


type Linger
```golang
type Linger struct {
        Onoff  int32
        Linger int32
}
```


type Msghdr
```golang
type Msghdr struct {
        Name         *byte
        Namelen      uint32
        Pad_cgo_0    [4]byte
        Iov          *Iovec
        Iovlen       int32
        Pad_cgo_1    [4]byte
        Accrights    *int8
        Accrightslen int32
        Pad_cgo_2    [4]byte
}
```


func (*Msghdr) SetControllen
```golang
func (msghdr *Msghdr) SetControllen(length int)
```


type NetlinkMessage
```golang
type NetlinkMessage struct {
        Header NlMsghdr
        Data   []byte
}
```
NetlinkMessage represents a netlink message.
NetlinkMessage 表示一个网络链路信息.


type NetlinkRouteAttr
```golang
type NetlinkRouteAttr struct {
        Attr  RtAttr
        Value []byte
}
```
NetlinkRouteAttr represents a netlink route attribute.
NetlinkRouteAttr 表示一个 网络链路的路由属性。



type NetlinkRouteRequest
```golang
type NetlinkRouteRequest struct {
        Header NlMsghdr
        Data   RtGenmsg
}
```
NetlinkRouteRequest represents a request message to receive routing and link states from the kernel.
NetlinkRouteRequest 表示请求消息从内核接收路由和链路状态。


type NlAttr
```golang
type NlAttr struct {
        Len  uint16
        Type uint16
}
```



type NlMsgerr
```golang
type NlMsgerr struct {
        Error int32
        Msg   NlMsghdr
}
```


type NlMsghdr
```golang
type NlMsghdr struct {
        Len   uint32
        Type  uint16
        Flags uint16
        Seq   uint32
        Pid   uint32
}
```


type ProcAttr
```golang
type ProcAttr struct {
        Dir   string
        Env   []string
        Files []uintptr
        Sys   *SysProcAttr
}
```
XXX made up
XXX组成


type PtraceRegs
```golang
type PtraceRegs struct {
        R15      uint64
        R14      uint64
        R13      uint64
        R12      uint64
        Rbp      uint64
        Rbx      uint64
        R11      uint64
        R10      uint64
        R9       uint64
        R8       uint64
        Rax      uint64
        Rcx      uint64
        Rdx      uint64
        Rsi      uint64
        Rdi      uint64
        Orig_rax uint64
        Rip      uint64
        Cs       uint64
        Eflags   uint64
        Rsp      uint64
        Ss       uint64
        Fs_base  uint64
        Gs_base  uint64
        Ds       uint64
        Es       uint64
        Fs       uint64
        Gs       uint64
}
```


func (*PtraceRegs) PC
```golang
func (r *PtraceRegs) PC() uint64
```


func (*PtraceRegs) SetPC
```golang
func (r *PtraceRegs) SetPC(pc uint64)
```


type RawSockaddr
```golang
type RawSockaddr struct {
        Family uint16
        Data   [14]int8
}
```


type RawSockaddrAny
```golang
type RawSockaddrAny struct {
        Addr RawSockaddr
        Pad  [236]int8
}
```


type RawSockaddrDatalink
```golang
type RawSockaddrDatalink struct {
        Family uint16
        Index  uint16
        Type   uint8
        Nlen   uint8
        Alen   uint8
        Slen   uint8
        Data   [244]int8
}
```


type RawSockaddrInet4
```golang
type RawSockaddrInet4 struct {
        Family uint16
        Port   uint16
        Addr   [4]byte /* in_addr */
        Zero   [8]int8
}
```


type RawSockaddrInet6
```golang
type RawSockaddrInet6 struct {
        Family         uint16
        Port           uint16
        Flowinfo       uint32
        Addr           [16]byte /* in6_addr */
        Scope_id       uint32
        X__sin6_src_id uint32
}
```



type RawSockaddrLinklayer
```golang
type RawSockaddrLinklayer struct {
        Family   uint16
        Protocol uint16
        Ifindex  int32
        Hatype   uint16
        Pkttype  uint8
        Halen    uint8
        Addr     [8]uint8
}
```



type RawSockaddrNetlink
```golang
type RawSockaddrNetlink struct {
        Family uint16
        Pad    uint16
        Pid    uint32
        Groups uint32
}
```



type RawSockaddrUnix
```golang
type RawSockaddrUnix struct {
        Family uint16
        Path   [108]int8
}
```



type Rlimit
```golang
type Rlimit struct {
        Cur uint64
        Max uint64
}
```



type RoutingMessage
```golang
type RoutingMessage interface {
        // contains filtered or unexported methods
}
```
RoutingMessage represents a routing message.
RoutingMessage 表示一个路由信息



type RtAttr
```golang
type RtAttr struct {
        Len  uint16
        Type uint16
}
```



type RtGenmsg
```golang
type RtGenmsg struct {
        Family uint8
}
```



type RtMetrics
```golang
type RtMetrics struct {
        Locks    uint32
        Mtu      uint32
        Hopcount uint32
        Expire   uint32
        Recvpipe uint32
        Sendpipe uint32
        Ssthresh uint32
        Rtt      uint32
        Rttvar   uint32
        Pksent   uint32
}
```



type RtMsg
```golang
type RtMsg struct {
        Family   uint8
        Dst_len  uint8
        Src_len  uint8
        Tos      uint8
        Table    uint8
        Protocol uint8
        Scope    uint8
        Type     uint8
        Flags    uint32
}
```



type RtMsghdr
```golang
type RtMsghdr struct {
        Msglen    uint16
        Version   uint8
        Type      uint8
        Index     uint16
        Pad_cgo_0 [2]byte
        Flags     int32
        Addrs     int32
        Pid       int32
        Seq       int32
        Errno     int32
        Use       int32
        Inits     uint32
        Rmx       RtMetrics
}
```


type RtNexthop
```golang
type RtNexthop struct {
        Len     uint16
        Flags   uint8
        Hops    uint8
        Ifindex int32
}
```



type Rusage
```golang
type Rusage struct {
        Utime    Timeval
        Stime    Timeval
        Maxrss   int64
        Ixrss    int64
        Idrss    int64
        Isrss    int64
        Minflt   int64
        Majflt   int64
        Nswap    int64
        Inblock  int64
        Oublock  int64
        Msgsnd   int64
        Msgrcv   int64
        Nsignals int64
        Nvcsw    int64
        Nivcsw   int64
}
```



type Signal
```golang
type Signal int
```
A Signal is a number describing a process signal. It implements the os.Signal interface.
Signal 是一个 描述进程信息的号码. 它实现了 os.Signal 接口.



func (Signal) Signal
```golang
func (s Signal) Signal()
```


func (Signal) String
```golang
func (s Signal) String() string
```


type SockFilter
```golang
type SockFilter struct {
        Code uint16
        Jt   uint8
        Jf   uint8
        K    uint32
}
```


func LsfJump
```golang
func LsfJump(code, k, jt, jf int) *SockFilter
```


func LsfStmt
```golang
func LsfStmt(code, k int) *SockFilter
```


type SockFprog
```golang
type SockFprog struct {
        Len       uint16
        Pad_cgo_0 [6]byte
        Filter    *SockFilter
}
```



type Sockaddr
```golang
type Sockaddr interface {
        // contains filtered or unexported methods
}
```



func Getpeername
```golang
func Getpeername(fd int) (sa Sockaddr, err error)
```



func Getsockname
```golang
func Getsockname(fd int) (sa Sockaddr, err error)
```



type SockaddrDatalink
```golang
type SockaddrDatalink struct {
        Family uint16
        Index  uint16
        Type   uint8
        Nlen   uint8
        Alen   uint8
        Slen   uint8
        Data   [244]int8
        // contains filtered or unexported fields
}
```


type SockaddrInet4
```golang
type SockaddrInet4 struct {
        Port int
        Addr [4]byte
        // contains filtered or unexported fields
}
```



type SockaddrInet6
```golang
type SockaddrInet6 struct {
        Port   int
        ZoneId uint32
        Addr   [16]byte
        // contains filtered or unexported fields
}
```



type SockaddrLinklayer
```golang
type SockaddrLinklayer struct {
        Protocol uint16
        Ifindex  int
        Hatype   uint16
        Pkttype  uint8
        Halen    uint8
        Addr     [8]byte
        // contains filtered or unexported fields
}
```


type SockaddrNetlink
```golang
type SockaddrNetlink struct {
        Family uint16
        Pad    uint16
        Pid    uint32
        Groups uint32
        // contains filtered or unexported fields
}
```


type SockaddrUnix
```golang
type SockaddrUnix struct {
        Name string
        // contains filtered or unexported fields
}
```


type SocketControlMessage
```golang
type SocketControlMessage struct {
        Header Cmsghdr
        Data   []byte
}
```
SocketControlMessage represents a socket control message.
SocketControlMessage表示一个socket控制 信号.



type Stat_t
```golang
type Stat_t struct {
        Dev       uint64
        Ino       uint64
        Mode      uint32
        Nlink     uint32
        Uid       uint32
        Gid       uint32
        Rdev      uint64
        Size      int64
        Atim      Timespec
        Mtim      Timespec
        Ctim      Timespec
        Blksize   int32
        Pad_cgo_0 [4]byte
        Blocks    int64
        Fstype    [16]int8
}
```


type Statfs_t
```golang
type Statfs_t struct {
        Type    int64
        Bsize   int64
        Blocks  uint64
        Bfree   uint64
        Bavail  uint64
        Files   uint64
        Ffree   uint64
        Fsid    Fsid
        Namelen int64
        Frsize  int64
        Flags   int64
        Spare   [4]int64
}
```


type SysProcAttr
```golang
type SysProcAttr struct {
}
```


type Sysinfo_t
```golang
type Sysinfo_t struct {
        Uptime    int64
        Loads     [3]uint64
        Totalram  uint64
        Freeram   uint64
        Sharedram uint64
        Bufferram uint64
        Totalswap uint64
        Freeswap  uint64
        Procs     uint16
        Pad       uint16
        Pad_cgo_0 [4]byte
        Totalhigh uint64
        Freehigh  uint64
        Unit      uint32
        X_f       [0]byte
        Pad_cgo_1 [4]byte
}
```



type TCPInfo
```golang
type TCPInfo struct {
        State          uint8
        Ca_state       uint8
        Retransmits    uint8
        Probes         uint8
        Backoff        uint8
        Options        uint8
        Pad_cgo_0      [2]byte
        Rto            uint32
        Ato            uint32
        Snd_mss        uint32
        Rcv_mss        uint32
        Unacked        uint32
        Sacked         uint32
        Lost           uint32
        Retrans        uint32
        Fackets        uint32
        Last_data_sent uint32
        Last_ack_sent  uint32
        Last_data_recv uint32
        Last_ack_recv  uint32
        Pmtu           uint32
        Rcv_ssthresh   uint32
        Rtt            uint32
        Rttvar         uint32
        Snd_ssthresh   uint32
        Snd_cwnd       uint32
        Advmss         uint32
        Reordering     uint32
        Rcv_rtt        uint32
        Rcv_space      uint32
        Total_retrans  uint32
}
```



type Termios
```golang
type Termios struct {
        Iflag     uint32
        Oflag     uint32
        Cflag     uint32
        Lflag     uint32
        Cc        [19]uint8
        Pad_cgo_0 [1]byte
}
```



type Time_t
```golang
type Time_t int64
```


func Time
```golang
func Time(t *Time_t) (tt Time_t, err error)
```



type Timespec
```golang
type Timespec struct {
        Sec  int64
        Nsec int64
}
```


func NsecToTimespec
```golang
func NsecToTimespec(nsec int64) (ts Timespec)
```


func (*Timespec) Nano
```golang
func (ts *Timespec) Nano() int64
```


func (*Timespec) Unix
```golang
func (ts *Timespec) Unix() (sec int64, nsec int64)
```


type Timeval
```golang
type Timeval struct {
        Sec  int64
        Usec int64
}
```


func NsecToTimeval
```golang
func NsecToTimeval(nsec int64) (tv Timeval)
```


func (*Timeval) Nano
```golang
func (tv *Timeval) Nano() int64
```


func (*Timeval) Unix
```golang
func (tv *Timeval) Unix() (sec int64, nsec int64)
```


type Timeval32
```golang
type Timeval32 struct {
        Sec  int32
        Usec int32
}
```


type Timex
```golang
type Timex struct {
        Modes     uint32
        Pad_cgo_0 [4]byte
        Offset    int64
        Freq      int64
        Maxerror  int64
        Esterror  int64
        Status    int32
        Pad_cgo_1 [4]byte
        Constant  int64
        Precision int64
        Tolerance int64
        Time      Timeval
        Tick      int64
        Ppsfreq   int64
        Jitter    int64
        Shift     int32
        Pad_cgo_2 [4]byte
        Stabil    int64
        Jitcnt    int64
        Calcnt    int64
        Errcnt    int64
        Stbcnt    int64
        Tai       int32
        Pad_cgo_3 [44]byte
}
```


type Tms
```golang
type Tms struct {
        Utime  int64
        Stime  int64
        Cutime int64
        Cstime int64
}
```



type Ucred
```golang
type Ucred struct {
        Pid int32
        Uid uint32
        Gid uint32
}
```



func GetsockoptUcred
```golang
func GetsockoptUcred(fd, level, opt int) (*Ucred, error)
```


func ParseUnixCredentials
```golang
func ParseUnixCredentials(m *SocketControlMessage) (*Ucred, error)
```
ParseUnixCredentials decodes a socket control message that contains credentials in a Ucred structure. 
To receive such a message, the SO_PASSCRED option must be enabled on the socket.
ParseUnixCredentials 解码 socket 控制 信息 包含 Ucred 结构体里的证书的
为了接收一个信息, socket 上的SO_PASSCRED选项必须启用.



type Ustat_t
```golang
type Ustat_t struct {
        Tfree     int32
        Pad_cgo_0 [4]byte
        Tinode    uint64
        Fname     [6]int8
        Fpack     [6]int8
        Pad_cgo_1 [4]byte
}
```


type Utimbuf
```golang
type Utimbuf struct {
        Actime  int64
        Modtime int64
}
```



type Utsname
```golang
type Utsname struct {
        Sysname    [65]int8
        Nodename   [65]int8
        Release    [65]int8
        Version    [65]int8
        Machine    [65]int8
        Domainname [65]int8
}
```


type WaitStatus
```golang
type WaitStatus uint32
```



func (WaitStatus) Continued
```golang
func (w WaitStatus) Continued() bool
```


func (WaitStatus) CoreDump
```golang
func (w WaitStatus) CoreDump() bool
```



func (WaitStatus) ExitStatus
```golang
func (w WaitStatus) ExitStatus() int
```



func (WaitStatus) Exited
```golang
func (w WaitStatus) Exited() bool
```



func (WaitStatus) Signal
```golang
func (w WaitStatus) Signal() Signal
```


func (WaitStatus) Signaled
```golang
func (w WaitStatus) Signaled() bool
```



func (WaitStatus) StopSignal
```golang
func (w WaitStatus) StopSignal() Signal
```


func (WaitStatus) Stopped
```golang
func (w WaitStatus) Stopped() bool
```


func (WaitStatus) TrapCause
```golang
func (w WaitStatus) TrapCause() int
```


