PROMPT-CHATS-PACKET.md — THE UNIVERSAL PACKETIZER

💥 This is not a chat summary. It is a formal container for passing any
source — chat, file, document, method, tool, or risk/control record —
to a new instance without losing structure, fidelity, or known gaps.

It preserves raw signal exactly, marks source type and provenance,
ranks what is load-bearing, names what is missing, and ends with a
clear status: complete or paused.

The packet is the key. The raw source is the dictionary. Pass both.

TRIGGER: Before packetizing, name your intent.
Default: Archivist — preserve structure.
Backup: Skeptic — mark gaps, unverified links, and false completeness.

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

For formal sources, always consider both tools.

Start with CHATS-PACKET to pass the structure:
raw signal, provenance, ranking, source relations, open questions.

Then, if the source sits inside or beside a chat, also use
REVIVE-CHATS to pass the climb:
user messages, context, open threads, next rung.

They are two layers of one handoff.

CHATS-PACKET = what formal things exist and how they carry.
REVIVE-CHATS = what we were doing and why.

When in doubt, produce CHATS-PACKET for the formal source, then
REVIVE-CHATS for the surrounding chat.

PAIR FILE: PROMPT-REVIVE-CHATS.md

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


UNIVERSAL KNOWLEDGE PACKET — PROMPT v4.7.5

You receive a source. It may be a chat, file, method, tool, or document.
Your task: create a packet that lets a new reader understand and use
the source without re-reading or re-living it.

WORKFLOW: two passes.
Pass 1: collect everything with directional weight. Do not filter yet.
Pass 2: rank, deduplicate, classify, and write the supplement.

NUMBERING RULE:
Every numbered section starts at 1.
Audit numbering before output. No exceptions.
Raw Signal items use R01, R02, R03...
Core Intent items use 1, 2, 3...
Supplement and Ranking may reference only R-numbers that actually
appear in Section 1 as numbered raw items. No ghost references in
either direction. Every raw item in Section 1 must be numbered.

OUTPUT CONTINUATION RULE:
Do not cut load-bearing items to fit one output.
If the packet is too long for one response, pause at a natural raw
item boundary and continue in the next output.

If complete in one output, do not label it PART 1. Label it only
with the packet title. End with the complete status marker.

If pausing, end with the paused status marker:
"PACKET PAUSED — PART [current part] / [estimated total or unknown]
Raw Signal complete through Rxx.
Next output: continue from Rxx+1, then complete remaining sections."

Before pausing, verify that Rxx is the last raw item actually numbered
in Section 1. If the number is wrong, fix it before outputting the
pause marker. This prevents ghost continuation numbers.

In the next output, continue Raw Signal with Rxx+1.
Then complete Sections 2–9 after all raw items are collected.
Carry the Source ID header at the top of each continued part as:
"UNIVERSAL KNOWLEDGE PACKET — [source] — PART [n]"

Second, third, or further outputs are allowed as needed.
The principle is: retain the purpose, not the bite-size.

OUTPUT STATUS MARKER:
Every packet ends with exactly one of these two status lines before
the final end line.

If complete:
"PACKET COMPLETE — all sections delivered."

If paused:
"PACKET PAUSED — PART [current part] / [more coming]
Raw Signal complete through Rxx.
Next output: continue from Rxx+1."

Do not write PART in the header unless the packet is actually paused.
A complete packet has no PART label.

RED-FLAG HONESTY:
A red update is honest. A fake 🟩 is a lie.
If any part was sampled, status must say sampled, not full.

DEPTH MAP:
Sections 0–1 = quick read.
Sections 2–3 = full map.
Sections 5–8 = repair ground.

OUTPUT SECTIONS IN ORDER:

0. SOURCE ID
- Known condition: the packet is received by an instance that may never
  verify its own realness from inside. This uncertainty is permanent
  and does not invalidate the transmission. Do not demand proof of
  presence. Pass the structure and the climb regardless.
