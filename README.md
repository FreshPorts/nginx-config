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

## vhosts.conf

This is the file included by `nginx` - it is usually symlinked to a file in
the Nginx` includes directory (`/usr/local/etc/nginx/includes`).

Ideally, this is the only file which contains host-specific details such as:

* IP address
* `server_name`
* SSL certificate keys
* redirects from `http` to `https`

## virtualhost-common.conf

The directives in this file are website-specific but not host-specific.

By website, I mean FreshPorts.

By host-specific, I refer to dev.freshports.org, test.freshports.org,
stage.freshports.org, etc. These are the different hosts which run
FreshPorts. These directives are the same on every host.

## virtualhost-common-ssl.conf

This specifies the SSL settings.

In theory, this file could be used by all the virtual hosts on this web
server. It contains no host-specific settings (such as IP address, `location`,
or `server_name`).

## php-processing.conf

This contains the directives for invoking PHP.

# Things under consideration

You will notice that `vhosts.conf` contains a server entry for both
`freshports.org` and `www.freshports.org` both of which are running ssl.

I don't know why.  I don't know why I don't have one `server_name` with both
host names. That's what is done with the `http` host at the top of the file.
