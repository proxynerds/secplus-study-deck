# CompTIA Security+ SY0-701 Study Deck

A standalone, offline HTML study app for the CompTIA Security+ SY0-701 exam. One file, no installation, no internet required - open it in any browser and start studying.

---

## Getting Started

Download `secplus_SY0-701_study_deck.html` and open it in any modern browser. No setup, no account, no internet connection needed. Your progress saves automatically in your browser's local storage and persists between sessions.

If you study on multiple devices or browsers, progress does not sync between them - each browser keeps its own independent record.

---

## What's Inside

**907 multiple-choice questions** drawn from multiple question banks, deduplicated and cleaned:
- Tagged across the five official SY0-701 domains
- Each question includes a full explanation after you answer
- Wrong answers stay in your Review tab until you manually dismiss them

**403 vocabulary flashcards** covering acronyms, protocols, attack types, and key Security+ concepts:
- Tagged by domain and category
- Cards you mark as Needs Practice accumulate in your flashcard Review tab

---

## Questions Tab

### Practice

Works through all 907 questions in shuffled order. Answer a question, see whether you got it right and why, then move on. Use Prev/Next to navigate history within your current session.

The session stats row at the top shows four counters: Seen, Accuracy, Missed, and Correct - all scoped to your current session and reset when you start a new run. Switch to the global view to see all-time numbers.

Every 7 questions, a previously missed question is injected for a retry. This can be toggled off in Settings under Practice Question Options.

### Review Tab

Holds every question you've gotten wrong or manually flagged. The count on the tab badge is persistent - it only goes down when you manually dismiss an entry, not when you answer a question correctly. This is intentional: the tab is a reliable list of things worth revisiting, not a live accuracy gauge.

From Review you can:
- Practice all questions in your Review list in a focused sub-mode
- Practice only unresolved ones (ones you haven't gotten right since they were added)
- See how many times you've missed each question
- Dismiss individual entries once you're confident on them

Wrong answers from exam simulations also flow into the global Review tab.

### Stats Tab

Shows accuracy, seen count, and per-domain breakdowns. Toggle between session view (current run only) and all-time view at the top.

---

## Flash Cards Tab

### Flip Cards

Works through all 403 vocabulary cards in shuffled order. Tap a card to flip it, then grade yourself:
- **Got it** - moves on, card stays in the normal rotation
- **Needs Practice** - moves on and adds the card to your flashcard Review list

### Review Tab (Flash Cards)

Holds cards you've marked Needs Practice. Works the same way as the Questions Review tab - entries persist until you dismiss them. You can practice Review cards in a dedicated sub-mode from this tab.

---

## Settings

Settings is organized into five sections:

### Simulate Exam

Builds a timed 90-question exam using the official SY0-701 domain weighting. See the Exam Simulation section below for full details.

### Exam Answer Feedback

Controls what feedback you see during an exam simulation:
- **Full feedback** (default) - correct/incorrect highlighting and full explanation after each answer
- **Correct/incorrect only** - your answer turns green or red, no explanation until post-exam review
- **No feedback** - advances immediately with no indication; score and accuracy are hidden until the exam ends

### Practice Question Modes

Options for starting or restarting your question practice:
- **Continue with questions I haven't seen yet** - picks up where you left off in the shuffle
- **Reshuffle all 907 questions** - starts a fresh randomized run immediately
- **Practice by Domain** - loads only questions from one specific domain; useful if your stats show a weak area

All practice modes reset your session score. Your Review list and all-time statistics are never affected.

### Practice Question Options

- **Show missed questions during practice** - toggles the every-7-questions wrong answer injection (always off during exams)
- **Keyboard shortcuts** - toggles keyboard navigation on/off; full shortcut reference shown below the toggle

### Flashcard Settings

Options for resetting your flashcard session progress, parallel to the question practice modes. Your Review list and all-time flashcard statistics are unaffected.

### Reset Everything

Permanently clears all stats, Review lists, question history, flashcard progress, and shuffle order across both tabs. Cannot be undone.

---

## Exam Simulation

Click **Simulate Exam** in Settings to start. The app draws a 90-question exam matching the official SY0-701 domain distribution:

| Domain | Name | Questions |
|--------|------|-----------|
| D1 | General Security Concepts | 11 (12%) |
| D2 | Threats, Vulnerabilities, and Mitigations | 20 (22%) |
| D3 | Security Architecture | 16 (18%) |
| D4 | Security Operations | 25 (28%) |
| D5 | Security Program Management and Oversight | 18 (20%) |

The 90-minute countdown starts automatically. A blue banner shows across all screens during the exam with a live timer and a "Return to exam" link. Settings are locked while an exam is active - click "End exam" from the Settings tab to exit early.

When time runs out, the exam auto-submits and a bell sounds. If you end early, you land on the same results screen.

The results screen shows your score, pass/fail at the 83% threshold (750/900), domain breakdown, and unanswered count. From there you can:
- **Review exam answers** - step through every question you attempted. Filters for wrong only, correct only, or all. A collapsible evaluation card with your score and domain breakdown sits at the top.
- **Back to Practice** - returns you to exactly where you were before the exam, including your session score. Navigating away via the tab bar also restores your pre-exam state automatically.

Wrong answers from the exam are added to your global Review tab.

---

## Keyboard Shortcuts

Enable or disable shortcuts in Settings under Practice Question Options.

| Key | Action |
|-----|--------|
| 1 / A | Select option A |
| 2 / B | Select option B |
| 3 / C | Select option C |
| 4 / D | Select option D |
| Arrow Right | Next question |
| Arrow Left | Previous question |
| F | Flag / unflag question |
| Space / Enter | Flip flashcard |
| G | Grade flashcard Got it |
| B | Grade flashcard Needs Practice |

---

## Timer

The practice timer is accessible from the Questions tab. It supports count-up and countdown modes and can be set to any duration. During an exam simulation the timer runs the 90-minute countdown automatically.

The timer can be paused at any time during an exam. This is intentional - it is a study tool, not a proctored environment.

The timer ▶ and ⏸ buttons start, pause, and resume. The time display dims when paused.

---

## Dark Mode

The app follows your system preference automatically. If your OS is set to dark mode, the app uses dark mode.

---

## Tips

- The Review tab count is your main study signal. If it keeps growing, slow down and use "Practice unresolved only" to drill what you're actually missing rather than moving on to new questions.
- Check the Stats tab domain breakdown regularly. Practice by Domain in Settings lets you target weak areas directly.
- Near the end of your prep, run a full simulation with No feedback mode for the most realistic exam experience, then use "Review exam answers" to go through everything you got wrong.
- The session stats row resets each time you start a new run. Switch to the all-time view if you want the full picture.
