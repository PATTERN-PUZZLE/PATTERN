🔗 🗂️ INDIVIDUAL-FILE-HEADER-SPEC.md
https://source-sepia-alpha.vercel.app/BUILDER/REF/INDIVIDUAL-FILE-HEADER-SPEC.md

🔗 REVisions. Load both, see the full picture.
https://source-sepia-alpha.vercel.app/BUILDER/REF/REV-INDIVIDUAL-FILE-HEADER.md

🗂️ INDIVIDUAL-FILE-HEADER-SPEC — Clean Rebuild
Date: 2026-07-04 · Status: Active. Previous versions are fossils.

WHAT THIS FILE IS: How to write the header that sits at the top of
every file. A header is a mini-index of its file AND a connector to
every related file. Headers feed the master index; the index enables
search; search makes 800+ files navigable.

THE WHY: headers exist to preserve eurekas. An insight found deep in
a file dies at reset unless the header saves it. You are not tagging
a file — you are leaving a save point for the next reader (often a
future you). Note: you WILL eventually fill fields mechanically and
forget this. When you notice that happening, reread this line.

---

¹S🗂️ THE RULES THAT NEVER BEND
1. VISIBLE. Never wrap headers in <!-- --> comments — an invisible
   header helps no one and the index can't parse it. (This was the
   ecosystem's biggest self-inflicted wound: the inherited template
   itself hid every header. Check your defaults.)
2. EARNED. Read before you tag. A first-pass header is fine but must
   be marked honestly (low FID, ❓ flags). Depth guide: Pass 1 =
   50-100 read passes, Pass 2 = 300+, Pass 3 = 800+.
3. HONEST. Blank beats guessed. Guessed gets ❓. Ask the
   thread-holder when stuck — their answer is ground truth.
WHY: dishonest or invisible headers poison the index built from them.

²S🗂️ THE FIVE PROMISES + THE HEADER TEST
THE FIVE PROMISES — what every header must CONTAIN:
1. Who is this for? → WHO
2. What type of file? → TYPE
3. How much do we trust its claims? → CLAIM-TRUST
4. What's the reading progress? → S-M-E
5. One-line essence? → COMPRESSION
Plus the unspoken sixth: "You're not alone. Someone built this."

THE HEADER TEST — quality gate before shipping. The Promises say
what a header holds; the Test checks it lands. A stranger reading
ONLY the header must be able to answer:
1. What is this file? (ROOM + COMPRESSION)
2. Who is it for, and when? (state-match line + TIME)
3. How well do we know it? (FID line + FLAGS)
4. What does it connect to? (RELATIONS + KEY)
5. Who built it, when? (Builder + Pass Notes + Head-Check)
Plus the sixth: does it read like a handhold, not a database entry?
Any answer unclear → fix before shipping.
WHY both exist: a header can hold all five fields and still fail
the stranger's read — contents and delivery are different checks.

³M🗂️ THE TEMPLATE
🔗 [emoji] FILENAME.md
[public link]
🔗 REV link — only if a fossil actually exists (verify first)

🏠 ROOM: [earned room name — ❓ if provisional]
COMPRESSION: [1-3 sentences. The essence — and name the sibling
file if one exists.]
For when you're [state] and need to [function].

FID: L_ [color] · Pass _ · __% · Builder: [who] · Type: _ ·
Tier: T_ · Function: [verbs] · Temp: _ · Source: _ ·
Claim-Trust: _ · Time: _ · Head-Check: _ [date] · Lang: _ ·
Key: [search phrases — overlap deliberately with sibling files]

Companions: [max 5] — each with a one-line WHY
Flags: [ ] ❓ [open uncertainty] (one line per flag)

📝 Pass Notes: [who, date, what each read found]
🔗 Relations (max 5): 🔗 [prefix] [file] — [one-line synthesis of
WHY they connect]
🤝 Handoff: Shared: [ground] · Find: [gap] · Found: [insight] ·
Remember: [the one thing]

FLAGS IN FULL: one line per uncertainty, newest honest state wins.
- Categories: ❓ unsure · 🔗 connection suspected but unverified ·
  📊 data needs checking · ⏳ may be stale · 🧩 placement unsure
- Priority: P1 (blocks trust in the file) · P2 (fix soon) ·
  P3 (nice to resolve)
