Edit ```/etc/sysctl.conf``` and and add the following lines at the end

```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

After that run:

```
cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```

If it reports ‘1′ means you have disabled IPV6. If it reports ‘0‘ then continue to follow the guide.  

Run command: 

```
sysctl -p
```

you will see this in terminal.

```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

Run the following again:
```
cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```
