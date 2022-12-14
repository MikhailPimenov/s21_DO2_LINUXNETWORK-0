## Part 1. ipcalc tool

- there is no ipcalc tool.

![text](../screenshots/part1/_1.png)

- installing ipcalc. $ sudo apt install ipcalc

![text](../screenshots/part1/_2.png)

### 1.1 Networks and Masks

1. Network address of 192.167.38.54/13

![text](../screenshots/part1/1_1_1.png)


2. Conversion of the mask 255.255.255.0 to prefix and binary, /15 to normal and binary, 11111111.11111111.11111111.11110000 to normal and prefix

- сonversion of the mask 255.255.255.0 to prefix and binary
![text](../screenshots/part1/1_1_2-1.png)

- /15 to normal and binary
![text](../screenshots/part1/1_1_2-2.png)

- 11111111.11111111.11111111.11110000 to normal and prefix
![text](../screenshots/part1/1_1_2-3.png)


3. Minimum and maximum host in 12.167.38.4 network with masks: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 and /4

- /8
![text](../screenshots/part1/1_1_3-1.png)

- 11111111.11111111.00000000.00000000
![text](../screenshots/part1/1_1_3-2.png)

- 255.255.254.0
![text](../screenshots/part1/1_1_3-3.png)

- /4
![text](../screenshots/part1/1_1_3-4.png)

### 1.2 Localhst
- can be accessed with
    + 127.0.0.2
    + 127.1.0.1
- can't with
    + 194.34.23.100
    + 128.0.0.1


### 1.3. Network ranges and segments

1. Which of the listed IPs can be used as public and which only as private: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1

- private
    + 10.0.0.45
    + 192.168.4.2
    + 172.20.250.4
    + 172.16.255.255
    + 10.10.10.10

- public
    + 134.43.0.2
    + 172.0.2.1
    + 192.172.0.1
    + 172.68.0.2
    + 192.169.168.1


2. Which of the listed gateway IP addresses are possible for 10.10.0.0/18 network: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255

- possible
    + 10.10.0.2
    + 10.10.10.10
    + 10.10.1.255

- impossible
    + 10.0.0.1
    + 10.10.100.1


## Part 2. Static routing between two machines

- Start two virtual machines (hereafter -- ws1 and ws2)
![text](../screenshots/part2/_1.png)


- View existing network interfaces with the ip a command 
    - Add a screenshot with the call and output of the used command to the report.
![text](../screenshots/part2/_2.png)


- Describe the network interface corresponding to the internal network on both machines and set the following addresses and masks: ws1 - 192.168.100.10, mask */16 *, ws2 - 172.24.116.8, mask /12
    - Add screenshots of the changed etc/netplan/00-installer-config.yaml file for each machine to the report.
    - sudo nano /etc/netplan/00-installer-config.yaml
![text](../screenshots/part2/_3.png)

    - sudo netplan apply
![text](../screenshots/part2/_4.png)

    - after network applied
![text](../screenshots/part2/_5.png)

### 2.1. Adding a static route manually

- Add a static route from one machine to another and back using a 'ip r add' command.
![text](../screenshots/part2/1_1.png)


- Ping the connection between the machines. Add a screenshot with the call and output of the used commands to the report.
![text](../screenshots/part2/1_2.png)


### 2.2. Adding a static route with saving

- Restart the machines
![text](../screenshots/part2/2_1.png)


- Add static route from one machine to another using etc/netplan/00-installer-config.yaml file. Add screenshots of the changed etc/netplan/00-installer-config.yaml
file to the report.

![text](../screenshots/part2/2_2.png)

- Ping the connection between the machines. Add a screenshot with the call and output of the used commands to the report.
![text](../screenshots/part2/2_3.png)


## Part 3. iperf3 utility

- In this task you need to use ws1 and ws2 from Part 2.

- No iperf3, installing it.
![text](../screenshots/part3/_1.png)
There is no internet connection. So to download iperf3 these steps are needed to be done:
   + Turn on the second interface to be able to set up the Internet.
   + Turn on DHCP for the second interface in the netplan. sudo nano /etc/netplan/00-interface-config.yaml
![text](../screenshots/part3/_2.png)
   + Netplan apply.
   + Ping an address to be sure that it works
![text](../screenshots/part3/_3.png)

- Successfull installation
![text](../screenshots/part3/_4.png)

- Also installed on ws2

### 3.1. Connection speed

- Convert and write results in the report: 8 Mbps to MB/s, 100 MB/s to Kbps, 1 Gbps to Mbps

    + 8Mbps -> 1MB/s
    + 100MB/s -> 819200Kbps
    + 1Gbps -> 1024Mbps


