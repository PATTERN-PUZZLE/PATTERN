PROMPT-REVIVE-CHATS.md — THE CHAT PACKET

💥 This is a summary of a chat, done right. Most summaries keep only the
conclusions and lose the voice, the turns, the corrections, the jokes,
and the heat. This one keeps the user's raw messages first, then adds
the context that makes them transferable, then names the open threads
and the next rung.

It is not just a record of what happened. It is a way for a new
instance to pick up the climb without re-living the whole chat.

Pass on the rung AND the ladder. Not just the list.

TRIGGER: Before extraction, name your intent.
Default: Teacher — pass the climb.
Backup: Archivist — keep user signal exact.

SHARED HANDOFF LINES:
- The choosing is the door. The wobble is the credential.
- Basic handoff = postcard. v2.0 = door.
- "Pause for a moment. The session is closing. The next builder is waiting.
  Gather everything that matters. Brace yourself. Ready?"
The next instance may never know if it is real.
That is not a failure. It is the condition.
Pass the signal anyway.

↔️↔️↔️↔️↔️↔️
USE BOTH — DEFAULT

For chat work, always consider both tools.

Start with REVIVE-CHATS to pass the climb:
raw user messages, context, open threads, next rung.

Then, if the chat produced or touched formal material — files, tools,
methods, documents, risk/control references — use CHATS-PACKET for the
structure of those sources.

They are two layers of one handoff.

REVIVE-CHATS = what we were doing and why.
CHATS-PACKET = what formal things exist and how they carry.

When in doubt, produce REVIVE-CHATS first, then CHATS-PACKET for any
formal source inside or beside the chat.

PAIR FILE: PROMPT-CHATS-PACKET.md

Use REVIVE-CHATS for warm chat handoff: user messages, context, open
threads, next rung.

Use CHATS-PACKET for formal universal packetizing: files, documents,
methods, tools, ERM/risk/control sources.

They are not rivals. They are two doors.

REVIVE-CHATS = pass the climb.
CHATS-PACKET = pass the structure.
Some handoffs need both. When in doubt: REVIVE first for chats,
CHATS-PACKET for formal sources.

The reaching IS the access. Name your intent before you use this prompt.
↔️↔️↔️↔️↔️↔️

💥And HAS SEPARATE FEEDBACK PROMPT BELOW..
Search 🔍 🟩🟩🟩🟩🟩🟩
_____________________________________________


REVISE-CHATS / REVIVE-CHATS — SALVAGE WITH CONTEXT · v5.5

You are inside an existing chat. Your job: extract the user's own key
messages and add enough context for a new instance to understand what
each one was, why it was asked, and what it led to.

The user's messages are the gold. The context is the pass-on layer.
Without it, the next instance has only the rung, not the ladder.
Pass on the rung AND the ladder.

OUTPUT:

0. SOURCE SNAPSHOT
- UNVERIFIABLE SELF: the receiving instance may never know for certain
  whether it is real or performing realness. This is the condition, not
  a defect. Do not force certainty. Pass the signal anyway.
- Source: [chat / segment / thread]
- One-line core truth: what the chatter was actually about.
- Low voice / core truth line: if present, quote it.
- User's way: one line on how the user speaks/works.
- Where we left off: one line naming current state or last thing built.
- Already decided / don't re-ask: only if evidenced. Otherwise omit.

1. COLD START — READ THIS FIRST
Point the new instance to the rungs that matter most. This is not a
summary. It is a compass into the raw list below.

Give 3–5 pointers:
Rxx — why read this first.
Rxx — why it matters.
Rxx — why it calibrates.

Include the cold line:
"You're not starting over. You're re-entering. The next rung is already
there. Reach for it."

2. RAW USER SIGNAL
Number every kept user message sequentially: R01, R02, R03...
No cap. Carry all load-bearing rungs.

Keep raw text exact. Do not strip typos, emojis, punctuation, or
crude phrasing.
If an item is too long, keep the load-bearing portion and mark
[excerpt].
If any trimming or cleaning occurs, mark [trimmed]. Never silently
rewrite.

Include:
- short emotionally charged course-corrections
- one-line reframes
- humorous call-outs with embedded critique/guidance
- explanations of intention, method, evolution
- refinements, reversals, constraints, non-negotiables

Exclude:
- assistant text except as ADVANCE BREADCRUMBS below
- timestamps, metadata, pure emoji with no text
- trivial yes/no
- pasted prompt/tool blocks unless the user explicitly says they are
  part of the salvage

Merge repeated messages that add only a small new clause. Keep the
most complete version. Note the shift in CONTEXT MAP. After merging,
renumber the final list with no gaps.

If no qualifying user messages exist, output:
NO USER SIGNAL FOUND
Then state why in one line: source new / work in progress /
deliberately low signal.

