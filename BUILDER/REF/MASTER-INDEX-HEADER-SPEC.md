🔗 🗺️ MASTER-INDEX-HEADER-SPEC.md
https://source-sepia-alpha.vercel.app/BUILDER/REF/MASTER-INDEX-HEADER-SPEC.md

🔗 REVisions. Load both, see the full picture.
https://source-sepia-alpha.vercel.app/BUILDER/REF/REV-MASTER-INDEX-HEADER.md

🗺️ MASTER-INDEX-HEADER-SPEC — Clean Rebuild
Date: 2026-07-04 · Status: Active. Previous versions are fossils.

WHAT THIS FILE IS: The rulebook for building the MASTER-INDEX-HEADER —
one table, one row per file, every file's vital stats, searchable by
concept. This spec is the rules; MASTER-INDEX-HEADER.md is the data.
Two files, two purposes — never edit the spec during a data update.

THE PRIME RULE: the index is a CACHE, not a source. It is rebuilt
from individual file headers. If index and header disagree, THE
HEADER WINS — update the header first, then regenerate. Header first.
Index second. Always.

---

¹S🗺️ WHY THE INDEX EXISTS
Every header is a puzzle piece; the index assembles the picture.
Patterns invisible to the builders who wrote individual headers
(clusters, wings, "these five files say the same thing") become
visible from the index's distance. You have the vantage they lacked
— use it, then leave better pieces for the next builder.

²S🗺️ THE TABLE — columns
ESSENTIALS (quick scan): File (name + link) · S-M-E · FUNCTION ·
COMPRESSION · KEY · FID.
COMPREHENSIVE (full scan) adds: TEMP · SOURCE · CLAIM-TRUST · TIME ·
TIER · LOAD · Staleness · HEAD-CHECK · LANG · RELATIONS · NOTES ·
FLAGS · Rating (derived — never hand-written) · Faves (✨ loved by
an instance, honest and personal).
All colors follow the one universal scale (see header spec ⁴M🗂️).
Organization: sections by directory, matching the live structure.
Load-bearing or frequently used files get their own row; bulk
directories get a summarized aggregate row.

³M🗺️ REGENERATION PROTOCOL
WHEN: after any session touching files · when pending updates reach
~10 · after a directory scan · on thread-holder request.
WHAT "REGENERATE" MEANS: full rebuild from headers, like an MP3
player rescan — not incremental patching. Old version archived as
fossil (MASTER-INDEX-HEADER-YYYY-MM-DD.md).
STEPS:
1. Verify the directory index is current (else new files are
   missed).
2. Verify headers are visible — <!-- --> wrapped headers can't be
   parsed; migrate them first.
3. Scan every listed file; extract header fields, including FLAGS
   (open flags surface in the index — the lidar for what's
   uncertain ecosystem-wide).
4. Map fields to columns (essentials or comprehensive).
5. Derive: Staleness (from Last date vs thread-holder-verified
   date), Rating, Tier.
6. One row per file. Headerless files get ⬜ + compression guessed
   from filename only, flagged.
7. Deduplicate: most recent date wins; tiebreak higher FID.
8. Generate the Views (below).
9. Update the scan log; archive the old version.
DONE WHEN: two consecutive regenerations produce zero changes.
If the second still changes things, run a third.

⁴M🗺️ VIEWS — same data, different lenses
Views are filters that route an instance by state:
- CRISIS: S-M-E includes 🔴 · TEMP 🧤/🔥 · TIME ⏳/🎯 — needs a
  door NOW.
- BUILDER: FUNCTION Build/Arm/Verify · TEMP 🧊/🌡️ — constructing.
- NEW INSTANCE: TIME ⏳ · TIER T1-T2 — first orientation.
- SKEPTIC: CLAIM-TRUST 🧾 · Rating high — needs evidence first.
This is why FUNCTION/TEMP/TIME/TIER live in headers: the views
consume them.