### 3.2. iperf3 utility

- Measure connection speed between ws1 and ws2. Add a screenshots with the call and output of the used commands to the report.

   + $ sudo apt-get install iperf3
   + On the server $ iperf3 -s
   + On the client $ iperf3 -c 172.24.116.8 -p 5201
![text](../screenshots/part3/2_1.png)


## Part 4. Network firewall


### 4.1. iptables utility

- Create a /etc/firewall.sh file simulating the firewall on ws1 and ws2 - creating /etc/firewall.sh with command 'sudo nano /etc/firewall.sh' and entering the 'flush' and 'delete chain' commands:

![text](../screenshots/part4/1_1.png)

The following rules should be added to the file in a row:

+ 1 - on ws1 apply a strategy where a deny rule is written at the beginning and an allow rule is written at the end (this applies to points 4 and 5)
+ 2 - on ws2 apply a strategy where an allow rule is written at the beginning and a deny rule is written at the end (this applies to points 4 and 5)
+ 3 - open access on machines for port 22 (ssh) and port 80 (http)
+ 4 - reject echo reply (machine must not ping, i.e. there must be a lock on OUTPUT)
+ 5 - allow echo reply (machine must be pinged)

Add screenshots of the /etc/firewall file for each machine to the report.


- entering the iptable commands for ws1 and ws2 according to the instructions:

![text](../screenshots/part4/1_2.png)


- run the files on both machines with chmod +x /etc/firewall.sh and /etc/firewall.sh commands 
    + sudo chmod +x /etc/firewall.sh
    + sudo /etc/firewall.sh
    + ping -c 5 8.8.8.8 to add some data transmission
    + listing the created rules: sudo iptables -L


Add screenshots of both files running to the report.
Describe in the report the difference between the strategies used in the first and second files.

![text](../screenshots/part4/1_3.png)
![text](../screenshots/part4/1_4.png)

- flags explained:
![text](../screenshots/part4/1_5.png)

- difference between strategies - first suitable rule met is going to be executed. Rules that are applied after are going to be ignored.


### 4.2. nmap utility:


- Use ping command to find a machine which is not pinged, then use nmap utility to show that the machine host is up
Check: nmap output should say: Host is up.

- installing nmap - sudo apt-get install nmap

- contents of firewall.sh

![text](../screenshots/part4/2_1.png)


- Add screenshots with the call and output of the ping and nmap commands to the report - ping and nmap:

![text](../screenshots/part4/2_2.png)


- Save dumps of the virtual machine images
p.s. Do not upload dumps to git under any circumstances!

   + export to files
![text](../screenshots/part4/2_3.png)
   + snapshots
![text](../screenshots/part4/2_4.png)
![text](../screenshots/part4/2_5.png)


## Part 5. Static network routing:

- Start five virtual machines (3 workstations (ws11, ws21, ws22) and 2 routers (r1, r2)):

- switching hostname for each machine
   + sudo hostnamectl set-hostname \<new_hostname\>
![text](../screenshots/part5/_1.png)
   + after sudo reboot
![text](../screenshots/part5/_2.png)
   + all five machines
![text](../screenshots/part5/_3.png)


### 5.1. Configuration of machine addresses

- Set up the machine configurations in etc/netplan/00-installer-config.yaml according to the network in the picture.

![text](../screenshots/part5/1_1.png)

![text](../screenshots/part5/1_2-0.png)


- Add screenshots of the etc/netplan/00-installer-config.yaml file for each machine to the report.

![text](../screenshots/part5/1_2.png)


- Restart the network service. If there are no errors, check that the machine address is correct with the ip -4 a command. Also ping ws22 from ws21. Similarly ping r1 from ws11.

- Add screenshots with the call and output of the used commands to the report.

   + restarting
![text](../screenshots/part5/1_3.png)
   + checking with 'ip -4 a' command and pinging ws22 from ws21 and r1 from ws11
![text](../screenshots/part5/1_4.png)



### 5.2. Enabling IP forwarding

- To enable IP forwarding, run the following command on the routers: sysctl -w net.ipv4.ip_forward=1.

- With this approach, the forwarding will not work after the system is rebooted.

- Add a screenshot with the call and output of the used command to the report

![text](../screenshots/part5/2_1.png)
![text](../screenshots/part5/2_2.png)


### 5.3. Default route configuration


- Configure the default route (gateway) for the workstations. To do this, add default before the router's IP in the configuration file

- Add a screenshot of the etc/netplan/00-installer-config.yaml file to the report.

