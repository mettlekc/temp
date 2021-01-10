# Ubuntu FTP 서버 설정

## vsftpd

- Very Secure FTP Server Daemon
- GPLv2 License

## Install

```bash
apt update
apt search vsftpd
apt install vsftpd
```

```bash
systemctl start vsftpd.service
```

## Settings

```bash
sudo gedit /etc/vsftpd.conf
```

```bash
listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
pasv_enable=Yes
pasv_min_port=10000
pasv_max_port=11000
user_sub_token=$USER
local_root=/home/$USER/ftp

userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
```

```bash
systemctl stop vsftpd.service
systemctl enable vsftpd.service
systemctl status vsftpd.service
```