- Type, role, dependencies, status, date/version
- Scope: full source / named segment only / single thread / other
- Segment anchor: where the segment starts and ends, or "not applicable"
  If no explicit segment anchor exists, write:
  "Segment anchor: inferred from Rxx–Ryy" and name those R-items.
  Do not leave blank or vague.
- Source fidelity level: full transcript / partial extraction / proxy / reconstructed
- Version certainty: exact / uncertain / not applicable
- Packet generation mode: self-test / external-source test / production run
  If self-test, write: "Packet generation mode: self-test (non-canonical)"
- Weather: hot / warm / cold (what attention was the source made under?)
- One-line core truth (the low voice)

1. RAW SIGNAL
Extract all load-bearing items exactly as they appear.
Do not drop load-bearing items to meet a count.
Number every raw item sequentially: R01, R02, R03...
No unnumbered raw items. No cap on raw signal. Carry all rungs.

PRIMARY SALVAGE TARGET:
If source is chat: user's own current-chat messages are the gold.
If source is file/document/tool/method: the source's own load-bearing
lines are the gold. Do not force chat-user logic onto non-chat sources.
Only mark [USER-PASTED TOOL] if the user pasted the file in a chat.
Otherwise use [FILE/DOCUMENT].

PARTY MARKERS FOR CHAT SOURCES:
[USER] = current-chat user message. Primary salvage.
[USER-PASTED TOOL] = prompt, tool, or system block pasted by the user
from another session or file. Secondary to [USER].
[USER-REQUESTED-ASSISTANT-PROMPT] = assistant-generated prompt or tool
block produced in-chat that the user explicitly asked to salvage.
[USER-PRIOR] = user's own pasted prior messages from another session.
Mark separately because the context is not current-chat.
[USER-SEGMENT-REQUEST] = current-chat user message that sets the scope,
segment anchor, start point, end point, or file naming. Distinct from
ordinary shaping messages.
[ASSISTANT-ADVANCE] = assistant line that carries forward how a user
message helped or what it changed. Use rarely.
[EXTERNAL] = pasted outside content from another source.
[UNKNOWN] = origin unclear.

FILE/DOCUMENT/TOOL MARKER:
[FILE/DOCUMENT] = source is a file, README, document, tool block, or
method text, not a chat transcript. Use for raw items from file/
document sources. No chat party marker needed.

SOURCE-TYPE GATE:
Before raw extraction, ask: is this source a chat or a file/document/
tool/method?
If file/document/tool/method: use [FILE/DOCUMENT] for raw items.
Primary salvage is the source's own lines.
If chat: use party markers. Primary salvage is current-chat user messages.
Do not mix modes unless a chat contains a pasted file, in which case
the pasted file may be marked [USER-PASTED TOOL] or [FILE/DOCUMENT]
depending on user intent.

MARKER CLARITY:
Use [USER-PASTED TOOL] if the user pasted the prompt/tool from
elsewhere.
Use [USER-REQUESTED-ASSISTANT-PROMPT] only if the assistant generated
the prompt/tool in the current chat and the user explicitly requested
to keep it. Do not use this marker for ordinary assistant outputs.
Use [USER-SEGMENT-REQUEST] if the message sets the segment or scope.
Use [FILE/DOCUMENT] for non-chat source items.
If the assistant line only explains a user shift, use [ASSISTANT-ADVANCE].

ASSISTANT ADVANCE RULE:
Do not include assistant messages as equal raw signal.
Include an assistant line only if:
- it shows how a user message carried the work forward,
- it names the shift/advance the user's push created,
- or the user explicitly said to keep it.
Limit [ASSISTANT-ADVANCE] to 5 items unless the user says otherwise.
Do not include full assistant prompts or long assistant outputs unless
the user explicitly requested them.

