---
sidebar_position: 1
title: C++ overview
---

# C++ overview

Add `ServerQueueCore` to your module's dependencies:

```csharp
PublicDependencyModuleNames.AddRange(new string[] { "ServerQueueCore" });
```

## The queue core

`FServerQueueCore` (`Core/ServerQueueCore.h`) holds all queue logic, with no networking and no engine clock. Every
call takes the current time, so it can be driven and tested directly.

- `UServerQueueSubsystem::GetCore()` returns the server's instance.
- `OnQueueEvent` reports slot grants, expiries and queue changes.
- Parties go through `RequestJoinParty` and `JoinParty`.
- `UServerQueueSubsystem::RequestJoinAsync` takes an `FServerQueueJoinRequest` with the party member keys or the
  leader key.

## Your own game session

If you can't use `ServerQueueGameSession`, call the queue from your own game session class:

```cpp
#include "Server/ServerQueueSubsystem.h"

FString AMyGameSession::ApproveLogin(const FString& Options)
{
	const FString ErrorMessage = Super::ApproveLogin(Options);
	UServerQueueSubsystem* Queue = UGameInstance::GetSubsystem<UServerQueueSubsystem>(GetGameInstance());
	if (ErrorMessage.IsEmpty() && Queue && Queue->IsActive())
	{
		Queue->CaptureLoginOptions(Options);
	}
	return ErrorMessage;
}

bool AMyGameSession::AtCapacity(bool bSpectator)
{
	// The queue tracks slots, including reservations the engine does not know about.
	UServerQueueSubsystem* Queue = UGameInstance::GetSubsystem<UServerQueueSubsystem>(GetGameInstance());
	if (Queue && Queue->IsActive())
	{
		return false;
	}
	return Super::AtCapacity(bSpectator);
}
```

## A ticket per connection

`UServerQueueClientSubsystem::OnMakeAuthTicket()` makes the ticket for each connection to a queue, reconnects
included, for tickets that work only once. It wins over **Set Auth Ticket**; return an empty string to fall back to
it.

```cpp
UServerQueueClientSubsystem* Queue = GetGameInstance()->GetSubsystem<UServerQueueClientSubsystem>();
Queue->OnMakeAuthTicket().BindUObject(this, &UMyTicketSubsystem::MakeTicket);
```

Server Queue Steam uses this for Steam session tickets. See [Steam](../features/steam).
