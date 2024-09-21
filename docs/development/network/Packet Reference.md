# Packet Reference

- [[HI] Handshake](#Handshake)
- [[ID] Version information (Server->Client)](#version-information-server-client)

## Handshake

- Header: `HI`
- Direction: `Client->Server`

### Fields

| Key    | Type        | Rules |
|--------|-------------|-------|
| `hdid` | `string`    |       |

- `hdid` is a value that uniquely identifies the device (eg. hard drive ID, browser fingerprint)

### Behavior

When the Server receives `HI` it should
respond with `ID`.

## Version information (Server->Client)

- Header: `ID`
- Direction: `Server-Client`

### Fields

| Key             | Type     | Rules                       |
|-----------------|----------|-----------------------------|
| `player_number` | `number` | Positive integer            |
| `version`       | `string` | Should be in format `x.y.z` |

- `player_number` should be the number of players currently on the server

### Behavior


## Version information (Client->Server)

- Header: `ID`
- Direction: `Client->Server`

### Fields

| Key        | Type     | Rules                       |
|------------|----------|-----------------------------|
| `software` | `string` | Name of software            |
| `version`  | `string` | Should be in format `x.y.z` |