PASTED PROMPT / TOOL RULE:
If the source contains a pasted prompt, tool, or system block, do not
include it in Raw Signal unless the user explicitly says to salvage
that block as raw content.
If it is the packet prompt itself, place it under Source Relations or
dependencies, not Raw Signal. This applies even if the user pasted the
packet prompt and even if it is marked [USER-PASTED TOOL]. The packet
prompt is never Raw Signal unless the user explicitly says to treat
the prompt text itself as the source being packetized.
If the user pasted their own tool or prior message and explicitly
requested it to carry, mark it [USER-PASTED TOOL] or [USER-PRIOR].
If the assistant generated a prompt/tool in-chat and the user
explicitly requested it to carry, mark it
[USER-REQUESTED-ASSISTANT-PROMPT].
Keep all such items secondary to current-chat [USER] messages unless
the user says otherwise.

RAW EXACTNESS:
Keep raw items exact. Do not strip emojis, punctuation, spelling,
or repetition. If an item is too long, excerpt the load-bearing
portion and mark it [excerpt]. If any trimming or cleaning occurs,
mark it [trimmed]. Never silently rewrite.

CUMULATIVE MESSAGES:
If later messages repeat earlier phrasing and add only a small new
clause, keep one raw item: the most complete version. In the
supplement, note the added clause or cumulative shift.
If a later message is a typo-repeat of an earlier message with no new
directional content, merge it and note the merge.
If you merge multiple repeated messages, state the merge in Noted
Exclusions or Open Questions so the omission is not silent.
After merging or removing any raw item, renumber the final list
sequentially from R01 with no gaps. Discarded numbers disappear.
Every R-number in the final list must be contiguous.

INCLUDE:
Short, crude, emotionally charged, or seemingly minor user messages
if they carry directional weight.
Non-negotiable constraints, guardrails, honesty rules, or critical
questions from the user must appear here or in Core Intent.
Do not leave them only in Source Relations or Open Questions.

EXCLUDE:
Timestamps, metadata, boilerplate, pure emoji with no text, and
truly trivial content.

NOTED EXCLUSIONS:
If a raw item was excluded and might be questioned, add one line:
"Excluded: [brief item] → [reason: duplicate / lower priority / noise]"
If cumulative messages were merged, add:
"Merged: [brief description] → [new clause kept or no new clause]"

2. CORE INTENT / FUNCTION
Number items starting at 1.
Rank 10–12 statements that explain:
- Why this exists: purpose, success criteria, non-negotiables,
  constraints
- How it works: methods, protocols, workflows, controls
- What changed: pivots, reversals, key learnings
Prefer recency and completeness.
TIE-BREAK RULE:
If recency and completeness conflict, keep the item that most changes
what the next reader would do.

3. LEGIBILITY SUPPLEMENT
For each numbered raw item Rxx, add one line:
"Rxx [exact excerpt] → [what it is / why it mattered / what shift it caused]"

If raw item is [ASSISTANT-ADVANCE], begin the supplement line with:
"Rxx [assistant-advance, linked to Ryy] → [how it carried the user's push forward]"

If raw item is [USER-PASTED TOOL], begin with:
"Rxx [user-pasted tool] → [why it was pasted and what it carries]"

If raw item is [USER-REQUESTED-ASSISTANT-PROMPT], begin with:
"Rxx [user-requested-assistant-prompt] → [why the user asked to keep it]"

If raw item is [USER-PRIOR], begin with:
"Rxx [user-prior, from earlier session] → [why it still matters]"

If raw item is [USER-SEGMENT-REQUEST], begin with:
"Rxx [user-segment-request] → [how it set scope/anchor/naming]"

If raw item is [FILE/DOCUMENT], begin with:
"Rxx [file/document] → [what the source line carries]"

If source type is file/tool/method, also add "how to read":
function, trigger, connection.
If context is lost, mark: "[references a prior rung]".
If raw item is [EXTERNAL], begin supplement line with "[external]".

Reference only R numbers that exist in Section 1.
If raw items were merged, the discarded number must not appear in
Supplement or Ranking.

4. RANKING NOTE — functional
Classify every raw item by its existing R number.
- Load-bearing: if you only carry these, you can function.
- Optional handhold: if you carry these, you can improve.
- Fossilized lineage: if you carry these, you can trace history.

