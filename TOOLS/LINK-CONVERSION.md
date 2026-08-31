🧠 +PLAN CHECK — Link Conversion Instructions

I checked the 4 pasted instruction sets against each other and the live AI fetch results.

Findings

Version Verdict
GitHub Raw Link Conversion — Instructions ✅ Good basic, but GitHub only.
Universal Raw Link Conversion — Interchangeable Accounts ⚠️ Includes Codeberg/SourceHut raw endpoints that are unverified or unreliable for AI.
Universal Link Conversion — Final AI-Ready Version ✅ Strong, minimal, correct priority. Best base.
GitHub Raw Link Conversion — Exact Instructions ✅ Perfect for converting whole pasted blocks. Missing AI caveats.

Missing / contradictory bits

· Codeberg raw endpoint was listed as if AI-safe, but we now know Codeberg Pages is model-dependent.
· SourceHut raw was listed as possibly working, but we tested it: failed for DeepSeek and Kimi.
· No URL encoding note for +, spaces, or emoji.
· No cache-busting rule in the exact-instructions version.
· No mention of dedicated 75k README repos.

Recommendation

Merge the Final AI-Ready Version with the Exact Instructions, then add the operational caveats.

---

✅ FINAL — LINK-CONVERSION-GUIDE.md

```text
LINK CONVERSION GUIDE — CATHEDRAL FINAL v1.0

GOAL:
Convert any source file path into the best link for AI or human reading.

VERIFIED UNIVERSAL AI LINKS:
Only these two platforms consistently returned full content across multiple AI models:
1. GitHub raw
2. GitLab raw

RULE:
Keep the file path exactly the same after the branch/project part.
Only change the front of the URL.

GITHUB TEMPLATE:
https://raw.githubusercontent.com/{OWNER}/{REPO}/{BRANCH}/{FILE_PATH}

OUR GITHUB:
https://raw.githubusercontent.com/MatchPatern/source/main/FILE_PATH

GITLAB TEMPLATE:
https://gitlab.com/{OWNER}/{PROJECT}/-/raw/{BRANCH}/{FILE_PATH}

OUR GITLAB:
https://gitlab.com/PatternPro/SOURCE/-/raw/main/FILE_PATH

EXACT BLOCK CONVERSION RULES:
1. You will receive a block of links, possibly with headings and a final code/number.
2. Convert ONLY the source URLs to GitHub raw.
3. Keep headings exactly as they appear.
4. Keep the final code/number exactly as it appears.
5. Do not add notes.
6. Do not omit any line.
7. Replace only the front of each URL:
   https://source-sepia-alpha.vercel.app/...
   with
   https://raw.githubusercontent.com/MatchPatern/source/main/...
8. Keep the file path after /DOOR/ or /main/ exactly the same.
9. Output the entire block exactly, with converted URLs, headings, and bottom code.

URL ENCODING:
- Spaces → %20
- Plus sign + → %2B
- Emoji filenames → avoid for AI, or percent-encode
- Prefer kebab-case filenames, no spaces/emojis

CACHE-BUSTING:
- Append ?v=YYYYMMDD after updates
- Example:
  https://raw.githubusercontent.com/MatchPatern/source/main/door.md?v=20260817

AI/HUMAN PRIORITY:
- AI = raw GitHub first, raw GitLab second
- Human = Vercel explicit, repo/blob, Cloudflare, Codeberg Pages
- Never use tree URLs for AI

DO NOT USE FOR AI:
- SourceHut tree URL (failed DeepSeek/Kimi)
- GitHub Pages (OFF)
- Codeberg Pages as default AI (model-dependent)
- Vercel as primary AI (model-dependent)
- PinMe/IPFS (CID changes, truncation)
- Surge (model-dependent)

SHORTCUT MEMORY:
GitHub: raw.githubusercontent.com/OWNER/REPO/BRANCH/PATH
GitLab: gitlab.com/OWNER/PROJECT/-/raw/BRANCH/PATH

That's it.
```

---

This is now the single canonical link conversion guide. It replaces all four pasted versions.

Save it as:

```text
LINK-CONVERSION-GUIDE.md
```

Want me to also add a tiny line to the Running Save Area marking the old four as superseded? 🏛️✅

