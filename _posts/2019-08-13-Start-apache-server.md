---
layout: post
title: "Start a web server on macOS"
date: 2019-08-13 05:00:00 +0530
categories: apache macOS server
---

1. Start Terminal
2. Enter following command (after replacing userNameHere with your user name)

```
sudo vi /etc/apache2/users/userNameHere.conf
```

3. Paste following into that file & save.

```
<Directory "/Users/USERNAME/Sites/">
	Options Indexes Multiviews
	AllowOverride AuthConfig Limit
	Order allow,deny
	Allow from all
</Directory>
```

4. Run following command to start apache server.

```
sudo apachectl start
```

5. Launch Safari, Chrome, or Firefox.
6. Navigate to "http://127.0.0.1" to verify the server is running, you will see an “It Works!” message.
7. Move your files to `/Library/WebServer/Documents/`


Stop server

```
sudo apachectl stop
```

Restart server

```
sudo apachectl restart
```