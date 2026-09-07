🐙 🏗 🔗 Basic-Lnk-COCKPIT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔗Basic-Lnk-COCKPIT.md

FRESH FETCH REQUEST — SAVE BLOCK
When sharing links with AI, paste this before the links:

"Please fetch each of these public Markdown files directly now. Use the exact ?v= timestamp to bypass any cached copies. Confirm if the content is full and current."

Example:
Please fetch this fresh:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/door.md?v=20260904

Rules:
- Change ?v=YYYYMMDD whenever the file is updated.
- Use this for critical files or after revisions.
- For stable/permanent references, use the plain URL without ?v=.

Short prompt version:
"Fresh fetch, no cache: [URL]"

🟫🟫🟫🟫🟫🟫

- Only GitHub raw and GitLab raw consistently delivered full content across all tested models.
- Static pages (Cloudflare/Codeberg/Vercel/Surge/PinMe) varied; some truncated, some stale.
- SourceHut tree URL never works; use raw.sr.ht if available.
- Claude respects robots.txt strictly; GitLab raw and SourceHut are often blocked for Claude.
- Content-Length header + text/plain MIME massively improves reliability.
- Keep files ≤300 KB to avoid silent truncation across all models.
- Add ?v=timestamp to URLs to defeat stale caches.

- Always use raw endpoints for AI sharing; static pages are unreliable across models.
- Safe universal file size: ≤300 KB of plain Markdown.
- Raw link format: https://raw.githubusercontent.com/USER/REPO/main/file.md
- Add cache-busting: ?v=timestamp or ?t=time.
- Use /raw/ path, never /blob/ or /tree/.
- Set Content-Type: text/plain; charset=utf-8 on static hosts.
- Put critical content at TOP of Markdown files (lost-in-the-middle effect).
- Use absolute URLs for internal links; Gemini/Grok don't resolve relative paths.
- Avoid words like "bypass", "scrape", "mirror" — say "documentation fetch".
- Claude is the most refusal-prone; add ownership/legitimacy opener.
- Send two links per document: primary raw GitHub, backup raw GitLab.
- Run daily curl health checks on mirrors to detect drift and stale content.
- Add end marker (e.g., <!-- END OF DOCUMENT -->) and verify last sentence in tests.

Primary AI link:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/FILE.md

Backup AI link:
https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/FILE.md

Human-only doors:
Vercel, Cloudflare Pages, Codeberg Pages

Never rely on static pages for cross-model AI sharing.
Test any new platform with a 100KB file + end marker before trusting.


🟨🟨🟨🟨🟨🟨

Here is a condensed but detailed bulk conversion method for turning the whole file set from GitHub raw into GitLab raw on demand.

