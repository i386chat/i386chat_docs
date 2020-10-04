# Possible Discord-esque Gateway.

## Client Initialisation
- Client will connect to websocket (e.g. ws://comms.i386.sh)
- Upon connection, client will immedietly receive Function Code 100 - containing what the server expects from the client (eg. heartbeat interval)
- Client is expected to send a heartbeat over the Websocket under Function Code 110, the Websocket may send this Function Code to the client, in which case the client must respond as if it is sending a regular heartbeat.

