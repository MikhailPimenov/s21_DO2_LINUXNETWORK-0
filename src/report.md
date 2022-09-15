## Part 1. ipcalc tool

- there is no ipcalc tool.

![text](../screenshots/part1/1.png)

- installing ipcalc. $ sudo apt install ipcalc

![text](../screenshots/part1/2.png)

# 1.1 Networks and Masks

- 1. Network address of 192.167.38.54/13

![text](../screenshots/part1/1_1_1.png)


- 2. Conversion of the mask 255.255.255.0 to prefix and binary, /15 to normal and binary, 11111111.11111111.11111111.11110000 to normal and prefix

- сonversion of the mask 255.255.255.0 to prefix and binary
![text](../screenshots/part1/1_1_2-1.png)

- /15 to normal and binary
![text](../screenshots/part1/1_1_2-2.png)

- 11111111.11111111.11111111.11110000 to normal and prefix
![text](../screenshots/part1/1_1_2-3.png)


- 3. Minimum and maximum host in 12.167.38.4 network with masks: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 and /4
![text](../screenshots/part1/1_1_3-1.png)


# 1.2 Localhst
- can be accessed with
    127.0.0.2
    127.1.0.1
- can't with
    194.34.23.100
    128.0.0.1


# 1.3. Network ranges and segments

- 1. Which of the listed IPs can be used as public and which only as private: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1

- private
    10.0.0.45
    192.168.4.2
    172.20.250.4
    172.16.255.255
    10.10.10.10

- public
    134.43.0.2
    172.0.2.1
    192.172.0.1
    172.68.0.2
    192.169.168.1


- 2. Which of the listed gateway IP addresses are possible for 10.10.0.0/18 network: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255

- possible
    10.10.0.2
    10.10.10.10
    10.10.1.255

- impossible
    10.0.0.1
    10.10.100.1
