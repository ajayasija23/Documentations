# 🎁 Gifting Flow – Live Streaming App

This document outlines the gifting flow logic used in a live streaming platform. It supports different scenarios including **non-PK**, **Solo PK**, and **Team PK** sessions.

---

## 🟩 Gifting When Not in PK

- Deduct the gift’s coin cost from the **sender’s balance**.
- Credit the gift to the **receiver’s account**.
- Validate that the sender has sufficient balance before proceeding.

---

## 🟦 Gifting During Solo PK

- The process remains the same as in the non-PK scenario:
  - Deduct coins from the sender.
  - Credit the gift to the selected host.
- Additionally, keep track of the **total number of gifts received by each host**.
- This data is used to determine the **PK result** at the end of the session.

---

## 🟨 Gifting During Team PK

- Follow the same base flow as in Solo PK.
- Instead of tracking gifts per host, **aggregate gift totals for each team**.
- The outcome of the PK battle is based on the **team’s total gift value**.

---

## ⚙️ General Notes

- Make sure real-time events are emitted to update all participants in the room.
- Display gift animations and update scores on both the sender and receiver sides.
- Ensure backend validates room state (PK active/inactive) and user roles.

---

## ✅ Summary

| Scenario      | Coin Deduction | Gift Crediting | Score Tracking |
|---------------|----------------|----------------|----------------|
| Not in PK     | ✅              | ✅              | ❌              |
| Solo PK       | ✅              | ✅              | Per Host       |
| Team PK       | ✅              | ✅              | Per Team       |

---

## 📌 Conclusion

This gifting flow ensures a seamless experience for both viewers and streamers, while supporting competitive PK formats. It can be easily extended with features like combo gifts, multipliers, and real-time leaderboards.
