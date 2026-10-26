# Setup Wireguard peer on the Pi

Instal Wiregard
``` bash
sudo apt update
sudo apt install wireguard qrencode
```

Generate Pi's public/private keys
``` bash
sudo install -m 700 -d /etc/wireguard
sudo sh -c 'umask 077; wg genkey | tee /etc/wireguard/server.key | wg pubkey > /etc/wireguard/server.pub'
```

Create Wireguard server configuration
``` bash
sudo cat /etc/wireguard/server.key
sudo vim /etc/wireguard/wg0.conf
``` 

Paste the following content with the private key value:
``` text
[Interface]
Address = 10.10.10.1/24
ListenPort = 51820
PrivateKey = YOUR_SERVER_PRIVATE_KEY
SaveConfig = true
```

Adjust Wireguard congiguration rights to protect it access:
``` bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

Start Wireguard:
``` bash
sudo systemctl enable --now wg-quick@wg0
```

Checks Wireguard is running:
``` bash
sudo systemctl status wg-quick@wg0
sudo wg
ip addr show wg0
sudo ss -lunp | grep 51820
```

# Add a new peer: my phone

Generate public/private/pre-shared keys for my phone
``` bash
sudo -i
cd /etc/wireguard
umask 077

wg genkey | tee phone.key | wg pubkey > phone.pub
wg genpsk > phone.psk
```

Exit from root user
``` bash
exit
```

Add the phone into the configuration
``` bash
sudo vim /etc/wireguard/wg0.conf
```

and add this configuration
``` text
[Peer]
# Phone
PublicKey = YOUR_PHONE_PUBLIC_KEY
PresharedKey = YOUR_PHONE_PRESHARED_KEY
AllowedIPs = 10.10.10.2/32
```

Restart Wireguard and check
``` bash
sudo systemctl restart wg-quick@wg0
sudo wg
```

Add the port forwarding rule on your router
```
Protocol:       UDP
External port:  51820
Internal IP:    192.168.1.2
Internal port:  51820
```

# Configure the phone

Create the phone Configuration: phone.conf
``` bash
sudo vim /etc/wireguard/phone.conf
```

Add the following content into phone.conf
``` text
[Interface]
PrivateKey = YOUR_PHONE_PRIVATE_KEY
Address = 10.10.10.2/32
DNS = 10.10.10.1

[Peer]
PublicKey = YOUR_SERVER_PUBLIC_KEY
PresharedKey = YOUR_PHONE_PRESHARED_KEY
Endpoint = YOUR_PUBLIC_IP:51820
AllowedIPs = 10.10.10.0/24
PersistentKeepalive = 25
```

# Import configuration on the phone

Generate a QR code with the wireguard phone's configuration
``` bash
sudo apt install qrencode
sudo sh -c 'qrencode -t ansiutf8 < /etc/wireguard/phone.conf'
```

Install official Wireguard application on the phone.
Add a VPN by scanning the QR Code






Source:
- https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-debian-11


