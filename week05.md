                               Week 5
Task1:

In this task we are going to learn about VLANS . it is an isolated network to work between some chosen host. I mean , among all  the hosts in the network , if we want to isolate only 2 or 3 or 4 host to able to communicate each other we make these host to a specific port number in the switch and this is VLANS. 

•	Here is the network with 4 host and one open switch.
 <img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/6b7104d4-57bf-48b5-ac5a-dab8e319e5d5" />



•	Next, we configure all the ip address of these host. After that I needed to run a command ( sh /etc/openvswitch/init.sh) in the switch to make it work to connect one host to another. After that pinging was successful. 
(By default in GNS3, an OpenvSwitch has 16 ports (eth0, eth0, …, eth15) on a bridge br0. To view the port information: ovs-vsctl show )
(Add VLANs on Access (Untagged) Ports for Host-to-Switch 
When a host connects to a switch, the switch port is called an access port or untagged port. For each access port you should assign a VLAN ID or tag. For example, to connect the devices on access ports eth1 and eth2` to VLAN ID 10, and the devices on access portseth3andeth4`` on VLAN ID 20)


•	So in the switch, we need to make 2 port to make VLANS. For that, host 1,2 will be in one port and host 3,4 will be in another port. For this the following command will be 
Ovs-vsctl set port eth1 tag=447 
Ovs-vsctl set port eth2 tag=447 

Ovs-vsctl set port eth3 tag=448
Ovs-vsctl set port eth4 tag=448 

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/ac633672-8a1c-4c80-b298-871c3363f2df" />

 

•	Now we can see the information about host and port in the switch 
 

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/4a6ec5e4-1f57-45a6-9ceb-85046854108c" />




•	After porting in the switch, if we want the ping host1 to host 3,it will be unreachable because host are in different port/lan. 
 
<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/cbb7c529-a025-43e1-ade5-23d80e0c9f3e" />

Task2:

•	Here is the new network with one linux router here.
 

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/922e77bc-dba3-46a7-bc06-c674d603ea83" />


•	Now we need to connect switch to router and those port as well. Fo this we gonna run some command here.



   (While there is a single physical link from the switch to router, connecting to a single physical interface/port on the router, with VLANs the router needs to treat each VLAN as a different IP subnet. Therefore the router needs a different IP address for each VLAN. We can achieve this using sub-interfaces in Linux. If your router is using interface eth0, we will create a sub-interface called eth0.10 for VLAN ID 10, and similar for other VLAN IDs. This is performed with the ip link command: 
ip link add link eth0 name eth0.10 type vlan id 10 
You can repeat for the other VLAN, e.g., eth0.20 for VLAN ID 20. 
Now IP addresses can be assigned to the sub-interfaces (just the same as any interface), e.g., 
ip address add 1.2.3.4/24 dev eth0.10 )


<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/af8f4c4c-5847-4d84-a92f-be92792d7267" />

 




