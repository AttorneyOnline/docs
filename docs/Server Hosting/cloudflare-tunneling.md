# Cloudflare Tunneling

Cloudflare Tunneling is a way to tunnel through a public port on the internet to a private port on the server. In plain english this means to make your AO server reachable to anyone, regardless of where it is running.
As an added bonus, it permits clients to connect via wss (secure websockets), which means it fixes the https issue that many AO servers face.

## Prerequisites

You will need an account and a domain registered in cloudflare. This requires a small yearly payment, but it's quite simple to set up. You can get started with that [here](https://domains.cloudflare.com/).

## Process

For this demonstration, we will use troidtech.com as an example domain.

First, you will want to go to [one.dash.cloudflare.com](https://one.dash.cloudflare.com/)

It might ask you to set up a plan, but just choose the free one. Nothing of this should charge you anything.

Once you've done that, click Tunnels under Networks. It should look something like this:

![Network Tunnels](https://i.imgur.com/HaYxZKG.png)

We will install Cloudflared (not WARP) on the machine we're planning to run the AO server on
with specific instructions for the OS later.
For now, you can move onto the next step, which is clicking Add a tunnel,
which should bring you to a page that looks like this:

![Create a tunnel](https://i.imgur.com/TfvPvRo.png)

Again, choose cloudflared and not WARP. After clicking next, you'll be asked to choose a name for your tunnel,
like this:

![Name the tunnel](https://i.imgur.com/0YoIEZT.png)

You can name it what you want, but if it will be running on a dedicated server, I recommend using the server's hostname,
since you will generally only need one tunnel per machine. But again, it doesn't matter that much.

After clicking "Save tunnel", you'll be asked to run some commands, like this:

![Choose a port](https://i.imgur.com/bPxOspP.png)

Note that the commands will be different for each OS.
Also note that if you have a Ubuntu machine, you will want to use the Debian instructions.
(Ubuntu is a derivative of Debian, so it should work fine.)

One note about Windows: the given command will install cloudflared as a Windows service
(use Win+R to open the search bar and type `services.msc` to open the services control panel).

For Debian/Ubuntu, the instructions with `sudo cloudflared service install...` will install cloudflared as a `systemd` service.
You can check the status of cloudflared by using `systemctl status cloudflared`.

In either case, you may want to look up how these work to ensure you tunnel is running correctly.
Once you start cloudflared, you should see it come up under 'Connectors':

![Cloudflared running](https://i.imgur.com/1v5STux.png)

Then, you can click "Next".
