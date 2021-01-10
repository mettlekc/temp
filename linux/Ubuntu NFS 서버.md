# Ubuntu NFS

## Server

```bash
apt-get install nfs-common nfs-kernel-server rpcbind portmap
```

```bash
mkdir /mnt/data
chmod -R 777 /mnt/data
```

/etc/exports

```text
/mnt/data *(rw,async,no_subtree_check)
/mnt/.../test 10.0.0.1/16(rw,async,no_subtree_check)
```

- rw : read and write operation
- sync : write any chage to the disc before applying it
- no_subtree_check : prevent subtree checking 

```bash
exportfs -a
systemctl restart nfs-kernel-server
```

## Client

```bash
apt-get install nfs-common
```

```bash
mkdir /public_data
```

```bash
mount 10.0.0.2:/mnt/data /public_data
```