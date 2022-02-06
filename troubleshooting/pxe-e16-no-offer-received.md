# PXE-E16: No offer received

![](<../.gitbook/assets/2021-08-05 17-17-18.PNG>)

This probably means that you should double check your DHCP server configuration.

I found it helpful here to snif some packets on port 67 68

```
sudo tcpdump -i eth0 udp port 67 and port 68 -e -n -vv
```
