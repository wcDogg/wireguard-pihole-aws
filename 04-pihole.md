# Pi-hole

Default installation of Pi-hole.

## Install

```bash
# Get + run Pi-Hole install script
sudo curl -sSL https://install.pi-hole.net | bash

# Static IP Needed = Continue
# Upstream DNS Provider = Google
# Blacklists = Yes
# Admin Web Interface = Yes
# Web Server = Yes
# Enable Logging = Yes
# FTL Privacy Mode = 0 Show everything (for now)

# During install, you'll hit a 
# 'DNS resolution is currently unavailable' error.
# Ctrl+C, run command above, and select Update to continue.
```

## Change Password + Log In

```bash
# Change the password
pihole -a -p StrongNewPassword

# Verify you can log in at:
http://site.com/admin

# Note hostname in upper-right corner = site.com
```

## Permit All Origins

In the Pi-hole web UI: 

1. Settings > DNS tab > Interface Settings > Permit all origins > Save


## Change Group Permissions

```bash
# The web server user needs to be a member of the
# 'pihole' group for full functionality in admin dashboard
sudo usermod -a -G pihole www-data
```

## Run Debug

```bash
# Run debug
pihole debug

# This error is not a problem ...
[i] Default IPv6 gateway: fe80::c2d:5cff:fe5c:c0e9
   * Pinging fe80::c2d:5cff:fe5c:c0e9...
ping6: Warning: source address might be selected on device other than: eth0
[✗] Gateway did not respond. (https://discourse.pi-hole.net/t/why-is-a-default-gateway-important-for-pi-hole/3546)

# ... if this test passes
*** [ DIAGNOSING ]: Name resolution (IPv6) using a random blocked domain and a known ad-serving domain
[✓] orl.ex2landings.com is :: on lo (::1)
[✓] orl.ex2landings.com is :: on eth0 (2600:1f18:61c8:d000:94fd:d562:e2a0:576c)
[✓] orl.ex2landings.com is :: on eth0 (fe80::c1b:5dff:fead:64a7)
[✓] doubleclick.com is 2607:f8b0:4004:c08::8b via a remote, public DNS server (2001:4860:4860::8888)
```

## Test Internally from SSH Shell

```bash
# Status should = SERVFAIL
dig sigfail.verteiltesysteme.net @127.0.0.1 -p 53

# Status should = NOERROR 
dig sigok.verteiltesysteme.net @127.0.0.1 -p 53

# Status should = NOERROR 
dig pi-hole.net @127.0.0.1 -p 53
```

## Test Externally from Separate Command Line

```powershell
# nslookup uses port 53 by default
nslookup pi-hole.net 3.xxx.xxx.xxx
nslookup pi-hole.net site.com

# Options (nslookup >help)
nslookup -all -debug -type=ANY -class=ANY pi-hole.net 3.xxx.xxx.xxx
```

## Static IP

Pi-hole's documentation states:

* Pi-hole needs a static IP address to properly function (a DHCP reservation is just fine). 
* Users may run into issues because we currently install `dhcpcd5`.
* We append some lines to `/etc/dhcpcd.conf` in order to statically assign an IP address.

For me, running Ubuntu 20.04 LTS on AWS Lightsail:

* The Pi-hole installer recognized I was not using Raspbian.
* It warned I need to ensure my own static IP.
* It did not install `dhcpcd5`.
* There is no `/etc/dhcpcd.conf`.


## References

* [Pi-hole.net](https://pi-hole.net/)
* [Pi-hole Docs](https://docs.pi-hole.net/)
* [Pi-hole: The pihole Command with Examples](https://discourse.pi-hole.net/t/the-pihole-command-with-examples/738#logging)

