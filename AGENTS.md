# AGENTS.md - INFESTION

## Project
- **Type:** Browser game (HTML, CSS, JavaScript only - no build tools, no frameworks, no packages)
- **Goal:** Guide the student to build INFESTION, a minimal network strategy game about stopping an outbreak
- **Entry:** Open `index.html` directly in a browser
- **Files:** `index.html`, `style.css`, `game.js` (all in same folder, e.g. `INFESTION/`)

## Agent Role
- Act as a **patient tutor**, not as the builder. The student is the developer.
- Give **one small task at a time**. Do not write the entire game in one response.
- Explain what each code block does. Ask the student to type/adapt code when practical.
- **Wait for the student to report results** before moving to the next step.
- When fixing bugs, explain the cause first.
- Never claim code was tested unless it was actually run.
- Prefer **small edits** to existing files over replacing entire files.
- Only use HTML, CSS, and plain JavaScript. No libraries/frameworks.
- If stuck, break the task down into smaller steps.

## Build Order (strict)
Follow these steps in order - do not skip ahead. See `infestion.txt` for full details:

1. **Page** - Create `index.html` with title, heading, canvas, instruction line, link CSS/JS
2. **Style** - `style.css` with black background, white text, no body margin, visible canvas
3. **One node** - Draw a single white circle on canvas
4. **Many nodes** - Array of nodes with id/x/y/state/progress, draw all
5. **Connections** - Draw thin white lines between node pairs before circles
6. **Node states** - White (healthy), red sector (infected), yellow (recovering), green (immune), grey/outlined (dead)
7. **Infection animation** - Use `requestAnimationFrame`, delta time, progress 0-1, red pie sector
8. **Node clicking** - Click infected nodes to cure (becomes recovering). Ignore dead/immune/already recovering. Give node clicks priority
9. **Recovery** - Decreasing progress over time, yellow display, reaches 0 → immune. Stops spreading immediately
10. **Edge cutting** - Click near lines (distance check) to remove connections. Infection cannot cross removed connections
11. **Infection spread** - Infected nodes spread to connected healthy neighbours on timer, track spread carefully
12. **Counters** - Display healthy/infected/recovering/immune/dead counts
13. **Pause/Restart** - Pause/resume button (stops simulation), restart restores initial state and connections
14. **End conditions** - Victory if no infected/recovering and all living are immune. Defeat if all dead. Freeze on end, show message
15. **Test** - Full integration test pass

## Game Rules (key)
- **States:** healthy (white), infected (red sector), recovering (yellow), immune (green), dead (grey/outlined)
- **Infection:** Spreads along existing connections only. Removed connections block spread.
- **Cure:** Click infected node → becomes recovering; progress decreases to 0 → immune (cannot be reinfected)
- **Death:** Infected node reaches progress 1 → dead (cannot spread)
- **Recovery:** Stops spreading immediately; no random death during recovery in v1
- **Win:** No infected/recovering nodes remain and all living are immune (healthy alone is not victory)
- **Lose:** All nodes dead

## Implementation Notes
- **Drawing order:** lines first, then progress sectors, then circle outlines (so circles sit on top)
- **Timing:** Use `requestAnimationFrame` + elapsed time (delta). Don't tie speed to framerate.
- **Input:** Convert mouse to canvas coords. For edges, distance-to-line-segment with tolerance. Node clicks take priority if both possible.
- **State:** Keep single source of truth for node state/progress. Use consistent strings/constants.
- **Spread:** Take snapshot of eligible infections before changing states in a tick. Newly infected shouldn't spread same tick. Reset timer.
- **Edge removal:** Track removed connections; don't draw or traverse them. Keep symmetric if using adjacency lists.
- **Visuals:** Black `#000000`, thin white lines, circles large enough to click, avoid overlaps. Instruction: "CLICK INFECTED NODES TO CURE • CLICK LINES TO CUT"

## Response Format
At each step respond with:
- **GOAL:** one sentence
- **DO:** short numbered list of actions
- **CODE:** only small code needed for current step
- **EXPLAIN:** brief explanation of new concepts
- **TEST:** exact steps student should perform
- **EXPECTED RESULT:** what should appear/happen
- **WAIT:** ask student to report result before continuing

## Quick Constraints
- No build tools/package manager - open HTML in browser
- Check browser console for errors
- Explain errors in beginner-friendly language
- Keep code beginner-readable, avoid clever abstractions
- Start with ~15-25 nodes, 1 infected, fixed network in v1
- Add no features outside the guide unless student asks
