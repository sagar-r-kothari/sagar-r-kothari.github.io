---
description: 'Start, Stop, Restart Apache Server on macOS, step by step guide'
---

# Start Apache Server

1. Start Terminal
2. Enter following command \(after replacing userNameHere with your user name\)

```text
sudo vi /etc/apache2/users/userNameHere.conf
```

1. Paste following into that file & save.

```text
<Directory "/Users/USERNAME/Sites/">
    Options Indexes Multiviews
    AllowOverride AuthConfig Limit
    Order allow,deny
    Allow from all
</Directory>
```

1. Run following command to start apache server.

```text
sudo apachectl start
```

1. Launch Safari, Chrome, or Firefox.
2. Navigate to "[http://127.0.0.1](http://127.0.0.1)" to verify the server is running, you will see an “It Works!” message.
3. Move your files to `/Library/WebServer/Documents/`

Stop server

```text
sudo apachectl stop
```

Restart server

```text
sudo apachectl restart
```

