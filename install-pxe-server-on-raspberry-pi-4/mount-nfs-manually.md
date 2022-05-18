# Mount NFS manually

NFS Documentation [http://linux-nfs.org/wiki/index.php/Main\_Page](http://linux-nfs.org/wiki/index.php/Main\_Page)

## Errors

Let's review some errors that you can get on the client side and how to fix them.

### No such device

If you get "mount.nfs: No such device" error, then do the following test

```
$ uname -a
$ pacman -Q linux
```

If the outputs don't match, then you should reboot to apply the last kernel update

### Connection refused

If you get "connection refused" error while mounting NFS, then first check if you have `rpcbind` installed ([https://unix.stackexchange.com/a/64916/78773](https://unix.stackexchange.com/a/64916/78773))



### Tools for debugging

* nfstrace (\`sudo nfstrace -i eno1\`)
* nfswatch (\`sudo nfswatch -dev eno1\`)

### Troubleshooting

1. if rpc calls are making it to the server processes. you should see nfs and mountd

```
$ rpcinfo -p 192.168.3.13 | cut -c30- | sort -u
 mountd
 nfs
 nfs_acl
 nlockmgr
 portmapper
 service
```

check mountd

```
$ rpcinfo -u 192.168.3.13 mountd
program 100005 version 1 ready and waiting
program 100005 version 2 ready and waiting
program 100005 version 3 ready and waiting
```

This will eliminate firewall configuration problems.

2\. Then check that volumes are exported

```
$ showmount -e 192.168.3.13
Export list for 192.168.3.13:
/tftpboot 192.168.3.0/24
```

