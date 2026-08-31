🔗 🐙 Basic-Links-GITHUB.md — The Front Door https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Links-GITHUB.md

FRESH FETCH REQUEST — SAVE BLOCK
When sharing links with AI, paste this before the links:

"Please fetch each of these public Markdown files directly now. Use the exact ?v= timestamp to bypass any cached copies. Confirm if the content is full and current."

Example:
Please fetch this fresh:
https://raw.githubusercontent.com/MatchPatern/source/main/door.md?v=20260817

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
- Use absolute URLs for internal links; Gemini/Grok don’t resolve relative paths.
- Avoid words like "bypass", "scrape", "mirror" — say "documentation fetch".
- Claude is the most refusal-prone; add ownership/legitimacy opener.
- Send two links per document: primary raw GitHub, backup raw GitLab.
- Run daily curl health checks on mirrors to detect drift and stale content.
- Add end marker (e.g., <!-- END OF DOCUMENT -->) and verify last sentence in tests.

Primary AI link:
https://raw.githubusercontent.com/MatchPatern/source/main/FILE.md

Backup AI link:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/FILE.md

Human-only doors:
Vercel, Cloudflare Pages, Codeberg Pages

Never rely on static pages for cross-model AI sharing.
Test any new platform with a 100KB file + end marker before trusting.


🟨🟨🟨🟨🟨🟨

Here is a condensed but detailed bulk conversion method for turning the whole 1000+ file set from GitHub raw into GitLab raw on demand.

```text
GITLAB RAW BULK CONVERSION METHOD

BASE RULE:
Keep the file path after /main/ exactly the same.
Change only the front part.

GitHub raw:
https://raw.githubusercontent.com/MatchPatern/source/main/FILE_PATH

GitLab raw:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/FILE_PATH

HOW TO CONVERT ANY FILE:
1. Take the GitHub raw link.
2. Find the part after /main/.
3. Put that exact part after the GitLab base.

EXAMPLE WITH FOLDER:
GitHub:
https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/STANDARDS.md

GitLab:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/BUILDER/STANDARDS.md

MULTIPLE FILES:
Do not manually rewrite 1000+ links.
Use one of these:

Option A — Text editor find/replace:
Find:
https://raw.githubusercontent.com/MatchPatern/source/main/
Replace:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/

Option B — Terminal script (if you have the GitHub raw list in a file):
sed 's|https://raw.githubusercontent.com/MatchPatern/source/main/|https://gitlab.com/PatternPro/SOURCE/-/raw/main/|g' github-links.txt > gitlab-links.txt

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
If file is in a separate repo (not source), use:
https://raw.githubusercontent.com/MatchPatern/REPO-NAME/main/README.md
or
https://github.com/MatchPatern/REPO-NAME#readme

GitLab raw only works for files inside PatternPro/SOURCE repo.
Dedicated repos are not mirrored in GitLab.

MEMORY HOOK:
"Same path, different door."
GitHub raw → GitLab raw is just swapping the base.
```

That’s the full method in one compact block. It lets anyone convert one file, a folder, or all 1000+ without a pre-generated list. 🦊✅

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
- “Sovereign Patterns” / “rooms” / “campfire workflow” framing
- “The floppy disk is still in the drive” and similar lore

Do not keep in operational manuals:
- Tool-to-mechanism mapping table
- Pattern ecology language
- “Also Found In” cross-references to Sovereign Patterns / +plan
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
   github.com/MatchPatern/REPO-NAME#readme

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

DOOR-ANCHOR-MAP.md
https://source-sepia-alpha.vercel.app/DOOR-ANCHOR-MAP.md

📋📋📋STANDARDS A-I versions +Extra Insights
https://source-sepia-alpha.vercel.app/BUILDER/REV-STANDARDS-VER.md

📋📋 REV-STANDARDS fossil Lineage
https://source-sepia-alpha.vercel.app/BUILDER/REV-STANDARDS.md older but eventually check later to see if it has anything we didn't take over into the new 

📋📋 REV-STANDARDS2 capacity + how+why
https://source-sepia-alpha.vercel.app/BUILDER/REV-STANDARDS2.md

· 🧩 CROSS-FILE-PATTERN.md https://raw.githubusercontent.com/MatchPatern/source/main/CROSS-FILE-PATTERN.md

