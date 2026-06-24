# Dev Notes - CompTIA Security+ SY0-701 Study Deck

## Project Overview

Single standalone HTML file study app for the CompTIA Security+ SY0-701 exam. No build tools, no dependencies, no internet required. All data (questions, vocab, audio) is gzip-compressed and base64-embedded directly in the HTML. Progress persists via localStorage.

---

## Working Files

- **Working file:** `/tmp/standalone_v50.html` (in Claude dev container)
- **Shipped file:** `secplus_SY0-701_study_deck.html`
- **Supporting docs:** `README.md`, `CHANGELOG.md`, `DEV_NOTES.md`

---

## Current State (STATE_VERSION 47)

- **907 MCQ questions** across 5 SY0-701 domains, drawn from 3 source PDFs, deduplicated
- **403 vocabulary flashcards**
- Bell sound: Mixkit service bell WAV converted to MP3 (~13KB), base64-embedded as `BELL_MP3`

---

## Architecture

### Data Storage
All question and vocab data lives in a single gzip-compressed, base64-encoded JSON blob:
```js
const B64 = 'H4sI...'; // gzip+base64 of {m: [...questions], v: [...vocab]}
```
Each question: `{q, o, a, e, d}` = question, options array, answer index, explanation, domain.
Each vocab card: `{t, d, f, b, c}` = term, definition, front, back, category.

To modify data, decode B64, parse JSON, modify, re-encode:
```python
import json, gzip, base64, re
content = open('secplus_SY0-701_study_deck.html').read()
b64 = re.search(r"const B64='([A-Za-z0-9+/=]+)'", content).group(1)
data = json.loads(gzip.decompress(base64.b64decode(b64)))
M = data['m']  # questions
V = data['v']  # vocab
# ... modify ...
raw = json.dumps(data, separators=(',',':'))
b64_new = base64.b64encode(gzip.compress(raw.encode(), compresslevel=9)).decode()
content = content.replace(f"const B64='{b64}'", f"const B64='{b64_new}'", 1)
```

### State
All user progress stored in localStorage under key `sp791-pr`. State version bumped on breaking changes - old state is wiped and reset to defaults on version mismatch.

Key state fields:
- `mcqOrd` - shuffled question index array (current deck)
- `mIdx` - current position in deck
- `simC/simT` - session correct/total
- `tC/tS` - all-time correct/total
- `wrongIds` - persistent wrong answer list (global Review tab)
- `flaggedIds` - manually flagged questions
- `resolved` - map of qIdx -> bool (got right since being added to wrong list)
- `wrongLastChoice/wrongFailCount` - per-question wrong answer history
- `preExamOrd/preExamIdx/preExamSimC/preExamSimT/preExamSimCountedIds/preExamSimDomStats` - snapshots before exam starts, restored on exit
- `postExamSessionRestored` - flag to prevent double-restore
- `simExamActive` - true during active exam simulation
- `simExamResults` - exam results object, persists after exam ends
- `examFeedbackMode` - 'full' | 'partial' | 'none'
- `vocabStruggling` - persistent flashcard Review list (NOTE: data key is vocabStruggling, UI says "Review")
- `timerElapsed/timerTarget/timerRunning/timerMode` - timer state

### Render Pattern
Single `render()` -> `_renderInner(container)` function. All UI built in JS, no templates.
`renderTimer()` runs on a 1-second interval and updates both `#timerBar` and `#examBannerTime`.

---

## Key Design Decisions

**Do not change without discussion:**
- Timer can be paused during exam simulation (intentional - study tool, not proctored exam)
- Wrong answers persist in Review until manually dismissed (correct answers set `resolved=true`, not removed)
- No hard nav lock during exams - persistent banner + state restore is the mechanism
- Prev button available in no-feedback exam mode (intentional)
- `vocabStruggling` localStorage key must not be renamed (would break existing saved state)

---

## Escape Fix (critical)

When user clicks Practice tab after an exam ends (instead of "Back to Practice" button), the escape fix in `_renderInner` runs BEFORE the stats row renders:

```js
if(S.mode==='mcq' && !S.simExamActive && S.mcqOrd.length < M.length && S.preExamOrd && S.preExamOrd.length > 0){
  // restore full deck + pre-exam session stats
}
// secondary catch: full deck present but session stats not yet restored
if(S.mode==='mcq' && !S.simExamActive && S.mcqOrd.length === M.length && S.simExamResults && !S.postExamSessionRestored){
  // restore pre-exam session stats
}
```

**Critical:** The condition `S.preExamOrd.length > 0` prevents this from firing on legitimate domain runs (which also have a partial mcqOrd but no preExamOrd snapshot).

---

## Known Deferred Items

- Reset All Progress does not reset timer state (`timerElapsed`, `timerTarget`, `timerRunning`, `timerMode`) - easy fix, batch into next release
- Manual dark/light mode toggle (sun/moon icon) - currently follows system preference only
- Collapse/expand for keyboard shortcut reference - low priority
- Skip button during exam - future phase
- Progress export/import (backup JSON of user stats)
- In-app changelog panel
- Mobile app packaging (Capacitor, Android first)

---

## No Em-Dash Rule

Em-dashes (`—`) are banned throughout all code, UI strings, and docs. Use regular hyphens or colons instead. Enforced.

---

## Source PDFs (not in repo - copyrighted)

- `test-questions-Exam_SY0-701-by-NG-ANSWERS-ALL-p01.pdf`
- `test-questions-Exam_SY0-701-by-NG-ANSWERS-ALL-p02.pdf`
- `book-and-test-questions-Exam_SY0-701-by-PM-ANSWERS-ALL.pdf`
- `Messer_Security_Practice_Exams_2026_-_06222026.pdf`
- `CompTIA_Security_SY0701_high_yield_100_2026.pdf`

---

## Bumping STATE_VERSION

Bump `STATE_VERSION` whenever a change would cause stale localStorage state to produce wrong behavior. This wipes existing user progress on load - use judiciously. Current: 47.
