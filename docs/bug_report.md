# Lemon Clicker Bug Report

**Name:**
**Tickets solved:** <!-- e.g. 1, 2, 3, 5, 6 (at least five) -->

Copy the template below once for each ticket you solved. 

---

## Ticket 1: Stats always say "Stats Unavailable"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** `server/index.ts`

**What was wrong:** The frontend requested `/api/stat`, but the server route was `/api/stats`, so the request failed since it could not find the correct route.

### Fix

<!-- What did you change, and why does that change fix the problem? -->

Changed the server route in index.ts to `/api/stat`

### How did you find it?

I searched for "Stats unvailable" and fouund that it appears when stats is null. I followed the code back to the fetch request in MenuScreenController.ts, then compared its URL with its route in server/index.ts and noticed that they didn't match.

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I thought the stats might not be loading from the server. I compared the fetch URL with the server route and found that they did not match.

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
The start screen showed "Stats unvailable".
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
`updateStats()` showed this message when stats was null.
3. **Traced back to:**
`refreshStats()` called `loadStatsFromServer()`. If loading failed, stats stayed null.
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
The fetch URL in `loadStatsFromServer()` was `/api/stat` instead of `/api/stats`.

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
I saved the change and refreshed the game. The stats appeared with Average: 19.0 and Scores saved: 1.