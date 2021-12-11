# Minecraft Home Server

Notes and links for config of a home Minecraft server.

-----

### VM

Created VM in XenOrchastra.

```text
4x CPU
8gb
1x LAN
40gb HDD
```

Installed Ubuntu 20-LTS, picked up MAC address, reserved IP in DNS. Reboot, connected with PuTTY.

```bash
sudo apt update
sudo apt upgrade
```

Allow auto removal of unused kernals and dependencies, to personal taste.

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

Taking a snapshot here.

-----

Install git.

```bash
sudo apt install git build-essential
```

Java runtime.

```bash
sudo apt install openjdk-11-jre-headless
java -version
```

Create a minecraft user and setup folders.

```bash
sudo useradd -r -m -U -d /opt/minecraft -s /bin/bash minecraft
sudo su minecraft
cd ~
mkdir -p ~/{backups,tools,server}
```

Install mcrcon.

```bash
git clone https://github.com/Tiiffi/mcrcon.git ~/tools/mcrcon
cd ~/tools/mcrcon
gcc -std=gnu11 -pedantic -Wall -Wextra -O2 -s -o mcrcon mcrcon.c
./mcrcon -v
```

Taking a second snapshot here.

-----














