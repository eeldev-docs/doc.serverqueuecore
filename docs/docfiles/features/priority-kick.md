---
sidebar_position: 4
title: Priority kick
---

# Priority kick

With **Allow Priority Kick** on (or `-QueuePriorityKick` on the server command line), a player with at least
**Priority Kick Min Priority** (Admin by default) who queues on a full server gets a slot right away.

The slot comes from:

1. a lower priority player in their grace period, if there is one,
2. otherwise the lowest priority player in the game who joined last.

That player is kicked with **Priority Kick Message**. Implement `ClientWasKicked` in your player controller to show
the message to them.

Priority kick is for single players. Parties wait for free slots.
