# Cloudflare Tunneling

Cloudflare Tunneling is a way to tunnel through a public port on the internet to a private port on the server. In plain english this means to make your AO server reachable to anyone, regardless of where it is running.
As an added bonus, it permits clients to connect via wss (secure websockets), which means it fixes the https issue that many AO servers face.

## Prerequisites

You will need an account and a domain registered in cloudflare. This requires a small yearly payment, but it's very simple to set up. You can get started with that [here](https://domains.cloudflare.com/).

## Process

For this demonstration, we will use troidtech.com as an example domain.

First, you will want to go to [one.dash.cloudflare.com](https://one.dash.cloudflare.com/)

It might ask you to set up a plan, but just choose the free one. Nothing of this should charge you anything.

Once you've done that, click Tunnels under Networks. It should look something like this:

![Network Tunnels](https://i.imgur.com/5BYxQj2.png)

