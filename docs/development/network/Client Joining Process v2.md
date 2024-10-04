# Client Joining Process v2

## Client states

A client generally has three states relating to connecting to a server.

### Not Connected

This is the initial state and means that the client has not yet established
a connection with the server.

### Connected

This means that the client has established a connection
(TCP socket or websocket) connection to the server. This is
generally where the Server becomes aware of the Client.
This is a transient state, as the Client should move to a joined
state by initiating the joining process.

### Joined

This is the final state where the client has received sufficient
data from the server. It should be possible for the
Client to send and receive most packets in this state.

## Joining process

Once the Client has entered the Connected state, it should
initiate the joining process. Client and Server need to use
the following process to properly move to the Joined state:

1. Client sends `HI`
2. Server sends `ID`, `PN`(optional)
3. Client sends `ID`
4. Server sends `FL`, `ASS`(optional)
5. Client sends `askchaa`
6. Server sends `SI`
7. Client sends `RC`
8. Server sends `SC`
9. Client sends `RM`
10. Server sends `SM`
11. Client sends `RD`
12. Server sends `DONE`

Once the Server has sent DONE and Client has
received it, the Client is in the Joined state.
