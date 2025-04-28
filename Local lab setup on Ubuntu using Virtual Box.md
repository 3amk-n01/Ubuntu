### How to setup you local lab on Ubuntu using Virtual Box
 
This Setup is used for `Networks` that isolated from internet and accessible
with the guests  use same interface .

1. Setup `dhcp` server on that network for easy usage .
```bash

	VBoxManage dhcpserver add --network=lab1 --server-ip=192.168.5.1 --netmask=255.255.255.0 --lower-ip=192.168.5.2 --upper-ip=192.168.5.10 --enable

```

2. Add your guest to that network .
![[lab1.png]]

3. start the guest-1 [ kali for example ] and it should take ip automatically .
4. also do the same for other guests .