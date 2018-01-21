# Network Specification  #

The following document covers how 1.7.5, 1.8 an 2.3.0+ versions of AO clients connect and communicate with servers.

## Legend ##

AO = Attorney Online  
char = character, as in the game character, not the char datatype  
type(x) = an array of type separated by x: typextypextype  
C = client  
S = server  
[x] = x is an optional argument(usually used by version 2.3.0+)  
{x} = x is encrypted using the "fantacrypt" algorithm  

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
S: **PV#<player_id:  int>#CID#<char_id: int>#%** (the server indicated that a character was successfully picked
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
<text_color: int>#%**  
S: (same, although may be slightly modified depending on server software and config)
  
For more information on valid arguments and client behavior, see [this](https://github.com/Attorney-Online-Engineering-Task-Force/Attorney-Online-Client-Remake/wiki/In-character-chat-messages%5BMS%5D)

## OOC messages

OOC messages, (abbreviation for "Out-of-character") are simple messages with a name and message associated with them. They're used by servers to convey information and by users to not interrupt ongoing gameplay.  
  
CT#<name: string>#<message: string>#%

## Escape Codes

Escape codes allows characters like '#' to be sent in messages.

'%' = "\<percent\>"  
'#' = "\<num\>"  
'$' = "\<dollar\>"  
'&' = "\<and\>"  

# Music

C: **MC#<songname: string>#<char_id: int>#%**  
S: (same)

Some servers support a looping feature where a song is played continuously, without receiving client music requests.

# Judge commands

In-game, characters in the "jud" position have some special abilities.  
  
## Witness Testimony/Cross Examination

These are animated gifs that display in the playing area that facilitate gameplay. They are often shortened to WTCE.  
  
C: **RT#testimony1/testimony2#%** (testimony1 is WT, testimony2 is CE)  
S: (same)

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
OOC command: /kick <char_name: string>/<char_id: int>  
  
S: **KK#<char_id: int>#%**

## Banning

The ultimate penalty. Disconnects and prevents client from reconnecting.  
OOC command: /ban <char_name: string>/<char_id: int>  
  
S: **KB#<char_id: int>#%**  
  
There is another packet associated with banning. The server should send this when a banned client attempts to connect;  
  
S: **BD#%**  
  
This properly informs the client as to why a connection fails.

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
