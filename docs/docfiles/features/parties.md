---
sidebar_position: 5
title: Parties
---

# Parties

Friends can queue together. A party holds one place in the queue and gets all its slots at once, so nobody is left
behind in the queue while the others play.

1. The leader calls **Join Queue With Party** with the server address and the other members' ids.
2. Each member calls **Join Queue As Party Member** with the same address and the leader's id.

The ids come from your own party or lobby system: a bare player id (for example a SteamID64), or a queue id from
**Get Player Queue Id** on a player state's unique net id. A member may call before the leader; their client keeps
asking until the leader's party is in the queue.

From the console:

```text
ServerQueue.JoinWithParty 203.0.113.10 76561198000000002 76561198000000003
ServerQueue.JoinParty 203.0.113.10 76561198000000001
```

## Rules

| Rule | Behaviour |
|---|---|
| Who can join | Only players the leader listed. A member who joins leaves any other place in that queue |
| Forming | Until every member joined, the party keeps its place but gets no slots, and does not hold anyone up. After **Party Form Timeout**, members who have not joined are left out and the rest go on |
| Priority | The lowest priority among the leader and the members who joined, so a party never moves anyone ahead of their own priority |
| Getting slots | When the party is first in line, free slots are held for it until there are enough for everyone. Later players and direct logins cannot take them; higher priorities still go first, as always |
| Already playing | Members already on the server are left out; with nobody left, the leader queues alone |
| Leaving | A member leaving (or declining) leaves the rest waiting. The leader leaving takes the party out of the queue; members get **On Join Refused** with **Party Disbanded** |
| Disconnects | Every player keeps their place for **Queue Reconnect Timeout**, like a single player. A leader who stays away takes the party along |
| Too large | Parties larger than **Max Party Size** or the server are refused (**Party Too Large**). A party that can never fit the slots its priority may use (for example after a settings change) is removed |

**Max Party Size** is 4 by default; 1 turns parties off.

## Progress

While waiting, **On Party Updated** reports how many members are in the queue out of the party size, and the queue
panel shows "Waiting for your party (2 of 4 here)". Queue position and length count places, so a waiting party
counts once.