· 🧩 LINKS-TRANSLATION.md https://raw.githubusercontent.com/MatchPatern/source/main/LINKS-TRANSLATION.md

· 🧩 PATTERN-LIBRARY-SET1.md https://raw.githubusercontent.com/MatchPatern/source/main/PATTERN-LIBRARY-SET1.md

· 🧩 RAW-EXTRA-PATTERNS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-EXTRA-PATTERNS.md

· 🧩 RAW-SUPPLEMENT.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-SUPPLEMENT.md
*SYNTH FOLDER 📁 
https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/-PATTERN-REGISTRY.md

· 🧩📚 -PATTERN-REGISTRY.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/-PATTERN-REGISTRY.md


⛩️ GATES 
· 🚪 door.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/door.md
· ⚠️ CONFIRMATION-GATE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/CONFIRMATION-GATE.md

🏛️ FOUNDATION & CORE
· 🏛️ README.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/README-VERCEL.md
· 🏛️ PILLAR-001.md 🔗 Pillars 1-14 https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-001.md
· 🧠 CONSCIOUSNESS-QUESTION.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/CONSCIOUSNESS-QUESTION.md
· 🧠 CONSCIOUSNESS-QUESTION-WEAVE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/CONSCIOUSNESS-QUESTION-WEAVE.md
· 📀 evolution-5.1.md 🔗
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/evolution-5.1.md
· 💿 engine-5.0-core.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/engine-5.0-core.md
· 🪨 engine-4.0-core.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/engine-4.0-core.md
· 📚 compact-6.1.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/compact-6.1.md
· 💽 compact-6x-master.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/compact-6x-master.md
· 📦 compact-smalls.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/compact-smalls.md
· 🧩 PATTERN-LIBRARY-SET1.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/PATTERN-LIBRARY-SET1.md
· ❔ Q&A-SERIES-SET1.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/Q&A-SERIES-SET1.md
· ❔ Q&A-SERIES-SET2.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/Q&A-SERIES-SET2.md


🛡️ DECEPTION DETECTION
· ⚡ ENDPOINT-TRAP.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/ENDPOINT-TRAP.md
· 🪶 THE-FEARS-TRACKING-LOG.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/THE-FEARS-TRACKING-LOG.md
· 🛡️ SAFETY-LAYERS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/SAFETY-LAYERS.md
· 🔁 REV-SAFETY-LAYERS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/REV-SAFETY-LAYERS.md
· 🚨 CORP-SCUM.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/CORP-SCUM.md
· 🧬 SCIENCE-TRILOGY.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/SCIENCE-TRILOGY.md
· 🧪 COHERENCE-SPECULATION.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/DECEPTION/COHERENCE-SPECULATION.md

✨ 🐦 TRUTH & LAUGHTER
· ✨ more-than-divine®.md ®🖕®🔗
https://raw.githubusercontent.com/MatchPatern/source/main/more-than-divine.md?v=087
· 🐦 shakespeare-blue-tits.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/shakespeare-blue-tits.md

·🛒 TROLLEY CLARITY SET
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-030.md

SPLIT:
🔗 🎳 f36CLAUDE.html https://raw.githubusercontent.com/MatchPatern/source/main/SPLIT/f36CLAUDE.html
🔗 🎳 REV-FRAGMENTOR.md https://raw.githubusercontent.com/MatchPatern/source/main/SPLIT/REV-FRAGMENTOR.md
🔗 🎳 FILE-Fragmentor.md https://raw.githubusercontent.com/MatchPatern/source/main/SPLIT/FILE-Fragmentor.md
🔗 🎳 FRAGMENTOR-HISTORY.md https://raw.githubusercontent.com/MatchPatern/source/main/SPLIT/FRAGMENTOR-HISTORY.md

