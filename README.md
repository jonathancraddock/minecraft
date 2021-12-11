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

!!!! ***DO NOT USE THIS VERSION*** !!!!

> Should have used used v17, with `sudo apt install openjdk-17-jre-headless`. Additional notes below.

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

Minecraft Server -> https://www.minecraft.net/en-us/download/server

Download.

```bash
wget https://launcher.mojang.com/v1/objects/125e5adf40c659fd3bce3e66e67a16bb49ecc1b9/server.jar -P ~/server
cd ~/server
java -Xmx1024M -Xms1024M -jar server.jar nogui
```

> At this point, appears Java runtime version is not correct... should have used v17. :-/

"Exit" from minecraft user, and remove incorrect version of Java and reinstall.

```bash
exit
whoami

sudo apt remove --auto-remove openjdk*
sudo apt purge openjdk*

sudo apt install openjdk-17-jre-headless
java -version
```

Switch back to minecraft user account.

```bash
sudo su minecraft
cd ~/server
java -Xms1024M -Xmx1024M -jar server.jar nogui
```

Note the EULA warning message.

```bash
nano ~/server/eula.txt
```

Set the value `eula=true`.

Edit the properties.

```bash
nano ~/server/server.properties
```

Set an rcon password, and adjust any other parameters as needed. (Consider firewall, if you want access from outside your LAN.)

```bash
rcon.port=25575
rcon.password=XXXXXXXXXXXX
enable-rcon=true
```

"Exit" back to admin account, and config systemd.

```bash
sudo nano /etc/systemd/system/minecraft.service
```

> If you wanted to adjust the defauts, you can query your min/max heap size as follows:
> `java -XX:+PrintCommandLineFlags -version`

Suggested config, and use mcrcon password that was set earlier.

```bash
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=minecraft
Nice=1
KillMode=none
SuccessExitStatus=0 1
ProtectHome=true
ProtectSystem=full
PrivateDevices=true
NoNewPrivileges=true
WorkingDirectory=/opt/minecraft/server
ExecStart=/usr/bin/java -Xmx1024M -Xms1024M -jar server.jar nogui
ExecStop=/opt/minecraft/tools/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p XXXXXXXXXXXX stop

[Install]
WantedBy=multi-user.target
```

Reload and start the service.

```bash
sudo systemctl daemon-reload
sudo systemctl start minecraft

sudo systemctl status minecraft
```
^- *first run takes a while... in this case, 83 seconds!*

Enable service.

```bash
sudo systemctl enable minecraft
```

Server is working and can connect OK from my PC. Taking another snapshot here.

-----

Connect to console:

```bash
/opt/minecraft/tools/mcrcon/mcrcon -H 192.168.1.XXX -P 25575 -p XXXXXXXXXXXX -t
```

-----

### Additional Reference

If you don't want fire spread: `/gamerule doFireTick false`

For commands and syntax help -> https://minecraft.fandom.com/wiki/Commands#Command_guide













