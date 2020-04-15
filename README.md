## Install mc

```
apt-get update && apt-get upgrade -y && apt-get install -y mc && apt-get install -y git
```

## Create user, setup ssh

```
sudo adduser fivem
usermod -aG sudo fivem
su fivem
mkdir /home/fivem/.ssh 0600
```

Generate ssh key on the **local machine**:

```
ssh-keygen -o -t ed25519 -f id_hh
```

Copy public key from `id_hh.pub` file on the **local** machine

And

Paste it to `/home/fivem/.ssh/authorized_keys` file on the **server**

```
nano /home/fivem/.ssh/authorized_keys
```

### Setup ssh

```
sudo tee /etc/ssh/sshd_config <<EOF
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

Restart ssh server

```
sudo systemctl restart ssh
```

Setup firewall

```
sudo ufw status verbose
sudo ufw allow 22022/tcp
sudo ufw allow 443/tcp
sudo ufw allow 80/tcp
sudo ufw enable
```
