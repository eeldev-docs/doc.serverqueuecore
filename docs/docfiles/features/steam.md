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

The dedicated server must be logged on to Steam. Both Steam online subsystems log it on anonymously when the server
creates an internet (non-LAN) session, unless it is already logged on (see [Login token](#login-token)). Until Steam
answers, the player waits at the beacon; after **Verify Timeout** they are refused.

Refusals are logged on the server, for example
`STEAMCORE:7656... failed verification: Steam: no license for the game`.

## Login token

An anonymous log on is enough to verify players. To give the server a persistent Steam account, log it on with a
game server login token (GSLT) **before** it creates the session; the online subsystem then keeps that log on.

1. Create a token at [Steam Game Server Account Management](https://steamcommunity.com/dev/managegameservers), with
   your game's App ID. A token only logs on for that app, and one server at a time can use it.
2. Give it to the server from an environment variable or a file, not the command line, which the server logs.
3. Call **Log On** on SteamCore's **Steam Game Server**, wait until **B Logged On** is true, then create the session.

```cpp
#include "SteamGameServer/SteamProGameServer.h"

if (USteamProGameServer* SteamServer = USteamProGameServer::GetSteamGameServer())
{
	SteamServer->LogOn(FPlatformMisc::GetEnvironmentVariable(TEXT("STEAM_GSLT")));
}

// Later, for example on a timer: create the internet session once SteamServer->BLoggedOn() is true.
```

The server's Steam id shows which log on it got: account type 3 (bits 52 to 55 of `GetServerSteamID()`) is a
persistent account, 4 is anonymous.

## Steam sockets

The queue beacon uses the net driver set for **BeaconNetDriver**, not the game's **GameNetDriver**. By default both
are the engine's `IpNetDriver`, so the beacon port (15000) is a UDP port that must be open on the server, like the
game port.

To send the queue over Steam's network too, set both definitions to SteamCore's sockets driver in
`DefaultEngine.ini`, with `IpNetDriver` as fallback, and turn on `bUseSteamNetworking`:

```ini
[/Script/Engine.Engine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="/Script/SteamCoreSockets.SteamCoreSocketsNetDriver",DriverClassNameFallback="/Script/OnlineSubsystemUtils.IpNetDriver")
+NetDriverDefinitions=(DefName="BeaconNetDriver",DriverClassName="/Script/SteamCoreSockets.SteamCoreSocketsNetDriver",DriverClassNameFallback="/Script/OnlineSubsystemUtils.IpNetDriver")
+NetDriverDefinitions=(DefName="DemoNetDriver",DriverClassName="/Script/Engine.DemoNetDriver",DriverClassNameFallback="/Script/Engine.DemoNetDriver")

[OnlineSubsystemSteamCore]
bUseSteamNetworking=true
bAllowP2PPacketRelay=true
```

With relays on (`bAllowP2PPacketRelay`), the server listens on Steam P2P. The beacon port and the game port are then
virtual ports on the server's Steam id, so no port has to be opened, and the queue address is
`steam.<server Steam id>:15000`. **Get Session Queue Address** gives that address for a search result, and the slot
travels to `steam.<server Steam id>:<game port>`. With relays off, the driver listens on real UDP ports with Steam's
protocol, and the address is `ip:15000` as before.

The beacon and game ports must differ in both cases. When Steam networking is off or Steam is not running, the
fallback `IpNetDriver` is used.

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
