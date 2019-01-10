# Plesk
## Configuring Passive FTP
Typically when using NATing within your solution, you will need to configure passive FTP differently to ensure it functions correctly.This guide explains the process. The guide also assumes you are running ProFTPd.

---

### Step 1.

Edit the `/etc/proftpd.conf` file and locate the `Global` section. Within the global section, you will need to explicitly set the passive FTP ports like the following:
  
  ```
  <Global>
    PassivePorts 40000 40100
  </Global>
  ```
  
 Do not remove the existing content from the global section.
 
### Step 2.

Restart the FTP service using the following command. The specific command you will run repends on your Operating System. If you are unsure as to which to use, then default to the CentOS 6 example.

#### CentOS 6
```
service xinetd restart
```

#### CentOS 7
```
systemctl restart xinetd
```

### Step 3.

Ensure that the passive ports you have configured on your firewall. If you have a hardware firewall / security groups then you will likely need to configure these within your hosting provider's control panel. If you are using a software firewall such as `iptables` then the following command may be useful.
```
iptables -A INPUT -p tcp --match multiport --dports 40000:40100 -j ACCEPT 
```

---

If you continue to have issues with this issue, then I would suggest contacting your hosting provider to ensure that it is configured correctly.
