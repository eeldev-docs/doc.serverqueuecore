---
sidebar_position: 1
title: Client
---

# Client

**Server Queue Client Subsystem** (Game Instance subsystem) is everything a client needs to queue for a server.

## Functions

| Function | Description |
|---|---|
| **Set Auth Ticket** | The ticket sent to the server's [auth provider](../features/player-verification). Call before **Join Queue** |
| **Join Queue** | Queues for a server (`host[:beacon port]`) and travels there when a slot is ready |
| **Join Queue With Party** | Queues as [party](../features/parties) leader with the listed member ids |
| **Join Queue As Party Member** | Joins the leader's party in the queue |
| **Leave Queue** | Leaves the queue |
| **Travel To Server** | Travels to the server with the reserved slot, when **Auto Travel On Slot Ready** is off |
| **Connect To Server** | Connects to a server's queue beacon without queueing, for example to ping it |
| **Disconnect** | Closes the beacon connection |
| **Ping** | Pings the beacon; the answer arrives in **On Pong** |
| **Get State** | The client state, see below |
| **Get Queue Position**, **Get Queue Length** | Position and length in places; a waiting party counts once |
| **Get Estimated Wait** | Estimated wait in seconds |
| **Is In Party**, **Get Party Num Joined**, **Get Party Size** | Party progress while waiting |

## Events

| Event | When |
|---|---|
| **On State Changed** | The client state changed |
| **On Queue Updated** | New position, queue length and wait estimate |
| **On Party Updated** | How many party members are in the queue, out of the party size |
| **On Slot Ready** | A slot is reserved; the client travels unless **Auto Travel On Slot Ready** is off |
| **On Join Refused** | The queue refused the join, with the reason (below) |
| **On Reservation Expired** | The reserved slot was not used in time |
| **On Welcome** | Connected to the queue beacon (and verified, with an auth provider) |
| **On Pong** | Answer to **Ping**, with the round trip and the server's info |
| **On Rejected** | The server rejected the connection: protocol mismatch, no usable player id, or shutting down. The client is already Idle and does not reconnect |
| **On Connection Lost** | The queue connection is gone and the client stopped trying, or the server refused to verify the player |

## Client states

| State | Meaning |
|---|---|
| Idle | Not connected to a queue |
| Connecting | Connecting to the server's queue beacon |
| Connected | Connected, not in the queue |
| Queued | Waiting in the queue |
| Slot Ready | A slot is reserved; the client may travel to the server |
| Reconnecting | Lost the queue connection and trying to get the position back |

## Join results

| Result | Meaning |
|---|---|
| Slot Ready | A slot is reserved for the player; it may connect now |
| Queued | The player waits in the queue |
| Already In Game | The player already holds an active slot on the server |
| Queue Full | The queue is at **Max Queue Length** |
| Invalid Player | The player id is not usable |
| Party Too Large | The party has more players than **Max Party Size** or the server's slots |
| Party Not Found | The party leader is not in the queue with a party (yet) |
| Not In Party | The party leader did not list this player |
| Party Disbanded | The party left the queue: the leader left, or it was removed |
| Removed By Admin | The server took the player out of the queue with **Remove Player** |

## Blueprint library

**Server Queue Blueprint Library**:

| Function | Description |
|---|---|
| **Format Estimated Wait** | Turns seconds into display text |
| **Get Client State Text** | Display text for a client state |
| **Get Session Queue Length** | The number of waiting players on a session search result, see [Server browser](../features/server-browser) |
| **Get Session Queue Address** | The queue address ("host:port") of a session search result, for **Join Queue**. See [Server browser](../features/server-browser) |
| **Get Player Queue Id** | The queue id for a unique net id, for [parties](../features/parties) |
