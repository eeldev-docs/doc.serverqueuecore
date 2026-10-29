---
sidebar_position: 4
title: Example project
---

# Example project

**ServerQueueExample** is a Blueprint-only Unreal Engine 5.8 project that uses the plugin from start to finish:

- **Queue**: join a full server's queue, see your position and estimated wait, travel in when a slot is free.
- **[Priorities](../features/priorities) and reserved slots**: one of the two slots is reserved for VIP and up, and an
  Admin who finds the server full takes a regular player's slot ([priority kick](../features/priority-kick)).
- **[Player verification](../features/player-verification)**: a Blueprint auth provider refuses players whose ticket
  is "bad".
- **[Parties](../features/parties)**: queue as a party leader with your friends' queue ids, or join a leader's party.
- **[Server browser](../features/server-browser)**: find servers with their players, queue length and queue address,
  and join their queue.
- **Reconnect grace period**: leave the server and come back within 10 seconds, and your slot is still yours.
- **Ready check**: untick **Travel by itself** and the menu counts down with a **Join server** button when your slot is
  ready. Wait too long and the slot goes to the next player.
- **Ping**: ping a server's queue from the server browser to see its round trip, players and map before you join.
- **Several servers**: two servers on one computer, each with its own game and beacon port. The server browser lists
  both with their queue, so players can pick the shortest one.
- **Listen server**: **Host game** hosts the game in your own game, with the queue.
- **Lend Reserved Slots**: regular players borrow a free VIP slot instead of waiting.
- **Priority file**: give a player a priority in `Saved/ServerQueue/Priorities.json`, press **Reload settings**, and
  they move up the queue without a restart.
- **[Server status and admin tools](../blueprint/server)**: the HUD shows the server's queue and free slots, and
  admins can move a waiting player up, take them out of the queue, reload the settings, pause the queue, or shut the server down and tell
  the waiting players.
- **Ready-made queue panel**: the same queue in a Widget Blueprint with **Server Queue Widget** as parent, which needs
  no graph.
- **[Steam](../features/steam)**: the same project on Steam, with Steam ticket verification.

Everything is in `Content/ServerQueueExample`. There is no C++.

## Requirements

- Unreal Engine 5.8.
- The plugins **Server Queue Core**, **Server Queue Steam** and **SteamCore PRO**, in the engine or in the project's
  `Plugins` folder.

## Run it

Players start on the menu map (`Maps/Menu`), and the server runs `Maps/Game`.

1. Start a dedicated server from the project folder:

   ```bash
   UnrealEditor.exe ServerQueueExample.uproject /Game/ServerQueueExample/Maps/Game -server -log
   ```

   Or use `Start-Server.ps1`, which takes the queue settings as parameters, for example
   `.\Start-Server.ps1 -Slots 3 -ReservedSlots 0 -LendReservedSlots`. Its other parameters are `-MaxQueueLength`,
   `-Port`, `-BeaconPort`, `-PriorityFile`, `-Steam`, `-SteamSockets` and `-Engine` (the engine folder, `UE_5.8` by default).

2. Start three or more games, from the editor (**Play → Standalone Game**) or with:

   ```bash
   UnrealEditor.exe ServerQueueExample.uproject -game -windowed -ResX=800 -ResY=720
   ```

3. Press **Join queue** in each game.

The server has two slots, and one is reserved for VIP and up. The first player gets in, and the second waits in the
queue. Then try these:

| Try this | What happens |
|---|---|
| Type `vip` as the ticket, then **Join queue** | You get the reserved slot, past the waiting player |
| Type `admin` with a full server | A regular player is kicked to make room for you |
| Type `bad` | The auth provider refuses you |
| **Leave server** in the game | Your slot is kept for 10 seconds (**Reconnect Grace Period**), then goes to the next player in the queue |
| **Leave server**, then **Join queue** within 10 seconds | You get your slot back at once, past the waiting players |
| Type `admin`, get in, and put a waiting player's queue id in the HUD's **Admin** box | **Make VIP** moves them up the queue, and **Remove from queue** takes them out. Other players are refused |
| Untick **Travel by itself when the slot is ready**, then **Join queue** with a full server | When your slot is ready, the menu counts down and shows **Join server**. Wait 30 seconds (**Reservation Timeout**) and the slot goes to the next player |
| With two players waiting, put the second one's queue id in `Saved/ServerQueue/Priorities.json` as `{ "players": [ { "id": "<queue id>", "priority": "Supporter" } ] }`, then press **Reload settings** as admin | The second player moves up to first. Priorities from the file count for new players without a reload |
| As admin, **Pause queue**, then **Resume queue** within 45 seconds | Waiting players show Reconnecting, then get their places back (**Client Reconnect Duration**) |
| As admin, **Shut down server** | Waiting players are told that the server is shutting down, and the server quits 3 seconds later |
| Start the server with `-QueueMaxLength=1`, then three games | The third game is refused because the queue is full |
| Start the server with `-QueueLendReservedSlots`, then two games | The second player borrows the free VIP slot instead of waiting. A VIP who comes next waits first in the queue, and gets the slot when the borrower leaves |
| Start a second server with `-port=7778 -BeaconPort=15001`, then **Find servers** | Both servers are listed, each with its own queue address and queue length. **Join queue** on a row queues for that server |
| **Host game** in one game, then **Join queue** in two others | The first game hosts a listen server with the queue (**Run On Listen Server**). The host's own player takes no slot, so the first player gets in and the second waits |
| **Ready-made panel** | The same queue in a panel without a graph of its own. **Back to the example menu** returns |
| **Find servers** | The server shows up with its players, queue length and queue address |
| **Ping** on a server row | Connects to its queue beacon, shows the round trip, players and map, and disconnects |
| A party | Copy the members' queue ids from their menus into **Members' queue ids** and press **Queue as leader**. Then press **Join leader's party** in each member's game, with the leader's queue id |

