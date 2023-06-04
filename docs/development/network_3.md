## Network Protocol

This document suggests a potential rework of AO2s netcode, deprecated AO1s netcode and encoding pattern in favor of a more commonly used and supported industry standard.

### Table of contents
- [Packet Structure](#packet-structure)
- [Handshake](#handshake)
  - [Client Information](#client-information)
  - [Server Information](#server-information)
  - [Character List](#character-list)
- [Area List](#area-list)
- [Music List](#music-list)

# Packet Structure
All packets are formatted in a uniform way. You can only send one packet at a time.
The server will respond with a reponse header when an error is encountered.

`Normal Packet:`
```json
{
  "type": "HEADER",
  "content": {
    "key": "value"
  }
}
```
`On-Error Packet:`
```json
{
  "header": "ERROR",
  "header" : "HEADER",
  "error" : "errortext"
}
```


# Handshake

## Client Information

The Handshake is initiated by the client.

`Client:`
```json
{
  "type": "join",
  "HWID": "",
  "APPID": "AttorneyOnlineClient",
  "VERSION": "3.0"
}
```

## Server Information
`Server:`
```json
{
  "type": "serverinfo",
  "APPID": "Akashi",
  "VERSION": "1.0",
  "ASSET_HOST": "URL"
}
```

## Character List
```json
{
  "type": "characterlist",
  "characterlist": [
    "Phoenix",
    "Edgeworth"
  ]
}
```
Finishing to load the charlist ends the handshake. The client can now show the character UI.


# Area List
`Server:`
```json
{
  "type": "arealist",
  "areas": [
    "Basement",
    "Courtroom"
  ]
}
```
# Music List
`Server:`
```json
{
  "type": "musiclist",
  "musiclist": [
    {
      "category": "Ace Attorney",
      "songs": [
        "Objection.opus",
        "Pursuit.opus"
      ]
    }
  ]
}
```