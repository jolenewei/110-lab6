# Lemon Clicker Bug Report

**Name:** Jolene Wei
**Tickets solved:** 1, 2, 3, 4, 5 <!-- e.g. 1, 2, 3, 5, 6 (at least five) -->

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


---

## Ticket 2: Best Scores always shows 0"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** `server/index.ts`

**What was wrong:** The function was setting the score to the top score only when (entries[i].score < best), but that makes the best score always 0.

### Fix

<!-- What did you change, and why does that change fix the problem? -->
I changed the condition to if (entries[i].score > best), then it will set the best equal to the score. That way, if a score is higher than what is currently set as best, then best will be updated to the new top score.

### How did you find it?

I searched for "Best:" and then read through the function for how the best score variable is calculated and noticed that the inequality was logically wrong.

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**


#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
The start screen showed best score as 0 when the last score was 19.
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Searched up "Best:" and saw that there was a for loop that was calculating the best score variable.
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
The logic was wrong and the inequality needed to be flipped in order to calculate the top score accurately.

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
I saved the change and refreshed the game. The stats appeared with Best: 19, which was accurate.


---

## Ticket 3: The lemon only lands in four spots"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** `GameScreenView.ts`

**What was wrong:** Math.round() was used around Math.random() in the function randomizeLemonPosition() in GameScreenView.ts and that made it so that the random value that was the multiplier would either round to 0 or 1.


### Fix

<!-- What did you change, and why does that change fix the problem? -->
I deleted the Math.round() function surrounding Math.random() in both constants randomX and randomY in the function randomizeLemonPosition(). 

### How did you find it?

I guessed that there would be a click lemon event in GameScreenController and I found handleLemonClick() and in that function, I saw this.view.randomizeLemonPosition() being called. I then found that function in GameScreenView.ts, and looked at the code for how the random values were calculated, where Math.round() seemed unnecessary. 

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
The lemon was only moving to places on the edges of the screen, probably meaning x and y was only being set to whole numbers. 

2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Went to GameScreenController first, looking for what happened whenever the lemon was clicked.
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Found handleLemonClick(), and saw randomizeLemonPosition() being called there --> went to that function
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
Saw that Math.round() in the code would lead to only either 0 or 1 being multiplied by x and y values, leading to the lemon to only show up on the edges of the screen.

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
I tested it out and clicked on the lemon a few times, and the lemon was no longer showing up only on the edge of the screen. 


---

## Ticket 4: Stuck on Game Over Screen"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** `GameScreenController.ts`

**What was wrong:**
The endGame() function was not properly calling stopTimer(). It had stopTimer instead of stopTimer(), so the function wasn't actually being called. Without stopTimer() being called, the timer keeps going down and it would keep ending the game on a loop. 

### Fix

<!-- What did you change, and why does that change fix the problem? -->
I changed "stopTimer" to "stopTimer()" so that the endGame() function properly calls stopTimer(). 

### How did you find it?

I guessed that there would be something wrong with the game timer so I looked in GameScreenController.ts and traced the stopTimer function.

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I guessed that there would be something wrong with the game timer so I looked in GameScreenController.ts and traced the stopTimer function, and saw that it wasn't called properly in endGame(). I guessed this because the game was ended "every second" -> which made me think of the timer. 

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
The start screen showed best score as 0 when the last score was 19.
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Searched up "Best:" and saw that there was a for loop that was calculating the best score variable.
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
The logic was wrong and the inequality needed to be flipped in order to calculate the top score accurately.

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
I saved the change and tried playing the game again. When the game ended, it didn't refresh the page on a loop and it let me start a new game.


---

## Ticket 5: Leaderboard dates show "undefined""

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** `ResultsScreenView.ts`, `updateLeaderboard()`, and the `LeaderboardEntry` type in `ResultsScreenModel.ts`.

**What was wrong:** The frontend expected `timestamp`, but the server sent the date as `time`. Reading the missing property displayed "undefined."


### Fix

<!-- What did you change, and why does that change fix the problem? -->
I changed the frontend to use `time` so it matched the server. 

### How did you find it?

I followed the leaderboard data from the controller to the view, then compared the property used by the view with the one created by the server.

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I thought the date property might be missing or named differently. I checked the server code and found that it used `time`, while the frontend used `timestamp`.

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
The lemon was only moving to places on the edges of the screen, probably meaning x and y was only being set to whole numbers. 

2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Went to GameScreenController first, looking for what happened whenever the lemon was clicked.
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
Found handleLemonClick(), and saw randomizeLemonPosition() being called there --> went to that function
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
Saw that Math.round() in the code would lead to only either 0 or 1 being multiplied by x and y values, leading to the lemon to only show up on the edges of the screen.

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
I played a new game and checked the leaderboard. The date appeared instead of "undefined."