# Webhooks

Webhooks will take advantage of API endpoints to send messages to their appropriate channels.

These WILL require a Bot token to take advantage of, a regular user will NOT be able to send a webhook message.

## Sending Webhook Message

Presuming your instance is on `localhost` and you are using API version 2 - to send a message to channel ID C_2123452123, you would send a POST request to:

``http://localhost/api/v2/channels/C_2123452123/messages``

### Body
```json
{
   "content": "Webhook message would go here.",
   "nonce": "<random INTEGER, not string>"
}

Please note that you must have your Authorization header set properly, more information can be found in the Authorization documentation.

