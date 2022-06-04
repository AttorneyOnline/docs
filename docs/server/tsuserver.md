# Hosting with tsuserver3

tsuserver3 is, currently, the only choice for simple server hosting. It’s a fast, easy-to-setup server program that runs on Python. Most servers use it now, so commands and behavior will be very familiar when you switch from server to server.

## Installing Python

Step one is to install the latest version of Python 3. The default settings will work fine, but you need to **select Add Python 3 to PATH**, like this fellow demonstrates:

(TODO illustration)

(yes this screenshot is outdated -- yours is probably going to be a much newer version)

Now visit the [tsuserver3 GitHub](https://github.com/AttorneyOnline/tsuserver3) and download the tsuserver3 zip with the "Clone or download" button. Extract it to your favorite folder. **Do not put anything from AO in the server folder.**

You may wish to read the readme on GitHub for instructions on how to set up tsuserver and its prerequisites (as this documentation is not complete).

Once that’s done, your goal is ultimately to get a “Server started” message. But before that...

## Configuring the server

You probably noticed that the readme mentioned something about editing the values of the configs to your liking. Sure enough, now that you have your own server, you have to customize it. Nobody wants another copy of Official Unofficial, do they?

So open your freshly-renamed config folder and find six YAML files. You can open them with any normal text editor. Some of their options are self-explanatory; some of them are more vague. All of them are very particular. Follow the format strictly or it won’t work. On names and other strings that contain symbols, put ‘single quotes’ around them to prevent pyYAML errors. Never use tab; indents in YAML are done with spaces only.

The following is a reference of each config file and what they specify:

### `config.yaml`

Fortunately, this config comes with comments.

#### Basic

- `hostname`: Name that the server goes by when sending responses in the server OOC chat. Users will get an error when attempting to use this name, so that everyone knows server messages are actually coming from the server. <dollar> is a universal AO placeholder for the $ sign and Hostnames often start with it.
- `motd`: The server sends this message in OOC chat when you connect.
- `playerlimit`: Total amount of players that can connect, across all areas.
- `port`: The one you forwarded. Make sure it matches the port-forward rule you set up.
- `local <true/false>`: Makes the server listen on your computer only. Keep false if you want others to be able to connect.
- `modpass`: The password you /login with to become moderator, which gives you access to powerful mod commands. Keep this secure.
  - This advanced configuration allows you to set multiple passwords and give them to different users. Put it in the line under
modpass:
  ```yaml
    mod1:
      password: 'one password'
    mod2:
      password: 'a different password'
  ```

#### webAO

- `use_websockets <true/false>`: Whether the server will open itself up for webAO users.
- `websocket_port`: The port that webAO users will connect with; cannot be the same as the normal port. This means if you’re using websockets you need to forward two different ports.

#### Master server options

- `use_masterserver <true/false>`: Whether the server will add itself to the public server list when starting up. Set false for a private server. Your friends will then have to add entries in serverlist.txt with your server’s ip, port, and name.
- `masterserver_ip`: Keep this as master.aceattorneyonline.com
- `masterserver_port`: Keep this as 27016
- `masterserver_name`: The name your server will have on the public server list.
- `masterserver_description`: The description your server will have in the description box.

#### Flood guards

- `music_change_floodguard`/`wtce_floodguard`: Settings to stop music and judge sign spam, respectively:
  - `times_per_interval`: Amount of requests in an interval of time required to trigger mute, stopping them from changing music/using judge signs.
  - `interval_length`: Length in seconds of the interval.
  - `mute_length`: Length in seconds of the mute.

#### Internal use

- `timeout`: How long a client can stay on without sending a keepalive packet. Guards against background Internet traffic and zombies. Keep at the default value (250) unless you know what you’re doing.
- `debug <true/false>`: Turns on advanced logging in debug.log, which is used to diagnose problems. I would keep it true for long sessions unless you have space limitations, so you don’t miss anything when a problem occurs.


### `areas.yaml`

Your chatrooms where all the fun goes down. Configured with a series of area entries that start with their name.

- `area`: The area’s name.
- `background`: The name of the background that this area will start with. Should be one of the options in backgrounds.yaml.
- `bglock <'true'/'false'>`: Default setting for this area’s background lock, which sets whether or not anyone can come in and change the background. HAS to be in 'single quotes' to apply correctly!
- `evidence_mod`: Special keyword that determines who can change evidence in this area. See the readme.
- `locking_allowed <true/false>`: Whether users can lock this area for themselves and their buddies.
- `iniswap_allowed <true/false>`: Whether players can use the iniswapping “glitch” in this area, or send emotes not found in the characters/ folder (see below)

### `backgrounds.yaml`

A list of background folder names that will be available for switching between. The actual background that each area starts with is set in `areas.yaml`.

### `characters.yaml`

A list of character folder names that will be loaded into your server’s character menu.

### `iniswaps.yaml`

Exceptions to the iniswap_allowed setting. Shows which characters can be swapped out for each other.

### `music.yaml`

Basically, a list of file names to go into the music list. tsuserver is fancy and does them in categories. For your own safety, put ‘single quotes’ around each song and category name to avoid errors.

- `category`: Text in the red line inside the music list that each of the following songs will appear under. You can put any `--~~||formatting||~~--` you want to distinguish them further.
- `songs`: Paired -name and length entries for each song that goes into the category:
- `name`: Name of the .mp3 file as it appears in sounds/music in your server’s contents, e.g. ‘Annonce the Truth(T&T).mp3’
- `length`: Length in seconds of the song, before it loops again. Set to 0 for no looping.

## Using the `characters/` folder

tsuserver can now check if the emotes that players use in their messages are valid. If you give tsuserver a characters/ folder (like in the client) full of your char.inis, messages with invalid emotes will be rejected. Any characters that don’t have a char.ini placed here and appear in your characters.yaml will be free to use any emote they have in their ini. You should already have the vanilla version of this folder with tsuserver.

To add new characters, you don't need to copy over the animations as well - just the `char.ini` files.

## Troubleshooting
    
If you double click `start_server.py` and it immediately closes on you, it’s probably a configuration problem. However, to be more certain about what the problem is, you can run it from the command line with `py -3 start_server.py` to get the Python error output. This will tell you where the problem is coming from and what exactly it is. If you don’t understand the error, you can get help in the Discord server.

---

_This section was adapted from Trey's_ [How to Server](https://docs.google.com/document/d/1xOd7SDCBmSA0RM8M7JX7xACA4YUsQn72Bob9HBOzhVM/edit#) _guide._