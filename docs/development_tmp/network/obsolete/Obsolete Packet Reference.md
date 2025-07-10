# Obsolete Packet Reference

These packets are considered obsolete and should no longer be used.
Do not use these headers when defining new packets.

- [ALL (Masterserver)](#ALL-masterserver)
- [ALL (Client)](#ALL-client)
- [askforservers](#askforservers)
- [decryptor](#decryptor)
- [CHECK](#CHECK)
- [CT (Masterserver)](#CT-masterserver)
- [DOOM](#DOOM)
- [HI](#HI)
- [ID](#ID)
- [MU](#MU)
- [NOSERV](#NOSERV)
- [OPPASS](#OPPASS)
- [PING](#PING)
- [PSDD](#PSDD)
- [RE](#RE)
- [SCC](#SCC)
- [SN](#SN)
- [SR](#SR)
- [SV](#SV)
- [UM](#UM)
- [VC](#VC)
- [IL](#IL)

# ALL (Masterserver)

Receiver: `Masterserver`

When the Masterserver receives this, it should respond with `ALL (Client)`.

Serialized: `ALL#%`

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

Serialized: `ALL#{server1_name}&{server1_desc}&{server1_ip}&{server1_port}#{server2_name}&{server2_desc}&{server2_ip}&{server2_port}#...#%`

# askforservers

- Sender: `Client`
- Receiver: `Masterserver`

When Masterserver receives this, it should send `SN` containing the first Server info.

Serialized: `askforservers#%`

# decryptor

- Sender: `Server`
- Receiver: `Client`

| Key   | Type     | Rules            |
|-------|----------|------------------|
| `key` | `number` | Positive integer |

This packet is involved in an obsolete mechanism for encrypting
client headers. See FantaCrypt for more info.

Serialized: `decryptor#{key}#%`

# CHECK

- Sender: `Masterserver`
- Receiver: `Server`

Keepalive packet.

Serialized: `CHECK#%`

# CT (Masterserver)

- Sender: `Client, Masterserver`
- Receivers: `Client, Masterserver`

The masterserver used to support OOC chat like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `CT` with Server as receiver is not obsolete.

# DOOM

- Sender: `Masterserver`
- Receiver: `Client`

Indicates that the Client has been banned from AO. When the client receives this,
it should display a popup reading "Glory to Arstotzka!" before closing the program.

Serialized: `DOOM#%`

# EVENT

- Sender: `Masterserver`
- Receiver: `Client`

Indicates the start of a special event.

When Client receives this, it should display a scripted dialogue of Sho asking the player to bring FanatSors to his lair
or face certain destruction.

Serialized: `EVENT%`

# HI

- Sender: `Client`
- Receiver: `Masterserver`

The masterserver used to support HI packets like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `HI` with Server as receiver is not obsolete.

# ID

- Sender: `Client`
- Receiver: `Masterserver`

The masterserver used to support ID packets like the Server does.
This is no longer the case. The specification is the same (See Packet Reference).
Note that `ID` with Server as receiver is not obsolete.

# MU

- Sender: `Server`
- Receivers: `Client`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |

When the Client receives this, it should visually indicate that the player
has been muted.

Serialized: `MU#{char_id}#%`

# NOSERV

- Sender: `Masterserver`
- Receiver: `Server`

Indicates that the server is not listed on the Masterserver.

Serialized: `NOSERV#%`

# OPPASS

- Sender: `Server`
- Receivers: `Client`

| Key           | Type           | Rules                |
|---------------|----------------|----------------------|
| `modpass`     | `string`       | Valid hex (0-9, A-F) |

Sends the modpass to the client. Please never implement this.

Serialized: `OPPASS#{modpass}#%`

# PING

- Sender: `Server`  
- Receiver: `Masterserver`

When Masterserver receives this, it should respond with `NOSERV` if the server is
not listed and `PSDD` if it is.

Serialized: `PING#%`

# PSDD

- Sender: `Masterserver`
- Receiver: `Server`

Has one field called `status` which is always `0` (presumably). Indicates
successful listing.

Serialized: `PSDD#0#%`

# RE

- Sender: `Client`
- Receiver: `Server`

(no fields)

When the Server receives this, it should respond with `LE`.

Serialized: `RE#%`

# SCC

- Sender: `Server`
- Receiver: `Masterserver`

| Key               | Type     | Rules |
|-------------------|----------|-------|
| `port`            | `number` |       |
| `name`            | `string` |       |
| `description`     | `string` |       |
| `server_software` | `string` |       |

Requests listing on the Masterserver.
When the Masterserver receives this, it should list the Server and send `PSDD`.

Serialized: `SCC#{port}#{name}#{description}#{server_software}#%`

# SN

- Sender: `Masterserver`
- Receiver: `Client`

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

Serialized: `SN#{entry number}#{ip}#{server version}#{port}#{name}#{desc}#%`

# SR

- Sender: `Client`
- Receiver: `Masterserver`

| Key               | Type     | Rules |
|-------------------|----------|-------|
| `entry_number`    | `number` |       |

When Masterserver receives this, it should send the Server info at `entry_number`.

Serialized: `SR#{entry_number}#%`

# SV

- Sender: `Masterserver`
- Receiver: `Client`

| Key       | Type     | Rules |
|-----------|----------|-------|
| `version` | `string` |       |

Version from the Masterserver.

Serialized: `SV#{version}#%`

# UM

- Sender: `Server`
- Receiver: `Client`

| Key       | Type     | Rules            |
|-----------|----------|------------------|
| `char_id` | `number` | Positive integer |

When the Client receives this, it should visually indicate that the player
has been unmuted.

Serialized: `UM#{char_id}#%`

# VC

- Sender: `Client`
- Receiver: `Masterserver`

Purpose unknown.

Serialized: `VC#%`

# IL

- Sender: `Server`
- Receiver: `Client`

| Key       | Type                 | Rules                                |
|-----------|----------------------|--------------------------------------|
| `players` | `array[player_data]` | `player_data` must be a valid object |

Sends a list of player data to the Client.

## player_data

| Key         | Type     | Rules  |
|-------------|----------|--------|
| `ip`        | `string` |        |
| `char_name` | `string` |        |
| `char_id`   | `number` |        |

This packet contains a list of all connected players.
When the Client receives it, it should show a list of player data.

Serialized: `IL#{player1_ip}|{player1_char_name}|{player1_char_id}#{player2_ip}|{player2_char_name}|{player2_char_id}#...#%` (note the unusual separator)
