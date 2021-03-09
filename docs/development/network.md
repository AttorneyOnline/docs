## Network Protocol

Attorney Online's network protocol is one with a rich and colorful past. It has mostly remained intact since AO1 due to a constant demand for backward compatibility; even to this day, AO1 clients are still able to find servers from the lobby and join a game.

- [Network Protocol](#network-protocol)
  - [Handshake](#handshake)
    - [Hard drive ID](#hard-drive-id)
    - [Client version information](#client-version-information)
    - [Feature list](#feature-list)
    - [Player count](#player-count)
    - [Resource counts](#resource-counts)
    - [Character list](#character-list)
    - [Evidence list](#evidence-list)
    - [Music list](#music-list)
    - [Final confirmation](#final-confirmation)
  - [Character selection](#character-selection)
    - [Taken characters](#taken-characters)
    - [Choose character](#choose-character)
  - [In-character commands](#in-character-commands)
    - [In-character message](#in-character-message)
    - [Background](#background)
    - [Music](#music)
    - [Penalty (health) bars](#penalty-health-bars)
    - [Witness Testimony/Cross Examination (WT/CE)](#witness-testimonycross-examination-wtce)
    - [Set position](#set-position)
    - [Available positions](#available-positions)
  - [Out-of-character message](#out-of-character-message)
  - [Evidence](#evidence)
    - [List](#list)
    - [Add](#add)
    - [Remove](#remove)
    - [Edit](#edit)
  - [Areas](#areas)
    - [Switch area](#switch-area)
    - [Area updates](#area-updates)
    - [Music list](#music-list-1)
    - [Area list](#area-list)
  - [Casing](#casing)
    - [Case preferences update](#case-preferences-update)
    - [Case alert](#case-alert)
  - [Moderator commands](#moderator-commands)
    - [Call mod](#call-mod)
    - [Kick](#kick)
    - [Ban](#ban)
  - [Keep alive](#keep-alive)
  - [Subtheme switching](#subtheme-switching)
  - [Timers](#timers)
  - [Escape codes](#escape-codes)
  - [Obsolete](#obsolete)
    - [FantaCrypt](#fantacrypt)
    - [Slow loading](#slow-loading)
    - [Mod password](#mod-password)
    - [IP list](#ip-list)
    - [Mute](#mute)
- [Master server protocol](#master-server-protocol)
  - [Paginated list (obsolete)](#paginated-list-obsolete)

### Handshake

#### Hard drive ID

**Client:** `HI#{hdid}#%`

Sends the client's hard drive ID (supposedly a unique identifier) to the server for ban tracking. This increases the work needed to evade a ban, as it is not as simple as using a VPN.

#### Client version information

**Client:** `ID#{client}#{version}#%`

Sends the client name and version to the server.

**Response:** Player count, feature list

#### Feature list

**Server:** `FL#{feature1}#{feature2}#...#%`

Lists the features supported by the server, essentially acting as a list of feature flags.

(The FL packet was introduced in 2.3.3; prior to 2.3.3, features depended on detecting a compatible server version.)

Features introduced in 2.1.0 (the first AO2 release):

- `yellowtext`: Enables the use of yellow text.
- `flipping`: Enables the use of emote flipping.
- `customobjections`: Enables the use of a single custom objection named `custom`.
- `fastloading`: Enables the use of "fast loading" instead of the legacy loading protocol.
- `noencryption`: Disables FantaCrypt for the remainder of the session.

Introduced in 2.3 - 2.5:

- `deskmod`: Allows forcing desk/no desk. Introduced in 2.3.2.
- `evidence`: Enables evidence. Introduced in 2.4.0.

Introduced in 2.6:

- `cccc_ic_support`: Enables 2.6 extensions for the in-character command.
- `arup`: Indicates that the server broadcasts area updates.
- `casing_alerts`: Enables casing alerts.
- `modcall_reason`: Enables calling moderators with a custom reason.

Introduced in 2.8:

- `looping_sfx`: Enables looping SFX extensions for the in-character command.
- `additive`: Enables additive text.
- `effects`: Enables effect overlays.

#### Player count

**Server:** `PN#{players}#{max}#%`

Specifies the number of players in the server and the player limit. The player limit is not enforced in the client, and often not in the server either.

#### Resource counts

**Client:** `askchaa#%`<br>
**Server:** `SI#{char_cnt}#{evi_cnt}#{music_cnt}#%`

Requests character, evidence, and track counts ahead of time for memory allocation.

#### Character list

**Client:** `RC#%`<br>
**Server:** `SC#{char_name}&{char_desc}#...#%`

Requests a full list of characters. Note that `char_desc` is obsolete and likely not present.

#### Evidence list

**Client:** `RE#%`<br>
**Server:** `LE#{name}&{description}&{image}#...#%`

See the [evidence list](#list) packet.

#### Music list

**Client:** `RM#%`<br>
**Server:** `SM#{music_name}#...#%`

Requests a full list of music tracks.

Note that areas are also considered tracks. Typically, tracks are also contained in categories, which are prefixed with multiple equals signs (`==`).

Music tracks must contain file extensions.

#### Final confirmation

**Client:** `RD#%`<br>
**Server:** `DONE#%`

The client confirms that it has joined the server and is ready to join the default room. Typically, before `DONE` is sent, the list of taken characters is also sent, among other information such as the message of the day (displayed in OOC chat).

### Character selection

#### Taken characters

**Server:** `CharsCheck#{taken}#...#%`

The server sends an array of numbers representing whether or not each character of that index is taken or not.

`taken` may be one of the following:

- `-1`: character is taken
- ` 0`: character is free

#### Choose character

**Client:** `CC#0#{char_id}#{hdid}#%`<br>
**Server:** `PV#{player_id}#CID#{char_id}#%`

The client selects a character to use in the room. Note that `hdid` is an obsolete parameter. (The first parameter was once a player ID, but player IDs are also obsolete.)

If the character cannot be used, the server will not respond. Otherwise, the server responds with the character selected.

> **Note:** A `PV` response may be sent at any time to force a character switch, and the character ID may not necessarily be the one requested by the player.

### In-character commands

#### In-character message

**Client:**
```
MS#
{desk_mod}#
{preanim}#
{character}#
{emote}#
{message}#
{side}#
{sfx-name}#
{emote_modifier}#
{char_id}#
{sfx-delay}#
{shout_modifier}#
{evidence}#
{flip}#
{realization}#
{text_color}#
{showname}#
{other_charid}#
{self_offset}#
{noninterrupting_preanim}#
{sfx_looping}#
{screenshake}#
{frames_shake}#
{frames_realization}#
{frames_sfx}#
{additive}#
{effect}#%
```

**Server**:
```
MS#  
{desk_mod}#
{preanim}#
{character}#
{emote}#
{message}#
{side}#
{sfx-name}#
{emote_modifier}#
{char_id}#
{sfx-delay}#
{shout_modifier}#
{evidence}#
{flip}#
{realization}#
{text_color}#
{showname}#
{other_charid}#
{other_name}#
{other_emote}#
{self_offset}#
{other_offset}#
{other_flip}#
{noninterrupting_preanim}#
{sfx_looping}#
{screenshake}#
{frames_shake}#
{frames_realization}#
{frames_sfx}#
{additive}#
{effect}#%
```

An in-character (IC) message is a basic form of viewport event in which a animation is displayed on the screen with various parameters. Line breaks are included for cleanliness and are not present in the actual packet.

> Note that the only difference between the client and server messages is that the client does not have the `other_name` or `other_emote` fields.

Reference of fields:

- **desk_mod**: Whether or not to override desk appearance.
  * `chat`: Positions "def", "pro", and "wit" default to desk and the positions "hld", "hlp" and "jud" to no desk.
  * `0`: desk is hidden
  * `1`: desk is shown
  * `2`: desk is hidden during preanim, shown when it ends
  * `3`: desk is shown during preanim, hidden when it ends
  * `4`: desk is hidden during preanim, character is centered and pairing is ignored, when it ends desk is shown and pairing is restored
  * `5`: desk is shown during preanim, when it ends character is centered and pairing is ignored

- **preanim**: The animation that plays before the character starts talking. Does not include file extension.

- **character**: The folder name of the character that is talking.

- **emote**: The emote that should play. Does not include `(a)`/`(b)` prefixes or file extensions.

- **message**: The chat message as to be displayed in the chatbox and the IC log. Note that the message may contain markup that must be parsed.

- **side**: Which side the character is on. See the character authoring page for valid positions.

- **sfx_name**: Name of the sound effect that should play during the preanimation (if the preanimation is enabled).

- **emote_modifier**: A number that dictates emote behavior:
  * `0`: do not play preanimation; overridden to 2 by a non-`0` objection modifier
  * `1`: play preanimation (and sfx)
  * `2`: play preanimation and play objection
  * `3`: _unused_
  * `4`: _unused_
  * `5`: no preanimation and zoom
  * `6`: objection and zoom, no preanim

- **char_id**: Character identifier; dictates the index of the character in the character list (starting from zero).

- **sfx_delay**: Dictates how long in milliseconds the client should wait after the preanimation has started playing before playing the associated sound effect.

- **objection_modifier**: Dictates if the player uses a shout.
  * `0`: nothing
  * `1`: "Hold it!"
  * `2`: "Objection!"
  * `3`: "Take that!"
  * `4&{name}`: custom shout (since 2.8)

- **evidence**: ID of the evidence presented. 0 is no evidence presented, so evidence ID effectively starts from 1.

- **flip**: Dictates if the emote should be flipped.
  * `0`: no flip
  * `1`: flip

- **realization**: Dictates whether a realization flash and sound effect should play or not.
  * `0`: no realization
  * `1`: realization

- **text_color**: Dictates which color the text of the chat message should be.
  * `0`: white
  * `1`: green
  * `2`: red
  * `3`: orange
  * `4`: blue (disables talking animation)
  * `5`: yellow
  * `6`: rainbow (removed in 2.8)

- **showname**: If used, this will show a custom name (showname) for the character.

- **other_charid**: The character ID of the person the player wishes to pair up with.

- **other_name**: The folder name of the character the player is pairing up with (in case of INI swapping).

- **other_emote**: The emote the user's pair was doing. Note that by default, zooms (that are correctly defined as such) do not update this value, so a pair of zooms will not appear. Zooms also enjoy special privileges, in that (assuming they are correctly defined, again) they make the pair disappear in the client and get centered.

- **self_offset**: the percentage by which the character is shifted horizontally, from `-100` (one whole screen's worth to the left) to `100` (one whole screen's worth to the right). This parameter also stores vertical offset, which is self-explanatory.
  * `{x_offset}`: <2.9
  * `{x_offset}&{y_offset}`: 2.9+

- **other_offset**: The user's pair's `self_offset`, basically.

- **other_flip**: The user's pair's `char_id2/flip`, basically.

- **noninterrupting_preanim**: If `1`, the text begins at the same time as the preanimation.

- **sfx_looping**: If `1`, the sound effect loops until another emote is played.

- **screenshake**: If `1`, the screen shakes (TODO: on preanimation or on chat?).

- **frames_shake**: A list of frames for which the screen should shake (TODO: list format).

- **frames_realization**: A list of frames for which the screen should flash (TODO: list format).

- **frames_sfx**: A list of frames for which the sound effect should play.

- **additive**: If `1`, does not clear the chatbox's previous message.

- **effect**: The overlay effect to be displayed.

All sections from `showname` onwards is 2.6+. `sfx_looping` onwards is 2.8+.

> Note that servers may modify specific values of this message. For example, disemvoweling and shaking modify your text, and `/force_nonint_pres` forces your preanims to be noninterrupting. Therefore, to determine if your message was successfully sent, the character ID should be compared instead of the message text or the entire packet.

#### Background

**Server:** `BN#{background}%`<br>
**Server:** `BN#{background}#{position}%` (since 2.8)

Sets the background of the viewport, and optionally the position.

Clients can set the background with a server chat command, such as `/bg`.

#### Music

**Client:** `MC#{songname}#{char_id}#{showname}#{looping}#{channel}#{effects}#%`<br>
**Server:** same

Plays the specified track (with file extension) and records the event to the IC log.

Since 2.6, a `showname` piece is added at the end. This is for the chatlog to be able to correctly store and show who changed the music, so that it does not end up confusing players with the character's name instead of the showname.

Since 2.8, the track is expected to loop clientside. Before 2.8, the track was not expected to loop, and thus servers had to add a looping feature where the `MC` packet is sent to replay the track.

- **looping**: if greater than zero, indicates client-side looping
- **channel**: channel number to play on (0-3)
  - Typically, channel 0 is used as the main BGM channel, whereas channel 1 is used for area-specific soundscapes.
- **effects**: a bit field of transition effects:
  - Bit 0: fade in
  - Bit 1: fade out
  - Bit 2: sync position

The canonical "empty track" is `~stop.mp3`.

#### Penalty (health) bars

**Client:** `HP#{bar}#{value}#%`<br>
**Server:** same

Updates the penalty bar. This is typically only allowed when the player is in the judge (`jud`) position.

- **bar**: the bar to be updated
  - `1`: defense bar (red bar)
  - `2`: prosecution bar (blue bar)
- **value**: the value to set the bar to (0-10)

#### Witness Testimony/Cross Examination (WT/CE)
  
**Client:** `RT#{animation}#%`<br>
**Server:** same

Overlays a non-looping special animation. This is typically only allowed when the player is in the judge (`jud`) position.

**animation** may be one of the following:

- `testimony1` - "Witness Testimony"
- `testimony2` - "Cross Examination"
- `judgeruling#0` - "Not Guilty" (since 2.6)
- `judgeruling#1` - "Guilty" (since 2.6)

#### Set position

**Server:** `SP#{side}#%`

Sets the position dropdown on the client to `side`. Added in 2.8.

#### Available positions

**Server:** `SD#{side1}*{side2}*...#%`

Overrides the position dropdown on the client to the specified list of positions. Added in 2.8.

### Out-of-character message

**Client:** `CT#{name}#{message}#%`<br>
**Server:** `CT#{name}#{message}#{is_from_server}#%`

Represents a simple message with name and text. OOC is used to convey information without interrupting ongoing gameplay.

`is_from_server` is an addition that arrived with 2.6. If `1`, the name is colored with the theme's designated OOC server color.

### Evidence

Supported by 2.4 onward. Every evidence item has three attributes: name, description and image.

> Note that the whole evidence list is broadcasted every time the client performs any evidence operation, to ensure synchronization between clients. The server also sends the evidence list when the client joins an area.

#### List

**Client:** `RE#%`<br>
**Server:** `LE#{name}&{description}&{image}#...#%`

#### Add

**Client:** `PE#{name}#{description}#{image}#%`

#### Remove

**Client:** `DE#{id}#%`

#### Edit

**Client:** `EE#{id}#{name}#{description}#{image}#%`

### Areas

#### Switch area

**Client:** `MC#{area_name}#{char_id}#%`

Switches to a different area/room. As areas are more or less a hack built into the music list, the `MC` packet is reused for this purpose.

The server will typically reset the health bars, background, and evidence.

If the specified area does not exist, the packet is ignored.

#### Area updates

**Server:**
- `ARUP#0#{area1_players}#{area2_players}#...#%`
- `ARUP#1##{area1_status}##{area2_status}##...#%`
- `ARUP#2##{area1_cm}##{area2_cm}##...#%`
- `ARUP#3##{area1_locked}##{area2_locked}##...#%`

Indicates additional information about all areas.

The first argument indicates the information being sent about every area:

- `0`: player counts
- `1`: area statuses
- `2`: case managers (CMs)
- `3`: locked states (string, not boolean)

This packet has as many pieces (plus one) as there are areas on the server. It describes, in order, every area's given property.

For instance, a packet of `ARUP#0#4#3#7#2#0#0#%` would mean that:

- There are 4 players in the first area (or, technically, in the zeroth area)
- There are 3 in the second,
- There are 7 in the third,
- There are 2 in the fourth,
- And 0 in the last two.

This packet was added in 2.6.

#### Music list

**Server:** `FM#{track1}#{track2}#...#%`

Like the standard music list packet, but excludes areas.

This can be used to update the music list on a per-area basis.

This packet was added in 2.8.

#### Area list

**Server:** `FM#{area1}#{area2}#...#%`

Like the standard music list packet, but excludes music and only lists areas.

This can be used to add and remove areas dynamically.

This packet was added in 2.8.

### Casing

#### Case preferences update

**Client:** `SETCASE#{caselist}#{cm}#{def}#{pro}#{judge}#{jury}#{steno}#%`

Updates the user's casing alert preferences.

Parameters:

- **caselist**: the list of cases this user is willing to host (assuming they are also willing to CM) (not used)
- **cm**: `1` if the user is willing to host cases (not used)
- **def**: `1` if the user is willing to defend a case / play as a defense attorney (or a co-defense attorney)
- **pro**: `1` if the user is willing to prosecute a case / play as a prosecutor (or a co-prosecutor)
- **judge**: `1` if the user is willing to judge a case
- **jury**: `1` if the user is willing to be a member of the jury in a case
- **steno**: `1` if the user is willing to be the stenographer of a case

It is up to the server to decide what to do with this packet. By default, tsuserver stores the information gathered from it, and uses it to decide whom to alert.

Additionally, this packet is sent everytime the "Casing" tickbox on the game area is toggled. When it is turned off, a `SETCASE#""#0#0#0#0#0#0#%` packet is sent (indicating that the user is not interested in casing).

#### Case alert

**Client:** `CASEA#{case_title}#{need_def}#{need_pro}#{need_judge}#{need_jury}#{need_steno}#%`<br>
**Server:** same, except `{message}` instead of `{case_title}`

Sends an alert to players about a case needing participants.

Where the arguments are:

- **case_title**: the title of the case being played
- **message**: the title of the case, but may be modified by the server -- for example, by default, tsuserver adds the "X user needs this and that for Turnabout Y." text instead.
- **need_def**: `1` if the user needs a defense attorney,
- **need_pro**: `1` if the user needs a prosecutor,
- **need_judge**: `1` if the user needs a judge,
- **need_jury**: `1` if the user needs jurors,
- **need_steno**: `1` if the user need a stenographer.

The above packet, when sent from clientside, requests the server to sent the serverside packet to all relevant users.

Users are targeted if they marked themselves as at least one of the roles the announcement is looking for (using the `SETCASE` packet).

This packet may be rate-limited.

####

### Moderator commands

#### Call mod
  
**Client:** `ZZ#{reason}#%`<br>
**Server:** `ZZ#{message}#%`<br>

Alerts all mods on guard with a sound cue.

The call mod message typically contains information such as which character called, which area they called from, and a user-specified message.

#### Kick

**Server:** `KK#{reason}#%`

Notifies a client that they were kicked.

#### Ban

**Server:** `KB#{reason}#%`

Notifies a client that they were kicked and banned.
  
**Server:** `BD#{reason}#%`
  
Notifies a client that they cannot join because they are banned.

### Keep alive
  
**Client:** `CH#<char_id: int>#%`<br>
**Server:** `CHECK#%`

Sent to ensure that the server and client are still alive. Some servers expect the client to send this packet as often as 10 seconds.

#### Subtheme switching

**Server:** `ST#{subtheme}#{reload}#%`

Instructs the client to switch to a given subtheme. The client can ignore this if the user's subtheme is manually set.

**Parameters:**
* **subtheme:** The subtheme to switch to.
* **reload:** If `1`, the client will reload theme.

#### Timers

**Server:** `TI#{timer_id: int}#{command: int}#{time: int}#%`

Instructs the client to manipulate the timers on its UI.

**Parameters:**
* **timer_id:** The ID of the timer to manipulate, `0`-`4`. Typically, `0` is used as a "global" timer (be it server-wide or per-"hub")
* **command:**
  * `0`: Start/resume/sync timer at `time`
  * `1`: Pause timer at `time`
  * `2`: Show timer
  * `3`: Hide timer
* **time:** The time to display on the timer, in milliseconds.


### Escape codes

Escape codes allows characters like '#' to be sent in messages.

- `%`: `<percent>`
- `#`: `<num>`
- `$`: `<dollar>`
- `&`: `<and>`

### Obsolete

#### FantaCrypt

AO1 encrypted the headers of the packets sent by the client to the server.

- FantaCrypt seeds a PRNG with a known value (the decryption key), where the lower two bytes are used in the PRNG, and the upper byte of those two bytes is used in the cipher.
- The current key is XOR'ed with the current byte.
- To get the key value for the next byte, the byte and key are added together.
- Then, some multiplication and addition is done to that value. (This PRNG behavior is similar to a [linear congruential generator](https://en.wikipedia.org/wiki/Linear_congruential_generator).)
- Finally, everything but the lower two bytes are discarded.

However, there was a major flaw in the implementation of FantaCrypt: the server supplies the client with the initial key to use for every packet. Thus, it is totally vulnerable to a replay attack.

Many clients do not actually even implement the FantaCrypt algorithm, but instead use a hardcoded key of 5, which is sent as 0x34 in the 'decryptor' packet. This is because the 'decryptor' packet argument is the key to be used, but encrypted. The decryptor value is always encrypted with a magic number key value: 322 decimal. Moreover, FantaCrypt is only used in messages sent by the client to the server and not vice versa.

The encryption algorithm was likely plagiarized from a [Stack Overflow answer](https://stackoverflow.com/questions/14411975/simple-code-to-encrypt-an-ini-file-string-using-a-password).

#### Slow loading

Before "fast loading" character and music requests were developed, a slower, paginated set of requests was used:

|Message|Description|Typical response
|-------|-----------|--------
|`askchar2#%`|Get first page of characters|`CI#0#...#%`
|`AN#{n}#%`|Get page `n` of characters|`CI#{n}#...#%`, or `EI#1#...#%` if nonexistent page
|`AE#{n}#%`|Get `n`th evidence|`EI#{n}#...#%`, or `EM#0#...#%` if nonexistent page
|`EM#{n}#%`|Get page `n` of tracks |`EM#{n}#...#%`, or `CharsCheck#...#%` and remainder of handshake

#### Mod password

**Server:** `OPPASS#{modpass in hex}#%`

Previously, servers would send the mod password in hex encoding to the clients; the reason for this was to allow a clientside login check to show the moderator UI. It is entirely unknown how it was considered logical to send the mod password in plain text to all clients.

#### IP list

**Server:** `IL#{ip}|{char_name}|{char_id}#...#%`

Sends a list of all players. This is an infrequently used command and is typically sent as a response to `/ip` (an obsolete command).

The server might opt to send an OOC message instead.

#### Mute

**Server:** `MU#{char_id}#%` (mute)<br>
**Server:** `UM#{char_id}#%` (unmute)

Shows an overlay on the client using `char_id` that indicates that they are muted.


## Master server protocol

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

### Paginated list (obsolete)
 
**Client:** `askforservers#%` (returns first server on the list starting from 0)<br>
**Server:** first server<br>
**Client:** `SR#{n}#%`<br>
**Server:** server of index `n`

----

Thanks to stonedDiscord and Aleks for initial AO1 reverse engineering; OmniTroid for AO2 protocol documentation; and Cerapter for 2.6 protocol documentation.
