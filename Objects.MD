# Objects

i386chat will refer to certain arrays as Objects, below is information about each object and what it should contain.

## Token Object
Must be signed as a JWT with the server's secret. Identifies the user.
```json
{
  "type": "user",
  "id": 6982582592583503353
}
```

### Structure Information
|Name|Data Type|Description|
-----|---------|-----------|
type | String | User's type (are they a user, or a bot?)
id | Integer | User's ID (compound key of Users database, along with UUID)

## User Object
```json
{
  "id": 6982582592583503353,
  "name": "xFuney",
  "uuid": "ccad",
  "admin": false,
  "bio": "Toast."
}
```

### Structure Information
|Name|Data Type|Description|
-----|---------|-----------|
id | Integer | User's ID (compound key of Users database, along with UUID)
name | String | User's Name (not to be used as a unique identifier)
uuid | String | User's Unique Identifier (compound key of Users database, with User ID)
admin | Boolean | Whether or not the user is an Administrator.
bio | String | User's bio.

## Message Object
```json
{
  "content": "MESSAGE CONTENTS!",
  "nonce": 9835823582359235235,
  "user": <User Object>
}
```

### Structure Information
|Name|Data Type|Description|
-----|---------|-----------|
content | String | Message contents.
nonce | Integer | Client's message identifier for optimistic message sending.
user | User Object | User Object of the user sending the message.

## Room Object
```json
{
  "id" : 935235823592358235,
  "name": "dank-memes",
  "description": "Dank memes room!",
  "visibility": true,
  "creator": <User Object>
}
```

### Structure Information
|Name|Data Type|Description|
-----|---------|-----------|
id | Integer | Unique identifier for the room (primary key of Rooms DB)
name | String | Name of room.
description | String | Description of the room.
visibility | Boolean | Whether or not the room is public (*true* for Public, *false* for Private)
creator | User Object | User Object of the creator of the room.
  
  