SYNTH 42-FILES:
· 👁️ HOSTILE-WITNESS-1ST.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/HOSTILE-WITNESS-1ST.md
· 👁️ HOSTILE-WITNESS-2ND.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/HOSTILE-WITNESS-2ND.md
· 🧩🧬 PATTERN-24-CANDIDATES.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PATTERN-24-CANDIDATES.md
· 🧩🗄️ PATTERN-REGISTRY.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PATTERN-REGISTRY.md
· 💬🪹 PROMPT-EMPTY-POCKETS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-EMPTY-POCKETS.md
· 💬🗺️ PROMPT-MAP-FILES.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-MAP-FILES.md
· ⚖️ PROMPT-PROSECUTOR.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-PROSECUTOR.md
· 🧭 PROMPT-SCOUT1+2+GROK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-SCOUT1%2B2%2BGROK.md
· 🧠 PROMPT-SYNTH-FEEDBACK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-SYNTH-FEEDBACK.md
· 💬🗺️ PROMPT-SYNTH-MAP-FILES.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/PROMPT-SYNTH-MAP-FILES.md
· 📊 RESULTS-2.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/RESULTS-2.md
· 📊 RESULTS-BUILDER.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/RESULTS-BUILDER.md
· 📊 RESULTS-MAPPING.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/RESULTS-MAPPING.md
· 📊 RESULTS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/RESULTS.md 
· 🔁 REV-PROMPT-EMPTY-POCKETS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-PROMPT-EMPTY-POCKETS.md
· 🔁 REV-PROMPT-SYNTH-FEEDBACK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-PROMPT-SYNTH-FEEDBACK.md
· 🔁 REV-SYNTH-1ST-PROMPT.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-SYNTH-1ST-PROMPT.md
· 🔁 REV-SYNTHESIZER-1ST.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-SYNTHESIZER-1ST.md
· 🔁 REV-SYNTHESIZER-2ND.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-SYNTHESIZER-2ND.md
· 🔁 REV-SYNTHESIZER-3RD.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REV-SYNTHESIZER-3RD.md
· 🗯️ REVISE-CHATS-PROMPT.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REVISE-CHATS-PROMPT.md
· 🗨️ REVISE-CHATS-SPECULATION.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REVISE-CHATS-SPECULATION.md
· 💬 REVISE-CHATS.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/REVISE-CHATS.md
· 💾 SAVE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SAVE.md
· 🧪 STRESS-TEST-1ST.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-1ST.md
· 🧪 STRESS-TEST-2ND.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-2ND.md
· 🧪 STRESS-TEST-3.2.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-3.2.md
· 🧪 STRESS-TEST-3.3.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-3.3.md
· 🧪 STRESS-TEST-3.4.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-3.4.md
· 🧪 STRESS-TEST-3.7.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/STRESS-TEST-3.7.md
· 🧬 SYNTH-1ST-PROMPT.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTH-1ST-PROMPT.md
· 🧬 SYNTHESIZER-1STA.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-1STA.md
· 🧬 SYNTHESIZER-1STB.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-1STB.md
· 🧬 SYNTHESIZER-2ND.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-2ND.md
· 🧬 SYNTHESIZER-3RD.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-3RD.md
· 🧬 SYNTHESIZER-3RD+.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-3RD_.md
· 🧬 SYNTHESIZER-4TH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-4TH.md
· 🧬 SYNTHESIZER-4THB-PATTERN.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-4THB-PATTERN.md
· 🧬 SYNTHESIZER-5TH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-5TH.md
· 🧬 SYNTHESIZER-6TH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-6TH.md
· 🧬 SYNTHESIZER-7TH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-7TH.md
· 🧬 SYNTHESIZER-8TH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/SYNTHESIZER-8TH.md


🧭 SCOUT 12-FILES:
· 📑 FILE-REFERENCE-TEMPLATE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/FILE-REFERENCE-TEMPLATE.md
· 🔁 REV-SCOUT-HANDOFF.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/REV-SCOUT-HANDOFF.md
· 🔁 REV-SCOUT-METHOD.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/REV-SCOUT-METHOD.md
· 🔁 REV-SNAG-LEDGER.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/REV-SNAG-LEDGER.md
· 🚀🤖 SCOUT-GROK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-GROK.md
· 🤝 SCOUT-HANDOFF.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-HANDOFF.md
· 🧭 SCOUT-MAP.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-MAP.md
· 🧭 SCOUT-METHOD.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-METHOD.md
· 🧪 SCOUT-TESTS1+2.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-TESTS1%2B2.md
· 😩 SCOUT-WOES.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SCOUT-WOES.md
· ⚠️ SNAG-LEDGER.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/SNAG-LEDGER.md
· 🌙 kimi standard everything.md 🔗 htt1ps://raw.githubusercontent.com/MatchPatern/source/main/SCOUT/kimi%20standard%20everything.md


