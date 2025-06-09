---
date: 2025-06-09T11:57:32+02:00
# description: ""
# image: ""
lastmod: 2025-06-09
showTableOfContents: false
# tags: ["",]
title: "Raspberry Pi"
type: "post"
---

Basic Raspberry Pi set  up.

1. SSH into the Raspberry Pi. 

```bash
ssh username@remote_host
```

Then update all packages.

```bash
sudo apt update && sudo apt upgrade -y
```

2. Copy my public key to a server with `ssh-copy-id`.

```bash
ssh-copy-id username@remote_host
```

3. Disable password authentication in the Raspberry Pi. Edit `/etc/ssh/sshd_config` as root.

```
PasswordAuthentication no
```

4.  Restart the SSH service

```bash
sudo service ssh restart
```

5. Enable automatic security upgrades with `unattended-upgrades`

```bash
sudo apt install unattended-upgrades
```

6. Turn on the service

```bash
sudo systemctl start unattended-upgrades
sudo systemctl enable unattended-upgrades
```

7. Automatically mount a drive for user on boot.

Find the  drive.

```bash
lsblk
```

Get the drive UUID.

```bash
blkid
```

Edit `/etc/fstab`.

```
UUID=<uuid> <mount_point> auto defaults,uid=1000,gid=1000,umask=077,nofail 0 1
```

Reload the systemd daemon (optional) 

```bash
sudo systemctl daemon-reload
```

# References

- https://manpages.debian.org/experimental/unattended-upgrades/unattended-upgrades.8
- https://unix.stackexchange.com/questions/204641/automatically-mount-a-drive-using-etc-fstab-and-limiting-access-to-all-users-o

