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
| **Reload Settings** | Rereads settings and recreates providers, like `ServerQueue.Reload` |
| **Get Queue Length** | Places in the queue; a party counts once |
| **Get Num Waiting Players** | Waiting players, party members included |
| **Get Num Players In Game** | Players in the game |
| **Get Num Free Slots** | Free slots |
| **Remove Player** | Removes a waiting player or cancels their unused reservation. Removing a party leader takes the party out of the queue |
| **Set Player Priority** | Sets a player's priority until the next reload and reorders the queue |
| **On Queue Status Changed** | The queue or the slots changed, for example to publish the numbers to your own server list |

## Server Queue Beacon Host Subsystem

World subsystem on the server. With **Auto Start Beacon Host** on (the default) the beacon starts by itself when a
server world begins play, so you only need it to control the beacon yourself.

| Function | Description |
|---|---|
| **Start Beacon Host** | Starts listening on the beacon port |
| **Stop Beacon Host** | Stops the beacon |
| **Is Hosting** | Whether the beacon is listening |

## Blueprint providers

Subclass these in Blueprint and add them in Project Settings:

| Class | Override | See |
|---|---|---|
| **Blueprint Permission Provider** | **Get Priority** | [Priorities](../features/priorities) |
| **Blueprint Auth Provider** | **Is Player Valid** | [Player verification](../features/player-verification) |
