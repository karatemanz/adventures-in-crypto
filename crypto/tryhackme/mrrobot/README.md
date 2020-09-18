# Mr. Robot CTF

**Simply Hack the Machine is the ask**

## Recon

1. nmap => saved initial file
2. gobuster => saved initial file
3. from gobuster directories found /rdf which generated a download file which contained
```
user&#039;s // Possible Wordpress User
```
4. from gobuster /robots was found showing:
```
User-agent: *
fsocity.dic
key-1-of-3.txt
```
5. these files can be downloaded with wget; for example:
```
wget $IP/fsocity.dic
```
6. downloaded both files fsocity.dic appears to be a word dictionary and key-1-of-3.txt is our first key.
7. [HackerSploit](https://www.youtube.com/watch?v=0VJyfJzbPE4) Recommended since the dictionary is so large that cleaning the dictionary up will help the attack to be much faster. Using the following ensures the file is unique and sorted before using it for a brute force attack.
```
cat fsocity.dic | sort -u | uniq > fsclean.dic
```
8. HackerSploit also mentioned that due to the fact the the SSL for this site is expired for the wordpress page means the brute force attack will be easier to conduct (TODO RESEARCH THIS)

9. Captured Login Request with burpsuite (wp-brute/login-request.txt) same request as curl here

```
curl -i -s -k -X $'POST' \
    -H $'Host: 10.10.115.77' -H $'Content-Length: 98' -H $'Cache-Control: max-age=0' -H $'Upgrade-Insecure-Requests: 1' -H $'Origin: http://10.10.115.77' -H $'Content-Type: application/x-www-form-urlencoded' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9' -H $'Referer: http://10.10.115.77/wp-login.php' -H $'Accept-Encoding: gzip, deflate' -H $'Accept-Language: en-US,en;q=0.9' -H $'Cookie: wordpress_test_cookie=WP+Cookie+check' -H $'Connection: close' \
    -b $'wordpress_test_cookie=WP+Cookie+check' \
    --data-binary $'log=test&pwd=123&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.115.77%2Fwp-admin%2F&testcookie=1' \
    $'http://10.10.115.77/wp-login.php'
```


