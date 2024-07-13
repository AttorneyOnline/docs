## Network Protocol

Attorney Online's network protocol is one with a rich and colorful past. It has mostly remained intact since AO1 due to a constant demand for backward compatibility; even to this day, AO1 clients are still able to find servers from the lobby and join a game.

- [Network Protocol](#network-protocol)
  - [Handshake](#handshake)
    - [Version information](#version-information)
    - [Hard drive ID](#hard-drive-id)
    - [Asset Link](#asset-link)
    - [Player count](#player-count)
    - [Resource counts](#resource-counts)
    - [Character list](#character-list)
    - [Evidence list](#evidence-list)
    - [Music list](#music-list)
    - [Final confirmation](#final-confirmation)
  - [Players](#players)
    - [Catalog player](#catalog-player)
    - [Update player](#update-player)
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
    - [Add](#add)
    - [Remove](#remove)
    - [Edit](#edit)
  - [Areas](#areas)
    - [Switch area](#switch-area)
    - [Area updates](#area-updates)
    - [Music list](#music-list-1)
    - [Area list](#area-list)
  - [Moderator commands](#moderator-commands)
    - [Authentication](#authenticate)
    - [Call mod](#call-mod)
    - [Kick](#kick)
    - [Ban](#ban)
    - [Popup](#popup)
  - [Keep alive](#keep-alive)
  - [Subtheme switching](#subtheme-switching)
  - [Timers](#timers)
  - [Judge controls](#judge-controls)
  - [Escape codes](#escape-codes)
  - [Special thanks](#special-thanks)

### Handshake

#### Version information

**Client:** `ID#{client}#{version}#{protocol version}%`
**Server:** `ID#{player number}#{server}#{version}%`

Sends the client's name, version and supported protocol version. The server replies with the player number, server internal name and version.

**Server response:** Version information
**Client response:** Hard drive ID

#### Hard drive ID

**Client:** `HI#{hdid}`

Sends the client's hard drive ID (supposedly a unique identifier) to the server for ban tracking. This increases the work needed to evade a ban, as it is not as simple as using a VPN.

**Server response:** Player count, asset link (if defined)

#### Player count

**Server:** `PN#{players}#{max}#{server_description}`

Specifies the number of players in the server and the player limit. The player limit is not enforced in the client, and often not in the server either.<br/>
Since 2.10 the server description is appended to the packet to support server descriptions in the favourite tab.

#### Asset link

**Server:** `ASS#{asset_link}`

Specifies the asset link of the server. It allows server to set a custom content repository, which is used for WebAO or client music streaming.

#### Resource counts

**Client:** `askchaa`<br>
**Server:** `SI#{char_cnt}#{evi_cnt}#{music_cnt}`

Requests character, evidence, and track counts ahead of time for memory allocation.

#### Character list

**Client:** `RC`<br>
**Server:** `SC#{char_name}&{char_desc}#...`

Requests a full list of characters. Note that `char_desc` is obsolete and likely not present.

#### Evidence list

**Server:** `LE#{name}&{description}&{image}#...`

See the [evidence list](#list) packet.

#### Music list

**Client:** `RM`<br>
**Server:** `SM#{music_name}#...`

Requests a full list of music tracks.

Note that areas are also considered tracks. Typically, tracks are also contained in categories, which are prefixed with multiple equals signs (`==`).

Music tracks must contain file extensions.

#### Final confirmation

**Client:** `RD`<br>
**Server:** `DONE`

The client confirms that it has joined the server and is ready to join the default room. Typically, before `DONE` is sent, the list of taken characters is also sent, among other information such as the message of the day (displayed in OOC chat).

### Players

The servers sends information about the current players.

#### Catalog player

Signals whatever to add or remove a player to the list.

**Server:** `PC#{id}#{update}`

* `id`: Id of the player.
* `update`: Update type enum.

###### Update type
* `0`: Add player
* `1`: Remove player
  
#### Update player

Provides new information regarding an existing player.

**Server:** `PU#{id}#{type}#{data}`

* `id`: Id of the player.
* `type`: Data type enum.
* `data`: Data

###### Data type
* `0`: Name
* `1`: Character
* `2`: Character name
* `3`: Area id

### Character selection

#### Taken characters

**Server:** `CharsCheck#{taken}#...`

The server sends an array of numbers representing whether or not each character of that index is taken or not.

`taken` may be one of the following:

- `-1`: character is taken
- ` 0`: character is free

#### Choose character

**Client:** `CC#0#{char_id}#{hdid}`<br>
**Server:** `PV#{player_id}#CID#{char_id}`

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
{effect}#
{blips}
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
{effect}#
{blips}
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

- **blips**: The sound effect to play while processing text, colloquially the "blips."

All sections from `showname` onwards is 2.6+. `sfx_looping` onwards is 2.8+. `blips` onward is 2.10.2+.

> Note that servers may modify specific values of this message. For example, disemvoweling and shaking modify your text, and `/force_nonint_pres` forces your preanims to be noninterrupting. Therefore, to determine if your message was successfully sent, the character ID should be compared instead of the message text or the entire packet.

#### Background

**Server:** `BN#{background}%`<br>
**Server:** `BN#{background}#{position}%` (since 2.8)

Sets the background of the viewport, and optionally the position.

Clients can set the background with a server chat command, such as `/bg`.

#### Music

**Client:** `MC#{songname}#{char_id}#{showname}#{effects}`<br>
**Server:** `MC#{songname}#{char_id}#{showname}#{looping}#{channel}#{effects}`

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

**Client:** `HP#{bar}#{value}`<br>
**Server:** same

Updates the penalty bar. This is typically only allowed when the player is in the judge (`jud`) position.

- **bar**: the bar to be updated
  - `1`: defense bar (red bar)
  - `2`: prosecution bar (blue bar)
- **value**: the value to set the bar to (0-10)

#### Witness Testimony/Cross Examination (WT/CE)
  
**Client:** `RT#{animation}#{variant}`<br>
**Server:** same

Overlays a non-looping special animation. This is typically only allowed when the player is in the judge (`jud`) position.

**animation** may be one of the following:
  - `testimony1` - "Witness Testimony"
  - `testimony2` - "Cross Examination"
  - `judgeruling` - "Not Guilty/Guilty"
  - Anything else - Custom splash. SFX name and animation filename are exactly the string provided (since 2.9)
**variant** variant of the animation to use depending on certain types.
  - `testimony1` Disables the testimony overlay.
  - `judgeruling`
    - `0` Plays Not Guilty animation.
    - `1` Plays Guilty animation.

#### Set position

**Server:** `SP#{side}`

Sets the position dropdown on the client to `side`. Added in 2.8.

#### Available positions

**Server:** `SD#{side1}*{side2}*...`

Overrides the position dropdown on the client to the specified list of positions. Added in 2.8.

### Out-of-character message

**Client:** `CT#{name}#{message}`<br>
**Server:** `CT#{name}#{message}#{is_from_server}`

Represents a simple message with name and text. OOC is used to convey information without interrupting ongoing gameplay.

`is_from_server` is an addition that arrived with 2.6. If `1`, the name is colored with the theme's designated OOC server color.

### Evidence

Supported by 2.4 onward. Every evidence item has three attributes: name, description and image.

> Note that the whole evidence list is broadcasted every time the client performs any evidence operation, to ensure synchronization between clients. The server also sends the evidence list when the client joins an area.

#### List

**Server:** `LE#{name}&{description}&{image}#...`

#### Add

**Client:** `PE#{name}#{description}#{image}`

#### Remove

**Client:** `DE#{id}`

#### Edit

**Client:** `EE#{id}#{name}#{description}#{image}`

### Areas

#### Switch area

**Client:** `MC#{area_name}#{char_id}`

Switches to a different area/room. As areas are more or less a hack built into the music list, the `MC` packet is reused for this purpose.

The server will typically reset the health bars, background, and evidence.

If the specified area does not exist, the packet is ignored.

#### Area updates

**Server:**
- `ARUP#0#{area1_players}#{area2_players}#...`
- `ARUP#1##{area1_status}##{area2_status}##...`
- `ARUP#2##{area1_cm}##{area2_cm}##...`
- `ARUP#3##{area1_locked}##{area2_locked}##...`

Indicates additional information about all areas.

The first argument indicates the information being sent about every area:

- `0`: player counts
- `1`: area statuses
  -  Recognized statuses: `IDLE`, `LOOKING-FOR-PLAYERS`, `CASING`, `RECESS`, `RP`, `GAMING`
- `2`: case managers (CMs)
  - This should be set to `FREE` if there are no case managers in the area. 
- `3`: locked states (string, not boolean)
  - Recognized lock states: `FREE`, `SPECTATABLE`, `LOCKED` 

This packet has as many pieces (plus one) as there are areas on the server. It describes, in order, every area's given property.

For instance, a packet of `ARUP#0#4#3#7#2#0#0` would mean that:

- There are 4 players in the first area (or, technically, in the zeroth area)
- There are 3 in the second,
- There are 7 in the third,
- There are 2 in the fourth,
- And 0 in the last two.

This packet was added in 2.6.

#### Music list

**Server:** `FM#{track1}#{track2}#...`

Like the standard music list packet, but excludes areas.

This can be used to update the music list on a per-area basis.

This packet was added in 2.8.

#### Area list

**Server:** `FM#{area1}#{area2}#...`

Like the standard music list packet, but excludes music and only lists areas.

This can be used to add and remove areas dynamically.

This packet was added in 2.8.

### Moderator commands

#### Authenticate
**Server:** `AUTH#{state: int}`

**Parameters:**
**state:**
- `1` or higher: Successful log-in. Displays the guard button.
- `0`: Unsuccessful log-in attempt.
- `-1` or lower: Log-out. Hides the guard button.

#### Call mod
  
Alerts all mods on guard with a sound cue. The call mod message typically contains information such as which character called, which area they called from, and a user-specified message.

**Client:** `ZZ#{reason}#{client id}`<br>
**Server:** `ZZ#{reason}`<br>

`reason`: Reason provided by the caller.
`client id`: The client ID of the callee or -1 if it's a general call.

#### Kick

**Server:** `KK#{reason}`

Notifies a client that they were kicked.

#### Ban

**Server:** `KB#{reason}`

Notifies a client that they were kicked and banned.
  
**Server:** `BD#{reason}`
  
Notifies a client that they cannot join because they are banned.

#### Popup

**Server:** `BB#{message}`

Notifies a client with a popup containing the specified **message**.

### Keep alive
  
**Client:** `CH#<char_id: int>`<br>
**Server:** `CHECK`

Sent to ensure that the server and client are still alive. Some servers expect the client to send this packet as often as 10 seconds.

### Subtheme switching

**Server:** `ST#{subtheme}#{reload}`

Instructs the client to switch to a given subtheme. The client can ignore this if the user's subtheme is manually set.

**Parameters:**
* **subtheme:** The subtheme to switch to.
* **reload:** If `1`, the client will reload theme.

### Timers

**Server:** `TI#{timer_id: int}#{command: int}#{time: int}`

Instructs the client to manipulate the timers on its UI.

**Parameters:**
* **timer_id:** The ID of the timer to manipulate, `0`-`4`. Typically, `0` is used as a "global" timer (be it server-wide or per-"hub")
* **command:**
  * `0`: Start/resume/sync timer at `time`
  * `1`: Pause timer at `time`
  * `2`: Show timer
  * `3`: Hide timer
* **time:** The time to display on the timer, in milliseconds.

### Judge controls

**Server:** `JD#{state: int}`

Instructs the client to show or hide the judge controls.

**Parameters:**
* **state:**
  * `-1`: Show or hide the judge controls, depending on the client's position.
  * `0`: Hide the judge controls.
  * `1`: Show the judge controls.

### Escape codes

Escape codes allows characters like '#' to be sent in messages.

- `%`: `<percent>`
- `#`: `<num>`
- `$`: `<dollar>`
- `&`: `<and>`

### Special thanks

Thanks to stonedDiscord and Aleks for initial AO1 reverse engineering; OmniTroid for AO2 protocol documentation; and Cerapter for 2.6 protocol documentation.
