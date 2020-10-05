# Possible Discord-esque Gateway.

## Client Initialisation
- Client will connect to websocket (e.g. ws://comms.i386.sh)
- Upon connection, client will immedietly receive Function Code 100 - containing what the server expects from the client (eg. heartbeat interval)
- Client is expected to send a heartbeat over the Websocket under Function Code 110, the Websocket may send this Function Code to the client, in which case the client must respond as if it is sending a regular heartbeat.

## Payload Structure

Field | Data Type | Description
------|-----------|------------
f | Integer | Function Code for this payload
cnf | JSON, String, Integer | Event data.
s | Integer | Sequence number for session resumption, might be scrapped.
t | String | Event name for this payload.

Note that `s` and `t` values will be ignored if the Function Code is not 120.

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

##### "Configuration" Structure
Field | Data Type | Description
------|-----------|------------
hb_int | Integer | Interval in milliseconds that the client should send a heartbeat to the server.

### Function Code 110 - Heartbeat

Client must send this every ``hb_int`` milliseconds.

#### Body
```json
{
  "f": 110
}
```

If Function Code 110 isn't sent back to the client by the server, presume connection failure and close Websocket.

##### "Configuration" Structure

No parameters are sent on a heartbeat Function Code.

### Function Code 120 - Execute

Client sends this when it wishes to interact with the server.

#### Body
```json
{
  "f": 120,
  "cnf": {},
  "s": 21,
  "e": "EVENT_HERE"
}
```
#### "Configuration" Structure

The structure of the configuration depends on the event that is being sent to the server. See [Events](https://github.com/i386chat/i386chat_docs/blob/main/Possible_Gateway.MD#events).

## Events

|Event Name|Description|
-----------|------------
MESSAGE_CREATE | Client creating a message.
MESSAGE_DELETE | Client deleting a message.
START_TYPING | Client has started typing.
SYSTEM_BAN_ADD | Client is trying to add a system-wide ban.
CHANNEL_ADD | Client is creating a new channel.
CHANNEL_MOVE | Client is moving into another channel.
