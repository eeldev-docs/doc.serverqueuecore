---
sidebar_position: 1
title: Priorities and reserved slots
---

# Priorities and reserved slots

Every player has a priority: **Regular**, **Supporter**, **VIP** or **Admin**. The queue serves higher priorities
first, and join order within a priority.

## Permission providers

Permission providers decide each player's priority. A player gets the highest priority any active provider returns.
Results are cached for **Priority Cache Duration** (5 minutes by default).

| Provider | Source |
|---|---|
| Config (on by default) | **Priority List** in Project Settings |
| File | A JSON file on the server, reread when it changes |
| HTTP | `GET` **Priority Url**, with `{id}`, `{type}` and `{key}` replaced |
| Blueprint | Subclass **Blueprint Permission Provider** and override **Get Priority** |

Add providers to **Permission Providers** in Project Settings, or pass one on the server command line with
`-QueuePermissionProvider=<class path>`.

### Player ids

Ids can be the bare id (`76561198000000001`) or the full key with the online subsystem's id type
(`STEAM:76561198000000001`).

### File

**Priority File** defaults to `Saved/ServerQueue/Priorities.json` (`-QueuePriorityFile=` overrides it). Relative
paths start at the project directory.

```json
{
  "players": [
    { "id": "76561198000000001", "priority": "VIP" },
    { "id": "STEAM:76561198000000002", "priority": "Admin" }
  ]
}
```

### HTTP

The server sends `GET` to **Priority Url**, for example `https://api.example.com/priority?id={id}&type={type}`, and
expects:

```json
{ "priority": "VIP" }
```

**Http Timeout** (5 s) limits how long it waits.

### Direct logins

Players joining through the queue always get their looked-up priority. A direct login (no queue) cannot wait for a
lookup, so it uses the cached priority, or Regular on a first visit.

### Changing priorities on a running server

- `ServerQueue.Priority <key or id> <priority>` sets a player's priority until the next reload and reorders the queue.
- `ServerQueue.Reload` rereads the settings, recreates the providers and forgets cached and admin-set priorities.

## Reserved slots

**Reserved Slots** is the part of **Total Slots** kept for higher priorities. Players with at least **Reserved Slot
Min Priority** (Supporter by default) use reserved slots first and public slots after that. Regular players only use
public slots.

With **Lend Reserved Slots** on, Regular players may also use free reserved slots. A lent slot returns to the
reserved pool when its player leaves.
