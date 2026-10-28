---
sidebar_position: 1
slug: /
title: Introduction
---

# ServerQueueCore

**Server queue and reserved slots for Unreal Engine dedicated servers.**

When a dedicated server is full, players normally get "server full" and have to try again and again.
ServerQueueCore gives them a place in line instead. Waiting players hold a lightweight beacon connection, see their
position and an estimated wait, and travel in by themselves when a slot is free. Supporters, VIPs and admins can be
served first, keep reserved slots, and friends can queue together as a party.

The plugin is **online subsystem agnostic**: it works with Null, Steam, EOS or your own backend. Everything is
available from **Blueprint and C++**.

:::info Beta
ServerQueueCore is in beta (version 0.7.0). The API can still change between versions.
:::

:::tip
New to the plugin? Start with [Installing the plugin](docfiles/getting_started/installing-plugin) and the
[Quick Start](docfiles/getting_started/quick-start), or run the [Demo](docfiles/getting_started/demo) to see a
working queue in a few minutes.
:::

## Features

| Feature | Description |
|---|---|
| **[Queue](docfiles/getting_started/how-it-works)** | Waiting players hold a beacon connection, get position updates and a one-time token when a slot is ready. |
| **[Gated logins](docfiles/features/login-rules)** | The game server only lets in players with a reservation, a grace period or a free slot. |
| **[Reserved slots](docfiles/features/priorities#reserved-slots)** | Part of the capacity is kept for higher priorities, optionally lent out while unused. |
| **[Priorities](docfiles/features/priorities)** | Regular, Supporter, VIP and Admin from a config list, a JSON file, an HTTP service or Blueprint. |
| **[Player verification](docfiles/features/player-verification)** | Only verified players get into the queue: HTTP, Blueprint or [Steam](docfiles/features/steam). |
| **[Priority kick](docfiles/features/priority-kick)** | Admins get in right away on a full server; the lowest priority player makes room. |
| **[Parties](docfiles/features/parties)** | Friends hold one place in the queue and get all their slots at once. |
| **[Map changes and reconnects](docfiles/getting_started/how-it-works#map-changes-and-reconnects)** | Players in the game come back through a grace period; waiting players keep their position. |
| **[Server browser](docfiles/features/server-browser)** | Publishes the number of waiting players into the online session: "64/64 (+12 waiting)". |
| **[Queue panel](docfiles/features/queue-panel)** | A ready-made widget with address box, Join and Leave, position and wait estimate. |
| **[Setup validation](docfiles/getting_started/quick-start#validate-the-setup)** | One click checks the beacon, game session, slots and providers. |
| **[Blueprint](docfiles/blueprint/client) + [C++](docfiles/cpp/cpp-overview)** | Subsystems, events and nodes, plus a networking-free queue core you can drive and test directly. |

## Supported engine versions

| Engine | Status |
|---|---|
| Unreal Engine 5.8 | ✅ Supported |

## Supported platforms and backends

| Backend | Status |
|---|---|
| Null / LAN | ✅ Supported |
| Steam (SteamCore PRO or OnlineSubsystemSteam) | ✅ Supported, with Steam verification through the [Server Queue Steam](docfiles/features/steam) add-on |
| EOS | ✅ Supported. There is no built-in EOS verifier: send an EOS id token and check it with the [HTTP or Blueprint auth provider](docfiles/features/player-verification) |
| Your own backend | ✅ Supported |

Windows and Linux.
