---
title: Changelog
---

# Changelog

## Unreleased

### New

- **Get Session Queue Address** on the Blueprint library: the queue address ("host:port") of a session search result,
  to pass to **Join Queue** from a server browser. The server now publishes its beacon port in the online session
  (`BEACONPORT`), which the online subsystems use to resolve it. See [Server browser](docfiles/features/server-browser).
- A session created after the map loaded, for example from Blueprint, gets the queue length and beacon port at once
  instead of on the next change.
- **Get Player Priority** on the Server Queue Subsystem: the priority the queue has for a player, for example to allow
  admin actions only for admins.
- New join result **Removed By Admin**.
- **Shut Down Queue** and **Is Shutting Down** on the Server Queue Beacon Host Subsystem: tell the waiting players that
  the server is shutting down before it quits.
- Command line `-QueueLendReservedSlots` turns on **Lend Reserved Slots**.
- `ServerQueue.Validate` warns when GameNetDriver and BeaconNetDriver use different driver classes, for example the
  game on Steam sockets and the queue beacon on `IpNetDriver`, which needs an open beacon port.

### Changed

- **Reload Settings** and `ServerQueue.Reload` look up the priority of every waiting and playing player again, so a
  changed priority file or provider reorders the queue at once. Before, waiting players kept their old priority, and
  admins lost theirs until they joined again.
- A client that the server rejects goes to Idle at once, before **On Rejected**, instead of when the server closes the
  connection a second later.

### Fixed

- The reject reason **Server Shutting Down** was never sent. **Shut Down Queue** sends it.

- A player taken out of the queue with **Remove Player** was not told, and kept showing their old place in the queue.
  They now get **On Join Refused** with **Removed By Admin**.
- **Set Player Priority** with an empty player id is ignored.

- Compiles on Unreal Engine 5.6 and 5.7: the queue panel no longer uses the address box style getter added in 5.7.

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
