# Firewall

## AWS Firewall

This topic is a work in progress. As of right now, I am using the AWS Lightsail firewall. Once PiVPN is running, close all ports except: 

```bash
# IPv4
22/TCP      # SSH restrict to my non-WG IP + AWS shell
Ping (ICMP) # Restrict to my non-WG IP

# IPv4 + IPv6
51820/UDP   # PiVPN WireGuard
```

This appears to be working correctly. For example, I must be connected via WireGuard to: 

* Access the Pi-hole web UI at http://site.com/admin or 3.xxx.xxx.xxx/admin
* To do `nslookup pi-hole.net site.com`
* To do `nslookup -port=51820 pi-hole.net wcd.red`

**The remainder of this page is NOT usable information at this time.**


## Files

* /etc/sysctl.conf
* /etc/sysctl.d/
* /etc/pivpn/wireguard/setupVars.conf
* /etc/wireguard/wg0.conf


## IP Tables

* https://dev.to/tangramvision/what-they-don-t-tell-you-about-setting-up-a-wireguard-vpn-1h2g

```bash
iptables -A INPUT -p udp -m udp --dport 51820 -j ACCEPT

iptables -A FORWARD -i wg0 -j ACCEPT
iptables -A FORWARD -o wg0 -j ACCEPT
```

* https://docs.pi-hole.net/main/prerequisites/

```bash
# Edit this file
/etc/wireguard/wg0.conf

#
# IPv4
# Pi-hole Admin UI
iptables -I INPUT 1 -s 192.168.0.0/16 -p tcp -m tcp --dport 80 -j ACCEPT
# DNS
iptables -I INPUT 1 -s 127.0.0.0/8 -p tcp -m tcp --dport 53 -j ACCEPT
iptables -I INPUT 1 -s 127.0.0.0/8 -p udp -m udp --dport 53 -j ACCEPT
iptables -I INPUT 1 -s 192.168.0.0/16 -p tcp -m tcp --dport 53 -j ACCEPT
iptables -I INPUT 1 -s 192.168.0.0/16 -p udp -m udp --dport 53 -j ACCEPT
# Pi-hole FTL API
iptables -I INPUT 1 -p tcp -m tcp --dport 4711 -i lo -j ACCEPT
iptables -I INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

#
# IPv6
ip6tables -I INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

#
# Not using Pi-hole DHCP - don't need these
iptables -I INPUT 1 -p udp --dport 67:68 --sport 67:68 -j ACCEPT
ip6tables -I INPUT -p udp -m udp --sport 546:547 --dport 546:547 -j ACCEPT
```

## UFW

```bash
# Edit this file
editor /etc/sysctl.conf

# Uncomment this line
net.ipv4.ip_forward = 1

# Status
sudo ufw status numbered

# Blank slate
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Important!! Don't lock yourself out.
sudo ufw allow 22/any

# Allow web traffic
sudo ufw allow 80/tcp 
sudo ufw allow 443/tcp 

# Allow from WG Clients
sudo ufw allow from 192.168.2.0/24

# Enable
sudo ufw enable

# Restart service
sudo ufw reload

# Remove
sudo ufw delete <rule number>
```

## References

* [Lightsail Firewall](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/understanding-firewall-and-port-mappings-in-amazon-lightsail)
* [Ubuntu Wiki: UncomplicatedFirewall](https://wiki.ubuntu.com/UncomplicatedFirewall)
* [Ubuntu: 20.04 LTS UFW Manual](https://manpages.ubuntu.com/manpages/focal/en/man8/ufw.8.html)
* [Pi-hole: Prerequisites](https://docs.pi-hole.net/main/prerequisites/)
