# Packet Reference

This document provides a reference for all packets that can be handled by an AO-compatible
Client and Server. Each headline corresponds to the header of the packet in question.
Note that some packets are (de)serialized differently depending on the handler being the Server or Client.
In these cases, the packet is marked with (Client) and (Server), respectively.

- [HI](#HI)
- [ID](#ID-Server))

# HI

Receivers: `Server`

## Fields

| Key    | Type        | Rules |
|--------|-------------|-------|
| `hdid` | `string`    |       |

`hdid` is a value that uniquely identifies the device (e.g. hard drive ID, browser fingerprint)

## Behavior

When the Server receives `HI` it should
respond with `ID`. See also Client Joining Process.

## Version information (Server->Client)

- Header: `ID`
- Direction: `Server->Client`

### Fields

| Key             | Type     | Rules                       |
|-----------------|----------|-----------------------------|
| `player_number` | `number` | Positive integer            |
| `software`      | `string` | Name of the software        |
| `version`       | `string` | Should be in format `x.y.z` |

`player_number` should be the number of players currently on the server
`software` should be the name of the software the server is on
`version` is the server software's version

### Behavior

When the Client receives ID it should send ID back.

# ID (Server)

- Header: `ID`
- Direction: `Server`

### Fields

| Key        | Type     | Rules                       |
|------------|----------|-----------------------------|
| `software` | `string` | Name of software            |
| `version`  | `string` | Should be in format `x.y.z` |

