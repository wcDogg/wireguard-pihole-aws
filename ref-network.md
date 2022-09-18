# Network Reference

Some different ways of looking at a network.

```bash
# 172.26.0.1
ip -4 route | grep default | cut -d ' ' -f 3
# fe80::c2d:5cff:fe5c:c0e9
ip -6 route | grep default | cut -d ' ' -f 3

# -- nothing
ip -4 route | grep lo | cut -d ' ' -f 3
# lo
ip -6 route | grep lo | cut -d ' ' -f 3

# 172.26.0.1, eth0, eth0
ip -4 route | grep eth0 | cut -d ' ' -f 3
# eth0, eth0, fe80::c2d:5cff:fe5c:c0e9
ip -6 route | grep eth0 | cut -d ' ' -f 3

# wg0 
ip -4 route | grep wg0 | cut -d ' ' -f 3
# wg0
ip -6 route | grep wg0 | cut -d ' ' -f 3


ifconfig lo
# lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
#         inet 127.0.0.1  netmask 255.0.0.0
#         inet6 ::1  prefixlen 128  scopeid 0x10<host>
#         loop  txqueuelen 1000  (Local Loopback)
#         RX packets 14166  bytes 954781 (954.7 KB)
#         RX errors 0  dropped 0  overruns 0  frame 0
#         TX packets 14166  bytes 954781 (954.7 KB)
#         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ifconfig eth0
# eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
#         inet 172.26.8.187  netmask 255.255.240.0  broadcast 172.26.15.255
#         inet6 2600:1f18:61c8:d000:94fd:d562:e2a0:576c  prefixlen 128  scopeid 0x0<global>
#         inet6 fe80::c1b:5dff:fead:64a7  prefixlen 64  scopeid 0x20<link>
#         ether 0e:1b:5d:ad:64:a7  txqueuelen 1000  (Ethernet)
#         RX packets 12128  bytes 1625878 (1.6 MB)
#         RX errors 0  dropped 0  overruns 0  frame 0
#         TX packets 9759  bytes 3493679 (3.4 MB)
#         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ifconfig wg0
# wg0: flags=209<UP,POINTOPOINT,RUNNING,NOARP>  mtu 1420
#         inet 10.177.23.1  netmask 255.255.255.0  destination 10.177.23.1
#         inet6 fd11:5ee:bad:c0de::1  prefixlen 64  scopeid 0x0<global>
#         unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
#         RX packets 0  bytes 0 (0.0 B)
#         RX errors 0  dropped 0  overruns 0  frame 0
#         TX packets 0  bytes 0 (0.0 B)
#         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ip addr
# 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
#     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
#     inet 127.0.0.1/8 scope host lo
#        valid_lft forever preferred_lft forever
#     inet6 ::1/128 scope host
#        valid_lft forever preferred_lft forever
# 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc fq_codel state UP group default qlen 1000
#     link/ether 0e:1b:5d:ad:64:a7 brd ff:ff:ff:ff:ff:ff
#     inet 172.26.8.187/20 brd 172.26.15.255 scope global dynamic eth0
#        valid_lft 2808sec preferred_lft 2808sec
#     inet6 2600:1f18:61c8:d000:94fd:d562:e2a0:576c/128 scope global dynamic noprefixroute
#        valid_lft 425sec preferred_lft 115sec
#     inet6 fe80::c1b:5dff:fead:64a7/64 scope link
#        valid_lft forever preferred_lft forever
# 3: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
#     link/none
#     inet 10.177.23.1/24 scope global wg0
#        valid_lft forever preferred_lft forever
#     inet6 fd11:5ee:bad:c0de::1/64 scope global
#        valid_lft forever preferred_lft forever
```