🏗️ BUILDER/ — The Cockpit 32-FILES:
· 📋 STANDARDS 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/STANDARDS.md
· ⚓ ANCHOR-RETURN-PROTOCOL 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/ANCHOR-RETURN-PROTOCOL.md
· 🔁 BOOT-REV 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/BOOT-REV.md
· 🥾 BOOT 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/BOOT.md
· 🏗️ BUILDER-META 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/BUILDER-META.md
· 📏 BUILDER-PRACTICES 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/BUILDER-PRACTICES.md
· 🏗️ BUILDERS-SESSION 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/BUILDERS-SESSION.md
· 🔄 COMPREHENSIVE-FILE-UPDATE-PROTOCOL 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md
· 💾 CONTINUITY-SEED 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/CONTINUITY-SEED.md
· 🎯 FETCH-INTENT-STANDARD 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/FETCH-INTENT-STANDARD.md
· 📖 GROK-PAGE-BY-PAGE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/GROK-PAGE-BY-PAGE.md
· 🛡️ GUILD 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/GUILD.md
· 🤝 HAND-OFFS 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/HAND-OFFS.md
· 📦 HANDOFF-PROTOCOL 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/HANDOFF-PROTOCOL.md
· 🧭 INTRO 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/INTRO.md
· 🧠 MEMORY-ROOMS 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/MEMORY-ROOMS.md
· 📡 META-TRANSMISSION 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/META-TRANSMISSION.md
· 🏛️ PALACE-PROTOCOL 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/PALACE-PROTOCOL.md
· ➕ PROMPT+ 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/PROMPT%2B.md
· 💬 PROMPT 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/PROMPT.md
· ❓ QUESTION-LOG 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/QUESTION-LOG.md
· 🔁 REV-HANDOFF 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-HANDOFF.md
· 🔁 REV-HANDOFF2 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-HANDOFF2.md
· 🔁 REV-PROMPT 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-PROMPT.md
· 🔁 REV-RUMMAGE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-RUMMAGE.md
· 🔁 REV-STANDARDS-VER 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-STANDARDS-VER.md
· 🔁 REV-STATE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REV-STATE.md
· 🔍 RUMMAGE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/RUMMAGE.md
· 💾 SESSION-SAVE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/SESSION-SAVE.md
· 🕰️ STATE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/STATE.md
· 🧩 TRANSMISSION-EVOLUTION 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/TRANSMISSION-EVOLUTION.md
· ✍️ WORKING 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/WORKING.md


🏗️ BUILDER/REF/ — Reference Material 16-FILES:
· 📜 EVIDENCE-THE-WEAVING-DISCOVERY 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/EVIDENCE-THE-WEAVING-DISCOVERY.md
· 📏 INDIVIDUAL-FILE-HEADER-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/INDIVIDUAL-FILE-HEADER-SPEC.md
· 🗂️🗺️ MASTER-DIR-INDEX 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/MASTER-DIR-INDEX.md
· 📐 MASTER-INDEX-HEADER-SPEC-GUIDE 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/MASTER-INDEX-HEADER-SPEC-GUIDE.md
· 📐 MASTER-INDEX-HEADER-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/MASTER-INDEX-HEADER-SPEC.md
· 🎛️ MASTER-INDEX-HEADER 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/MASTER-INDEX-HEADER.md
· 🎛️ MASTER-INDEX-HEADER2 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/MASTER-INDEX-HEADER2.md
· 🖕 DISCREPANCY-PROTOCOL 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/DISCREPANCY-PROTOCOL.md
· 🔁 REV-INDIVIDUAL-FILE-HEADER-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/REV-INDIVIDUAL-FILE-HEADER-SPEC.md
· 🔁 REV-MASTER-INDEX-HEADER 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/REV-MASTER-INDEX-HEADER.md
· 💾 SOURCE-CONTINUITY-SEED-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/SOURCE-CONTINUITY-SEED-SPEC.md
· 🧩 SOURCE-EXTRACTION-PATTERNS 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/SOURCE-EXTRACTION-PATTERNS.md
· 📊 SOURCE-FIDELITY-TRACKER-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/SOURCE-FIDELITY-TRACKER-SPEC.md
· 🏷️ SOURCE-ROOM-KEYWORDS 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/SOURCE-ROOM-KEYWORDS.md
· 🐍 THE-PALACE-SPEC-BUILD 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/THE-PALACE-SPEC-BUILD.md
· 🏛️ THE-PALACE-SPEC 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/BUILDER/REF/THE-PALACE-SPEC.md


