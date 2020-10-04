# Possible Discord-esque Gateway.

## Client Initialisation
- Client will connect to websocket (e.g. ws://comms.i386.sh)
- Upon connection, client will immedietly receive Function Code 100 - containing what the server expects from the client (eg. heartbeat interval)
- Client is expected to send a heartbeat over the Websocket under Function Code 110, the Websocket may send this Function Code to the client, in which case the client must respond as if it is sending a regular heartbeat.

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

### Function Code 110 - Heartbeat

Client must send this every ``hb_int`` milliseconds.

#### Body
```json
{
  "f": 110
}
```

If Function Code 110 isn't sent back by the client, presume connection failure and close Websocket.
