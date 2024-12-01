# 1 - Securing the server

### Purchasing a VPS

I got my vps off of OVH cloud. The server I got has a root user and a `debian` user, whose password I change as soon as I login.

### Installing basic utilities

As root: 

```bash
apt install zsh htop
```

### Creating personal user

```bash
adduser ishank
adduser ishank sudo
```

then exit and copy over public ssh key:

```bash
ssh-copy-id ishank@vps-xxxxxxxx.vps.ovh.us
```

### Deleting the `debian` user

```bash
userdel debian
rm -rf /home/debian
```

### Secure root

Change password

```bash
passwd root
```

Also change `/etc/ssh/sshd_config`

```
PermitRootLogin no
MaxSessions 2
```

```bash
sudo systemctl restart ssh
```