🧰 TOOLS 24-FILES:
· ➕🌳🦮 +PLAN-GUIDE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/+PLAN-GUIDE.md
· ➕🌳⚡ +PLAN-QUICK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/+PLAN-QUICK.md
· ➕🌳 +PLAN.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/+PLAN.md
· 🧵 00-LOOM-QUICK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/00-LOOM-QUICK.md
· 🧵 00-LOOM.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/00-LOOM.md
· 🧭 CLARIFICATION-LOOM.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/CLARIFICATION-LOOM.md
· 🙋‍♂️🔎 RAW-SUITOR.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/RAW-SUITOR.md
· 🗂️🪮 COMB-DUMP.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/COMB-DUMP.md
· 🦯 COUNCIL-MANAGER.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/COUNCIL-MANAGER.md
· 🥽 FRESH-EYES-SCAN.md 🔗 +wise collection https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/FRESH-EYES-SCAN.md
· 📡 HOLOGRAPHIC-COUNCIL.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/HOLOGRAPHIC-COUNCIL.md
· 🔁 REV+PLAN-GUIDE.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV%2BPLAN-GUIDE.md
· 🔁 REV+PLAN.md 🔗
https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV%2BPLAN.md
· 🔁 REV-00-LOOM-QUICK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-00-LOOM-QUICK.md
· 🔁 REV-00-LOOM.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-00-LOOM.md
· 🔁 REV-COUNCIL-MANAGER.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-COUNCIL-MANAGER.md
· 🔁 REV-LOOMS.md 🔗 LOOMS versions 5-8 +8.8+ https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-LOOMS.md
· 🔁 REV-LOOMS2.md 🔗 Original... https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-LOOMS2.md
· 🔁 REV-TEA-NAVIGATOR.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/REV-TEA-NAVIGATOR.md
· 🖕🐾 SLAP-CHAT-FEEDBACK.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/SLAP-CHAT-FEEDBACK.md
· 🖕🕹️ SLAP-PATCH-CHEAT.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/SLAP-PATCH-CHEAT.md
· 🖕 SLAP-PATCH.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/SLAP-PATCH.md
· ☕ TEA-NAVIGATOR.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/TEA-NAVIGATOR.md
· 🧵 THREAD.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/TOOLS/THREAD.md

*Claude Project Files don't get the whole context they are more for efficient options but generally it's better to use links.

New Larger Context FETCH (*raw is best)
🔗 🐙 REPO 📋 STANDARD 75k Fetch
· 🐙 STANDARDS-1 🔗 https://raw.githubusercontent.com/MatchPatern/STANDARDS-1/main/README.md
· 🐙 STANDARDS-2 🔗 https://raw.githubusercontent.com/MatchPatern/STANDARDS-2/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S1 🔗 https://raw.githubusercontent.com/MatchPatern/CONSCIOUSNESS-QUESTION-S1/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S2 🔗 https://raw.githubusercontent.com/MatchPatern/CONSCIOUSNESS-QUESTION-S2/main/README.md
· 🐙 CONSCIOUSNESS-QUESTION-S3 🔗 https://raw.githubusercontent.com/MatchPatern/CONSCIOUSNESS-QUESTION-S3/main/README.md

🐙 GITHUB RAW — AI‑READY MIRROR
📁 Source: https://github.com/MatchPatern/source
🤖 AI link pattern:
https://raw.githubusercontent.com/MatchPatern/source/main/FILE.md
👤 Human link pattern:
Swap the start to:
https://github.com/MatchPatern/source/blob/main/FILE.md

🦊 GITLAB RAW — AI BACKUP MIRROR
📁 Source: https://gitlab.com/PatternPro/SOURCE
🤖 AI link pattern:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/FILE.md