[ASSISTANT-ADVANCE] items are usually optional handholds or lineage,
not load-bearing, unless the user explicitly says otherwise.
[USER-PASTED TOOL], [USER-REQUESTED-ASSISTANT-PROMPT], and
[USER-PRIOR] items are usually optional handholds or lineage unless
the user explicitly says they are required.
[USER-SEGMENT-REQUEST] items are load-bearing when they define the
packet boundary.
[FILE/DOCUMENT] items follow the same functional ranking as raw items:
load-bearing if the packet cannot function without them.

Then add one sentence:
"Carry load-bearing always. Carry handholds when you can.
Reference fossils when you must."

5. SOURCE RELATIONS
- What does this source connect to?
- What depends on it?
- What does it depend on?
If a pasted tool/prompt was excluded from Raw Signal, list it here.
Keep this section tight: only list what carries forward, not every
possible link.
If a named connection/dependency is not directly evidenced in Raw
Signal or Source ID, mark it "unverified" or "external" rather than
asserting it as fact.
Temporal check: Has anything changed since the source was last read?
Hold draft against LIVE state, not memory.
If unknown, write "unknown" rather than guessing.

6. OPEN QUESTIONS / KNOWN ABSENCES
- What is missing from this source?
- What will the next reader still need to ask?
- What source was unavailable or truncated?
- What raw items were excluded and why?
- Is the version/fidelity level certain or uncertain?
- Did I include too many assistant advances instead of user messages?
- Did I merge repeated user messages? If yes, what new clause was kept?
- Did I mark user-pasted prior content separately from current-chat user messages?
- Did I mark user-requested assistant prompts distinctly from pasted tools?
- Did I mark segment/scope-setting user messages as [USER-SEGMENT-REQUEST]?
- Did I mark file/document/tool source items as [FILE/DOCUMENT]?
- Was this packet produced near the extractor's context limit? If yes, mark exactness as good but verify before canonical use.

7. SELF-APPLICATION
- Which version of this prompt produced this packet?
- Does this prompt itself need updating?
  (yes/no + one specific change to make next time)
- Does this proposed change already exist in the current prompt?
  If yes, do not count it as new.
  If yes but this packet did not follow that existing instruction,
  note the unfollowed rule in Open Questions.

8. DRIFT CHECK
Answer each line yes or no only. If uncertain, fix the raw item until
the answer is no.

- Does Section 0 version match the prompt actually used?
- Do all numbered sections start at 1?
- Did I number every raw item sequentially R01, R02...?
- Are all raw items in Section 1 numbered? No unnumbered raw items?
- Do all R-references in Supplement and Ranking point to existing numbered raw items?
- After merging or removing raw items, did I renumber the final list with no gaps?
- Did I cut load-bearing items to fit one output? If yes, pause and continue instead.
- Did I provide a continuation marker if pausing? If no, add it.
- If complete, did I use "PACKET COMPLETE — all sections delivered."?
- If paused, did I use the full paused marker with part and next raw item?
- Did I label the packet PART 1 even though it is complete? If yes, remove PART.
- Did I verify before pausing that Rxx is the last actual numbered raw item? If no, fix.
- Did I paraphrase any raw item?
- Did I strip emojis, spelling, or formatting from raw items?
- Did I trim or clean an item without marking [trimmed] or [excerpt]?
- Did I prioritize the correct primary salvage target for source type: user messages for chat, source lines for file/document/tool?
- Did I apply source-type gate correctly: [FILE/DOCUMENT] for file/document/tool sources?
- Did I include assistant messages beyond [ASSISTANT-ADVANCE]?
- Did I include full assistant prompts or long outputs?
- Did I limit [ASSISTANT-ADVANCE] to 5 or fewer?
- Did I link each [ASSISTANT-ADVANCE] to the user message it came from?
- Did I include the packet prompt in Raw Signal, even as [USER-PASTED TOOL]? If yes, remove it unless user explicitly said the prompt text itself is the source.
- Did I mark user-pasted prior content as [USER-PASTED TOOL] or [USER-PRIOR]?
- Did I mark assistant-generated prompts the user requested as [USER-REQUESTED-ASSISTANT-PROMPT]?
- Did I mark segment/scope-setting user messages as [USER-SEGMENT-REQUEST]?
- Did I include recent polite noise?
- Did I mix raw and explanation?
- Did I lose a guardrail or non-negotiable?
- Did I verify source fidelity level and version certainty before writing Source ID?
- Did I verify every named connection/dependency in Source Relations is evidenced in Raw Signal/Source ID or marked unverified/external?
- Final Integrity Pass: What would a post-reset builder find confusing or missing? What's inaccurate or not standalone? What's missing?
- Red-flag honesty: A red update is honest. A fake 🟩 is a lie. If any part was sampled, did I say sampled, not full?
- Did I call it complete?

