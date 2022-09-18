# Port Reference

```bash
# IPv4
22/TCP      # SSH restrict to my non-WG IP + AWS shell
Ping (ICMP) # Restrict to my non-WG IP

# IPv4 + IPv6
443/HTTPS   
80/HTTP     # Pi-hole web UI
53/TCP      # DNS
53/UDP      # DNS
51820/UDP   # PiVPN WireGuard

# IPv4 open temp for testing
5335/UDP    # Unbound

# localhost only
4711/TCP    # Pi-hole FTL API

# Not using Pi-hole DHCP - don't need these
67/UDP      # IPv4
547/UDP     # IPv6 
```

## References

* [Pi-hole: Prerequisites](https://docs.pi-hole.net/main/prerequisites/)
* [Pi-hole: ]
