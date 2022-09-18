# Windows: PuTTY

A WireGuard client is a computer or mobile device that will use the VPN. When a client is added, a config file is created. 

For a mobile device, WireGuard can display the config as a QR code for scanning. I'm using PowerShell 7 which does not display the codes correctly. 

Alternatively, the QR code can be saved as a PNG. But, because this is an Ubuntu server with no desktop, there is no baked-in way to display PNG. My easiest option is to copy the PNGs to my PC where I can easily display them - hence PuTTY. 

For a computer client, you can display the config in Linux. From here you can copy-paste to a local file on your computer. But since I already need PuTTY, I'll use it for the config files as well. 


## Install PuTTY

1. Go here: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. Install the full package.


## Convert .pem to .ppk

When you create a Lightsail instance, the SSH key you download is a .pem file. To use with PuTTY, convert this file to .ppk using PuTTYgen. 

1. Launch the PuTTYgen app.
2. Click the Load button and select your `my-key.pem` file.
3. At the bottom, under Parameters:
   1. Type of Key to Generate = RSA
   2. Number of Bits = 2048
4. Click Save Private Key
   1. Save to `C:\Users\user\.ssh` 
   2. Save as as `my-key.ppk`. 


## Copy Files from Linux to Windows

The command to copy a file from Linux to Windows looks like this.

```bash
# Change permission of configs dir
# Mine was root - needed to be ubuntu (user)
sudo chown -R ubuntu:ubuntu /home/ubuntu/configs

# In a separate PowerShell not SSHed to Ubuntu
pscp -i C:\Users\user\.ssh\aws-vpn-root.ppk ubuntu@site.com:/home/ubuntu/configs/my-client.png D:\WireGuard
```

## Connect Using PuTTY

Instead of PowerShell, you can use PuTTY.

1. Launch the PuTTY app.
2. Session 
   1. Hostname = site.com
   2. Port = 22
   3. Connection Type = SSH
3. Connection 
   1. Enable TCP keepalives = True
4. Connection > SSH > Auth
   1. Browse to `my-key.ppk`
5. **Important** Session
   1. Enter a session name.
   2. Click Save Session.
6. Click Open.
7. Accept the warning.
8. Login as = ubuntu (username on server)


## References

* [AWS: Connect to your Linux instance from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

