# TryHackMe Attack Flow

## Using Nmap

Running initial analysis on host and saving results for reference.
```
nmap -sC -sV -oN nmap/initial $IP
```

Against a host that's not ping-able
```
nmap -Pn -sC -sV -oN nmap/initial $IP
```