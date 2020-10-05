# Errors

Errors are usually returned as part of a 400 BAD REQUEST, and are structured like such:

```json
{
    "code": 1,
    "error": "Error body"
}
```

## Error Codes

### Code 0
Invalid token.

### Code 1
User does not exist in User DB.

### Code 3
Room does not exist in Room DB.

### Code 4
Bot does not exist in Bot DB.

### Code 5
User is banned from the server.

### Code 99
SQLite failure.

### Code 544541
Tea.

### Gateway-only Error Codes

### Code 4000
Disconnected from gateway - payload over 4096 bytes.

### Code 4001
Disconnected from gateway - timeout.

### Code 4002
Disconnected from gateway - ratelimited.

### Code 4003
Disconnected from gateway - gateway is shutting down.
