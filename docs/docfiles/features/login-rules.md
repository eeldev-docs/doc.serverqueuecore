---
sidebar_position: 6
title: Login rules
---

# Login rules

With `ServerQueueGameSession` (or your own game session calling `CaptureLoginOptions`), the game server checks every
login:

| Player | Result |
|---|---|
| Holds a reservation and presents its token | Let in |
| Holds a reservation, wrong or missing token | Refused |
| In their grace period (with or without token) | Let in |
| No slot, a slot is free for them and **Allow Direct Join** is on | Let in |
| No slot otherwise | Refused: "ServerQueue: the server is full. Join through the queue." |

A reservation lasts **Reservation Timeout** (30 s). A player who leaves the game keeps their slot for **Reconnect
Grace Period** (120 s, 0 turns it off), so they can come back without queueing.
