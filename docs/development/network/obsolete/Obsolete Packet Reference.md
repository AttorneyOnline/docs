# Obsolete Packet Reference

These packets are considered obsolete and should no longer be used.

- [decryptor](#decryptor)
- [OPPASS](#OPPASS)
- [IL](#IL)

# decryptor

Receiver: `Client`

| Key   | Type     | Rules            |
|-------|----------|------------------|
| `key` | `number` | Positive integer |

This packet is involved in an obsolete mechanism for encrypting
client headers. See FantaCrypt for more info.

# OPPASS

Receivers: `Client`

| Key           | Type           | Rules                |
|---------------|----------------|----------------------|
| `modpass`     | `string`       | Valid hex (0-9, A-F) |

Sends the modpass to the client. Please never implement this.

# IL

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
Note that it is serialized using `|` as subseparator.
