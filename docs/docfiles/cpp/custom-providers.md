---
sidebar_position: 2
title: Custom providers
---

# Custom providers

Subclass `UServerQueuePermissionProvider` or `UServerQueueAuthProvider` (`Providers/ServerQueueProviders.h`). Add the
class to the settings (**Permission Providers** or **Auth Provider**), or pass its path on the server command line
with `-QueuePermissionProvider=` or `-QueueAuthProvider=`.

Both get an `FServerQueuePlayerInfo`:

| Field | Description |
|---|---|
| `PlayerKey` | Queue key, `<IdType>:<Id>` |
| `PlayerId` | The player id as text, for example a SteamID64 |
| `IdType` | Online platform of the id, for example `STEAM`, `EOS` or `NULL` |
| `AuthTicket` | The ticket the client sent. Empty unless an auth provider is set |

Call the completion delegate **exactly once**, now or later. `Reload()` is called when the queue starts and on
`ServerQueue.Reload`.

## Permission provider

```cpp
UCLASS(meta = (DisplayName = "My Backend"))
class UMyPermissionProvider : public UServerQueuePermissionProvider
{
	GENERATED_BODY()

public:
	virtual void ResolvePriority(const FServerQueuePlayerInfo& Player, FServerQueueOnPriorityResolved OnComplete) override
	{
		// Look the player up, now or asynchronously. Return Regular when unknown.
		OnComplete.ExecuteIfBound(IsSupporter(Player.PlayerId) ? EServerQueuePriority::Supporter : EServerQueuePriority::Regular);
	}
};
```

## Auth provider

```cpp
UCLASS(meta = (DisplayName = "My Backend"))
class UMyAuthProvider : public UServerQueueAuthProvider
{
	GENERATED_BODY()

public:
	virtual void VerifyPlayer(const FServerQueuePlayerInfo& Player, FServerQueueOnPlayerVerified OnComplete) override
	{
		// The reason is logged on the server when the player is refused.
		const bool bValid = CheckTicket(Player.PlayerId, Player.AuthTicket);
		OnComplete.ExecuteIfBound(bValid, bValid ? FString() : TEXT("ticket rejected"));
	}
};
```
