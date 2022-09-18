# WireGuard with Pi-hole and Unbound on AWS LightSail

A technical writer's notes on how to create a WireGuard VPN with Pi-hole ad blocking and Unbound recursive DNS on Ubuntu 20.04 LTS in AWS Lightsail. 

## Steps

* [Ubuntu 20.04 LTS Server on AWS Lightsail](01-aws-instance.md)
* [Domain Name](02-domain-name.md)
* [Connect and Configure Ubuntu](03-ubuntu.md)
* [Pi-hole](04-pihole.md)
* [Unbound](05-unbound.md)
* [PuTTY](06-putty.md)
* [PiVPN WireGuard](07-pivpn.md)
* [Firewall](08-firewall.md)
* [Logging](09-logging.md)
* [Network Reference](ref-network.md)
* [Port Reference](ref-ports.md)

## TODO

* This project works and is secure. However, it uses the Lightsail firewall. Ultimately, I want to use either IPtables or Uncomplicated Firewall. 
* While I do have IPv6 enabled on Ubuntu, the wg0 network has `fd11:5ee:bad:c0de::1`. I haven't found a solution for this. It does not prevent me from using WG on my IPv4 and IPv6 devices. 
