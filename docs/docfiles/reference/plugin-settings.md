---
sidebar_position: 1
title: Plugin Settings
---

# Plugin Settings

**Project Settings → Plugins → Server Queue**, stored in `DefaultGame.ini` under
`[/Script/ServerQueueCore.ServerQueueSettings]`. Many settings can be overridden on the server command line.

## Slots and queue

| Setting | Default | Command line | Description |
|---|---|---|---|
| Total Slots | 16 | `-QueueTotalSlots=` | The server's capacity |
| Reserved Slots | 0 | `-QueueReservedSlots=` | Part of Total Slots kept for higher priorities |
| Lend Reserved Slots | Off | `-QueueLendReservedSlots` | Regular players may use free reserved slots |
| Reserved Slot Min Priority | Supporter | | Lowest priority that uses reserved slots |
| Max Queue Length | 100 | `-QueueMaxLength=` | Invited party members count |
| Max Party Size | 4 | `-QueueMaxPartySize=` | 1 turns parties off |
| Party Form Timeout | 60 s | | How long a party waits for its members |
| Reservation Timeout | 30 s | `-QueueReservationTimeout=` | How long a reserved slot waits for its player |
| Reconnect Grace Period | 120 s | `-QueueGracePeriod=` | How long a player who left keeps their slot; 0 turns it off |
| Queue Reconnect Timeout | 60 s | | How long a waiting player keeps their place after losing the connection |
| Allow Direct Join | On | | Players with no reservation may log in while a slot is free for them |
| Position Update Interval | 1 s | | How often waiting players get their position |

## Priorities and verification

| Setting | Default | Command line | Description |
|---|---|---|---|
| Permission Providers | Config | `-QueuePermissionProvider=<class path>` (adds one) | See [Priorities](../features/priorities) |
| Priority List | empty | | Used by the Config provider |
| Priority File | `Saved/ServerQueue/Priorities.json` | `-QueuePriorityFile=` | Used by the File provider |
| Priority Url | empty | | Used by the HTTP provider |
| Priority Cache Duration | 300 s | | How long looked-up priorities are kept |
| Auth Provider | none | `-QueueAuthProvider=<class path>` | See [Player verification](../features/player-verification) |
| Auth Url | empty | | Used by the HTTP auth provider |
| Http Timeout | 5 s | | For the HTTP providers |
| Allow Priority Kick | Off | `-QueuePriorityKick` | See [Priority kick](../features/priority-kick) |
| Priority Kick Min Priority | Admin | | |
| Priority Kick Message | | | Sent to the kicked player |

## Server browser

| Setting | Default | Description |
|---|---|---|
| Publish Queue To Session | On | Writes the number of waiting players into the online session |
| Session Queue Length Key | `SQ_QUEUE` | Session setting key for it |

## Client

| Setting | Default | Description |
|---|---|---|
| Auto Travel On Slot Ready | On | Travel to the server by itself when a slot is ready |
| Client Reconnect Duration | 45 s | How long to keep trying to reconnect to the queue after losing the connection |

## Beacon

| Setting | Default | Command line | Description |
|---|---|---|---|
| Auto Start Beacon Host | On | | Start the queue beacon when a server world begins play |
| Run On Listen Server | Off | | Also run the queue on listen servers. Dedicated servers always run it. The host's own player takes no slot |
| Beacon Port | 15000 | `-BeaconPort=` | Port the beacon listens on, and the default port clients connect to |
| Min Ping Interval | 0.25 s | | Faster pings from one client are ignored |
