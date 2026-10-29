---
sidebar_position: 3
title: Demo
---

# Demo

The plugin comes with a demo map, `/ServerQueueCore/Demo/ServerQueueDemo` (**Plugins → Server Queue Core Content →
Demo** in the Content Browser). It uses `ServerQueueDemoGameMode`, which sets the game session and shows the
[queue panel](../features/queue-panel).

## Run it

1. Start a server with one slot on the demo map:

   ```bash
   UnrealEditor.exe YourProject.uproject /ServerQueueCore/Demo/ServerQueueDemo -server -log -QueueTotalSlots=1
   ```

2. Open the demo map in the editor and play it as **Standalone**, twice.
3. Press **Join** in both windows.

The first player gets in. The second waits in the queue, with their position and an estimated wait, and travels in
by themselves as soon as the first player leaves.

For priorities, parties, player verification and the server browser in Blueprint, see the
[example project](example-project).

:::tip
Enable **Show Plugin Content** in the Content Browser settings if you can't see the plugin's content folder.
:::