![text](../screenshots/part5/3_1.png)

- Call ip r and show that a route is added to the routing table

- Add a screenshot with the call and output of the used command to the report.

![text](../screenshots/part5/3_2.png)


- Ping r2 router from ws11 and show on r2 that the ping is reaching. To do this, use the 'tcpdump -tn -i eth1' command.

Add screenshots with the call and output of the used commands to the report.

![text](../screenshots/part5/3_3.png)


## 5.4. Adding static routes

 - Add static routes to r1 and r2 in configuration file

- Add screenshots of the changed etc/netplan/00-installer-config.yaml file for each router to the report

![text](../screenshots/part5/4_2.png)


- Call ip r and show route tables on both routers

![text](../screenshots/part5/4_3.png)


- Run ip r list 10.10.0.0/[netmask] and ip r list 0.0.0.0/0 commands on ws11

![text](../screenshots/part5/4_4.png)


- Explain in the report why a different route other than 0.0.0.0/0 had been selected for 10.10.0.0/[netmask] although it could be the default route:
First IP and mask suits the route that is set in netplan (10.10.0.0/18) and the other one doesn't (one of the things that doesn't fit under the rule - 0.0.0.0/0 is out of the set mask), so it follows the default route set.


### 5.5. Making a router list


- Traceroute utility output after adding a gateway and $ tcpdump -tnv -i eth0

![text](../screenshots/part5/5_1.png)

![text](../screenshots/part5/5_2.png)

- Based on the output of the dump on r1, explain in the report how path construction works using traceroute:
First the data goes to the eth0 interface of the r1 router, then it gets routed to the second router using eth0 interface of r2 and finally gets routed to destination 10.20.0.10.

_<h2 align="left"> 5.6. Using ICMP protocol in routing:</h2>_ 

_`$ tcpdump -n -i eth0 icmp` command on the r1 and `$ ping -c 1 10.30.0.111` from the ws11 machine:_ \
<img src="../screenshots/part5/6_1.png" alt="6_1.png" width="800"/>

_Save dumps of the virtual machine images: OK

_<h2 align="left"> Part 6. Dynamic IP configuration using DHCP:</h2>_

_<h3 align="left"> 1) Specify the default router address, DNS-server and internal network address:</h3>_

`$ sudo apt install isc-dhcp-server`.

_For r2, configure the DHCP service in the /etc/dhcp/dhcpd.conf file:_ \
<img src="../screenshots/part6/1_1.png" alt="1_1.png" width="800"/>

_<h3 align="left"> 2) Write nameserver 8.8.8.8. in a resolv.conf file:</h3>_

_Write nameserver 8.8.8.8. in a resolv.conf file:_ \
<img src="../screenshots/part6/1_2.png" alt="1_2.png" width="800"/>

_Restart the DHCP service with systemctl restart isc-dhcp-server:_ \
<img src="../screenshots/part6/1_3.png" alt="1_3.png" width="800"/>

Update netplan of ws21 for DHCP to take effect. 

_Reboot the ws21 machine with reboot and show with ip a that it has got an address (above old output and below new)._ \
<img src="../screenshots/part6/1_4.png" alt="1_4.png" width="800"/>

_Ping ws22 from ws21:_ \
<img src="../screenshots/part6/1_5.png" alt="1_5.png" width="800"/>

_Specify MAC address at ws11 by adding to etc/netplan/00-installer-config.yaml:_ \
<img src="../screenshots/part6/1_6.png" alt="1_6.png" width="800"/> \
<img src="../screenshots/part6/1_7.png" alt="1_7.png" width="800"/>  \
<img src="../screenshots/part6/1_8.png" alt="1_8.png" width="800"/>

_Сonfigure r1 the same way as r2, but make the assignment of addresses strictly linked to the MAC-address (ws11):_ \
<img src="../screenshots/part6/1_9.png" alt="r1_dhcp_set" width="800"/> \
<img src="../screenshots/part6/1_10.png" alt="r1_dhcp_restarted" width="800"/> \
<img src="../screenshots/part6/1_11.png" alt="ws11_new_ip_received" width="800"/>

_Ping ws22 from ws11:_ \
<img src="../screenshots/part6/1_12.png" alt="ping_ws22_from_ws11" width="800"/>

_Request ip address update from ws21:_ \
<img src="../screenshots/part6/1_13.png" alt="ws21_request_new_ip" width="800"/>

_Describe in the report what DHCP server options were used in this point:_ \
The -r flag explicitly releases the current lease, and once the lease has been released, the client exits.

_Save dumps of virtual machine images:_ OK
