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
- [FL](#FL)
- [FM](#FM)
- [HI](#HI)
- [ID (Client)](#ID-Client)
- [ID (Server)](#ID-Server)
- [KB](#KB)
- [KK](#KK)
- [PN](#PN)
- [RC](#RC)
- [SC](#SC)
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

# RC

Receivers: `Server`

(no fields)

When the Server receives `RC` it should respond with `SC`.

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

# ZZ

Receivers: `Client, Server`

| Key      | Type     | Rules                     |
|----------|----------|---------------------------|
| `reason` | `string` | Length `<=255` (Optional) |

This packet is used to notify mods of rulebreaking (call mod).

When the Server receives this, it should send the same packet to all connected mods.
When the Client receives this, it should send an audio/visual notification and show the reason somewhere
(subject to Client settings).