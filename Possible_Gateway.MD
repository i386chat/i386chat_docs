# Possible Discord-esque Gateway.

## Client Initialisation
- Client will connect to websocket (e.g. ws://comms.i386.sh)
- Upon connection, client will immedietly receive Function Code 100 - containing what the server expects from the client (eg. heartbeat interval)
- Client is expected to send a heartbeat over the Websocket under Function Code 120, the Websocket may send this Function Code to the client, in which case the client must respond as if it is sending a regular heartbeat.

## Payload Structure

Field | Data Type | Description
------|-----------|------------
f | Integer | Function Code for this payload
cnf | JSON, String, Integer | Event data.
s | Integer | Sequence number for session resumption, might be scrapped.
t | String | Event name for this payload.

Note that `s` and `t` values will be ignored if the Function Code is not 130.

Payloads will be limited to a maximum of **4096** bytes - going over this will result in connection termination.

## Function Codes

### Function Code 100 - Introduce and Configure

Only the server can send this function code - sent as soon as client starts initial connection.

#### Body
```json
{
  "f": 100,
  "cnf": {
    "hb_int": 20000
  }
}
```

#### "Configuration" Structure
Field | Data Type | Description
------|-----------|------------
hb_int | Integer | Interval in milliseconds that the client should send a heartbeat to the server.

### Function Code 110 - Present

Client-only code. Should send this before doing ANYTHING ELSE on the gateway.

#### Body
```json
{
  "f": 110,
  "cnf": {
    "token": "token_here"
  }
}
```

#### "Configuration" Structure
Field | Data Type | Description
------|-----------|------------
token | String | Your authentication token.

If authentication is successful and the payload is successful, the server will respond with Function Code 140.

### Function Code 120 - Heartbeat

Client must send this every ``hb_int`` milliseconds.

#### Body
```json
{
  "f": 120
}
```

If Function Code 120 isn't sent back to the client by the server, presume connection failure and close Websocket.

#### "Configuration" Structure

No parameters are sent on a heartbeat Function Code.

### Function Code 130 - Execute

Client sends this when it wishes to interact with the server.

#### Body
```json
{
  "f": 130,
  "cnf": {},
  "s": 21,
  "e": "EVENT_HERE"
}
```
### "Configuration" Structure

The structure of the configuration depends on the event that is being sent to the server. See [Events](https://github.com/i386chat/i386chat_docs/blob/main/Possible_Gateway.MD#events).

### Function Code 140 - Ready

Server sends this after the client identifies itself with Function Code 110.

#### Body
```json
{
  "f": 140,
  "cnf": {
    "v": 6,
    "session_id": 9683494578353
  }
}
```

#### Configuration Structure
|Field|Data Type|Description
------|---------|-----------
v | Integer | Gateway API version.
session_id | Integer | Unique Session Identifier for communication management/resumption.

## Events

|Event Name|Description|
-----------|------------
MESSAGE_CREATE | Client creating a message.
MESSAGE_DELETE | Client deleting a message.
START_TYPING | Client has started typing.
SYSTEM_BAN_ADD | Client is trying to add a system-wide ban.
CHANNEL_ADD | Client is creating a new channel.
CHANNEL_MOVE | Client is moving into another channel.
ROOM_USER_ADD | Other client is joining this channel
ROOM_USER_REMOVE | Other client is leaving this channel.
GATEWAY_DOWN | Gateway is going down - client should prepare for Websocket disconnection.

