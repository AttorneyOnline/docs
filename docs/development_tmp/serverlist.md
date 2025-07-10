# Serverlist

## Introduction

Typically, a client will want a list of online servers to show to users. The endpoint `https://servers.aceattorneyonline.com`
can be used for this purpose.

## Fetching servers

A list of online servers can be fetched with

`https://servers.aceattorneyonline.com/servers`

This will return a JSON array of servers with the following structure:

```(json)
[
    {
        "ip": "myserver.example.com",
        "port": 27010,
        "ws_port": 27011,
        "wss_port": 27012,
        "players": 10,
        "name": "Example server name",
        "description": "Example server description",
    }
]
```

Name and description should be pretty self-explanatory. Players is the number of players connected to the server. The other fields are explained below.

## ip

This field contains the hostname or IP address where the server is hosted.

## Ports

The server should contain at least one of the following ports:

### port

This field contains the port where the server is listening to incoming TCP connections.

### ws_port

This field contains the port where the server is listening to incoming WebSocket connections.

### wss_port

This field contains the port where the server is listening to incoming Secure WebSocket connections.

Note that it may not be sensible to display servers that don't have any supported port types to the user.

## Advertising a server

Servers can be advertised to the serverlist by sending a POST request to `https://servers.aceattorneyonline.com/servers`, using the same JSON structure as above. Note that all fields (at least one of the port types) are required.
