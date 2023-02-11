# Footprinting

## Infrastructure-based Enumeration

### Domain Information

| **Commands** |  **Description**  |
| --------------|-------------------|
| [crt.sh](https://crt.sh/) | Online subdomain finder |
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | Certificate transparency. |
| `for i in $(cat ip-addresses.txt);do shodan host $i;done` | Scan each IP address in a list using Shodan. |
| `dig any inlanefreight.com` |  DNS Records |

### Cloud Resources

| **Commands** |  **Description**  |
| --------------|-------------------|
| [Domain.glass](https://domain.glass/) | Third-party providers such as domain.glass can also tell us a lot about the company's infrastructure. |
| Wappalyzer | Extension |
| [Gray](https://buckets.grayhatwarfare.com/) | Another very useful provider is GrayHatWarfare. We can do many different searches, discover AWS, Azure |

## Host-based Enumeration

### FTP

|**Command**|**Description**|
|-|-|
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Interact with the FTP service on the target using encrypted connection. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files on the target FTP server. |
| `get` | To download a file |
| `put` | To upload a file |
| `find / -type f -name ftp* 2>/dev/null | grep scripts` | Nmap FTP Scripts |

### SMB

|**Command**|**Description**|
|-|-|
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB and to see available shares |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | SMB enumeration using enum4linux. |
| `samrdump.py 10.129.14.128` | Impacket - Samrdump.py |
