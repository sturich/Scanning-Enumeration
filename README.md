# Scanning-Enumeration

<h2>**Using Kioptrix via vulnhub**</h2>

# Kioptrix Scanning and Enumeration Guide

This repository provides detailed instructions and tools for scanning and enumerating the Kioptrix virtual machine (VM) hosted on VulnHub. Kioptrix is a popular vulnerable VM designed to help security enthusiasts and professionals practice penetration testing techniques in a controlled environment.

VulnHub is a great platform for learning and improving skills in cybersecurity, providing a range of vulnerable VMs for practice. This repository covers step-by-step approaches for:

- Setting up Kioptrix on your local machine using VirtualBox or VMware
- Running common scanning tools like Nmap and Netcat for service enumeration
- Identifying potential attack vectors and weaknesses
- Collecting valuable information to exploit vulnerabilities

This repository is ideal for learners looking to enhance their enumeration, scanning, and exploitation skills.

## Features:
1. **Detailed Setup Instructions**  
   Step-by-step guide on how to set up the Kioptrix VM on your local machine using VirtualBox or VMware, ensuring a smooth experience for beginners.

2. **Comprehensive Scanning Techniques**  
   Learn how to effectively use Nmap to perform network scans, identify open ports, and understand the information provided by different types of scans.

3. **Service Enumeration**  
   Learn how to identify and enumerate services running on the Kioptrix VM using tools like Netcat and Gobuster to help identify potential vulnerabilities.

4. **Vulnerability Assessment**  
   Instructions on how to use Nikto, Hydra, and other tools for vulnerability scanning, helping you uncover misconfigurations and weak spots in the system.

5. **Exploit Identification**  
   Techniques to identify common exploits for the vulnerabilities found in Kioptrix, as well as guidance on how to leverage these weaknesses for exploitation in a controlled environment.

## Requisites:
1. **Virtualization Software**  
   To run the Kioptrix VM, you'll need a virtualization platform like [VirtualBox](https://www.virtualbox.org/) or [VMware](https://www.vmware.com/), which will allow you to host the vulnerable machine on your local machine.

2. **Basic Knowledge of Penetration Testing Tools**  
   Familiarity with common penetration testing tools like Nmap, Netcat, Gobuster, and Nikto is recommended, as this repository assumes basic knowledge of scanning and enumeration techniques.

<h2>Languages and Utilities Used</h2>

- <b>Kioptrix</b> 
- <b>Vulnhub</b>
- <b>VirtualBox or VMware</b>
- <b>NMAP</b>

<h2>Environments Used </h2>

- <b>Linux</b>

<h2>Program walk-through:</h2>

installing Kioptrix via vulnhub

