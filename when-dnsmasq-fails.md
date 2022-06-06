# When dnsmasq fails

When I have **dnsmasq** installed I many times saw this error message:

```
dnsmasq: failed to bind DHCP server socket: Address already in use
```

Which obviously means that DHCP server is already running, so look for ISC DHCP server running:

```
sudo systemctl status isc-dhcp-server.service
```
