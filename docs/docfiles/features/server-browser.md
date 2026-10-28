---
sidebar_position: 7
title: Server browser
---

# Server browser

The queue keeps the game session's **Max Players** at **Total Slots**. With **Publish Queue To Session** on, it also
writes the number of waiting players (party members included) into the online session, under **Session Queue Length
Key** (`SQ_QUEUE`). A server list can then show "64/64 (+12 waiting)".

Read it with **Get Session Queue Length** on a session search result.

For other server lists, for example Steam server rules or your own master server, bind **On Queue Status Changed** on
the **Server Queue Subsystem** and publish the numbers yourself.
