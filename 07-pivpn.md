# PiVPN WireGuard


## Install WireGuard App on Clients

I suggest your current computer + 1 mobile device for testing.

* [WireGuard Installation](https://www.wireguard.com/install/)


## Before and After

* Check your IP: https://www.iplocation.net/find-ip-address
* Do a speed test: https://speedtest.net
* Pull up some Pi-hole test sites:
  * https://fuzzthepiguy.tech/adtest/
  * https://weedmaps.com/
* Pull up some sites with ads: 
  * https://msn.com 
  * https://forbes.com
  * https://hackaday.com


## Install PiVPN

```bash
# Install
sudo apt update
curl -L https://install.pivpn.io | bash

# Automated Installer = OK
# Static IP Needed = OK
# IP Information = OK
# Local Users = OK
# Choose a User = ubuntu
# Installation Mode = WireGuard
# Default WireGuard Port = 51820 (default)
# Confirm Custom Port = Yes
# Pi-hole detected = Yes
# Public IP or DNS = DNS Entry
# Public DNS name = site.com
# Confirm DNS Name = Yes
# Server Keys will be generated = Yes
# Unattended upgrades recommended = Yes
# Enable unattended upgrades = No (already enabled)
# Installation complete!
# Reboot = Yes
```

## Test

```bash
# Help 
pivpn help

# Debug 
pivpn -d

# Output
=============================================
::::            Self check               ::::
:: [OK] IP forwarding is enabled
:: [OK] Iptables MASQUERADE rule set
:: [OK] WireGuard is running
:: [OK] WireGuard is enabled
(it will automatically start on reboot)
:: [OK] WireGuard is listening on port 51820/udp
=============================================
```

## Enable NAT

NAT is rewriting a packet's source and destination address as it passed through a router or firewall.

```bash
# Edit this file
sudo editor /etc/wireguard/wg0.conf

# Add these lines to [INTERFACE]
# Assumes Internet is on eth0 - may be enp2s0 or similar on newer Ubuntu
PostUp = iptables -w -t nat -A POSTROUTING -o eth0 -j MASQUERADE; ip6tables -w -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -w -t nat -D POSTROUTING -o eth0 -j MASQUERADE; ip6tables -w -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# Restart services
service networking restart
```

## TODO: Fix wg0 IPv6

As a further check, I looked at `ip addr`. There is a new wg0 network and it has a bad IPv6 address: `fd11:5ee:bad:c0de::1`. 

I still haven't found a solution for this. This hasn't prevented me from using WireGuard. 


## configs Directory

WireGuard stores the client config files in `/home/ubuntu/configs` (not root). For some reason, every time a config file is created, it's restricted to root. I found it easiest to create all the configs I needed, then recursively change the permissions. 

* https://github.com/pivpn/pivpn/issues/1608
  
```bash
sudo chown -R ubuntu:ubuntu /home/ubuntu/configs
```

## Add Clients/Peers

```bash
# Add client
sudo pivpn -a -n client-name

# Edit config
sudo editor /home/ubuntu/configs/client-name.conf

# Change this line to
# Routs all traffic through tunnel
AllowedIPs = 0.0.0.0/0, ::/0
```

## QR Code for Mobile Client

Fom mobile clients/peers, generate a QR code for scanning. 

```bash
# Add client
sudo pivpn -a -n client-name

# This does not display right in PowerShell
sudo pivpn -qr client-name

# Instead, save QR code as PNG
qrencode -t png -o /home/ubuntu/configs/client-name.png -r /home/ubuntu/configs/client-name.conf

# In a separate PowerShell not connected to Ubuntu,
# transfer PNG to Windows for easy display + scanning.
pscp -i C:\Users\user\.ssh\my-key.ppk ubuntu@site.com:/home/ubuntu/configs/client-name.png D:\WireGuard
```

## Config File for Computer Client

```bash
# Add client
sudo pivpn -a -n client-name

# Display config + copy-paste to a file on the client
cat /home/ubuntu/configs/client-name.config

# Or use PuTTY to transfer it to your computer
pscp -i C:\Users\user\.ssh\my-key.ppk ubuntu@site.com:/home/ubuntu/configs/client-name.conf D:\WireGuard
```

## Remove a Client

```bash
# Remove client
pivpn remove
```

## Connect via WireGuard

1. Launch the WireGuard app. 
2. Click **Add Tunnel"".
3. Either scan the QR code or import the config file.
4. Activate and test the connection. 

## References

* [PiVPN](https://www.pivpn.io/)
* [PiVPN Docs](https://docs.pivpn.io/)
* [Pi-hole: Enable NAT on Server](https://docs.pi-hole.net/guides/vpn/wireguard/internal/#enable-nat-on-the-server)

