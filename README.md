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
