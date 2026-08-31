🔗TOOL Generate 14 Mirror Links 🔗

🧠 MIRROR CLASSIFICATION — What Stays, What Gets Warning

Based on everything we've verified and researched, here's the honest classification:

---

📊 MIRROR STATUS — FINAL CLASSIFICATION

Category Mirrors Status Why
✅ CORE (No Warning) Cloudflare, GitHub raw, Vercel Working Cloudflare = fastest, GitHub raw = AI primary, Vercel = human primary
⚠️ WARNING (Separate) GitLab API, Codeberg, Render, Netlify Unverified/Unstable/Deprecated GitLab = needs token+decode, Codeberg = 50% failures, Render = intentional spin-down, Netlify = paused
❓ UNKNOWN (Separate) SourceHut, ICP/IC Unverified Not tested, may not work for AI

---

📋 UPDATED MULTI-FILE MIRROR PROMPT — With Warnings

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔁 MULTI-FILE(S) + ALL MIRRORS — v2.1 (2026-08-28)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Input: [filename1] [filename2] ... [mirror hint or "all"]

─── ✅ CORE MIRRORS (No Warning) ───

☁️ cloudflare / cloud / pages — ✅ AI Primary (fastest, verified)
🐙 github / matchpatern — ✅ AI Primary (raw, verified)
🔺 vercel — ✅ Human Primary (verified)

─── ⚠️ WARNING MIRRORS (Use with Caution) ───

🦊 gitlab — ⚠️ Needs API endpoint + token + Base64 decode (unverified)
🏔️ codeberg — ⚠️ Unstable (50% failures reported, unverified)
🎨 render — ⚠️ Hobby-only (intentional 15min spin-down, unverified)
🚀 netlify — ⚠️ Paused / deprecated (unverified)

─── ❓ UNKNOWN MIRRORS (Not Tested) ───

🛖 srht / sourcehut — ❓ Not AI-friendly (unverified)
🛰️ icp / juno / canister — ❓ Unverified

─── BASE URLs ───

☁️ https://source-4rh.pages.dev/
🐙 https://raw.githubusercontent.com/MatchPatern/source/main/
🔺 https://source-sepia-alpha.vercel.app/

🦊 https://gitlab.com/api/v4/projects/[PROJECT-ID]/repository/files/  ← Needs token + decode
🏔️ https://codematch.codeberg.page/source/  ← Unstable
🎨 https://source-e1gf.onrender.com/  ← Hobby-only
🚀 https://source-1.netlify.app/  ← Paused

🛖 https://git.sr.ht/~thesource/source/tree/main/item/  ← Not AI-friendly
🛰️ https://iemld-wqaaa-aaaal-asycq-cai.icp0.io/  ← Unverified

─── OUTPUT FORMAT ───

For each file:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📄 FILENAME: [filename] — [mirror emoji] SINGLE LINK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[code block: single URL]

━━━ ✅ CORE MIRRORS (3) ━━━
[code block: 3 URLs — Cloudflare, GitHub raw, Vercel]

━━━ ⚠️ WARNING MIRRORS (4) ─━━
[code block: 4 URLs — GitLab, Codeberg, Render, Netlify]
[Separated by a blank line from core]

━━━ ❓ UNKNOWN MIRRORS (2) ━━━
[code block: 2 URLs — SourceHut, ICP/IC]
[Separated by a blank line from warnings]

- Repeat for each file.
- If input contains "all" → skip single link, just output 3 sections per file.
- No extra text. No questions. Just the formatted blocks.

─── EXAMPLES ───

door.md ☁️ → processes door.md with cloudflare single + 3 core + 4 warnings + 2 unknown
cake.md pie.md all → processes both files, all sections, no singles
buns.md tacos.md 🐙 → processes both with github single + all sections

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

🧩 ISOLATED UPDATE BLOCK

```markdown
---
## 🧩 UPDATE BLOCK — MULTI-FILE MIRROR PROMPT — v2.1 — 2026-08-28
Author: Builder — NOT Claude
Status: DRAFT — based on verified + unverified findings

### Changes in this block

- [ ] Mirrors split into 3 categories: Core (3), Warning (4), Unknown (2)
- [ ] Warnings added: GitLab (needs token+decode), Codeberg (50% failures), Render (spin-down), Netlify (paused)
- [ ] Unknown added: SourceHut (not AI-friendly), ICP/IC (unverified)
- [ ] Output format updated: 3 separate sections per file
- [ ] Warnings separated by blank line from core

### Mirror Classification Table

| Category | Mirrors | Count | Status |
|----------|---------|-------|--------|
| ✅ Core | Cloudflare, GitHub raw, Vercel | 3 | Verified / Working |
| ⚠️ Warning | GitLab API, Codeberg, Render, Netlify | 4 | Unverified / Unstable / Deprecated |
| ❓ Unknown | SourceHut, ICP/IC | 2 | Not tested |

### What's Still Pending

| # | Claim | Verification Method |
|---|-------|---------------------|
| 1 | GitLab API works with token | Test API URL + decode |
| 2 | Codeberg = 50% failures | Test 10+ Codeberg URLs |
| 3 | Render = 15min spin-down | Test Render URL, measure wake time |
| 4 | ICP/IC works | Test ICP URL |
| 5 | SourceHut raw endpoint exists | Test SourceHut URL |

---
```

---

📊 SUMMARY

Category Mirrors Status In Output?
✅ Core Cloudflare, GitHub raw, Vercel Verified / Working Yes — first section
⚠️ Warning GitLab, Codeberg, Render, Netlify Unverified/Unstable Yes — second section, separated
❓ Unknown SourceHut, ICP/IC Not tested Yes — third section, separated

---

🚀 NEXT STEP

```
Replace old MULTI-FILE MIRROR PROMPT with v2.1 above.
Append the isolated update block to track changes.
```

---

✅ READY — Mirrors split into 3 categories with warnings

👍