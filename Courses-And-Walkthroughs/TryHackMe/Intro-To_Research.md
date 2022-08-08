# Intro To Research

**[Link to the room.](https://tryhackme.com/room/introtoresearch#)**

## Task 1.
Not much to say, just connecting to TryHackMe.

## Task 2.
- Example research question, with the example being analyzing a picture for hidden information.
- [Steganography](https://0xrick.github.io/lists/stego/)
- It also has research questions, which helps to train the research skills.

**Questions**

- In the Burp Suite Program that ships with Kali Linux, what mode would you use to manually send a request (often repeating a captured request numerous times)?
    - Repeater
- What hash format are modern Windows login passwords stored in?
    - NTLM
- What are automated tasks called in Linux?
    - cron jobs
- What number base could you use as a shorthand for base 2 (binary)?
    - base 16
- If a password hash starts with $6$, what format is it (Unix variant)?
    - SHA512crypt

## Task 3.
If you're inclined towards the CLI on Linux, Kali comes pre-installed with a tool called "searchsploit" which allows you to search ExploitDB from your own machine. This is offline, and works using a downloaded version of the database, meaning that you already have all of the exploits already on your Kali Linux!

- [Exploit DB](https://www.exploit-db.com/)

**Questions:**

- What is the CVE for the 2020 Cross-Site Scripting (XSS) vulnerability found in WPForms?
    - CVE-2020-10385
- There was a Local Privilege Escalation vulnerability found in the Debian version of Apache Tomcat, back in 2016. What's the CVE for this vulnerability?
    - CVE-2016-1240
- What is the very first CVE found in the VLC media player?
    - CVE-2007-0017
- If you wanted to exploit a 2020 buffer overflow in the sudo program, which CVE would you use?
    - CVE-2019-18634


## Task 4.

**Questions**

- SCP is a tool used to copy files from one computer to another.
What switch would you use to copy an entire directory?
    - `-r`
- fdisk is a command used to view and alter the partitioning scheme used on your hard drive. What switch would you use to list the current partitions?
    - `-l`
- Nano is an easy-to-use text editor for Linux. There are arguably better editors (Vim, being the obvious choice); however, nano is a great one to start with. What switch would you use to make a backup when opening a file with nano?
    - `-B`
- Netcat is a basic tool used to manually send and receive network requests. What command would you use to start netcat in listen mode, using port 12345?
    - `nc -l -p 12345`
 