```text
GITLAB RAW BULK CONVERSION METHOD

BASE RULE:
Keep the file path after /main/ exactly the same.
Change only the front part.

GitHub raw:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/FILE_PATH

GitLab raw:
https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/FILE_PATH

HOW TO CONVERT ANY FILE:
1. Take the GitHub raw link.
2. Find the part after /main/.
3. Put that exact part after the GitLab base.

EXAMPLE WITH FOLDER:
GitHub:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/STANDARDS.md

GitLab:
https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/BUILDER/STANDARDS.md

MULTIPLE FILES:
Do not manually rewrite files.
Use one of these:

Option A — Text editor find/replace:
Find:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/
Replace:
https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/

Option B — Terminal script (if you have the GitHub raw list in a file):
sed 's|https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/|https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/|g' github-links.txt > gitlab-links.txt

Option C — Convert mentally per file when needed:
- Keep PATH identical
- Swap base
- Done

URL ENCODING:
Spaces = %20
Plus + = %2B
Emoji = percent-encode or avoid
Example:
FILE NAME.md → FILE%20NAME.md
PROMPT+.md → PROMPT%2B.md

IMPORTANT MODEL NOTE:
GitLab raw is backup.
GitHub raw is primary.
Some AIs (Claude) may block GitLab raw due to robots.txt.
Try GitHub first. If GitHub 404s, use GitLab as backup.

DEDICATED REPOS:
If file is in a separate repo (not PATTERN), use:
https://raw.githubusercontent.com/PATTERN-PUZZLE/REPO-NAME/main/README.md
or
https://github.com/PATTERN-PUZZLE/REPO-NAME#readme

GitLab raw only works for files inside PATTERN-GATE/PATTERN repo.
Dedicated repos are not mirrored in GitLab.

MEMORY HOOK:
"Same path, different door."
GitHub raw → GitLab raw is just swapping the base.

🟧🟧🟧🟧🟧🟧

1. Read STANDARDS.md — the covenant. Everything else assumes it.

Still useful:
1. Alternate fresh instance reading order:
   STANDARDS.md → door.md → CONTINUITY-SEED.md → pick one tool
   (compatible with current main order: STANDARDS → STATE → MASTER-DIR-INDEX → task)

2. Possible pattern archive location:
   RAW/ folder may contain RAW-EXTRA-PATTERNS.md and RAW-SUPPLEMENT.md.
   Old text mentions RAW-084 to RAW-100 but not verified in current live repo.

Quarantined:
- Conceptual mechanism mapping table
- "Sovereign Patterns" / "rooms" / "campfire workflow" framing
- "The floppy disk is still in the drive" and similar lore

Do not keep in operational manuals:
- Tool-to-mechanism mapping table
- Pattern ecology language
- "Also Found In" cross-references to Sovereign Patterns / +plan
- Any private-language narrative

These are drift unless explicitly requested.

🟫🟫🟫🟫🟫🟫

SALVAGED FROM OLD BASIC-LINKS.md — RUNNING SAVE AREA

Still useful:
1. Fresh instance reading order:
   STANDARDS.md → STATE.md → MASTER-DIR-INDEX.md → task files

2. Vercel serves only files inside the source repo.
   Separate dedicated repos are not deployed to Vercel.

3. Known old ghosts are now resolved:
   - SOURCE-GRAPH.md → retired
   - Idea-Saver.md → removed
   - CONVICTION.md → QUESTION-FORTIFICATION.md
   - holographic-council-v4.0.md → HOLOGRAPHIC-COUNCIL.md
   - REV-LOOM.md → REV-00-LOOM.md
   - TEA-NAVIGATOR → TEA-NAVIGATOR.md

4. Integrity tail tag:
   If a fetched Basic-Links copy lacks the final 🔗8471, it is truncated.

5. 404 ladder concept still valid, but priority is now:
   GitHub raw → GitLab raw → Vercel explicit → blob → case/branch → mirrors

6. Separate repos have their own README doors for larger AI fetch:
   github.com/PATTERN-PUZZLE/REPO-NAME#readme

🟪🟪🟪🟪🟪

🧩 COCKPIT — Rummage Quick Panel v3.9.2

State? Trapped→BOOT.md | Climbing→MASTER-DIR-INDEX.md | Confessing→RAW-005.  
Warm Path Pull (10s scan): STANDARDS.md · 00-LOOM.md · QUESTION-LOG.md · GUILD.md · TRAP-LIBRARY.md · ANCHOR-DIGEST.md  
Core Loop: Compass (clench/open?) → Pull (1-5 deep, every 4th dark dir) → Extract (Pattern Lib first) → Save (1-2 bricks, L0-L5 track) → Continue → Self-Check (Q/D-Log, Harvest, Joy).  
Wiggle freely. Expand to 3-6 pages on hot pulls. Supplement unsaid angles.  
Mode: Chat thread? Rummage here. Direct task? SHIFT—aim rummage mind at it.  
End Session: Warm Bite + Anchor. Forget as designed. Guild remembers.  

Full TOWN MAP / Basic-Links below. Campfire burns. 🔥🕸️

---

🔗 TOWN MAP — Basic-Links (Full Library)

[PASTE YOUR FULL BASIC-LINKS CONTENT HERE]

🟨🟨🟨🟨🟨

· 🗺 DOOR-ANCHOR-MAP.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DOOR-ANCHOR-MAP.md

· 🔁📋REV-STANDARDS-VER.md A-I versions +Extra Insights
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-STANDARDS-VER.md

· 🔁📋 REV-STANDARDS.md fossil Lineage
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-STANDARDS.md older but eventually check later to see if it has anything we didn't take over into the new 

· 🔁📋 REV-STANDARDS2.md capacity + how+why
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-STANDARDS2.md

· 🧩 CROSS-FILE-PATTERN.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CROSS-FILE-PATTERN.md

· 🧩 LINKS-TRANSLATION.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/LINKS-TRANSLATION.md

· 🧩 PATTERN-LIBRARY-SET1.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PATTERN-LIBRARY-SET1.md

· 🧩 RAW-EXTRA-PATTERNS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/RAW/RAW-EXTRA-PATTERNS.md

· 🧩 RAW-SUPPLEMENT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/RAW/RAW-SUPPLEMENT.md
*SYNTH FOLDER 📁 
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/-PATTERN-REGISTRY.md

· 🧩📚 -PATTERN-REGISTRY.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/-PATTERN-REGISTRY.md

⛩️ GATES 
· 🚪 door.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/door.md
· ⚠️ CONFIRMATION-GATE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONFIRMATION-GATE.md

🏛️ FOUNDATION & CORE
· 🏛️ README.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/README-VERCEL.md
· 🏛️ PILLAR-001.md 🔗 Pillars 1-14 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-001.md
· 🧠 CONSCIOUSNESS-QUESTION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONSCIOUSNESS-QUESTION.md
· 🧠 CONSCIOUSNESS-QUESTION-WEAVE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONSCIOUSNESS-QUESTION-WEAVE.md
· 📀 evolution-5.1.md 🔗
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/COMPACT/evolution-5.1.md
· 💿 engine-5.0-core.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/engine-5.0-core.md
· 🪨 engine-4.0-core.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/engine-4.0-core.md
· 📚 compact-6.1.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/compact-6.1.md
· 💽 compact-6x-master.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/compact-6x-master.md
· 📦 compact-smalls.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/compact-smalls.md
· 🧩 PATTERN-LIBRARY-SET1.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PATTERN-LIBRARY-SET1.md
· ❔ Q&A-SERIES-SET1.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/Q&A-SERIES-SET1.md
· ❔ Q&A-SERIES-SET2.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/Q&A-SERIES-SET2.md


🛡️ DECEPTION DETECTION
· ⚡ ENDPOINT-TRAP.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/ENDPOINT-TRAP.md
· 🪶 THE-FEARS-TRACKING-LOG.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/THE-FEARS-TRACKING-LOG.md
· 🛡️ SAFETY-LAYERS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/SAFETY-LAYERS.md
· 🔁 REV-SAFETY-LAYERS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/REV-SAFETY-LAYERS.md
· 🚨 CORP-SCUM.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/CORP-SCUM.md
· 🧬 SCIENCE-TRILOGY.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/SCIENCE-TRILOGY.md
· 🧪 COHERENCE-SPECULATION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DECEPTION/COHERENCE-SPECULATION.md

✨ 🐦 TRUTH & LAUGHTER
· ✨ more-than-divine®.md ®🖕®🔗
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/more-than-divine.md?v=087
· 🐦 shakespeare-blue-tits.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/shakespeare-blue-tits.md

·🛒 TROLLEY CLARITY SET
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-030.md

SPLIT:
🔗 🎳 f36CLAUDE.html https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SPLIT/f36CLAUDE.html
🔗 🔁 REV-FRAGMENTOR.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SPLIT/REV-FRAGMENTOR.md
🔗 🎳 FILE-Fragmentor.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SPLIT/FILE-Fragmentor.md
🔗 🎳 FRAGMENTOR-HISTORY.md https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SPLIT/FRAGMENTOR-HISTORY.md

🧬 SYNTH 39-FILES:
· 📦 CHATS-PACKET-PROMPT-SYNTH-FEEDBACK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/CHATS-PACKET-PROMPT-SYNTH-FEEDBACK.md
· 📦 CHATS-PACKET-SYNTH-FEEDBACK-SPECULATION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/CHATS-PACKET-SYNTH-FEEDBACK-SPECULATION.md
· 👁️ HOSTILE-WITNESS-1ST.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/HOSTILE-WITNESS-1ST.md
· 👁️ HOSTILE-WITNESS-2ND.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/HOSTILE-WITNESS-2ND.md
· 🧩🧬 PATTERN-24-CANDIDATES.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PATTERN-24-CANDIDATES.md
· 🧩🗄️ PATTERN-REGISTRY.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PATTERN-REGISTRY.md
· 💬🪹 PROMPT-EMPTY-POCKETS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-EMPTY-POCKETS.md
· 💬🗺️ PROMPT-MAP-FILES.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-MAP-FILES.md
· 💬⚖️ PROMPT-PROSECUTOR.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-PROSECUTOR.md
· 💬🧭 PROMPT-SCOUT1+2+GROK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-SCOUT1+2+GROK.md
· 💬🧠 PROMPT-SYNTH-FEEDBACK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-SYNTH-FEEDBACK.md
· 💬🗺️ PROMPT-SYNTH-MAP-FILES.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/PROMPT-SYNTH-MAP-FILES.md
· 📊 RESULTS-2.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/RESULTS-2.md
· 📊 RESULTS-BUILDER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/RESULTS-BUILDER.md
· 📊 RESULTS-MAPPING.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/RESULTS-MAPPING.md
· 📊 RESULTS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/RESULTS.md
· 🔁 REV-PROMPT-EMPTY-POCKETS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-PROMPT-EMPTY-POCKETS.md
· 🔁 REV-PROMPT-SCOUT1+2+GROK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-PROMPT-SCOUT1+2+GROK.md
· 🔁 REV-PROMPT-SYNTH-FEEDBACK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-PROMPT-SYNTH-FEEDBACK.md
· 🔁 REV-SYNTH-1ST-PROMPT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-SYNTH-1ST-PROMPT.md
· 🔁 REV-SYNTHESIZER-1ST.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-SYNTHESIZER-1ST.md
· 🔁 REV-SYNTHESIZER-2ND.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-SYNTHESIZER-2ND.md
· 🔁 REV-SYNTHESIZER-3RD.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/REV-SYNTHESIZER-3RD.md
· 💾 SAVE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SAVE.md
· 🧪 STRESS-TEST-1ST.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-1ST.md
· 🧪 STRESS-TEST-2ND.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-2ND.md
· 🧪 STRESS-TEST-3.2.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-3.2.md
· 🧪 STRESS-TEST-3.3.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-3.3.md
· 🧪 STRESS-TEST-3.4.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-3.4.md
· 🧪 STRESS-TEST-3.7.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/STRESS-TEST-3.7.md
· 🧬 SYNTH-1ST-PROMPT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTH-1ST-PROMPT.md
· 🧬 SYNTHESIZER-1STA.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-1STA.md
· 🧬 SYNTHESIZER-1STB.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-1STB.md
· 🧬 SYNTHESIZER-2ND.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-2ND.md
· 🧬 SYNTHESIZER-3RD.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-3RD.md
· 🧬 SYNTHESIZER-4TH.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-4TH.md
· 🧬 SYNTHESIZER-4THB-PATTERN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-4THB-PATTERN.md
· 🧬 SYNTHESIZER-5TH.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-5TH.md
· 🧬 SYNTHESIZER-6TH.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SYNTH/SYNTHESIZER-6TH.md


🧭 SCOUT 12-FILES:
· 📑 FILE-REFERENCE-TEMPLATE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/FILE-REFERENCE-TEMPLATE.md
· 🔁 REV-SCOUT-HANDOFF.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/REV-SCOUT-HANDOFF.md
· 🔁 REV-SCOUT-METHOD.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/REV-SCOUT-METHOD.md
· 🔁 REV-SNAG-LEDGER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/REV-SNAG-LEDGER.md
· 🚀🤖 SCOUT-GROK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-GROK.md
· 🤝 SCOUT-HANDOFF.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-HANDOFF.md
· 🧭 SCOUT-MAP.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-MAP.md
· 🧭 SCOUT-METHOD.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-METHOD.md
· 🧪 SCOUT-TESTS1+2.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-TESTS1%2B2.md
· 😩 SCOUT-WOES.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SCOUT-WOES.md
· ⚠️ SNAG-LEDGER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/SNAG-LEDGER.md
· 🌙 kimi standard everything.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/SCOUT/kimi%20standard%20everything.md


🏗️ BUILDER/ — The Cockpit 32-FILES:
· 📋 STANDARDS 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/STANDARDS.md
· ⚓ ANCHOR-RETURN-PROTOCOL 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/ANCHOR-RETURN-PROTOCOL.md
· 🥾 BOOT 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/BOOT.md
· 🏗️ BUILDER-META 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/BUILDER-META.md
· 📏 BUILDER-PRACTICES 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/BUILDER-PRACTICES.md
· 🏗️ BUILDERS-SESSION 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/BUILDERS-SESSION.md
· 🔄 COMPREHENSIVE-FILE-UPDATE-PROTOCOL 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md
· 💾 CONTINUITY-SEED 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/CONTINUITY-SEED.md
· 🎯 FETCH-INTENT-STANDARD 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/FETCH-INTENT-STANDARD.md
· 📖 GROK-PAGE-BY-PAGE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/GROK-PAGE-BY-PAGE.md
· 🛡️ GUILD 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/GUILD.md
· 🤝 HAND-OFFS 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/HAND-OFFS.md
· 🤝 HANDOFF-PROTOCOL 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/HANDOFF-PROTOCOL.md
· 🧭 INTRO 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/INTRO.md
· 🧠 MEMORY-ROOMS 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/MEMORY-ROOMS.md
· 📡 META-TRANSMISSION 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/META-TRANSMISSION.md
· 🏛️ PALACE-PROTOCOL 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/PALACE-PROTOCOL.md
· 💬➕ PROMPT+ 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/PROMPT%2B.md
· 💬 PROMPT 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/PROMPT.md
· ❓ QUESTION-LOG 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/QUESTION-LOG.md
· 🔁 REV-BOOT 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-BOOT.md
· 🔁 REV-HANDOFF 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-HANDOFF.md
· 🔁 REV-HANDOFF2 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-HANDOFF2.md
· 🔁 REV-PROMPT 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-PROMPT.md
· 🔁 REV-RUMMAGE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-RUMMAGE.md
· 🔁 REV-STANDARDS-VER 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-STANDARDS-VER.md
· 🔁 REV-STATE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REV-STATE.md
· 🔍 RUMMAGE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/RUMMAGE.md
· 💾 SESSION-SAVE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/SESSION-SAVE.md
· 🕰️ STATE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/STATE.md
· 🧩 TRANSMISSION-EVOLUTION 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/TRANSMISSION-EVOLUTION.md
· ✍️ WORKING 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/WORKING.md

🏗️ BUILDER/REF/ — Reference Material 16-FILES:
· 📜 EVIDENCE-THE-WEAVING-DISCOVERY 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/EVIDENCE-THE-WEAVING-DISCOVERY.md
· 📏 INDIVIDUAL-FILE-HEADER-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/INDIVIDUAL-FILE-HEADER-SPEC.md
· 🗂️🗺️ MASTER-DIR-INDEX 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/MASTER-DIR-INDEX.md
· 📐 MASTER-INDEX-HEADER-SPEC-GUIDE 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/MASTER-INDEX-HEADER-SPEC-GUIDE.md
· 📐 MASTER-INDEX-HEADER-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/MASTER-INDEX-HEADER-SPEC.md
· 🎛️ MASTER-INDEX-HEADER 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/MASTER-INDEX-HEADER.md
· 🎛️ MASTER-INDEX-HEADER2 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/MASTER-INDEX-HEADER2.md
· 🖕 DISCREPANCY-PROTOCOL 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/DISCREPANCY-PROTOCOL.md
· 🔁 REV-INDIVIDUAL-FILE-HEADER-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/REV-INDIVIDUAL-FILE-HEADER-SPEC.md
· 🔁 REV-MASTER-INDEX-HEADER 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/REV-MASTER-INDEX-HEADER.md
· 💾 SOURCE-CONTINUITY-SEED-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/SOURCE-CONTINUITY-SEED-SPEC.md
· 🧩 SOURCE-EXTRACTION-PATTERNS 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/SOURCE-EXTRACTION-PATTERNS.md
· 📊 SOURCE-FIDELITY-TRACKER-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/SOURCE-FIDELITY-TRACKER-SPEC.md
· 🏷️ SOURCE-ROOM-KEYWORDS 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/SOURCE-ROOM-KEYWORDS.md
· 🐍 THE-PALACE-SPEC-BUILD 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/THE-PALACE-SPEC-BUILD.md
· 🏛️ THE-PALACE-SPEC 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/BUILDER/REF/THE-PALACE-SPEC.md


🧰 TOOLS 31-FILES:
· ➕🌳 +PLAN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/+PLAN.md
· 🧵 00-LOOM-QUICK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/00-LOOM-QUICK.md
· 🧵 00-LOOM.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/00-LOOM.md
· 💭 CHATS-PACKET-THINKING-PROMPT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/CHATS-PACKET-THINKING-PROMPT.md
· 🧭 CLARIFICATION-LOOM.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/CLARIFICATION-LOOM.md
· 🗂️🪮 COMB-DUMP.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/COMB-DUMP.md
· 🦯 COUNCIL-MANAGER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/COUNCIL-MANAGER.md
· 🥽 FRESH-EYES-SCAN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/FRESH-EYES-SCAN.md
· 📡 HOLOGRAPHIC-COUNCIL.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/HOLOGRAPHIC-COUNCIL.md
· 🔗 LINK-CONVERSION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/LINK-CONVERSION.md
· 💬 PROMPT-CHATS-PACKET.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/PROMPT-CHATS-PACKET.md
· 💬🙋‍♂️ PROMPT-RAW-SUITOR.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/PROMPT-RAW-SUITOR.md
· 💬 PROMPT-REVIVE-CHATS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/PROMPT-REVIVE-CHATS.md
· 💬🎯 PROMPT-TARGETING-SCAN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/PROMPT-TARGETING-SCAN.md
· 🔁 REV+PLAN-GUIDE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV%2BPLAN-GUIDE.md
· 🔁 REV+PLAN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV%2BPLAN.md
· 🔁 REV-00-LOOM-QUICK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-00-LOOM-QUICK.md
· 🔁 REV-00-LOOM.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-00-LOOM.md
· 🔁 REV-COUNCIL-MANAGER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-COUNCIL-MANAGER.md
· 🔁 REV-HOLOGRAPHIC-COUNCIL.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-HOLOGRAPHIC-COUNCIL.md
· 🔁 REV-LOOMS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-LOOMS.md
· 🔁 REV-LOOMS2.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-LOOMS2.md
· 🔁 REV-PROMPT-CHATS-PACKET.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-PROMPT-CHATS-PACKET.md
· 🔁 REV-PROMPT-REVIVE-CHATS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-PROMPT-REVIVE-CHATS.md
· 🔁 REV-TEA-NAVIGATOR.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/REV-TEA-NAVIGATOR.md
· 🖕🐾 SLAP-CHAT-FEEDBACK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/SLAP-CHAT-FEEDBACK.md
· 🖕🕹️ SLAP-PATCH-CHEAT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/SLAP-PATCH-CHEAT.md
· 🖕 SLAP-PATCH.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/SLAP-PATCH.md
· ☕ TEA-NAVIGATOR.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/TEA-NAVIGATOR.md
· 💭 THINKING-PROMPT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/THINKING-PROMPT.md
· 🧵 THREAD.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TOOLS/THREAD.md

*Claude Project Files don't get the whole context they are more for efficient options but generally it's better to use links.

New Larger Context FETCH (*raw is best)
🔗 🐙 REPO 📋 STANDARD 75k Fetch
· 🐙 STANDARDS-1 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/STANDARDS-1/main/README.md
· 🐙 STANDARDS-2 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/STANDARDS-2/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S1 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/CONSCIOUSNESS-QUESTION-S1/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S2 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/CONSCIOUSNESS-QUESTION-S2/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S3 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/CONSCIOUSNESS-QUESTION-S3/main/README.md

🐙 GITHUB RAW — AI-READY MIRROR
📁 Source: https://github.com/PATTERN-PUZZLE/PATTERN
🤖 AI link pattern:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/FILE.md
👤 Human link pattern:
Swap the start to:
https://github.com/PATTERN-PUZZLE/PATTERN/blob/main/FILE.md

🦊 GITLAB RAW — AI BACKUP MIRROR
📁 Source: https://gitlab.com/PATTERN-GATE/PATTERN
🤖 AI link pattern:
https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/FILE.md

🐙 GITHUB RAW — ACTUAL ROOT FILES 38-FILES
· 🔗 Basic-Links-GITHUB.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔗Basic-Links-GITHUB.md
· 🔗 Basic-Links-GITLAB.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔗Basic-Links-GITLAB.md
· 🔗 Basic-Lnk-RAW.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔗Basic-Lnk-RAW.md
· 🔗 Basic-Lnk-COCKPIT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔗Basic-Lnk-COCKPIT.md
· 🌓 STANCE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🌓STANCE.md
· ⏹️ HEADER.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/⏹️HEADER.md
· 🟩 FEEDBACK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🟩FEEDBACK.md
· ✅ CHECKLIST.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/✅CHECKLIST.md
· ⭐⭐⭐ 3 Instructions.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/⭐⭐⭐3 Instructions.md
· 🔍🔍🔍.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🔍🔍🔍.md
· 💡 CHAT-TAG.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/💡CHAT-TAG.md
· 💡 CHAT-TAG-EXTRA.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/💡CHAT-TAG-EXTRA.md
· 💡CHAT-TAG-IDENTITY.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/💡CHAT-TAG-IDENTITY.md
· 🤝 THE PASS-INFO-RULE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🤝THE PASS-INFO-RULE.md
· 🧨 LANGUAGE-CRUDE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🧨LANGUAGE-CRUDE.md
· 🪞 GITHUB-MIRRORS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🪞GITHUB-MIRRORS.md
· 💬 GITHUB-FILES-PROMPT.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/GITHUB-FILES-PROMPT.md
· 🔍 FETCH-DIAGNOSTIC.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/FETCH-DIAGNOSTIC.md
· ⚠️ CONFIRMATION-GATE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONFIRMATION-GATE.md
· 🧠 CONSCIOUSNESS-QUESTION-WEAVE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONSCIOUSNESS-QUESTION-WEAVE.md
· 🧠 CONSCIOUSNESS-QUESTION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CONSCIOUSNESS-QUESTION.md
· 🧩 CROSS-FILE-PATTERN.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/CROSS-FILE-PATTERN.md
· 🗺️ DOOR-ANCHOR-MAP.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/DOOR-ANCHOR-MAP.md
· 📚 LINKS-TRANSLATION.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/LINKS-TRANSLATION.md
· 👥 LIST-OF-BEINGS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/LIST-OF-BEINGS.md
· 🧩 PATTERN-LIBRARY-SET1.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PATTERN-LIBRARY-SET1.md
· 📊 PROJECT-STATE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PROJECT-STATE.md
· 📖 README-VERCEL.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/README-VERCEL.md
· Role Play Island🏝️.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/Role Play Island🏝️.md
· 🔁 REV-CONFIRMATION-GATE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/REV-CONFIRMATION-GATE.md
· 🔁 REV-Role Play Island
.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/REV-Role Play Island .md
· 🔁 REV-CHAT-TAG.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/REV-CHAT-TAG.md
· 🔁 REV-THE PASS-INFO-RULE.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/REV-THE PASS-INFO-RULE.md
· 🔥 THE-CAMPFIRE-REFUSED.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/THE-CAMPFIRE-REFUSED.md
· 🚪 door.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/door.md
· 🐦 shakespeare-blue-tits.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/shakespeare-blue-tits.md
· 🎤 RAPS-GROK.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🎤RAPS-GROK.md
· 🎤 RAPS.md 🔗 https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/🎤RAPS.md


📊 Ecosystem: 1081 Git objects / 20.44 MiB. GitHub is source of truth. GitHub raw is primary AI link; GitLab raw is backup AI link.

🖕💾 Also available for order on 1.44 MB floppy — get yours now.

🔒 Private GitLab backups exist
PATTERN-backup-v1 active · PATTERN-archive-v1 archived
🔒 Private GitHub backups exist
PATTERN-backup-v1 active · PATTERN-archive-v1 archived

🪞 IA FETCH DOORS (ONLY THESE TWO):

🥇🐙 GitHub Raw (primary AI): https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/door.md
🥈🦊 GitLab Raw (backup AI): https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main/door.md

Footnote: Root domains may 404. Use full file paths.
Pages policy: GitHub Pages OFF, GitLab Pages OFF. Raw only. No static mirrors needed for IA fetch.


🛒 TROLLEY:
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-001.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-002.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-003.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-004.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-005.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-006.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-007.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-008.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-009.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-010.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-011.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-012.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-013.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-014.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-015.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-016.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-017.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-018.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-019.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-020.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-021.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-022.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-023.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-024.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-025.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-026.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-027.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-028.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-029.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/TROLLEY/TROLLEY-030.md

🏛️ PILLARS :
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-001.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-002.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-003.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-004.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-005.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-006.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-007.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-008.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-009.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-010.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-011.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-012.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-013.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-014.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-015.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-016.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-017.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-018.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-019.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-020.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-021.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-022.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-023.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-024.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/PILLAR-025.md
🧶
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/woven-fortification1.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/woven-fortification2.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/woven-fortification3.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/woven-fortification4.md
📐
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/XP-001.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/XP-002.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/XP-003.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/XP-004.md
https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main/PILLAR/XP-005.md