![Screenshot 2025-02-21 at 20 36 39](https://github.com/user-attachments/assets/1b4dde4f-3d3a-4b1c-b096-f87b3a30e096)

NMAP

![Screenshot 2025-02-21 at 20 37 47](https://github.com/user-attachments/assets/9b24975b-ab66-4aec-a776-ce9736fdd86a)

Linux has an inbuilt feature called arp scan  
Type: arp-scan -l

![Screenshot 2025-02-21 at 20 38 36](https://github.com/user-attachments/assets/b06d66bf-e7bf-4186-b586-3a5cc0f912bd)

What i am looking for is VMware

![Screenshot 2025-02-21 at 20 39 58](https://github.com/user-attachments/assets/d5a9614f-8402-4fd5-bef3-dab1a5399422)

I will use ifconfig to get our IP address.  
I will copy the first 3 digits of the IP address eg 192.168.57  
I will then type the following.  
netdiscover -r 192.168.57.0/24

This will scan the entire subnet

![Screenshot 2025-02-21 at 20 42 24](https://github.com/user-attachments/assets/cd583a49-57bc-4d61-82cf-9fc3f34a501c)

![Screenshot 2025-02-21 at 20 43 20](https://github.com/user-attachments/assets/3b67bba4-6e6b-4b41-ba90-68ba15719978)

Ignore the .1, .2 & .254 so we know the system address is .134 so we can start attacking it.

So we are going to use NMAP to scan for open ports, we know the 3 part handshake is SYN SYNACK ACK but we want to scan this in stealth mode, this used to be done by doing nmap -sS but this has been changed to default now.

If clients are running good security then nmap will be picked up, just dont assume companies are running good security.

So in stealth mode nmap would send over the SYN package and if the port is open you will get yeah you can connect to me and they send back the SYNACK but nmap will then send back RST (reset) to say i dont need to connect now so we dont actually establish a connection and this was why nmap sS was called stealth mode.

nmap command

![Screenshot 2025-02-21 at 20 45 40](https://github.com/user-attachments/assets/f9de784c-3e09-4d15-bdb9-5452ab912ae0)

-T4 = speed, this can be between 1 & 5, the slower you do this the better the scan but the longer it will take so its advised to run a minimum of 3 - 5 default to run these for best performance is T4.

-p- stands for i want to scan all ports, if you leave blank it will scan the top 1000 ports but there are 65,535 ports so you could miss something valuable.

-A stands for everything, i want the version, operating system, fingerprinting, anything you can tell me.

![Screenshot 2025-02-21 at 20 46 39](https://github.com/user-attachments/assets/1cb1aa83-b769-40ed-b73c-b9e54c63ceb9)

Put in IP address so it knows what to scan

![Screenshot 2025-02-21 at 20 47 36](https://github.com/user-attachments/assets/24ccc6d1-e45a-476e-8d56-356682030360)

--help is good to find what you need

![Screenshot 2025-02-21 at 20 48 38](https://github.com/user-attachments/assets/2c050a56-b756-4d61-b52b-17152dcfef8b)

To make things quicker i could remove the -A and this will scan all the ports quicker, from this i can use the list of ports and scan -A so i am not doing all to every port, just the ports i have identified. **A future project to be added to the repositories** script this out to say. Hey i want to scan this address using NMAP then once returned it can automatically scan -A to all these ports.

Scan completed -A

![Screenshot 2025-02-21 at 20 55 58](https://github.com/user-attachments/assets/280c9145-38fd-40a7-a736-8e7b984fbc96)

So i can see from here that port 22 is running ssh with a version number of 2.9p2

![Screenshot 2025-02-21 at 20 57 47](https://github.com/user-attachments/assets/b2638093-06a7-4676-9202-a52029c31bcc)

I can see that port 80 & 443 are running Apache/1.3.20 (Red-Hat / Linux)

![Screenshot 2025-02-21 at 20 58 39](https://github.com/user-attachments/assets/b4fa7aa7-071b-4846-9fb2-1d6d574923ec)

111 & 139 always play together. So we basically have smb open

![Screenshot 2025-02-21 at 20 59 57](https://github.com/user-attachments/assets/e94b25ba-7937-4984-a97c-798b9a45ba5d)

Its found Linux 2.4 operating system, its most likely pulling that down as a guess from Red-Hat & Apache.

![Screenshot 2025-02-21 at 21 01 04](https://github.com/user-attachments/assets/af223e05-31a1-4ae4-b2d5-2aef4e79cd1c)

Scan UDP as there are 65,535 ports to scan there too!

![Screenshot 2025-02-21 at 21 02 06](https://github.com/user-attachments/assets/58bb2709-3f38-4636-8283-8d2bb6a31b25)

Remove the -A and change -p- to -p as UDP takes forever to scan because it is a protocol connection it doesnt have that instant response time. So when we scan UDP its typical to scan the top 1000.

# Enumerating HTTP and HTTPS Part 1

So if i think like an attacker and looking at the list.

![Screenshot 2025-02-21 at 21 05 51](https://github.com/user-attachments/assets/78535c97-7fa1-4261-af57-3034a03bf1db)

It is encouraging when i see ports 80, 443, 139 sometimes you will see 445 with it, these are encouraging as they are commonly found with exploits. If i think about all the exploits that have been out there for a website for example or if i think to samba or SMB related exploits for example in 2017 there was malware that went around called 'want to cry' and that was based off of something called Eternal Blue, also known as M.S. 17 0 1 0. It was a pretty wicked exploit that utilised a flaw in SMB. SMB has been historically bad and so has websites. Now when we see something like port 22, port 22 is SSH and historically it hasn't been that bad. Now we can try attacks against it like brute force attacks or we can try something like default credentials or route tor on it for example. But when we look at it we may be able to enumerate the version but theres not usually what is called remote code execution on SSH, remote code execution being that we can run an exploit against it and get something called a shell back.  
It is not very common to attack SSH, it wouldn't be classed as a low hanging fruit, as an attacker i am looking for the most juiciest of fruit first.

I will initially investigate port 80 & 443

First use the IP address and go out to the website

![Screenshot 2025-02-21 at 21 10 34](https://github.com/user-attachments/assets/8859f3a0-6123-4324-aa28-5dbe37ca8c2f)

**NOTE:** If you used Burpsuite you will need to change back your settings.

![Screenshot 2025-02-21 at 21 11 31](https://github.com/user-attachments/assets/a50fc82d-4b5a-4501-9d2e-522c006250f2)

Website 192.168.57.134 for both HTTP (80) & HTTPS (443)

![Screenshot 2025-02-21 at 21 12 40](https://github.com/user-attachments/assets/e29163ba-995a-4daa-bf19-6a53f87ceab7)

What we have here is a default webpage, now when we talk about a network penetration testing or web application penetration test. If we see a default webpage like this, its an automatic finding.

WHY is it a finding?

Is it exploitable? NO not really but...  
it tells us a bit of information about the architecture running behind the scenes and it tells us about the clients potential hygiene.

From this page we can see its running Apache  
We know the box is potentially running Red Hat Linux

It also brings up a few questions

1.  Are there other directories behind this page? directory busting (192.168.57.134/admin(maybe the directories there?))
2.  Are they hosting a website somewhere else thats just not at this IP address on this base?
3.  Or they are not hosting a website and just left port 80 & 443 open for no reason and put this default webpage out there?

As an attacker that would signal **poor hygiene**, if the client is potentially just putting this out there, what other vulnerabilities might they have?

**THIS WOULD BE WRITTEN UP IN A REPORT FOR THE TEST.**

Example of notes:

![Screenshot 2025-02-21 at 21 14 23](https://github.com/user-attachments/assets/8f51e4ad-f62c-419b-9747-65e3ad8f12e4)

It is good to keep in-depth notes, this enables the report writing to be easier, also save these with the client name and date, potentially a client may come back to you in the future and ask about something, you then have the in-depth notes to refer back to.

![Screenshot 2025-02-21 at 21 15 52](https://github.com/user-attachments/assets/925cb5ac-eab6-47ea-9d03-1dbe962fc86b)











