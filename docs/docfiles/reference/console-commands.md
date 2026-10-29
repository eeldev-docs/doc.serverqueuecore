---
sidebar_position: 2
title: Console commands
---

# Console commands

| Command | Side | Description |
|---|---|---|
| `ServerQueue.Join <host[:port]>` | Client | Queues for a server and travels there when a slot is ready |
| `ServerQueue.JoinWithParty <host[:port]> <member id>...` | Client | Queues as party leader with the listed members |
| `ServerQueue.JoinParty <host[:port]> <leader id>` | Client | Joins the leader's party in the queue |
| `ServerQueue.Leave` | Client | Leaves the queue |
| `ServerQueue.Connect <host[:port]>` | Client | Connects to a server's queue beacon without queueing |
| `ServerQueue.Ping` | Client | Pings the beacon and logs the round trip |
| `ServerQueue.Disconnect` | Client | Closes the beacon connection |
| `ServerQueue.Status` | Both | Prints beacon state, slots and the queue |
| `ServerQueue.Reload` | Server | Rereads settings, recreates providers, forgets cached and admin priorities, and looks up the priorities of waiting and playing players again |
| `ServerQueue.Remove <key or id>` | Server | Removes a waiting player or cancels their unused reservation |
| `ServerQueue.Priority <key or id> <priority>` | Server | Sets a player's priority until the next reload; reorders the queue |
| `ServerQueue.Validate` | Both | Checks the setup and prints problems |

The port is the beacon port (15000 by default).

Server commands work over any remote console your server offers, since they are ordinary console commands.
