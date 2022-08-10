# Hip Flask

Hip Flask is a beginner to intermediate level walkthrough. It aims to provide an in-depth analysis of the thought-processes involved in attacking an exposed webserver hosting a custom application in a penetration testing context.

Specifically, this room will look at exploiting a Python Flask application via two very distinctive flaws to gain remote code execution on the server. A simple privilege escalation will then be carried out resulting in full root access over the target.

The tasks in this room will cover every step of the attack process in detail, including providing possible remediations for the vulnerabilities found.  There are no explicit pre-requisites to cover before attempting this room; however, further resources will be linked at relevant sections should you wish further practice with the topic in question. That said, knowledge of Python and basic hacking fundamentals will come in handy. When in doubt: research!

Firefox is highly recommended for the web portion of this room. Use a different browser if you like, but be warned that any and all troubleshooting notes will be aimed at Firefox.

## CMDs

`$ T_IP="10.10.70.20"`


#### Normal scan
```$ nmap $T_IP
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-08 06:58 EDT
Nmap scan report for 10.10.91.3
Host is up (0.049s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE
22/tcp  open  ssh
53/tcp  open  domain
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 2.65 seconds
```

#### Discovery Scan
```
$ sudo nmap -vv $T_IP -oN Initial-SYN-Scan
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-10 03:20 EDT
Initiating Ping Scan at 03:20
Scanning 10.10.70.20 [4 ports]
Completed Ping Scan at 03:20, 0.09s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 03:20
Completed Parallel DNS resolution of 1 host. at 03:20, 0.00s elapsed
Initiating SYN Stealth Scan at 03:20
Scanning 10.10.70.20 [1000 ports]
Discovered open port 53/tcp on 10.10.70.20
Discovered open port 443/tcp on 10.10.70.20
Discovered open port 80/tcp on 10.10.70.20
Discovered open port 22/tcp on 10.10.70.20
Completed SYN Stealth Scan at 03:20, 0.94s elapsed (1000 total ports)
Nmap scan report for 10.10.70.20
Host is up, received syn-ack ttl 63 (0.053s latency).
Scanned at 2022-08-10 03:20:19 EDT for 0s
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE REASON
22/tcp  open  ssh     syn-ack ttl 63
53/tcp  open  domain  syn-ack ttl 63
80/tcp  open  http    syn-ack ttl 63
443/tcp open  https   syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.14 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.060KB)

```

#### Service Scan on specific ports

```
 sudo nmap -p 22,53,80,443 -sV -Pn -vv $T_IP -oN service-scan
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-10 03:23 EDT
NSE: Loaded 45 scripts for scanning.
Initiating Parallel DNS resolution of 1 host. at 03:23
Completed Parallel DNS resolution of 1 host. at 03:23, 0.00s elapsed
Initiating SYN Stealth Scan at 03:23
Scanning 10.10.70.20 [4 ports]
Discovered open port 443/tcp on 10.10.70.20
Discovered open port 53/tcp on 10.10.70.20
Discovered open port 80/tcp on 10.10.70.20
Discovered open port 22/tcp on 10.10.70.20
Completed SYN Stealth Scan at 03:23, 0.09s elapsed (4 total ports)
Initiating Service scan at 03:23
Scanning 4 services on 10.10.70.20
Completed Service scan at 03:24, 16.12s elapsed (4 services on 1 host)
NSE: Script scanning 10.10.70.20.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 03:24
Completed NSE at 03:24, 0.61s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 03:24
Completed NSE at 03:24, 0.41s elapsed
Nmap scan report for 10.10.70.20
Host is up, received user-set (0.053s latency).
Scanned at 2022-08-10 03:23:56 EDT for 17s

PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
53/tcp  open  domain   syn-ack ttl 63 (unknown banner: Now why would you need this..?)
80/tcp  open  http     syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
443/tcp open  ssl/http syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.92%I=7%D=8/10%Time=62F35D17%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,4B,"\0I\0\x06\x85\0\0\x01\0\x01\0\0\0\0\x07version\x04
SF:bind\0\0\x10\0\x03\xc0\x0c\0\x10\0\x03\0\0\0\0\0\x1f\x1eNow\x20why\x20w
SF:ould\x20you\x20need\x20this\.\.\?");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.72 seconds
           Raw packets sent: 4 (176B) | Rcvd: 4 (176B)

```
Important part:

