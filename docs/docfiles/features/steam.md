---
sidebar_position: 3
title: Steam
---

# Steam

**Server Queue Steam** is an add-on plugin that only lets players into the queue when Steam accepts their session
ticket. It is built on [SteamCore PRO](https://steamcore-pro.eeldev.com) and needs ServerQueueCore 0.7.0 or later.

## Setup

1. Enable **Server Queue Steam**. It enables ServerQueueCore and SteamCore PRO.
2. Run the game on Steam: SteamCore's online subsystem (`DefaultPlatformService=SteamCore`, see the SteamCore PRO
   docs) or the engine's **OnlineSubsystemSteam** (`DefaultPlatformService=Steam`).
3. **Project Settings → Plugins → Server Queue**: set **Auth Provider** to **Steam**, or start the server with
   `-QueueAuthProvider=/Script/ServerQueueSteam.ServerQueueSteamAuthProvider`.
4. Turn **Allow Direct Join** off, so every player comes through the verified queue.

Nothing else is needed on the client: each connection to a queue gets a new Steam session ticket.

## How it works

- **Client:** the **Server Queue Steam Ticket Subsystem** makes a session ticket for every connection to a queue,
  reconnects included. It cancels the ticket once the queue welcomed the player, when the connection is lost and on
  shutdown. It only makes tickets when the game runs on SteamCore or the engine's Steam online subsystem; otherwise
  the queue sends the ticket set with **Set Auth Ticket**, if any.
- **Server:** the Steam auth provider checks that the player's id is a Steam id, starts a Steam auth session with the
  ticket (`BeginAuthSession`), waits for Steam's answer and ends the session again (`EndAuthSession`), so the game
  can start its own for the same player later.

The dedicated server must be logged on to Steam. Both Steam online subsystems do that when the server creates an
internet (non-LAN) session. Until Steam answers, the player waits at the beacon; after **Verify Timeout** they are
refused.

Refusals are logged on the server, for example
`STEAMCORE:7656... failed verification: Steam: no license for the game`.

## Settings

**Project Settings → Plugins → Server Queue Steam**:

| Setting | Default | |
|---|---|---|
| Make Tickets | On | Clients send a new session ticket per connection. Turn off to send your own with **Set Auth Ticket** |
| Verify Timeout | 10 s | How long the server waits for Steam |
| Allow Family Sharing | On | Let in players who borrowed the game through Steam Family Sharing |

## Blueprint and C++

- **Server Queue Steam Ticket Subsystem**: **Make Ticket** (hex text, cancels the previous ticket), **Cancel Ticket**.
- `UServerQueueSteamAuthProvider` (`ServerQueueSteamAuthProvider.h`): `ParseSteamId`, `DecodeTicket`.

:::note
Tickets carry no network identity, so any server of your game can verify them while they are valid. The client
cancels each one as soon as the queue has verified it.
:::
