# Protocol

## Frame Structure
A frame consits of a 10 byte header followed by variable length data.
```mermaid
---
title: Packet Structure
---
packet-beta
0-1: "Version"
2-3: "Encoding"
4-6: "Message"
7-10: "Length"
11-31: "Data (variable length)"
```

### Version

A single byte that specifies the version of the protocol in use.

### Encoding Types

A single byte that defines how the data in the frame is encoded. The following encoding types are supported:

| Code | Encoding    |
| ---- | ----------- |
| 0    | JSON        |
| 1    | MessagePack |

### Message Types

Two bytes that define the type of the message. Below is the list of message types with their corresponding descriptions.

| Code | Name                    | Description                                           | Data |
| ---- | ----------------------- | ----------------------------------------------------- | ---- |
| 0    | `BROADCAST_REQUEST`     | A message to send to all peers in the lobby.          | TBC  |
| 1    | `BROADCAST_RESPONSE`    | Response indicating if the broadcast was accepted.    | TBC  |
| 2    | `BROADCAST`             | A message send to all peers in the lobby.             | TBC  |
| 3    | `CREATE_LOBBY_REQUEST`  | A Request to create a new lobby.                      | TBC  |
| 4    | `CREATE_LOBBY_RESPONSE` | Response indicating if the lobby was created.         | TBC  |
| 5    | `JOIN_LOBBY_REQUEST`    | Request to join an existing lobby.                    | TBC  |
| 6    | `JOIN_LOBBY_RESPONSE`   | Response indicating if the join request was accepted. | TBC  |
| 7    | `DISCONNECTION`         | A peer has disconnected from the lobby.               | TBC  |

### Length
TODO

### Data
TODO

## Private Lobby Workflow

### Lobby Creation Process

The following diagram illustrates the flow for creating a private lobby:

```mermaid
sequenceDiagram
    participant S as Server
    actor A
    actor B
    actor C
    A->>S: CREATE_LOBBY_REQUEST
    S->>+A: CREATE_LOBBY_RESPONSE
    Note over S,A: Lobby created
    A-->>B: Lobby details
    A-->>C: Lobby details
    B->>S: JOIN_LOBBY_REQUEST
    S->>A: JOIN_LOBBY_RESPONSE
    S->>B: JOIN_LOBBY
    Note over B: Joined lobby
    C->>S: JOIN_LOBBY_REQUEST
    S->>A: JOIN_LOBBY_RESPONSE
    S->>B: JOIN_LOBBY_RESPONSE
    S->>C: JOIN_LOBBY_RESPONSE
    Note over C: Joined lobby
```

### Message relay

#### Host-to-Peer Message Relay

When the host sends a message to all peers in the lobby:

```mermaid
sequenceDiagram
    participant S as Server
    actor H as Host
    actor P1
    actor P2
    H->>S: Broadcast
    par Broadcast to peers
        S->>P1: Broadcast
        S->>P2: Broadcast
    end
```

#### Peer-to-Host Message Relay

When a peer sends a request to the host, which is then broadcast to all peers:

```mermaid
sequenceDiagram
    participant S as Server
    actor H as Host
    actor P1
    actor P2
    P1->>S: Send request
    S->>H: Send request
    H->>S: Broadcast
    par Broadcast to peers
        S->>P1: Broadcast
        S->>P2: Broadcast
    end
```

