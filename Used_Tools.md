# Used Tools and cmd used in HackTheBox

- nmap
  - Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing.
- winpeas
  - Windows Privilege Escalation Awesome Scripts
- gobuster
  - Gobuster is a tool used to brute-force:

    URIs (directories and files) in web sites.
    DNS subdomains (with wildcard support).
    Virtual Host names on target web servers.
    Open Amazon S3 buckets.
- mysql
  - mysql is a simple SQL shell (with GNU readline capabilities). It supports interactive and non-interactive
       use. When used interactively, query results are presented in an ASCII-table format. When used
       non-interactively (for example, as a filter), the result is presented in tab-separated format. The output
       format can be changed using command options.
- responder
  - This package contains Responder/MultiRelay, an LLMNR, NBT-NS and MDNS poisoner. It will answer to specific NBT-NS (NetBIOS Name Service) queries based on their name suffix (see: http://support.microsoft.com/kb/163409). By default, the tool will only answer to File Server Service request, which is for SMB.

The concept behind this is to target your answers, and be stealthier on the network. This also helps to ensure that you don’t break legitimate NBT-NS behavior. You can set the -r option via command line if you want to answer to the Workstation Service request name suffix.
- smbclient
  - This tool is part of the samba(7) suite.

       smbclient is a client that can 'talk' to an SMB/CIFS server. It offers an interface similar to that of the
       ftp program (see ftp(1)). Operations include things like getting files from the server to the local
       machine, putting files from the local machine to the server, retrieving directory information from the
       server and so on.
- impacket
  - Impacket is a collection of Python classes for working with network
protocols. Impacket is focused on providing low-level
programmatic access to the packets and for some protocols (e.g.
SMB1-3 and MSRPC) the protocol implementation itself.
Packets can be constructed from scratch, as well as parsed from 
raw data, and the object-oriented API makes it simple to work with 
deep hierarchies of protocols. The library provides a set of tools
as examples of what can be done within the context of this library.
- SecLists
  - Cracking and fuzzing lists.
  - SecLists is the security tester's companion. It's a collection of multiple types of lists used during security assessments, collected in one place. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more. The goal is to enable a security tester to pull this repository onto a new testing box and have access to every type of list that may be needed.
  - common.txt
- evil-winrm
  - This package contains the ultimate WinRM shell for hacking/pentesting.

WinRM (Windows Remote Management) is the Microsoft implementation of WS-Management Protocol. A standard SOAP based protocol that allows hardware and operating systems from different vendors to interoperate. Microsoft included it in their Operating Systems in order to make life easier to system administrators.
- john
  - Password Cracker
  - John the Ripper is an Open Source password security auditing and password recovery tool available for many operating systems.
- Burp Suite
  - A web crawler (also known as a web spider or web robot) is a program or automatedscript which browses the World Wide Web in a methodical, automated manner. This processis called Web crawling or spidering. Many legitimate sites, in particular searchengines, use spidering as a means of providing up-to-date data.If you tunnel web traffic through Burp Suite (without intercepting the packets), bydefault it can passively spider the website, update the site map with all of thecontents requested and thus creating a tree of files and directories without sendingany further requests.


## CMD's

- nc
  - netcat  is  a simple unix utility which reads and writes data across network connections, using TCP or UDP
protocol. It is designed to be a reliable "back-end" tool that can be used directly or  easily  driven  by
other  programs  and  scripts. 
- ftp
  - Ftp is the user interface to the Internet standard File Transfer Protocol.  The program allows a user to
     transfer files to and from a remote network site.
- openvpn
  - The OpenVPN Community Edition (CE) is an open source Virtual Private Network (VPN) project. It creates secure connections over the Internet using a custom security protocol that utilizes SSL/TLS. This community-supported OSS (Open Source Software) project, using a GPL license, is supported by many OpenVPN Inc.