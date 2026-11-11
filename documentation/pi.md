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

## ZSH
```bash
sudo apt install zsh
chsh -s /usr/bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo apt install fonts-powerline -y
```

## Install docker
Do not install Docker with Debian official package. Follow Docker official documentation: https://docs.docker.com/engine/install/debian/

``` bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

``` bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl status docker
``` 

Add user into Docker's usergroup
``` bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

Docker stats have access to memory used by containers. Monitoring apps, such Beszel won't be able to monitor docker's containers.
Enable cgroups memory accounting.
``` bash
sudo vi /boot/firmware/cmdline.txt
````

Add at <u>**the end of line**</u>:
```
cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

Reboot the Pi

Reference: https://akashrajpurohit.com/blog/resolving-missing-memory-stats-in-docker-stats-on-raspberry-pi/

## Collection of tools

| Tools | What it brings | How to install |
|---|---|---|
| git | Version control | `apt install git` |
| FastFetch | Display information about the host | `apt install fastfetch` |
| LazyDocker | TUI to manager docker containers | `curl -fsSL https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash` |
| LazyGit | Git TUI | `apt install lazygit` |
