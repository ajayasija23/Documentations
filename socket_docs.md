# 🃏 Jokerz Chat & Streaming API Documentation

This document provides details on integrating the **Jokerz Chat System** and **Live Streaming System** via REST APIs and Socket.IO events.

---

## 🌐 Base URL

```
https://staging-ws.jokerz.fun/
```

---

## 📦 REST API Endpoints

### 🔹 GET: User's Chat Rooms

- **Endpoint:** `/rooms`  
- **Description:** Retrieves all chat rooms the authenticated user is a member of.

---

### 🔹 GET: Messages in a Room

- **Endpoint:** `/rooms/:roomId/messages`  
- **Description:** Fetches messages for a specific room.  
- **Path Parameters:**
  - `roomId` — ID of the room.

---

### 🔹 POST: Create or Get Private Chat Room

- **Endpoint:** `/rooms/private`  
- **Description:** Creates or fetches a one-on-one private room between two users.

---

### 🔹 GET: All Users

- **Endpoint:** `/users`  
- **Description:** Retrieves the list of all users available for starting new chats.

---

## 🔌 Socket.IO Events

### 📥 Connect to Socket

```js
const socket = io("https://staging-ws.jokerz.fun", {
  auth: {
    token: "YOUR_AUTH_TOKEN"
  }
});
```

---

### 🔹 `join_room`

**Client Emit:**

```json
{ "roomId": "ROOM_ID" }
```

**Server Emit (Event: `joined_room`):**

```json
{ "roomId": "ROOM_ID" }
```

---

### 🔹 `leave_room`

**Client Emit:**

```json
{ "roomId": "ROOM_ID" }
```

**Server Emit (Event: `left_room`):**

```json
{ "roomId": "ROOM_ID" }
```

---

### 🔹 `typing_start`

**Client Emit:**

```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast (Event: `user_typing`):**

```json
{
  "userId": "USER_ID",
  "username": "USERNAME",
  "roomId": "ROOM_ID"
}
```

---

### 🔹 `typing_stop`

**Client Emit:**

```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast (Event: `user_stop_typing`):**

```json
{
  "userId": "USER_ID",
  "roomId": "ROOM_ID"
}
```

---

### 🔹 `send_message`

**Client Emit:**

```json
{
  "roomId": "ROOM_ID",
  "message": "Hello!",
  "receiverId": "RECEIVER_ID",
  "messageType": "text"
}
```

**Server Broadcast (Event: `new_message`):**

```json
{
  "senderId": "SENDER_ID",
  "receiverId": "RECEIVER_ID",
  "roomId": "ROOM_ID",
  "message": "Hello!",
  "messageType": "text",
  "timestamp": "TIMESTAMP",
  "isRead": false,
  "editedAt": null,
  "isDeleted": false,
  "sender": {
    "id": "SENDER_ID",
    "username": "USERNAME"
  }
}
```

---

### 🔹 `mark_read`

**Client Emit:**

```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast (Event: `messages_read`):**

```json
{
  "userId": "USER_ID",
  "roomId": "ROOM_ID"
}
```

---

## 📺 Jokerz Streaming Events

### ▶️ Start Stream

**Client Emit (`start_stream`):**

```json
{
  "hostId": "12345",
  "streamId": "abcde",
  "metaData": {
    "title": "Live Cooking Show",
    "goal_id": "goal_6789",
    "user_id": "user_001",
    "picture_url": "https://example.com/image.jpg",
    "is_public": true,
    "topic_id": "topic_123"
  }
}
```

**Server Emit (`stream_started`):**

```json
{
  "isInvited": false,
  "agencyEarning": "500",
  "audienceCount": 120,
  "coinsReceived": "1500",
  "createdAt": "2025-08-08T10:30:00Z",
  "duration": "45",
  "force_ended": false,
  "hostEarning": "1000",
  "hostId": 9876,
  "id": "live_001",
  "isEnded": false,
  "parentAgencyEarning": "300",
  "streamId": "stream_abc123",
  "updatedAt": "2025-08-08T11:15:00Z",
  "userName": "AjayAsija",
  "userPic": "https://example.com/avatar.jpg",
  "metaData": {
    "title": "Tech Talk with Ajay",
    "goal_id": "goal_123",
    "user_id": "user_987",
    "picture_url": "https://example.com/thumbnail.jpg",
    "is_public": true,
    "topic_id": "topic_xyz"
  }
}
```

---

### ⏹️ End Stream

**Client Emit (`end_stream`):**

```json
{
  "hostId": "35",
  "metaData": {
    "goal_id": "1528371c-0329-4b37-a816-085ce06f3374",
    "is_public": true,
    "picture_url": "https://firebasestorage.googleapis.com/v0/b/jokerz-ea1b2.firebasestorage.app/o/dev%2Fuploads%2Fstream_35?alt=media&token=d8c90e3d-74e8-4375-a935-2b7f3c5a8198",
    "title": "zjzjdjdjhdhd",
    "topic_id": "63837a11-1d76-47c9-92a6-7783489f6afd",
    "user_id": "5898cf04-2edb-446f-92b4-39dea8b760d5"
  },
  "streamId": "509101"
}
```

**Server Emit (`stream_ended`):**

```json
{
  "streamerName": "aja",
  "id": "ad32b25a-a1ce-4f82-962f-792724f88470",
  "hostId": 41,
  "userName": "aja",
  "userPic": "https://firebasestorage.googleapis.com/v0/b/jokerz-ea1b2.firebasestorage.app/o/dev%2Fuploads%2F41?alt=media&token=ab017624-068e-4f48-9dd6-6738bdf7ce4c",
  "streamId": "216896",
  "duration": "0",
  "coinsReceived": "0",
  "hostEarning": "0",
  "agencyEarning": "0",
  "parentAgencyEarning": "0",
  "audienceCount": 0,
  "isEnded": true,
  "createdAt": "2025-08-07T20:28:01.618Z",
  "updatedAt": "2025-08-07T20:28:01.618Z",
  "force_ended": false
}
```

---

### 👥 Join Stream

**Client Emit (`join_stream`):**

```json
{
  "unique_id": "35",
  "streamId": "535982",
  "user_pic": "https://firebasestorage.googleapis.com/v0/b/jokerz-ea1b2.firebasestorage.app/o/dev%2Fuploads%2F35?alt=media&token=acdef4b9-9e95-4449-a34a-1a83f2d22a61"
}
```

**Server Emit (`viewer_joined`):**

```json
{
  "viewerId": "f058a15d-9280-432f-9cc2-3ab327adcced",
  "viewerName": "aja",
  "user_pic": "https://firebasestorage.googleapis.com/v0/b/jokerz-ea1b2.firebasestorage.app/o/dev%2Fuploads%2F41?alt=media&token=ab017624-068e-4f48-9dd6-6738bdf7ce4c",
  "unique_id": "41"
}
```

---

## 📝 Notes

- All REST API endpoints require authentication via Bearer Token.
- Socket.IO requires authentication using the `auth.token` parameter during connection.
- Always emit `join_room` before sending or listening for messages.

---

## 📞 Contact

For support or integration help, please contact the backend team or raise an issue in your project tracker.
