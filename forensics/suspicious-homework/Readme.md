Run `file` against `homework.zip` to see that it is actually a JPEG.

Research ways to hide data inside JPEG images, and find the popular tool steghide.

Research ways to bruteforce steghide, and find stegseek.

Run stegseek against the file:
```
└─$ stegseek homework.zip     
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "secured1"         
[i] Original filename: "flag.txt".
[i] Extracting to "homework.zip.out".

└─$ cat flag.txt 
SUCTF{brut3f0rc1ng_5t3gh1d3}
```

Flag: `SUCTF{brut3f0rc1ng_5t3gh1d3}`