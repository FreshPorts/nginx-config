# nginx-config

These files exist at `/usr/local/etc/freshports/` and are installed by the
`freshports-www` package. Each file is installed with a `.sample` suffix so
as to not overwrite any existing cofiguration on upgrades.

Separate files are used to avoid duplicating the same content in multiple
places. I know some claim this makes the configuration more complex. For me,
it keeps thing simple.

How do I use this?

My `nginx.conf` file contains this line:

```
include /usr/local/etc/nginx/includes/*.conf;
```

This pulls in any files which match that glob. In my case, I pull in only
`*.conf` files from that directory.

In that directory, a single symlink is created which links to the
configuration file.

```
[dan@dev-nginx01:/usr/local/etc/nginx/includes] $ ls -l
total 1
lrwxr-xr-x  1 root  wheel  37 Aug  4 04:24 freshports.org.conf -> /usr/local/etc/freshports/vhosts.conf
lrwxr-xr-x  1 root  wheel  55 Jan 26  2019 freshsource.org.conf -> /usr/home/dan/src/freshsource/configuration/vhosts.conf
[dan@dev-nginx01:/usr/local/etc/nginx/includes] $ 
```
