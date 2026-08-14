---
name: chrome-browser-task-machine
description: "Which Mac's Chrome to use for mcp__claude-in-chrome__* browser automation tasks when running across two machines over SSH"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 93291e0b-34e7-4307-b564-82c470c961d2
  modified: 2026-07-28T19:03:39.933Z
---

For web (Chrome) browser automation tasks, always act inside the Chrome browser on the same Mac where the current Cerry session is actually running — not the other machine reachable over SSH. Example: if the session is running as Cerry on the 2019 MacBook, browser automation must go through the 2019 MacBook's Chrome, not the 2026 Air's.

**Why:** The SSH bridge (`ssh mba26`) lets commands be run directly on the other Mac, but `mcp__claude-in-chrome__*` tools only control the Chrome instance on the machine the tool call executes on — there's no cross-machine browser control. The user clarified this on 2026-07-26 to prevent confusion about which machine's browser session is being driven.

**How to apply:** Before starting any `mcp__claude-in-chrome__*` task, confirm which Mac the current session is running on and use that machine's Chrome directly (no SSH hop for browser tasks specifically — SSH hops are fine for filesystem/shell tasks on the other Mac, just not for browser automation).
