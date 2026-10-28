---
sidebar_position: 5
title: Testing
---

# Testing

## Locally

Run the server and clients from the editor binaries with `-server` and `-game`, as in the [Demo](demo). Use
`-QueueTotalSlots=1` so a second client has to queue, and `ServerQueue.Status` on the server to print the beacon
state, the slots and the queue.

## Automation tests

The plugin ships with automation tests under `ServerQueue.Core.*`, `ServerQueue.Providers.*` and
`ServerQueue.Validation.*`, and `ServerQueue.Steam.*` in Server Queue Steam. Run them from
**Tools → Session Frontend → Automation**.

The queue logic lives in `FServerQueueCore`, which has no networking and takes the current time on every call, so
your own tests can drive it directly. See [C++](../cpp/cpp-overview).
