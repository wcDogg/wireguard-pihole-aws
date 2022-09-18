# Connect and Update Ubuntu


## Connect

```powershell
# Using public IPv4
ssh -i ~/.ssh/my-key.pem ubuntu@3.xxx.xxx.xxx

# Using domain
ssh -i ~/.ssh/my-key.pem ubuntu@site.com
```

## Set Hostname

```bash
# Change host name to domain name
sudo hostnamectl set-hostname site.com

# View hostname
hostnamectl

# Edit hosts file
sudo editor /etc/hosts

# Add domain to first line
127.0.0.1 localhost site.com
```

## Set Time Zone

```bash
# Set timezone to match server instance
sudo timedatectl set-timezone America/New_York

# View datetime info
timedatectl
```

## Enable IP Forwarding

This step is in preparation for routing all Internet traffic through WireGuard.

```bash
# Edit this file
sudo editor /etc/sysctl.d/99-sysctl.conf

# Uncomment these lines
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1

# Apply changes
sudo sysctl -p

# Output
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
```

## Update Ubuntu

```bash
# Update package list
sudo apt update

# Apply updates
# IMPORTANT: At the OpenSSH prompt, select 'Keep local version'
sudo apt upgrade -y

# Remove unneeded packages if prompted
sudo apt autoremove

# Reboot
sudo reboot
```

## Install Dependencies

```bash
# Network utility
sudo apt -y install net-tools

# Unbound 
sudo apt install dns-root-data
sudo apt autoremove

# WG QR codes as PNG
# Already installed on Ubuntu 
sudo apt install qrencode
```

## Unattended Upgrades

NO ACTION - The Lightsail-configured defaults are correct. This is here as reference. 

```bash
# Install
sudo apt install unattended-upgrades

# Edit this file
sudo editor /etc/apt/apt.conf.d/50unattended-upgrades

# Add these rules
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}:${distro_codename}";
        "${distro_id}:${distro_codename}-security";
//      "${distro_id}:${distro_codename}-updates";
//      "${distro_id}:${distro_codename}-proposed";
//      "${distro_id}:${distro_codename}-backports";
};
```

## References

* [Pi-hole: Route the entire Internet traffic through the WireGuard tunnel](https://docs.pi-hole.net/guides/vpn/wireguard/route-everything/#route-the-entire-internet-traffic-through-the-wireguard-tunnel)
* [Pi-hole: Enable IP forwarding on the server](https://docs.pi-hole.net/guides/vpn/wireguard/internal/#enable-ip-forwarding-on-the-server)