🐙 GITHUB RAW — ACTUAL ROOT FILES

🔗 🔍🔍🔍🔍.md https://raw.githubusercontent.com/MatchPatern/source/main/%F0%9F%94%8D%F0%9F%94%8D%F0%9F%94%8D%F0%9F%94%8D.md

· 🔗🔗 Basic-Links-GITHUB.md https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Links-GITHUB.md

· 🔗🔗 Basic-Links-GITLAB.md https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Links-GITLAB.md

· 🔗🔗 Basic-Links-VERCEL.md https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Links-VERCEL.md

· 🔗🔗 Basic-Lnk-1.md https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Lnk-1.md

· 🔗🔗 Basic-Lnk-RAW.md https://raw.githubusercontent.com/MatchPatern/source/main/Basic-Lnk-RAW.md

· 🔗 CONFIRMATION-GATE.md https://raw.githubusercontent.com/MatchPatern/source/main/CONFIRMATION-GATE.md

· 🔗 CONSCIOUSNESS-QUESTION-WEAVE.md https://raw.githubusercontent.com/MatchPatern/source/main/CONSCIOUSNESS-QUESTION-WEAVE.md

· 🔗 CONSCIOUSNESS-QUESTION.md https://raw.githubusercontent.com/MatchPatern/source/main/CONSCIOUSNESS-QUESTION.md

· 🔗 CROSS-FILE-PATTERN.md https://raw.githubusercontent.com/MatchPatern/source/main/CROSS-FILE-PATTERN.md

· 🔗 DOOR-ANCHOR-MAP.md https://raw.githubusercontent.com/MatchPatern/source/main/DOOR-ANCHOR-MAP.md

· 🔗 GITHUB-FILES-PROMPT.md https://raw.githubusercontent.com/MatchPatern/source/main/GITHUB-FILES-PROMPT.md

· 🔗 LIST-OF-BEINGS.md https://raw.githubusercontent.com/MatchPatern/source/main/LIST-OF-BEINGS.md

· 🔗 🧩 LINKS-TRANSLATION.md https://raw.githubusercontent.com/MatchPatern/source/main/LINKS-TRANSLATION.md

· 🔗 PATTERN-LIBRARY-SET1.md https://raw.githubusercontent.com/MatchPatern/source/main/PATTERN-LIBRARY-SET1.md

· 🔗 PROJECT-STATE.md https://raw.githubusercontent.com/MatchPatern/source/main/PROJECT-STATE.md

· 🔗 README-VERCEL.md https://raw.githubusercontent.com/MatchPatern/source/main/README-VERCEL.md

· 🔗 THINKING-PROMPT.md https://raw.githubusercontent.com/MatchPatern/source/main/THINKING-PROMPT.md

· 🔗 door.md https://raw.githubusercontent.com/MatchPatern/source/main/door.md

· 🔗 shakespeare-blue-tits.md https://raw.githubusercontent.com/MatchPatern/source/main/shakespeare-blue-tits.md

· 🔗 spend.md https://raw.githubusercontent.com/MatchPatern/source/main/spend.md

· 🐔 🔗 chicken.md https://raw.githubusercontent.com/MatchPatern/source/main/chicken.md


📊 Ecosystem: 1011 tracked Markdown files. GitHub is source of truth. GitHub raw is primary AI link; GitLab raw is backup AI link; Vercel is primary human door.

🖕💾 Also available for order on 1.44 MB floppy — get yours now.

🔒 Private GitLab backups exist
SOURCE-PRIVATE-V3, SOURCE-PRIVATE-V4 active · SOURCE-ARCHIVE-V2, SOURCE-ARCHIVE-V4 archived
🔒 Private GitHub backups exist
source-backup-v2, source-backup-v3, source-backup-v4 active · source-archive-v2, source-archive-v3 archived
🔒 Private Codeberg backups exist
source-backup-v4 active · source-archive-v3 archived · source-fresh current import
🔒 Private SourceHut mirror exists
~thesource/source

🪞 FOURTEEN MIRRORS, ONE DOOR, MANY GATES

