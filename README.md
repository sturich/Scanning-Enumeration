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
- <b>Nikto Web Vulnerability Scanner</b>
- <b>METASPLOIT</b>

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

If you click on the documentation link you get a 404 error page but this is showing a bit more information than it should do, this would be called information disclosure so something else to note in the report. We can see the following:

![Screenshot 2025-02-21 at 21 17 25](https://github.com/user-attachments/assets/972f3f67-9663-41c4-9a99-b387dd07ad35)

We also have kioptrix.level1, this is an internal information hostname. We can get naming interventions from the client, we could potentially identify how they are utilising naming conventions on their internal network.

Nikto - Web vulnerability scanner. (NOTE: IF THE WEBSITE IS RUNNING GOOD SECURITY YOU MAY RUN INTO ISSUES, IT MIGHT AUTO BLOCK IT IF ITS DETECTED. ITS NOT ALWAYS THE CASE, YOU WILL HAVE TO USE YOUR HUNCH BASED ON WHAT YOU HAVE SEEN SO FAR AS TO WHETHER YOU THINK THEY ARE RUNNING GOOD SECURITY / WEB APPLICATION FIREWALL).

![Screenshot 2025-02-21 at 21 19 26](https://github.com/user-attachments/assets/6edfc2fb-ccc7-4040-94b6-d7046ff4ab89)

![Screenshot 2025-02-21 at 21 20 47](https://github.com/user-attachments/assets/f690cc2c-4df6-46dc-bef2-bd444079e5da)

-h = host

As we can see HTTPS (443) didnt work but HTTP (80) did

![Screenshot 2025-02-21 at 21 21 45](https://github.com/user-attachments/assets/73fe3b12-5405-493b-921f-bd0aefbdb5bd)

![Screenshot 2025-02-21 at 21 22 34](https://github.com/user-attachments/assets/9dcb363e-9575-4e2b-b990-989ce8927bcb)

It is giving us some vulnerabilities back (highlighted)

![Screenshot 2025-02-21 at 21 23 18](https://github.com/user-attachments/assets/3fc8d7b8-f488-466b-bfbf-742eacc143b0)

Its showing us whats missing in terms of protections

![Screenshot 2025-02-21 at 21 24 08](https://github.com/user-attachments/assets/5ca05356-2b40-46c2-9e67-023b4fdefeb4)

We can see Apache, ssl and open SSL appears to be outdated

![Screenshot 2025-02-21 at 21 24 47](https://github.com/user-attachments/assets/dae38ab1-a0c4-44d3-bf51-629afd23f0c8)

A 1.3.20 is pretty outdated to the 2.4.37, we would notate these findings for the report.

We can also see the attacks that are susceptible too.

![Screenshot 2025-02-21 at 21 25 41](https://github.com/user-attachments/assets/423725c8-e12a-4450-8663-f17bdd2732b7)

A DoS would be outside of the scope so we wouldn't be interested in this (RED)  
Overflows (GREEN) we would skip over the local buffer but the ssl/2.8.4 is a REMOTE buffer overflow meaning we dont have to physically be there, we can be somewhere else.

It does a little bit of directory busting for us by running a wordlist like Admin, usage, manual etc.

![Screenshot 2025-02-21 at 21 27 08](https://github.com/user-attachments/assets/04f5067b-5ad9-4242-85d8-a7872ba5b2bc)

Save scan in directory to refer back to later, copy scan and place in mkdir kioptrix for example

![Screenshot 2025-02-21 at 21 28 03](https://github.com/user-attachments/assets/17b213e2-2091-4857-a774-804675717d7f)

Also add the ssl vulnerability to our notes.

![Screenshot 2025-02-21 at 21 29 11](https://github.com/user-attachments/assets/ca8d32c7-d1fb-48e7-8553-02011d3e997f)

# DIRBUSTER

![Screenshot 2025-02-21 at 21 30 33](https://github.com/user-attachments/assets/e0f6a5c5-fde0-4d4b-9cd1-10a8435ca9ff)

Syntax is important, it is going to want the port 80 at the end

![Screenshot 2025-02-21 at 21 36 41](https://github.com/user-attachments/assets/d9c71158-6524-4112-9644-70be6e8ca190)

Tick go faster  
Then click in browse

![Screenshot 2025-02-21 at 21 37 47](https://github.com/user-attachments/assets/3409228e-99b5-4b94-a36c-8fddd604cd07)

![Screenshot 2025-02-21 at 21 38 19](https://github.com/user-attachments/assets/b4aa7355-1418-40d1-bd36-6f2cf431d8b4)

Go to base folder

![Screenshot 2025-02-21 at 21 39 18](https://github.com/user-attachments/assets/399a602f-faa9-4205-b153-92f6fd66c681)

Go into User file (Usr)

![Screenshot 2025-02-21 at 21 40 00](https://github.com/user-attachments/assets/1bd5563a-dc0e-413e-ad8a-3ae4779f0b03)

Then share folder

![Screenshot 2025-02-21 at 21 40 49](https://github.com/user-attachments/assets/cec1be4b-d223-4c24-b980-1a1e8a82752f)

Start typing wordlist it will show the file

![Screenshot 2025-02-21 at 21 41 38](https://github.com/user-attachments/assets/1fd7e855-bf5c-4ebc-86bc-b569c7c35f08)

Then you will see dirbuster

![Screenshot 2025-02-21 at 21 42 22](https://github.com/user-attachments/assets/d0f2ad5a-61d7-4299-a7ad-f4abd4552d32)

From here we can pick a variety of lists (recommended in the course is the small list) but if you dont find anything then move up to medium and out on the inter web is a large list.

![Screenshot 2025-02-21 at 21 43 23](https://github.com/user-attachments/assets/4061ed81-1ccf-42bb-998d-2e302942f60b)

![Screenshot 2025-02-21 at 21 43 52](https://github.com/user-attachments/assets/40172e6f-6f7e-456a-9aa3-cbf49ea4e5d3)

What i am doing is going out to the web directories and using these wordlists, these wordlists have hundreds if not thousands of well known directories for example Admin or cgi bin. It is also going to go up against specific file extensions, we know from our investigations that we are up against an Apache website and Apache runs PHP, if we were up against a Microsoft website then we would change this to asp or aspx. This is why enumeration is so important because we need to know what is running on the back end to make the most use out of it. It is always good to run against others for example text file or zip file. You could also add rar, pdf, docx but the more you list the longer it will take to run

![Screenshot 2025-02-21 at 21 45 30](https://github.com/user-attachments/assets/155330d1-91fc-4385-8a4e-d89b88a39a81)

For this scan i will just run with PHP

Click start and you will see the scanning page

![Screenshot 2025-02-21 at 21 46 32](https://github.com/user-attachments/assets/8f00021f-85b0-407f-99da-f8f62cbaf9e0)

![Screenshot 2025-02-21 at 21 47 12](https://github.com/user-attachments/assets/34997bec-439d-41d0-9be9-1538ee21e5f1)

So we can see different response codes.

200's = ok  
400's = error eg 404 not found.  
300's = typically redirect  
500's = server errors

You can right click these and go to the page to see whats on there.

![Screenshot 2025-02-21 at 21 48 34](https://github.com/user-attachments/assets/6285f312-8243-4a4c-863e-3aec6c755caf)

![Screenshot 2025-02-21 at 21 49 15](https://github.com/user-attachments/assets/adf71cd9-29ab-4a95-b1e6-b7dbc8fe11bb)

We can see a bit of info here, we can see Webinizer Version 2.01 so we can copy this and see if there is anything about this. ADD TO NOTES for later in case there is anything exploitable.
_______________________________________________
<b>Always use source code of webpage to check the comments, sometimes things can be left in the comments like username, password etc.<b/>
------------------------------------------------
**Enumerating SMB Port 139 (file share, think at work you have a common drive example M'Drive at work)**

NOTES:  
Question: My enum4linux and/or smbclient are not working. I am receiving "Protocol negotiation failed: NT_STATUS_IO_TIMEOUT". How do I resolve?

Resolution:

On Kali, edit /etc/samba/smb.conf

Add the following under global:

client min protocol = CORE

client max protocol = SMB3

![Screenshot 2025-02-21 at 21 54 04](https://github.com/user-attachments/assets/c272d6f4-a082-44d7-8bd8-1049c81e998e)

Not much information here but because we run the -A scan so it does run script for us and we can see the scripts run below.

![Screenshot 2025-02-21 at 21 53 17](https://github.com/user-attachments/assets/99962e27-afd2-4740-b7f8-530d19f6995f)


We can see the information returned back, we can see its possibly running SMB2 but we are not sure, if we know the version number then there may be an exploit for it, so we need to find this information.

We also have to try and connect to this system, see if there is anything open to us, can we access the files etc.

We are going to start up a new terminal and run METASPLOIT (exploitation Framework)

![Screenshot 2025-02-21 at 21 55 25](https://github.com/user-attachments/assets/c5844cf4-b749-4a41-ae5d-b438aa04055e)

We need to find something for SMB enumeration

![Screenshot 2025-02-21 at 21 56 21](https://github.com/user-attachments/assets/e466984a-d30a-4f9a-833c-da29b586f06d)

Search smb

![Screenshot 2025-02-21 at 21 57 18](https://github.com/user-attachments/assets/2c488513-d941-46b4-97aa-505d976ac139)

You will get a really long list but we know auxiliary modules are enumeration  
The first part is what type of module this is. example post.

![Screenshot 2025-02-21 at 21 58 16](https://github.com/user-attachments/assets/ae9527a1-4aa0-4663-beb8-a0e486290b15)

The second part is what type of action it is doing.

![Screenshot 2025-02-21 at 21 59 16](https://github.com/user-attachments/assets/fe88ef2c-2664-4cfd-ba31-931d652f3e9a)

So what we are looking for is SMB version information so we need to look through the list.

![Screenshot 2025-02-21 at 22 00 01](https://github.com/user-attachments/assets/1112f4b5-4047-420e-b990-af2bba113fe3)

So to use this module you can either copy and paste it or use the number with 'use' as the command.

![Screenshot 2025-02-21 at 22 00 56](https://github.com/user-attachments/assets/23611eb3-c6a0-46d8-bde0-07561b90b57a)

Now it shows we are in auxiliary module.

![Screenshot 2025-02-21 at 22 01 40](https://github.com/user-attachments/assets/16d53226-e019-4abe-acf4-2a76e3c04971)

If you type 'info' you will get all the information for this module.

![Screenshot 2025-02-21 at 22 02 50](https://github.com/user-attachments/assets/f31177e2-d0c7-471c-aedc-5bdd907dea7a)

You can also type in 'options' just to get the options rather than printing out everything.

![Screenshot 2025-02-21 at 22 03 36](https://github.com/user-attachments/assets/d03e1f01-a52d-45fd-aafc-204e826eb6c4)

RHOSTS = Remote host (victim)

![Screenshot 2025-02-21 at 22 04 25](https://github.com/user-attachments/assets/dac419be-3576-4bad-8df6-c5b5dad3fdac)

Type run.

![Screenshot 2025-02-21 at 22 05 38](https://github.com/user-attachments/assets/d6b3c5bc-03c6-43cc-9970-b69dd5c3673d)

This is very specific and this is going to help us out. <b>NOTE THIS IN TEXT<b/> for later.

![Screenshot 2025-02-21 at 22 07 07](https://github.com/user-attachments/assets/2bd86109-0cf2-4122-8ea0-3d29e96a40dc)

<b>IMPORTANT TO DOCUMENT FINDINGS, THIS WILL SET YOU APART SHOWING YOUR ABILITY TO INFORMATION GATHER AND ENUMERATE.<b/>

Open up a new tab to use SMB Client

![Screenshot 2025-02-21 at 22 08 28](https://github.com/user-attachments/assets/74c30f9c-af62-4b0f-a2b1-450f5d28313f)

SMB Client is going to try and connect to the file share that is out there. If we have the ability to connect with anonymous access, what we will do is we can get in there and potentially see files. Now these files may give us an idea what is going on in the network or they may even be valuable to us, they maybe back up files or stored passwords in a txt file. You dont know what you will find until you actually look.

-L = to list out the files.  
the syntax starts with \ or you can use \\ <ip address="">\ (if you run with just \ then you dont need to put \ at the end).</ip>

![Screenshot 2025-02-21 at 22 09 43](https://github.com/user-attachments/assets/5bc653b2-a6b4-4d97-884e-f481ec43004f)

![Screenshot 2025-02-21 at 22 10 34](https://github.com/user-attachments/assets/cb04f270-0890-411f-878f-8194d74f5231)

Hit enter on root password because we dont know it.

![Screenshot 2025-02-21 at 22 11 33](https://github.com/user-attachments/assets/fe87375f-0cef-45d0-8ffe-d16a0361cfff)

So we listed out a file share but we will need to try and connect in a different way.

Remove the -L and add one of the 2 file shares we found, IPC is not very valuable so lets try Admin

![Screenshot 2025-02-21 at 22 12 38](https://github.com/user-attachments/assets/86030a72-0bf7-4056-b957-e3064bfb6bfb)

Hit enter

![Screenshot 2025-02-21 at 22 13 36](https://github.com/user-attachments/assets/11ac8762-d262-41cd-9900-7f7c5fcd7601)

We can see wrong password so its not going to let us connect with anonymous access.

We can try IPC to see.

![Screenshot 2025-02-21 at 22 14 50](https://github.com/user-attachments/assets/28093c81-b3ec-44d7-b934-9f79e7dac318)

We can see that we have now gained access. (this is interesting)  
We can type help to see a list of commands.

![Screenshot 2025-02-21 at 22 15 57](https://github.com/user-attachments/assets/d657f2b5-882f-4862-9ed3-99912918c458)

We can try ls as we are now in a Linux machine, but we are access denied so this is a dead end.

![Screenshot 2025-02-21 at 22 16 53](https://github.com/user-attachments/assets/a81c551a-5e0e-46c7-a119-cfbc71e0781a)
-------------------------------------------------------
# Enumerating SSH

![Screenshot 2025-02-21 at 22 20 36](https://github.com/user-attachments/assets/e0b752b0-84f3-4ba7-819f-de9c2143b45a)

Make a note of it for later

![Screenshot 2025-02-21 at 22 21 19](https://github.com/user-attachments/assets/beb744cc-c324-4676-b15c-16ff0dbe42c9)

Sometimes you will get a scan back with just open ssh and no version number so we are going to see if we can connect to this to gather the version number (as if we didn't have it)

So to connect with ssh you just type ssh (IP Address)

![Screenshot 2025-02-21 at 22 22 21](https://github.com/user-attachments/assets/0ae4bf51-f2d2-4b74-9d10-b501ce5a3100)

But sometimes you may get an error no matching key exchange method found so you need to add a bit of syntex.

ssh (IP Address) -oKexAlgorithms=+

Then copy the green box and paste after the plus sign

![Screenshot 2025-02-21 at 22 23 24](https://github.com/user-attachments/assets/85ce10cc-4b38-4c42-a053-9c1ffb5a4d34)

You will get one more error.

![Screenshot 2025-02-21 at 22 24 19](https://github.com/user-attachments/assets/809034f1-3a8d-4ea8-bf07-a50643d40e00)

This is going to ask about a cipher, it says no matching cipher found.  
We are going to add -c and copy the text in the green box and put it at the end.

![Screenshot 2025-02-21 at 22 25 19](https://github.com/user-attachments/assets/778ca501-d2ba-4546-aa1e-66a4ec205990)

And this should provide the opportunity to connect.

![Screenshot 2025-02-21 at 22 26 24](https://github.com/user-attachments/assets/f4495269-c015-4734-9dc4-8e7097f8c437)

Type in 'YES'

Its now asking us for a password.

![Screenshot 2025-02-21 at 22 27 17](https://github.com/user-attachments/assets/c334772c-b322-4e35-bf0d-366f4f17f8ba)

So there is nothing for us here (hit ctrl C)

Why did we attempt to do this?  
Sometimes a banner is exposed and this will tell you what it is running.
-----------------------------------------------------------------------------
# Researching Potential Vulnerabilities

Notes moved to Cherrytree to allow for easier viewing.

![Screenshot 2025-02-21 at 22 29 58](https://github.com/user-attachments/assets/23149018-fe64-47c6-b8a5-4a7e67b29693)

This is set in order of attacking starting with 80, 443,139 and 445 are the most juiciest of the fruit available.  
Out of all the items we have identified, i would say that mod_ssl/2.8.4 is the biggest opportunity so we will start with this first.

Go out to google and search for exploit.

![Screenshot 2025-02-21 at 22 31 18](https://github.com/user-attachments/assets/495a85b6-91c9-4359-a2de-2c7b7ad9a9c5)

Open these 2 links

![Screenshot 2025-02-21 at 22 31 55](https://github.com/user-attachments/assets/cd9e5e7d-c6ba-494b-8682-6b58593d07cf)

www.exploit-db.com

![Screenshot 2025-02-21 at 22 32 42](https://github.com/user-attachments/assets/79281b03-25f6-47eb-a46d-13b68f412e06)

This website has the code which is always good to look through to try and understand it better, we can see this has quite a lot of different architectures for Linux

![Screenshot 2025-02-21 at 22 33 46](https://github.com/user-attachments/assets/07c4b998-95e2-4d7d-8c67-409aa9327449)

Code for overflow.

![Screenshot 2025-02-21 at 22 34 30](https://github.com/user-attachments/assets/df426068-cd6e-4895-a772-70e0f7c035ca)

Note this down for report, with details of web addresses.

![Screenshot 2025-02-21 at 22 35 11](https://github.com/user-attachments/assets/5c3915a1-72da-474e-aad8-7512dd9614d8)

Then look at what exploits are for the others via google, again note down the addresses and details for reports.

![Screenshot 2025-02-21 at 22 36 20](https://github.com/user-attachments/assets/78cc5ebd-8346-4a52-b9aa-b9f7ba1abaa0)

Sometimes you will see these types of websites. (cvedetails), these are worth a look, you look at the scoring and if there is a red (critical) then that is a good find as well.

![Screenshot 2025-02-21 at 22 37 16](https://github.com/user-attachments/assets/fb385005-244d-4c4a-a0cf-9e4aa2f6b5e0)

![Screenshot 2025-02-21 at 22 37 51](https://github.com/user-attachments/assets/2dc61fd5-8cf7-4090-aa13-a6913dccaa2e)

Same for the SMB Samba

![Screenshot 2025-02-21 at 22 38 50](https://github.com/user-attachments/assets/c681d698-7d6a-4694-88f0-bc5e0dfc6a5e)

![Screenshot 2025-02-21 at 22 39 31](https://github.com/user-attachments/assets/29a631ce-df95-4f07-98e2-3581dd50f047)

NOTE: Rapid 7 is good to see.

![Screenshot 2025-02-21 at 22 40 32](https://github.com/user-attachments/assets/84393a70-0062-4666-8eb0-090d66d24dd1)

Why Rapid 7?? Rapid 7 makes Metasploit!!

![Screenshot 2025-02-21 at 22 41 15](https://github.com/user-attachments/assets/db358056-d93b-49da-969a-a25acc92b17c)

Note down vulnerability

![Screenshot 2025-02-21 at 22 42 11](https://github.com/user-attachments/assets/edb97a4c-cd58-4891-979e-a478a59b4ea2)

If you dont have web access for some reason, you need the information on the fly because you are already in a machine, you can use 'searchsploit' on your terminal. Do not be specific with this search, if you are looking for Samba 2.2.1a just type in samba 2, this will provide lots of options so you will need to look through it all to identify the right one, in my opinion its much easier to use google but it can be used if your in a pinch.

You can see its searching for samba and a 2, this 2 can be anywhere in the version but it will highlight it red.

![Screenshot 2025-02-21 at 22 43 15](https://github.com/user-attachments/assets/06cf97a5-f351-4c62-99b7-481fd6e95f61)

**Notes So Far**  
These notes are for our assessment and to aid with the report writing once the assessment has been completed. You can use whatever software you are comfortable with, you can set it out how you want to, just ensure it is detailed.

![Screenshot 2025-02-21 at 22 44 32](https://github.com/user-attachments/assets/0b8abbaa-4b66-4fa4-a275-8b3f4a96cab0)

Make sure in findings you have the IP address in the screen shot.

![Screenshot 2025-02-21 at 22 45 28](https://github.com/user-attachments/assets/6ae2825d-0e63-425e-94e8-5aca88e2d504)

And the information disclosure.

![Screenshot 2025-02-21 at 22 46 12](https://github.com/user-attachments/assets/0dbaf701-a04a-4f0c-b1c3-eef07d52ff4a)






