```
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
53/tcp  open  domain   syn-ack ttl 63 (unknown banner: Now why would you need this..?)
80/tcp  open  http     syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
443/tcp open  ssl/http syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.92%I=7%D=8/10%Time=62F35D17%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,4B,"\0I\0\x06\x85\0\0\x01\0\x01\0\0\0\0\x07version\x04
SF:bind\0\0\x10\0\x03\xc0\x0c\0\x10\0\x03\0\0\0\0\0\x1f\x1eNow\x20why\x20w
SF:ould\x20you\x20need\x20this\.\.\?");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


#### UDP Scan results

```
$ sudo nmap -sU --top-ports 50 -vv --open $T_IP 10.10.70.20 -oN udp-top-ports
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-10 03:31 EDT
Initiating Ping Scan at 03:31
Scanning 10.10.70.20 [4 ports]
Completed Ping Scan at 03:31, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 03:31
Completed Parallel DNS resolution of 1 host. at 03:31, 0.00s elapsed
Initiating Ping Scan at 03:31
Scanning 10.10.70.20 [4 ports]
Completed Ping Scan at 03:31, 0.08s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 03:31
Completed Parallel DNS resolution of 1 host. at 03:31, 0.00s elapsed
Initiating UDP Scan at 03:31
Scanning 10.10.70.20 [50 ports]
Discovered open port 53/udp on 10.10.70.20
Increasing send delay for 10.10.70.20 from 0 to 50 due to max_successful_tryno increase to 4
Increasing send delay for 10.10.70.20 from 50 to 100 due to max_successful_tryno increase to 5
Increasing send delay for 10.10.70.20 from 100 to 200 due to max_successful_tryno increase to 6
Increasing send delay for 10.10.70.20 from 200 to 400 due to max_successful_tryno increase to 7
Increasing send delay for 10.10.70.20 from 400 to 800 due to max_successful_tryno increase to 8
Completed UDP Scan at 03:32, 52.97s elapsed (50 total ports)
Nmap scan report for 10.10.70.20
Host is up, received syn-ack ttl 63 (0.050s latency).
Scanned at 2022-08-10 03:31:39 EDT for 53s
Not shown: 46 closed udp ports (port-unreach)
PORT     STATE         SERVICE  REASON
53/udp   open          domain   udp-response ttl 63
68/udp   open|filtered dhcpc    no-response
631/udp  open|filtered ipp      no-response
5353/udp open|filtered zeroconf no-response
```


## Task 7.

With the initial enumeration done, let's have a look at some vulnerability scanning. We could keep using Nmap for this (making use of the NSE -- Nmap Scripting Engine); or we could do the more common thing and switch to an industry-standard vulnerability scanner: Nessus.

Unfortunately, due to licensing it is not possible to provide a machine with Nessus pre-installed. If you want to follow along with this section then you will need to download and install Nessus Essentials (the free version) for yourself. This is a relatively straight-forward process (which is covered in detail in the Nessus room), however, it can take quite a while! Nessus Essentials limits you significantly compared to the very expensive professional versions; however, it will do for our purposes here. This task is not essential to complete the room, so feel free to just read the information here if you would prefer not to follow along yourself.

The short version of the installation process is:

    Create a new Ubuntu VM (Desktop or Server, or another distro entirely). 40Gb hard disk space, 4Gb of RAM and 2 VCPUs worked well locally; however, you could probably get away with slightly less processing power for what we are using Nessus for here. A full list of official hardware requirements are detailed here, although again, these assume that you are using Nessus professionally.
    With the VM installed, go to the Nessus downloads page and grab an appropriate installer. For Ubuntu, Debian, or any other Debian derivatives, you are looking for a .deb file that matches up with your VM version (searching the page for the VM name and version -- e.g. "Ubuntu 20.04" -- can be effective here). Read and accept the license agreement, then download the file to your VM.
    Open a terminal and navigate to where you downloaded the package to. Install it with sudo apt install ./PACKAGE_NAME.
    This should install the Nessus server. You will need to start the server manually; this can be done with: sudo systemctl enable --now nessusd. This will permanently enable the Nessus daemon, allowing it to start with the VM, opening a web interface on https://LOCAL_VM_IP:8834.
    Navigate to the web interface and follow the instructions there, making sure to select Nessus Essentials when asked for the version. You will need a (free) activation code to use the server; this should be emailed directly from the server web interface. If that doesn't work then you can manually obtain an activation code from here.
    Allow the program some time to finish setting up, then create a username and password when prompted, and login!