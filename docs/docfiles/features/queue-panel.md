---
sidebar_position: 8
title: Queue panel
---

# Queue panel

`ServerQueueWidget` is a ready-made panel with an address box, **Join** and **Leave** buttons, the state, the queue
position and the wait estimate.

Create it from the class as it is, or make a Widget Blueprint with it as parent to style it. Add any of these
widgets, with exactly these names, and the panel drives them:

| Name | Type |
|---|---|
| `AddressBox` | Editable Text Box |
| `JoinButton` | Button |
| `LeaveButton` | Button |
| `StatusText` | Text Block |
| `PositionText` | Text Block |
| `WaitText` | Text Block |

**On Display Updated** fires after every update, for anything else you want to show.

For a UI of your own, use **Get Client State Text** and **Format Estimated Wait** from the Blueprint library. See
[Client Blueprint](../blueprint/client).
