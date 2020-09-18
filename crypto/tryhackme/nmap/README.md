# Learning nmap Notes

## Helpful nmap commands

```
nmap -h // help
nmap -sS <ip or hostname> // stealth scan (Syn Scan)
nmap -sU <ip or hostname> // UDP scan
nmap -O <ip or hostname> // Operating system info scan
nmap -sV <ip or hostname> // Service version scan
nmap -v <ip or hostname> // Verbose mode
nmap -vv <ip or hostname> // Very-Verbose mode
nmap -oX <ip or hostname> // XML formatted output
nmap -A <ip or hostname> // Aggressive scan mode
nmap -Pn <ip or hostname> // don't ping the host
```

## nmap Port Scanning

```
nmap <ip or hostname> // DEFAULT: scans ports up to 1000?
nmap -p <port> <ip or hostname> // scan specific port
nmap -p- <ip or hostname> // scan all ports
```


## nmap Timing

```
nmap timing -T<number>
(0) => 
(1) =>
(2) => 
(3) =>
(4) => 
(5) => Insane
```

## nmap Scripts (written in LUA)

You are capable of running built in scripts either directly, by category, by pattern matching on filename, and a variety of other options

```
nmap --script <*category> <script-name(s)>
```

# Common services/ports to look for with nmap
```
most communication protocols done with tcp or udp
(80) => http
(443) => https
(22) => ssh
() => smb (samba share)
() => ftp (file transfer)
() => smtp (email)
```