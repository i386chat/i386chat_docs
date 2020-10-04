# Websocketing

This is the main way clients shall interact with i386chat.

## Client Initialisation

- Client establishes a Websocket connection.
- Client sends a "Initialise_UserData" packet to the server containing the user's login token.
- Server will authenticate user, and return Error Code 0 if authentication fails.
- In the event server authenticates user, server will store UserData of user for verification - tying this to their socket ID.
- UserData is stored on the server only and not on the client, to prevent malicious tampering.

After successful initialisation, client may communicate fully.

## Sending Messages

Client must send **Message_Send** event.

### Sample Body
```json
{
    "content": "<text goes here>"
    "nonce": <randomly generated integer>
}
```

## Receiving Messages

**Message_Recv** will be fired to client when a message is received.

### Sample body
```json
{
   "userInfo": {
      "username": "zer0",
      "userId": "U_68240",
      "bio": "I am an i386chat user!"
   },
   "content": "I sent a message.",
   "nonce": <randomly generated string from other client's Message_Send event>
}
```
