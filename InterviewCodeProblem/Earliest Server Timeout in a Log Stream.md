# Earliest Server Timeout in a log stream
---

## Problem Description
You are given a **stream** of server logs.

Each log entry is a tuple:

(serverId, timestamp, eventType)
- serverId: integer ID of the server
- timestamp: integer time (seconds, ms, doesn’t matter as long as it’s comparable)
- eventType: either "start" or "end"

When you see a "start" event for a server, it means the server started handling a request at that time.
A request must finish within a fixed timeout T.
If no corresponding "end" event is seen for that server by time startTime + T, the server is considered timed out.

The log entries arrive in order of increasing timestamp (as a stream).
Task:
Detect the earliest timeout moment and return it as soon as it happens, while you are reading the logs.
You may not wait until you have read all logs to decide.
If no server ever times out, return / print something like null or -1.

---
## Core Observation


