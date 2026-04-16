TCP/IP Protocols and Principles

Task 1: Setting Static IP Addresses
Aim
Three different approaches to set static IP address on a Linux host.
Activities
1.	Create project named Setting-IP-<studentid>.

<img width="940" height="583" alt="image" src="https://github.com/user-attachments/assets/7c96f877-7ad4-4e27-bd4c-d87fdcca3751" />


 
 
2.	Add four (4) nodes of type Linux Host and one Ethernet switch, and connect together into a LAN

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/3be66695-84d6-42fd-9228-f366e0167da9" />

 



3. Select an IPv4 network address (e.g., 10.1.1.0/24) to use in your LAN. Your teacher may advise you to use a specific network address.
4. For two of the hosts, use the GNS3 Configure menu item to set static IP addresses on the nodes. Do not set the IP addresses of the other two hosts.

 
 
<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/a1e6fe28-0769-4a23-8477-91ea537c64df" />

<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/97571093-50a5-4db7-81cb-45863e91703c" />

5. Start all nodes.

 <img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/8b45fca5-8ac7-4339-a9f2-a3f4fd80a70e" />


6. On the 3rd host (a host that you did not configure with an IP address), open a console and edit the /etc/network/interfaces file to set a static IP address. Restart networking so the change takes effect.

 <img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/b3fadd4c-813b-43a1-9da9-96db2ce002ba" />


7. On the 4th host, open a console and use ip address add command to set a static IP address.

 <img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/ef76feac-c690-4753-9ed2-8753bc540980" />

8. Use ip address show command to view and check the IP addresses of all four hosts.

Task 2: Testing Network Connectivity and Delay with Ping
Aim
Learn the basics of ping to test if a device is reachable and to measure delay
Activities
1. Use the Setting-IP-<studentid> project which contains four Linux hosts and one Ethernet switch. Ensure all hosts have IP addresses. We will refer to the hosts as host A, host B, host C and host D.
2. Ping from host A to host B using no command line options. Stop the ping after at least five (5) response messages are received.

 <img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/53885395-3400-47af-bc02-3739813170ad" />


3.	View and understand the output of ping, especially the average RTT.

The source device learns the delay to get a message to the destination, and to receive a response. That is, to get there and back, which is called the round-trip time (RTT).
ping -c 5 192.168.56.20
•	ping → checks if another device is reachable 
•	-c 5 → send 5 packets only 
•	192.168.56.20 → target IP 
________________________________________
🔹 What each line means
Example line:
64 bytes from 192.168.56.20: icmp_seq=1 ttl=64 time=0.953 ms
✔️ 64 bytes
Size of the packet sent back
✔️ icmp_seq
Sequence number (packet number)
•	1 → first packet 
•	2 → second packet 
•	up to 5 
👉 This shows all packets were received in order
✔️ ttl=64
TTL = Time To Live
•	It decreases each time packet passes a router 
•	64 usually means it's very close / same network 
✔️ time=0.953 ms
Time taken for round trip (very fast = good connection)
________________________________________
🔹 What is ICMP?
ICMP = Internet Control Message Protocol
👉 It’s the protocol used by ping to send/receive messages
👉 Basically: “Are you alive?” → “Yes, I’m here!”
________________________________________
🔹 Final Summary (IMPORTANT PART)
5 packets transmitted, 5 received, 0% packet loss
✔️ Sent: 5
✔️ Received: 5
✔️ Loss: 0% → PERFECT connection
________________________________________
🔹 Latency stats
rtt min/avg/max/mdev = 0.417 / 0.638 / 0.953 / 0.179 ms
•	min → fastest response 
•	avg → average response 
•	max → slowest response 
•	mdev → variation (stability) 
👉 Your connection is:
•	Very fast (<1 ms) 
•	Very stable


4.	Ping from one host to an IP address that does not exist in your network. Wait at least 10 seconds before stopping the ping. View and understand the output, especially the packet loss.
<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/381948c4-d231-4d43-b000-4fd457936507" />

 
5. Try ping from different hosts with different command line options, e.g., changing the count, interval and size. Understand how and why the options impact the operation and results of ping. 
 
<img width="940" height="552" alt="image" src="https://github.com/user-attachments/assets/97d22e09-f781-4d31-b476-467d6a8be1a7" />


