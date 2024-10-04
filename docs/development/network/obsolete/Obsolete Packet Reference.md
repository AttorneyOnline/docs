# Obsolete Packet Reference

These packets are considered obsolete and should no longer be used.
Do not use these headers when defining new packets.

- [ALL (Masterserver)](#ALL-masterserver)
- [ALL (Client)](#ALL-client)
- [decryptor](#decryptor)
- [CT](#CT)
- [HI](#HI)
- [ID](#ID)
- [MU](#MU)
- [NOSERV](#NOSERV)
- [OPPASS](#OPPASS)
- [PING](#PING)
- [SN](#SN)
- [SV](#SV)
- [UM](#UM)
- [VC](#VC)
- [IL](#IL)

# ALL (Masterserver)

Receiver: `Masterserver`

When the Masterserver receives this, it should respond with `ALL (Client)`.

# ALL (Client)

Receiver: `Client`

| Key    | Type     | Rules |
|--------|----------|-------|
| `name` | `string` |       |
| `desc` | `string` |       |
| `ip`   | `string` |       |
| `port` | `string` |       |

When the Client receives this, it should render a list of servers
along with their description.

# decryptor

Receiver: `Client`

| Key   | Type     | Rules            |
|-------|----------|------------------|
| `key` | `number` | Positive integer |

This packet is involved in an obsolete mechanism for encrypting
client headers. See FantaCrypt for more info.

# CT

Sender: `Client, Masterserver`
Receiver: `Client, Masterserver`

The masterserver used to support OOC chat like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `CT` with Server as receiver is not obsolete.

# HI

Sender: `Client`
Receiver: `Masterserver`

The masterserver used to support HI packets like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `HI` with Server as receiver is not obsolete.

# ID

Sender: `Client`
Receiver: `Masterserver`

The masterserver used to support ID packets like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `ID` with Server as receiver is not obsolete.

# MU

Sender: `Server`
Receivers: `Client`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |

When the Client receives this, it should visually indicate that the player
has been muted.

# NOSERV

Sender: `Masterserver`
Receiver: `Server`

# OPPASS

Sender: `Server`
Receivers: `Client`

| Key           | Type           | Rules                |
|---------------|----------------|----------------------|
| `modpass`     | `string`       | Valid hex (0-9, A-F) |

Sends the modpass to the client. Please never implement this.

# PING

Sender: `Server`
Receiver: `Masterserver`

When Masterserver receives this, it should respond with `NOSERV` if the server is
not listed.

# SN

Sender: `Masterserver`
Receiver: `Client`

| Key               | Type     | Rules |
|-------------------|----------|-------|
| `entry_number`    | `number` |       |
| `ip`              | `string` |       |
| `server_version`  | `string` |       |
| `port`            | `number` |       |
| `name`            | `string` |       |
| `desc`            | `string` |       |

When the Client receives this it should append the single entry to
the serverlist in the UI.

# SV

Sender: `Masterserver`
Receiver: `Client`

| Key       | Type     | Rules |
|-----------|----------|-------|
| `version` | `string` |       |

Version from the Masterserver.

# UM

Sender: `Server`
Receivers: `Client`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |

When the Client receives this, it should visually indicate that the player
has been unmuted.

# VC

Sender: `Client`
Receivers: `Masterserver`

Purpose unknown.

# IL

Sender: `Server`
Receivers: `Client`

| Key       | Type                 | Rules                                |
|-----------|----------------------|--------------------------------------|
| `players` | `array[player_data]` | `player_data` must be a valid object |

## player_data

| Key         | Type     | Rules  |
|-------------|----------|--------|
| `ip`        | `string` |        |
| `char_name` | `string` |        |
| `char_id`   | `number` |        |

This packet contains a list of all connected players.
When the Client receives it, it should show a list of player data.

FantaCode serialization: `IL#{ip}|{char_name}|{char_id}#...#%`
