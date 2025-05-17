
# Lab Report: HTML and FTP Transfer from Windows 10 to Ubuntu Server

## Objectives

- Install Notepad++ on Windows 10 VM
- Create a basic HTML5 page and add extra elements to it
- Add presentation layer (CSS) to the HTML5 page
- Install FTP Server on the Ubuntu VM
- Install FTP FileZilla Client on Windows 10 VM to transfer files to the Ubuntu Web Server

---

## Creating HTML Document on Windows 10

1. Installed Notepad++.
2. Created a basic HTML5 page with the following content:

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>My Basic HTML5 Page</title>
<style type="text/css">
h1 { font: 40px Arial; color:#0000FF; }
div { font-family; Courier color; red}
</style>
</head>
<body>
<h2>Hello World!</h2>
<p>My username is: <b>ealao</b></p>
</body>
</html>
```

3. Saved the document and loaded it in a browser.

*...Add Image...*

---

## Setting up vsFTPD Server

1. Installed vsFTPD:

```bash
sudo apt-get install vsftpd
```

2. Verified port 21 is active:

```bash
netstat -tuna
```

3. Tested connectivity via telnet from Windows 10:

```cmd
telnet 10.0.0.200 21
```

4. Backed up original config:

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.BACK
```

5. Edited configuration:

```bash
sudo nano /etc/vsftpd.conf
```

Added or modified:

```ini
anonymous_enable=NO
local_enable=YES
write_enable=YES
allow_writeable_chroot=YES
chroot_local_user=YES
file_open_mode=0777
local_umask=022
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
```

6. Created FTP user:

```bash
sudo adduser FOLusername-ftp
```

Password: `Ubuntu1`

7. Created FTP directory and set permissions:

```bash
sudo mkdir /var/www/html/FOLusername-ftp
sudo chown nobody:nogroup /var/www/html/FOLusername-ftp
sudo chmod a-w /var/www/html/FOLusername-ftp
```

8. Verified permissions:

```bash
sudo ls -la /var/www/html/FOLusername-ftp
```

9. Created test file:

```bash
sudo mkdir -p /var/www/html/FOLusername-ftp/files
echo "testing FTP access" | sudo tee /var/www/html/FOLusername-ftp/files/ftp_test.txt
```

10. Verified test file:

```bash
cat /var/www/html/FOLusername-ftp/files/ftp_test.txt
```

11. Added user to FTP user list:

```bash
echo "FOLusername-ftp" | sudo tee -a /etc/vsftpd.userlist
cat /etc/vsftpd.userlist
```

12. Restarted vsftpd:

```bash
sudo systemctl restart vsftpd
```

---

## Setting Up FileZilla on Windows 10

1. Downloaded and installed FileZilla.
2. Attempted connection with:

```
Host: 10.0.0.200
Username: anonymous
```

Received TLS warning.

3. Connected with designated user:

```
Host: 10.0.0.200
Username: FOLusername-ftp
Password: Ubuntu1
```

Observed empty directory listing `/`.

*...Add Image...*

---

## Configuring Directory Access

1. Edited `vsftpd.conf`:

```ini
user_sub_token=$USER
local_root=/var/www/html/$USER/files
```

2. Restarted vsftpd:

```bash
sudo systemctl restart vsftpd
```

3. Logged in again via FileZilla:

```
Host: 10.0.0.200
Username: FOLusername-ftp
Password: Ubuntu1
```

Successfully accessed and viewed `ftp_test.txt`.

*...Add Image...*
