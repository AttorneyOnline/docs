# Network Specification  #

The following document covers how 1.7.5, 1.8 and 2.3.0+ versions of AO clients connect and communicate with servers.

## Legend ##

AO = Attorney Online  
char = character, as in the game character, not the char datatype  
type(x) = an array of type separated by x: typextypextype  
C = client  
S = server  
[x] = x is an optional argument(usually used by version 2.3.0+)  
{x} = x is encrypted using the "fantacrypt" algorithm  

# FantaCrypt #

AO1.x encrypted the headers of the packets sent by the client to the server.
FantaCrypt works by seeding a PRNG with a known value (the decryption key).
The lower two bytes are the only ones that are used in the PRNG.
Additionally, only the upper byte of those two bytes are used in the cipher.
The current key is XOR'ed with the current byte.
To get the key value for the next byte, the byte and key are added together.
Then, some multiplication and addition is done to that value.
This PRNG behavior is similar to a Linear Congruential Generator.
Finally, everything but the lower two bytes are discarded.
However, there was a major flaw in the implementation of FantaCrypt.
The server supplies the client with the initial key to use for every packet.
Thus, it is totally vulnerable to a replay atttack.
Many clients do not actually even implement the FantaCrypt algorithm, but instead
they use a hardcoded key of 5, which is sent as 0x34 in the 'decryptor' packet.
This is because the 'decryptor' packet argument is the key to be used, but encrypted.
The decryptor value is always encrypted with a magic number key value: 322 decimal.
FantaCrypt is only used in messages sent by the client to the server, as well.

# Handshake #

The handshake is largely the same regardless of server and client version.  
  
