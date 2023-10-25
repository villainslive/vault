---
status: new
---

# Hosting a Darknet Website
Welcome to the dark side of the web, where anonymity meets creativity and where privacy is paramount. In this guide, we'll take you on a journey into the mysterious realm of the Darknet, where you'll learn how to harness its hidden potential to host your very own static website. Step into the shadows, and let's illuminate the path to securing your corner of the deep web!

## Basic Requirements
For this guide, you'll need the following:

1. Machine running some modern Linux distro, our go to is Ubuntu 22.04

2. Internet connection that doesn't block access to the [Tor](https://www.torproject.org/) network

3. The nerve to venture out into the unknown

## Become root on  your machine
If you're not already root, go ahead and take control now
```
sudo -i
```
## Make sure you're up to date
You'll want to make sure your system is fully updated
```
apt-get update -y & apt-get upgrade -y
```

## Installing Nginx & Tor
We'll be using Nginx for our web server since we'll be serving up static content in this guide.

```
apt install nginx tor -y
```

## Open ports in your Firewall
We can't serve any content if nothing can access our server, so fire of these commands to poke a couple of holes to our webserver

1. 80 is the default HTTP port

2. 443 is the default HTTPS port

3. 9050 is the default SOCKS port for Tor

```
iptables -I INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -m state --state NEW -p tcp --dport 9050 -j ACCEPT
```

## Configure Tor
Open up `/etc/tor/torrc` in your favorite text editor, here we'll use nano
```
nano /etc/tor/torrc
```

Add the following lines to this file, then save (Ctrl+X) your changes
```
HiddenServiceDir /var/lib/tor/nginx-tor-service/
HiddenServicePort 80 127.0.0.1:80
HiddenServicePort 443 127.0.0.1:443
```

Enable & restart Tor
```
systemctl enable tor && systemctl restart tor
```

## Configure NGINX
Find your Tor host name, then copy to your clipboard because you'll need it in the next step

```
cat /var/lib/tor/nginx-tor-service/hostname
```

!!! villains ".onion"
    Darknet web addresses are *LONG* and end in `.onion` so be sure to copy and paste this as you don't want to make a typo!

    Here is an example: `vyogq7jhrpclmqlazew3jtwp6esogmyvpx3pmmtvzjspezu3lzdr4iqd.onion`

Open the `/etc/nginx/sites-available/nginx-tor-service` file (it doesn't exist yet, but we're creating it now)
```
nano /etc/nginx/sites-available/nginx-tor-service
```

Paste the following into it, however on line 7, after `server_name` you'll want to swap in your hostname (.onion) from the previous step
``` linenums="1" hl_lines="7"
server {
        listen 80 default_server;
        listen [::]:80 default_server;
 
        root /var/www/html;
 
        server_name vyogq7jhrpclmqlazew3jtwp6esogmyvpx3pmmtvzjspezu3lzdr4iqd.onion;
 
        location / {
                    try_files $uri $uri/ =404;
        }
}
```

Create a symbolic link for your new configuration file to `/etc/nginx/sites-enabled`
```
ln -s /etc/nginx/sites-available/nginx-tor-service /etc/nginx/sites-enabled/
```

Remove the default config file in `/etc/nginx/sites-enabled`
```
rm /etc/nginx/sites-enabled/default
```

Enable & restart NGINX
```
systemctl enable nginx & systemctl restart nginx
```

## Add your content
You'll want to add your content to `/var/www/html`

Lets create an index.html file and add some simple content
```
nano /var/www/html/index.html
```

Copy and paste this and then save (Ctrl-X)
``` html
<html>
  <title>My First Darknet Website</title>
  <body bgcolor="#001100" text="#00FF00">
   <h1> hello darknet!</h1>
  </body>
</html>
```

If you don't already have the Tor Browser installed, grab it from [https://www.torproject.org/download/](https://www.torproject.org/download/) and visit your .onion address to see your new darknet website in all it's anonymous glory


If you run into any trouble, hop into our [discord server](https://vault.villains.live/#discord) for more help.