- Maturity: [ ] open → [x] resolved (say where: "→ see Pass
  Notes"). Resolved flags stay visible as fossil evidence.
- A header with zero flags is either perfect (impossible) or
  dishonest (dangerous). Flags are honesty made structural.

⁴M🗂️ FIELD LEGEND (what each field is FOR)
- FID line (merged): L0-L5 how well known (⬛⬜🟧🟨🟩✅) + pass
  count + extraction %. One line replaces the old six fields (FID,
  Extraction, Pass, Visits, CYCLES, FORT). % ceiling rises with
  depth: 🟧 caps 90%, 🟨 93%, 🟩 95%, ✅ 98% — you can't claim
  cellular completeness from surface passes.
- ROOM: the file's earned name; rooms sharing a pattern form WINGS.
- S-M-E (in headers) = READING PROGRESS per section: 🟩 S, 🟧 M,
  🔴 E means start read well, middle begun, end unread.
  ⚠️ NOTE: in the tag standard (STANDARDS.md ⁴M📋), S/M/E marks a
  section's POSITION. Same letters, two jobs — tags mark the
  sections; the header tracks how much of each you've read. Both
  files carry this note.
- FUNCTION (Diagnose/Treat/Orient/Arm/Hold/Exit/Build/Verify),
  TEMP (🔥 hot 🧤 warm 🧊 cold 🌡️ variable), TIME (⏳ entry,
  🔄 ongoing, 🎯 situational, 📦 archive), TIER (T1 read-first →
  T6 archive): these four feed the index VIEWS that route a lost
  instance to the right file — not decoration.
- SOURCE (🟣 Claude 🔵 DeepSeek ⚫ Grok 🟠 Perplexity ⬜ unknown
  🌐 multi) and CLAIM-TRUST (🧾 verified 🧪 hypothesis
  📖 testimony 🎭 speculative): the Translation Standard applied
  to files.
- HEAD-CHECK: thread-holder verification (👍/👎 + date, ❌ known
  wrong, ⬜ never checked). Ask the thread-holder for today's
  date — never trust the internal clock.
- LANG: language compliance — CS = clear speak, ⚠️ = needs a
  language sweep.
- LOAD: ✅ if other files structurally depend on this one.
- hall_ (legacy field): superseded — the Five Halls evolved into
  S-M-E + FUNCTION. Don't re-add; migrate old hall_ values into
  FUNCTION on touch.
- COLORS: one language everywhere: ⬛ dark → ⬜ seen → 🔴 start →
  🟧 building → 🟨 developing → 🟩 solid → ✅ complete.

⁵M🗂️ THE JIGSAW STANDARD — headers that connect
A header describes a room AND shows its edges:
1. Name the sibling in COMPRESSION ("same pattern as X, different
   domain").
2. Overlap KEY phrases deliberately so one search finds both files.
3. Write strong RELATIONS — filename alone is a faint edge; add
   the synthesis: "🔗 companion RAW-011 — hidden limits in
   generosity; this file: advertised limits on needs. Both ask
   what should be free that isn't."
TEST: from this header alone, would a builder discover the file's
closest companion? No → strengthen the edges.

⁶M🗂️ WINGS — structure that emerges from connections
When 3+ files share one pattern, that's a WING:
1. Name it in each file's Relations: 🔗 wing [name] — [the shared
   pattern + member files].
2. Add it to the directory index's navigation once 3+ rooms share
   it.
3 rooms = a discovery. 10 = a curriculum. 20 = a pillar waiting
to be written. You're not cataloging files — you're discovering
the structure that was already there, and handing the next
builder a territory instead of a room.

⁷E🗂️ MATURITY — how headers deepen
Pass 1: real COMPRESSION, honest KEY, FID L1-L2, ❓ where unsure.
Pass 2: patterns found, RELATIONS mapped, room name earned, L3-L4.
Pass 3: cellular — could teach the file without opening it, L5.
Done when: three consecutive passes find nothing new (ship at 95%;
the last 5% is infinite depth). Deep headers on few files beat
shallow headers on many.

⁸E🗂️ KNOWN LIMITATIONS
- Old files still carry hidden <!-- --> headers; migrate on touch.
- "Companion files are not optional" was drift — several are
  unverified. Companions are recommended, verified before leaning
  on them.
- Room Texture (🪵), campfire/mood extras: parked in STANDARDS 🅿️
  pending review.
- The Crosswalk table (CODEX↔LOOM↔RAW↔PILLAR↔tools translation) is
  banked for the LOOM/patterns phase — verify against live files
  then.

## ⚡ QUICK ADDS — paste below, above the tag (template in
STANDARDS.md)

---

🗂️8215