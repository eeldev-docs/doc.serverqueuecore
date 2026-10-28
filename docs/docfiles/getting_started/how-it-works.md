---
sidebar_position: 4
title: How it works
---

# How it works

## Waiting at the beacon

Waiting players do not join the game. They hold a lightweight **Online Beacon** connection on a separate port
(15000 by default), and the server sends them position updates. When a slot is ready, the player gets a one-time
token and connects to the game with `?SQToken=<token>`. The server checks the token before letting them in.

A full game server therefore never has to accept and kick waiting players, and waiting costs it almost nothing.

## Slots

The server's capacity (**Total Slots**) is split into **public** and **reserved** slots. Every player has a
priority: **Regular**, **Supporter**, **VIP** or **Admin**. The queue serves higher priorities first, and join order
within a priority. See [Priorities](../features/priorities).

A player's slot goes through these states:

| State | Meaning | Ends when |
|---|---|---|
| Reserved | A slot is held for the player, who has not logged in yet | They log in, or **Reservation Timeout** passes |
| In game | The player is playing | They leave |
| Grace | The player left and can come back without queueing | They return, or **Reconnect Grace Period** passes |

## Map changes and reconnects

Map changes are handled:

- Players in the game return through their grace period.
- Waiting players reconnect to the queue automatically and keep their position. The server keeps it for
  **Queue Reconnect Timeout**, and the client keeps trying for **Client Reconnect Duration**.

## Versions

Clients and servers must run the same plugin version. The queue protocol (version 3 since 0.6.0) is checked when a
client connects, and a mismatch is rejected with **Protocol Mismatch**.
