Week4 tutorial

Task1 :	

•	Here is 2 network using 3 linux host and 1 router to connect them and one switch to connect host 1 and host2 together.
   <img width="940" height="522" alt="image" src="https://github.com/user-attachments/assets/10a8c3cf-2959-4f3a-ba35-2eb2b5eb6d1f" />



•	Using the command ip route show, we got the routing table. Here is the table accordingly
Host1
source	destination	Next route	interface
10.10.1.102	10.10.1.0/24	direct	Eth0
	any	10.10.1.1	Eth0
			

Host2
source	destination	Next route	interface
10.10.1.101	10.10.1.0/24	direct	Eth0
	any	10.10.1.1	
			

Host3
source	destination	Next route	interface
			
			
			

Router
source	destination	Next route	interface
	10.10.1.0/24	direct	Eth0
	10.10.2.0/24	direct	Eth1
			



•	Here is 3 screenshots of  pinging with one net to another network. 
  
<img width="819" height="356" alt="image" src="https://github.com/user-attachments/assets/be748137-1ba7-4d85-b3f6-d3135838ab10" />

<img width="824" height="329" alt="image" src="https://github.com/user-attachments/assets/5275996b-9ec7-4b3e-aca0-9be112bcb8b6" />


 <img width="761" height="421" alt="image" src="https://github.com/user-attachments/assets/356ad4f0-3ef9-4e44-88d4-a5e20de08440" />













Task 2

•	Here is the ospf network.

<img width="641" height="365" alt="image" src="https://github.com/user-attachments/assets/e2780100-317e-4cbf-a5cd-ca7ed7282f65" />

 

•	Here is the ip of neighbor router of FRR1 using the command show ip ospf neighbor. 
•	Here is also the routing table of 2 routers using the command show ip ospf route

<img width="940" height="482" alt="image" src="https://github.com/user-attachments/assets/fce3e99f-54d5-434b-81c6-0f3277ab972c" />

 

•	Using the traceroute command to another host of the other network, here is the paths I can see the way packet goes from one host1 to host 2

 
<img width="940" height="518" alt="image" src="https://github.com/user-attachments/assets/95f10561-8dae-437d-917d-22ec596e9c1a" />