C: (ordinary TCP handshake)  
S: **decryptor#{<decryptor: hex>}#%** (the argument is decrypted using **unsigned integer** 322 as decryption key)  
(all headers from the client are encrypted from here on)*  
C: **HI#<unique_hardware_id: string>#%**  
S: **ID#<player_id: int>#<software: string>[#<version: int.int.int>]#%** (player id is largely unused)  
[C: **ID#<software: string>#<version: int.int.int>#%**]  
S: **PN#<player_count :int>#<max_players: int>#%**  
[S: **FL#<feature1: string>#...#<featureN: string>#%**]  
  
*: not always true, if the server sends noencryption as an argument in the FL packet, headers are no longer encrypted(2.3.1+ only)  

# Loading #

Loading is the process where the server sends lists of its characters, music and sometimes evidence to the client. A prerequisite of loading is a successful handshake with a server.

## Loading 1.0 ##

Used by versions 1.8 and 1.7.5, this loading is very inefficient and suffers heavily from latency. Can also be used by 2.3.0+ in cases where the server does not support fast loading.  
  
C: **askchaa#%**  
S: **SI#<char_list_length: int>#<evidence_list_length: int>#<music_list_length: int>#%**  
C: **askchar2#%**  
S: **CI#<char_id: int>#<char_name: string>&<description: string>&<?: int>&<evidence: int(,)>&&<?: int>&#...#%**  (1-10 in every packet, continued in the ...)    
C: **AN#1#%**  
S: **CI#...#%**  
C: **AN#2#%**  
...  
S: **EI#<evidence_id: int>#<evidence_name: string>&<evidence_description: string>&1&<evidence_image_name: string>&##%**  (evidence loads one at a time)  
C: **AE#1#%**  
S: **EI#...#%**  
C: **AE#2#%**  
...  
S: **EM#<music_id: int>#<music_name: string>#...#% (music is also loaded in packs of 10)**  
C: **AM#1#%**  
S: **EM#...#%**  
C: **AM#2#%**  
...  
S: **CharsCheck#<char_taken: int>#...#%** (same size as character list, 0 = free, -1 = taken)  
S: **OPPASS#{<mod_pass: hex>}#%** (an obsolete artifact of the disastrous security of old AO servers, still used on AO clients 1.8 and lower to display a guard checkbox in the client)  
S: **DONE#%**  

A sample can be found [here](https://github.com/Attorney-Online-Engineering-Task-Force/Documentation/blob/master/Samples/Handshake%20%2B%20Loading%201.0%20Sample)
  
## Loading 2.0 ##

Designed to be *much* faster than Loading 1.0, most noticably on slow connections.

C: **askchaa#%**  
S: **SI#<char_list_length: int>#<evidence_list_length: int>#<music_list_length: int>#%**  
C: **RC#%**  
S: **SC#<char_name: string>[&<char_description: string>]#...#%** (contains the entire server character list)  
C: **RM#%**  
S: **SM#<music_name: string>#...#%** (same as characters)  
C: **RD#%**  
S: **CharsCheck#<char_taken: int>#...#%** (same size as character list, 0 = free, -1 = taken)  
S: **OPPASS#{<mod_pass: hex>}#%** (again, not the actual modpass)  
S: **DONE#%**  
  
What's important to note here is that since the character and music lists often are way too large for a single packet, TCP chops it into smaller ones. It is the client's responsiblity to piece them together on their end.(TCP makes sure that the segments will be handled in the correct order)  
  
A sample can be found [here](https://github.com/Attorney-Online-Engineering-Task-Force/Documentation/blob/master/Samples/Handshake%20%2B%20Loading%202.0%20Sample)

# Character picking

C: **CC#<player_id: int>#<char_id: int>#<unique_hardware_id: string>#%**  (there's strictly speaking no need to verify player id serverside)  
S: **PV#<player_id:  int>#CID#<char_id: int>#%** (the server indicates that a character was successfully picked)
# Messaging

For the most part, communication between clients happens this way. There are chiefly two message types, IC messages and OOC messages.  
  
## IC messages

IC messages, (abbreviation for "In-character") are the kind of messages sent with a character, an animation and many other modifiers. There is a total of 14 arguments, which makes the packet a bit complex. To make this a bit more manageable, there is a redundant newline for every argument. These are not present in the actual packet.

C:
**MS#  
chat/0/1# (0 = no desk, 1 = desk)  
<pre_emote: string>#  
<character: string>#  
<emote: string>#  
<message: unicode_string>#  
<side: string>#  
<sfx-name: string>#  
<emote_modifier: int>#  
<char_id: int>#  
<sfx-delay: int>#  
<shout_modifier: int>#  
<evidence: int>#  
<char_id2/flip: int>#  
<realization: int>#  
<text_color: int>#  
[<showname: string>]#  
[<other_charid: int>]#  
[<self_offset: int>]#  
[<noninterrupting_preanim: int>]#%**  

S: 
**MS#  
chat/0/1# (0 = no desk, 1 = desk)  
<pre_emote: string>#  
<character: string>#  
<emote: string>#  
<message: unicode_string>#  
<side: string>#  
<sfx-name: string>#  
<emote_modifier: int>#  
<char_id: int>#  
<sfx-delay: int>#  
<shout_modifier: int>#  
<evidence: int>#  
<char_id2/flip: int>#  
<realization: int>#  
<text_color: int>#  
[<showname: string>]#  
[<other_charid: int>]#  
[<other_name: string>]#  
[<other_emote: string>]#  
[<self_offset: int>]#  
[<other_offset: int>]#  
[<other_flip: int>]#  
[<noninterrupting_preanim: int>]#%**  

The following features have been added in 2.6 when the Case Café Custom Client features had been merged:

- `showname`: If used, this will show a custom showname for the character.
- `other_charid`: The character ID of the person the users wishes to pair up with.
- `other_name`: This is actually the literal name of the character -- that is, its folder. This is sent over in case the user's pair is iniswapping.
- `other_emote`: The emote the user's pair was doing. Note that by default, zooms (that are correctly defined as such) do not update this value, so a pair of zooms will not appear. Zooms also enjoy special privileges, in that (assuming they are correctly defined, again) they make the pair disappear in the client, and get centered.
- `self_offset`: the percentage, going from `-100` (one whole screen's worth to the left) to `100` (one whole screen's worth to the right).
- `other_offset`: The user's pair's `self_offset`, basically.
- `other_flip`: The user's pair's `char_id2/flip`, basically.
- `noninterrupting_preanim`: If `1`, the preanimation plays at the same time as the text goes.

Note that servers may overwrite specific values of this message. For example, disemvoweling and shaking modify your text, and `/force_nonint_pres` forces your preanims to be noninuterrupting.
  
For more information on valid arguments and client behavior, see [this](https://github.com/Attorney-Online-Engineering-Task-Force/Attorney-Online-Client-Remake/wiki/In-character-chat-messages%5BMS%5D)

## OOC messages

OOC messages, (abbreviation for "Out-of-character") are simple messages with a name and message associated with them. They're used by servers to convey information and by users to not interrupt ongoing gameplay.  
  
**CT#<name: string>#<message: string>#[<is_from_server: int>]#%**

The `is_from_server` packet piece is an addition that arrived with 2.6. 
If it is a `1`, the message sender's name is coloured with the `ooc_server_color` as defined in the current theme. 
Else, it is coloured with `ooc_default_color`. 
This is used to make pretending to be the server and leaving messages in its name harder.

Generally, only the server adds the `is_from_server` part. It does not care if the client adds it, it will just ignore that.

## Escape Codes

Escape codes allows characters like '#' to be sent in messages.

'%' = `<percent>`  
'#' = `<num>`  
'$' = `<dollar>`  
'&' = `<and>`  

# Music

C: **MC#<songname: string>#<char_id: int>#[<showname: string>]#%**  
S: (same)

Some servers support a looping feature where a song is played continuously, without receiving client music requests.

In 2.6, a `showname` piece is added to the end of this packet. This is for the chatlog to be able to correctly store and show who changed the music, so that it does not end up confusing when it does not refer to the user's showname, but their character's name.

# Area Switching

C: **MC#<areaname: string>#<char_id: int>#%**

The MC packet is reused for this purpose. Once received the server will send 2 HP's, 1 BN, and 1 LE packets, to load the area info. After that it will start sending messages from the new area. If the area does not exist the packet is ignored.

## Area updates

In 2.6, there is one further packet concerning areas.

S: 
**ARUP#0#<area1_p: int>#<area2_p: int>#...#%  
ARUP#1#<area1_s: string>##<area2_s: string>#...#%  
ARUP#2#<area1_cm: string>##<area2_cm: string>#...#%  
ARUP#3#<area1_l: string>##<area2_l: string>#...#%**  

Where the first argument is:

- `0`, if it talks about playercount,
- `1`, if it talks about the area's status,
- `2`, if it talks about the CM(s) of the area, and
- `3`, if it talks about the area being locked or not.

This packet has as many pieces (plus one) as there are areas on the server. It describes, in order, every area's given property. As an example, a packet of `ARUP#0#4#3#7#2#0#0#%` would mean that:

- There are 4 players in the first area (or in the zeroth area, actually)
- There are 3 in the second,
- There are 7 in the third,
- There are 2 in the fourth,
- And 0 in the last two.

# Casing

In the 2.6 update, a few packets have been introduced to supply casing needs.

## Case preferences update

This packet updates the user's casing alert preferences.

C: **SETCASE#<caselist: string>#<will_cm: int>#<will_def: int>#<will_pro: int>#<will_judge: int>#<will_jury: int>#<will_steno: int>#%**

Where the arguments are:

- `caselist`: The list of cases this user is willing to host (assuming they are also willing to CM) (not used currently),
- `will_cm`: `1` if the user is willing to host cases (not used currently),
- `will_def`: `1` if the user is willing to defend a case / play as a defence attorney (or a co-defence attorney),
- `will_pro`: `1` if the user is willing to prosecute a case / play as a prosecutor (or a co-prosecutor),
- `will_judge`: `1` if the user is willing to judge a case,
- `will_jury`: `1` if the user is willing to be a member of the jury in a case,
- `will_steno`: `1` if the user is willing to be the stenographer of a case.

It is up to the server to decide what to do with this packet. By default, tsuserver stores the information gathered from it, and uses it to decide whom to alert.

Additionally, this packet is sent everytime the "Casing" tickbox on the game area (not the "Casing" tickbox in the Settings!) is turned on or off. When it is turned off, a `SETCASE#""#0#0#0#0#0#0#%` packet is sent (signalling that the user does not want to play as anything).

## Case alert

This packet sends an alert to players about a case needing participants.

C: **CASEA#<casetitle: string>#<need_def: int>#<need_pro: int>#<need_judge: int>#<need_jury: int>#<need_steno: int>#%**  
S: **CASEA#<message: string>#<need_def: int>#<need_pro: int>#<need_judge: int>#<need_jury: int>#<need_steno: int>#%**

Where the arguments are:

- `casetitle`: The title of the case being played,
- `message`: This could also equal to the title of the case, but the server may choose to decorate it -- for example, by default, tsuserver adds the "X user needs this and that for Turnabout Y." text instead.
- `need_def`: `1` if the user needs a defence attorney,
- `need_pro`: `1` if the user needs a prosecutor,
- `need_judge`: `1` if the user needs a judge,
- `need_jury`: `1` if the user needs jurors,
- `need_steno`: `1` if the user need a stenographer.

The above packet, when sent from clientside, requests the server to sent the serverside packet to all the targeted users. 
The default behaviour is that users are targeted if they marked themselves as at least one of the roles the announcement is looking for using the `SETCASE` packet. 
There is a default cooldown of a minute on a case alert.
If a `CASEA` packet arrives to the client, the client plays the given SFX at the `case_call` line in the current theme's `courtroom_sounds.ini`, if one exists.

# Judge commands

In-game, characters in the "jud" position have some special abilities.  
  
## Witness Testimony/Cross Examination

These are animated gifs that display in the playing area that facilitate gameplay. They are often shortened to WTCE.  
  
C: **RT#testimony1/testimony2/[judgeruling]#[<variant: int>]#%** (testimony1 is WT, testimony2 is CE, judgeruling is Not Guilty / Guilty)  
S: (same)

`judgeruling` and its `variant`s are a 2.6 addition.
The `variant` part only comes in when it concerns `judgeruling`. 
Variant `0` is Not Guilty, variant `1` is Guilty.

## Penalty bars

There are two penalty bars in the client, which are changed by this judge command.  
  
C: **HP#1/2#0-10#%** (1 is the defense bar, 2 is the prosecution bar)
S: (same)

# Backgrounds

The client knows which background it should use based on this packet. Is often a part of the connection/loading process.

S: **BN#<background_name: string>#%**

# Evidence

The Attorney Online 2.4.0 client and later support the usage of evidence. It is worth noting that the evidence type has three attributes: name, description and image. The network usage is such;

## Add

C: **PE#<name: string>#<description: string>#<image: string>#%**  
S: **LE#<name: string>&<description: string>&<image: string>#...#%** (the entire evidence list is sent here)   

## Remove

C: **DE#<id: int>#%**  
S: **LE#<name: string>&<description: string>&<image: string>#...#%**

## Edit

C: **EE#<id: int>#<name: string>#<description: string>#<image: string>#%**  
S: **LE#<name: string>&<description: string>&<image: string>#...#%**

Take note that the LE header(i.e. the whole evidence list) is sent from the server every time the client does an operation. This is to ensure synchronization between clients. The server should also send its associated evidence list when the client joins a server or an area.

# Keep alive

The client should have some indication as to when the server stops responding. That is where the CH packet comes in, which should be sent by the client at regular intervals, and receive an appropriate response from the server.  
  
C: **CH#<char_id: int>#%**  
S: **CHECK#%**

# Moderator commands

## Call mod

A command anyone can send, this alerts all mods on guard with a sound cue.  
  
C: **ZZ#%**  
S: **ZZ#<call_mod_message: string>#%**  
  
The call mod message typically contains information such as which character called, which area called from and their IP. This depends on the serverside implementation, though.

## IP list

Moderators have a utility to list all the characters connected along with their IP addresses.  
OOC command: /ip  
  
Using this is optional. The server might opt to send a string formatted with newlines instead.  
  
S: **IL#<ip: string>|<char_name: string>|<char_id: int>|...#%** (extended to include all connected clients)  

## Muting

Prevents a client from sending IC messages. The server should enforce this serverside.  
OOC command: /mute <char_name: string>/<char_id: int>/-1(everyone)  
  
S: **MU#<char_id: int>#%** (mutes client with corresponding char_id)  
  
Unmuting is the same, except the header is UM instead of MU.

## Kicking

Disconnects a client from the server.  
OOC command: /kick <char_name: string>/<char_id: int> [reason: string]
  
S: **KK#<char_id: int>#<reason: string>#%**

In 2.6, a new `reason` field is added. Though it is not necessary in the calling of the OOC command (it is replaced with `N/A` if left empty), the packet requires one.

## Banning

The ultimate penalty. Disconnects and prevents client from reconnecting.  
OOC command: /ban <char_name: string>/<char_id: int> [reasong: string]
  
S: **KB#<char_id: int>#<reason: string>#%**  

In 2.6, a new `reason` field is added. Though it is not necessary in the calling of the OOC command (it is replaced with `N/A` if left empty), the packet requires one.
  
There is another packet associated with banning. The server should send this when a banned client attempts to connect;  
  
S: **BD#<reason: string>#%**  
  
This properly informs the client as to why a connection fails.

In 2.6.1, a new `reason` field is added. It is required when sending this packet.

# Master server

C: `askforservers#%` (returns first server on the list starting from 0)  
S: first server  
C: `SR#[id]#%`  
S: server of index `[id]`  

|Function     |Message       |Direction|
|-------------|--------------|---------|
|Chat|`CT#[username]#[message]#%`|Server|
|Version check|`VC#%`|Server|
|Client version (AO2)|`ID#[client software]#[version]#%`|Server|
|Hard drive ID (or anything in practice)|`HI#[hdid]#%`|Server|
|Get all servers (AO2)|`ALL#%`|Server|
|Server entry (paginated) (AO1)|`SN#[entry number]#[ip]#[server version]#[port]#[name]#[desc]#%`|Client|
|Server entry (all) (AO2)|`ALL#[[name]&[desc]&[ip]&[port]#]%`|Client|
|Server version|`SV#[version]#%`|Client|
|Ping|`PING#%`|Server|
|Server not advertised (reply to ping)|`NOSERV#%`|Advertiser|
|Heartbeat|`SCC#[port]#[name]#[description]#[server software]#%`|Server|
|Heartbeat success|`PSDD#0#%`|Advertiser|
|Keepalive|`CHECK#%`|Advertiser|
|Global ban|`DOOM#%`|Client|
