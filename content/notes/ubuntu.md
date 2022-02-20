+++
title = "Ubuntu and Linux"
+++

These are general Linux and system notes from the perspective of an Ubuntu Server/Pop!_OS user.


## systemd
At its core, Ubuntu's are `systemd` distros.

<https://systemd.io/>

Curiously there's an anti-systemd website, though the concerns raised stem from 2015/2016, 8 or more years ago.

<https://nosystemd.org/>

#### Resources
* <https://www.linode.com/docs/guides/what-is-systemd/>
* <https://www.liquidweb.com/kb/what-is-systemctl-an-in-depth-overview/>


## hostnamectl
```bash
hostnamectl set-hostname MyComputer
```

## groups
```bash
# Create a group 'newgroup'
sudo groupadd newgroup

# Add 'user' to 'newgroup'
sudo usermod -aG newgroup user

# Reload changes for group (otherwise logout and log back in)
newgrp newgroup
```