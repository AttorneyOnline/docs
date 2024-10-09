# Secure websockets (the hard way)

## Why?

As you might have noticed, practically every web connection to AO servers is unencrypted.
This materializes on the web in the form of the ever-present warning:

![image](https://github.com/Troid-Tech/docs/assets/14360110/bf810697-e88f-4472-a4c3-a11f49db9b7c)

As well as a blaring NOT SECURE in the address bar:

![image](https://github.com/Troid-Tech/docs/assets/14360110/12123741-09d2-43ea-b289-1051f8b745a6)

The modern web strongly prefers being encrypted, so deviating from that
raises some warnings here and there.

There is also the issue of privacy at play. Since the traffic is unencrypted, practically
anyone can eavesdrop (and even modify!) traffic going to and from an AO server. This may be unfortunate for
a number of reasons.

If you want to make joining your server more convenient (no more warning) and secure (encrypted),
you can configure your server to use secure websockets.

Do note that this is a bit comprehensive, but you'll end up with a pretty solid infrastructure
for more advanced setups, such as hosting custom content or other services.

## Prerequisites

Setting this up properly isn't completely trivial and you will need
at least basic knowledge of networking, server administration, command line and certificates.
Don't worry though, we'll only need the basics and it's not as hard as it sounds.

You will also need a handful of things:
- A domain name (eg. example.com, can be bought from a domain registrar)
- A server with a public IP address
- A supported AO server (currently only KFO-Server, though it's a quite modest feature to implement in other servers)

## Overview

Most of what's going to happen is actually _outside_ the AO server, since managing certificates and configuring
them typically isn't handled by application servers. We're basically going to let another program
handle the encryption and then just forward the unencrypted traffic (locally) to the AO server.

<img width="928" alt="Untitled" src="https://github.com/Troid-Tech/docs/assets/14360110/974d375f-a55e-489b-a29b-20a85f4ed0da">

As you can see, we're going to need an intermediate program to handle the encryption and decryption.
We typically call this a "reverse proxy" for reasons we don't need to talk about right now.

For the sake of this example, we'll use these example values:
- Domain name: `coolserver.example.com`
- Server IP: `1.2.3.4`
- Websocket port: `50001`
- Secure websocket port: `50002`
- Webroot: `/var/www/coolserver.example.com` (where the webserver will look for files to serve)
- Certbot folder: `/path/to/certbot/` (where certbot keeps its files)

## Configuring DNS

A lot of the things we're going to do depend on the domain name, so we need to set that up first.

What you want to do is to go to your registrar, make a subdomain (eg. coolserver.example.com)
and point it to your server's IP address. Let's say your server's IP address is 1.2.3.4.

As with many other steps, the details of this depend on your registrar, but they typically have a web interface
where you can manage your domain's DNS records. You want to add an A record that points to your server's IP.

You can test that this works correctly by using [dnschecker.org](https://dnschecker.org/) or similar tools.

## Setting up the webserver

The first thing we need to do is to set up a webserver which will act as a reverse proxy.
For this guide, we'll use nginx. Installing nginx is a bit different depending on your operating system,
but their [tutorial](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)
should be a good place to start.

When you've set it up and can see the "Welcome to nginx!" page, this step is done.
Don't worry though, we'll have plenty of fun configuring it later.

(Once you've set up nginx you can also use it to host custom content,
but that's a topic for another guide.)

## Getting a certificate

The first thing we need to encrypt our traffic is a certificate. This is basically a file that
proves that we are, in face, example.com and not evilexample.com. That's why we need the domain name.

There are a number of ways to get a certificate, but the most common way is to use Let's Encrypt,
since it's free and fairly simple to set up. They have a tool called [certbot](https://certbot.eff.org/),
which is what we're going to use. Now, the details of how to set up certbot depends a bit on your
operating system, but the key point to reach is being able to run `certbot renew` without errors.
You may need to use the webserver to prove that you're coolserver.example.com, but certbot will guide you through that.

You can test that this works correctly by running `certbot certificates` and seeing that you have a certificate for coolserver.example.com,
as well as `certbot renew` not giving any errors. You should also know _where_ the certificate is stored, since we'll need that later.

## Setting up the webserver (again)

This time around, we'll need to configure nginx to use the certificate and serve a webpage over https,
to check that everything is set up correctly.

The tidiest way to configure a new "site" for nginx is simply to make a new file in the `/sites-available/`
folder, which you may need to search for. Let's put a file here called `coolserver.example.com` with the following content:

```nginx
server {
        listen 443 ssl;
        server_name coolserver.example.com;

        ssl_certificate /path/to/certbot/config/live/coolserver.example.com/fullchain.pem;
        ssl_certificate_key /path/to/certbot/config/live/coolserver.example.com/privkey.pem;

        location / {
                root /var/www/coolserver.example.com;
                index index.html;
        }
}
```

You can then create a simple index.html file in `/var/www/coolserver.example.com`.
The contents of this file can just be something like "Hello, world!", it doesn't matter.


You can test that this works correctly by attempting to access a this webpage over https.
(eg. https://coolserver.example.com/index.html).

You'll know that it works correctly when the webpage loads and your browser doesn't look ready to collapse
out of terror, anger, confusion or any combination of the three.

## Setting up the webserver (but actually this time)

Now that we have a working webserver, we can set up the reverse proxy.

We'll need to modify the `coolserver.example.com` config file
(you might want to take a backup of the old one)
file to look like this instead:

```nginx
server {
        listen 50002 ssl;
        server_name coolserver.example.com;

        ssl_certificate /home/dskoland/tools/certbot/config/live/coolserver.example.com/fullchain.pem;
        ssl_certificate_key /home/dskoland/tools/certbot/config/live/coolserver.example.com/privkey.pem;

        location / {
                proxy_pass http://localhost:50001;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "Upgrade";
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
```

Note that the config is quite similar, but instead of serving a file, we are passing
along the traffic to the AO server (who is hopefully listening on port 50001). This way,
the AO server sees the connection as if it was a regular, unencrypted websocket connection.

To test if this works correctly, make sure the AO server is running and then try to connect via wss
by using a URL like this:
```
https://web.aceattorneyonline.com/client.html?mode=join&connect=wss://coolserver.example.com:50002&serverName=CoolServer
```

Do note that in connect it says `wss` instead of the usual `ws`. This is because it's a secure websocket.
The website itself (web.aceattorneyonline.com) is also served over https, so it's not going to complain about
being "not secure" or other nonsense.

## Advertising secure sockets

In order for clients to know that your server supports secure websockets, you need to advertise it in the server list
as such. You can do this by adding the `secure-websockets-port` in your config.yml.
In this example, it should be 50002.

You'll know that this works correctly once your server shows up in the serverlist and upon clicking on it,
we connect securely.

## Trouble?

If you're having serious trouble with this process, you can always ask for help in the #tech_support channel in the
[official Discord server](https://discord.gg/wWvQ3pw).
