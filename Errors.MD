# Errors

Errors are usually returned as a 400 BAD REQUEST, and are structured like such:

```json
{
    code: 1,
    error: "Error body"
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
