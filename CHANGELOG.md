# CompTIA Security+ SY0-701 Study Deck - Project Changelog & Scope Tracker

Single-file HTML study app. No external dependencies, no build step, fully offline.
Current shipped file: `secplus_SY0-701_study_deck.html`

---

## Current State (as of STATE_VERSION 32)

- **907 MCQ questions**, domain-tagged (D1–D5), sourced from 3 question banks (NG, Professor Messer, CompTIA High-Yield 2026), deduplicated
- **403 vocabulary/flashcards**, domain + category tagged
- Two top-level parent tabs: **Questions** (Practice / Stats / Review) and **Flash Cards** (Flip Cards / Stats / Struggling)
- Shared **Settings** tab (Questions reset options, domain-filtered practice, Flash Cards reset options, keyboard shortcuts toggle + reference, destructive full reset)
- **Simulate Exam** mode: 90 questions built from official SY0-701 domain weighting (12/22/18/28/20%), 90-minute auto-starting countdown timer, auto-submits on timeout, dedicated results screen (pass/fail, domain breakdown, unanswered count, copy-to-clipboard), resumes prior practice session (position **and** score) on "Back to Practice"
- **Review system**: unifies "answered wrong" and "manually flagged" into one list, persistent fail counters, resolved/dismiss flow, bulk practice sub-mode with independent history navigation
- **Struggling (Flash Cards)**: same persistent pattern as Review, mirrored for vocab
- Global (all-time) vs session stats split for both Questions and Flash Cards, with domain breakdowns
- Keyboard shortcuts (answer with 1–4/A–D, arrow-key nav, flag with F, space/enter to flip cards, G/B to grade), toggleable, documented in-app
- Timer: count-up/countdown toggle, manual set, validated input, auto-behavior tied to exam mode
- `STATE_VERSION` mechanism: bump on any schema/behavior change to force clean localStorage reset on load - this has been the standard release mechanism throughout

---

## Resolved Bug Log (chronological, condensed)

