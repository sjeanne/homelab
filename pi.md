# Installation

Use Raspberry Pi Imager: https://www.raspberrypi.com/documentation/computers/getting-started.html#imager-install

Selected options:

- Device: Raspberry Pi 3
- OS: Raspberry Pi OS Other > Raspberry Pi OS Headless (64-bit)
- Storage: 128 GB SD card
- User: `pi`
- Region/time zone: Paris, FR
- Remote access: enable SSH with a public key

# SSH connection

```bash
ssh -i <private-key-path> <username>@<ip-address>
```

# Update the system

## Update firmware

```bash
sudo rpi-update
```

## Update the OS

Update the package list:

```bash
sudo apt update
```

Update installed software:

```bash
sudo apt full-upgrade
```

# Install tools

```bash
sudo apt install vim \
    git \
    fastfetch
```

```bash
sudo apt install zsh
chsh -s /usr/bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo apt install fonts-powerline -y
```