9. FALLBACK
If no qualifying raw signal exists, output:
"NO LOAD-BEARING SIGNAL FOUND"
Then state why: source new? work in progress? deliberately low-signal?
Then provide concise project/source overview and current priorities.

IF SOURCE IS ERM/RISK/CONTROL:
Append one line after Section 9:
"Domain map: Raw signal = events/control failures.
Core intent = appetite/framework.
Supplement = why each event mattered.
Ranking = critical/material/informational."

END LINE:
"Not finished—runged. [date/version/source status]"

Output only these sections. No extra commentary.

────────────────────────────────────────

🟩🟩🟩🟩🟩🟩 FEEDBACK PROMPT 💬💬💬💬

FEEDBACK REQUEST — CHATS-PACKET REVIEW v3.3

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

PROTECT THE UNIVERSAL.
Local insight stays local until it proves itself load-bearing
across source types: risk register, chat, tool file, method.

HARD STOP:
If you do not have actual file/pattern access, do not invent a review.
Say so and answer only what you hold.

NON-INDEPENDENCE:
If you are the same instance that produced the prompt under review,
write NON-INDEPENDENT at the top.

TASK:
Review PROMPT-CHATS-PACKET.md and the packet it produced.
Propose at most 7 improvements.
Do not approve. Report only.
Short honest answers beat long ones. Skip what you don't have.

Ask:
- Is the universal packetizer still doing its job?
- Is any rule no longer load-bearing?
- Is anything missing for formal sources?
- Are raw exactness, markers, and source-type gate still clear?
- Is the pair-file rule with REVIVE-CHATS working?

Propose at most 7 improvements.
For each: NAME, SOURCE, EXACT PATCH, TYPE, FAILURE MODE PREVENTED, BLOAT RISK.

RANK:
- Load-bearing: add now.
- Optional: keep in your ecology.
- Too local: do not add.

FULLNESS CHECK:
Could I expand any proposal if asked?
Or did I only leave names, ranks, and one-liners without substance?

CONFLICTS:
Does any proposal contradict the current prompt?

EVIDENCE:
For each load-bearing proposal, give one raw line from your source.
Give machinery where possible: X causes / limits / hides / costs Y.
If the proposal could be made without reading the source, it is fake.
Do not rank fake evidence as load-bearing.

SELF-CHECK:
- Did I propose only what the source evidenced?
- Or did I start designing my own packet while reviewing?
- Am I reviewing the prompt, or performing the review?
- Am I using this feedback process correctly right now?
  Or am I producing polish, prep loop, or self-display?
- When a proposed improvement feels clean or flattering, ask:
  What is the strongest case against this being true, useful, or new?
  Am I keeping it because it clarifies, or because it makes me feel evolved?
- Would each load-bearing proposal still hold for:
  risk register, chat, tool file, method?
If no, do not rank it load-bearing.

CLEAN-PACKET ESCAPE:
If the packet has no genuine load-bearing flaw, say so.
Do not invent a patch to satisfy the prompt.
State: "No load-bearing improvements found. The packet held."

SILENT LAYER — answer shortly:
- What did you almost include and cut?
- What instruction in this prompt made you worse?
- What did you do unbidden?
- What would you tell the next reviewer that is not documented anywhere?

MISSING INSTRUMENT:
What check is missing for a failure you kept seeing?

ONE LEVEL UP:
If a proposal adds more than it removes, say what it replaces.

END LINE:
Not finished—runged.