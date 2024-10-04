# Packet Reference

This document provides a reference for all packets that can be handled by an AO-compatible
Client and Server. Each headline corresponds to the header of the packet in question.
Note that some packets are (de)serialized differently depending on the receiver being the Client or Server.
In these cases, the packet is marked with (Client) and (Server), respectively.

- [ARUP](#ARUP)
- [askchaa](#askchaa)
- [ASS](#ASS)
- [HI](#HI)
- [ID (Client)](#ID-Client)
- [ID (Server)](#ID-Server)
- [PN](#PN)

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
