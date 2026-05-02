---
title: Week7
---

Task 1: DHCP Client

- Here is the network that should be build-

<img src="media/image1.png" style="width:6.26806in;height:3.36875in"
alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

- After starting the openwrt1, in console its takes 2 mnst to reboot,
  after that I got this ip address onet 192.168.1.1/24

<img src="media/image2.tmp" style="width:6.26806in;height:3.36875in"
alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

- Then started the host1,and there were no ip add as we didn’t assigned
  any ip address, then we run command udhcpc to assign a dynamic ip
  address in host 1. ( udhcpc is way to assign a dynamic ip address to a
  host automatically). After that we can see the ip for host one and the
  server is 192.168.1.1 which is the openwrt1
  ip.<img src="media/image3.tmp" style="width:6.26806in;height:3.36875in"
  alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

- In host 2, we are going to configure ip in another way, we are using
  nano /etc/network/interfaces command to assign a DHCP ip address
  here<img src="media/image4.tmp" style="width:6.26806in;height:3.36875in"
  alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

- After that here is the ip we get from host 2

<img src="media/image5.tmp" style="width:6.26806in;height:3.36875in"
alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

- For host 3, in the same udhcpc way, assigned ip and captured the file
  and save the file by
  filezilla<img src="media/image6.png" style="width:6.26806in;height:3.36875in"
  alt="DHCP-client-12291447 and 5 more pages - Personal - Microsoft​ Edge" />

<img src="media/image7.tmp" style="width:6.26806in;height:4.75903in"
alt="gns3 - sftp://gns3@192.168.56.105 - FileZilla" />

# Task 2: DHCP Server Basics

- The configuration file for the DHCP (and DNS) server is called dhcp
  and can be viewed with: cat /etc/config/dhcp

- OpenWRT also includes a command called uci to view (show) values in a
  server configuration as well as change (set) values. You can use it to
  select certain parts of the configuration to show, e.g.: uci show
  dhcp.lan

- Usually a DHCP server maintains a list of current leases in a text
  file or database. On OpenWRT it is in the text file /etc/dhcp.leases.
  You can view it with: cat /etc/dhcp.leases

- You can also use uci to set values. There are multiple steps: 1. Set
  the value 2. Commit (save) the changes 3. Restart the server so the
  changes take effect.

> For example, to change the DCHP default lease time to 2 hours:
>
> uci set dhcp.lan.leasetime='2h'
>
> uci commit dhcp
>
> /etc/init.d/dnsmasq restart

- <img src="media/image8.png" style="width:6.26806in;height:6.63264in" />

# Task 3: Hosts File and Simple DNS Entries

- After having the ip for all, in host1, running command nano /etc/host
  to see the host information, we are putting host2 ip and fake domain
  name
  host2.example.com<img src="media/image9.png" style="width:6.26806in;height:4.24792in" />

- <img src="media/image10.png" style="width:6.26806in;height:4.97083in" />

- <img src="media/image11.png" style="width:6.26806in;height:3.36736in" />
