Challenge 2: Network Traffic Analysis using Wireshark

Original Question:
"Analyze the PCAP file and answer:



1)What is the WebAdmin password?

2)What is the version number of the attacker’s FTP server?

3)Which port was used to gain access to the victim Windows host?

4)What is the name of a confidential file on the Windows host?

5)What is the name of the log file that was created at 4:51 AM on the Windows host?


My Analysis:


1)What is the WebAdmin password?

<img width="1317" height="695" alt="Q1,1" src="https://github.com/user-attachments/assets/4069bea2-cc22-4f88-b6bd-85a421d574ce" />

I decided to check HTTP traffic. Typed in http in display filter.

Packet 4121 looks promising because of a GET request for a password.txt. Right click on packet > followed HTTP stream.

<img width="927" height="406" alt="Q1,2" src="https://github.com/user-attachments/assets/ac087a61-467c-4617-bcf7-e53223f2f893" />

Answer = sbt123

HTTP traffic does not have encryption so data can be seen in plain text. Checked the frame info of packet 4123 and I was also able to find the password there.


2. What is the version number of the attacker’s FTP server?

Based on the previous question, we can infer that the device used to get the password.txt file is the attacker. Type in “ftp and ip.src == 192.168.56.1” in the display filter.
<img width="1127" height="570" alt="Q2,1" src="https://github.com/user-attachments/assets/65cc4f04-0aff-4684-aae5-2de26ee02de1" />

A brief google search on pyftpdlib shows that it is a python FTP server library.

Answer: 1.5.5

Version number can also be seen by following the TCP stream on packet 4243.
<img width="885" height="214" alt="Q2,2" src="https://github.com/user-attachments/assets/7d08e42c-25ec-496e-b05c-c700dba2198d" />


3. Which port was used to gain access to the victim Windows host?

I decided to look at all the traffic coming from the compromised host 192.168.56.1. Type in “ip.src == 192.168.56.1” in the display filter.

I reviewed the results and found the HTTP traffic when the attacker downloaded the password.txt file. I checked packet 4128 which is the first packet after the HTTP traffic. I followed the TCP stream by right clicking the packet > Follow > TCP stream.

<img width="1292" height="337" alt="Q3,1" src="https://github.com/user-attachments/assets/79dc412e-715e-4140-a6b3-0aae2ef9c62e" />

<img width="359" height="441" alt="Q3,2" src="https://github.com/user-attachments/assets/420dc4d5-0b7e-4a85-94da-add9f5ed9084" />

The attacker moved to the desktop and listed the files in the location. It then created a txt file which contains commands to connect to the server using anonymous FTP and download a binary called malware.exe.

I went back to the main screen and looked for the destination port.

Answer = 8081


4. What is the name of a confidential file on the Windows host?

I followed the TCP stream of packet 4128. I checked the files in the Desktop directory and found a file called Employee_Information_CONFIDENTIAL.txt.

<img width="1284" height="709" alt="Q4,1" src="https://github.com/user-attachments/assets/36d67e88-c91a-409a-806d-3d72962eefb1" />

Answer =  Employee_Information_CONFIDENTIAL.txt


5. What is the name of the log file that was created at 4:51 AM on the Windows host?

Similar process as before, I followed TCP stream of packet 4128. A log file was created at 4:51 AM called LogFile.log.
<img width="1296" height="702" alt="Q5,1" src="https://github.com/user-attachments/assets/ae4dadba-2ea6-48f0-9e49-ff88c164f6ba" />

