
# Jokerz Chat API Documentation

This document outlines the REST API endpoints and Socket.IO events for integrating the **Jokerz Chat System**.

---

## 🌐 Base URL

```
https://staging-ws.jokerz.fun/
```

---

## 📦 REST API Endpoints

### 🔹 Get User's Chat Rooms

**Endpoint:** `/rooms`  
**Method:** `GET`  
**Description:** Retrieves all chat rooms the authenticated user is a member of.

---

### 🔹 Get Messages in a Room

**Endpoint:** `/rooms/:roomId/messages`  
**Method:** `GET`  
**Description:** Fetches messages for a specific room.

**Path Parameters:**
- `roomId` — ID of the room.

---

### 🔹 Create or Get Private Chat Room

**Endpoint:** `/rooms/private`  
**Method:** `POST`  
**Description:** Creates or fetches a one-on-one private room between two users.

---

### 🔹 Get All Users

**Endpoint:** `/users`  
**Method:** `GET`  
**Description:** Retrieves the list of all users available for starting new chats.

---

## 🔌 Socket.IO Events

### 📥 Connect to Socket

```javascript
const socket = io("https://staging-ws.jokerz.fun", {
  auth: {
    token: "YOUR_AUTH_TOKEN"
  }
});
```

---

### 🔹 `join_room`

Joins the user to a specific room.

**Client Emit:**
```json
{ "roomId": "ROOM_ID" }
```

**Server Emit:**
```json
{ "roomId": "ROOM_ID" } // Event: 'joined_room'
```

---

### 🔹 `leave_room`

Leaves a specific chat room.

**Client Emit:**
```json
{ "roomId": "ROOM_ID" }
```

**Server Emit:**
```json
{ "roomId": "ROOM_ID" } // Event: 'left_room'
```

---

### 🔹 `typing_start`

Notifies others in the room that the user has started typing.

**Client Emit:**
```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast:**
```json
{
  "userId": "USER_ID",
  "username": "USERNAME",
  "roomId": "ROOM_ID"
} // Event: 'user_typing'
```

---

### 🔹 `typing_stop`

Notifies others in the room that the user has stopped typing.

**Client Emit:**
```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast:**
```json
{
  "userId": "USER_ID",
  "roomId": "ROOM_ID"
} // Event: 'user_stop_typing'
```

---

### 🔹 `send_message`

Sends a message to a specific room.

**Client Emit:**
```json
{
  "roomId": "ROOM_ID",
  "message": "Hello!",
  "receiverId": "RECEIVER_ID",   // Optional
  "messageType": "text"          // Default: 'text'
}
```

**Server Broadcast:**
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
} // Event: 'new_message'
```

---

### 🔹 `mark_read`

Marks all messages in a room as read by the current user.

**Client Emit:**
```json
{ "roomId": "ROOM_ID" }
```

**Server Broadcast:**
```json
{
  "userId": "USER_ID",
  "roomId": "ROOM_ID"
} // Event: 'messages_read'
```

---

## Stream Events
```
start_stream
join_stream
end_stream
chat_message_stream  ---- events emit
 
listen events--
stream_started
viewer_joined
stream_ended
stream_chat_message
```

## 📝 Notes

- All REST API endpoints require authentication via bearer token.
- Socket.IO also requires authentication using the `auth.token` parameter during connection.
- Ensure to emit `join_room` before sending or listening for messages in a room.

---

## 📞 Contact

For support or integration help, please contact the backend team or raise an issue in your project tracker.
