---
sidebar_position: 7
title: Server browser
---

# Server browser

The queue keeps the game session's **Max Players** at **Total Slots**. With **Publish Queue To Session** on, it also
writes the number of waiting players (party members included) into the online session, under **Session Queue Length
Key** (`SQ_QUEUE`). A server list can then show "64/64 (+12 waiting)".

Read it with **Get Session Queue Length** on a session search result.

For other server lists, for example Steam server rules or your own master server, bind **On Queue Status Changed** on
the **Server Queue Subsystem** and publish the numbers yourself.

## Joining from the server list

**Get Session Queue Address** gives a search result's queue address ("host:port"). Pass it to **Join Queue** instead
of joining the session directly, so the player waits in the queue when the server is full. The server publishes its
beacon port in the session for this (`BEACONPORT`).

```text
Find Sessions → For Each result → Get Session Queue Address → Join Queue
```

:::note
The NULL online subsystem leaves full servers out of LAN search results, so on LAN a full server, the one with a queue,
does not show up. Steam and other online services list full servers.
:::
