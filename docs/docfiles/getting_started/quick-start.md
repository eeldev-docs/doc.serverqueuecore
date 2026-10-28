---
sidebar_position: 2
title: Quick Start
---

# Quick Start

## 1. Use the queue's game session

Set your game mode's **Game Session Class** to `ServerQueueGameSession`.

Already have your own game session class? Call `UServerQueueSubsystem::CaptureLoginOptions(Options)` from its
`ApproveLogin`, and return `false` from `AtCapacity` while the queue is active.

:::caution
Without this step the queue still tracks slots, but it cannot check the tokens players log in with, and it writes a
warning to the log.
:::

## 2. Open the beacon port

Waiting players connect to the queue beacon, which listens on its own port next to the game port:
**UDP 15000** by default. Open it in your firewall and on your hosting provider, or change it with **Beacon Port**
(or `-BeaconPort=` on the server command line).

`BeaconNetDriver` is defined in the engine's `BaseEngine.ini`, so no extra net driver setup is needed unless your
project removed it.

## 3. Set the slots

**Project Settings → Plugins → Server Queue**: set **Total Slots** (the server's capacity) and, if you want them,
**Reserved Slots** for higher priorities. See [Plugin Settings](../reference/plugin-settings) for everything else.

## 4. Join through the queue

On the client, call **Join Queue** on the **Server Queue Client Subsystem** with the server address, instead of
opening the server directly:

```text
Join Queue  Address: 203.0.113.10        (beacon port 15000)
Join Queue  Address: 203.0.113.10:15001  (another beacon port)
```

The client connects to the queue. When a slot is ready, it travels to the server by itself (turn off **Auto Travel
On Slot Ready** to do that yourself with **Travel To Server**). Show the progress with the
[queue panel](../features/queue-panel), or bind **On Queue Updated** and **On Slot Ready** in your own UI.

From the console: `ServerQueue.Join 203.0.113.10`.

## Validate the setup

Run **Tools → Validate Server Queue Setup** in the editor, or `ServerQueue.Validate` in the console. It checks the
beacon net driver and port, your game modes' game session, the slot settings and the providers.

A server runs the same checks when the queue starts and writes any problems to the log.
