---
title: Changelog
---

# Changelog

## 0.7.0

### New

- **A new ticket for every connection.** `UServerQueueClientSubsystem::OnMakeAuthTicket()` lets the game make a new
  ticket each time the client connects to a queue, including reconnects. It wins over **Set Auth Ticket**; return an
  empty string to fall back to it. This is for tickets that work only once, such as Steam session tickets.
- **Server Queue Steam.** A new add-on plugin that verifies players with Steam session tickets. See
  [Steam](docfiles/features/steam).

## 0.6.0

### New

- **Party queues.** Friends can queue together. The leader queues with the other members' ids, each member joins the
  leader's party, and the party holds one place in the queue and gets all its slots at once. See
  [Parties](docfiles/features/parties).
- **Join Queue With Party**, **Join Queue As Party Member** and **On Party Updated** on the client subsystem. Members
  keep asking while the leader's party is not in the queue yet.
- New settings **Max Party Size** and **Party Form Timeout**, with setup validation.
- New join results **Party Too Large**, **Party Not Found**, **Not In Party** and **Party Disbanded**.
- Console commands `ServerQueue.JoinWithParty` and `ServerQueue.JoinParty`.
- The queue panel shows the party status: "Waiting for your party (2 of 4 here)".
- **Get Player Queue Id** on the Blueprint library.

### Changed

- The queue protocol is now version 3. Clients and servers must run the same plugin version.
- `SQ_QUEUE` and **On Queue Status Changed** count waiting players, party members included.

## 0.5.0

First release: queue beacon with token-gated logins, priorities from permission providers, player verification,
priority kick, queue panel, demo map, setup validation and server browser integration.