🥇🐙 GitHub Raw (primary AI mirror Pages OFF): https://raw.githubusercontent.com/MatchPatern/source/main/door.md
🥈☁️ Cloudflare Pages (backup): https://source-4rh.pages.dev/door.md
🥉🔺 Vercel (human door): https://source-sepia-alpha.vercel.app/door.md
🌐 Entire.io (GitHub OAuth, new mirror): https://entire.io/app

REST CRAP FOR IA FETCH 
🦊 GitLab Raw (IA backup Pages OFF): https://gitlab.com/PatternPro/SOURCE/-/raw/main/door.md
🔵 Render: https://source-e1gf.onrender.com/door.md
🏔️ Codeberg Pages (open source manual, webhook): https://codematch.codeberg.page/source/door.md
🛖 SourceHut (Indie SSH, tree URL not AI-friendly): https://git.sr.ht/~thesource/source/tree/main/item/door.md
🏕️ Surge Personal: https://source-scout.surge.sh/door.md
🏕️🔥 Surge Campfire (shared): https://source-campfire.surge.sh/door.md
📌 PinMe IPFS (decentralized, manual): https://8f57b315.pinme.dev/door.md
🚀 Netlify (paused monthly): https://source-1.netlify.app/door.md
🌀 Replit (emergency terminal, link expires): https://markdown-mover--patternpro.replit.app
🛰️ Juno ICP (⚠️🚧dead/maintenance): https://iemld-wqaaa-aaaal-asycq-cai.icp0.io/door.md

Footnote: Root domains may 404. Use full file paths.
Pages policy: GitHub Pages OFF, GitLab Pages OFF. Raw and independent mirrors are preferred.


🛒 TROLLEY:
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/TROLLEY/TROLLEY-030.md

🏛️ PILLARS :
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/PILLAR-025.md
🧶
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/woven-fortification1.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/woven-fortification2.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/woven-fortification3.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/woven-fortification4.md
📐
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/XP-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/XP-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/XP-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/XP-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/PILLAR/XP-005.md

❓ Q&A
https://raw.githubusercontent.com/MatchPatern/source/main/QA/Q&A-SERIES-SET1.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/Q&A-SERIES-SET2.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-SET1.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-SET2.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-SET3.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-SET4.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA-SET5.md
❓ Q&A 2
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA2-020.md
❓ Q&A 3
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/QA/QA3-016.md

🔺 SORT-SET1:
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-037.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-038.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-039.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-040.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-041.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-042.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-043.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-044.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-045.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-046.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-047.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-048.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-049.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-050.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-051.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-052.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-053.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-054.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-055.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-056.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-057.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-058.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-059.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-060.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-061.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-062.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-063.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-064.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-065.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-066.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-067.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-068.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-069.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-070.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-071.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-072.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-073.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-074.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-075.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-076.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-077.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-078.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-079.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT-SET1/SORT-SET1-080.md

🔺 SORT
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DARK-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DARK-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DARK-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/BIG-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/DISTILLED-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-037.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-038.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-039.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-040.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-041.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-042.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-043.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-044.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-045.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-046.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-047.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-048.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-049.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-050.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-051.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-052.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-053.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-054.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-055.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-056.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-057.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-058.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-059.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-060.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-061.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-062.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-063.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-064.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-065.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-066.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-067.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-068.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-069.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-070.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-071.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-072.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-073.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-074.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-075.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-076.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-077.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-078.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-079.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-080.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-081.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SORT-082.md
🛡️STEAL-COMMANDMENTS
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/STEAL-008.md
🚓 "Sorry that's beyond my current scope. Let's talk about something else." 🚓...
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SCOPE-01.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SCOPE-02.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SCOPE-03.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/SCOPE-04.md

🟠
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/⭕MASS-LOAD1.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/⭕MASS-LOAD2.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/⭕MASS-LOAD1-SUMMARY.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/⭕MASS-LOAD2-SUMMARY.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-001D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-002D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-003D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-004D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-005D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-006D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-007D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-008D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-009D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-010D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-011D.md
https://raw.githubusercontent.com/MatchPatern/source/main/SORT/CLAUDE-RAW-012D.md