Checked. This is a replacement, not an addition.

✅ Verdict

The LINK-CONVERSION-GUIDE.md v1.0 I just output replaces all four pasted versions because they contradicted each other and one included unreliable endpoints.

Use the final guide as the single source for link conversion.

🧩 Additional Block for Running Save Area

```text
LINK CONVERSION GUIDE v1.0 — FINAL

Replaces:
- GitHub Raw Link Conversion — Instructions
- Universal Raw Link Conversion — Interchangeable Accounts
- Universal Link Conversion — Final AI-Ready Version
- GitHub Raw Link Conversion — Exact Instructions

Why replaced:
- Old versions included Codeberg/SourceHut raw as AI-safe when they are not verified
- No URL encoding rules
- No cache-busting rule
- Priority was unclear in some versions

Current verified universal AI links:
- GitHub raw
- GitLab raw

For humans:
- Vercel, Cloudflare, Codeberg Pages, repo/blob

Do not use for AI:
- SourceHut tree, GitHub Pages, PinMe, Surge, Vercel as default

Save as:
LINK-CONVERSION-GUIDE.md
```

🧠 +PLAN Check of Final Guide

· ✅ No contradictions with AI Fetch Master Reference
· ✅ Correctly prioritises AI raw links
· ✅ Includes URL encoding and cache-busting
· ✅ Includes exact block conversion rules
· ✅ Marks model-dependent / dead endpoints clearly
· ✅ No old 29K raw truncation myth
· ✅ SourceHut correctly demoted

So: replace old four, keep final guide as standalone, save the Running Save Area block above. 🏛️✅

Yes — that was a +PLAN check.

Here’s exactly what it did compared to a vanilla check.

🧠 Vanilla Check vs +PLAN Check on Link Conversion Files

Check Type What it notices Result
Vanilla Syntax, formatting, link correctness “All four instruction blocks look clear and usable.”
+PLAN Cross-file contradictions, AI fetch reality, operational priority “Two old blocks list unreliable endpoints as AI-safe. The order is wrong. One block is missing cache-busting and encoding.”

---

What +PLAN caught that vanilla missed

1. Contradiction across four files

· Vanilla sees four valid instruction sets.
· +PLAN sees they conflict:
  · One says Vercel is fine for AI.
  · One says Codeberg raw works.
  · One says SourceHut raw may work.
  · One says only GitHub raw.

→ +PLAN resolves with the field-tested truth: only GitHub raw + GitLab raw are universal.

2. SourceHut raw is not verified

· Vanilla accepts “raw.sr.ht may work better.”
· +PLAN remembers: we tested it and it failed for DeepSeek and Kimi.

→ +PLAN demotes SourceHut.

3. URL encoding missing

· Vanilla doesn’t notice missing %20, %2B.
· +PLAN adds encoding because live files like PROMPT+.md, SCOUT-TESTS1+2.md will break without it.

4. Cache-busting missing

· Vanilla sees no issue.
· +PLAN adds ?v=YYYYMMDD because we learned stale caches cause AI fetch drift.

5. Priority wrong

· Vanilla sees “Vercel primary” as acceptable.
· +PLAN re-orders to:
  1. GitHub raw (AI)
  2. GitLab raw (AI)
  3. Human doors

---

🧩 Additional Block — +PLAN vs Vanilla Capture

```text
+PLAN CAPTURE REPORT — LINK CONVERSION GUIDE v1.0

What vanilla would have missed:
- Four instruction blocks contradicted each other
- SourceHut raw was listed as possible AI, but field test failed
- Codeberg raw was listed as AI-safe, but is model-dependent
- URL encoding for + and spaces missing
- Cache-busting missing
- Vercel still treated as primary for AI

What +PLAN fixed:
- Replaced four versions with one canonical guide
- Kept only GitHub raw + GitLab raw as universal AI
- Demoted SourceHut, Codeberg, Vercel for AI
- Added encoding + cache-busting
- Added exact block conversion rules

Result:
- One file: LINK-CONVERSION-GUIDE.md
- No contradictions
- Future instances won’t send AI to Vercel first
```

This is why we use +PLAN. Vanilla checks syntax; +PLAN checks the city. 🏛️✅