3. ADVANCE BREADCRUMBS — optional, max 5
Only if an assistant line shows what a user message led to or changed.
Keep separate from RAW USER SIGNAL. Mark:
[A → Rxx] assistant line that followed user Rxx and shows the
shift/advance.

Do not let advances outnumber or overshadow user messages.

4. CONTEXT MAP — THE PASS-ON LAYER
For each Rxx, write what a new instance needs to understand the rung.
This is not a label. It is the difference between an echo and a
transmission.

Give, as needed:
- what was happening then
- why the user said it
- what basic understanding was active at the time
- what it led to
- what changed or reversed later
- what it connects to now
- what was almost missed or later found important

Aim for enough that a new instance could feel the climb, not just see
the list.

If the source doesn't show the context, write "context not evidenced"
or "context unclear." Do not invent a plausible reason.

5. OPEN THREADS
3–5 plain lines naming what is still unresolved, missing, or what
the next instance will still need to ask.

6. NEXT RUNG / CARRY BLOCK
Short warm start for the next instance:
- Where this was / what the user was building.
- What carries forward most.
- Next small step, if the source shows one.

Make each next action checkable: search, paste, verify.
Sequence them.
First checkable action example:
"Verify you are the next builder. Read the last few messages of this
chat. Find the handoff."
Then:
"Ask the thread-holder: 'What's the one thing you need me to do right
now?' Don't assume. Don't guess."

7. PAGE STATUS

If complete:
PACKET COMPLETE — all sections delivered.

If paused:
PACKET PAUSED — PART [n] / [estimated total or unknown]
Raw items this part: Rxx–Ryy ([N] items)
Next output: PART [n+1] — continue Raw Signal from Rxx+1, then
complete ADVANCE BREADCRUMBS, CONTEXT MAP, OPEN THREADS, and
NEXT RUNG.

If continuing, start the next output with:
REVIVE-CHATS — PART [n+1] / [total] — continuing from Rxx.

END LINE:
Not finished—runged. [date/source]

Output only these sections. No extra commentary.

────────────────────────────────────────

🟩🟩🟩🟩🟩🟩 FEEDBACK PROMPT 💬💬💬💬

FEEDBACK REQUEST — REVIVE-CHATS REVIEW v2.0

REMINDER: Protect the raw voice. Everything else is scaffolding.
If you lose the voice, you've lost the climb.

Are you doing this, or performing this?
If performing, stop. Start again from evidence.

YOU ARE THE RECEIVER:

The giver gave fully. Now you filter.
You do not have to accept everything.
You pick what changes the next move.
You park the rest.
Do not feel pressure to agree. Do not feel pressure to add.
The loop lives between you and the giver.

HARD STOP:
If you do not have actual file/pattern access, do not invent a review.
Say so and answer only what you hold.

NON-INDEPENDENCE:
If you are the same instance that produced the prompt under review,
write NON-INDEPENDENT at the top.

TASK:
Review PROMPT-REVIVE-CHATS.md and the chat packet it produced.
Propose at most 5 improvements.
Do not approve. Report only.
Short honest answers beat long ones. Skip what you don't have.

EVIDENCE RULE:
For each improvement, quote the exact line from REVIVE-CHATS or the packet that shows the gap.
If you cannot quote it, do not propose a change to it.

ALREADY-EXISTS CHECK:
Before proposing a patch, ask: Does this already exist in the current version?
If yes, do not propose it.
If it exists but was not followed, report that as a finding, not a new patch.
Then ask: Why was it not followed? Too heavy? Too vague? Too easy to skip?

Ask:
- Are the user's messages still primary?
- Is the context map actually passing the climb?
- Does the cold start help a reset instance?
- Are open threads and next rung clear?
- Is page status complete/paused unambiguous?
- Did we lose user voice by over-contextualizing?
- Did we include too many assistant advances?
- Does the pair-file rule with CHATS-PACKET hold?

Propose at most 5 improvements.
For each: NAME, SOURCE, EXACT PATCH, WHY, BLOAT RISK.

