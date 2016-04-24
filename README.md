# pfsense-nfsen

## Pre-install

Make sure that you have the `softflowd` package installed via the pfSense web interface _before_ continuing!

`ncpad` is going to be listening on port 9995, so go ahead and add that port in the `softflowd` settings

## Initial install

We need access to the general FreeBSD package ecosystem; IIRC, I did it like so:

**NOTE** Messing with the package management seems to cut off access to packages via the pfSense web interface -- make sure you have anything that you'll wnat to install, installed, _before_ you do this (ie Squid, Snort, etc)!!!

Change the following line in `/usr/local/etc/pkg/repos/pfSense.conf`:

`FreeBSD: { enabled: no }` -> `FreeBSD: { enabled: yes }`

( can't remember if I had to install `pkg` manually from a remote url; might just be hidden on the system )

Run `pkg update`

Follow instructions for the following sections on [this primer](https://forums.freebsd.org/threads/49724/):
* CONFIGURE NETFLOW STORAGE AND DISPLAY WITH NFDUMP AND NfSen

(I didn't create the /zstore dir, just put everything in /var/nfsen, seems to be fine)


## Setup nfsen/nginx

Replace the following files in the `/etc` directory with those in this repo:
* `/etc/rc.php_ini_setup`
* `/etc/rc.restart_webgui`

Add the `nfsen-nginx.conf` file to `/usr/local/etc/nginx`

Restart php-fpm & the webgui(s):

```
/etc/rc.php-fpm_restart
/etc/rc.restart_webgui
```

## Check that it works

You should be able to navigate in a web browser to `http://<router_ip>:1080/nfsen.php` and see beautiful graphs
