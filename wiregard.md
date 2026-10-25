Instal Wiregard
``` bash
sudo apt update
sudo apt install wireguard qrencode
```

Generate keys
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

sudo chmod 600 /etc/wireguard/wg0.conf

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

Generate keys for my phone
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
```
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



Source:
- https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-debian-11


