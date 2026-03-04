### Challenge 1: Network Traffic Analysis using Wireshark

**Original Question:**  
"Analyze the PCAP file and answer:  
1. What protocol is running on port 3942?  
2. What IP address was pinged twice?  
3. How many DNS query responses are in the capture?  
4. What IP address sent the most bytes?"


**lets start:Challenge1
**Download PCAP:** [challenge1-PCAP1-files>SBT-PCAP1.png]

**My Analysis:**

![Packet List with Filter](screenshots/challenge1-packetlist.png)


0.Wireshark overview

![Wireshark startup - loading PCAP file]
1<img width="1920" height="1080" alt="Wireshark overview 1" src="https://github.com/user-attachments/assets/845a5e73-a981-4898-94fa-9336c62c2097" />

![Wireshark overview - initial packet statistics]
2<img width="1920" height="1080" alt="Wireshark overview 2" src="https://github.com/user-attachments/assets/73f5e01c-3108-453f-9157-ec2dd3c2712d" />


1.What protocol is running on port 3942?

The PCAP file was opened in Wireshark to determine the protocol associated with port 3942. By inspecting the packet details pane, the transport and application layer information can be observed to identify which protocol is being used for the communication.

<img width="1920" height="1080" alt="Protocol Identification for Port 3942" src="https://github.com/user-attachments/assets/929a356d-8c6e-45d1-8d23-432ff16dd07c" />

Observation

From the packet details section in Wireshark, the protocol field shows that the traffic associated with port 3942 is using the [SSDP] protocol.
filter used udp.port==3942

ANS}The protocol used for port 3942 is [SSDP].follow with the image above.

2.What IP address was pinged twice? 

To determine which IP address was pinged twice, the ICMP protocol traffic was analyzed in Wireshark. ICMP is commonly used for ping operations, where an ICMP Echo Request is sent to a destination host and an ICMP Echo Reply is returned.
By applying a Wireshark display filter for ICMP packets, it is possible to isolate all ping-related traffic and examine the source and destination IP addresses involved in the communication.

<img width="1920" height="1080" alt="Identifying the IP Address Pinged Twice " src="https://github.com/user-attachments/assets/8d4d5c99-8b3a-4c14-9175-8c8a5885ff23" />

The icmp display filter was applied in Wireshark to view only ICMP traffic.
Observation
The packet list shows two ICMP Echo Request packets directed toward the same destination IP address.

ANS}The IP address that was pinged twice is [8.8.4.4].

3. How many DNS query responses are in the capture?

 DNS (Domain Name System) is used to translate domain names into IP addresses. When a system tries to access a website, it first sends a DNS query, and the DNS server sends back a DNS response with the IP address.
To find how many DNS responses are in the packet capture, the DNS traffic can be filtered in Wireshark and the packets marked as responses can be counted.

<img width="1920" height="1080" alt="DNS Query and Response Packet Count" src="https://github.com/user-attachments/assets/e70a0d25-0dfc-4aa6-86e2-b7dd63aad438" />

In Wireshark, the following display filter was applied to show only DNS response packets:
dns.flags.response == 1

Once the filter is applied then you need to click on the first dns query response you see on the packet header pane and then when clicked on the dns query response packet details pane will open look for domain name system and then dns>flags>message>right-click-on-message>apply>selected then in the bottom right corner in packets bytes pane u will get the no of packets that were captured, In our case it is 90 packets were captured.

ANS}The packet capture contains [90] DNS query response packets.

4. What IP address sent the most bytes?"
Explanation:
To identify which IP address sent the most data in the packet capture, Wireshark provides a feature called Conversations Statistics. This feature shows how much data was sent and received between different IP addresses. By examining the bytes sent column, we can determine which IP transmitted the highest amount of data.


Look atimage 1:  <img width="1410" height="795" alt="Identifying ip with most transmitted bytes  1" src="https://github.com/user-attachments/assets/1c9e31ab-5cb3-4b9e-b568-2a55894282e3" />

Look at image 2: <img width="1920" height="1080" alt="Identifying ip with most transmitter bytes  2" src="https://github.com/user-attachments/assets/103d680a-74ae-4caa-bb6f-db166de7dde9" />

The IPv4 Conversations statistics tool in Wireshark was used to analyze the amount of data sent by each IP address.

Steps performed:
Open the PCAP file in Wireshark>
Click Statistics in the top menu>
Select Conversations>
Go to the IPv4 tab>
then click on tx bytes and arrange them in descending order by clicking the tx byte column 2 or 3 times and then the ip with the most packets transferred will appear on top.

the Bytes or Bytes A → B column to identify which IP sent the largest amount of data.

Observation:
The conversation statistics show that the IP address [IP ADDRESS] transmitted the largest number of bytes in the capture.

ANS}The IP address that sent the most bytes is [IP ADDRESS].


Conclusion

In this analysis, the provided PCAP file was examined using Wireshark to understand the network activity captured in the traffic. Different filters and analysis features were used to investigate specific questions such as identifying the protocol used on a particular port, detecting ICMP ping activity, examining DNS responses, and determining which IP address transmitted the most data.

Through this process, the packet capture revealed useful information about the communication taking place between hosts. By applying protocol filters and reviewing packet details, it was possible to trace the behavior of the network traffic and identify the relevant IP addresses and protocols involved.

Overall, this exercise helped demonstrate how Wireshark can be used to inspect packet captures, filter specific types of traffic, and extract meaningful information from network data during a basic traffic analysis investigation.
