Run `file` against `kp`, and see that it is a KeePass database.
```
└─$ file kp
keeper: Keepass password database 1.x KDB, 3 groups, 2 entries, AES, 50000 key transformation rounds
```

Run `keepass2john` to obtain a hash for the file.
```
└─$ keepass2john kp > hash.txt
```

Use `john` with `rockyou.txt` to get the master password.
```
└─$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64])
Cost 1 (iteration count) is 50000 for all loaded hashes
Cost 2 (version) is 1 for all loaded hashes
Cost 3 (algorithm [0=AES 1=TwoFish 2=ChaCha]) is 0 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
pineapple        (kp)     
1g 0:00:00:03 DONE (2024-05-02 10:56) 0.3134g/s 290.9p/s 290.9c/s 290.9C/s wesley..lawrence
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
See that the master password is cracked as `pineapple`

Use `kpcli --kdb kp` and enter the master password.
```
└─$ kpcli --kdb kp            
Provide the master password: *************************

KeePass CLI (kpcli) v3.8.1 is ready for operation.
Type 'help' for a description of available commands.
Type 'help <command>' for details on individual commands.

kpcli:/>
```

Browse through the passwords and find the flag.
```
kpcli:/> dir
=== Groups ===
eMail/
Internet/
Gaming/
kpcli:/> cd Gaming
kpcli:/Gaming> ls
=== Entries ===
0. league acc                                                             
kpcli:/Gaming> show -f 0

Title: league acc
Uname: COOKIEMONSTER123
 Pass: dQw4w9WgXcQ
  URL: 
Notes: SUCTF{r0cky0u_t0_th3_r3scu3}
```

Flag: `SUCTF{r0cky0u_t0_th3_r3scu3}`