# INNERGCLAW SYSTEM REFRESH

## Why this refresh happened
This refresh came out of a real OpenClaw cleanup experience.

The system was still usable, but it had started to feel noisy, split-brained, and harder to trust than it should have been. There were old service paths still hanging around, one optional service had fallen into a restart loop, logs were filling with repeat errors, and memory embeddings were retrying against a quota wall.

Nothing here is meant as drama. It is a practical record of what a refresh looks like when the goal is simple:

- make the system calmer
- make the setup easier to understand
- remove stale or risky paths
- preserve rollback safety
- keep working features intact

This repo is written for users who want the short version of the experience, what was refreshed, what improved, and what should be watched next.

---

## What was refreshed

### 1) The broken optional node service was removed
An old optional node service had drifted out of alignment and was stuck in a repeated restart/error loop.

**What changed**
- the broken service was backed up first
- it was stopped and uninstalled
- its restart loop was verified as gone

**Why it matters**
- less background noise
- cleaner process state
- fewer confusing errors during health checks

---

### 2) A stale insecure remote route was removed
The system still had an old plaintext remote websocket route in config. That route was not part of the desired steady-state setup anymore.

**What changed**
- the stale remote gateway route was removed
- the old node config tied to that route was disabled
- the system was kept local-only on loopback

**Why it matters**
- fewer moving parts
- clearer architecture
- safer default behavior

---

### 3) Backups and log preservation were done before repair
This refresh was handled like production maintenance, not guesswork.

**What changed**
- configs were backed up
- service files were backed up
- logs were preserved and archived
- fresh log files were recreated cleanly

**Why it matters**
- rollback stays possible
- historical errors are still available for review
- new log activity is easier to read

---

### 4) Memory embedding noise was paused safely
The gateway was repeatedly trying embedding requests and running into quota failures, which created avoidable retry noise.

**What changed**
- memory search was disabled only
- the rest of the system was left alone
- the gateway was reloaded cleanly afterward

**Why it matters**
- retry bursts stopped
- the gateway stayed healthy
- channel/chat behavior remained available

---

## What the system looks like now
The refreshed setup is intentionally simpler.

### Current architecture
- local-only gateway
- loopback/local websocket only
- optional node service removed
- active runtime uses the user-local OpenClaw install
- channel connectivity preserved
- memory-search embeddings disabled for now

### What “healthy” means now
A healthy system now looks like this:
- the gateway is running locally and answering normally
- the gateway stays on loopback instead of using a stale remote route
- the optional node service does not reappear on its own
- logs stay readable instead of filling with repeated retry storms
- channels continue working normally

---

## What improved after the refresh
The biggest difference is not flashy. It is operational clarity.

### Before
- mixed service history
- stale remote config
- optional node-service failure loop
- heavy log noise
- memory embedding retry bursts

### After
- simpler local-only layout
- broken optional service removed
- stale insecure route removed
- logs archived and reset cleanly
- memory quota noise stopped
- healthier base for future maintenance

---

## What users should watch over the next few weeks
This is the practical watchlist after a cleanup like this.

### First 24–48 hours
Watch for:
- any gateway restart instability
- channel disconnects
- return of repeated memory quota bursts
- signs that the removed node service has reappeared

### First 1–2 weeks
Watch for:
- unusual log growth
- repeated warnings that look new, not known
- channel behavior becoming inconsistent
- dashboard or local UI behavior becoming flaky

### Weeks 2–4
Watch for:
- whether local-only architecture still fits the real workflow
- whether memory search should stay off or be reintroduced later with a better provider/setup
- whether remaining low-priority warnings can be cleaned up safely
- whether the older inactive install path can be retired cleanly

---

## Remaining warnings
The system is cleaner, not “finished forever.” A few items remain.

### Local Control UI insecure-auth compatibility flag
There is still a local compatibility flag enabled for the Control UI.

**Current read**
- acceptable in a loopback-only local setup
- not ideal as a final hardened state
- should be reviewed later, not rushed now

### Tool-profile startup warning
A low-priority warning still appears for an unavailable tool entry.

**Current read**
- mostly noise
- not a stability issue
- safe to defer

### Older secondary install path still exists on disk
An older install remains present but is no longer actively running the broken service.

**Current read**
- not urgent
- worth cleaning up only in a deliberate future pass

---

## What was intentionally not changed
To keep risk low, this refresh did **not** try to redesign everything.

The following areas were intentionally left alone:
- channel configuration
- TTS configuration
- approval policy
- project application code
- broader tool-policy design

That restraint is part of the refresh strategy. Fix the instability first. Expand scope later only if needed.

---

## Rollback mindset
The refresh was performed with backups first so rollback stays possible.

If a future review decides a change should be reversed, the path is straightforward because the key config and service state were preserved before edits.

---

## Recommended next maintenance after a stable observation window
If the refreshed system stays stable for 24–48 hours and continues behaving well over the following weeks, the next smart maintenance steps are:

1. **Security hardening pass**
   - review whether the local Control UI compatibility flag can be turned off safely

2. **Version hygiene pass**
   - unify remaining runtime references around one current OpenClaw version
   - carefully retire the inactive older install path

3. **Memory strategy pass**
   - decide whether memory search should stay disabled
   - or return later with a provider/setup that will not create quota retry noise

4. **Log hygiene pass**
   - confirm logs remain calm and readable
   - archive historical noise as needed

---

## README purpose
This README is meant to help users understand the refresh in plain language:
- what was wrong
- what was refreshed
- what is better now
- what still needs watching
- what should wait for a later maintenance pass

---

## Summary
INNERGCLAW SYSTEM REFRESH is the story of a cleanup done the right way:
- back up first
- remove drift
- simplify architecture
- stop noisy failure loops
- keep working features intact
- leave a cleaner base than the one you started with

That is the refresh.
