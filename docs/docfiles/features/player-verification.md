---
sidebar_position: 2
title: Player verification
---

# Player verification

Without an auth provider, the queue trusts the player id the connection carries, like a normal login. With one, the
queue beacon asks the client for a ticket and lets only verified players into the queue.

| Provider | Check |
|---|---|
| HTTP | `POST` to **Auth Url** with the player id, id type and ticket |
| Blueprint | Subclass **Blueprint Auth Provider** and override **Is Player Valid** |
| Steam | Steam session tickets through the Steam game server API, in the [Server Queue Steam](steam) add-on |
| Your own | Subclass `UServerQueueAuthProvider` in C++, see [Custom providers](../cpp/custom-providers) |

Set **Auth Provider** in Project Settings, or start the server with `-QueueAuthProvider=<class path>`.

:::caution Turn off Allow Direct Join
While **Allow Direct Join** is on, players can still connect straight to the game while slots are free, without
being verified. Turn it off when verification matters, so every player comes through the verified queue. The setup
validation warns about this.
:::

## The ticket

On the client, call **Set Auth Ticket** before **Join Queue** with the ticket your backend checks: an EOS id token,
your own session token, and so on. Without it, the online subsystem's access token is sent.

Tickets that work only once, such as Steam session tickets, need a new one for every connection, reconnects
included. Bind `OnMakeAuthTicket()` on the client subsystem in C++ for that (Server Queue Steam does it for you).

## HTTP

The server sends a `POST` to **Auth Url**:

```json
{ "playerId": "76561198000000001", "idType": "STEAM", "ticket": "..." }
```

The player is verified when the response is a 2xx with:

```json
{ "valid": true }
```

## Logs

The server logs every result, for example `STEAM:7656... verified` or
`STEAM:7656... failed verification: <reason>`. A refused player's queue connection is closed, and the client gets
**On Connection Lost**.