The HUD in the game shows the server's queue, for example `Server: 1 waiting, 2/2 in game, free slots: 0 public,
0 reserved`. The server log shows it too, and [`ServerQueue.Status`](../reference/console-commands) prints all of it.

:::warning
The demo tickets are not secure, because any player can type `admin`. A real game verifies players with Server Queue
Steam or its own auth provider, and gives priorities by player id.
:::

## What is where

| Asset | Shows |
|---|---|
| `UI/WBP_QueueMenu` | The client side: **Join Queue**, **Join Queue With Party**, **Join Queue As Party Member**, **Leave Queue**, **Set Auth Ticket**, and the client subsystem's events (**On State Changed**, **On Queue Updated**, **On Party Updated**, **On Slot Ready**, **On Join Refused**, **On Reservation Expired**, **On Rejected**, **On Connection Lost**), and **Travel To Server** for the ready check. Also **Find Sessions** for the server browser, and **Open Level** with the option `listen` for **Host game**. The functions **Read Join Form** (ticket and address for the three Join buttons), **Set Status** and **Clear Queue Info** keep the event graph short. A spinner shows while the player waits |
| `UI/WBP_ServerRow` | One search result: its queue address from **Get Session Queue Address** (in the pure function **Get Queue Address**), **Get Session Queue Length**, then **Join Queue**. Its **Ping** button runs **Connect To Server**, **Ping** from **On Welcome**, shows **On Pong**, then **Disconnect** |
| `UI/WBP_QueuePanel` | The ready-made panel: a Widget Blueprint with **Server Queue Widget** as parent. Widgets named `AddressBox`, `JoinButton`, `LeaveButton`, `StatusText`, `PositionText` and `WaitText` are run by the parent class. Its graph only adds a spinner from **On Display Updated** and the Back button |
| `UI/WBP_GameHud` | In the game: the server's queue status, **Leave server**, and the **Admin** box |
| `Blueprints/BP_ExampleGameState` | On the server, **On Queue Status Changed** with **Get Num Waiting Players** and **Get Num Free Slots**, copied into replicated variables for the HUD |
| `Blueprints/BP_GamePlayerController` | Shows the HUD and runs its admin buttons on the server: the pure function **Is Admin** checks with **Get Player Priority** that the player is an admin, and **Get Admin Target Id** reads the HUD's Admin box, then **Set Player Priority**, **Remove Player**, **Reload Settings**, **Stop Beacon Host** and **Start Beacon Host** (pause and resume), or **Shut Down Queue** followed by quit three seconds later |
| `Blueprints/BP_ExampleGameMode` | The server side: **Server Queue Game Session** as the game session class, **On Queue Status Changed** written to the log, and the online session for the server browser (SteamCore PRO's **Create SteamCore PRO Session**, which works on a dedicated server). **Is Server** makes it run on a listen server too |
| `Blueprints/BP_MenuGameMode`, `BP_MenuPlayerController` | Show the menu |
| `Providers/BP_DemoPriorityProvider` | A **Blueprint Permission Provider**: priority by player id (**Id Priorities**), else by the demo ticket |
| `Providers/BP_DemoAuthProvider` | A **Blueprint Auth Provider**: refuses the ticket "bad" |

The queue settings are in `Config/DefaultGame.ini` (**Project Settings → Plugins → Server Queue**): two slots, one
reserved, no direct joins, a 10 second reconnect grace period, auto travel off (the menu travels by itself unless you untick it), priority kick for Admins, the queue on listen
servers, the two Blueprint providers, and the File permission provider. See
[Plugin settings](../reference/plugin-settings) for each setting.

## Server browser on LAN

The NULL online subsystem leaves full servers out of LAN search results. On LAN, press **Find servers** while the
server has a free slot. On Steam, full servers are listed too.

## Steam

Steam must be running, and each game needs its own Steam account. A second game on the same account is refused
because that account is already in the game. One account is enough to try it: start the server and one game on the
same computer, and join `127.0.0.1`. App 480 is Valve's Spacewar test app; use your own app id for a real game.

Add these to the server and to every game:

```bash
-ini:Engine:[OnlineSubsystem]:DefaultPlatformService=SteamCore -ini:Engine:[OnlineSubsystemSteamCore]:bEnabled=true
```

Add these to the server as well:

```bash
-InternetSession -QueueAuthProvider=/Script/ServerQueueSteam.ServerQueueSteamAuthProvider
```

`-InternetSession` makes `BP_ExampleGameMode` create an internet session, which logs the server on to Steam. The Steam
auth provider then lets in only players whose Steam session ticket Steam accepts, and each game sends a new ticket by
itself. Queue ids are SteamIDs, so give priorities in `BP_DemoPriorityProvider`'s **Id Priorities**. Untick **LAN**
before you press **Find servers**.

### Over Steam's network

The project's net drivers are SteamCore's Steam sockets, with `IpNetDriver` as fallback, so by default everything
runs over plain UDP. Add `-ini:Engine:[OnlineSubsystemSteamCore]:bUseSteamNetworking=true` to the server and the game,
or start the server with `.\Start-Server.ps1 -SteamSockets`. The game and the queue beacon then both go over Steam P2P:
port 15000 becomes a virtual port on the server's Steam id, and no port has to be opened. The queue address is
`steam.<server Steam id>:15000`. Type it in the game's console as `ServerQueue.Join steam.<id>:15000`. **Find servers**
gives it too with your own app id; on app 480 the first results are other games' servers, so yours may not be
listed. The server's log shows the id as `Now tracking socket ... for addr <id>:15000`.
See [Steam sockets](../features/steam#steam-sockets).
