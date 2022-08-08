# Hip Flask

Hip Flask is a beginner to intermediate level walkthrough. It aims to provide an in-depth analysis of the thought-processes involved in attacking an exposed webserver hosting a custom application in a penetration testing context.

Specifically, this room will look at exploiting a Python Flask application via two very distinctive flaws to gain remote code execution on the server. A simple privilege escalation will then be carried out resulting in full root access over the target.

The tasks in this room will cover every step of the attack process in detail, including providing possible remediations for the vulnerabilities found.  There are no explicit pre-requisites to cover before attempting this room; however, further resources will be linked at relevant sections should you wish further practice with the topic in question. That said, knowledge of Python and basic hacking fundamentals will come in handy. When in doubt: research!

Firefox is highly recommended for the web portion of this room. Use a different browser if you like, but be warned that any and all troubleshooting notes will be aimed at Firefox.

## CMDs

`$ T_IP="10.10.91.3"`

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
```Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-08 07:03 EDT
Nmap scan report for 10.10.91.3
Host is up (0.048s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE
22/tcp  open  ssh
| ssh-hostkey: 
|   3072 17:9a:c9:93:84:75:ce:b9:c7:06:ff:d2:f2:85:df:f1 (RSA)
|   256 cb:be:13:77:86:ad:3f:12:0f:0d:8b:e6:48:4f:a1:71 (ECDSA)
|_  256 c5:f6:da:7c:c7:b4:4d:f7:1d:29:27:b0:a8:fd:3e:a1 (ED25519)
53/tcp  open  domain
| dns-nsid: 
|_  bind.version: Now why would you need this..?
80/tcp  open  http
|_http-title: Site doesn't have a title (application/octet-stream, text/plain).
443/tcp open  https
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=hipflasks.thm/organizationName=Hip Flasks Inc/stateOrProvinceName=Argyll and Bute/countryName=GB
| Not valid before: 2022-08-08T10:55:30
|_Not valid after:  2023-08-08T10:55:30
|_http-title: Site doesn't have a title (application/octet-stream, text/plain).
| tls-alpn: 
|_  http/1.1
| tls-nextprotoneg: 
|_  http/1.1
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=8/8%OT=22%CT=1%CU=40179%PV=Y%DS=2%DC=I%G=Y%TM=62F0ED9B
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=FE%GCD=1%ISR=102%TI=Z%CI=Z%TS=A)OPS(O1=M50
OS:8ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6
OS:=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF
OS:=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%
OS:Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y
OS:%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%R
OS:D=0%Q=)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%
OS:RUD=G)IE(R=N)

Network Distance: 2 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.06 seconds
```