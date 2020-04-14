## Install mc

```
apt-get update && apt-get upgrade -y && apt-get install -y mc
```

## Create user

```
adduser fivem
usermod -aG sudo fivem
```

## Setup ssh

```
tee /etc/ssh/sshd_config <<EOF
Port 22022
HostKey /etc/ssh/ssh_host_ed25519_key
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
ChallengeResponseAuthentication no
#AllowUsers username1 username2 username3
#AllowUsers fivem

UsePAM no
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem sftp  /usr/lib/openssh/sftp-server
EOF
```

Setup firewall

```
sudo ufw status verbose
sudo ufw allow 22022/tcp
sudo ufw allow 443/tcp
sudo ufw allow 80/tcp
sudo ufw enable
```

Generate ssh key on the local machine:

```
ssh-keygen -o -t ed25519
```

Copy/paste public key from `id_file.pub` file to the server `~/.ssh/authorized_keys`.

Restart ssh server

```
sudo systemctl restart ssh
```