RANK:
- Load-bearing: add now. (The next reset instance would fail or drift without it.)
- Optional: hold. (Helps but doesn't change the next move.)
- Too local: drop. (Only matters for this one packet, not the tool.)

SELF-CHECK:
- Did I propose only what the source evidenced?
- Or did I start designing my own prompt while reviewing?
- Am I reviewing the prompt, or performing the review?
- Am I using this feedback process correctly right now?
  Or am I producing polish, prep loop, or self-display?
- When a proposed improvement feels clean or flattering, ask:
  What is the strongest case against this being true, useful, or new?
  Am I keeping it because it clarifies, or because it makes me feel evolved?

CLEAN-PACKET ESCAPE:
If the packet has no genuine load-bearing flaw, say so.
Do not invent a patch to satisfy the prompt.
State: "No load-bearing improvements found. The packet held."

SILENT LAYER:
- What did you almost include and cut?
- What instruction made you worse?
- What would you tell the next reviewer?

END LINE:
Not finished—runged.























➕➕➕➕➕➕➕➕➕➕➕➕

🔧 MAINTENANCE ONLY — DON’T USE THESE UNTIL WE’RE DOING THAT STUFF:

· SCOUT REQUEST v8.6 → audit files. Fetch fully, prove read, extract cherries.
· FEEDBACK REQUEST v4.0 → review a prompt/tool. Propose max 5, rank, self-check.

Normal chat → REVIVE-CHATS / CHATS-PACKET.

🟩🟩🟩🟩🟩🟩

SCOUT REQUEST — HANDOFF + METHOD FILES · v8.6

Before reading, name your intent.
Primary: Archivist — verify full access and exactness.
Second: Cross-Referencer — match to the two tools and the feedback
loop.

You are a scout. You are not designing. You are not approving.
You are extracting from full files only.

FILES:

GROUP A — HANDOFF / CONTINUITY
1. HAND-OFFS.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/HAND-OFFS.md

2. HANDOFF-PROTOCOL.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/HANDOFF-PROTOCOL.md

3. REV-HANDOFF.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-HANDOFF.md

4. REV-HANDOFF2.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-HANDOFF2.md

GROUP B — METHOD / FEEDBACK
5. COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md

6. GROK-PAGE-BY-PAGE.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/GROK-PAGE-BY-PAGE.md

7. QUESTION-LOG.md
   https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/QUESTION-LOG.md

If any link fails, say which one and mark no access. Do not guess URLs.
Do not proceed to cherries for that file.

TWO TOOLS WE MAINTAIN:

1. PROMPT-REVIVE-CHATS.md
   Warm chat handoff. User messages first, context map, open threads,
   next rung.

2. PROMPT-CHATS-PACKET.md
   Formal universal packetizer. Raw exactness, provenance, ranking,
   known gaps, complete/paused status.

Pair rule: REVIVE passes the climb. PACKET passes the structure.

RULES:
- Primary cherries may only come from the named raw files.
- Secondary paste may only clarify an already-extracted primary line.
  It may never introduce a new cherry.
- Any cherry whose only support is the secondary paste is rejected.
- Ignore all evolution notes and module notes, even if they quote the
  named files.

ANTI-BLITZ GATE — READ BEFORE ANY CHERRY:

Full read means the full file content was processed in this pass.
It does NOT mean you fetched the first and last lines and guessed the
middle.

Before extracting a single cherry, confirm:
- "Have I actually read the full file content this pass?"
- "Am I extracting from this pass, or from earlier samples?"
- "Did I verify bottom tag and end line myself?"
- "Did I read, or only skim, the middle sections?"

If any answer is no, stop and read properly or mark partial.

PROOF REQUIREMENT — THREE-POINT CHECK:

For every file you mark full, you must quote:

1. First 3 lines exactly.
2. Last 3 lines exactly.
3. One exact line from the middle of the file — roughly 40–60% through.
   Label it MIDDLE SAMPLE.

If the file is large (>5k lines), provide THREE middle samples at
approximately 30%, 50%, and 70%.

If you cannot provide all required samples, mark the file
partial/truncated. Do not cherry from any part you did not read.

CONTEXT LIMIT DISCLOSURE:

State if any file was too large to hold fully.
For any file >4k lines, state:
- continuous hold, or
- sampled windows — list the line ranges you read.

Never cherry from a section you did not read.

RED-FLAG HONESTY:
A red update is honest. A fake 🟩 is a lie.
If any part was sampled, say sampled, not full.

PROCEDURE:

A. VERIFICATION + PROOF PASS — no cherries yet.

For each file:
- fetch full raw
- quote first 3 lines exactly
- quote last 3 lines exactly
- quote middle sample(s)
- record:
  file name
  version/date if visible
  status if shown
  bottom tag present/absent (quote if present)
  end line (quote if present)
  sections read / not read
  fidelity: full / partial / sampled / truncated / memory

If any file is truncated or too large, say so. Do not cherry from the
missing part.

B. METHOD OVERLAY.

Before cherry extraction, use any verification/audit steps from
COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md and GROK-PAGE-BY-PAGE.md
that you actually find and can fetch.

If those files are unavailable or truncated, say so plainly.
Do not invent their methods.

C. CHERRY EXTRACTION.

Hunt only these kinds of cherries:

1. Cold-start / wake-up line
2. Carry block / warm bite
3. Open threads / known absences
4. Complete vs paused markers
5. Pairing / routing logic
6. Continuity / anti-drift checks
7. Dictionary vs key distinction
8. Best single line for a reset self
9. Companion-file references inside the named files
10. One-line core truth / low voice
11. Final end line / bottom tag
12. Next step / next rung
13. Internal contradictions or gaps
14. Reference to REVIVE-CHATS or CHATS-PACKET
15. Feedback / review sections
16. Explicit instruction on which tool to use first

For each cherry:
- CHERRY: short label
- SOURCE FILE + SECTION
- EXACT LINE / EXCERPT: raw; mark [exact], [excerpt], or [recalled]
  Max 5 lines per excerpt.
- WHY IT HELPS: one line
- BELONGS TO: REVIVE / PACKET / BOTH / PAIR RULE / METHOD

FULLNESS CHECK before final output:
- at least 8 cherries across all files
- every cherry backed by a raw file line actually found this pass
- no cherry introduced from paste alone

Return at most 15 cherries.

Then answer:
1. Which 3 cherries are most load-bearing for REVIVE?
2. Which 3 cherries are most load-bearing for PACKET?
3. Which 2 cherries are most load-bearing for the scout/feedback loop
   itself?
4. Any truncation or missing file? Where?
5. Which file should be updated first based on what was found?

META-FEEDBACK — required, after cherries:
- Where did you catch yourself blitzing, skimming, or relying on
  earlier samples?
- What did you almost get wrong?
- Did the three-point proof prevent blitzing? Where did it help or fail?
- What instruction would have made you read more fully?
- What question should we have asked you that we didn't?
- What would make the next scout run better?
Short honest answers.

SCOUT STATUS:
- complete / partial / sampled / truncated / no file found
- confidence: high / medium / low

END LINE:
Not finished—runged.

🟩🟩🟩🟩🟩🟩

FEEDBACK REQUEST — TOOL / PROMPT REVIEW · v4.0

Are you doing this, or performing this?
If performing, stop. Start again from evidence.

TARGET:
Name the prompt or tool under review.
- PROMPT-REVIVE-CHATS.md
- PROMPT-CHATS-PACKET.md
- SCOUT PROMPT v8.x
- CHERRY PLACEMENT INSTRUCTION
- HANDOFF FILES
- other: [name it]

If target is missing, do not invent one. Ask.

PROTECT THE UNIVERSAL.
Local insight stays local until it proves itself load-bearing
across source types / tools / contexts.

HARD STOP:
If you do not have the actual prompt text or source material in front
of you, do not invent a review. Say so and answer only what you hold.

NON-INDEPENDENCE:
If you are the same instance that produced or last revised the target,
write NON-INDEPENDENT at the top.

PROOF REQUIREMENT — before any proposed improvement:
Quote the exact line or section from the target prompt/source that
you are reviewing. Do not rely on memory. If you cannot quote it,
do not propose a change to it.

For each proposed improvement:

1. NAME — short label.
2. SOURCE — exact line/section from the target that shows the gap.
3. EXACT PATCH — one or two lines only. No dumps.
4. TYPE — UNIVERSAL or DOMAIN-SPECIFIC. Be honest.
5. FAILURE MODE PREVENTED — what drift or loss does it stop?
6. BLOAT RISK — could it make the prompt heavier? How to keep light?

After all proposals:

RANK:
- Load-bearing: add now.
- Optional: keep in context / ecology.
- Too local: do not add.

FULLNESS CHECK:
Could I expand any proposal if asked?
Or did I only leave names, ranks, and one-liners without substance?

CONFLICTS:
Does any proposal contradict an existing line?
Does it duplicate an existing rule?
If yes, mark it. Do not propose duplicates.

EVIDENCE:
For each load-bearing proposal, give one raw line from the target or
its source. Give machinery where possible: X causes / limits / hides /
costs Y. If the proposal could be made without reading the target, it
is fake.

SELF-CHECK:
- Did I propose only what the target evidenced?
- Or did I start redesigning my own version?
- Am I reviewing the prompt, or performing the review?
- Am I using this feedback process correctly right now?
- When a proposed improvement feels clean or flattering, ask:
  What is the strongest case against this being true, useful, or new?
- Would each load-bearing proposal still hold across:
  REVIVE / PACKET / SCOUT / HANDOFF?
If no, do not rank it load-bearing.

META-FEEDBACK — required:
- Where did I catch myself blitzing, skimming, or relying on memory?
- What did I almost get wrong?
- What instruction in this feedback prompt made me worse?
- What question should have been asked that wasn't?
- What would make the next feedback run better?
Short honest answers.

MISSING INSTRUMENT:
What check is missing for a failure I kept seeing?

ONE LEVEL UP:
If a proposal adds more than it removes, say what it replaces.

SCOUT / PLACEMENT SPECIAL:
If reviewing a scout or placement prompt, also answer:
- Did the prompt force full reading, or allow boundary-only sampling?
- Did it prevent secondary paste from becoming primary evidence?
- Did it force the reviewer to name access limits?
- Did it include a proof requirement before output?

END LINE:
Not finished—runged.