# Unbound


## Install

```bash
# Install
sudo apt -y install unbound

# DNS server will fail to start,
# but otherwise there should be no errors.

# Create this file
sudo editor /etc/unbound/unbound.conf.d/pi-hole.conf

# Copy this as-is and save
# https://docs.pi-hole.net/guides/dns/unbound/#configure-unbound
```

## Restrict FTL

```bash
# Create 99-edns.conf file: 
sudo editor /etc/dnsmasq.d/99-edns.conf

# Add this - signals FTL to adhere to a limit: 
edns-packet-max=1232
```

## Disable resolvconf for unbound

* https://docs.pi-hole.net/guides/dns/unbound/#disable-resolvconf-for-unbound-optional

```bash
# Check status
# Active = inactive (dead)
# Fails because /sbin/resolvconf does not exist
sudo systemctl status unbound-resolvconf.service

# Disable the service
sudo systemctl disable unbound-resolvconf.service
sudo systemctl stop unbound-resolvconf.service

# Reboot + run previous tests
sudo reboot

#
# STOP HERE
# Pi-hole did not install dhcpcd as docs say. 

# Propagate changes
sudo systemctl restart dhcpcd

# Nameserver in this file...
cat /etc/resolv.conf

# Should match nameserver in this file,
# but Pi-hole did not install dhcpcd
cat /etc/dhcpcd.conf
```

## Check Status

```bash
# Check status - Active = active(running)
service unbound status

# I have this warning, but all tests DO work
unbound[576:0] warning: so-rcvbuf 1048576 was not granted. Got 42598
# Warning relates to this file
/run/unbound.pid

# It's not clear why this file isn't accessible
# Directory: drwxr-xr-x 
# File:      -rw-r--r--
```

## Test Internally from SSH shell 

```bash
# Status should = SERVFAIL
dig sigfail.verteiltesysteme.net @127.0.0.1 -p 5335

# Status should = NOERROR 
dig sigok.verteiltesysteme.net @127.0.0.1 -p 5335

# Status should = NOERROR 
dig pi-hole.net @127.0.0.1 -p 5335
```

## Test Externally from a separate command line

Temporarily open port 5335 in AWS firewall.

```powershell
# Using public IPv4 address
nslookup -port=5335 pi-hole.net 3.xxx.xxx.xxx

# Using domain name
nslookup -port=5335 pi-hole.net site.com
```

## Configure Pi-hole

1. Log in to the Pi-hole web interface - http://<Public IPv4>/admin.
2. Settings > DNS tab.
3. Uncheck the existing Upstream DNS Servers.
4. Add Custom IPv4 = 127.0.0.1#5335.
5. Scroll down + Save.
6. Settings > System > Restart DNS Resolver button.
7. Re-run the Unbound and Pi-hole command line tests.

```powershell
# From a separate command line
nslookup pi-hole.net site.com
nslookup -port=5335 pi-hole.net site.com
```

## References

* [Unbound](https://www.nlnetlabs.nl/projects/unbound/about/)
* [RedHat: Introduction to Unbound](https://www.redhat.com/sysadmin/bound-dns)
* [Pi-hole: Unbound Guide](https://docs.pi-hole.net/guides/dns/unbound/)
* [F5: Validating Resolver](https://clouddocs.f5.com/training/community/dns/html/class2/module5/module5.html)


## About Unbound

Unbound is a validating, recursive, caching DNS resolver. 

* A DNS resolver maps a request for twitter.com to its IP address (104.244.42.193).
* A validating resolver asks twitter.com to include a DNSSEC signature in its response. DNSSEC is used to cryptographically validate the authenticity of a response. 
* Caching remembers twitter.com's IP address so you can access it faster in the future. Caching is a feature of recursive resolvers. 
* The first step in finding twitter.com's IP address is to consult a recursive DNS resolver to see if it's been cached. Unbound is a recursive DNS server, so it checks its own cache first, before reaching out to upstream resolver - probably Google. 
* If both Unbound + Google haven't cached the IP address for twitter.com, then Unbound traverses a hierarchy of authoritative DNS servers on your behalf. 

The net result is that it's much harder for services like Google to build a picture of your Internet usage. 