- Review-injection infinite loop (root cause: `revMode` cleared in wrong place; fixed by centralizing in `nxt()`)
- "Current" button removed per spec (Prev/Next only)
- Mismatched explanation on a backup/restore question (wrong card's explanation attached)
- PDF extraction artifacts: page-number bleed, `\x02` soft-hyphen/line-break characters (multiple passes, including 2 missed instances found via user testing post-merge), doubled-term explanations ("DMARC DMARC..." → "DMARC. DMARC...")
- 10 multi-select ("Select TWO/THREE") and malformed 5–6-option questions removed; 3 more fixed by trimming bled-in E/F options
- Truncated options completed (4 instances across multiple sessions, including 2 confirmed-truncated-in-source-PDF cases)
- One image-dependent unanswerable question removed (referenced a visual never captured in source text)
- Stats double-counting (review-injected re-encounters were inflating Seen/Accuracy) - fixed via `countedIds` first-time-only gating, mirrored for session (`simCountedIds`) and Flash Cards (`vocTotalCounted`/`vocSimCountedIds`)
- Prev/Next button edge-case bugs (showing with nowhere to go) in 3 separate nav systems (Practice, Wrong-practice, Vocab-practice)
- "Reset" popup using `confirm()` as a binary choice while running reset code unconditionally (Cancel didn't cancel) - replaced with explicit Settings-page options
- Render-lock conflict causing blank screens when practice-queue completion tried to re-render from inside an active render call - fixed with `setTimeout` deferral (2 instances: wrong-practice, vocab-practice)
- Flagging mechanic not integrating with Review tab - flagging now adds to the unified review list with correct resolved/badge state depending on prior answer history
- Sim-exam "Back to Practice" restoring deck position but not session score state - fixed by snapshotting and restoring `simC/simT/simCountedIds/simDomStats` alongside `mcqOrd/mIdx`
- One genuine exact-duplicate question found and removed from the original 784-question pool via systematic fuzzy-match dedup pass
- Keyboard ArrowRight not working while viewing history (only called `nxt()`, never `nextQ()`)
- Flag button incorrectly hidden while viewing history
- Reset All Progress (and the lighter practice resets) leaving a stale `timerInterval` running, which kept ticking and persisting elapsed time against the just-reset state - fixed by clearing the interval in `resetAll`/`startNewRun`/`startDomainRun`/`startNewVocabRun`; Reset Everything now restores full timer defaults, the lighter resets stop/zero elapsed while preserving the user's custom mode/target

---

## Feature Build Log (condensed, by area)

**Core practice loop:** shuffle, dark/light mode, every-7-questions review injection (now user-toggleable), domain tagging, domain-filtered practice decks

**Stats:** global vs session split (Questions and Flash Cards independently), domain breakdowns, scope-aware top stats row

**Review/Struggling:** full question+options+explanation display, persistent fail counters, bulk practice sub-mode w/ independent nav, resolve/dismiss flow, manual flagging unified into the same list

**Settings:** Questions reset (continue-unseen / reshuffle-all), domain-specific practice launch, Flash Cards reset (parallel structure), Simulate Exam launch, review-injection toggle, keyboard-shortcut toggle + reference table, destructive full reset (clearly separated, red-styled)

**Timer:** count-up/countdown modes, manual set with input validation (positive int, ≤3 digits), auto-start/auto-submit tied to exam mode, never auto-resumes stale state on page load

**Vocab/Flash Cards:** expanded from 99 → 418 → 403 cards (added official CompTIA acronym list + concept terms, then trimmed low-relevance generic IT terms), domain + category tagging, Prev/Next history nav, session-based "this cycle" tracking, persistent Struggling list

**Data quality:** multiple sanitation passes across 3 source PDFs, exact + fuzzy duplicate detection, vocab definition enrichment (217 cards had thin "just the expansion" definitions rewritten with real exam context), em-dash removal project-wide (data + UI strings), font-color consistency pass (secondary vs tertiary text hierarchy)

---

## Open / Deferred Items

- User is actively stress-testing the current build (STATE_VERSION 32); few new bugs expected at this point - project has plateaued into a stable, polished tool. Most remaining work is "buttoning things up" rather than major fixes.
- More sample questions incoming (user has additional source material) - confirmed next content addition, no fixed timeline
- Sound effects (timer-expiration bell/chime) - confirmed next feature to build; sourcing in progress (see Sound Effects Sourcing below)
- Multi-cert genericization (Network+, A+, etc.) - explicitly deprioritized; would require extracting domain config (names, percentages, exam length) into a swappable config layer rather than hardcoded values; only worth doing if there's real intent to build additional cert apps
- Import/export tooling for question banks - tied to the above; currently all data merges are done via one-off Python scripts in the dev environment, not a repeatable pipeline
- Mobile app packaging (Capacitor-wrapped, Android first) - see Environment Migration Plan below; meaningfully larger scope than anything done so far (signing, store accounts, review process, payment handling if sold)
- In-app "What's New" / changelog panel - proposed, not built
- Progress export/import (user's own stats/wrong-list/struggling-list as a backup JSON, independent of question-bank content) - proposed, not built
- In-app bug report helper (pre-filled state snapshot) - proposed, not built
- Skip button during exam (skip question, return to it later) - future phase, noted
- Prev button restriction in no-feedback exam mode - out of scope; user can peek at history if they want
- Settings visible-but-grayed during exam (vs current full lockout) - future phase enhancement, out of scope now
- "Simulate again" shortcut on Practice or Review page - easy, future phase
- Manual dark/light mode toggle (sun/moon icon) - currently the app follows system preference only; an in-app override would let users switch without changing OS settings. Easy to add.

---

## Intentional Design Decisions (do not change without discussion)

- **Timer can be paused during exam simulation.** The user explicitly wants this. This is a study tool, not a proctored environment - the ability to pause is a deliberate affordance. If a future request comes in to lock the timer during exam mode, push back and reference this note first.

---

## Environment Migration Plan

**Status: planning phase, not yet started on the actual file-split.**

### Decision log
- Target: dedicated Ubuntu LXD container, terminal-only, no GUI, accessed via SSH from Windows daily-driver machine
- VS Code Remote-SSH recommended for visual editing/diffing without putting a desktop environment on the container
- LXD confirmed as the standardized virtualization approach going forward (over KVM VMs)
- User also needs a **local** (non-container) dev environment on the Windows daily machine, separate from this project, for existing PowerShell scripts and potential PowerShell-to-Python conversions. This is a parallel, ongoing need - not blocking the LXD container build.
- User will relay setup notes and progress between Claude Code instances (other LXD/KVM hosts, the new dev container once built) and this chat via `.md` files and prompts, rather than doing all setup work directly in this conversation.

### Confirmed file-split plan (not yet executed)
```
secplus-deck/
  src/
    questions.json       <- 907 MCQs, human-readable, not base64
    vocab.json            <- 403 flashcards
    app.js                <- all application logic
    styles.css
  build.py                <- compresses json + inlines everything into the single-file output
  dist/
    secplus_SY0-701_study_deck.html   <- generated, shareable artifact
  CHANGELOG.md
```
**Confirmed: the single-file HTML remains fully reproducible from source.** `build.py` regenerates it on demand; sharing the app with friends stays exactly as simple as it is today (one file, no build step needed on the recipient's end). The only workflow change is that day-to-day editing happens in small source files instead of the 366KB combined file, which is the main token-cost fix.

### Sequencing
1. User builds/provisions the LXD container per the steps already discussed (Ubuntu 24.04, Node.js, git, Claude Code CLI, OpenSSH)
2. User bootstraps a fresh Claude Code session in that container using notes/prompts prepared from this conversation and other Claude Code sessions
3. Once the container environment is confirmed working, the actual file-split (single HTML -> src/ structure + build.py) happens there, ideally executed by Claude Code directly in that environment rather than in this chat, since that's the more token-efficient venue for a mechanical refactor of this size
4. This chat's role going forward: planning, advising, coaching on the migration; NOT performing the split itself

---

## Sound Effects Sourcing

**Status: in progress.**

Goal: a timer-expiration sound (bell/chime) for when the Simulate Exam countdown hits zero.

Plan: source a few candidate sounds (royalty-free / appropriately licensed), present options for the user to listen to and pick one, then integrate the approved file into the project.

Next step: user will need to add the approved sound file to project files (project knowledge) once selected, since audio files cannot be generated directly - they need to be sourced from a royalty-free library and uploaded.

