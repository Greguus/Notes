mount -t cifs -o username=<username>,password=<password> //<IP or hostname>/<PATH> <Local PATH>
Permanent mount (fstab):
//<IP or hostname>/<PATH> <Local PATH> cifs username=<username>,password=<password>,iocharset=utf8        0       0  
This is very insecure... anyone can read fstab!!! More secure is creating a credentials file and restricting permissions.