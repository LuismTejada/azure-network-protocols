<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>
# azure-network-protocols
<p align="center">

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First you will need to create to VMs on Azure. One machine will be a Linux machine and the other will be a Windows 10 machine. Both will have two CPUs and they must be on the same VNET. Once that's done go on the Windows machine and download Wireshark. Here's a link to download Wireshark: https://www.wireshark.org/download.html and once that is done open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol. Ping tests connectivity between hosts. When we filter Wireshark to only capture ICMP packets and ping the private IP address of our Linux machine we can visually see the packets on Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. The picture below demonstrates that.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Next we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on its firewall. Once we do that we will stop receiving echo replies from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow traffic by allowing ICMP on the Linux Network Security Groups page on Azure.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI, it just gives the user access to the machine's CLI. We will set the Wireshark filter to capture SSH packets only. When we SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that Wireshark starts to immediately capture SSH packets.
</p>
<br />

Next we will use Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol. This works on ports 67/68. It's used to assign IP addresses to machines. We will request a new IP address with the command "ipconfig /renew". Once we enter the command Wireshark will capture DHCP traffic.

Time to filter DNS traffic. We will set Wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is Google's IP address.

Lastly we will filter for RDP traffic. When we enter tcp.port==3389, traffic is spammed non-stop because we are using Remote Desktop Protocol to connect to our Virtual Machine
