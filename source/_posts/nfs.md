---
title: nfs
date: 2020-01-08 21:52:32
categories:
tags:
---
Network File System (NFS) is a popular distributed filesystem protocol that enables users to mount remote directories on their server. It does not require client to have big disk space to store all files. NFS lets you leverage storage space in a different location and allows you to write onto the same space from multiple servers or clients in an effortless manner. Following commands are used to config CentOS 7.6 server.

<!--more-->
# At NFS Server End

## Install Required Packages
```bash
yum install nfs-utils
mkdir /var/nfsshare
chmod -R 755 /var/nfsshare
chown nfsnobody:nfsnobody /var/nfsshare
```

## Start the Services and Enable Boot Time Running

```bash
systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
```

## Config File

```bash
vim /etc/exports
```
write the following to make a sharing point into the file.

**`/var/nfsshare 192.168.0.101(rw,sync,no_root_squash,no_all_squash)`**

and then restart
```bash
systemctl restart nfs-server
```


## Add the NFS service override in CentOS 7 firewall-cmd public zone service

```bash
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload
```

# NFS Client End
```
yum install nfs-utils
mkdir -p /mnt/nfs/var/nfsshare
mount -t nfs 192.168.0.100:/var/nfsshare /mnt/nfs/var/nfsshare/
```
```
[root@client1 ~]# df -kh
Filesystem                    Size  Used Avail Use% Mounted on
/dev/mapper/centos-root        39G  1.1G   38G   3% /
devtmpfs                      488M     0  488M   0% /dev
tmpfs                         494M     0  494M   0% /dev/shm
tmpfs                         494M  6.7M  487M   2% /run
tmpfs                         494M     0  494M   0% /sys/fs/cgroup
/dev/mapper/centos-home        19G   33M   19G   1% /home
/dev/sda1                     497M  126M  372M  26% /boot
192.168.0.100:/var/nfsshare   39G  980M   38G   3% /mnt/nfs/var/nfsshare
192.168.0.100:/home           19G   33M   19G   1% /mnt/nfs/home
```

# Permanent NFS mounting

```
vim /etc/fstab
```

```
[...]
192.168.0.100:/var/nfsshare    /mnt/nfs/var/nfsshare   nfs defaults 0 0
```