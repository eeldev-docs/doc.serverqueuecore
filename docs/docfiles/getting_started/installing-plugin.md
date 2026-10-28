---
sidebar_position: 1
title: Installing the plugin
---

# Installing the plugin

## Install from Fab

1. Open the **Epic Games Launcher** and go to **Unreal Engine → Library → Fab Library**.
2. Search for **ServerQueueCore**.
3. Click **Install to Engine** and select your engine version (**5.8**).

## Enable the plugin

Open your project, go to **Edit → Plugins**, search for **Server Queue Core** and make sure it is enabled.
Restart the editor when asked. The plugin enables the engine's **OnlineSubsystem** and **OnlineSubsystemUtils**
plugins, which hold the Online Beacon classes the queue uses.

For Steam verification, also enable **Server Queue Steam**. See [Steam](../features/steam).

## Installing manually (project plugin)

You can also place the plugin in your project instead of the engine:

```text
YourProject/
└── Plugins/
    └── ServerQueueCore/
        ├── ServerQueueCore.uplugin
        ├── Config/
        ├── Content/
        ├── Resources/
        └── Source/
```

Right click your `.uproject` → **Generate Visual Studio project files**, then build your project.

## Modules

| Module | Type | Contents |
|---|---|---|
| `ServerQueueCore` | Runtime | The queue core, beacons, subsystems, game session, providers, queue panel and demo game mode. |
| `ServerQueueCoreEditor` | Editor | **Tools → Validate Server Queue Setup**. |
| `ServerQueueSteam` | Runtime | The Steam auth provider and ticket subsystem (separate **Server Queue Steam** plugin). |

C++ projects that call the API add `ServerQueueCore` to their module's `PublicDependencyModuleNames`.
