# 🎁 Live Streaming Gifting Flow Documentation

This document outlines the backend logic and flow of the gifting system used in a live streaming platform. The system supports both **regular live sessions** and **PK sessions** (Solo PK and Team PK).

---

## 📌 Overview

The gifting system enables users to send virtual gifts to hosts during live streams. Gifts impact host visibility, earnings, and in PK sessions, determine outcomes based on gift scores.

---

## 🎬 Gifting Flow (Non-PK)

1. **User Sends Gift**:
   - Sender selects a gift and target host.
   - Frontend sends the request to the backend.

2. **Backend Flow**:
   - Check sender’s wallet balance.
   - Deduct coins based on gift value.
   - Credit gift to the host.
   - Update gift logs and statistics.

3. **Response & Broadcast**:
   - Notify sender of success/failure.
   - Broadcast the gift to all viewers via socket/websocket.

---

## ⚔️ Gifting Flow (PK Mode)

PK Mode includes:
- **Solo PK**: Host vs Host
- **Team PK**: Team A vs Team B

### 1. **Sender Initiates Gift**
- Sender chooses a host (who is currently in a PK session).
- Sends gift to that host.

### 2. **Backend Flow**
- Deduct coins from sender.
- Credit gift to the receiver.
- Update:
  - Host's gift stats
  - PK session stats (Host/Team scores)
  - Real-time PK leaderboard (optional)

### 3. **Special Considerations**
- Ensure the PK session is active.
- Validate if host/team is participating.
- Assign the gift points to the correct PK side (Solo or Team).
- Emit updated PK scores via socket to frontend.

---

## 🧠 PK Result Evaluation

- After the PK ends, system compares total gift points:
  - **Solo PK**: Host A vs Host B
  - **Team PK**: Team A vs Team B

- Winner is determined based on total gift value during PK duration.
- Tie logic or bonus rounds can be configured if needed.

---

## 📊 Optional Enhancements

- Combo gifts
- Gift animations
- Event-based bonuses (e.g., double gift points)
- Analytics dashboard
- VIP-only gifts

---

## ✅ Summary

| Feature            | Supported |
|--------------------|-----------|
| Real-time delivery | ✅         |
| PK-aware gifting   | ✅         |
| Score tracking     | ✅         |
| Modular logic      | ✅         |

---

## 📞 Need Help?

For questions or integration support, feel free to reach out to the backend team.

