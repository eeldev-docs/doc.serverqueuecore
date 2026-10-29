---
sidebar_position: 2
title: Server
---

# Server

## Server Queue Subsystem

Game Instance subsystem, active on the server.

| Function / event | Description |
|---|---|
| **Is Active** | Whether the queue runs on this server |
| **Reload Settings** | Rereads settings and recreates providers, like `ServerQueue.Reload`, then looks up the priority of every waiting and playing player again |
| **Get Queue Length** | Places in the queue; a party counts once |
| **Get Num Waiting Players** | Waiting players, party members included |
| **Get Num Players In Game** | Players in the game |
| **Get Num Free Slots** | Free slots |
| **Get Player Priority** | The priority the queue has for a player: the one from **Set Player Priority**, else the last one the permission providers gave. Regular for a player it does not know. Use it to check that a player who asks for an admin action is an admin |
| **Remove Player** | Removes a waiting player or cancels their unused reservation. Removing a party leader takes the party out of the queue. The removed player gets **On Join Refused** with **Removed By Admin** |
| **Set Player Priority** | Sets a player's priority until the next reload and reorders the queue |
| **On Queue Status Changed** | The queue or the slots changed, for example to publish the numbers to your own server list |

## Server Queue Beacon Host Subsystem

World subsystem on the server. With **Auto Start Beacon Host** on (the default) the beacon starts by itself when a
server world begins play, so you only need it to control the beacon yourself.

| Function | Description |
|---|---|
| **Start Beacon Host** | Starts listening on the beacon port |
| **Stop Beacon Host** | Stops the beacon. Waiting players lose the connection and try to reconnect for **Client Reconnect Duration**, so starting it again within that time keeps their places |
| **Shut Down Queue** | Tells every connected player that the server is shutting down (**On Rejected** with **Server Shutting Down**) and refuses new ones. Call it a few seconds before the server quits, so the players hear it before the connection closes |
| **Is Shutting Down** | Whether **Shut Down Queue** was called |
| **Is Hosting** | Whether the beacon is listening |

## Blueprint providers

Subclass these in Blueprint and add them in Project Settings:

| Class | Override | See |
|---|---|---|
| **Blueprint Permission Provider** | **Get Priority** | [Priorities](../features/priorities) |
| **Blueprint Auth Provider** | **Is Player Valid** | [Player verification](../features/player-verification) |
