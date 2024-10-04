# Packet Reference

This document provides a reference for all packets that can be handled by an AO-compatible
Client and Server. Each headline corresponds to the header of the packet in question.
Note that some packets are (de)serialized differently depending on the receiver being the Client or Server.
In these cases, the packet is marked with (Client) and (Server), respectively.

- [ARUP](#ARUP)
- [askchaa](#askchaa)
- [ASS](#ASS)
- [AUTH](#AUTH)
- [BD](#BD)
- [BN](#BN)
- [CASEA](#CASEA)
- [CC](#CC)
- [CH](#CH)
- [CharsCheck](#CharsCheck)
- [CHECK](#CHECK)
- [DE](#DE)
- [DONE](#DONE)
- [EE](#EE)
- [FA](#FA)
- [FL](#FL)
- [FM](#FM)
- [HI](#HI)
- [ID (Client)](#ID-Client)
- [ID (Server)](#ID-Server)
- [JD](#JD)
- [KB](#KB)
- [KK](#KK)
- [LE](#LE)
- [MA](#MA)
- [MC](#MC)
- [MS](#MS)
- [PE](#PE)
- [PN](#PN)
- [PR](#PR)
- [PU](#PU)
- [PV](#PV)
- [RC](#RC)
- [RD](#RD)
- [SC](#SC)
- [SETCASE](#SETCASE)
- [ST](#ST)
- [TI](#TI)
- [ZZ](#ZZ)

# ARUP

Receivers: `Client`

| Key           | Type                 | Rules |
|---------------|----------------------|-------|
| `update_type` | `number`             | `0-3` |
| `update_data` | `array[int\|string]` |       |

Updates a status for all areas.
Should only be sent to clients who have sent `arup` in `FL`.

`update_type` indicates what data is being updated

- `0`: player counts, this means `update_data` contains the number of players in each area
- `1`: area statuses, `update_data` contains the status of each area
  -  Valid statuses: `IDLE`, `LOOKING-FOR-PLAYERS`, `CASING`, `RECESS`, `RP`, `GAMING`
- `2`: case managers, this means `update_data` contains the name of CM(s)? (uncertain)
  - This should be set to `FREE` if there are no case managers in the area.
- `3`: locked states, (string, not boolean)
  - Valid lock states: `FREE`, `SPECTATABLE`, `LOCKED`

For instance, a packet FantaCoded as `ARUP#0#4#3#7#2#0#0#%` would mean that:

- There are 4 players in the first area (or, technically, in the zeroth area)
- There are 3 in the second,
- There are 7 in the third,
- There are 2 in the fourth,
- And 0 in the last two.

# askchaa

Receivers: `Server`

(no fields)

When Server receives `askchaa` it should respond with `SI`.

# ASS

Receivers: `Client`

| Key         | Type        | Rules               |
|-------------|-------------|---------------------|
| `asset_url` | `string`    | Valid URL (http(s)) |

Indicates what base URL Client should use for fetching assets via web.

# AUTH

Receivers: `Client`

| Key          | Type     | Rules      |
|--------------|----------|------------|
| `auth_state` | `number` | `-1, 0, 1` |

`auth_state` indicates the state of authentication (for moderator) the Client should be in.

Valid states are:
- `1`, Successful login. Displays the guard button.
- `0`, Unsuccessful login attempt.
- `-1`, Logout. Hides the guard button.

# BB

| Key       | Type     | Rules              |
|-----------|----------|--------------------|
| `message` | `string` | Length `<=255`     |

When the Client receives this, it should show the player `message`
in a popup window or similar.

# BD

Receivers: `Client`

| Key       | Type     | Rules                       |
|-----------|----------|-----------------------------|
| `reason`  | `string` | Length `<=255`              |

When the Client receives this, it should inform the player that they cannot
join the server because they are banned and give `reason` as the reason.

# BN

Receivers: `Client`

| Key          | Type     | Rules        |
|--------------|----------|--------------|
| `background` | `string` |              |
| `position`   | `string` |              |

When Client receives this, it should set the background to `background`
and the position to `position`.

# CASEA

Receivers: `Server, Client`

| Key          | Type     | Rules          |
|--------------|----------|----------------|
| `case_title` | `string` | Length `<=255` |
| `message`    | `string` | Length `<=255` |
| `need_def`   | `number` | `0, 1`         |
| `need_pro`   | `number` | `0, 1`         |
| `need_judge` | `number` | `0, 1`         |
| `need_jury`  | `number` | `0, 1`         |
| `need_steno` | `number` | `0, 1`         |

- `case_title`: the title of the case being played
- `message`: the title of the case, but may be modified by the server -- for example, by default, tsuserver adds the "X user needs this and that for Turnabout Y." text instead.
- `need_def`: `1` if the user needs a defense attorney, `0` otherwise
- `need_pro`: `1` if the user needs a prosecutor, `0` otherwise
- `need_judge`: `1` if the user needs a judge, `0` otherwise
- `need_jury`: `1` if the user needs jurors, `0` otherwise
- `need_steno`: `1` if the user need a stenographer, `0` otherwise

When Server receives this, it should send it to all connected Clients.
When Client receives this, it can render `message` in OOC chat, but may disable it through configuration.

# CC

Receiver: `Server`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |
| `hdid`    | `string` |                  |

This packet is sent by the Client to the Server to indicate that the Client
tries to select the character identified by `char_id`.

`hdid` is considered obsolete and can be omitted.

When the Server receives this, it should send `PV` if the character was
selected successfully.

Serialized as `CC#0#{char_id}#{hdid}#%` (hardcoded 0)

# CH

Receiver: `Server`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |

`char_id` is the character id of the Client that sent `CH`.
The Client is expected to send `CH` at least once every 10 seconds.
This indicates that the Client is still connected.

When the Server receives `CH` it should respond with `CHECK` and
reset the timeout timer for the Client.

# CharsCheck

Receiver: `Client`

| Key     | Type            | Rules |
|---------|-----------------|-------|
| `taken` | `array[number]` |       |

This packet indicates which characters are taken and not.

The value of `taken` may be one of the following:
- `-1`: character is taken
- ` 0`: character is free

The position in the array decides which character this corresponds to.

When the client receives this packet, it should visually indicate which
characters are taken.

# CHECK

Receiver: `Client`

(no fields)

Sent by the server as a response to `CH`. No further action is needed
by the client.

# DE

Receivers: `Server`

| Key  | Type     | Rules |
|------|----------|-------|
| `id` | `number` |       |

When the Server receives this, it should delete the evidence with the associated `id`.

# DONE

Receivers: `Client`

(no fields)

When the Client receives this, the joining process is complete and it
should render character select.

# EE

Receivers: `Server`

| Key           | Type     | Rules          |
|---------------|----------|----------------|
| `id`          | `number` |                |
| `name`        | `string` | Length `<=255` |
| `description` | `string` | Length `<=255` |
| `image`       | `string` | Length `<=255` |

When the Server receives this, it should update the evidence with the associated `id` with the new data.

# FA

Receivers: `Client`

| Key     | Type               | Rules        |
|---------|--------------------|--------------|
| `areas` | `array[area_data]` |              |

## area_data

| Key    | Type     | Rules          |
|--------|----------|----------------|
| `name` | `string` | Length `<=255` |

When the Client receives this, it should update its area list accordingly.

# FL

Receivers: `Client`

| Key        | Type            | Rules |
|------------|-----------------|-------|
| `features` | `array[string]` |       |

Indicates which features the Server supports.

Valid features and what they indicate support for:
- `yellowtext`, using yellow text (and more colors?) in `MS`
- `flipping`, horizontally mirroring a characters sprite in `MS`
- `customobjections`, using a custom objection in `MS` (named `custom.gif`)
- `fastloading`, using the joining process v2 (fast loading) (See Client Joining Process)
- `noencryption`, using unencrypted headers (FantaCrypt). This means all subsequent headers must not be encrypted.
- `deskmod`, using `deskmod` in `MS`
- `evidence`, using evidence in `MS`
- `cccc_ic_support`, pairing characters in `MS`
- `arup`, the Server sending the `ARUP` packet
- `casing_alerts`, broadcasting casing alerts.
- `modcall_reason`, modcalling with a reason (see `ZZ`)
- `looping_sfx` , using looping sfx in `MS`
- `additive`, using additive text in `MS`
- `effects`, using effect overlays in `MS`
- `y_offset`, using y offset in `MS`
- `expanded_desk_mods`, using `deskmod` modifiers 2 through 5 in `MS`
- `auth_packet`, using the `AUTH` packet

# FM

Receivers: `Client`

| Key          | Type            | Rules                           |
|--------------|-----------------|---------------------------------|
| `music_list` | `array[object]` | Object must be valid music_data |

For the `music_data` object, see `SM`. This packet functions like `SM`, but does never include areas.

When received, the Client should store this data in memory for later use.
It should also display a list of these (using `name`) to the user.

# HI

Receivers: `Server`

| Key    | Type        | Rules |
|--------|-------------|-------|
| `hdid` | `string`    |       |

`hdid` is a value that uniquely identifies the device (e.g. hard drive ID, browser fingerprint)

When the Server receives `HI` it should
respond with `ID`. See also Client Joining Process.

# ID (Client)

Receivers: `Client`

| Key             | Type     | Rules                       |
|-----------------|----------|-----------------------------|
| `player_number` | `number` | Positive integer            |
| `software`      | `string` |                             |
| `version`       | `string` | Should be in format `x.y.z` |

`player_number` should be the number of players currently on the server
`software` should be the name of the software the server is on
`version` is the server software's version

When the Client receives `ID (Client)` it should send `ID (Server)` back.

# ID (Server)

Receivers: `Server`

| Key        | Type     | Rules                       |
|------------|----------|-----------------------------|
| `software` | `string` | Name of software            |
| `version`  | `string` | Should be in format `x.y.z` |

# JD

Receivers: `Client`

| Key     | Type     | Rules |
|---------|----------|-------|
| `state` | `number` |       |

When Client receives this, it should show or hide judge controls:
- `-1`: Show or hide the judge controls, depending on the client's position (by convention, show if `jud`)
- `0`: Hide the judge controls.
- `1`: Show the judge controls.

# KB

Receivers: `Client`

| Key       | Type     | Rules                       |
|-----------|----------|-----------------------------|
| `reason`  | `string` | Length `<=255`              |

When the Client receives this, it should notify the player that they
have been banned and give `reason` as the reason.

# KK

Receivers: `Client`

| Key       | Type     | Rules                       |
|-----------|----------|-----------------------------|
| `reason`  | `string` | Length `<=255`              |

When the Client receives this, it should notify the player that they
have been kicked and give `reason` as the reason.

# LE

Receivers: `Client`

| Key        | Type                   | Rules |
|------------|------------------------|-------|
| `evidence` | `array[evidence_data]` |       |

## evidence_data

| Key           | Type     | Rules                   |
|---------------|----------|-------------------------|
| `name`        | `string` | Length `<=255`          |
| `description` | `string` | Length `<=255`          |
| `image`       | `string` | Length `<=255`          |

When Client receives `LE` it should update its evidence list
and show the new evidence using the values in the packet. `image` is
the filename of the image to use.

# MA

Receivers: `Client`

| Key        | Type     | Rules |
|------------|----------|-------|
| `id`       | `number` |       |
| `duration` | `number` |       |
| `reason`   | `string` |       |

This packet indicates that moderator action has been taken against a player.

- `id`: Id of the player.
- `duration`: Duration of the action in minutes.
- `reason`: Provided reason by the moderator.

# MC

Receivers: `Server, Client`

| Key        | Type     | Rules                   |
|------------|----------|-------------------------|
| `name`     | `string` | Length `<=255`          |
| `char_id`  | `number` |                         |
| `showname` | `string` | Length `<=255`          |
| `looping`  | `string` | Not present from Client |
| `channel`  | `string` | Not present from Client |
| `effects`  | `number` | `0-2`                   |

The purpose of this packet is two-fold. The simplest use is by the Client to change
area. In that case, it simply sends area name in `name` and its current character in `char_id`.

However, if `name` has a file extension, it is considered to be a music change.

When the Server receives this packet, it should simply send it to connected Clients in the area.

When the Client receives this packet, it should play the specified song in `name`.

Since 2.8, the track is expected to loop clientside. Before 2.8, the track was not expected to loop, and thus servers had to add a looping feature where the `MC` packet is sent to replay the track.

- `name`: the name of the song
- `char_id`: id of the char that played the song
- `showname`: Showname that should be shown (`<showname>` played song.mp3)
- `looping`: if `1`, indicates client-side looping. `0` otherwise.
- `channel`: channel number to play on (`0-3`)
  - Typically, channel `0` is used as the main BGM channel, whereas channel `1` is used for area-specific soundscapes.
- `effects`: transition effects:
  - `0`: fade in
  - `1`: fade out
  - `2`: sync position

The canonical "empty track" is `~stop.mp3`.

# MS

Has its own page. See MS Packet Reference.

# PE

Receivers: `Server`

| Key           | Type     | Rules                   |
|---------------|----------|-------------------------|
| `name`        | `string` | Length `<=255`          |
| `description` | `string` | Length `<=255`          |
| `image`       | `string` | Length `<=255`          |

When the Server receives this it should update its corresponding evidence list
and send `LE` to all Clients in the corresponding area.

# PN

Receivers: `Client`

| Key                  | Type     | Rules            |
|----------------------|----------|------------------|
| `player_count`       | `number` | Positive integer |
| `max_players`        | `number` | Positive integer |
| `server_description` | `string` | Optional field   |

`player_count` indicates how many players are currently on the Server.
`max_players` also indicates how many players are the most permitted. Note that this is rarely enforced in practice.
`Server description` is a textual description of the server.

# PR

Receivers: `Client`

| Key    | Type     | Rules |
|--------|----------|-------|
| `id`   | `number` |       |
| `type` | `number` |       |

When the Client receives this, it should add or remove a player to their playerlist.

- `id` is the player's ID.
- `type` is the update type:
- - `0`: Add player
- - `1`: Remove player

# PU

Receivers: `Client`

| Key    | Type             | Rules |
|--------|------------------|-------|
| `id`   | `number`         |       |
| `type` | `number`         |       |
| `data` | `string\|number` |       |

When the Client receives this, it should update data about a player in their playerlist.

- `id` is the player's ID.
- `type` is the data type:
- - `0`: Update player's name
- - `1`: Update player's character id
- - `2`: Update player's character name
- - `3`: Update player's area id
- `data` is the new data.

# PV

Receivers: `Client`

| Key         | Type             | Rules |
|-------------|------------------|-------|
| `player_id` | `number`         |       |
| `char_id`   | `number`         |       |

When the Client receives this, it should hide char select and
initialize the character for use, using the `char_id` provided.

Serialized as `PV#{player_id}#CID#{char_id}#%`

> **Note:** A `PV` response may be sent at any time to force a character switch, and the character ID may not necessarily be the one requested by the player.

# RC

Receivers: `Server`

(no fields)

When the Server receives `RC` it should respond with `SC`.

# RD

Receivers: `Server`

(no fields)

When the Server receives `RD` it should respond with `DONE`.

# RM

Receivers: `Server`

(no fields)

When the Server receives `RM`, it should respond with `SM`.

# SC

Receivers: `Client`

| Key         | Type            | Rules                          |
|-------------|-----------------|--------------------------------|
| `char_data` | `array[object]` | Object must be valid char_data |

## char_data

| Key        | Type     | Rules          |
|------------|----------|----------------|
| `id`       | `number` | Value `>=0`    |
| `name`     | `string` | Length `<=100` |
| `desc`     | `string` | Length `<=100` |
| `evidence` | `string` | Length `<=100` |

`id` is the character's unique ID
`name` is the character's name
`desc` is the character's description. This is mostly unused.
`evidence` is the character's evidence(?).

When received, the Client should store this data in memory for later use.

# SETCASE

Receivers: `Server`

| Key        | Type     | Rules  |
|------------|----------|--------|
| `caselist` | `array`  |        |
| `cm`       | `string` | `0, 1` |
| `def`      | `string` | `0, 1` |
| `pro`      | `string` | `0, 1` |
| `judge`    | `string` | `0, 1` |
| `jury`     | `string` | `0, 1` |
| `steno`    | `string` | `0, 1` |

- `caselist`: the list of cases this user is willing to host (assuming they are also willing to CM) (not used)
- `cm`: `1` if the user is willing to host cases (not used), `0` otherwise
- `def`: `1` if the user is willing to defend a case / play as a defense attorney (or a co-defense attorney), `0` otherwise
- `pro`: `1` if the user is willing to prosecute a case / play as a prosecutor (or a co-prosecutor), `0` otherwise
- `judge`: `1` if the user is willing to judge a case, `0` otherwise
- `jury`: `1` if the user is willing to be a member of the jury in a case, `0` otherwise
- `steno`: `1` if the user is willing to be the stenographer of a case, `0` otherwise

Not clear how this is used in practice.

# SM

Receivers: `Client`

| Key          | Type            | Rules                           |
|--------------|-----------------|---------------------------------|
| `music_list` | `array[object]` | Object must be valid music_data |

## music_data

| Key    | Type     | Rules          |
|--------|----------|----------------|
| `name` | `string` | Length `<=150` |

`name` is the full name of the song OR area. If it does not have an extension (containing `.`),
is it interpreted as an area.

When received, the Client should store this data in memory for later use.
It should also display a list of these (using `name`) to the user (both music and areas).

# ST

Receivers: `Client`

| Key             | Type     | Rules          |
|-----------------|----------|----------------|
| `subtheme_name` | `string` | Length `<=100` |
| `should_reload` | `number` | `0, 1`         |

When the Client receives this, it should switch to the subtheme specified by
`subtheme_name`. If `should_reload` is `1`, it should reload the theme.
If it's 0, it does not need to reload.

# TI

Receivers: `Client`

| Key        | Type     | Rules            |
|------------|----------|------------------|
| `timer_id` | `number` | `0-4`            |
| `command`  | `number` | `0-3`            |
| `time`     | `number` | Positive integer |

When the Client receives this, it should adjust the timers in the UI accordingly.

`timer_id`,  the ID of the timer to manipulate.
Typically, `0` is used as a "global" timer (be it server-wide or per-"hub")

`command`:
- `0`: Start/resume/sync timer at `time`
- `1`: Pause timer at `time`
- `2`: Show timer
- `3`: Hide timer

`time`, the time to display on the timer, in milliseconds.

# ZZ

Receivers: `Client, Server`

| Key      | Type     | Rules                     |
|----------|----------|---------------------------|
| `reason` | `string` | Length `<=255` (Optional) |

This packet is used to notify mods of rulebreaking (call mod).

When the Server receives this, it should send the same packet to all connected mods.
When the Client receives this, it should send an audio/visual notification and show the reason somewhere
(subject to Client settings).
