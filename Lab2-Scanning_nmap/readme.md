# Nmap Lab
## Scenario
You are a New Network Admin and you need to check what ports are open on one of your system. You know that it’s a Linux Machine and you know which network it’s on but you’re not sure of it’s IP address.

## Objectives
1. Find the machine we're looking for.
2. Scan for open ports on the target machine.
3. Identify services running on open ports.

## Environment
* Windows 10 VM
* Metasploitable 2
* Machines are on the same network

## Estimated Duration
30-45 Minutes

## Part 1: Device Detection and OS Scanning
1. Nmap is normally run in the command line, however, for new network admins and ethical hackers, Nmap also provides a graphical user interface (GUI) to work with called Zenmap. Sadly, nmap is no longer supporting zenmap debian/kali installations. For zenmap demos, we'll install it on Windows 10 running as administrator. You can download the exe for install [here](https://nmap.org/download.html).
<img src="images/nmap_open.png" alt="Zenmap Open" width="400px"/>
2. Before we begin, we want to clear everything form the command box except for “nmap”. When we first join a network, we want to see who else is out there. We can do this by clicking in the Target Field and typing our network id “10.0.2.0/24” and pressing “Scan”
<img src="images/init_scan.png" alt="Initial Scan" width="400px"/>

3. According to nmap there are five hosts on my network. Currently however we have very little information on each of the addresses returned. We know which one is ours but what about the others? If we look in the host details tab all we see is the statistics on what we have scanned. NOTE: It is possible that devices may not respond to Nmap’s initial ping and therefore will not get listed here by nmap.
<img src="images/host_details.png" alt="Host Details" width="400px"/>

4. We can use nmap to determine (or give us a best guess) of what operating systems are on the network. To do this we can go to Profile->New Profile and here we’ll give our new profile a name. I have called it “OS Scan”. From there we can go to Scan and select the option Operating System Detection (-O) and click “Save Changes”

OS Scan p1  |  OS Scan p2
:----------:|:------------:
![Create OS Scan 1](images/creating_profile_part1.png) | ![Create OS Scan 2](images/creating_profile_part2.png)

5. Once you have the profile saved you can then select it and run it in the profile section.
<img src="images/os_scan.png" alt="OS Scan" width="400px"/>

6. After running the scan we can see that we’re working with a linux 2.6.9-2.6.33 computer at address 10.0.2.7
<img src="images/host_details2.png" alt="Host Details with OS Scan" width="400px"/>

7. Continuing forward we will focus on the 10.0.2.7 metasploitable2 machine so we can change our target field to that address. This will increase the speed of our scans and reduce the noise (excess traffic) we produce while scanning.

## Part 2: Port Scanning
1. Now that we have identified our target, we can proceed to scan it and discover what ports it has open and what services it has running. Select Profile->New Profile, I named mine “Full Port Scan”, and go to Scans. For this example, I selected TCP Scans-> SYN Scan (-sS). You will only be able to run this scan if you are running nmap as root. If you are not able to run nmap as root you can choose Connect Scan (-sC). We will also select Target->ports and enter the range 1-65535 and click Save Changes 