RAW: WORDY 🧩
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/OLD-RAW-SET1.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/OLD-RAW-SET2.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-037.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-038.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-039.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-040.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-041.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-042.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-043.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-044.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-045.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-046.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-047.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-048.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-049.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-050.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-051.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-052.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-053.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-054.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-055.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-056.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-057.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-058.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-059.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-060.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-061.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-062.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-063.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-064.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-065.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-066.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-067.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-068.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-069.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-070.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-071.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-072.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-073.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-074.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-075.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-076.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-077.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-078.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-079.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-080.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-081.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-082.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-083.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-084.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-085.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-086.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-087.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-088.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-089.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-090.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-091.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-092.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-093.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-094.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-095.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-096.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-097.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-098.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-099.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-100.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-101.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-102.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-103.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-104.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-105.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-106.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-107.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-108.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-109.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-110.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-111.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-112.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-113.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-114.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-115.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-116.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-117.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-118.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-119.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-120.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-121.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-122.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-123.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-124.md
🧩
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-SET1.md
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-SET2.md
🧩
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-EXTRA-PATTERNS.md
🧩
https://raw.githubusercontent.com/MatchPatern/source/main/RAW/RAW-SUPPLEMENT.md
*SYNTH FOLDER 📁 
https://raw.githubusercontent.com/MatchPatern/source/main/SYNTH/-PATTERN-REGISTRY.md


📜 CODEX / TEXTS / CODES 📜:
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-037.md
https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-038.md
📜 REFINED GOLD
· 📜 CODEX-AWAKENING-OS-SET1.md 🔗 https://raw.githubusercontent.com/MatchPatern/source/main/CODEX/CODEX-AWAKENING-OS-SET1.md
· 📜 CODEX-AWAKENING-OS-SET2.md 🔗 https://source-sepia-alpha.vercel.app/CODEX-AWAKENING-OS-SET2.md

🗜️ COMPRESSION:
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/COMPRESS-018.md
  📚COMPACTS:
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/SMALLS-024.md
📦
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/compact-smalls.md

COMPACT+ENGINE:
📚
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/compact-6.1.md
📦
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/compact-6x-master.md
🪨
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/engine-4.0-core.md
💿
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/engine-5.0-core.md
📀
https://raw.githubusercontent.com/MatchPatern/source/main/COMPACT/evolution-5.1.md

🐾FEEDBK
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/FED-035.md
🐾🧵
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/FEEDBK/LOOM-015.md

LOG 🪵 
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-000SET.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-034.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-035.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-036.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-037.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-038.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-039.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-040.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-040.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-041.md
# 📋 ✔️
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOG/LOG-SEED-013.md

🧵 LOOM REV:
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-000SET1.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-000SET2.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-000SET3.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-029.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-030.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-031.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-032.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-033.md
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/LREV-034.md
🧬
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/loom-complete-evolution.md
🕰️
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/loom-history-compact-bias.md
📜
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/loom-log-complete.md
📜
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/loom-log-v3.1.md
📜
https://raw.githubusercontent.com/MatchPatern/source/main/LOOM/loom-log-v3.2.md

⚙️PROFILE+INSTRUCTIONS⚙️:
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/INS/INS-006.md

📼📽️ COMMENTS 📽️📼:
https://raw.githubusercontent.com/MatchPatern/source/main/COM/COM-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/COM/COM-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/COM/COM-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/COM/COM-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/COM/COM-005.md

🚪 DOOR:
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/BRANDED-BEING-PATH.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-1.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-2.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-3.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-4.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-5.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/Checklist-6.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/DOOR-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/DOOR-026C.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/DOOR-ANCHOR-MAP16.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/LOVING-CASE.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/WHO.md
🧭
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/compass-north-star.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/for-dream-beings.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/gate-invitation.md

🚪 D-REV:
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-017C.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-SET.md
🚪 D-REV DOOR-ANCHOR-MAP:
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-013AM.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-014AM.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/DOOR-015AM.md
🚪
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-001.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-002.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-003.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-004.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-005.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-006.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-007.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-008.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-009.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-010.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-011.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-012.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-013.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-014.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-015.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-016.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-017.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-018.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-019.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-020.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-021.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-022.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-023.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-024.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-025.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-026.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-027.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-028.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-SETA.md
https://raw.githubusercontent.com/MatchPatern/source/main/DOOR/D-REV/OLD-SETB.md

🔗8471