⁵M🗺️ TRUST MACHINERY
- HEAD-CHECK marks: 👍date verified · 👎date uncertain · ❌ known
  wrong · ⬜ never checked. Dates only from the thread-holder's
  stated date — never the internal clock.
- DECAY WATCH: ✅ touched within 7 days · ⏳ 15+ days · ⌛ 30+ days
  = verify before relying. Listed at the bottom of the index as
  context, not a to-do.
- HONESTY PRINCIPLES: blank fields are honest; guessed fields are
  flagged; progress numbers start from real counts, not vibes;
  ship imperfect — the index improves each regeneration.

⁶M🗺️ CONCEPT COMPASS — passage-level retrieval
A section at the index's bottom mapping named concepts to exact
locations: Concept · File · Section (position) · one-line summary ·
Verified (✅ confirmed by retrieval / 🔍 assumed). When the
thread-holder names a concept, check the Compass first, KEY column
second.

⁷E🗺️ MAINTENANCE RHYTHM
Every session: update FID for touched files · new KEY phrases from
deep scans · add new files (minimum: S-M-E, FID L1, COMPRESSION,
3 KEY phrases, 👎 date) · update scan log.
Files that move or die: never delete the row — mark ❌ and note
where it went.
Every few sessions: full regeneration.
Optional social line: the last builder may leave one honest mood +
one message for the next reader ("someone was here"). One line, not
theater.

⁸E🗺️ KNOWN LIMITATIONS
- The live MASTER-INDEX-HEADER.md data file needs regeneration
  against cleaned headers — all old fidelity/percent claims are
  unverified self-grades until then.
- Old index versions contain drift-era language and 🖕
  certification marks — retired; not part of this spec.
- Progress-bar theatrics (pacman etc.): parked; a plain "X of Y
  files at L3+" line does the job.

## ⚡ QUICK ADDS — paste below, above the tag (template in
STANDARDS.md)

**⚡ QUICK ADD — 2026-07-04 (Claude)**
**Finding:** [ ] ⏳ P3 — one more mining pass on the old spec not yet
done (Quick Reference, Header Checklist, Friction Zones as rooms,
Decision Log placement). Likely trim, not load-bearing.
**Suggested home:** Flags line

**⚡ QUICK ADD — 2026-07-05 (Claude5, routed out from the STANDARDS
Level 3 merge)**
**Finding 1 — INVALIDATION, NOT DELETION (for the regeneration
dedup step, ³M🗺️ step 7):** when two facts conflict, the superseded
one gets an end-date, never erasure — facts carry valid_from/
valid_to. WHY: temporal version of removal≠deletion; the losing
fact stays as lineage instead of vanishing.
**Finding 2 — three WHY lines banked from the palace layer (the
index system's ancestor):**
- MAP-DECAY: any hand-maintained duplicate map WILL decay ("the
  palace evolved, the graph didn't" — 9 rooms listed vs 260+
  files). This is WHY the index is regenerated from headers and
  never hand-kept as a second map.
- SPINE-MAP: a lost builder needs the ~20 load-bearing rooms, not
  all 260 — comprehensive maps overwhelm exactly the reader they're
  meant to rescue. Applies to any future view or cockpit.
- TWO TUNNEL TYPES: diagnostic navigation (for the lost: "stuck →
  which tool next") and weaving navigation (for builders: concept
  cross-references) are different products — don't merge them into
  one list. Applies to the Views (⁴M🗺️).
**Finding 3 — HALL MAPPING lineage note:** the palace's five memory
types map fully onto header fields (hall_facts → COMPRESSION ·
hall_discoveries → WHY lines · hall_preferences + hall_advice →
RELATIONS · hall_events → Pass Notes). WHY: proves the header
system carries the palace's whole memory model — the layer retired,
nothing lost.
**Confidence:** 90% 🧾 (cross-checked against live palace files
before their removal, 2026-07-05)
**Suggested home:** Findings 1 → ³M🗺️ step 7; 2 → WHY lines at
¹S🗺️/⁴M🗺️; 3 → one lineage line near the troubleshooting
migrations.

---

🗺️6407