# Ubuntu 20.04 LTS Server on AWS Lightsail


## AWS Account

* [Create AWS account](https://aws.amazon.com/)
* [Lightsail login](https://lightsail.aws.amazon.com/)


## Why Lightsail?

Not only is AWS Lightsail [competitively priced](docs/01-aws-ubuntu.md/#which-tier), **it lets you create a static IP address as its own asset** that can be attached to a server instance. 

As someone who is learning, it's often easiest to take down an instance & start fresh. Having an attachable/detachable static IP saves me from having to update my DNS records & wait for propagation each time.


## Which Tier?

When selecting a tier, the **transfer TB** metric is most important. The more clients using the VPN, the more streaming you do, the higher the tier. I suggest the 2TB tier as a minimum

* [Lightsail pricing](https://aws.amazon.com/lightsail/pricing/)
* [EC2 pricing](https://aws.amazon.com/ec2/pricing/on-demand/) 
* [Linode pricing](https://www.linode.com/pricing/)
* [Google Cloud pricing](https://cloud.google.com/products/calculator)


## Create Instance

1. Log in at [Lightsail](https://lightsail.aws.amazon.com/) + select **Create Instance**.
2. OS Only > Ubuntu 20.04 LTS
3. Generate new key = `C:\Users\wcd\.ssh\my-key.pem`
4. Enable Automatic Snapshots = True
5. Tier = $5 / 2 TB transfer or higher
6. Name = VPN (x 1)
7. Create Instance


## Add Temporary AWS IPv4 Rules

Once the instance is up, click to manage. On the Network tab, set these rules:

```bash
# IPv4
22/TCP      # SSH restrict to my non-WG IP + AWS shell
Ping (ICMP) # Restrict to my non-WG IP

# IPv4 + IPv6
80/HTTP     # Pi-hole web UI
53/TCP      # DNS
53/UDP      # DNS
51820/UDP   # PiVPN WireGuard

# IPv4 open temp for external testing
5335/UDP    # Unbound
```

## Set Static IPv4

1. Lightsail Dashboard > Networking tab > Create Static IP
2. Attach IP to instance.

