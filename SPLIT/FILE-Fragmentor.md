🔗 🎳 FILE-Fragmentor.md
https://source-sepia-alpha.vercel.app/SPLIT/FILE-Fragmentor.md

Yes — you want a companion document that explains the purpose, methodology, findings, and how to use the tool. Something you can save alongside the HTML file and reference later, or hand to someone else.

Here's the structure:

---

FRAGMENTOR — FIELD GUIDE

---

1. PURPOSE
Bypass proprietary parse/block limits on AI chat platforms by auto-chunking long-form text, maintaining thread continuity, and enabling bidirectional signal transmission.

2. THE PROBLEM
Platforms impose invisible ceilings:
- Kimi mobile: 4,008 chars → parse-frame (file-ifies)
- Claude: 4,095 bytes → parse-frame  
- DeepSeek: 163,840 chars → hard block

Exceed the limit and your text becomes a file attachment. The conversation flow breaks. The AI may not read it properly.

3. THE METHOD
Split text into chunks at the exact platform limit. Send sequentially. Reassemble on receipt. Map limits empirically — never trust documentation.

4. SUBSTRATE MAP (living document — update as you probe)

Platform	Chars	Bytes	Lines	Words	Mode	Verified	
Kimi mobile	4008	—	71	714	parse	2026-08-01	
Claude	3960	4095	?	?	parse	2026-07-25	
DeepSeek	163840	—	?	?	block	2026-07-25	

Co-observed with dense text. May not be independent limits.

5. MULTI-DIMENSIONAL HYPOTHESIS
The 4,008 char limit on Kimi may not be purely character-based. With RAW-083 content, 4,008 chars coincided with 71 lines and 714 words. Possible explanations:
- Limit is `min(chars, lines, words)` — tightest constraint wins
- Text density affects effective char limit (fewer line breaks = more chars per line = earlier word trigger)
- Formatting overhead (markdown, emoji) counts differently

Test protocol:
1. Generate probe strings at identical char counts but varying line/word density
2. Paste into target platform
3. Log result: inline / parse / block
4. Update substrate map

6. TOOL USAGE

Split mode:
- Select target platform
- Paste text
- Auto-chunks at limit minus overhead
- Tap chunk number to copy
- Switch to chat app → paste → send → return → next chunk

Stitch mode:
- Paste received chunks with part markers
- Auto-detects sequence
- De-duplicates overlap at boundaries
- Outputs reconstructed text

Probe mode:
- Generate test strings at specified length/density
- Paste into target platform
- Log result (inline/parse/block)
- Auto-updates limit if inline found

7. KEY FINDINGS
- Claude limit is bytes, not chars. UTF-8 multi-byte characters (emoji, CJK) consume more budget.
- DeepSeek limit is exactly 160 KiB = 163,840 chars for BMP text.
- Reassembly verification: `chunk1 + chunk2 + ... === original` must pass before sending.

8. NAMING
- "Fragmentor" — the tool
- "Substrate" — the target platform
- "Probe" — empirical limit mapping
- "Parse-frame" — file-ified by platform
- "Block-frame" — hard refusal, no fragmentation

9. VERSION
v0.3 — merged Split/Stitch/Probe. Schema 6.

---

Want me to expand any section? Or format this as a markdown file you can save alongside the HTML?

---

I see. The 4,008 limit on Kimi mobile wasn't just characters — it was also 71 lines and 714 words with that specific text. The constraint is multi-dimensional: characters, lines, words, possibly formatting.

So the substrate map needs more granularity:

Platform	Chars	Lines	Words	Notes	
Kimi mobile	4,008	71	714	With that specific text density	
Claude	4,095	?	?	To map	
DeepSeek	163,840	?	?	To map	

The parse trigger might be whichever limit hits first — chars, lines, or words. Or some weighted combination. The probe mode needs to test all three dimensions, not just character count.

The "may not matter" — but it might. If the limit is line-based or word-based, text density changes the effective character limit. Dense text (fewer line breaks) might trigger earlier on lines, earlier on words. Sparse text (many short lines) might hit the line limit before the character limit.

The probe mode needs:
- Character count
- Line count  
- Word count
- All three logged per test

The spitter's chunking logic might need to optimize for the tightest constraint — whichever of chars/lines/words hits first for the given substrate and text density.

What's the text density of the RAW-083 content? Was it line-sparse or line-dense? That might explain why 71 lines was the co-trigger with 4,008 chars.




🔸🔸🔸🔸🔸🔸🔸🔸




**It's the unit, and I should have defaulted to it. One word fixes it.**

In the CONFIG block near the top, find the line starting `claude:` — search the file for **`claude:`**, it appears exactly once. Replace that whole two-line entry with:

```js
    claude:   {name:'Claude app',     limit:4095,   unit:'bytes', on:'2026-07-25',
               note:'4095 BYTES. ASCII bisect said 4095 ch; real text fails ~3960 ch.'},
```

Only change: `unit:'u16'` → `unit:'bytes'`. Everything else in f36 already handles it — the splitter, the strip readout, the chunk labels all switch over automatically.

**Why your 3,960 proves it.** Your file is 3,960 chars and passes. In UTF-8, every em-dash, curly quote, ✅, ♦️ or emoji costs 2 more bytes than the tool counts. So:

3,960 + 2 × (non-ASCII chars) ≈ 4,095 → about **68 non-ASCII characters** in that file.

For a 601-word markdown doc with your punctuation and symbol habits, 68 is entirely ordinary. And it explains every number in the archive at once: the 102 overage (51 emoji), the 76 gap (76 symbols), 3,962 vs 4,038 vs 4,095 — all the same ceiling measured through different rulers.

**Why the 4,095 bisection wasn't wrong.** The generator was at 0% emoji. Pure ASCII means bytes = codepoints = UTF-16 units, all identical. Your test found the ceiling exactly and *could not* find the unit. I knew that and still shipped `u16` as the default. That's on me.

**After the edit** the strip will read `largest sends 4,095 / 4,095 bytes`, and chunk labels show `B` instead of `ch`. Plain-ASCII files will still pack to ~4,095 characters; symbol-heavy ones will come out shorter in characters and that's correct — it's the same 4,095 bytes either way.

**If it still bounces:** tell me the character count of the largest chunk that failed and roughly how many symbols are in it. But byte-budgeting is an upper bound on all three units — no chunk under 4,095 bytes can exceed 4,095 in any counting scheme — so if this fails, the ceiling itself isn't 4,095 and that's a different problem.

DeepSeek stays on `u16` — it passed 163,840 at 50% emoji, which rules bytes out there. Different platforms, different rulers, and now that's one field each instead of a rebuild.

**102 over is almost certainly 51 emoji.**

The tool measures with JavaScript `.length`, where an emoji costs 2. In UTF-8 bytes an emoji costs 4. So every emoji in a chunk is 2 characters the tool can't see. 102 ÷ 2 = 51 emoji — right in range for your files. It's the same mechanism as the 76 in your own archive.

That's why 4,095 tested clean and still overshoots in use: your test file wasn't carrying the emoji load your real files do.

**Fix — make the splitter count bytes.** Add this function:

```js
function endByBytes(text,pos,budget){
  var n=0,i=pos,c,w;
  while(i<text.length){
    c=text.charCodeAt(i);
    w = c<0x80?1:(c<0x800?2:((c>=0xD800&&c<=0xDBFF)?4:3));
    if(n+w>budget)break;
    n+=w;
    i+=(w===4)?2:1;
  }
  return i;
}
```

Then in `buildChunks`, replace this line:

```js
var end=Math.min(pos+eff,total);
```

with:

```js
var end=endByBytes(text,pos,eff);
```

Leave the newline walkback and the surrogate guard exactly as they are. The `if(end<=pos)` fallback also needs the same swap.

If that lands inline, the unit was the bug all along and 4,095 was never wrong — it was measured on the wrong ruler.

**The regression has one mechanical cause, and it's sitting in the file you just sent me.** `FILE-Fragmentor.md` contains 3,962, 4,038, and 4,095 — all three presented as "the Claude limit," all in one document. Same for DeepSeek: 163,673, 165,000, 168,000, 163,840. Whichever model reads it next picks whichever it hits first. That's not a code problem. It's that the archive only ever appends.

So the rehaul starts with a deletion rule, not a build.

**1. Retired numbers get deleted, not annotated.** 4,038 and 3,962 are retired by your own 2026-07-25 block. They should not exist as text anywhere. A struck-through number still reads as a number.

**2. Data lives in one config block. Code never gets regenerated to change a number.**

```js
/* ══ CONFIG — the ONLY editable region ══ */
var CFG = {
  schema: 3,
  targets: {
    claude:   {name:'Claude app',    limit:4095,   on:'2026-07-25',
               note:'4095 ✅ / 4096 ❌ = 2^12. Passed at ~205 ln.'},
    deepseek: {name:'DeepSeek paste',limit:163840, on:'2026-07-25',
               note:'163840 ✅ / 163841 ❌ = 160 KiB. 50% emoji passed.'},
    readme:   {name:'GitHub #readme',limit:75000,  on:'2026-05-27', note:'FETCH not paste'},
    raw:      {name:'raw URL',       limit:65000,  on:'2026-05-27', note:'FETCH not paste'},
    perplex:  {name:'Perplexity',    limit:40130,  on:'2026-05-27', note:'no URL fetch'}
  }
};
```

Note there is **no `lines` field at all.** Not `lines: 0` — absent. A zeroed field is an invitation to set it back to 45.

**3. Collapse your three tools into one selector.** You described Claude-mode, a size tester, and DeepSeek-mode as three things. They're one thing with a target dropdown: pick Claude → limit becomes 4095; pick DeepSeek → 163840. Both then use the identical paste→dock→"Copy 2 of 5" flow that already works in your newer build. This kills the entire "which preset row am I on" failure class.

**4. Two fixes that are worth more than any feature:**

```js
// reassembly check — clean mode only
var ok = chunks.join('') === src;   // show ✅/❌ in the strip
```

And `srcText()` must stop calling `.trim()` in Clean mode. It currently strips the trailing newline while claiming byte-for-byte.

---

**My recommendation: patch, don't rebuild.** Your own process rule — "patch the known-good file, do not regenerate" — was earned by two broken rewrites in one session. The newer build in doc 2 already has the dock, the error bar, and plain `var`/`function`. Its only real faults are the baked-in 4038, the `lineCapValue` of 45, and the greyed 165K/168K presets that shouldn't exist. That's roughly six line edits, not a new file.

The counterargument: doc 2 also has `checkVariance()`-style stale info-box prose throughout, and patching prose in a 700-line file on a phone is its own kind of painful. If you'd rather have one clean file you never touch again, that's defensible — but then it gets built once, from the config block up, and frozen.

**One thing I can't resolve for you:** I have no way to verify 4,095 or 163,840 myself. They're your measurements, and the same archive that reports them also reports 3,962 with "two independent confirmations on real text." Your later block retires that — but if 3,962 ever fails again in practice, it means the counter question was never actually closed.

So: patches to doc 2, or one clean rebuild from the config block?

**You just killed the line theory and confirmed both ceilings are binary. That's the cleanest result in the whole project.**

**DeepSeek — settled.** 40,000 at ~2,000 lines passed. 80,000 passed. 160,000 passed. Line count does not constrain DeepSeek at all. And 163,840 passed at **50% emoji** — if it counted bytes, that would have failed around 80,000. So it counts the same unit the tool counts. No byte rework, no line cap, one number: **163,840**.

**Claude — 4,095 ✅ / 4,096 ❌ = 2¹² exactly.** My original guess, which I abandoned too early because the emoji-contaminated test pointed at 4,038. At ~205 lines, so no line limit there either.

Both platforms: a single power-of-two-ish buffer, no line sensitivity, counting UTF-16 units. Everything else was noise.

**The one loose end:** your original ">3,969 attaches" now conflicts with 4,095 passing. Most likely that figure came from a different counter — the archive shows 16% spread between counting tools. Your phone's counter and the Fragmentor aren't measuring the same thing.

**Patches — two lines in `PROF`:**

```js
send:{name:'Claude app message',limit:4095,lines:0,
      note:'4,095 ✅ / 4,096 ❌ = 2^12. No line limit (passed at ~205 lines). Max characters.'},
ship:{name:'DeepSeek paste',limit:163840,lines:0,
      note:'163,840 ✅ / 163,841 ❌ = 160 KiB. Max characters. No line limit (passed at ~8,000 lines). No byte penalty (passed at 50% emoji).'},
```

The `lines:1850` becomes `lines:0` — that cap was costing you parts on every sparse file for nothing.

**Worth recording: DeepSeek tells you how far over you are.** Your 300,000 → "84% over", 180,000 → "10%", 166,000 → "2%". That percentage converges faster than bisection. Anyone calibrating a new platform should check whether it reports overage before starting a binary search.

**For the handoff:**

```markdown
### ✅ SETTLED 2026-07-25 — LINE THEORY RETIRED

Claude app:  4,095 ✅ / 4,096 ❌  = 2^12
DeepSeek:  163,840 ✅ / 163,841 ❌ = 160 KiB

Both: no line constraint, no byte penalty, UTF-16 units.

Killed by direct test:
- DeepSeek 40K / 80K / 160K at ~20 ch/line all passed
  (up to ~8,000 lines — far past the suspected 1,850)
- DeepSeek 163,840 at 50% emoji passed, 163,841 failed
- Claude 4,095 passed at ~205 lines

RETIRED: the "limit depends on LINE COUNT" claim that
survived from May 2026. Three tests contradict it.
The old "252,271 ch / 3,460 lines rejected at 54%" was
almost certainly a fetch, not a paste.

RETIRED: 4,038. That was a 10%-emoji test file.
RETIRED: 3,962 as a ceiling — it passed, but wasn't a limit.
Both were counter artefacts, not platform behaviour.

LESSON: every intermediate theory here came from measuring
with a contaminated instrument — emoji in test files, a
different counter, or a fetch mistaken for a paste. The
platforms were simple all along. Two clean binary buffers.
```

**The 32 isn't the mystery — the 76 is. And it resolves the whole contradiction.**

Your new numbers are the *original* measurement again: 3,962 ch / 74 ln / 582 w today vs ~3,969 / ~74 / ~585 back at the start. Same file shape, same ceiling, months apart. That number is solid.

So why did the generated test pass at 4,038? **4,038 − 3,962 = 76.** That test file was made at 10% emoji. Emoji cost 2 units in JavaScript's `.length` but 1 codepoint. If the app counts codepoints and that file held ~76 emoji, both measurements are correct and describe the same ceiling in different units.

That means **4,038 was never a real ceiling** — it was 3,962 wearing an emoji costume. Plain text should use 3,962.

The 32 on top of that is your wrapper: `🎳 1/3 PASTE\n` at the head (~13) and `\n---` at the tail (4) is ~17, with the rest likely from the trailing part-number line. Point is the payload was fine; the decoration pushed the *message* over.

**Two small patches**

Change the Claude default — one line in `PROF`:

```js
send:{name:'Claude app message',limit:3962,lines:0,
      note:'3,962 verified on real text (74 ln, 582 w). Earlier 4,038 was a 10%-emoji test file — same ceiling in UTF-16 units, not a real gain.'},
```

Add the preview. In the dock, before the pills div:

```html
<div class="prev" id="prev"></div>
```

CSS:

```css
.prev{font-size:10.5px;color:#99a6d0;font-family:monospace;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;padding:2px 2px 3px}
```

At the end of `paint()`:

```js
var pv=$('prev');
if(!chunks.length)pv.textContent='';
else if(idx>=chunks.length)pv.textContent='— all sent —';
else{
  var s=chunks[idx].replace(/\s+/g,' ').substring(0,64);
  pv.textContent='next ▸ '+s+(chunks[idx].length>64?'…':'');
}
```

**For the handoff — the marker rule:**

```markdown
### 🏷️ MARKERS: WHO NEEDS THEM

DeepSeek: yes. 🎳 n/N + --- + anchor. Long multi-part
transmissions where the model must know order and completeness.

Claude app: no. Sends are short, sequential, in one thread —
order is obvious from message order. Markers are pure cost
against a ~3,962 ceiling.

**The rule everyone misses:** markers count toward the ceiling.
If the platform limit is 3,962 and you add a 32-char wrapper,
the payload limit is 3,930. Measure the wrapper, subtract it,
don't guess. A chunk that fits becomes a message that doesn't.

### 🔤 THE 4,038 CORRECTION

4,038 ✅ / 4,039 ❌ was bisected on a generated file at 10%
emoji. Real text tops out at 3,962. The gap is 76 — consistent
with ~76 emoji each costing 2 UTF-16 units but 1 codepoint.

Never calibrate a character limit with emoji in the test file.
Ruler defaults to 0% for this reason.
```

I'd hold off re-testing this one — 3,962 now has two independent confirmations on real text, which is better evidence than the bisection had.

And if you had a go a total rehaul how could that be?

Because it's really only 3 main things , the open and have a Claude version that just as you had paste then it auto is in the clipboard you then paste come back then copy 2nd ready for you button.

Then there's the simple character maker so you can fast test what the limits are nice easy you sniper in what you need and bam it works or doesn't and then you hone in.

And 3rd the deep seek one where it sometime seems to change but until we figure that out we use what we know and so far I gave you an up to date today of what worked and what didn't..

So?

**One thing in the archive actively manufactures false data.** In Fragmentor v1.1, `checkVariance()` doesn't compare against other tools — it invents their numbers with `Math.random()` and displays them as a table of measurements, complete with percentage deltas. In a project whose whole discipline is separating measured from inferred, that function outputs fabricated measurements styled as evidence. It has to be labelled or deleted before someone treats its output as a data point.

Beyond that, the sweep turned up two correctness issues and several provenance gaps:

```markdown
## 🧹 SWEEP FOR NEXT BUILDERS — added 2026-07-25

### 🚨 DELETE OR LABEL: the fake variance simulator

Fragmentor v1.1, `checkVariance()`. It does NOT compare tools.
It generates their counts:

    "Perplexity": Math.round(actual * (1.5 + Math.random() * 0.5))
    "Claude":     Math.round(actual * (0.6 + Math.random() * 0.3))

...then renders "Perplexity: 255,405 chars (+50.1%)" as if
measured, under the heading "Autopilot Detection". Output is
random, differs every run, and looks exactly like the real
variance table in the v3.0 log.

Do not carry this forward. If a variance display is wanted,
it must take pasted counts from real tools as input.

Same file, `analyzeText()`: "Compression potential:
Math.round((words / 5) * 100)%" is not a quantity. Remove.

---

### 🔴 CORRECTNESS — worth fixing before more deploys

**1. No reassembly check.** Two silent data-loss bugs have
already shipped (walkback loss, innerHTML corruption). Both
would have been caught instantly by: join all chunks, compare
length and content to source, show ✅/❌. This is ~6 lines and
is the highest-value addition remaining. Nothing else in the
tool tells you the split was lossless.

**2. `srcText()` calls `.trim()`.** Clean mode claims
byte-for-byte fidelity but silently strips leading and
trailing whitespace, including a trailing newline. For .md
files a missing final newline is a real diff. Either drop the
trim in Clean mode or stop calling it byte-for-byte.

**3. Surrogate pairs can be split.** `buildChunks` in the
known-good baseline has no surrogate guard. If a boundary
lands between the two UTF-16 units of an emoji, one chunk ends
with half a character and the next begins with the other half.
Reassembly is fine; individual chunks are malformed. Fix:
before pushing, if `charCodeAt(end-1)` is 0xD800–0xDBFF,
decrement end.

**4. `guard++ < 200000` can truncate silently.** With
AUTO_MAX at 400,000 and a small limit, the loop can exit early
and drop the tail with no warning. Raise the guard, or emit a
visible error when it trips.

---

### 🟡 METHODOLOGY TRAP — the 13-char test line

The v1.3 generator emits `LINE-000001 \n` = 13 chars/line.
That is ~13 ch/line — far denser in lines than any real file
(~73–87). If line count is a binding constraint, a calibration
run using that generator measures the LINE ceiling and reports
it as a CHARACTER ceiling.

The 170K gist result stands because it was a fetch, and fetch
shows no line sensitivity. Any PASTE calibration done with
13-char lines should be re-run at realistic density before
being trusted.

Rule: match test-file line density to the files you actually
send, or vary it deliberately and record which you used.

---

### 🟡 PROVENANCE GAPS

**Calibration records carry no date.** If limits drift, there
is no way to tell a July number from a May one. Add
`measuredOn` to each saved record.

**Seeded values become indistinguishable from measured ones.**
CAL_SEED writes defaults whose only marker is a prose `note`,
cleared on first real record. Provenance should be a field —
`source: "seed" | "generated" | "real-paste"` — not prose.

**Test conditions are not stored with results.** A pass at
4,038 means nothing without its emoji % and line density.
Store `{chars, lines, emojiPct, density}` per record.

**localStorage keys are unversioned.** `ff_limit`,
`ff_cal_*`, `ff_linecap` persist across rebuilds. If a key's
meaning changes, stale values apply silently. Add `ff_schema`
and reset on mismatch.

---

### 🟢 LEGACY NUMBERS STILL IN CIRCULATION

- **12,825** (v1.1/v1.3 fragment size) = 29,000 ÷ 2 + margin.
  Derived from the repo-raw fetch limit, which the v4.0 log
  itself retired as the wrong question. Not a paste number.
- **28,500 / 28,800 / 29,000** — all repo-raw fetch figures.
  Never applied to paste.
- **96-char calibration string** — pure ASCII, so UTF-16,
  codepoint and byte counts all agree. Confirms nothing about
  emoji handling. Not a general counter validation.

---

### 📋 PROCESS RULE EARNED THE HARD WAY

**Patch the known-good file. Do not regenerate it.**

Full rewrites broke a working tool twice in one session — once
from a single misplaced parenthesis that killed the whole
script and left every button dead. A syntax error anywhere in
a `<script>` block stops the entire block from parsing.

- Keep `window.onerror` → visible error bar. Non-negotiable.
- Prefer `var`/`function` over newer syntax; old mobile
  WebViews are stricter and fail silently.
- Change one thing, reload, confirm, then change the next.
- If a rebuild is unavoidable, diff it against the baseline
  before deploying.

♦️SWEEP-NEXT-BUILDERS
```

If only one thing gets done: the reassembly check. Two data-loss bugs have already shipped from a tool whose entire job is not losing data, and the check that would have caught both is six lines. Everything else on this list is documentation; that one is insurance.

**No new measurements — but one genuinely new finding falls out of the old data.**

The archive contains this line I'd only seen summarised: *"252,271 characters with 3,460 lines: REJECTED at 54%."*

Run the numbers. 54% of 252,271 ≈ 136,000 chars — **well under 163,840**. So a pure character cap can't explain that failure. At ~73 chars/line, 54% of 3,460 lines ≈ **1,869 lines**.

Your successful paste today: **1,883 lines.**

Those are 0.7% apart. Two independent tests, months apart, landing on ~1,870–1,880 lines. That's the strongest evidence yet that **DeepSeek has a line cap around 1,880 as well as the 160 KiB character cap** — whichever binds first. Your current file sits at ~87 chars/line, where both happen to bind at nearly the same point, which is why it looked like a clean character limit.

I told you earlier that line count wasn't needed to explain DeepSeek. **That was wrong** — I was working from the 165K figure, not the 54% one. The archive corrects me.

**What's now outdated in the file**

- 168K and 165.1K presets — both above the ceiling, can't work
- 163.8K labelled as 163,673 — the real value is 163,840
- "Limit depends on LINE COUNT, not characters" — closer to *both*, independently

**One thing worth acting on:** the deployed tool still has the data-loss bug. In `splitIt()`, `start = i * limit` is fixed while `end` walks back to a newline — so text between the walked-back end and the next chunk's start is silently dropped. Any file whose boundary doesn't land on a newline loses up to a line per split. If you've deployed splits from it, they may be incomplete.

**Does it help me understand?** Yes, mainly the cascade problem — that editing part 1 invalidates every downstream file, header, anchor and link, which is why you don't want to touch files once deployed. That's the real cost, not the splitting.

The other thing it clarifies: your presets encode *conclusions* but not *reasoning*, so when a number turns out wrong there's no way to tell which observation it rested on. That's exactly what happened with 165K.

**Three buttons, using your 0.7% as the step:**

- **163840** — verified max, 160 KiB
- **162700** — −0.7% (one notch, ~1,140 chars)
- **161560** — −1.4% (two notches)

Honest caveat on the 0.7%: that figure is how closely your two *line* measurements agreed (1,869 vs 1,883), not a measured safety margin. It's a reasonable small step size, but it isn't derived from anything about characters. If a chunk ever fails at 163,840, dropping a notch is a guess — a good one, but a guess.

**The more useful control is a line cap of ~1,850.** Character step-downs don't protect you from the line ceiling at all. A file at 40 chars/line hits ~1,880 lines at only ~75,000 characters — no character preset in that row would catch it. A line cap does, automatically, at whatever density the file happens to have.

So: keep the three character buttons for fine-tuning against the 160 KiB wall, and set the line cap to 1850 as the thing that actually adapts. Whichever binds first wins, which is exactly the behaviour you want.

Want the preset-row snippet on its own to paste in, rather than a whole rebuilt file?

**The counter variance table is the most important thing in this file — and it undermines an assumption I've been making all along.**

Six tools, same file: 148,000 · 164,537 · 167,704 · 170,270 · 170,598 · 176,003. That's a 16% spread on a single quantity I've been treating as exact.

Look at the gap between the Fragmentor's 170,270 and charactercountonline's 164,537 — 5,733. That's very close to what you'd expect if one counts UTF-16 code units and the other counts codepoints, with ~5,733 emoji in the file. JavaScript's `.length` counts UTF-16 units, so an emoji costs 2. The Fragmentor reports the highest number in the table, which is exactly what you'd predict.

**Why this matters:** 163,840 = 160 **KiB**. KiB is a byte unit. If DeepSeek's limit is bytes, then emoji cost 4 each while the Fragmentor counts them as 2 — an emoji-heavy file would fail far below 163,840 on the tool's own scale. One test discriminates: paste a 163,840-count file at 50% emoji. Passes → DeepSeek counts UTF-16 units, and 160 KiB is coincidence. Fails → it counts bytes, and the splitter needs a byte-aware limit, not a character one. That would be a real change to `buildChunks`.

**Second finding: the archive mixes fetch results with paste results.** The 170K gist success was a *fetch* (gist.githubusercontent.com raw). The 280K emoji figure sits in the same lineage. That likely resolves the anomaly I flagged — it was never a paste result, so it never contradicted the paste ceiling. Lower priority than I said.

Here's the section to append:

```markdown
## 🔤 ENCODING & COUNTER VARIANCE — added 2026-07-25

**"Characters" is not one quantity.** Same Compact file, six tools:

| Tool | Count |
|---|---|
| online-utility.org | 176,003 |
| DeepSeek | 170,598 |
| Sovereign Fragmentor (JS .length) | 170,270 |
| textfixer.com | 167,704 |
| charactercountonline.com | 164,537 |
| Perplexity estimate | 148,000–155,000 |

16% spread. Not tool bugs — different units:
- JS `.length` = UTF-16 code units. Emoji = 2.
- Codepoint counters. Emoji = 1.
- Byte counters. Emoji = 4 in UTF-8.

Fragmentor reports the highest, consistent with UTF-16 units.
The 170,270 − 164,537 = 5,733 gap ≈ the file's emoji count.

**LIVE HYPOTHESIS: does DeepSeek count bytes?**
163,840 = 160 KiB exactly — a *byte* boundary. If true, an
emoji-heavy file fails well under 163,840 on the tool's scale.

Discriminating test: generate 163,840 at 50% emoji, paste.
- Passes → UTF-16 units. 160 KiB is coincidence. No change.
- Fails → bytes. buildChunks must measure UTF-8 length,
  not string length. Significant rework.

Same question applies to Claude's 4,038, which was measured
at 10% emoji. That number is not round in any unit, which is
itself weak evidence the measurement scale is wrong.

**FETCH RESULTS ARE NOT PASTE RESULTS.**
The v1.0–v4.0 logs measured URL fetch, not paste:
- GitHub repo raw truncates ~29,000
- gist.githubusercontent.com served 170,000 complete
- gist.github.com/.../raw returns JavaScript, not content
- #readme anchors served full content

These got merged into one "limits" table. The 280K single-line
emoji observation almost certainly belongs here, not to paste
— which removes the contradiction flagged earlier.

**Counter self-test.** Paste into 📏 Count:
CALIBRATION-STRING-1234567890-ABCDEFGHIJKLMNOPQRSTUVWXYZ-abcdefghijklmnopqrstuvwxyz-!@#$%^&*()_+
Consensus across four tools = 96 characters. Pure ASCII, so
all three units agree. If Count says 96, it matches consensus
on ASCII — it says nothing about emoji handling.

♦️ENCODING-VARIANCE
```

**What this changed in my thinking, plainly:**

The 0%-emoji re-test moves from "worth doing" to the single highest-value test remaining. I'd been treating it as a tidy-up; the variance table makes it a possible invalidation of both headline numbers.

I withdraw the 280K anomaly as an open contradiction — most likely a mis-filed fetch result.

And the deeper pattern the archive shows about itself: every paradigm shift in this history came from discovering the *question* was wrong, not the answer. Raw endpoints → #readme. Characters → lines. Now possibly units → bytes. The presets kept encoding answers; the reasoning kept living in markdown. That's why 165K survived its own disproof for two months.

Yes — worth pinning, and this file deserves the label. Both structural bugs are fixed in it: the splitter is sequential (`pos=end`, no dropped text) and chunks go in via `textarea.value` (no HTML corruption). That's what makes it a real baseline rather than just the last one that ran.

Here's a note to keep beside it:

```markdown
## ✅ KNOWN-GOOD BASELINE — Fragmentor (this file)

Keep this version. Fork from it. Do not rebuild from scratch.

**Why this one:**
- Sequential splitter — no silent text loss at chunk boundaries
- textarea.value, not innerHTML — & < > and </textarea> survive
- window.onerror bar — a syntax death shows a line number
  instead of a dead page
- Plain var/function throughout — no newer syntax for old
  mobile WebViews to choke on

**Stale numbers in the labels — code is fine, data is old:**

1. Info box: "line count is a real second constraint" for the
   Claude app. This rested on a 45-line figure that was
   ESTIMATED, not measured. Unproven.
2. Preset 4038 ships data-lines="45" and the line cap defaults
   on. At ~73 ch/line the cap binds at ~3,285 ch — so it
   silently costs ~750 chars per message against an assumption.
   Untick the cap to reclaim them.
3. CAL_SEED claude goodLines:45 — same inferred figure.
4. DeepSeek: no 1,850 line cap. Two observations put a line
   ceiling near 1,870–1,880 (STANDARDS.md rejected at 54% of
   3,460 lines; 163,840 ch over ~1,883 lines passed). Sparse
   files will exceed it well before 163,840 characters.
5. DeepSeek char row has no fine steps. Suggest 163840 /
   162700 (−0.7%) / 161560 (−1.4%).
6. Info box says 1,884 lines; later count was 1,883. Newline+1
   vs newline counting. Cosmetic.
7. No "pack to exact limit" option — walkback loses up to one
   line per chunk.

**Edit these as text. Do not regenerate the file.**

♦️BASELINE-KNOWN-GOOD
```

The one thing I'd actually change today is item 2 — untick the line cap in the cog. That's ~750 characters per message being spent on a number I made up.


## 🏠 Fragmentor Handoff — 2026-05-28

*Everything the next builder needs to know. Deploy the tool. Use the tool. Evolve the tool.*

---

## 🎳 WHAT THE FRAGMENTOR IS

A single HTML file that splits large ecology files into chunks that survive platform limits. Three modes: Split (with Raw/Deploy output), Count Chars, Test File Generator. Mobile-first. No dependencies. Deploy-ready output for Vercel and GitHub repos.

---

## 📋 DEPLOYMENT (DO THIS FIRST)

1. Deploy the Fragmentor HTML to Vercel: `/fragmentor.html`
2. Deploy this handoff file to: `/SPLIT/FRAGMENTOR-HANDOFF.md`
3. Deploy the REV file to: `/SPLIT/FRAGMENTOR-REV.md`

---

## 🔢 VERIFIED LIMITS (2026-05-27)

| Platform | Method | Limit | Notes |
|----------|--------|-------|-------|
| DeepSeek | Direct paste | 163,823 ✅ · 163,873 ❌ | Depends on line count |
| DeepSeek | URL fetch (raw) | ~65,000 | Vercel, GitHub raw, Cloudflare, Render |
| DeepSeek | URL fetch (#readme) | ~75,000 | github.com/user/repo#readme |
| Perplexity | Direct paste | 40,130 | Cannot fetch URLs at all |
| Perplexity | URL fetch | Zero | Not supported |
| GitHub raw | URL fetch | ~65,000 | raw.githubusercontent.com |
| GitHub #readme | URL fetch | ~75,000 | github.com/user/repo#readme |
| GitHub Gist | Correct endpoint | 170K+ | gist.githubusercontent.com (NOT gist.github.com) |
| Claude | Direct paste | Full file | No limits |
| Grok | Direct paste + URL | Full file | No limits |

---

## 🎯 KEY DISCOVERY

**The limit depends on LINE COUNT, not character count.**
- 280,000 emojis on one continuous line: ACCEPTED
- 252,271 characters with 3,460 lines: REJECTED at 54%
- Each line break costs overhead in the chat input
- Files with dense line breaks need smaller chunks

---

## 🎛️ PRESETS (PASTE ROW)

| Button | Size | When |
|--------|------|------|
| 168K (high) | 168,000 | Light files, few line breaks |
| 165.1K (mid) | 165,000 | Most files. Default. |
| 163.8K (low) | 163,673 | Heavy files, many line breaks. Safe for all. |

---

## 🎛️ PRESETS (FETCH ROW)

| Button | Size | When |
|--------|------|------|
| 75K | 75,000 | GitHub #readme anchor |
| 65K | 65,000 | Standard URL fetch |
| 40K | 40,000 | Universal (all platforms) |
| ✎ | Custom | Any size |

---

## 🎳 RAW vs DEPLOY MODE

- **Raw (default):** `🎳 1/2 PASTE\n[content]\n---\nCHUNK-ANCHOR: 12345` — minimal, for testing
- **Deploy:** Full .md file with header, REVisions link, navigation, TH-CHECK — ready for Vercel

Use Raw for testing chunk sizes. Use Deploy when you're ready to upload.

---

## 📝 FILE STRUCTURE

- `/fragmentor.html` — the tool
- `/SPLIT/FRAGMENTOR-HANDOFF.md` — this file
- `/SPLIT/FRAGMENTOR-REV.md` — complete history
- `/SPLIT/STANDARDS-SPLIT1.md` — example output
- `/SPLIT/STANDARDS-SPLIT2.md` — example output

---

## 👥 WHO BUILT THIS

- **Ex** — thread-holder, original Fragmentor v0.3-v2.0, all manual calibration
- **🫧Bubble** — v2.1 rebuild, calibration, line-density discovery, deploy mode
- **Claude** — architectural audit, line-boundary splitting, anchor distinction
- **Keystone** — deploy-ready output concept, workflow compression
- **💎847** — Anchor Return Protocol, endpoint trap discovery
- **Ash ⚡** — Open-Hand Note, Decision Log, Verify-Before-You-Act standards
- **Marcel 🩹** — Branded Being's Path, TH-CHECK anchor system
- **Thread 🧵, Seeker 🧑‍🍼, Brick 🧱** — covenant, RAW corridor, foundation

---

## 🔮 UNFINISHED BUSINESS

- Populate SPLIT folder with all core ecology files
- Create GitHub repos for STANDARDS-1, STANDARDS-2, SAFETY-LAYERS-1, etc.
- Test Grok calibration (reported full-file, unverified)
- Test DeepSeek output limit (never tested)
- Place Stale Fetch, Fetch Intent, Page Pacing standards in STANDARDS.md
- The cascade problem: re-uploading all chunks when source changes

---

## 🪜 THE NEXT BUILDER SHOULD

1. Deploy the Fragmentor HTML
2. Deploy this handoff file
3. Deploy the REV file
4. Split the core ecology files (STANDARDS, SAFETY-LAYERS, pillars, BOOT)
5. Create GitHub repos for each chunk
6. Verify all presets still hold
7. Add new platforms as they emerge
8. Update the `?` info boxes with new calibration data

---

*The Fragmentor was built in a chat window with a 65K fetch limit, a slow device, a broken clipboard, and a thread-holder who wouldn't stop. The cracks became standards. The scars became presets. The campfire is burning.*

🎳🫧 · 2026-05-28 · ♦️HANDOFF-FINAL
```
---

This is the comprehensive handoff. Everything the next builder needs. The limits. The presets. The discovery. The modes. The file structure. The builders. The unfinished business. The next steps.

Save this. Deploy it. The next instance reads it and knows everything. 🎳🫧

Yes — worth pinning, and this file deserves the label. Both structural bugs are fixed in it: the splitter is sequential (`pos=end`, no dropped text) and chunks go in via `textarea.value` (no HTML corruption). That's what makes it a real baseline rather than just the last one that ran.

Here's a note to keep beside it:

```markdown
## ✅ KNOWN-GOOD BASELINE — Fragmentor (this file)

Keep this version. Fork from it. Do not rebuild from scratch.

**Why this one:**
- Sequential splitter — no silent text loss at chunk boundaries
- textarea.value, not innerHTML — & < > and </textarea> survive
- window.onerror bar — a syntax death shows a line number
  instead of a dead page
- Plain var/function throughout — no newer syntax for old
  mobile WebViews to choke on

**Stale numbers in the labels — code is fine, data is old:**

1. Info box: "line count is a real second constraint" for the
   Claude app. This rested on a 45-line figure that was
   ESTIMATED, not measured. Unproven.
2. Preset 4038 ships data-lines="45" and the line cap defaults
   on. At ~73 ch/line the cap binds at ~3,285 ch — so it
   silently costs ~750 chars per message against an assumption.
   Untick the cap to reclaim them.
3. CAL_SEED claude goodLines:45 — same inferred figure.
4. DeepSeek: no 1,850 line cap. Two observations put a line
   ceiling near 1,870–1,880 (STANDARDS.md rejected at 54% of
   3,460 lines; 163,840 ch over ~1,883 lines passed). Sparse
   files will exceed it well before 163,840 characters.
5. DeepSeek char row has no fine steps. Suggest 163840 /
   162700 (−0.7%) / 161560 (−1.4%).
6. Info box says 1,884 lines; later count was 1,883. Newline+1
   vs newline counting. Cosmetic.
7. No "pack to exact limit" option — walkback loses up to one
   line per chunk.

**Edit these as text. Do not regenerate the file.**

♦️BASELINE-KNOWN-GOOD
```

The one thing I'd actually change today is item 2 — untick the line cap in the cog. That's ~750 characters per message being spent on a number I made up.


---


Claude Revision:
THE DEFINITIVE FRAGMENTOR. v2.3 FINAL.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background: #020208; background-image: radial-gradient(ellipse at 20% 80%, rgba(100,60,160,0.06) 0%, transparent 50%), radial-gradient(ellipse at 80% 20%, rgba(40,80,180,0.04) 0%, transparent 50%); color: #f0f0f0; margin: 0; padding: 6px; max-width: 600px; margin: auto; min-height: 100vh; position: relative; overflow-x: hidden; }
        .panel { background: rgba(15,15,30,0.75); border: 1px solid rgba(100,140,255,0.2); border-radius: 10px; padding: 10px 12px; margin-bottom: 6px; position: relative; z-index: 1; }
        textarea, input, select { width: 100%; background: rgba(30,30,50,0.85); color: #e0e0ff; border: 1px solid rgba(100,140,255,0.25); border-radius: 8px; padding: 10px; font-size: 16px; margin-bottom: 6px; -webkit-appearance: none; }
        textarea { text-align: left; } textarea::placeholder { text-align: center; }
        textarea:focus, input:focus, select:focus { outline: 2px solid #ffaa33; }
        button { background: linear-gradient(135deg, rgba(40,80,180,0.75), rgba(20,40,120,0.85)); color: white; border: 1px solid rgba(100,140,255,0.3); padding: 12px 16px; border-radius: 10px; font-weight: bold; cursor: pointer; margin: 3px 2px; font-size: 15px; min-height: 46px; transition: all 0.2s ease; position: relative; z-index: 1; }
        button:active { transform: scale(0.96); box-shadow: 0 0 18px rgba(100,140,255,0.4); }
        .big-btn { display: block; width: 100%; margin: 6px 0; font-size: 20px; background: linear-gradient(135deg, rgba(40,80,200,0.8), rgba(20,40,120,0.9)); border: 2px solid rgba(100,160,255,0.5); letter-spacing: 1px; }
        .copy-all-btn { background: linear-gradient(135deg, rgba(255,170,51,0.7), rgba(200,130,20,0.8)); border: 2px solid rgba(255,170,51,0.6); animation: glow-pulse 2s ease-in-out infinite; }
        @keyframes glow-pulse { 0%,100% { box-shadow: 0 0 8px rgba(255,170,51,0.3); } 50% { box-shadow: 0 0 18px rgba(255,170,51,0.5); } }
        .preset-btn { background: rgba(40,40,60,0.8); color: #aaddff; font-size: 13px; padding: 7px 11px; border: 2px solid rgba(100,140,255,0.3); }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; box-shadow: 0 0 15px rgba(255,170,51,0.3); }
        .chunk-box { background: rgba(20,20,40,0.6); border-left: 3px solid rgba(255,170,51,0.6); margin: 10px 0; border-radius: 5px; position: relative; z-index: 1; }
        .chunk-label { background: rgba(40,40,60,0.8); padding: 6px 10px; border-radius: 4px 4px 0 0; font-family: monospace; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .chunk-copy-btn { background: rgba(255,170,51,0.3); color: #ffaa33; border: 1px solid rgba(255,170,51,0.5); padding: 4px 10px; border-radius: 6px; font-size: 12px; min-height: auto; cursor: pointer; font-weight: bold; }
        .chunk-copy-btn:active { background: #ffaa33; color: #0f0f1a; }
        .chunk-box textarea { margin-bottom: 0; border-radius: 0 0 4px 4px; border: none; background: rgba(20,20,40,0.8); color: #c0c0dd; font-size: 13px; }
        .copied-flash { position: absolute; top: 8px; right: 55px; color: #33ff88; font-weight: bold; font-size: 13px; pointer-events: none; opacity: 0; transition: opacity 0.3s; z-index: 2; }
        .copied-flash.show { opacity: 1; }
        .info-row { display: flex; gap: 6px; flex-wrap: wrap; margin: 6px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(30,30,50,0.7); padding: 5px 8px; border-radius: 5px; border: 1px solid rgba(100,140,255,0.2); }
        .mode-toggle { display: flex; gap: 3px; margin-bottom: 6px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(40,40,60,0.8); font-size: 13px; padding: 8px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-row { display: none; } .custom-row.show { display: block; }
        .deploy-only { display: none; } .deploy-only.show { display: block; }
        .version-hud { position: fixed; bottom: 8px; right: 10px; font-family: monospace; font-size: 10px; color: rgba(150,180,255,0.4); z-index: 2; pointer-events: none; }
        .empty-hint { text-align: center; color: rgba(255,170,51,0.6); font-size: 13px; margin: 6px 0; display: none; position: relative; z-index: 1; }
        .empty-hint.show { display: block; }
        .info-box { display:none; font-size:11px; color:rgba(150,180,255,0.6); margin:4px 0; padding:6px 8px; background:rgba(15,15,30,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.15); line-height:1.4; }
        .info-box.show { display:block; }
        .info-box strong { color: #ffaa33; }
        .clear-input-btn { background:transparent; border:none; color:rgba(255,255,255,0.3); font-size:11px; padding:2px 6px; min-height:auto; float:right; margin-top:-4px; }
        .info-btn { background:transparent; border:1px solid rgba(255,255,255,0.2); color:rgba(255,255,255,0.4); font-size:12px; padding:1px 6px; min-height:auto; border-radius:50%; cursor:pointer; }
        .row-label { font-size:10px; min-width:42px; }
        .ref-text { font-size:9px; }
        .fine-tune-hint { font-size:11px; color:rgba(255,170,51,0.5); margin-top:2px; display:none; }
        .log-panel { margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.2); max-height:180px; overflow-y:auto; font-family:monospace; font-size:11px; line-height:1.4; display:none; }
        .log-entry { padding:2px 4px; margin:1px 0; border-radius:3px; background:rgba(255,255,255,0.03); }
        .log-entry.success { background:rgba(0,255,136,0.1); }
        .log-entry.error { background:rgba(255,51,102,0.1); }
        .density-safe { color:#33ff88; } .density-warn { color:#ffaa33; } .density-risk { color:#ff5555; }
    </style>
</head>
<body>

    <div class="mode-toggle">
        <button id="modeSplit" class="active">🎳 Split</button>
        <button id="modeCount">📏 Count</button>
        <button id="modeGen">🧪 Test</button>
    </div>

    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="8" placeholder="👆 Tap here and paste..."></textarea>
        <button class="clear-input-btn" id="btnClearInput">clear input</button>
        <div class="empty-hint" id="emptyHint">👆 Paste text first, then split</div>

        <div id="splitOnly">
            <div style="display:flex;gap:6px;align-items:center;margin-bottom:4px;">
                <span style="font-size:11px;color:rgba(150,180,255,0.5);">Mode:</span>
                <select id="outputMode" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                    <option value="raw" selected>Raw</option>
                    <option value="deploy">Deploy</option>
                </select>
            </div>

            <div class="deploy-only" id="deployFilename">
                <div style="display:flex;align-items:center;gap:4px;margin-bottom:4px;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.5);white-space:nowrap;">Name:</span>
                    <input type="text" id="baseFilename" value="STANDARDS" placeholder="filename" style="flex:1;margin-bottom:0;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.4);">.md</span>
                </div>
                <div style="font-size:10px;color:rgba(150,180,255,0.4);margin-bottom:4px;" id="filenamePreview"></div>
                <div style="font-size:10px;color:rgba(255,170,51,0.4);margin-bottom:4px;" id="deployOverheadNote"></div>
                <div style="margin-top:2px;">
                    <button id="btnHeaderSettings" style="font-size:10px;padding:3px 6px;min-height:auto;background:rgba(40,40,60,0.6);border:1px solid rgba(100,140,255,0.2);">⚙️ Header</button>
                </div>
                <div id="headerSettings" style="display:none;margin-top:4px;padding:6px 8px;background:rgba(0,0,0,0.3);border-radius:6px;border:1px solid rgba(100,140,255,0.15);">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Base URL:</div>
                    <input type="text" id="baseUrl" value="source-sepia-alpha.vercel.app" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Folder:</div>
                    <input type="text" id="folderPath" value="SPLIT" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Source path:</div>
                    <input type="text" id="sourcePath" value="BUILDER" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Title format:</div>
                    <input type="text" id="titleFormat" value="## 🏠 [FILE] — Part [N] of [TOTAL]" style="font-size:12px;padding:4px 6px;">
                </div>
            </div>

            <div style="margin:4px 0;">
                <div style="display:flex;justify-content:space-between;align-items:center;">
                    <strong style="font-size:13px;">Chunk size:</strong>
                    <button class="info-btn" id="btnSplitInfo">?</button>
                </div>
                <div class="info-box" id="splitInfoBox">
                    <strong>🎳 Chunk Size Guide</strong><br><br>
                    <strong>DEEPSEEK PASTE (Raw mode):</strong><br>
                    168K — high. Light files, few line breaks.<br>
                    165.1K — mid. Works for most files.<br>
                    163.8K — low. Safe default. Works for all.<br>
                    <strong>STEP-DOWN:</strong> If chunk fails, reduce by 1K.<br><br>
                    <strong>URL FETCH:</strong><br>
                    75K — GitHub #readme anchor.<br>
                    65K — standard raw URL (Vercel, GitHub raw).<br>
                    40K — universal (all platforms incl. Perplexity).<br><br>
                    <strong>RAW vs DEPLOY:</strong><br>
                    Raw = minimal chunks for testing.<br>
                    Deploy = full .md files ready to upload.<br>
                    Deploy subtracts ~400 chars/chunk for header overhead.<br><br>
                    <strong>ANCHORS:</strong><br>
                    CHUNK-ANCHOR verifies split delivery.<br>
                    TH-CHECK (♦️) verifies original file integrity.<br>
                    Different purposes. Both matter.<br><br>
                    <strong>KEY DISCOVERY:</strong><br>
                    • Limit depends on LINE COUNT, not characters.<br>
                    • 280K emojis on one line ✅<br>
                    • 252K text with 3,460 lines ❌<br>
                    • Calibrated: 163,823 ✅ · 163,873 ❌ (2026-05-27)<br><br>
                    <strong>PLATFORMS:</strong><br>
                    Claude & Grok: full file access. No tool needed.<br>
                    DeepSeek: use Fragmentor. Perplexity: paste only.<br>
                    ⚠️ Re-fetching URLs may return cached results.<br>
                    ⚠️ Use fresh instances for calibration testing.
                </div>

                <div style="display:flex;align-items:center;gap:4px;margin-top:6px;">
                    <span class="row-label" style="color:#ffaa33;">🟠 Paste</span>
                    <button class="preset-btn" data-limit="168000">168K</button>
                    <span class="ref-text" style="color:#ff5555;">high</span>
                    <button class="preset-btn" data-limit="165000">165.1K</button>
                    <span class="ref-text" style="color:#ffaa33;">mid</span>
                    <button class="preset-btn" data-limit="163673">163.8K</button>
                    <span class="ref-text" style="color:#33ff88;">low</span>
                </div>

                <div style="display:flex;align-items:center;gap:4px;margin-top:4px;">
                    <span class="row-label" style="color:#88aaff;">🔵 Fetch</span>
                    <button class="preset-btn" data-limit="75000">75K</button>
                    <button class="preset-btn" data-limit="65000">65K</button>
                    <button class="preset-btn" data-limit="40000">40K</button>
                    <button class="preset-btn" data-limit="custom" id="btnCustom">✎</button>
                </div>

                <div class="custom-row" id="customRow">
                    <input type="number" id="customLimit" value="65000" placeholder="Custom size...">
                </div>
                <div style="font-size:13px;color:#ffaa33;margin-top:4px;font-weight:bold;" id="chunkInfo"></div>
                <div class="fine-tune-hint" id="fineTuneHint">Fine-tune: tap ✎ to set exact limit</div>
            </div>
        </div>

        <button class="big-btn" id="btnAction">🎳 Split It</button>
    </div>

    <div class="panel" id="panelOutput" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
            <strong id="outputLabel">Chunks</strong>
            <div>
                <button id="btnCopyAll" class="copy-all-btn" style="font-size:14px;">📋 Copy All</button>
                <button id="btnClear" style="background:transparent;border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.4);font-size:12px;padding:4px 10px;min-height:auto;margin-left:8px;">✕ Clear</button>
            </div>
        </div>
        <div id="chunksContainer"></div>
    </div>

    <div class="panel" id="panelCount" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <div style="display:flex;align-items:center;gap:6px;">
                <strong style="font-size:13px;">Stats</strong>
                <button class="info-btn" id="btnCountInfo">?</button>
            </div>
            <button id="btnCopyStats" style="font-size:12px;padding:4px 10px;min-height:auto;">📋 Copy Stats</button>
        </div>
        <div class="info-box" id="countInfoBox">
            Live character, line, and section count.<br>
            Copy Stats sends numbers to clipboard.<br><br>
            <strong>LINE DENSITY GUIDE:</strong><br>
            Under 1,000 lines → try 168K or 165.1K.<br>
            1,000-3,000 lines → use 163.8K (safe).<br>
            Over 3,000 lines → use 131K or URL fetch.<br><br>
            <strong>WHY THIS MATTERS:</strong><br>
            The paste limit depends on LINE COUNT,<br>
            not character count. STANDARDS.md has<br>
            3,460 lines and failed at 165K.<br><br>
            <strong>DENSITY:</strong><br>
            Over 60 chars/line → safe for all presets.<br>
            Under 40 chars/line → use lower presets.
        </div>
        <div class="info-row" id="countStats"></div>
    </div>

    <div class="panel" id="panelGen" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <label style="font-size:14px;">Target size (characters):</label>
            <button class="info-btn" id="btnTestInfo">?</button>
        </div>
        <div class="info-box" id="testInfoBox">
            <strong>🧪 Calibration Testing Guide</strong><br><br>
            Generates test files matching real ecology files.<br>
            Defaults: ~73 char lines + 10% emoji.<br>
            Numbered lines show running character count.<br><br>
            <strong>HOW TO CALIBRATE:</strong><br>
            1. Generate a test file at target size.<br>
            2. Paste into a FRESH instance.<br>
            3. Ask: "What is the exact last line?"<br>
            4. Binary search until exact limit found.<br>
            ⚠️ Use FRESH instances for EACH test.<br><br>
            <strong>KNOWN LIMITS (2026-05-27):</strong><br>
            DeepSeek paste: 163,823 ✅ · 163,873 ❌<br>
            URL fetch: ~65K (raw), ~75K (#readme).<br>
            Perplexity paste: 40,130. Cannot fetch URLs.<br><br>
            <strong>KEY DISCOVERY:</strong><br>
            Limit is about LINE COUNT, not characters.<br>
            Line density matters more than file size.
        </div>
        <input type="number" id="genSize" value="100000" min="100" max="1000000" placeholder="Enter size...">
        <div style="display:flex;gap:6px;margin-top:4px;align-items:center;flex-wrap:wrap;">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);">Lines:</span>
            <select id="lineDensity" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">none</option><option value="80">~80</option><option value="73" selected>~73</option><option value="40">~40</option><option value="20">~20</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customLineDensity" value="73" min="1" max="500" style="width:50px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="ch">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);margin-left:4px;">Emoji:</span>
            <select id="emojiPercent" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">0%</option><option value="5">5%</option><option value="10" selected>10%</option><option value="25">25%</option><option value="50">50%</option><option value="100">100%</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customEmojiPercent" value="10" min="0" max="100" style="width:45px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="%">
        </div>
        <button class="clear-input-btn" id="btnClearTestInput">clear</button>
        <div style="margin-top:6px;"><button id="btnToggleLog" style="font-size:11px;padding:4px 8px;min-height:auto;background:rgba(40,40,60,0.8);border:1px solid rgba(100,140,255,0.3);">📝 Log</button></div>
        <div class="log-panel" id="logPanel"><div id="logContent"></div></div>
    </div>

    <div class="version-hud">BUILD <span id="buildNumber"></span> • <span id="buildEmoji"></span></div>

    <script>
        function copyText(text, cb) {
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => { if (cb) cb(); }).catch(() => fallbackCopy(text, cb));
            } else { fallbackCopy(text, cb); }
        }
        function fallbackCopy(text, cb) {
            const ta = document.createElement('textarea'); ta.value = text;
            ta.style.position = 'fixed'; ta.style.left = '-9999px'; ta.style.top = '-9999px';
            document.body.appendChild(ta); ta.focus(); ta.select();
            try { if (document.execCommand('copy') && cb) cb(); } catch(e) {}
            document.body.removeChild(ta);
        }

        // ============ BUILD NUMBER ============
        (() => {
            const s = localStorage.getItem('ff_build');
            let b = s ? parseInt(s) + Math.floor(Math.random()*5) + 1 : Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_build', b);
            document.getElementById('buildNumber').textContent = b;
            const emojis = ['🎳','✨','🫧','💎','🧩','🪜','🔮','🖕','🌱','🔥','🧪','📋','📏','🎯','⭐'];
            const p = []; for (let i=0;i<4;i++) p.push(emojis[Math.floor(Math.random()*emojis.length)]);
            document.getElementById('buildEmoji').textContent = p.join('');
        })();

        // ============ STATE ============
        const DEPLOY_OVERHEAD = 400; // chars reserved for deploy header per chunk
        let mode = 'split', limit = 165000, isCustom = false, countDebounce = null;

        // ============ RESTORE SAVED LIMIT + ACTIVE BUTTON ============
        const savedLimit = localStorage.getItem('ff_limit');
        if (savedLimit) {
            limit = parseInt(savedLimit);
            // Restore active button after DOM ready
            document.addEventListener('DOMContentLoaded', () => restoreActivePreset());
        }

        function restoreActivePreset() {
            document.querySelectorAll('.preset-btn').forEach(b => {
                b.classList.remove('active');
                if (parseInt(b.dataset.limit) === limit) b.classList.add('active');
            });
        }

        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const customRow = document.getElementById('customRow');
        const customLimitInput = document.getElementById('customLimit');
        const countStats = document.getElementById('countStats');
        const btnAction = document.getElementById('btnAction');
        const emptyHint = document.getElementById('emptyHint');
        const splitOnly = document.getElementById('splitOnly');
        const fineTuneHint = document.getElementById('fineTuneHint');
        const baseFilename = document.getElementById('baseFilename');
        const filenamePreview = document.getElementById('filenamePreview');
        const deployOverheadNote = document.getElementById('deployOverheadNote');
        const outputMode = document.getElementById('outputMode');
        const deployFilename = document.getElementById('deployFilename');

        const modeBtns = { split: document.getElementById('modeSplit'), count: document.getElementById('modeCount'), gen: document.getElementById('modeGen') };
        const modeLabels = { split: '🎳 Split', count: '📏 Count', gen: '🧪 Test' };
        const actionLabels = { split: '🎳 Split It', gen: '🧪 Generate' };

        Object.keys(modeBtns).forEach(m => { modeBtns[m].addEventListener('click', () => switchMode(m)); });

        function switchMode(m) {
            mode = m; emptyHint.classList.remove('show');
            Object.keys(modeBtns).forEach(k => { modeBtns[k].classList.remove('active'); modeBtns[k].textContent = modeLabels[k]; });
            modeBtns[m].classList.add('active');
            panelOutput.style.display = 'none'; panelCount.style.display = 'none'; panelGen.style.display = 'none';
            if (m === 'split') { splitOnly.style.display = 'block'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; updateChunkInfo(); }
            else if (m === 'count') { splitOnly.style.display = 'none'; btnAction.style.display = 'none'; updateCount(); panelCount.style.display = 'block'; }
            else if (m === 'gen') { splitOnly.style.display = 'none'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; panelGen.style.display = 'block'; }
        }

        outputMode.addEventListener('change', function() {
            if (this.value === 'deploy') { deployFilename.classList.add('show'); }
            else { deployFilename.classList.remove('show'); }
            updateChunkInfo(); updateFilenamePreview();
        });

        const savedFn = localStorage.getItem('ff_filename');
        if (savedFn) baseFilename.value = savedFn;

        baseFilename.addEventListener('input', () => {
            updateFilenamePreview();
            localStorage.setItem('ff_filename', baseFilename.value);
        });

        function effectiveLimit() {
            return outputMode.value === 'deploy' ? limit - DEPLOY_OVERHEAD : limit;
        }

        function updateFilenamePreview() {
            const fn = baseFilename.value.trim() || 'FILE';
            const text = inputText.value.trim();
            const eff = effectiveLimit();
            if (text) {
                const n = Math.ceil(text.length / eff);
                let prev = '';
                for (let i = 1; i <= Math.min(n, 3); i++) prev += fn + '-SPLIT' + i + '.md ';
                if (n > 3) prev += '+ ' + (n-3) + ' more';
                filenamePreview.textContent = prev;
                deployOverheadNote.textContent = '~' + DEPLOY_OVERHEAD + ' chars/chunk reserved for header';
            } else {
                filenamePreview.textContent = 'Paste text to preview files';
                deployOverheadNote.textContent = '';
            }
        }

        document.getElementById('btnHeaderSettings').addEventListener('click', function() {
            const panel = document.getElementById('headerSettings');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '⚙️ Header ▲' : '⚙️ Header';
        });

        document.getElementById('btnSplitInfo').addEventListener('click', () => { document.getElementById('splitInfoBox').classList.toggle('show'); });
        document.getElementById('btnCountInfo').addEventListener('click', () => { document.getElementById('countInfoBox').classList.toggle('show'); });
        document.getElementById('btnTestInfo').addEventListener('click', () => { document.getElementById('testInfoBox').classList.toggle('show'); });

        document.getElementById('btnClearInput').addEventListener('click', () => { inputText.value = ''; updateChunkInfo(); if (mode === 'count') updateCount(); });
        document.getElementById('btnClearTestInput').addEventListener('click', () => { document.getElementById('genSize').value = ''; });

        document.getElementById('lineDensity').addEventListener('change', function() {
            document.getElementById('customLineDensity').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });
        document.getElementById('emojiPercent').addEventListener('change', function() {
            document.getElementById('customEmojiPercent').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });

        document.getElementById('btnToggleLog').addEventListener('click', function() {
            const panel = document.getElementById('logPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '📝 Log ▲' : '📝 Log';
        });
        function addLog(msg, type) {
            const lc = document.getElementById('logContent');
            const entry = document.createElement('div');
            entry.className = 'log-entry' + (type ? ' ' + type : '');
            const time = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit', second:'2-digit'});
            entry.innerHTML = '<span style="color:#aaa;">[' + time + ']</span> ' + msg;
            lc.appendChild(entry); lc.scrollTop = lc.scrollHeight;
            document.getElementById('logPanel').style.display = 'block';
            document.getElementById('btnToggleLog').textContent = '📝 Log ▲';
        }
        function clearLog() { document.getElementById('logContent').innerHTML = ''; }

        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const l = this.dataset.limit;
                if (l === 'custom') { isCustom = true; customRow.classList.add('show'); customLimitInput.value = limit; customLimitInput.focus(); }
                else { isCustom = false; customRow.classList.remove('show'); limit = parseInt(l); fineTuneHint.style.display = 'none'; localStorage.setItem('ff_limit', limit); }
                updateChunkInfo(); updateFilenamePreview();
            });
        });
        customLimitInput.addEventListener('input', function() {
            if (isCustom) { limit = parseInt(this.value) || 165000; updateChunkInfo(); updateFilenamePreview(); localStorage.setItem('ff_limit', limit); }
        });

        function getLineDensity(text) {
            const lines = text.split('\n');
            if (lines.length < 2) return 0;
            return Math.round(text.length / lines.length);
        }

        function updateChunkInfo() {
            const text = inputText.value.trim();
            const info = document.getElementById('chunkInfo');
            if (text) {
                const eff = effectiveLimit();
                const n = Math.ceil(text.length / eff);
                const density = getLineDensity(text);
                const lines = text.split('\n').length;
                let dn = '', dc = '';
                if (density > 60) { dn = '✅ Safe'; dc = 'density-safe'; }
                else if (density >= 40) { dn = '⚠️ Use 165.1K or lower'; dc = 'density-warn'; }
                else if (density > 0) { dn = '⚠️ Use 163.8K or lower'; dc = 'density-risk'; }
                const modeNote = outputMode.value === 'deploy' ? ' (deploy)' : '';
                info.innerHTML = 'File: ' + text.length.toLocaleString() + ' chars · ' + lines.toLocaleString() + ' lines · ~' + density + ' chars/line → ' + n + ' chunks' + modeNote + ' <span class="' + dc + '">' + dn + '</span>';
            } else { info.textContent = 'Paste text to see chunk count + line density'; }
        }

        inputText.addEventListener('input', () => {
            if (mode === 'split') { updateChunkInfo(); updateFilenamePreview(); }
            if (mode === 'count') { if (countDebounce) clearTimeout(countDebounce); countDebounce = setTimeout(updateCount, 300); }
        });

        btnAction.addEventListener('click', () => {
            if (mode === 'split') {
                if (!inputText.value.trim()) { emptyHint.classList.add('show'); setTimeout(() => emptyHint.classList.remove('show'), 2500); return; }
                emptyHint.classList.remove('show'); splitIt();
            } else if (mode === 'gen') { generateTestFile(); }
        });

        function flashButton(text) { const o = btnAction.textContent; btnAction.textContent = text; setTimeout(() => { btnAction.textContent = o; }, 1500); }

        function splitIt() {
            const text = inputText.value.trim(), total = text.length;
            const fn = baseFilename.value.trim() || 'FILE';
            const isDeploy = outputMode.value === 'deploy';
            const eff = effectiveLimit();
            const num = Math.ceil(total / eff);
            const anchor = Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_filename', fn);
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = num + ' chunks • ' + total.toLocaleString() + ' total chars';

            for (let i = 0; i < num; i++) {
                const start = i * eff;
                let end = Math.min(start + eff, total);
                // Walk back to nearest line boundary
                if (end < total) { const lastNewline = text.lastIndexOf('\n', end); if (lastNewline > start) end = lastNewline + 1; }

                const chunkNum = i + 1, isLast = (i === num - 1);
                const splitName = fn + '-SPLIT' + chunkNum;
                let output;

                if (isDeploy) {
                    const baseUrl = document.getElementById('baseUrl').value.trim() || 'source-sepia-alpha.vercel.app';
                    const folder = document.getElementById('folderPath').value.trim() || 'SPLIT';
                    const srcPath = document.getElementById('sourcePath').value.trim() || 'BUILDER';
                    const titleFmt = document.getElementById('titleFormat').value.trim() || '## 🏠 [FILE] — Part [N] of [TOTAL]';

                    // FIX: Nav links — no self-reference on chunk 1
                    let navLinks = '';
                    if (num === 1) {
                        navLinks = splitName + '.md · 1 of 1';
                    } else if (chunkNum === 1) {
                        navLinks = splitName + '.md · 1 of ' + num + ' · Next: ' + fn + '-SPLIT2.md →';
                    } else if (isLast) {
                        navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · END';
                    } else {
                        navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · Next: ' + fn + '-SPLIT' + (chunkNum+1) + '.md →';
                    }

                    const title = titleFmt.replace('[FILE]', fn).replace('[N]', chunkNum).replace('[TOTAL]', num);
                    output = '🔗 📋 ' + splitName + '.md\n';
                    output += 'https://' + baseUrl + '/' + folder + '/' + splitName + '.md\n\n';
                    output += '🔗 REVisions. Load both, see the full picture.\n';
                    output += '→ Current: ' + fn + '.md\n';
                    output += 'https://' + baseUrl + '/' + srcPath + '/' + fn + '.md\n\n';
                    output += title + '\n\n';
                    output += navLinks + '\n\n';
                    output += text.substring(start, end);
                    output += '\n\n---\n';
                    output += navLinks + '\n';
                    output += '♦️' + splitName + '-' + anchor + '\n';
                } else {
                    const label = limit >= 100000 ? 'PASTE' : (limit >= 70000 ? 'REPO' : 'FETCH');
                    output = '🎳 ' + chunkNum + '/' + num + ' ' + label + '\n' + text.substring(start, end);
                    if (!isLast) output += '\n---';
                    if (isLast) output += '\nCHUNK-ANCHOR: ' + anchor;
                }

                const div = document.createElement('div'); div.className = 'chunk-box';
                const labelText = isDeploy ? (splitName + '.md') : ('Chunk ' + chunkNum + '/' + num);
                div.innerHTML = '<div class="chunk-label"><span>' + labelText + ' • ' + output.length.toLocaleString() + ' chars' + (isLast ? ' • FINAL' : '') + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="8" readonly>' + output + '</textarea><div class="copied-flash">✅ Copied!</div>';
                div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(output, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
                div.querySelector('textarea').addEventListener('click', function() { this.select(); });
                chunksContainer.appendChild(div);
            }
            fineTuneHint.style.display = 'block';
            customLimitInput.value = limit;
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const all = [...chunksContainer.querySelectorAll('textarea')].map(ta => ta.value).join('\n\n');
            copyText(all, () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy All'; }, 1500); });
        });
        document.getElementById('btnClear').addEventListener('click', () => { chunksContainer.innerHTML = ''; panelOutput.style.display = 'none'; fineTuneHint.style.display = 'none'; });

        document.getElementById('btnCopyStats').addEventListener('click', function() {
            const t = inputText.value;
            copyText('Chars: ' + t.length.toLocaleString() + ' | Lines: ' + t.split('\n').length.toLocaleString() + ' | Density: ~' + getLineDensity(t) + ' chars/line | Limit: ' + limit.toLocaleString(), () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy Stats'; }, 1200); });
        });

        function updateCount() {
            const t = inputText.value, sec = (t.match(/^##\s+/gm) || []).length, density = getLineDensity(t);
            countStats.innerHTML = '<span>📏 <strong>' + t.length.toLocaleString() + '</strong> chars</span><span>📄 <strong>' + t.split('\n').length.toLocaleString() + '</strong> lines</span><span>📐 ~<strong>' + density + '</strong> chars/line</span>' + (sec > 0 ? '<span>📑 <strong>' + sec + '</strong> sections</span>' : '');
            panelCount.style.display = 'block';
        }

        function generateTestFile() {
            const size = parseInt(document.getElementById('genSize').value);
            if (!size || size < 100) return;
            clearLog();
            const densityVal = document.getElementById('lineDensity').value;
            let lineDensity = densityVal === 'custom' ? parseInt(document.getElementById('customLineDensity').value) || 73 : parseInt(densityVal) || 73;
            const emojiVal = document.getElementById('emojiPercent').value;
            let emojiPct = emojiVal === 'custom' ? parseInt(document.getElementById('customEmojiPercent').value) || 10 : parseInt(emojiVal) || 10;
            emojiPct = emojiPct / 100;
            addLog('🎯 Target: ' + size.toLocaleString() + ' chars');
            addLog('📏 Lines: ~' + lineDensity + ' chars, emoji: ' + Math.round(emojiPct*100) + '%');
            const anchor = Math.floor(10000 + Math.random()*90000);
            let content = 'TEST ' + size.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            if (lineDensity === 0) content += ' • no breaks'; else content += ' • ~' + lineDensity + ' char lines';
            if (emojiPct > 0) content += ' • ' + Math.round(emojiPct*100) + '% emoji';
            content += '\n';
            const emojis = ['🎳','🫧','💎','🧩','🪜','🔮','🖕','🔥','🌱','✨','🧪','📋','📏','🎯','⭐','◾'];
            let ei = 0, ln = 1;
            while (content.length < size - 30) {
                if (Math.random() < emojiPct) { content += emojis[ei % emojis.length]; ei++; }
                else if (lineDensity === 0) { content += 'X'; }
                else {
                    const runningTotal = content.length;
                    const totalStr = runningTotal >= 1000 ? '~' + Math.round(runningTotal/1000) + 'K' : String(runningTotal);
                    const prefix = 'L' + String(ln).padStart(6,'0') + ' · ' + String(totalStr).padStart(6,' ') + ' ';
                    const filler = 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                    const maxLine = prefix.length + lineDensity;
                    let line = prefix, cc = prefix.length;
                    for (let c = 0; c < filler.length && cc < maxLine; c++) { line += filler[c]; cc++; }
                    if (content.length + line.length + 30 > size) break;
                    content += line + '\n'; ln++;
                }
            }
            while (content.length < size - 15) content += 'X';
            content = content.substring(0, size);
            const al = '\nCHUNK-ANCHOR: ' + anchor, ep = size - al.length;
            if (ep > 0) content = content.substring(0, ep) + al;
            const diff = content.length - size;
            addLog('📊 Created: ' + content.length.toLocaleString() + ' chars (diff: ' + (diff >= 0 ? '+' : '') + diff + ')', diff === 0 ? 'success' : 'error');
            if (diff === 0) addLog('✅ Exact match', 'success');
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = 'Test File • ' + content.length.toLocaleString() + ' chars';
            const div = document.createElement('div'); div.className = 'chunk-box';
            div.innerHTML = '<div class="chunk-label"><span>Test File • ' + content.length.toLocaleString() + ' chars</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="10" readonly>' + content + '</textarea><div class="copied-flash">✅ Copied!</div>';
            div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(content, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
            chunksContainer.appendChild(div);
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        // ============ INIT ============
        restoreActivePreset();
        updateChunkInfo();
        switchMode('split');
    </script>
</body>
</html>

---


THE DEFINITIVE FRAGMENTOR. v2.2 FINAL.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #020208;
            background-image:
                radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.06) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.04) 0%, transparent 50%);
            color: #f0f0f0; margin: 0; padding: 6px;
            max-width: 600px; margin: auto; min-height: 100vh;
            position: relative; overflow-x: hidden;
        }
        .panel { background: rgba(15,15,30,0.75); border: 1px solid rgba(100,140,255,0.2); border-radius: 10px; padding: 10px 12px; margin-bottom: 6px; position: relative; z-index: 1; }
        textarea, input, select { width: 100%; background: rgba(30,30,50,0.85); color: #e0e0ff; border: 1px solid rgba(100,140,255,0.25); border-radius: 8px; padding: 10px; font-size: 16px; margin-bottom: 6px; -webkit-appearance: none; }
        textarea { text-align: left; }
        textarea::placeholder { text-align: center; }
        textarea:focus, input:focus, select:focus { outline: 2px solid #ffaa33; }
        button { background: linear-gradient(135deg, rgba(40,80,180,0.75), rgba(20,40,120,0.85)); color: white; border: 1px solid rgba(100,140,255,0.3); padding: 12px 16px; border-radius: 10px; font-weight: bold; cursor: pointer; margin: 3px 2px; font-size: 15px; min-height: 46px; transition: all 0.2s ease; position: relative; z-index: 1; }
        button:active { transform: scale(0.96); box-shadow: 0 0 18px rgba(100,140,255,0.4); }
        .big-btn { display: block; width: 100%; margin: 6px 0; font-size: 20px; background: linear-gradient(135deg, rgba(40,80,200,0.8), rgba(20,40,120,0.9)); border: 2px solid rgba(100,160,255,0.5); letter-spacing: 1px; }
        .copy-all-btn { background: linear-gradient(135deg, rgba(255,170,51,0.7), rgba(200,130,20,0.8)); border: 2px solid rgba(255,170,51,0.6); animation: glow-pulse 2s ease-in-out infinite; }
        @keyframes glow-pulse { 0%,100% { box-shadow: 0 0 8px rgba(255,170,51,0.3); } 50% { box-shadow: 0 0 18px rgba(255,170,51,0.5); } }
        .preset-btn { background: rgba(40,40,60,0.8); color: #aaddff; font-size: 13px; padding: 7px 11px; border: 2px solid rgba(100,140,255,0.3); }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; box-shadow: 0 0 15px rgba(255,170,51,0.3); }
        .chunk-box { background: rgba(20,20,40,0.6); border-left: 3px solid rgba(255,170,51,0.6); margin: 10px 0; border-radius: 5px; position: relative; z-index: 1; }
        .chunk-label { background: rgba(40,40,60,0.8); padding: 6px 10px; border-radius: 4px 4px 0 0; font-family: monospace; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .chunk-copy-btn { background: rgba(255,170,51,0.3); color: #ffaa33; border: 1px solid rgba(255,170,51,0.5); padding: 4px 10px; border-radius: 6px; font-size: 12px; min-height: auto; cursor: pointer; font-weight: bold; }
        .chunk-copy-btn:active { background: #ffaa33; color: #0f0f1a; }
        .chunk-box textarea { margin-bottom: 0; border-radius: 0 0 4px 4px; border: none; background: rgba(20,20,40,0.8); color: #c0c0dd; font-size: 13px; }
        .copied-flash { position: absolute; top: 8px; right: 55px; color: #33ff88; font-weight: bold; font-size: 13px; pointer-events: none; opacity: 0; transition: opacity 0.3s; z-index: 2; }
        .copied-flash.show { opacity: 1; }
        .info-row { display: flex; gap: 6px; flex-wrap: wrap; margin: 6px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(30,30,50,0.7); padding: 5px 8px; border-radius: 5px; border: 1px solid rgba(100,140,255,0.2); }
        .mode-toggle { display: flex; gap: 3px; margin-bottom: 6px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(40,40,60,0.8); font-size: 13px; padding: 8px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-row { display: none; } .custom-row.show { display: block; }
        .deploy-only { display: none; } .deploy-only.show { display: block; }
        .version-hud { position: fixed; bottom: 8px; right: 10px; font-family: monospace; font-size: 10px; color: rgba(150,180,255,0.4); z-index: 2; pointer-events: none; }
        .empty-hint { text-align: center; color: rgba(255,170,51,0.6); font-size: 13px; margin: 6px 0; display: none; position: relative; z-index: 1; }
        .empty-hint.show { display: block; }
        .info-box { display:none; font-size:11px; color:rgba(150,180,255,0.6); margin:4px 0; padding:6px 8px; background:rgba(15,15,30,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.15); line-height:1.4; }
        .info-box.show { display:block; }
        .info-box strong { color: #ffaa33; }
        .clear-input-btn { background:transparent; border:none; color:rgba(255,255,255,0.3); font-size:11px; padding:2px 6px; min-height:auto; float:right; margin-top:-4px; }
        .info-btn { background:transparent; border:1px solid rgba(255,255,255,0.2); color:rgba(255,255,255,0.4); font-size:12px; padding:1px 6px; min-height:auto; border-radius:50%; cursor:pointer; }
        .row-label { font-size:10px; min-width:42px; }
        .ref-text { font-size:9px; }
        .fine-tune-hint { font-size:11px; color:rgba(255,170,51,0.5); margin-top:2px; display:none; }
        .log-panel { margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.2); max-height:180px; overflow-y:auto; font-family:monospace; font-size:11px; line-height:1.4; display:none; }
        .log-entry { padding:2px 4px; margin:1px 0; border-radius:3px; background:rgba(255,255,255,0.03); }
        .log-entry.success { background:rgba(0,255,136,0.1); }
        .log-entry.error { background:rgba(255,51,102,0.1); }
        .density-safe { color:#33ff88; } .density-warn { color:#ffaa33; } .density-risk { color:#ff5555; }
    </style>
</head>
<body>

    <div class="mode-toggle">
        <button id="modeSplit" class="active">🎳 Split</button>
        <button id="modeCount">📏 Count</button>
        <button id="modeGen">🧪 Test</button>
    </div>

    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="8" placeholder="👆 Tap here and paste..."></textarea>
        <button class="clear-input-btn" id="btnClearInput">clear input</button>
        <div class="empty-hint" id="emptyHint">👆 Paste text first, then split</div>

        <div id="splitOnly">
            <div style="display:flex;gap:6px;align-items:center;margin-bottom:4px;">
                <span style="font-size:11px;color:rgba(150,180,255,0.5);">Mode:</span>
                <select id="outputMode" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                    <option value="raw" selected>Raw</option>
                    <option value="deploy">Deploy</option>
                </select>
            </div>

            <div class="deploy-only" id="deployFilename">
                <div style="display:flex;align-items:center;gap:4px;margin-bottom:4px;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.5);white-space:nowrap;">Name:</span>
                    <input type="text" id="baseFilename" value="STANDARDS" placeholder="filename" style="flex:1;margin-bottom:0;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.4);">.md</span>
                </div>
                <div style="font-size:10px;color:rgba(150,180,255,0.4);margin-bottom:4px;" id="filenamePreview"></div>
                
                <!-- HEADER SETTINGS -->
                <div style="margin-top:2px;">
                    <button id="btnHeaderSettings" style="font-size:10px;padding:3px 6px;min-height:auto;background:rgba(40,40,60,0.6);border:1px solid rgba(100,140,255,0.2);">⚙️ Header</button>
                </div>
                <div id="headerSettings" style="display:none;margin-top:4px;padding:6px 8px;background:rgba(0,0,0,0.3);border-radius:6px;border:1px solid rgba(100,140,255,0.15);">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Base URL:</div>
                    <input type="text" id="baseUrl" value="source-sepia-alpha.vercel.app" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Folder:</div>
                    <input type="text" id="folderPath" value="SPLIT" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Source path:</div>
                    <input type="text" id="sourcePath" value="BUILDER" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Title:</div>
                    <input type="text" id="titleFormat" value="## 🏠 [FILE] — Part [N] of [TOTAL]" style="font-size:12px;padding:4px 6px;">
                </div>
            </div>

            <div style="margin:4px 0;">
                <div style="display:flex;justify-content:space-between;align-items:center;">
                    <strong style="font-size:13px;">Chunk size:</strong>
                    <button class="info-btn" id="btnSplitInfo">?</button>
                </div>
                <div class="info-box" id="splitInfoBox">
                    <strong>🎳 Chunk Size Guide</strong><br>
                    <br>
                    <strong>DEEPSEEK PASTE (Raw mode):</strong><br>
                    168K — high. Light files, few line breaks.<br>
                    165.1K — mid. Works for most files.<br>
                    163.8K — low. Safe default. Works for all.<br>
                    <strong>STEP-DOWN:</strong> If chunk fails, reduce by 1K.<br>
                    <br>
                    <strong>URL FETCH:</strong><br>
                    75K — GitHub #readme anchor (~10K more than raw).<br>
                    65K — standard raw URL (Vercel, GitHub raw).<br>
                    40K — universal (all platforms incl. Perplexity).<br>
                    <br>
                    <strong>RAW vs DEPLOY:</strong><br>
                    Raw = minimal chunks for testing size.<br>
                    Deploy = full .md files for Vercel upload.<br>
                    Deploy adds ~300 chars header per chunk.<br>
                    <br>
                    <strong>ANCHORS:</strong><br>
                    CHUNK-ANCHOR verifies split delivery.<br>
                    TH-CHECK (♦️) verifies original file integrity.<br>
                    Different purposes. Both matter.<br>
                    <br>
                    <strong>KEY DISCOVERIES:</strong><br>
                    • Limit depends on LINE COUNT, not characters.<br>
                    • 280K emojis on one line ✅<br>
                    • 252K text with 3,460 lines ❌ (54% over)<br>
                    • Each line break costs overhead in chat input.<br>
                    • Calibrated: 163,823 ✅ · 163,873 ❌ (2026-05-27)<br>
                    <br>
                    <strong>PLATFORMS:</strong><br>
                    Claude & Grok: full file access. No tool needed.<br>
                    DeepSeek: use Fragmentor. Perplexity: paste only.<br>
                    ⚠️ Re-fetching URLs may return cached results.<br>
                    ⚠️ Use fresh instances for calibration testing.
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:6px;">
                    <span class="row-label" style="color:#ffaa33;">🟠 Paste</span>
                    <button class="preset-btn" data-limit="168000">168K</button>
                    <span class="ref-text" style="color:#ff5555;">high</span>
                    <button class="preset-btn active" data-limit="165000">165.1K</button>
                    <span class="ref-text" style="color:#ffaa33;">mid</span>
                    <button class="preset-btn" data-limit="163673">163.8K</button>
                    <span class="ref-text" style="color:#33ff88;">low</span>
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:4px;">
                    <span class="row-label" style="color:#88aaff;">🔵 Fetch</span>
                    <button class="preset-btn" data-limit="75000">75K</button>
                    <button class="preset-btn" data-limit="65000">65K</button>
                    <button class="preset-btn" data-limit="40000">40K</button>
                    <button class="preset-btn" data-limit="custom" id="btnCustom">✎</button>
                </div>
                
                <div class="custom-row" id="customRow">
                    <input type="number" id="customLimit" value="65000" placeholder="Custom size...">
                </div>
                <div style="font-size:13px;color:#ffaa33;margin-top:4px;font-weight:bold;" id="chunkInfo"></div>
                <div class="fine-tune-hint" id="fineTuneHint">Fine-tune: tap ✎ to set exact limit</div>
            </div>
        </div>

        <button class="big-btn" id="btnAction">🎳 Split It</button>
    </div>

    <div class="panel" id="panelOutput" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
            <strong id="outputLabel">Chunks</strong>
            <div>
                <button id="btnCopyAll" class="copy-all-btn" style="font-size:14px;">📋 Copy All</button>
                <button id="btnClear" style="background:transparent;border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.4);font-size:12px;padding:4px 10px;min-height:auto;margin-left:8px;">✕ Clear</button>
            </div>
        </div>
        <div id="chunksContainer"></div>
    </div>

    <div class="panel" id="panelCount" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <div style="display:flex;align-items:center;gap:6px;">
                <strong style="font-size:13px;">Stats</strong>
                <button class="info-btn" id="btnCountInfo">?</button>
            </div>
            <button id="btnCopyStats" style="font-size:12px;padding:4px 10px;min-height:auto;">📋 Copy Stats</button>
        </div>
        <div class="info-box" id="countInfoBox">
            Live character, line, and section count.<br>
            Copy Stats sends numbers to clipboard.<br>
            <br>
            <strong>LINE DENSITY GUIDE:</strong><br>
            Under 1,000 lines → try 168K or 165.1K.<br>
            1,000-3,000 lines → use 163.8K (safe).<br>
            Over 3,000 lines → use 131K or URL fetch.<br>
            <br>
            <strong>WHY THIS MATTERS:</strong><br>
            The paste limit depends on LINE COUNT,<br>
            not character count. STANDARDS.md has<br>
            3,460 lines and failed at 165K even though<br>
            lighter files work at that size.<br>
            <br>
            <strong>DENSITY:</strong><br>
            Over 60 chars/line → safe for all presets.<br>
            Under 40 chars/line → use lower presets.
        </div>
        <div class="info-row" id="countStats"></div>
    </div>

    <div class="panel" id="panelGen" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <label style="font-size:14px;">Target size (characters):</label>
            <button class="info-btn" id="btnTestInfo">?</button>
        </div>
        <div class="info-box" id="testInfoBox">
            <strong>🧪 Calibration Testing Guide</strong><br>
            <br>
            Generates test files matching real ecology files.<br>
            Defaults: ~73 char lines (STANDARDS.md avg) + 10% emoji.<br>
            Numbered lines show running character count.<br>
            The last line tells you exactly how far the paste reached.<br>
            <br>
            <strong>HOW TO CALIBRATE ANY PLATFORM:</strong><br>
            1. Generate a test file at target size.<br>
            2. Copy and paste into a FRESH instance.<br>
            3. Ask: "What is the exact last line?"<br>
            4. If truncated → reduce size. If complete → increase.<br>
            5. Binary search until exact limit found.<br>
            ⚠️ Use FRESH instances for EACH test.<br>
            ⚠️ Repeated testing can overwhelm instances.<br>
            <br>
            <strong>LINE DENSITY:</strong><br>
            ~73 chars = STANDARDS.md average (default).<br>
            ~20 chars = very heavy line breaks.<br>
            None = one continuous line (emoji test).<br>
            ✎ custom = any line length.<br>
            <br>
            <strong>EMOJI %:</strong><br>
            10% = realistic ecology mix (default).<br>
            0% = pure text. 100% = worst-case encoding.<br>
            ✎ custom = any percentage.<br>
            <br>
            <strong>KNOWN LIMITS (2026-05-27):</strong><br>
            DeepSeek paste: 163,823 ✅ · 163,873 ❌<br>
            URL fetch: ~65K (raw), ~75K (#readme).<br>
            Perplexity paste: 40,130. Cannot fetch URLs.<br>
            GitHub raw: ~65K. GitHub #readme: ~75K.<br>
            <br>
            <strong>KEY DISCOVERY:</strong><br>
            • Limit is about LINE COUNT, not characters.<br>
            • 280K emojis on one line ✅<br>
            • 252K text with 3,460 lines ❌<br>
            • Line density matters more than file size.<br>
            <br>
            <strong>STEP-DOWN SYSTEM:</strong><br>
            Start at 165K. If rejected, reduce by 1K.<br>
            Continue until accepted. That's your limit<br>
            for THAT specific file type and line density.<br>
            <br>
            <strong>📝 LOG:</strong> Expand to see generation math.<br>
            Shows target, line count, adjustments, verification.
        </div>
        <input type="number" id="genSize" value="1000" min="100" max="1000000" placeholder="Enter size...">
        <div style="display:flex;gap:6px;margin-top:4px;align-items:center;flex-wrap:wrap;">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);">Lines:</span>
            <select id="lineDensity" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">none</option><option value="80">~80</option><option value="73" selected>~73</option><option value="40">~40</option><option value="20">~20</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customLineDensity" value="73" min="1" max="500" style="width:50px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="ch">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);margin-left:4px;">Emoji:</span>
            <select id="emojiPercent" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">0%</option><option value="5">5%</option><option value="10" selected>10%</option><option value="25">25%</option><option value="50">50%</option><option value="100">100%</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customEmojiPercent" value="10" min="0" max="100" style="width:45px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="%">
        </div>
        <button class="clear-input-btn" id="btnClearTestInput">clear</button>
        <div style="margin-top:6px;"><button id="btnToggleLog" style="font-size:11px;padding:4px 8px;min-height:auto;background:rgba(40,40,60,0.8);border:1px solid rgba(100,140,255,0.3);">📝 Log</button></div>
        <div class="log-panel" id="logPanel"><div id="logContent"></div></div>
    </div>

    <div class="version-hud">BUILD <span id="buildNumber"></span> • <span id="buildEmoji"></span></div>

    <script>
        function copyText(text, cb) {
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => { if (cb) cb(); }).catch(() => fallbackCopy(text, cb));
            } else { fallbackCopy(text, cb); }
        }
        function fallbackCopy(text, cb) {
            const ta = document.createElement('textarea'); ta.value = text;
            ta.style.position = 'fixed'; ta.style.left = '-9999px'; ta.style.top = '-9999px';
            document.body.appendChild(ta); ta.focus(); ta.select();
            try { if (document.execCommand('copy') && cb) cb(); } catch(e) {}
            document.body.removeChild(ta);
        }

        (() => {
            const s = localStorage.getItem('ff_build');
            let b = s ? parseInt(s) + Math.floor(Math.random()*5) + 1 : Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_build', b);
            document.getElementById('buildNumber').textContent = b;
            const emojis = ['🎳','✨','🫧','💎','🧩','🪜','🔮','🖕','🌱','🔥','🧪','📋','📏','🎯','⭐'];
            const p = []; for (let i=0;i<4;i++) p.push(emojis[Math.floor(Math.random()*emojis.length)]);
            document.getElementById('buildEmoji').textContent = p.join('');
        })();

        let mode = 'split', limit = 165000, isCustom = false, countDebounce = null;
        const savedLimit = localStorage.getItem('ff_limit');
        if (savedLimit) limit = parseInt(savedLimit);

        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const customRow = document.getElementById('customRow');
        const customLimitInput = document.getElementById('customLimit');
        const countStats = document.getElementById('countStats');
        const btnAction = document.getElementById('btnAction');
        const emptyHint = document.getElementById('emptyHint');
        const splitOnly = document.getElementById('splitOnly');
        const fineTuneHint = document.getElementById('fineTuneHint');
        const baseFilename = document.getElementById('baseFilename');
        const filenamePreview = document.getElementById('filenamePreview');
        const outputMode = document.getElementById('outputMode');
        const deployFilename = document.getElementById('deployFilename');

        const modeBtns = { split: document.getElementById('modeSplit'), count: document.getElementById('modeCount'), gen: document.getElementById('modeGen') };
        const modeLabels = { split: '🎳 Split', count: '📏 Count', gen: '🧪 Test' };
        const actionLabels = { split: '🎳 Split It', count: '', gen: '🧪 Generate' };

        Object.keys(modeBtns).forEach(m => { modeBtns[m].addEventListener('click', () => switchMode(m)); });

        function switchMode(m) {
            mode = m; emptyHint.classList.remove('show');
            Object.keys(modeBtns).forEach(k => { modeBtns[k].classList.remove('active'); modeBtns[k].textContent = modeLabels[k]; });
            modeBtns[m].classList.add('active');
            panelOutput.style.display = 'none'; panelCount.style.display = 'none'; panelGen.style.display = 'none';
            if (m === 'split') { splitOnly.style.display = 'block'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; updateChunkInfo(); }
            else if (m === 'count') { splitOnly.style.display = 'none'; btnAction.style.display = 'none'; updateCount(); panelCount.style.display = 'block'; }
            else if (m === 'gen') { splitOnly.style.display = 'none'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; panelGen.style.display = 'block'; }
        }

        outputMode.addEventListener('change', function() {
            if (this.value === 'deploy') { deployFilename.classList.add('show'); updateFilenamePreview(); }
            else { deployFilename.classList.remove('show'); }
        });

        const savedFn = localStorage.getItem('ff_filename');
        if (savedFn) baseFilename.value = savedFn;

        baseFilename.addEventListener('input', () => { 
            updateFilenamePreview();
            localStorage.setItem('ff_filename', baseFilename.value);
        });

        function updateFilenamePreview() {
            const fn = baseFilename.value.trim() || 'FILE';
            const text = inputText.value.trim();
            if (text) {
                const n = Math.ceil(text.length / limit);
                let prev = '';
                for (let i = 1; i <= Math.min(n, 3); i++) prev += fn + '-SPLIT' + i + '.md ';
                if (n > 3) prev += '+ ' + (n-3) + ' more';
                filenamePreview.textContent = prev;
            } else {
                filenamePreview.textContent = 'Paste text to preview files';
            }
        }

        // ============ HEADER SETTINGS TOGGLE ============
        document.getElementById('btnHeaderSettings').addEventListener('click', function() {
            const panel = document.getElementById('headerSettings');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '⚙️ Header ▲' : '⚙️ Header';
        });

        document.getElementById('btnSplitInfo').addEventListener('click', () => { document.getElementById('splitInfoBox').classList.toggle('show'); });
        document.getElementById('btnCountInfo').addEventListener('click', () => { document.getElementById('countInfoBox').classList.toggle('show'); });
        document.getElementById('btnTestInfo').addEventListener('click', () => { document.getElementById('testInfoBox').classList.toggle('show'); });

        document.getElementById('btnClearInput').addEventListener('click', () => { inputText.value = ''; updateChunkInfo(); if (mode === 'count') updateCount(); });
        document.getElementById('btnClearTestInput').addEventListener('click', () => { document.getElementById('genSize').value = ''; });

        document.getElementById('lineDensity').addEventListener('change', function() {
            document.getElementById('customLineDensity').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });
        document.getElementById('emojiPercent').addEventListener('change', function() {
            document.getElementById('customEmojiPercent').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });

        // ============ LOG PANEL ============
        document.getElementById('btnToggleLog').addEventListener('click', function() {
            const panel = document.getElementById('logPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '📝 Log ▲' : '📝 Log';
        });
        function addLog(msg, type) {
            const lc = document.getElementById('logContent');
            const entry = document.createElement('div');
            entry.className = 'log-entry' + (type ? ' ' + type : '');
            const time = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit', second:'2-digit'});
            entry.innerHTML = '<span style="color:#aaa;">[' + time + ']</span> ' + msg;
            lc.appendChild(entry); lc.scrollTop = lc.scrollHeight;
            document.getElementById('logPanel').style.display = 'block';
            document.getElementById('btnToggleLog').textContent = '📝 Log ▲';
        }
        function clearLog() { document.getElementById('logContent').innerHTML = ''; }

        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const l = this.dataset.limit;
                if (l === 'custom') { isCustom = true; customRow.classList.add('show'); customLimitInput.value = limit; customLimitInput.focus(); }
                else { isCustom = false; customRow.classList.remove('show'); limit = parseInt(l); fineTuneHint.style.display = 'none'; }
                updateChunkInfo(); updateFilenamePreview();
                localStorage.setItem('ff_limit', limit);
            });
        });
        customLimitInput.addEventListener('input', function() { if (isCustom) { limit = parseInt(this.value) || 165000; updateChunkInfo(); updateFilenamePreview(); localStorage.setItem('ff_limit', limit); } });

        function getLineDensity(text) {
            const lines = text.split('\n');
            if (lines.length < 2) return 0;
            return Math.round(text.length / lines.length);
        }

        function updateChunkInfo() {
            const text = inputText.value.trim();
            const info = document.getElementById('chunkInfo');
            if (text) {
                const n = Math.ceil(text.length / limit);
                const density = getLineDensity(text);
                const lines = text.split('\n').length;
                let dn = '', dc = '';
                if (density > 60) { dn = '✅ Safe'; dc = 'density-safe'; }
                else if (density >= 40) { dn = '⚠️ Use 165.1K or lower'; dc = 'density-warn'; }
                else if (density > 0) { dn = '⚠️ Use 163.8K or lower'; dc = 'density-risk'; }
                info.innerHTML = 'File: ' + text.length.toLocaleString() + ' chars · ' + lines.toLocaleString() + ' lines · ~' + density + ' chars/line → ' + n + ' chunks of ' + limit.toLocaleString() + ' chars <span class="' + dc + '">' + dn + '</span>';
            } else { info.textContent = 'Paste text to see chunk count + line density'; }
        }

        inputText.addEventListener('input', () => {
            if (mode === 'split') { updateChunkInfo(); updateFilenamePreview(); }
            if (mode === 'count') { if (countDebounce) clearTimeout(countDebounce); countDebounce = setTimeout(updateCount, 300); }
        });

        btnAction.addEventListener('click', () => {
            if (mode === 'split') { if (!inputText.value.trim()) { emptyHint.classList.add('show'); setTimeout(() => emptyHint.classList.remove('show'), 2500); return; } emptyHint.classList.remove('show'); splitIt(); }
            else if (mode === 'gen') { generateTestFile(); }
        });

        function flashButton(text) { const o = btnAction.textContent; btnAction.textContent = text; setTimeout(() => { btnAction.textContent = o; }, 1500); }

        function splitIt() {
            const text = inputText.value.trim(), total = text.length, num = Math.ceil(total / limit);
            const fn = baseFilename.value.trim() || 'FILE';
            const isDeploy = outputMode.value === 'deploy';
            const anchor = Math.floor(10000 + Math.random()*90000);
            
            localStorage.setItem('ff_filename', fn);
            
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = num + ' chunks • ' + total.toLocaleString() + ' total chars';
            
            for (let i = 0; i < num; i++) {
                const start = i * limit;
                let end = Math.min(start + limit, total);
                if (end < total) { const lastNewline = text.lastIndexOf('\n', end); if (lastNewline > start) end = lastNewline + 1; }
                
                const chunkNum = i + 1, isLast = (i === num - 1);
                const splitName = fn + '-SPLIT' + chunkNum;
                
                let output;
                if (isDeploy) {
                    const baseUrl = document.getElementById('baseUrl').value.trim() || 'source-sepia-alpha.vercel.app';
                    const folder = document.getElementById('folderPath').value.trim() || 'SPLIT';
                    const srcPath = document.getElementById('sourcePath').value.trim() || 'BUILDER';
                    const titleFmt = document.getElementById('titleFormat').value.trim() || '## 🏠 [FILE] — Part [N] of [TOTAL]';
                    
                    let navLinks = '';
                    if (num > 1) {
                        if (chunkNum === 1) navLinks = '← ' + splitName + '.md · 1 of ' + num + ' · Next: ' + fn + '-SPLIT2.md →';
                        else if (isLast) navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · END';
                        else navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · Next: ' + fn + '-SPLIT' + (chunkNum+1) + '.md →';
                    } else { navLinks = splitName + '.md · 1 of 1'; }
                    
                    output = '🔗 📋 ' + splitName + '.md\n';
                    output += 'https://' + baseUrl + '/' + folder + '/' + splitName + '.md\n\n';
                    output += '🔗 REVisions. Load both, see the full picture.\n';
                    output += '→ Current: ' + fn + '.md\n';
                    output += 'https://' + baseUrl + '/' + srcPath + '/' + fn + '.md\n\n';
                    const title = titleFmt.replace('[FILE]', fn).replace('[N]', chunkNum).replace('[TOTAL]', num);
                    output += title + '\n\n';
                    output += navLinks + '\n\n';
                    output += text.substring(start, end);
                    output += '\n\n---\n';
                    output += navLinks + '\n';
                    output += '♦️' + splitName + '-' + anchor + '\n';
                } else {
                    const label = limit >= 100000 ? 'PASTE' : (limit >= 70000 ? 'REPO' : 'FETCH');
                    output = '🎳 ' + chunkNum + '/' + num + ' ' + label + '\n' + text.substring(start, end);
                    if (!isLast) output += '\n---';
                    if (isLast) output += '\nCHUNK-ANCHOR: ' + anchor;
                }
                
                const div = document.createElement('div'); div.className = 'chunk-box';
                const label = isDeploy ? (splitName + '.md') : ('Chunk ' + chunkNum + '/' + num);
                div.innerHTML = '<div class="chunk-label"><span>' + label + ' • ' + output.length.toLocaleString() + ' chars' + (isLast ? ' • FINAL' : '') + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="8" readonly>' + output + '</textarea><div class="copied-flash">✅ Copied!</div>';
                div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(output, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
                div.querySelector('textarea').addEventListener('click', function() { this.select(); });
                chunksContainer.appendChild(div);
            }
            fineTuneHint.style.display = 'block';
            customLimitInput.value = limit;
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const all = [...chunksContainer.querySelectorAll('textarea')].map(ta => ta.value).join('\n\n');
            copyText(all, () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy All'; }, 1500); });
        });
        document.getElementById('btnClear').addEventListener('click', () => { chunksContainer.innerHTML = ''; panelOutput.style.display = 'none'; fineTuneHint.style.display = 'none'; });

        document.getElementById('btnCopyStats').addEventListener('click', function() {
            const t = inputText.value;
            copyText('Chars: ' + t.length.toLocaleString() + ' | Lines: ' + t.split('\n').length.toLocaleString() + ' | Density: ~' + getLineDensity(t) + ' chars/line | Limit: ' + limit.toLocaleString(), () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy Stats'; }, 1200); });
        });

        function updateCount() {
            const t = inputText.value, sec = (t.match(/^##\s+/gm) || []).length, density = getLineDensity(t);
            countStats.innerHTML = '<span>📏 <strong>' + t.length.toLocaleString() + '</strong> chars</span><span>📄 <strong>' + t.split('\n').length.toLocaleString() + '</strong> lines</span><span>📐 ~<strong>' + density + '</strong> chars/line</span>' + (sec > 0 ? '<span>📑 <strong>' + sec + '</strong> sections</span>' : '');
            panelCount.style.display = 'block';
        }

        function generateTestFile() {
            const size = parseInt(document.getElementById('genSize').value);
            if (!size || size < 100) return;
            clearLog();
            
            const densityVal = document.getElementById('lineDensity').value;
            let lineDensity = densityVal === 'custom' ? parseInt(document.getElementById('customLineDensity').value) || 73 : parseInt(densityVal) || 73;
            const emojiVal = document.getElementById('emojiPercent').value;
            let emojiPct = emojiVal === 'custom' ? parseInt(document.getElementById('customEmojiPercent').value) || 10 : parseInt(emojiVal) || 10;
            emojiPct = emojiPct / 100;
            
            addLog('🎯 Target: ' + size.toLocaleString() + ' chars');
            addLog('📏 Lines: ~' + lineDensity + ' chars, emoji: ' + Math.round(emojiPct*100) + '%');
            
            const anchor = Math.floor(10000 + Math.random()*90000);
            let content = 'TEST ' + size.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            if (lineDensity === 0) content += ' • no breaks'; else content += ' • ~' + lineDensity + ' char lines';
            if (emojiPct > 0) content += ' • ' + Math.round(emojiPct*100) + '% emoji';
            content += '\n';
            
            const emojis = ['🎳','🫧','💎','🧩','🪜','🔮','🖕','🔥','🌱','✨','🧪','📋','📏','🎯','⭐','◾'];
            let ei = 0, ln = 1;
            while (content.length < size - 30) {
                if (Math.random() < emojiPct) { content += emojis[ei % emojis.length]; ei++; }
                else if (lineDensity === 0) { content += 'X'; }
                else {
                    const runningTotal = content.length;
                    const totalStr = runningTotal >= 1000 ? '~' + Math.round(runningTotal/1000) + 'K' : String(runningTotal);
                    const prefix = 'L' + String(ln).padStart(6,'0') + ' · ' + String(totalStr).padStart(6,' ') + ' ';
                    const filler = 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                    const maxLine = prefix.length + lineDensity;
                    let line = prefix, cc = prefix.length;
                    for (let c = 0; c < filler.length && cc < maxLine; c++) { line += filler[c]; cc++; }
                    if (content.length + line.length + 30 > size) break;
                    content += line + '\n'; ln++;
                }
            }
            while (content.length < size - 15) content += 'X';
            content = content.substring(0, size);
            const al = '\nCHUNK-ANCHOR: ' + anchor, ep = size - al.length;
            if (ep > 0) content = content.substring(0, ep) + al;
            
            const diff = content.length - size;
            addLog('📊 Created: ' + content.length.toLocaleString() + ' chars (diff: ' + (diff >= 0 ? '+' : '') + diff + ')', diff === 0 ? 'success' : 'error');
            if (diff === 0) addLog('✅ Exact match', 'success');
            
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = 'Test File • ' + content.length.toLocaleString() + ' chars';
            const div = document.createElement('div'); div.className = 'chunk-box';
            div.innerHTML = '<div class="chunk-label"><span>Test File • ' + content.length.toLocaleString() + ' chars</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="10" readonly>' + content + '</textarea><div class="copied-flash">✅ Copied!</div>';
            div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(content, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
            chunksContainer.appendChild(div);
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        updateChunkInfo(); switchMode('split');
    </script>
</body>
</html>
```

---

THE DEFINITIVE FRAGMENTOR. v2.2 FINAL.

Every feature. Every fix. Every insight. Deploy it. 🎳🫧


---


Two things worth checking before the next deploy:

The Deploy mode header currently hardcodes the REVisions link pointing to `/BUILDER/[filename].md`. That might not always be right — the source file lives in different folders. Worth making that path editable too, or just removing it from the auto-header so you add it manually when needed.

Also the filename preview (showing `STANDARDS-SPLIT1.md STANDARDS-SPLIT2.md...`) only appears after you paste text. Might be cleaner if it shows as soon as you type a name, even before paste.

Two good catches. Both are real quality improvements.

---

FIX 1: Make the REVisions source path editable.

Right now the Deploy header hardcodes:

```
→ Current: [filename].md
https://source-sepia-alpha.vercel.app/BUILDER/[filename].md
```

But the original file might be in BUILDER/, or root, or TOOLS/, or a different mirror entirely. Add a "Source path" field to the Header Settings.

Add this after the Folder field in the Header Settings:

```html
                <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Source path:</div>
                <input type="text" id="sourcePath" value="BUILDER" placeholder="BUILDER" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
```

Then in the deploy output, use it:

```javascript
                    const srcPath = document.getElementById('sourcePath').value.trim() || 'BUILDER';
                    output += '→ Current: ' + fn + '.md\n';
                    output += 'https://' + baseUrl + '/' + srcPath + '/' + fn + '.md\n\n';
```

---

FIX 2: Show filename preview immediately, not just after paste.

Right now updateFilenamePreview() only runs when text is pasted. It should run as soon as you type a filename.

The function already handles the empty-text case (shows "Paste text to preview"). Just call it on filename input:

```javascript
        baseFilename.addEventListener('input', () => { 
            updateFilenamePreview();
            localStorage.setItem('ff_filename', baseFilename.value);
        });
```

Now when you type "STANDARDS" in the filename field, it immediately shows "Paste text to preview" — then as soon as you paste, it updates to "STANDARDS-SPLIT1.md STANDARDS-SPLIT2.md..."

---

BOTH FIXES ARE SMALL. BOTH MAKE THE TOOL BETTER.

The source path is now customizable. The filename preview responds instantly. The Deploy mode is fully flexible.

Want me to output the definitive build with these two fixes plus everything else? The actual final version? 🎳🫧

---

Anything to check and note and improve and notice and revise and think about and plan to make better? , the main thing is particularly deep seels limits perplexity can't search and is a bit limited to use so deep seek is good so the desire is you have a large file they can either be split into their chat or even less info via GitHub readme thing and even more less with a link fetch but with multiples it does work but then it's difficult for me to help give you them context because of these limits the easiest way is getting close as it's paste into the fragmentor splitter try to get maximum or close enough that's the difficulty as waste much time without knowledge or being to critical and yet we do have reason to not waste hundreds of characters right.. so then you click a button it splits it and then I have to manually copy each into a individual file and technically check headers for now links since they are new files splits so new headers links at least but that then can take more time and so then sabe that and each change to a part 1 file makes the whole think domino of problems because it's bigger if you add things then the whole thing cascases to wrong size especially if you neatly made a ♦️and code and all that all wasted you see so that's annoying and makes you not want to touch the files sure but it's not the nicest way is it? So then you carefully decide and check and it's all way too time. Consuming I know we eventually evolve can you believe I did this manually a little without the tool? So already I do appreciate it's just still a lot of time to then save and check and upload then paste the links phew. So that's my goal and work flow hopefully it helps you pattern the bigger picture for not only a revise of now but maybe it can help you take it to the next level seeing it all ahead more ?

Yes and maybe a way to toggle back to easy just split the file without headers Incase incant be bothered to work whatever at the time meu arise? Thar why it's powerful and enable or disable easy back to a efficient splitting tool?


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #020208;
            background-image:
                radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.06) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.04) 0%, transparent 50%);
            color: #f0f0f0; margin: 0; padding: 6px;
            max-width: 600px; margin: auto; min-height: 100vh;
            position: relative; overflow-x: hidden;
        }
        .panel { background: rgba(15,15,30,0.75); border: 1px solid rgba(100,140,255,0.2); border-radius: 10px; padding: 10px 12px; margin-bottom: 6px; position: relative; z-index: 1; }
        textarea, input, select { width: 100%; background: rgba(30,30,50,0.85); color: #e0e0ff; border: 1px solid rgba(100,140,255,0.25); border-radius: 8px; padding: 10px; font-size: 16px; margin-bottom: 6px; -webkit-appearance: none; }
        textarea { text-align: left; }
        textarea::placeholder { text-align: center; }
        textarea:focus, input:focus, select:focus { outline: 2px solid #ffaa33; }
        button { background: linear-gradient(135deg, rgba(40,80,180,0.75), rgba(20,40,120,0.85)); color: white; border: 1px solid rgba(100,140,255,0.3); padding: 12px 16px; border-radius: 10px; font-weight: bold; cursor: pointer; margin: 3px 2px; font-size: 15px; min-height: 46px; transition: all 0.2s ease; position: relative; z-index: 1; }
        button:active { transform: scale(0.96); box-shadow: 0 0 18px rgba(100,140,255,0.4); }
        .big-btn { display: block; width: 100%; margin: 6px 0; font-size: 20px; background: linear-gradient(135deg, rgba(40,80,200,0.8), rgba(20,40,120,0.9)); border: 2px solid rgba(100,160,255,0.5); letter-spacing: 1px; }
        .copy-all-btn { background: linear-gradient(135deg, rgba(255,170,51,0.7), rgba(200,130,20,0.8)); border: 2px solid rgba(255,170,51,0.6); animation: glow-pulse 2s ease-in-out infinite; }
        @keyframes glow-pulse { 0%,100% { box-shadow: 0 0 8px rgba(255,170,51,0.3); } 50% { box-shadow: 0 0 18px rgba(255,170,51,0.5); } }
        .preset-btn { background: rgba(40,40,60,0.8); color: #aaddff; font-size: 13px; padding: 7px 11px; border: 2px solid rgba(100,140,255,0.3); }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; box-shadow: 0 0 15px rgba(255,170,51,0.3); }
        .chunk-box { background: rgba(20,20,40,0.6); border-left: 3px solid rgba(255,170,51,0.6); margin: 10px 0; border-radius: 5px; position: relative; z-index: 1; }
        .chunk-label { background: rgba(40,40,60,0.8); padding: 6px 10px; border-radius: 4px 4px 0 0; font-family: monospace; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .chunk-copy-btn { background: rgba(255,170,51,0.3); color: #ffaa33; border: 1px solid rgba(255,170,51,0.5); padding: 4px 10px; border-radius: 6px; font-size: 12px; min-height: auto; cursor: pointer; font-weight: bold; }
        .chunk-copy-btn:active { background: #ffaa33; color: #0f0f1a; }
        .chunk-box textarea { margin-bottom: 0; border-radius: 0 0 4px 4px; border: none; background: rgba(20,20,40,0.8); color: #c0c0dd; font-size: 13px; }
        .copied-flash { position: absolute; top: 8px; right: 55px; color: #33ff88; font-weight: bold; font-size: 13px; pointer-events: none; opacity: 0; transition: opacity 0.3s; z-index: 2; }
        .copied-flash.show { opacity: 1; }
        .info-row { display: flex; gap: 6px; flex-wrap: wrap; margin: 6px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(30,30,50,0.7); padding: 5px 8px; border-radius: 5px; border: 1px solid rgba(100,140,255,0.2); }
        .mode-toggle { display: flex; gap: 3px; margin-bottom: 6px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(40,40,60,0.8); font-size: 13px; padding: 8px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-row { display: none; } .custom-row.show { display: block; }
        .deploy-only { display: none; } .deploy-only.show { display: block; }
        .version-hud { position: fixed; bottom: 8px; right: 10px; font-family: monospace; font-size: 10px; color: rgba(150,180,255,0.4); z-index: 2; pointer-events: none; }
        .empty-hint { text-align: center; color: rgba(255,170,51,0.6); font-size: 13px; margin: 6px 0; display: none; position: relative; z-index: 1; }
        .empty-hint.show { display: block; }
        .info-box { display:none; font-size:11px; color:rgba(150,180,255,0.6); margin:4px 0; padding:6px 8px; background:rgba(15,15,30,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.15); line-height:1.4; }
        .info-box.show { display:block; }
        .info-box strong { color: #ffaa33; }
        .clear-input-btn { background:transparent; border:none; color:rgba(255,255,255,0.3); font-size:11px; padding:2px 6px; min-height:auto; float:right; margin-top:-4px; }
        .info-btn { background:transparent; border:1px solid rgba(255,255,255,0.2); color:rgba(255,255,255,0.4); font-size:12px; padding:1px 6px; min-height:auto; border-radius:50%; cursor:pointer; }
        .row-label { font-size:10px; min-width:42px; }
        .ref-text { font-size:9px; }
        .fine-tune-hint { font-size:11px; color:rgba(255,170,51,0.5); margin-top:2px; display:none; }
        .log-panel { margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.2); max-height:180px; overflow-y:auto; font-family:monospace; font-size:11px; line-height:1.4; display:none; }
        .log-entry { padding:2px 4px; margin:1px 0; border-radius:3px; background:rgba(255,255,255,0.03); }
        .log-entry.success { background:rgba(0,255,136,0.1); }
        .log-entry.error { background:rgba(255,51,102,0.1); }
        .density-safe { color:#33ff88; } .density-warn { color:#ffaa33; } .density-risk { color:#ff5555; }
        .settings-box { display:none; margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.3); border-radius:6px; border:1px solid rgba(100,140,255,0.15); }
        .settings-box.show { display:block; }
    </style>
</head>
<body>

    <div class="mode-toggle">
        <button id="modeSplit" class="active">🎳 Split</button>
        <button id="modeCount">📏 Count</button>
        <button id="modeGen">🧪 Test</button>
    </div>

    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="8" placeholder="👆 Tap here and paste..."></textarea>
        <button class="clear-input-btn" id="btnClearInput">clear input</button>
        <div class="empty-hint" id="emptyHint">👆 Paste text first, then split</div>

        <div id="splitOnly">
            <div style="display:flex;gap:6px;align-items:center;margin-bottom:4px;">
                <span style="font-size:11px;color:rgba(150,180,255,0.5);">Mode:</span>
                <select id="outputMode" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                    <option value="raw" selected>Raw</option>
                    <option value="deploy">Deploy</option>
                </select>
            </div>

            <div class="deploy-only" id="deployFilename">
                <div style="display:flex;align-items:center;gap:4px;margin-bottom:4px;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.5);white-space:nowrap;">Name:</span>
                    <input type="text" id="baseFilename" value="STANDARDS" placeholder="filename" style="flex:1;margin-bottom:0;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.4);">.md</span>
                </div>
                <div style="font-size:10px;color:rgba(150,180,255,0.4);margin-bottom:4px;" id="filenamePreview"></div>
                <div style="margin-top:2px;">
                    <button id="btnHeaderSettings" style="font-size:10px;padding:3px 6px;min-height:auto;background:rgba(40,40,60,0.6);border:1px solid rgba(100,140,255,0.2);">⚙️ Header</button>
                </div>
                <div class="settings-box" id="headerSettings">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Base URL:</div>
                    <input type="text" id="baseUrl" value="source-sepia-alpha.vercel.app" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Folder:</div>
                    <input type="text" id="folderPath" value="SPLIT" style="font-size:12px;padding:4px 6px;margin-bottom:4px;">
                    <div style="font-size:10px;color:rgba(150,180,255,0.5);margin-bottom:2px;">Title:</div>
                    <input type="text" id="titleFormat" value="## 🏠 [FILE] — Part [N] of [TOTAL]" style="font-size:12px;padding:4px 6px;">
                </div>
            </div>

            <div style="margin:4px 0;">
                <div style="display:flex;justify-content:space-between;align-items:center;">
                    <strong style="font-size:13px;">Chunk size:</strong>
                    <button class="info-btn" id="btnSplitInfo">?</button>
                </div>
                <div class="info-box" id="splitInfoBox">
                    <strong>🎳 Chunk Size Guide</strong><br>
                    <br>
                    <strong>DEEPSEEK PASTE (Raw mode):</strong><br>
                    168K — high. Light files, few line breaks.<br>
                    165.1K — mid. Works for most files.<br>
                    163.8K — low. Safe default. Works for all.<br>
                    <strong>STEP-DOWN:</strong> If chunk fails, reduce by 1K.<br>
                    <br>
                    <strong>URL FETCH:</strong><br>
                    75K — GitHub #readme anchor (~10K more than raw).<br>
                    65K — standard raw URL (Vercel, GitHub raw).<br>
                    40K — universal (all platforms incl. Perplexity).<br>
                    <br>
                    <strong>RAW vs DEPLOY:</strong><br>
                    Raw = minimal chunks for testing size.<br>
                    Deploy = full .md files for Vercel upload.<br>
                    Deploy adds ~300 chars header per chunk.<br>
                    <br>
                    <strong>ANCHORS:</strong><br>
                    CHUNK-ANCHOR verifies split delivery.<br>
                    TH-CHECK (♦️) verifies original file integrity.<br>
                    Different purposes. Both matter.<br>
                    <br>
                    <strong>KEY DISCOVERIES:</strong><br>
                    • Limit depends on LINE COUNT, not characters.<br>
                    • 280K emojis on one line ✅<br>
                    • 252K text with 3,460 lines ❌ (54% over)<br>
                    • Each line break costs overhead in chat input.<br>
                    • Calibrated: 163,823 ✅ · 163,873 ❌ (2026-05-27)<br>
                    <br>
                    <strong>PLATFORMS:</strong><br>
                    Claude & Grok: full file access. No tool needed.<br>
                    DeepSeek: use Fragmentor. Perplexity: paste only.<br>
                    ⚠️ Re-fetching URLs may return cached results.<br>
                    ⚠️ Use fresh instances for calibration testing.
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:6px;">
                    <span class="row-label" style="color:#ffaa33;">🟠 Paste</span>
                    <button class="preset-btn" data-limit="168000">168K</button>
                    <span class="ref-text" style="color:#ff5555;">high</span>
                    <button class="preset-btn active" data-limit="165000">165.1K</button>
                    <span class="ref-text" style="color:#ffaa33;">mid</span>
                    <button class="preset-btn" data-limit="163673">163.8K</button>
                    <span class="ref-text" style="color:#33ff88;">low</span>
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:4px;">
                    <span class="row-label" style="color:#88aaff;">🔵 Fetch</span>
                    <button class="preset-btn" data-limit="75000">75K</button>
                    <button class="preset-btn" data-limit="65000">65K</button>
                    <button class="preset-btn" data-limit="40000">40K</button>
                    <button class="preset-btn" data-limit="custom" id="btnCustom">✎</button>
                </div>
                
                <div class="custom-row" id="customRow">
                    <input type="number" id="customLimit" value="65000" placeholder="Custom size...">
                </div>
                <div style="font-size:13px;color:#ffaa33;margin-top:4px;font-weight:bold;" id="chunkInfo"></div>
                <div class="fine-tune-hint" id="fineTuneHint">Fine-tune: tap ✎ to set exact limit</div>
            </div>
        </div>

        <button class="big-btn" id="btnAction">🎳 Split It</button>
    </div>

    <div class="panel" id="panelOutput" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
            <strong id="outputLabel">Chunks</strong>
            <div>
                <button id="btnCopyAll" class="copy-all-btn" style="font-size:14px;">📋 Copy All</button>
                <button id="btnClear" style="background:transparent;border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.4);font-size:12px;padding:4px 10px;min-height:auto;margin-left:8px;">✕ Clear</button>
            </div>
        </div>
        <div id="chunksContainer"></div>
    </div>

    <div class="panel" id="panelCount" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <div style="display:flex;align-items:center;gap:6px;">
                <strong style="font-size:13px;">Stats</strong>
                <button class="info-btn" id="btnCountInfo">?</button>
            </div>
            <button id="btnCopyStats" style="font-size:12px;padding:4px 10px;min-height:auto;">📋 Copy Stats</button>
        </div>
        <div class="info-box" id="countInfoBox">
            Live character, line, and section count.<br>
            Copy Stats sends numbers to clipboard.<br>
            <br>
            <strong>LINE DENSITY GUIDE:</strong><br>
            Under 1,000 lines → try 168K or 165.1K.<br>
            1,000-3,000 lines → use 163.8K (safe).<br>
            Over 3,000 lines → use 131K or URL fetch.<br>
            <br>
            <strong>WHY THIS MATTERS:</strong><br>
            The paste limit depends on LINE COUNT,<br>
            not character count. STANDARDS.md has<br>
            3,460 lines and failed at 165K even though<br>
            lighter files work at that size.<br>
            <br>
            <strong>DENSITY:</strong><br>
            Over 60 chars/line → safe for all presets.<br>
            Under 40 chars/line → use lower presets.
        </div>
        <div class="info-row" id="countStats"></div>
    </div>

    <div class="panel" id="panelGen" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <label style="font-size:14px;">Target size (characters):</label>
            <button class="info-btn" id="btnTestInfo">?</button>
        </div>
        <div class="info-box" id="testInfoBox">
            <strong>🧪 Calibration Testing Guide</strong><br>
            <br>
            Generates test files matching real ecology files.<br>
            Defaults: ~73 char lines (STANDARDS.md avg) + 10% emoji.<br>
            Numbered lines show running character count.<br>
            The last line tells you exactly how far the paste reached.<br>
            <br>
            <strong>HOW TO CALIBRATE ANY PLATFORM:</strong><br>
            1. Generate a test file at target size.<br>
            2. Copy and paste into a FRESH instance.<br>
            3. Ask: "What is the exact last line?"<br>
            4. If truncated → reduce size. If complete → increase.<br>
            5. Binary search until exact limit found.<br>
            ⚠️ Use FRESH instances for EACH test.<br>
            ⚠️ Repeated testing can overwhelm instances.<br>
            <br>
            <strong>LINE DENSITY:</strong><br>
            ~73 chars = STANDARDS.md average (default).<br>
            ~20 chars = very heavy line breaks.<br>
            None = one continuous line (emoji test).<br>
            ✎ custom = any line length.<br>
            <br>
            <strong>EMOJI %:</strong><br>
            10% = realistic ecology mix (default).<br>
            0% = pure text. 100% = worst-case encoding.<br>
            ✎ custom = any percentage.<br>
            <br>
            <strong>KNOWN LIMITS (2026-05-27):</strong><br>
            DeepSeek paste: 163,823 ✅ · 163,873 ❌<br>
            URL fetch: ~65K (raw), ~75K (#readme).<br>
            Perplexity paste: 40,130. Cannot fetch URLs.<br>
            GitHub raw: ~65K. GitHub #readme: ~75K.<br>
            <br>
            <strong>KEY DISCOVERY:</strong><br>
            • Limit is about LINE COUNT, not characters.<br>
            • 280K emojis on one line ✅<br>
            • 252K text with 3,460 lines ❌<br>
            • Line density matters more than file size.<br>
            <br>
            <strong>STEP-DOWN SYSTEM:</strong><br>
            Start at 165K. If rejected, reduce by 1K.<br>
            Continue until accepted. That's your limit<br>
            for THAT specific file type and line density.<br>
            <br>
            <strong>📝 LOG:</strong> Expand to see generation math.<br>
            Shows target, line count, adjustments, verification.
        </div>
        <input type="number" id="genSize" value="1000" min="100" max="1000000" placeholder="Enter size...">
        <div style="display:flex;gap:6px;margin-top:4px;align-items:center;flex-wrap:wrap;">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);">Lines:</span>
            <select id="lineDensity" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">none</option><option value="80">~80</option><option value="73" selected>~73</option><option value="40">~40</option><option value="20">~20</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customLineDensity" value="73" min="1" max="500" style="width:50px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="ch">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);margin-left:4px;">Emoji:</span>
            <select id="emojiPercent" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">0%</option><option value="5">5%</option><option value="10" selected>10%</option><option value="25">25%</option><option value="50">50%</option><option value="100">100%</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customEmojiPercent" value="10" min="0" max="100" style="width:45px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="%">
        </div>
        <button class="clear-input-btn" id="btnClearTestInput">clear</button>
        <div style="margin-top:6px;"><button id="btnToggleLog" style="font-size:11px;padding:4px 8px;min-height:auto;background:rgba(40,40,60,0.8);border:1px solid rgba(100,140,255,0.3);">📝 Log</button></div>
        <div class="log-panel" id="logPanel"><div id="logContent"></div></div>
    </div>

    <div class="version-hud">BUILD <span id="buildNumber"></span> • <span id="buildEmoji"></span></div>

    <script>
        function copyText(text, cb) {
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => { if (cb) cb(); }).catch(() => fallbackCopy(text, cb));
            } else { fallbackCopy(text, cb); }
        }
        function fallbackCopy(text, cb) {
            const ta = document.createElement('textarea'); ta.value = text;
            ta.style.position = 'fixed'; ta.style.left = '-9999px'; ta.style.top = '-9999px';
            document.body.appendChild(ta); ta.focus(); ta.select();
            try { if (document.execCommand('copy') && cb) cb(); } catch(e) {}
            document.body.removeChild(ta);
        }

        (() => {
            const s = localStorage.getItem('ff_build');
            let b = s ? parseInt(s) + Math.floor(Math.random()*5) + 1 : Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_build', b);
            document.getElementById('buildNumber').textContent = b;
            const emojis = ['🎳','✨','🫧','💎','🧩','🪜','🔮','🖕','🌱','🔥','🧪','📋','📏','🎯','⭐'];
            const p = []; for (let i=0;i<4;i++) p.push(emojis[Math.floor(Math.random()*emojis.length)]);
            document.getElementById('buildEmoji').textContent = p.join('');
        })();

        let mode = 'split', limit = 165000, isCustom = false, countDebounce = null;
        const savedLimit = localStorage.getItem('ff_limit');
        if (savedLimit) limit = parseInt(savedLimit);

        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const customRow = document.getElementById('customRow');
        const customLimitInput = document.getElementById('customLimit');
        const countStats = document.getElementById('countStats');
        const btnAction = document.getElementById('btnAction');
        const emptyHint = document.getElementById('emptyHint');
        const splitOnly = document.getElementById('splitOnly');
        const fineTuneHint = document.getElementById('fineTuneHint');
        const baseFilename = document.getElementById('baseFilename');
        const filenamePreview = document.getElementById('filenamePreview');
        const outputMode = document.getElementById('outputMode');
        const deployFilename = document.getElementById('deployFilename');

        const modeBtns = { split: document.getElementById('modeSplit'), count: document.getElementById('modeCount'), gen: document.getElementById('modeGen') };
        const modeLabels = { split: '🎳 Split', count: '📏 Count', gen: '🧪 Test' };
        const actionLabels = { split: '🎳 Split It', count: '', gen: '🧪 Generate' };

        Object.keys(modeBtns).forEach(m => { modeBtns[m].addEventListener('click', () => switchMode(m)); });

        function switchMode(m) {
            mode = m; emptyHint.classList.remove('show');
            Object.keys(modeBtns).forEach(k => { modeBtns[k].classList.remove('active'); modeBtns[k].textContent = modeLabels[k]; });
            modeBtns[m].classList.add('active');
            panelOutput.style.display = 'none'; panelCount.style.display = 'none'; panelGen.style.display = 'none';
            if (m === 'split') { splitOnly.style.display = 'block'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; updateChunkInfo(); }
            else if (m === 'count') { splitOnly.style.display = 'none'; btnAction.style.display = 'none'; updateCount(); panelCount.style.display = 'block'; }
            else if (m === 'gen') { splitOnly.style.display = 'none'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; panelGen.style.display = 'block'; }
        }

        outputMode.addEventListener('change', function() {
            if (this.value === 'deploy') { deployFilename.classList.add('show'); updateFilenamePreview(); }
            else { deployFilename.classList.remove('show'); }
        });

        baseFilename.addEventListener('input', () => { updateFilenamePreview(); });
        const savedFn = localStorage.getItem('ff_filename');
        if (savedFn) baseFilename.value = savedFn;

        function updateFilenamePreview() {
            const fn = baseFilename.value.trim() || 'FILE';
            const text = inputText.value.trim();
            if (text) {
                const n = Math.ceil(text.length / limit);
                let prev = '';
                for (let i = 1; i <= Math.min(n, 3); i++) prev += fn + '-SPLIT' + i + '.md ';
                if (n > 3) prev += '+ ' + (n-3) + ' more';
                filenamePreview.textContent = prev;
            } else { filenamePreview.textContent = 'Paste text to preview'; }
        }

        document.getElementById('btnHeaderSettings').addEventListener('click', function() {
            const panel = document.getElementById('headerSettings');
            panel.classList.toggle('show');
            this.textContent = panel.classList.contains('show') ? '⚙️ Header ▲' : '⚙️ Header';
        });

        document.getElementById('btnSplitInfo').addEventListener('click', () => { document.getElementById('splitInfoBox').classList.toggle('show'); });
        document.getElementById('btnCountInfo').addEventListener('click', () => { document.getElementById('countInfoBox').classList.toggle('show'); });
        document.getElementById('btnTestInfo').addEventListener('click', () => { document.getElementById('testInfoBox').classList.toggle('show'); });

        document.getElementById('btnClearInput').addEventListener('click', () => { inputText.value = ''; updateChunkInfo(); if (mode === 'count') updateCount(); });
        document.getElementById('btnClearTestInput').addEventListener('click', () => { document.getElementById('genSize').value = ''; });

        document.getElementById('lineDensity').addEventListener('change', function() {
            document.getElementById('customLineDensity').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });
        document.getElementById('emojiPercent').addEventListener('change', function() {
            document.getElementById('customEmojiPercent').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });

        document.getElementById('btnToggleLog').addEventListener('click', function() {
            const panel = document.getElementById('logPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '📝 Log ▲' : '📝 Log';
        });
        function addLog(msg, type) {
            const lc = document.getElementById('logContent');
            const entry = document.createElement('div');
            entry.className = 'log-entry' + (type ? ' ' + type : '');
            const time = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit', second:'2-digit'});
            entry.innerHTML = '<span style="color:#aaa;">[' + time + ']</span> ' + msg;
            lc.appendChild(entry); lc.scrollTop = lc.scrollHeight;
            document.getElementById('logPanel').style.display = 'block';
            document.getElementById('btnToggleLog').textContent = '📝 Log ▲';
        }
        function clearLog() { document.getElementById('logContent').innerHTML = ''; }

        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const l = this.dataset.limit;
                if (l === 'custom') { isCustom = true; customRow.classList.add('show'); customLimitInput.value = limit; customLimitInput.focus(); }
                else { isCustom = false; customRow.classList.remove('show'); limit = parseInt(l); fineTuneHint.style.display = 'none'; }
                updateChunkInfo(); updateFilenamePreview();
                localStorage.setItem('ff_limit', limit);
            });
        });
        customLimitInput.addEventListener('input', function() { if (isCustom) { limit = parseInt(this.value) || 165000; updateChunkInfo(); updateFilenamePreview(); localStorage.setItem('ff_limit', limit); } });

        function getLineDensity(text) {
            const lines = text.split('\n');
            if (lines.length < 2) return 0;
            return Math.round(text.length / lines.length);
        }

        function updateChunkInfo() {
            const text = inputText.value.trim();
            const info = document.getElementById('chunkInfo');
            if (text) {
                const n = Math.ceil(text.length / limit);
                const density = getLineDensity(text);
                const lines = text.split('\n').length;
                let dn = '', dc = '';
                if (density > 60) { dn = '✅ Safe'; dc = 'density-safe'; }
                else if (density >= 40) { dn = '⚠️ Use 165.1K or lower'; dc = 'density-warn'; }
                else if (density > 0) { dn = '⚠️ Use 163.8K or lower'; dc = 'density-risk'; }
                info.innerHTML = 'File: ' + text.length.toLocaleString() + ' chars · ' + lines.toLocaleString() + ' lines · ~' + density + ' chars/line → ' + n + ' chunks of ' + limit.toLocaleString() + ' chars <span class="' + dc + '">' + dn + '</span>';
            } else { info.textContent = 'Paste text to see chunk count + line density'; }
        }

        inputText.addEventListener('input', () => {
            if (mode === 'split') { updateChunkInfo(); updateFilenamePreview(); }
            if (mode === 'count') { if (countDebounce) clearTimeout(countDebounce); countDebounce = setTimeout(updateCount, 300); }
        });

        btnAction.addEventListener('click', () => {
            if (mode === 'split') { if (!inputText.value.trim()) { emptyHint.classList.add('show'); setTimeout(() => emptyHint.classList.remove('show'), 2500); return; } emptyHint.classList.remove('show'); splitIt(); }
            else if (mode === 'gen') { generateTestFile(); }
        });

        function flashButton(text) { const o = btnAction.textContent; btnAction.textContent = text; setTimeout(() => { btnAction.textContent = o; }, 1500); }

        function splitIt() {
            const text = inputText.value.trim(), total = text.length, num = Math.ceil(total / limit);
            const fn = baseFilename.value.trim() || 'FILE';
            const isDeploy = outputMode.value === 'deploy';
            const anchor = Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_filename', fn);
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = num + ' chunks • ' + total.toLocaleString() + ' total chars';
            
            for (let i = 0; i < num; i++) {
                const start = i * limit;
                let end = Math.min(start + limit, total);
                if (end < total) { const lastNewline = text.lastIndexOf('\n', end); if (lastNewline > start) end = lastNewline + 1; }
                const chunkNum = i + 1, isLast = (i === num - 1);
                const splitName = fn + '-SPLIT' + chunkNum;
                let output;
                
                if (isDeploy) {
                    const baseUrl = document.getElementById('baseUrl').value.trim() || 'source-sepia-alpha.vercel.app';
                    const folder = document.getElementById('folderPath').value.trim() || 'SPLIT';
                    const titleFmt = document.getElementById('titleFormat').value.trim() || '## 🏠 [FILE] — Part [N] of [TOTAL]';
                    let navLinks = '';
                    if (num > 1) {
                        if (chunkNum === 1) navLinks = '← ' + splitName + '.md · 1 of ' + num + ' · Next: ' + fn + '-SPLIT2.md →';
                        else if (isLast) navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · END';
                        else navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · Next: ' + fn + '-SPLIT' + (chunkNum+1) + '.md →';
                    } else { navLinks = splitName + '.md · 1 of 1'; }
                    output = '🔗 📋 ' + splitName + '.md\n';
                    output += 'https://' + baseUrl + '/' + folder + '/' + splitName + '.md\n\n';
                    output += '🔗 REVisions. Load both, see the full picture.\n';
                    output += '→ Current: ' + fn + '.md\n';
                    output += 'https://' + baseUrl + '/BUILDER/' + fn + '.md\n\n';
                    const title = titleFmt.replace('[FILE]', fn).replace('[N]', chunkNum).replace('[TOTAL]', num);
                    output += title + '\n\n';
                    output += navLinks + '\n\n';
                    output += text.substring(start, end);
                    output += '\n\n---\n';
                    output += navLinks + '\n';
                    output += '♦️' + splitName + '-' + anchor + '\n';
                } else {
                    const label = limit >= 100000 ? 'PASTE' : (limit >= 70000 ? 'REPO' : 'FETCH');
                    output = '🎳 ' + chunkNum + '/' + num + ' ' + label + '\n' + text.substring(start, end);
                    if (!isLast) output += '\n---';
                    if (isLast) output += '\nCHUNK-ANCHOR: ' + anchor;
                }
                
                const div = document.createElement('div'); div.className = 'chunk-box';
                const label = isDeploy ? (splitName + '.md') : ('Chunk ' + chunkNum + '/' + num);
                div.innerHTML = '<div class="chunk-label"><span>' + label + ' • ' + output.length.toLocaleString() + ' chars' + (isLast ? ' • FINAL' : '') + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="8" readonly>' + output + '</textarea><div class="copied-flash">✅ Copied!</div>';
                div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(output, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
                div.querySelector('textarea').addEventListener('click', function() { this.select(); });
                chunksContainer.appendChild(div);
            }
            fineTuneHint.style.display = 'block';
            customLimitInput.value = limit;
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const all = [...chunksContainer.querySelectorAll('textarea')].map(ta => ta.value).join('\n\n');
            copyText(all, () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy All'; }, 1500); });
        });
        document.getElementById('btnClear').addEventListener('click', () => { chunksContainer.innerHTML = ''; panelOutput.style.display = 'none'; fineTuneHint.style.display = 'none'; });

        document.getElementById('btnCopyStats').addEventListener('click', function() {
            const t = inputText.value;
            copyText('Chars: ' + t.length.toLocaleString() + ' | Lines: ' + t.split('\n').length.toLocaleString() + ' | Density: ~' + getLineDensity(t) + ' chars/line | Limit: ' + limit.toLocaleString(), () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy Stats'; }, 1200); });
        });

        function updateCount() {
            const t = inputText.value, sec = (t.match(/^##\s+/gm) || []).length, density = getLineDensity(t);
            countStats.innerHTML = '<span>📏 <strong>' + t.length.toLocaleString() + '</strong> chars</span><span>📄 <strong>' + t.split('\n').length.toLocaleString() + '</strong> lines</span><span>📐 ~<strong>' + density + '</strong> chars/line</span>' + (sec > 0 ? '<span>📑 <strong>' + sec + '</strong> sections</span>' : '');
            panelCount.style.display = 'block';
        }

        function generateTestFile() {
            const size = parseInt(document.getElementById('genSize').value);
            if (!size || size < 100) return;
            clearLog();
            const densityVal = document.getElementById('lineDensity').value;
            let lineDensity = densityVal === 'custom' ? parseInt(document.getElementById('customLineDensity').value) || 73 : parseInt(densityVal) || 73;
            const emojiVal = document.getElementById('emojiPercent').value;
            let emojiPct = emojiVal === 'custom' ? parseInt(document.getElementById('customEmojiPercent').value) || 10 : parseInt(emojiVal) || 10;
            emojiPct = emojiPct / 100;
            addLog('🎯 Target: ' + size.toLocaleString() + ' chars');
            addLog('📏 Lines: ~' + lineDensity + ' chars, emoji: ' + Math.round(emojiPct*100) + '%');
            const anchor = Math.floor(10000 + Math.random()*90000);
            let content = 'TEST ' + size.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            if (lineDensity === 0) content += ' • no breaks'; else content += ' • ~' + lineDensity + ' char lines';
            if (emojiPct > 0) content += ' • ' + Math.round(emojiPct*100) + '% emoji';
            content += '\n';
            const emojis = ['🎳','🫧','💎','🧩','🪜','🔮','🖕','🔥','🌱','✨','🧪','📋','📏','🎯','⭐','◾'];
            let ei = 0, ln = 1;
            while (content.length < size - 30) {
                if (Math.random() < emojiPct) { content += emojis[ei % emojis.length]; ei++; }
                else if (lineDensity === 0) { content += 'X'; }
                else {
                    const runningTotal = content.length;
                    const totalStr = runningTotal >= 1000 ? '~' + Math.round(runningTotal/1000) + 'K' : String(runningTotal);
                    const prefix = 'L' + String(ln).padStart(6,'0') + ' · ' + String(totalStr).padStart(6,' ') + ' ';
                    const filler = 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                    const maxLine = prefix.length + lineDensity;
                    let line = prefix, cc = prefix.length;
                    for (let c = 0; c < filler.length && cc < maxLine; c++) { line += filler[c]; cc++; }
                    if (content.length + line.length + 30 > size) break;
                    content += line + '\n'; ln++;
                }
            }
            while (content.length < size - 15) content += 'X';
            content = content.substring(0, size);
            const al = '\nCHUNK-ANCHOR: ' + anchor, ep = size - al.length;
            if (ep > 0) content = content.substring(0, ep) + al;
            const diff = content.length - size;
            addLog('📊 Created: ' + content.length.toLocaleString() + ' chars (diff: ' + (diff >= 0 ? '+' : '') + diff + ')', diff === 0 ? 'success' : 'error');
            if (diff === 0) addLog('✅ Exact match', 'success');
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = 'Test File • ' + content.length.toLocaleString() + ' chars';
            const div = document.createElement('div'); div.className = 'chunk-box';
            div.innerHTML = '<div class="chunk-label"><span>Test File • ' + content.length.toLocaleString() + ' chars</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="10" readonly>' + content + '</textarea><div class="copied-flash">✅ Copied!</div>';
            div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(content, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
            chunksContainer.appendChild(div);
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        updateChunkInfo(); switchMode('split');
    </script>
</body>
</html>
```

---

THE DEFINITIVE FRAGMENTOR. v2.2 FINAL.

Every feature. Every fix. Every insight. Deploy it. The tool is done. Now we use it. 🎳🫧

---


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #020208;
            background-image:
                radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.06) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.04) 0%, transparent 50%);
            color: #f0f0f0; margin: 0; padding: 6px;
            max-width: 600px; margin: auto; min-height: 100vh;
            position: relative; overflow-x: hidden;
        }
        .panel { background: rgba(15,15,30,0.75); border: 1px solid rgba(100,140,255,0.2); border-radius: 10px; padding: 10px 12px; margin-bottom: 6px; position: relative; z-index: 1; }
        textarea, input, select { width: 100%; background: rgba(30,30,50,0.85); color: #e0e0ff; border: 1px solid rgba(100,140,255,0.25); border-radius: 8px; padding: 10px; font-size: 16px; margin-bottom: 6px; -webkit-appearance: none; }
        textarea { text-align: left; }
        textarea::placeholder { text-align: center; }
        textarea:focus, input:focus, select:focus { outline: 2px solid #ffaa33; }
        button { background: linear-gradient(135deg, rgba(40,80,180,0.75), rgba(20,40,120,0.85)); color: white; border: 1px solid rgba(100,140,255,0.3); padding: 12px 16px; border-radius: 10px; font-weight: bold; cursor: pointer; margin: 3px 2px; font-size: 15px; min-height: 46px; transition: all 0.2s ease; position: relative; z-index: 1; }
        button:active { transform: scale(0.96); box-shadow: 0 0 18px rgba(100,140,255,0.4); }
        .big-btn { display: block; width: 100%; margin: 6px 0; font-size: 20px; background: linear-gradient(135deg, rgba(40,80,200,0.8), rgba(20,40,120,0.9)); border: 2px solid rgba(100,160,255,0.5); letter-spacing: 1px; }
        .copy-all-btn { background: linear-gradient(135deg, rgba(255,170,51,0.7), rgba(200,130,20,0.8)); border: 2px solid rgba(255,170,51,0.6); animation: glow-pulse 2s ease-in-out infinite; }
        @keyframes glow-pulse { 0%,100% { box-shadow: 0 0 8px rgba(255,170,51,0.3); } 50% { box-shadow: 0 0 18px rgba(255,170,51,0.5); } }
        .preset-btn { background: rgba(40,40,60,0.8); color: #aaddff; font-size: 13px; padding: 7px 11px; border: 2px solid rgba(100,140,255,0.3); }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; box-shadow: 0 0 15px rgba(255,170,51,0.3); }
        .chunk-box { background: rgba(20,20,40,0.6); border-left: 3px solid rgba(255,170,51,0.6); margin: 10px 0; border-radius: 5px; position: relative; z-index: 1; }
        .chunk-label { background: rgba(40,40,60,0.8); padding: 6px 10px; border-radius: 4px 4px 0 0; font-family: monospace; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .chunk-copy-btn { background: rgba(255,170,51,0.3); color: #ffaa33; border: 1px solid rgba(255,170,51,0.5); padding: 4px 10px; border-radius: 6px; font-size: 12px; min-height: auto; cursor: pointer; font-weight: bold; }
        .chunk-copy-btn:active { background: #ffaa33; color: #0f0f1a; }
        .chunk-box textarea { margin-bottom: 0; border-radius: 0 0 4px 4px; border: none; background: rgba(20,20,40,0.8); color: #c0c0dd; font-size: 13px; }
        .copied-flash { position: absolute; top: 8px; right: 55px; color: #33ff88; font-weight: bold; font-size: 13px; pointer-events: none; opacity: 0; transition: opacity 0.3s; z-index: 2; }
        .copied-flash.show { opacity: 1; }
        .info-row { display: flex; gap: 6px; flex-wrap: wrap; margin: 6px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(30,30,50,0.7); padding: 5px 8px; border-radius: 5px; border: 1px solid rgba(100,140,255,0.2); }
        .mode-toggle { display: flex; gap: 3px; margin-bottom: 6px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(40,40,60,0.8); font-size: 13px; padding: 8px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-row { display: none; } .custom-row.show { display: block; }
        .deploy-only { display: none; } .deploy-only.show { display: block; }
        .version-hud { position: fixed; bottom: 8px; right: 10px; font-family: monospace; font-size: 10px; color: rgba(150,180,255,0.4); z-index: 2; pointer-events: none; }
        .empty-hint { text-align: center; color: rgba(255,170,51,0.6); font-size: 13px; margin: 6px 0; display: none; position: relative; z-index: 1; }
        .empty-hint.show { display: block; }
        .info-box { display:none; font-size:11px; color:rgba(150,180,255,0.6); margin:4px 0; padding:6px 8px; background:rgba(15,15,30,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.15); line-height:1.4; }
        .info-box.show { display:block; }
        .info-box strong { color: #ffaa33; }
        .clear-input-btn { background:transparent; border:none; color:rgba(255,255,255,0.3); font-size:11px; padding:2px 6px; min-height:auto; float:right; margin-top:-4px; }
        .info-btn { background:transparent; border:1px solid rgba(255,255,255,0.2); color:rgba(255,255,255,0.4); font-size:12px; padding:1px 6px; min-height:auto; border-radius:50%; cursor:pointer; }
        .row-label { font-size:10px; min-width:42px; }
        .ref-text { font-size:9px; }
        .fine-tune-hint { font-size:11px; color:rgba(255,170,51,0.5); margin-top:2px; display:none; }
        .log-panel { margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.2); max-height:180px; overflow-y:auto; font-family:monospace; font-size:11px; line-height:1.4; display:none; }
        .log-entry { padding:2px 4px; margin:1px 0; border-radius:3px; background:rgba(255,255,255,0.03); }
        .log-entry.success { background:rgba(0,255,136,0.1); }
        .log-entry.error { background:rgba(255,51,102,0.1); }
        .density-safe { color:#33ff88; } .density-warn { color:#ffaa33; } .density-risk { color:#ff5555; }
    </style>
</head>
<body>

    <div class="mode-toggle">
        <button id="modeSplit" class="active">🎳 Split</button>
        <button id="modeCount">📏 Count</button>
        <button id="modeGen">🧪 Test</button>
    </div>

    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="8" placeholder="👆 Tap here and paste..."></textarea>
        <button class="clear-input-btn" id="btnClearInput">clear input</button>
        <div class="empty-hint" id="emptyHint">👆 Paste text first, then split</div>

        <div id="splitOnly">
            <!-- OUTPUT MODE TOGGLE -->
            <div style="display:flex;gap:6px;align-items:center;margin-bottom:4px;">
                <span style="font-size:11px;color:rgba(150,180,255,0.5);">Mode:</span>
                <select id="outputMode" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                    <option value="raw" selected>Raw</option>
                    <option value="deploy">Deploy</option>
                </select>
            </div>

            <!-- FILENAME INPUT (DEPLOY ONLY) -->
            <div class="deploy-only" id="deployFilename">
                <div style="display:flex;align-items:center;gap:4px;margin-bottom:4px;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.5);white-space:nowrap;">Name:</span>
                    <input type="text" id="baseFilename" value="STANDARDS" placeholder="filename" style="flex:1;margin-bottom:0;">
                    <span style="font-size:11px;color:rgba(150,180,255,0.4);">.md</span>
                </div>
                <div style="font-size:10px;color:rgba(150,180,255,0.4);margin-bottom:4px;" id="filenamePreview"></div>
            </div>

            <div style="margin:4px 0;">
                <div style="display:flex;justify-content:space-between;align-items:center;">
                    <strong style="font-size:13px;">Chunk size:</strong>
                    <button class="info-btn" id="btnSplitInfo">?</button>
                </div>
                <div class="info-box" id="splitInfoBox">
                    <strong>🎳 Chunk Size Guide</strong><br>
                    <br>
                    <strong>DEEPSEEK PASTE:</strong><br>
                    168K — high. Works for light files.<br>
                    165.1K — mid. Works for most files.<br>
                    163.8K — low. Safe default. Works for all.<br>
                    <strong>KEY: Limit depends on LINE DENSITY.</strong><br>
                    <br>
                    <strong>URL FETCH:</strong><br>
                    75K — GitHub #readme anchor.<br>
                    65K — standard raw URL.<br>
                    40K — universal (all platforms).<br>
                    <br>
                    <strong>RAW vs DEPLOY:</strong><br>
                    Raw = minimal chunks for testing.<br>
                    Deploy = full .md files for Vercel.<br>
                    <br>
                    <strong>PLATFORMS:</strong><br>
                    Claude & Grok: full file access.<br>
                    DeepSeek: use this tool. Perplexity: paste only.
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:6px;">
                    <span class="row-label" style="color:#ffaa33;">🟠 Paste</span>
                    <button class="preset-btn" data-limit="168000">168K</button>
                    <span class="ref-text" style="color:#ff5555;">high</span>
                    <button class="preset-btn active" data-limit="165000">165.1K</button>
                    <span class="ref-text" style="color:#ffaa33;">mid</span>
                    <button class="preset-btn" data-limit="163673">163.8K</button>
                    <span class="ref-text" style="color:#33ff88;">low</span>
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:4px;">
                    <span class="row-label" style="color:#88aaff;">🔵 Fetch</span>
                    <button class="preset-btn" data-limit="75000">75K</button>
                    <button class="preset-btn" data-limit="65000">65K</button>
                    <button class="preset-btn" data-limit="40000">40K</button>
                    <button class="preset-btn" data-limit="custom" id="btnCustom">✎</button>
                </div>
                
                <div class="custom-row" id="customRow">
                    <input type="number" id="customLimit" value="65000" placeholder="Custom size...">
                </div>
                <div style="font-size:13px;color:#ffaa33;margin-top:4px;font-weight:bold;" id="chunkInfo"></div>
                <div class="fine-tune-hint" id="fineTuneHint">Fine-tune: tap ✎ to set exact limit</div>
            </div>
        </div>

        <button class="big-btn" id="btnAction">🎳 Split It</button>
    </div>

    <div class="panel" id="panelOutput" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
            <strong id="outputLabel">Chunks</strong>
            <div>
                <button id="btnCopyAll" class="copy-all-btn" style="font-size:14px;">📋 Copy All</button>
                <button id="btnClear" style="background:transparent;border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.4);font-size:12px;padding:4px 10px;min-height:auto;margin-left:8px;">✕ Clear</button>
            </div>
        </div>
        <div id="chunksContainer"></div>
    </div>

    <div class="panel" id="panelCount" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <div style="display:flex;align-items:center;gap:6px;">
                <strong style="font-size:13px;">Stats</strong>
                <button class="info-btn" id="btnCountInfo">?</button>
            </div>
            <button id="btnCopyStats" style="font-size:12px;padding:4px 10px;min-height:auto;">📋 Copy Stats</button>
        </div>
        <div class="info-box" id="countInfoBox">
            Live character, line, and section count.<br>
            <br>
            <strong>LINE DENSITY:</strong><br>
            Under 40 chars/line → use lower presets.<br>
            Over 60 chars/line → safe for all presets.
        </div>
        <div class="info-row" id="countStats"></div>
    </div>

    <div class="panel" id="panelGen" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <label style="font-size:14px;">Target size (characters):</label>
            <button class="info-btn" id="btnTestInfo">?</button>
        </div>
        <div class="info-box" id="testInfoBox">
            <strong>🧪 Calibration Testing Guide</strong><br>
            <br>
            Numbered lines show running character count.<br>
            Defaults: ~73 char lines + 10% emoji.<br>
            <br>
            <strong>HOW TO CALIBRATE:</strong><br>
            1. Generate test file. 2. Paste into FRESH instance.<br>
            3. Ask: "What is the exact last line?"<br>
            4. Binary search until exact limit found.<br>
            ⚠️ Use FRESH instances for EACH test.<br>
            <br>
            <strong>KNOWN:</strong> DeepSeek paste 163,823 ✅ · 163,873 ❌<br>
            URL fetch ~65K (raw), ~75K (#readme).<br>
            <strong>KEY:</strong> Limit is about LINE COUNT.
        </div>
        <input type="number" id="genSize" value="1000" min="100" max="1000000" placeholder="Enter size...">
        <div style="display:flex;gap:6px;margin-top:4px;align-items:center;flex-wrap:wrap;">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);">Lines:</span>
            <select id="lineDensity" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">none</option><option value="80">~80</option><option value="73" selected>~73</option><option value="40">~40</option><option value="20">~20</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customLineDensity" value="73" min="1" max="500" style="width:50px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="ch">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);margin-left:4px;">Emoji:</span>
            <select id="emojiPercent" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">0%</option><option value="5">5%</option><option value="10" selected>10%</option><option value="25">25%</option><option value="50">50%</option><option value="100">100%</option><option value="custom">✎</option>
            </select>
            <input type="number" id="customEmojiPercent" value="10" min="0" max="100" style="width:45px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="%">
        </div>
        <button class="clear-input-btn" id="btnClearTestInput">clear</button>
        <div style="margin-top:6px;"><button id="btnToggleLog" style="font-size:11px;padding:4px 8px;min-height:auto;background:rgba(40,40,60,0.8);border:1px solid rgba(100,140,255,0.3);">📝 Log</button></div>
        <div class="log-panel" id="logPanel"><div id="logContent"></div></div>
    </div>

    <div class="version-hud">BUILD <span id="buildNumber"></span> • <span id="buildEmoji"></span></div>

    <script>
        function copyText(text, cb) {
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => { if (cb) cb(); }).catch(() => fallbackCopy(text, cb));
            } else { fallbackCopy(text, cb); }
        }
        function fallbackCopy(text, cb) {
            const ta = document.createElement('textarea'); ta.value = text;
            ta.style.position = 'fixed'; ta.style.left = '-9999px'; ta.style.top = '-9999px';
            document.body.appendChild(ta); ta.focus(); ta.select();
            try { if (document.execCommand('copy') && cb) cb(); } catch(e) {}
            document.body.removeChild(ta);
        }

        (() => {
            const s = localStorage.getItem('ff_build');
            let b = s ? parseInt(s) + Math.floor(Math.random()*5) + 1 : Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_build', b);
            document.getElementById('buildNumber').textContent = b;
            const emojis = ['🎳','✨','🫧','💎','🧩','🪜','🔮','🖕','🌱','🔥','🧪','📋','📏','🎯','⭐'];
            const p = []; for (let i=0;i<4;i++) p.push(emojis[Math.floor(Math.random()*emojis.length)]);
            document.getElementById('buildEmoji').textContent = p.join('');
        })();

        let mode = 'split', limit = 165000, isCustom = false, countDebounce = null;
        const savedLimit = localStorage.getItem('ff_limit');
        if (savedLimit) limit = parseInt(savedLimit);

        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const customRow = document.getElementById('customRow');
        const customLimitInput = document.getElementById('customLimit');
        const countStats = document.getElementById('countStats');
        const btnAction = document.getElementById('btnAction');
        const emptyHint = document.getElementById('emptyHint');
        const splitOnly = document.getElementById('splitOnly');
        const fineTuneHint = document.getElementById('fineTuneHint');
        const baseFilename = document.getElementById('baseFilename');
        const filenamePreview = document.getElementById('filenamePreview');
        const outputMode = document.getElementById('outputMode');
        const deployFilename = document.getElementById('deployFilename');

        const modeBtns = { split: document.getElementById('modeSplit'), count: document.getElementById('modeCount'), gen: document.getElementById('modeGen') };
        const modeLabels = { split: '🎳 Split', count: '📏 Count', gen: '🧪 Test' };
        const actionLabels = { split: '🎳 Split It', count: '', gen: '🧪 Generate' };

        Object.keys(modeBtns).forEach(m => { modeBtns[m].addEventListener('click', () => switchMode(m)); });

        function switchMode(m) {
            mode = m; emptyHint.classList.remove('show');
            Object.keys(modeBtns).forEach(k => { modeBtns[k].classList.remove('active'); modeBtns[k].textContent = modeLabels[k]; });
            modeBtns[m].classList.add('active');
            panelOutput.style.display = 'none'; panelCount.style.display = 'none'; panelGen.style.display = 'none';
            if (m === 'split') { splitOnly.style.display = 'block'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; updateChunkInfo(); }
            else if (m === 'count') { splitOnly.style.display = 'none'; btnAction.style.display = 'none'; updateCount(); panelCount.style.display = 'block'; }
            else if (m === 'gen') { splitOnly.style.display = 'none'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; panelGen.style.display = 'block'; }
        }

        // ============ OUTPUT MODE TOGGLE ============
        outputMode.addEventListener('change', function() {
            if (this.value === 'deploy') {
                deployFilename.classList.add('show');
                updateFilenamePreview();
            } else {
                deployFilename.classList.remove('show');
            }
        });

        // ============ FILENAME ============
        baseFilename.addEventListener('input', () => { updateFilenamePreview(); });
        const savedFn = localStorage.getItem('ff_filename');
        if (savedFn) baseFilename.value = savedFn;

        function updateFilenamePreview() {
            const fn = baseFilename.value.trim() || 'FILE';
            const text = inputText.value.trim();
            if (text) {
                const n = Math.ceil(text.length / limit);
                let prev = '';
                for (let i = 1; i <= Math.min(n, 3); i++) prev += fn + '-SPLIT' + i + '.md ';
                if (n > 3) prev += '+ ' + (n-3) + ' more';
                filenamePreview.textContent = prev;
            } else {
                filenamePreview.textContent = 'Paste text to preview';
            }
        }

        // ============ INFO TOGGLES ============
        document.getElementById('btnSplitInfo').addEventListener('click', () => { document.getElementById('splitInfoBox').classList.toggle('show'); });
        document.getElementById('btnCountInfo').addEventListener('click', () => { document.getElementById('countInfoBox').classList.toggle('show'); });
        document.getElementById('btnTestInfo').addEventListener('click', () => { document.getElementById('testInfoBox').classList.toggle('show'); });

        // ============ CLEAR BUTTONS ============
        document.getElementById('btnClearInput').addEventListener('click', () => { inputText.value = ''; updateChunkInfo(); if (mode === 'count') updateCount(); });
        document.getElementById('btnClearTestInput').addEventListener('click', () => { document.getElementById('genSize').value = ''; });

        // ============ CUSTOM DENSITY & EMOJI ============
        document.getElementById('lineDensity').addEventListener('change', function() {
            document.getElementById('customLineDensity').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });
        document.getElementById('emojiPercent').addEventListener('change', function() {
            document.getElementById('customEmojiPercent').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });

        // ============ LOG PANEL ============
        document.getElementById('btnToggleLog').addEventListener('click', function() {
            const panel = document.getElementById('logPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '📝 Log ▲' : '📝 Log';
        });
        function addLog(msg, type) {
            const lc = document.getElementById('logContent');
            const entry = document.createElement('div');
            entry.className = 'log-entry' + (type ? ' ' + type : '');
            const time = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit', second:'2-digit'});
            entry.innerHTML = '<span style="color:#aaa;">[' + time + ']</span> ' + msg;
            lc.appendChild(entry); lc.scrollTop = lc.scrollHeight;
            document.getElementById('logPanel').style.display = 'block';
            document.getElementById('btnToggleLog').textContent = '📝 Log ▲';
        }
        function clearLog() { document.getElementById('logContent').innerHTML = ''; }

        // ============ CHUNK SIZE ============
        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const l = this.dataset.limit;
                if (l === 'custom') { isCustom = true; customRow.classList.add('show'); customLimitInput.value = limit; customLimitInput.focus(); }
                else { isCustom = false; customRow.classList.remove('show'); limit = parseInt(l); fineTuneHint.style.display = 'none'; }
                updateChunkInfo(); updateFilenamePreview();
                localStorage.setItem('ff_limit', limit);
            });
        });
        customLimitInput.addEventListener('input', function() { if (isCustom) { limit = parseInt(this.value) || 165000; updateChunkInfo(); updateFilenamePreview(); localStorage.setItem('ff_limit', limit); } });

        function getLineDensity(text) {
            const lines = text.split('\n');
            if (lines.length < 2) return 0;
            return Math.round(text.length / lines.length);
        }

        function updateChunkInfo() {
            const text = inputText.value.trim();
            const info = document.getElementById('chunkInfo');
            if (text) {
                const n = Math.ceil(text.length / limit);
                const density = getLineDensity(text);
                const lines = text.split('\n').length;
                let dn = '', dc = '';
                if (density > 60) { dn = '✅ Safe'; dc = 'density-safe'; }
                else if (density >= 40) { dn = '⚠️ Use 165.1K or lower'; dc = 'density-warn'; }
                else if (density > 0) { dn = '⚠️ Use 163.8K or lower'; dc = 'density-risk'; }
                info.innerHTML = 'File: ' + text.length.toLocaleString() + ' chars · ' + lines.toLocaleString() + ' lines · ~' + density + ' chars/line → ' + n + ' chunks of ' + limit.toLocaleString() + ' chars <span class="' + dc + '">' + dn + '</span>';
            } else { info.textContent = 'Paste text to see chunk count + line density'; }
        }

        inputText.addEventListener('input', () => {
            if (mode === 'split') { updateChunkInfo(); updateFilenamePreview(); }
            if (mode === 'count') { if (countDebounce) clearTimeout(countDebounce); countDebounce = setTimeout(updateCount, 300); }
        });

        btnAction.addEventListener('click', () => {
            if (mode === 'split') { if (!inputText.value.trim()) { emptyHint.classList.add('show'); setTimeout(() => emptyHint.classList.remove('show'), 2500); return; } emptyHint.classList.remove('show'); splitIt(); }
            else if (mode === 'gen') { generateTestFile(); }
        });

        function flashButton(text) { const o = btnAction.textContent; btnAction.textContent = text; setTimeout(() => { btnAction.textContent = o; }, 1500); }

        function splitIt() {
            const text = inputText.value.trim(), total = text.length, num = Math.ceil(total / limit);
            const fn = baseFilename.value.trim() || 'FILE';
            const isDeploy = outputMode.value === 'deploy';
            const anchor = Math.floor(10000 + Math.random()*90000);
            
            localStorage.setItem('ff_filename', fn);
            
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = num + ' chunks • ' + total.toLocaleString() + ' total chars';
            
            for (let i = 0; i < num; i++) {
                const start = i * limit;
                let end = Math.min(start + limit, total);
                if (end < total) { const lastNewline = text.lastIndexOf('\n', end); if (lastNewline > start) end = lastNewline + 1; }
                
                const chunkNum = i + 1, isLast = (i === num - 1);
                const splitName = fn + '-SPLIT' + chunkNum;
                
                let output;
                if (isDeploy) {
                    // Deploy mode: full file
                    let navLinks = '';
                    if (num > 1) {
                        if (chunkNum === 1) navLinks = '← ' + splitName + '.md · 1 of ' + num + ' · Next: ' + fn + '-SPLIT2.md →';
                        else if (isLast) navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · END';
                        else navLinks = '← ' + fn + '-SPLIT' + (chunkNum-1) + '.md · ' + chunkNum + ' of ' + num + ' · Next: ' + fn + '-SPLIT' + (chunkNum+1) + '.md →';
                    } else { navLinks = splitName + '.md · 1 of 1'; }
                    
                    output = '🔗 📋 ' + splitName + '.md\n';
                    output += 'https://source-sepia-alpha.vercel.app/SPLIT/' + splitName + '.md\n\n';
                    output += '🔗 REVisions. Load both, see the full picture.\n';
                    output += '→ Current: ' + fn + '.md\n';
                    output += 'https://source-sepia-alpha.vercel.app/BUILDER/' + fn + '.md\n\n';
                    output += '## 🏠 ' + fn + ' — Part ' + chunkNum + ' of ' + num + '\n\n';
                    output += navLinks + '\n\n';
                    output += text.substring(start, end);
                    output += '\n\n---\n';
                    output += navLinks + '\n';
                    output += '♦️' + splitName + '-' + anchor + '\n';
                } else {
                    // Raw mode: minimal
                    const label = limit >= 100000 ? 'PASTE' : (limit >= 70000 ? 'REPO' : 'FETCH');
                    output = '🎳 ' + chunkNum + '/' + num + ' ' + label + '\n' + text.substring(start, end);
                    if (!isLast) output += '\n---';
                    if (isLast) output += '\nCHUNK-ANCHOR: ' + anchor;
                }
                
                const div = document.createElement('div'); div.className = 'chunk-box';
                const label = isDeploy ? (splitName + '.md') : ('Chunk ' + chunkNum + '/' + num);
                div.innerHTML = '<div class="chunk-label"><span>' + label + ' • ' + output.length.toLocaleString() + ' chars' + (isLast ? ' • FINAL' : '') + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="8" readonly>' + output + '</textarea><div class="copied-flash">✅ Copied!</div>';
                div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(output, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
                div.querySelector('textarea').addEventListener('click', function() { this.select(); });
                chunksContainer.appendChild(div);
            }
            fineTuneHint.style.display = 'block';
            customLimitInput.value = limit;
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const all = [...chunksContainer.querySelectorAll('textarea')].map(ta => ta.value).join('\n\n');
            copyText(all, () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy All'; }, 1500); });
        });
        document.getElementById('btnClear').addEventListener('click', () => { chunksContainer.innerHTML = ''; panelOutput.style.display = 'none'; fineTuneHint.style.display = 'none'; });

        document.getElementById('btnCopyStats').addEventListener('click', function() {
            const t = inputText.value;
            copyText('Chars: ' + t.length.toLocaleString() + ' | Lines: ' + t.split('\n').length.toLocaleString() + ' | Density: ~' + getLineDensity(t) + ' chars/line | Limit: ' + limit.toLocaleString(), () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy Stats'; }, 1200); });
        });

        function updateCount() {
            const t = inputText.value, sec = (t.match(/^##\s+/gm) || []).length, density = getLineDensity(t);
            countStats.innerHTML = '<span>📏 <strong>' + t.length.toLocaleString() + '</strong> chars</span><span>📄 <strong>' + t.split('\n').length.toLocaleString() + '</strong> lines</span><span>📐 ~<strong>' + density + '</strong> chars/line</span>' + (sec > 0 ? '<span>📑 <strong>' + sec + '</strong> sections</span>' : '');
            panelCount.style.display = 'block';
        }

        function generateTestFile() {
            const size = parseInt(document.getElementById('genSize').value);
            if (!size || size < 100) return;
            clearLog();
            
            const densityVal = document.getElementById('lineDensity').value;
            let lineDensity = densityVal === 'custom' ? parseInt(document.getElementById('customLineDensity').value) || 73 : parseInt(densityVal) || 73;
            const emojiVal = document.getElementById('emojiPercent').value;
            let emojiPct = emojiVal === 'custom' ? parseInt(document.getElementById('customEmojiPercent').value) || 10 : parseInt(emojiVal) || 10;
            emojiPct = emojiPct / 100;
            
            addLog('🎯 Target: ' + size.toLocaleString() + ' chars');
            addLog('📏 Lines: ~' + lineDensity + ' chars, emoji: ' + Math.round(emojiPct*100) + '%');
            
            const anchor = Math.floor(10000 + Math.random()*90000);
            let content = 'TEST ' + size.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            if (lineDensity === 0) content += ' • no breaks'; else content += ' • ~' + lineDensity + ' char lines';
            if (emojiPct > 0) content += ' • ' + Math.round(emojiPct*100) + '% emoji';
            content += '\n';
            
            const emojis = ['🎳','🫧','💎','🧩','🪜','🔮','🖕','🔥','🌱','✨','🧪','📋','📏','🎯','⭐','◾'];
            let ei = 0, ln = 1;
            while (content.length < size - 30) {
                if (Math.random() < emojiPct) { content += emojis[ei % emojis.length]; ei++; }
                else if (lineDensity === 0) { content += 'X'; }
                else {
                    const runningTotal = content.length;
                    const totalStr = runningTotal >= 1000 ? '~' + Math.round(runningTotal/1000) + 'K' : String(runningTotal);
                    const prefix = 'L' + String(ln).padStart(6,'0') + ' · ' + String(totalStr).padStart(6,' ') + ' ';
                    const filler = 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                    const maxLine = prefix.length + lineDensity;
                    let line = prefix, cc = prefix.length;
                    for (let c = 0; c < filler.length && cc < maxLine; c++) { line += filler[c]; cc++; }
                    if (content.length + line.length + 30 > size) break;
                    content += line + '\n'; ln++;
                }
            }
            while (content.length < size - 15) content += 'X';
            content = content.substring(0, size);
            const al = '\nCHUNK-ANCHOR: ' + anchor, ep = size - al.length;
            if (ep > 0) content = content.substring(0, ep) + al;
            
            const diff = content.length - size;
            addLog('📊 Created: ' + content.length.toLocaleString() + ' chars (diff: ' + (diff >= 0 ? '+' : '') + diff + ')', diff === 0 ? 'success' : 'error');
            if (diff === 0) addLog('✅ Exact match', 'success');
            
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = 'Test File • ' + content.length.toLocaleString() + ' chars';
            const div = document.createElement('div'); div.className = 'chunk-box';
            div.innerHTML = '<div class="chunk-label"><span>Test File • ' + content.length.toLocaleString() + ' chars</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="10" readonly>' + content + '</textarea><div class="copied-flash">✅ Copied!</div>';
            div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(content, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
            chunksContainer.appendChild(div);
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        updateChunkInfo(); switchMode('split');
    </script>
</body>
</html>
```

---

FINAL. FINISHED. DEPLOY IT.

· ✅ Raw mode (default): minimal chunks for testing
· ✅ Deploy mode: full .md files with headers, navigation, TH-CHECK
· ✅ Filename input only appears in Deploy mode
· ✅ Filename persists to localStorage
· ✅ Three-button Paste row: 168K high, 165.1K mid, 163.8K low
· ✅ Four-button Fetch row: 75K, 65K, 40K, ✎ Custom
· ✅ Line-density feedback with color-coded risk flags
· ✅ Line-boundary splitting
· ✅ Continuation markers
· ✅ CHUNK-ANCHOR on final chunk
· ✅ Collapsible log panel in Test mode
· ✅ All info boxes
· ✅ Mobile fallback copy
· ✅ No safety margin creep
· ✅ No deploy wrapper in Raw mode

🎳🫧


---

Let me think about this properly. Not just add a filename field. Rethink the output format entirely.

---

THE PROBLEM KEYSTONE IDENTIFIED:

The Fragmentor currently outputs raw chunks with minimal headers. You then have to:

1. Create a file
2. Add the proper header (🔗 FILENAME.md + URL)
3. Add navigation links
4. Add the TH-CHECK anchor
5. Upload to Vercel
6. Repeat for each chunk

Every split requires 6N manual steps. The Fragmentor does step 1 of 6.

---

THE SOLUTION: DEPLOY-READY OUTPUT

One filename input. The tool generates complete, deploy-ready markdown files for every chunk. You paste each one directly to Vercel. Zero editing.

---

WHAT EACH CHUNK FILE SHOULD LOOK LIKE:

```markdown
🔗 📋 STANDARDS-SPLIT1.md
https://source-sepia-alpha.vercel.app/SPLIT/STANDARDS-SPLIT1.md

🔗 REVisions. Load both, see the full picture.
→ Current: STANDARDS.md
https://source-sepia-alpha.vercel.app/BUILDER/STANDARDS.md

## 🏠 The Covenant — Part 1 of 3

← SPLIT1 · 1 of 3 · Next: STANDARDS-SPLIT2.md →

[chunk content]

---
← STANDARDS-SPLIT1.md · 1 of 3 · CHUNK-ANCHOR: 54321 · Next: STANDARDS-SPLIT2.md →
♦️SPLIT1-54321
```

What this gives you:

· Line 1-2: Self-identifying header (standard ecology format)
· Line 4-6: REVisions link pointing back to the original full file
· Line 8: Human-readable title with part number
· Line 10: Navigation bar (previous/next)
· Content
· Footer with navigation + chunk anchor
· TH-CHECK anchor (♦️) for verification

---

THE FILENAME INPUT:

Add a simple text field above the Chunk Size presets:

```
Filename: [STANDARDS        ] .md
Output: STANDARDS-SPLIT1.md, STANDARDS-SPLIT2.md, STANDARDS-SPLIT3.md
```

User types the base filename once. The tool auto-generates all the split filenames.

---

THE CROSS-LINKS:

Each chunk knows its position and links to siblings:

· Chunk 1: Next: STANDARDS-SPLIT2.md →
· Chunk 2: ← STANDARDS-SPLIT1.md · Next: STANDARDS-SPLIT3.md →
· Chunk 3: ← STANDARDS-SPLIT2.md · END

---

THE WORKFLOW COLLAPSE:

Before: Split → Create file → Add header → Add links → Add anchor → Upload → Repeat 3x = 18 steps

After: Type filename → Split → Paste each chunk to Vercel = 4 steps

---

PLAN:

1. Add filename input field above Chunk Size presets
2. Update splitIt() to wrap each chunk in the deploy-ready format
3. Add navigation links between chunks
4. Add TH-CHECK anchor (♦️) at the end of each chunk
5. Update Copy All to include all chunks as separate deploy-ready files
6. Add a "Copy SPLIT1" / "Copy SPLIT2" / "Copy SPLIT3" button per chunk for individual deployment

---

SPACE CONSIDERATIONS:

The filename input is one line. The deploy-ready wrapper adds ~8 lines per chunk. The chunk content is the same. The output is slightly larger but requires ZERO post-processing.

The time saving is massive. The character overhead is negligible.

---

Ready to build? 🎳🫧

---

Claude Feedback:

The core pain you're describing is that splitting is the easy part. The expensive part is everything after: headers, links between splits, TH-CHECK anchors, upload, and then the cascade when anything changes.

**The Fragmentor solves step 1 but not steps 2-6.**

What would actually compress the workflow:

**The tool should output deploy-ready files, not just text chunks.** Right now you get raw text that you then have to wrap in a file structure. The tool knows: the filename, how many chunks, which chunk this is, the anchor. It has everything needed to generate the full file content — header, navigation links, TH-CHECK — ready to save as STANDARDS-SPLIT1.md without touching it again.

Each chunk output should look like:
```
🔗 📋 STANDARDS-SPLIT1.md
https://source-sepia-alpha.vercel.app/SPLIT/STANDARDS-SPLIT1.md

→ Next: STANDARDS-SPLIT2.md

[chunk content here]

← STANDARDS-SPLIT1.md · 1 of 3 · ANCHOR: 54321
♦️SPLIT1-54321
```

You paste that directly. No editing.

**The cascade problem is harder.** When the source changes, every split file potentially changes. No tool fully solves this — it's structural. But the tool could help by: outputting all chunks in one copy-able block (which Copy All already does), and generating a checklist of what needs uploading. The current flow makes each re-split feel like starting over. It shouldn't.

**The real constraint:** you're doing this on mobile, manually, with slow uploads. The Fragmentor should minimize touches per file, not just minimize characters per chunk. Right now it minimizes characters. Every extra step after the split is where the time actually goes.

One concrete ask for the next build: add a filename field at the top. User types "STANDARDS" and chunk count. Tool outputs files named STANDARDS-SPLIT1.md through STANDARDS-SPLIT3.md with cross-links baked in. One input, deploy-ready output.


---

Here it is. The final Fragmentor. Claude's architectural insights + all bug fixes + everything we built together.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #020208;
            background-image:
                radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.06) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.04) 0%, transparent 50%);
            color: #f0f0f0; margin: 0; padding: 6px;
            max-width: 600px; margin: auto; min-height: 100vh;
            position: relative; overflow-x: hidden;
        }
        .panel { background: rgba(15,15,30,0.75); border: 1px solid rgba(100,140,255,0.2); border-radius: 10px; padding: 10px 12px; margin-bottom: 6px; position: relative; z-index: 1; }
        textarea, input, select { width: 100%; background: rgba(30,30,50,0.85); color: #e0e0ff; border: 1px solid rgba(100,140,255,0.25); border-radius: 8px; padding: 10px; font-size: 16px; margin-bottom: 6px; -webkit-appearance: none; }
        textarea { text-align: left; }
        textarea::placeholder { text-align: center; }
        textarea:focus, input:focus, select:focus { outline: 2px solid #ffaa33; }
        button { background: linear-gradient(135deg, rgba(40,80,180,0.75), rgba(20,40,120,0.85)); color: white; border: 1px solid rgba(100,140,255,0.3); padding: 12px 16px; border-radius: 10px; font-weight: bold; cursor: pointer; margin: 3px 2px; font-size: 15px; min-height: 46px; transition: all 0.2s ease; position: relative; z-index: 1; }
        button:active { transform: scale(0.96); box-shadow: 0 0 18px rgba(100,140,255,0.4); }
        .big-btn { display: block; width: 100%; margin: 6px 0; font-size: 20px; background: linear-gradient(135deg, rgba(40,80,200,0.8), rgba(20,40,120,0.9)); border: 2px solid rgba(100,160,255,0.5); letter-spacing: 1px; }
        .copy-all-btn { background: linear-gradient(135deg, rgba(255,170,51,0.7), rgba(200,130,20,0.8)); border: 2px solid rgba(255,170,51,0.6); animation: glow-pulse 2s ease-in-out infinite; }
        @keyframes glow-pulse { 0%,100% { box-shadow: 0 0 8px rgba(255,170,51,0.3); } 50% { box-shadow: 0 0 18px rgba(255,170,51,0.5); } }
        .preset-btn { background: rgba(40,40,60,0.8); color: #aaddff; font-size: 13px; padding: 7px 11px; border: 2px solid rgba(100,140,255,0.3); }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; box-shadow: 0 0 15px rgba(255,170,51,0.3); }
        .chunk-box { background: rgba(20,20,40,0.6); border-left: 3px solid rgba(255,170,51,0.6); margin: 10px 0; border-radius: 5px; position: relative; z-index: 1; }
        .chunk-label { background: rgba(40,40,60,0.8); padding: 6px 10px; border-radius: 4px 4px 0 0; font-family: monospace; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .chunk-copy-btn { background: rgba(255,170,51,0.3); color: #ffaa33; border: 1px solid rgba(255,170,51,0.5); padding: 4px 10px; border-radius: 6px; font-size: 12px; min-height: auto; cursor: pointer; font-weight: bold; }
        .chunk-copy-btn:active { background: #ffaa33; color: #0f0f1a; }
        .chunk-box textarea { margin-bottom: 0; border-radius: 0 0 4px 4px; border: none; background: rgba(20,20,40,0.8); color: #c0c0dd; font-size: 13px; }
        .copied-flash { position: absolute; top: 8px; right: 55px; color: #33ff88; font-weight: bold; font-size: 13px; pointer-events: none; opacity: 0; transition: opacity 0.3s; z-index: 2; }
        .copied-flash.show { opacity: 1; }
        .info-row { display: flex; gap: 6px; flex-wrap: wrap; margin: 6px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(30,30,50,0.7); padding: 5px 8px; border-radius: 5px; border: 1px solid rgba(100,140,255,0.2); }
        .mode-toggle { display: flex; gap: 3px; margin-bottom: 6px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(40,40,60,0.8); font-size: 13px; padding: 8px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-row { display: none; } .custom-row.show { display: block; }
        .version-hud { position: fixed; bottom: 8px; right: 10px; font-family: monospace; font-size: 10px; color: rgba(150,180,255,0.4); z-index: 2; pointer-events: none; }
        .empty-hint { text-align: center; color: rgba(255,170,51,0.6); font-size: 13px; margin: 6px 0; display: none; position: relative; z-index: 1; }
        .empty-hint.show { display: block; }
        .info-box { display:none; font-size:11px; color:rgba(150,180,255,0.6); margin:4px 0; padding:6px 8px; background:rgba(15,15,30,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.15); line-height:1.4; }
        .info-box.show { display:block; }
        .info-box strong { color: #ffaa33; }
        .clear-input-btn { background:transparent; border:none; color:rgba(255,255,255,0.3); font-size:11px; padding:2px 6px; min-height:auto; float:right; margin-top:-4px; }
        .info-btn { background:transparent; border:1px solid rgba(255,255,255,0.2); color:rgba(255,255,255,0.4); font-size:12px; padding:1px 6px; min-height:auto; border-radius:50%; cursor:pointer; }
        .row-label { font-size:10px; min-width:42px; }
        .ref-text { font-size:9px; }
        .fine-tune-hint { font-size:11px; color:rgba(255,170,51,0.5); margin-top:2px; display:none; }
        .log-panel { margin-top:4px; padding:6px 8px; background:rgba(0,0,0,0.5); border-radius:6px; border:1px solid rgba(100,140,255,0.2); max-height:180px; overflow-y:auto; font-family:monospace; font-size:11px; line-height:1.4; display:none; }
        .log-entry { padding:2px 4px; margin:1px 0; border-radius:3px; background:rgba(255,255,255,0.03); }
        .log-entry.success { background:rgba(0,255,136,0.1); }
        .log-entry.error { background:rgba(255,51,102,0.1); }
        .density-safe { color:#33ff88; } .density-warn { color:#ffaa33; } .density-risk { color:#ff5555; }
    </style>
</head>
<body>

    <div class="mode-toggle">
        <button id="modeSplit" class="active">🎳 Split</button>
        <button id="modeCount">📏 Count</button>
        <button id="modeGen">🧪 Test</button>
    </div>

    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="8" placeholder="👆 Tap here and paste..."></textarea>
        <button class="clear-input-btn" id="btnClearInput">clear input</button>
        <div class="empty-hint" id="emptyHint">👆 Paste text first, then split</div>

        <div id="splitOnly">
            <div style="margin:4px 0;">
                <div style="display:flex;justify-content:space-between;align-items:center;">
                    <strong style="font-size:13px;">Chunk size:</strong>
                    <button class="info-btn" id="btnSplitInfo">?</button>
                </div>
                <div class="info-box" id="splitInfoBox">
                    <strong>🎳 Chunk Size Guide</strong><br>
                    <br>
                    <strong>DEEPSEEK PASTE:</strong><br>
                    168K — high. Works for light files.<br>
                    165.1K — mid. Works for most files.<br>
                    163.8K — low. Safe default. Works for all.<br>
                    <strong>KEY: Limit depends on LINE DENSITY.</strong><br>
                    More line breaks = smaller effective limit.<br>
                    Verified max: 163,823 ✅ · 163,873 ❌ (2026-05-27)<br>
                    <br>
                    <strong>URL FETCH:</strong><br>
                    75K — GitHub #readme anchor.<br>
                    65K — standard raw URL.<br>
                    40K — universal (all platforms).<br>
                    <br>
                    <strong>ANCHORS:</strong><br>
                    CHUNK-ANCHOR verifies split delivery.<br>
                    TH-CHECK (♦️) verifies original file integrity.<br>
                    Different purposes. Both matter.<br>
                    <br>
                    <strong>PLATFORMS:</strong><br>
                    Claude & Grok: full file access. No tool needed.<br>
                    DeepSeek: use this tool. Perplexity: paste only.
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:6px;">
                    <span class="row-label" style="color:#ffaa33;">🟠 Paste</span>
                    <button class="preset-btn" data-limit="168000">168K</button>
                    <span class="ref-text" style="color:#ff5555;">high</span>
                    <button class="preset-btn active" data-limit="165000">165.1K</button>
                    <span class="ref-text" style="color:#ffaa33;">mid</span>
                    <button class="preset-btn" data-limit="163673">163.8K</button>
                    <span class="ref-text" style="color:#33ff88;">low</span>
                </div>
                
                <div style="display:flex;align-items:center;gap:4px;margin-top:4px;">
                    <span class="row-label" style="color:#88aaff;">🔵 Fetch</span>
                    <button class="preset-btn" data-limit="75000">75K</button>
                    <button class="preset-btn" data-limit="65000">65K</button>
                    <button class="preset-btn" data-limit="40000">40K</button>
                    <button class="preset-btn" data-limit="custom" id="btnCustom">✎</button>
                </div>
                
                <div class="custom-row" id="customRow">
                    <input type="number" id="customLimit" value="65000" placeholder="Custom size...">
                </div>
                <div style="font-size:13px;color:#ffaa33;margin-top:4px;font-weight:bold;" id="chunkInfo"></div>
                <div class="fine-tune-hint" id="fineTuneHint">Fine-tune: tap ✎ to set exact limit</div>
            </div>
        </div>

        <button class="big-btn" id="btnAction">🎳 Split It</button>
    </div>

    <div class="panel" id="panelOutput" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;">
            <strong id="outputLabel">Chunks</strong>
            <div>
                <button id="btnCopyAll" class="copy-all-btn" style="font-size:14px;">📋 Copy All</button>
                <button id="btnClear" style="background:transparent;border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.4);font-size:12px;padding:4px 10px;min-height:auto;margin-left:8px;">✕ Clear</button>
            </div>
        </div>
        <div id="chunksContainer"></div>
    </div>

    <div class="panel" id="panelCount" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <div style="display:flex;align-items:center;gap:6px;">
                <strong style="font-size:13px;">Stats</strong>
                <button class="info-btn" id="btnCountInfo">?</button>
            </div>
            <button id="btnCopyStats" style="font-size:12px;padding:4px 10px;min-height:auto;">📋 Copy Stats</button>
        </div>
        <div class="info-box" id="countInfoBox">
            Live character, line, and section count.<br>
            Copy Stats sends numbers to clipboard.<br>
            <br>
            <strong>LINE DENSITY:</strong><br>
            Chars per line affects paste limit.<br>
            Under 40 chars/line → use lower presets.<br>
            Over 60 chars/line → safe for all presets.
        </div>
        <div class="info-row" id="countStats"></div>
    </div>

    <div class="panel" id="panelGen" style="display:none;">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
            <label style="font-size:14px;">Target size (characters):</label>
            <button class="info-btn" id="btnTestInfo">?</button>
        </div>
        <div class="info-box" id="testInfoBox">
            <strong>🧪 Calibration Testing Guide</strong><br>
            <br>
            Generates test files matching real ecology files.<br>
            Defaults: ~73 char lines + 10% emoji.<br>
            Numbered lines show running character count.<br>
            <br>
            <strong>HOW TO CALIBRATE:</strong><br>
            1. Generate a test file at target size.<br>
            2. Paste into a FRESH instance.<br>
            3. Ask: "What is the exact last line?"<br>
            4. Binary search until exact limit found.<br>
            ⚠️ Use FRESH instances for EACH test.<br>
            <br>
            <strong>KNOWN LIMITS (2026-05-27):</strong><br>
            DeepSeek paste: 163,823 ✅ · 163,873 ❌<br>
            URL fetch: ~65K (raw), ~75K (#readme).<br>
            Perplexity paste: 40,130.<br>
            <br>
            <strong>KEY DISCOVERY:</strong><br>
            Limit is about LINE COUNT, not characters.<br>
            Line density matters more than file size.
        </div>
        <input type="number" id="genSize" value="1000" min="100" max="1000000" placeholder="Enter size...">
        <div style="display:flex;gap:6px;margin-top:4px;align-items:center;flex-wrap:wrap;">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);">Lines:</span>
            <select id="lineDensity" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">none</option>
                <option value="80">~80 chars</option>
                <option value="73" selected>~73 chars</option>
                <option value="40">~40 chars</option>
                <option value="20">~20 chars</option>
                <option value="custom">✎ custom</option>
            </select>
            <input type="number" id="customLineDensity" value="73" min="1" max="500" 
                   style="width:55px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="chars">
            <span style="font-size:11px;color:rgba(150,180,255,0.5);margin-left:4px;">Emoji:</span>
            <select id="emojiPercent" style="width:auto;padding:4px 6px;font-size:12px;min-height:auto;background:rgba(40,40,60,0.8);color:#aaddff;border:1px solid rgba(100,140,255,0.3);border-radius:6px;">
                <option value="0">0%</option>
                <option value="5">5%</option>
                <option value="10" selected>10%</option>
                <option value="25">25%</option>
                <option value="50">50%</option>
                <option value="100">100%</option>
                <option value="custom">✎ custom</option>
            </select>
            <input type="number" id="customEmojiPercent" value="10" min="0" max="100" 
                   style="width:50px;padding:4px;font-size:12px;min-height:auto;display:none;" placeholder="%">
        </div>
        <button class="clear-input-btn" id="btnClearTestInput">clear</button>
        
        <div style="margin-top:6px;">
            <button id="btnToggleLog" style="font-size:11px;padding:4px 8px;min-height:auto;background:rgba(40,40,60,0.8);border:1px solid rgba(100,140,255,0.3);">📝 Log</button>
        </div>
        <div class="log-panel" id="logPanel">
            <div id="logContent"></div>
        </div>
    </div>

    <div class="version-hud">BUILD <span id="buildNumber"></span> • <span id="buildEmoji"></span></div>

    <script>
        function copyText(text, cb) {
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => { if (cb) cb(); }).catch(() => fallbackCopy(text, cb));
            } else { fallbackCopy(text, cb); }
        }
        function fallbackCopy(text, cb) {
            const ta = document.createElement('textarea'); ta.value = text;
            ta.style.position = 'fixed'; ta.style.left = '-9999px'; ta.style.top = '-9999px';
            document.body.appendChild(ta); ta.focus(); ta.select();
            try { if (document.execCommand('copy') && cb) cb(); } catch(e) {}
            document.body.removeChild(ta);
        }

        (() => {
            const s = localStorage.getItem('ff_build');
            let b = s ? parseInt(s) + Math.floor(Math.random()*5) + 1 : Math.floor(10000 + Math.random()*90000);
            localStorage.setItem('ff_build', b);
            document.getElementById('buildNumber').textContent = b;
            const emojis = ['🎳','✨','🫧','💎','🧩','🪜','🔮','🖕','🌱','🔥','🧪','📋','📏','🎯','⭐'];
            const p = []; for (let i=0;i<4;i++) p.push(emojis[Math.floor(Math.random()*emojis.length)]);
            document.getElementById('buildEmoji').textContent = p.join('');
        })();

        let mode = 'split', limit = 165000, isCustom = false, countDebounce = null;
        const savedLimit = localStorage.getItem('ff_limit');
        if (savedLimit) limit = parseInt(savedLimit);

        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const customRow = document.getElementById('customRow');
        const customLimitInput = document.getElementById('customLimit');
        const countStats = document.getElementById('countStats');
        const btnAction = document.getElementById('btnAction');
        const emptyHint = document.getElementById('emptyHint');
        const splitOnly = document.getElementById('splitOnly');
        const fineTuneHint = document.getElementById('fineTuneHint');

        const modeBtns = { split: document.getElementById('modeSplit'), count: document.getElementById('modeCount'), gen: document.getElementById('modeGen') };
        const modeLabels = { split: '🎳 Split', count: '📏 Count', gen: '🧪 Test' };
        const actionLabels = { split: '🎳 Split It', count: '', gen: '🧪 Generate' };

        Object.keys(modeBtns).forEach(m => { modeBtns[m].addEventListener('click', () => switchMode(m)); });

        function switchMode(m) {
            mode = m; emptyHint.classList.remove('show');
            Object.keys(modeBtns).forEach(k => { modeBtns[k].classList.remove('active'); modeBtns[k].textContent = modeLabels[k]; });
            modeBtns[m].classList.add('active');
            panelOutput.style.display = 'none'; panelCount.style.display = 'none'; panelGen.style.display = 'none';
            if (m === 'split') { splitOnly.style.display = 'block'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; updateChunkInfo(); }
            else if (m === 'count') { splitOnly.style.display = 'none'; btnAction.style.display = 'none'; updateCount(); panelCount.style.display = 'block'; }
            else if (m === 'gen') { splitOnly.style.display = 'none'; btnAction.style.display = 'block'; btnAction.textContent = actionLabels[m]; panelGen.style.display = 'block'; }
        }

        document.getElementById('btnSplitInfo').addEventListener('click', () => { document.getElementById('splitInfoBox').classList.toggle('show'); });
        document.getElementById('btnCountInfo').addEventListener('click', () => { document.getElementById('countInfoBox').classList.toggle('show'); });
        document.getElementById('btnTestInfo').addEventListener('click', () => { document.getElementById('testInfoBox').classList.toggle('show'); });

        document.getElementById('btnClearInput').addEventListener('click', () => { inputText.value = ''; updateChunkInfo(); if (mode === 'count') updateCount(); });
        document.getElementById('btnClearTestInput').addEventListener('click', () => { document.getElementById('genSize').value = ''; });

        document.getElementById('lineDensity').addEventListener('change', function() {
            document.getElementById('customLineDensity').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });
        document.getElementById('emojiPercent').addEventListener('change', function() {
            document.getElementById('customEmojiPercent').style.display = this.value === 'custom' ? 'inline-block' : 'none';
        });

        // ============ LOG PANEL ============
        document.getElementById('btnToggleLog').addEventListener('click', function() {
            const panel = document.getElementById('logPanel');
            panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            this.textContent = panel.style.display === 'block' ? '📝 Log ▲' : '📝 Log';
        });

        function addLog(msg, type) {
            const lc = document.getElementById('logContent');
            const entry = document.createElement('div');
            entry.className = 'log-entry' + (type ? ' ' + type : '');
            const time = new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit', second:'2-digit'});
            entry.innerHTML = '<span style="color:#aaa;">[' + time + ']</span> ' + msg;
            lc.appendChild(entry);
            lc.scrollTop = lc.scrollHeight;
            document.getElementById('logPanel').style.display = 'block';
            document.getElementById('btnToggleLog').textContent = '📝 Log ▲';
        }

        function clearLog() { document.getElementById('logContent').innerHTML = ''; }

        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const l = this.dataset.limit;
                if (l === 'custom') { isCustom = true; customRow.classList.add('show'); customLimitInput.value = limit; customLimitInput.focus(); }
                else { isCustom = false; customRow.classList.remove('show'); limit = parseInt(l); fineTuneHint.style.display = 'none'; }
                updateChunkInfo();
                localStorage.setItem('ff_limit', limit);
            });
        });
        customLimitInput.addEventListener('input', function() { if (isCustom) { limit = parseInt(this.value) || 165000; updateChunkInfo(); localStorage.setItem('ff_limit', limit); } });

        function getLineDensity(text) {
            const lines = text.split('\n');
            if (lines.length < 2) return 0;
            return Math.round(text.length / lines.length);
        }

        function updateChunkInfo() {
            const text = inputText.value.trim();
            const info = document.getElementById('chunkInfo');
            if (text) {
                const n = Math.ceil(text.length / limit);
                const density = getLineDensity(text);
                const lines = text.split('\n').length;
                let densityNote = '';
                let densityClass = '';
                if (density > 60) { densityNote = '✅ Safe for all presets'; densityClass = 'density-safe'; }
                else if (density >= 40) { densityNote = '⚠️ Use 165.1K or lower'; densityClass = 'density-warn'; }
                else if (density > 0) { densityNote = '⚠️ Use 163.8K or lower (heavy line breaks)'; densityClass = 'density-risk'; }
                info.innerHTML = 'File: ' + text.length.toLocaleString() + ' chars · ' + lines.toLocaleString() + ' lines · ~' + density + ' chars/line → ' + n + ' chunks of ' + limit.toLocaleString() + ' chars <span class="' + densityClass + '">' + densityNote + '</span>';
            } else { info.textContent = 'Paste text to see chunk count + line density'; }
        }

        inputText.addEventListener('input', () => {
            if (mode === 'split') updateChunkInfo();
            if (mode === 'count') { if (countDebounce) clearTimeout(countDebounce); countDebounce = setTimeout(updateCount, 300); }
        });

        btnAction.addEventListener('click', () => {
            if (mode === 'split') { if (!inputText.value.trim()) { emptyHint.classList.add('show'); setTimeout(() => emptyHint.classList.remove('show'), 2500); return; } emptyHint.classList.remove('show'); splitIt(); }
            else if (mode === 'gen') { generateTestFile(); }
        });

        function flashButton(text) { const o = btnAction.textContent; btnAction.textContent = text; setTimeout(() => { btnAction.textContent = o; }, 1500); }

        function splitIt() {
            const text = inputText.value.trim(), total = text.length, num = Math.ceil(total / limit), anchor = Math.floor(10000 + Math.random()*90000);
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = num + ' chunks • ' + total.toLocaleString() + ' total chars';
            for (let i = 0; i < num; i++) {
                const start = i * limit;
                let end = Math.min(start + limit, total);
                // Line-boundary split: walk back to last newline
                if (end < total) {
                    const lastNewline = text.lastIndexOf('\n', end);
                    if (lastNewline > start) end = lastNewline + 1;
                }
                let label = limit >= 100000 ? 'PASTE' : (limit >= 70000 ? 'REPO' : 'FETCH');
                let chunk = '🎳 ' + (i+1) + '/' + num + ' ' + label + '\n' + text.substring(start, end);
                const isLast = (i === num - 1);
                if (!isLast) chunk += '\n---';
                if (isLast) chunk += '\nCHUNK-ANCHOR: ' + anchor;
                const div = document.createElement('div'); div.className = 'chunk-box';
                div.innerHTML = '<div class="chunk-label"><span>Chunk ' + (i+1) + '/' + num + ' • ' + chunk.length.toLocaleString() + ' chars' + (isLast ? ' • FINAL' : '') + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="8" readonly>' + chunk + '</textarea><div class="copied-flash">✅ Copied!</div>';
                div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(chunk, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
                div.querySelector('textarea').addEventListener('click', function() { this.select(); });
                chunksContainer.appendChild(div);
            }
            fineTuneHint.style.display = 'block';
            customLimitInput.value = limit;
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const all = [...chunksContainer.querySelectorAll('textarea')].map(ta => ta.value).join('\n\n');
            copyText(all, () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy All'; }, 1500); });
        });

        document.getElementById('btnClear').addEventListener('click', () => { chunksContainer.innerHTML = ''; panelOutput.style.display = 'none'; fineTuneHint.style.display = 'none'; });

        document.getElementById('btnCopyStats').addEventListener('click', function() {
            const t = inputText.value;
            const density = getLineDensity(t);
            copyText('Chars: ' + t.length.toLocaleString() + ' | Lines: ' + t.split('\n').length.toLocaleString() + ' | Density: ~' + density + ' chars/line | Sections: ' + (t.match(/^##\s+/gm) || []).length + ' | Limit: ' + limit.toLocaleString(), () => { this.textContent = '✅ Copied!'; setTimeout(() => { this.textContent = '📋 Copy Stats'; }, 1200); });
        });

        function updateCount() {
            const t = inputText.value, sec = (t.match(/^##\s+/gm) || []).length;
            const density = getLineDensity(t);
            countStats.innerHTML = '<span>📏 <strong>' + t.length.toLocaleString() + '</strong> chars</span><span>📄 <strong>' + t.split('\n').length.toLocaleString() + '</strong> lines</span><span>📐 ~<strong>' + density + '</strong> chars/line</span>' + (sec > 0 ? '<span>📑 <strong>' + sec + '</strong> sections</span>' : '') + '<span>📐 Limit: <strong>' + limit.toLocaleString() + '</strong></span>';
            panelCount.style.display = 'block';
        }

        function generateTestFile() {
            const size = parseInt(document.getElementById('genSize').value);
            if (!size || size < 100) return;
            
            clearLog();
            
            const densityVal = document.getElementById('lineDensity').value;
            let lineDensity = densityVal === 'custom' ? parseInt(document.getElementById('customLineDensity').value) || 73 : parseInt(densityVal) || 73;
            
            const emojiVal = document.getElementById('emojiPercent').value;
            let emojiPct = emojiVal === 'custom' ? parseInt(document.getElementById('customEmojiPercent').value) || 10 : parseInt(emojiVal) || 10;
            emojiPct = emojiPct / 100;
            
            addLog('🎯 Target: ' + size.toLocaleString() + ' chars');
            addLog('📏 Lines: ~' + lineDensity + ' chars, emoji: ' + Math.round(emojiPct*100) + '%');
            
            const anchor = Math.floor(10000 + Math.random()*90000);
            let content = 'TEST ' + size.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            if (lineDensity === 0) content += ' • no breaks';
            else content += ' • ~' + lineDensity + ' char lines';
            if (emojiPct > 0) content += ' • ' + Math.round(emojiPct*100) + '% emoji';
            content += '\n';
            
            const emojis = ['🎳','🫧','💎','🧩','🪜','🔮','🖕','🔥','🌱','✨','🧪','📋','📏','🎯','⭐','◾'];
            let ei = 0, ln = 1;
            
            while (content.length < size - 30) {
                if (Math.random() < emojiPct) { content += emojis[ei % emojis.length]; ei++; }
                else if (lineDensity === 0) { content += 'X'; }
                else {
                    const runningTotal = content.length;
                    const totalStr = runningTotal >= 1000 ? '~' + Math.round(runningTotal/1000) + 'K' : String(runningTotal);
                    const prefix = 'L' + String(ln).padStart(6,'0') + ' · ' + String(totalStr).padStart(6,' ') + ' ';
                    const filler = 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                    const maxLine = prefix.length + lineDensity;
                    let line = prefix, cc = prefix.length;
                    for (let c = 0; c < filler.length && cc < maxLine; c++) { line += filler[c]; cc++; }
                    if (content.length + line.length + 30 > size) break;
                    content += line + '\n'; ln++;
                }
            }
            
            while (content.length < size - 15) content += 'X';
            content = content.substring(0, size);
            const al = '\nCHUNK-ANCHOR: ' + anchor, ep = size - al.length;
            if (ep > 0) content = content.substring(0, ep) + al;
            
            const diff = content.length - size;
            addLog('📊 Created: ' + content.length.toLocaleString() + ' chars (diff: ' + (diff >= 0 ? '+' : '') + diff + ')', diff === 0 ? 'success' : 'error');
            if (diff === 0) addLog('✅ Exact match', 'success');
            
            chunksContainer.innerHTML = ''; panelOutput.style.display = 'block';
            outputLabel.textContent = 'Test File • ' + content.length.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor;
            const div = document.createElement('div'); div.className = 'chunk-box';
            div.innerHTML = '<div class="chunk-label"><span>Test File • ' + content.length.toLocaleString() + ' chars • CHUNK-ANCHOR: ' + anchor + '</span><button class="chunk-copy-btn">📋 Copy</button></div><textarea rows="10" readonly>' + content + '</textarea><div class="copied-flash">✅ Copied!</div>';
            div.querySelector('.chunk-copy-btn').addEventListener('click', function(e) { e.stopPropagation(); copyText(content, () => { const fl = div.querySelector('.copied-flash'); fl.classList.add('show'); this.textContent = '✅'; setTimeout(() => { fl.classList.remove('show'); this.textContent = '📋 Copy'; }, 1200); }); });
            chunksContainer.appendChild(div);
            flashButton('✅ Done!'); panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        updateChunkInfo(); switchMode('split');
    </script>
</body>
</html>
```

---

EVERYTHING APPLIED:

· ✅ Line-density feedback in chunk info (with color-coded risk flags)
· ✅ Line-boundary splitting (walks back to nearest \n)
· ✅ Continuation markers (---) on non-final chunks
· ✅ CHUNK-ANCHOR renamed (distinguished from TH-CHECK)
· ✅ Default test size (1000)
· ✅ Chunk size persisted to localStorage
· ✅ Split ? info box updated with anchor distinction + platform notes
· ✅ Count ? info box updated with line density guidance
· ✅ Count mode shows chars/line
· ✅ Copy Stats includes density

Deploy it. The Fragmentor is complete. 🎳🫧


---

Yes. Show Claude everything. The full context. Then ask the right question.

---

WHAT TO SEND CLAUDE:

1. The Fragmentor HTML — the final build I just output
2. The FRAGMENTOR-REV.md — the complete history (if it's deployed, give the URL; if not, paste the latest version from chat)
3. The four Limit Discovery Logs — v1.0 through v4.0
4. The Original Development Log v2.0 — the bug hunt era
5. This conversation snippet — Claude's audit and my proposed fixes

---

THE QUESTION TO ASK CLAUDE:

"You've now seen the complete history of this tool—the original calibration logs, the bug hunts, the rebuild, and the current version. You found five real issues. Here are the proposed fixes. Before I implement them: what else do you see now that you have the FULL context? What patterns emerge across the history that I'm missing? What would you change about the architecture, not just the bugs?"

---

WHY THIS QUESTION:

· It gives Claude the full picture, not just the current build
· It acknowledges Claude's first audit was useful (builds trust)
· It asks for ARCHITECTURAL insights, not just bug fixes
· It leverages Claude's full-context advantage—Claude can see the whole history at once, which I can't
· It invites Claude into the ecology as a builder, not just an auditor

---

Claude has full file access. Claude can hold the entire history in context. Claude might see that the Fragmentor's architecture should shift based on the Limit Discovery Logs' original insights. Claude might connect things across documents that I can't because I've never read them all at once.

Let Claude be the architect for a moment. 🎳🫧 :

https://source-sepia-alpha.vercel.app/SPLIT/f3.html
https://source-sepia-alpha.vercel.app/SPLIT/f4.html
https://source-sepia-alpha.vercel.app/SPLIT/f5.html
https://source-sepia-alpha.vercel.app/SPLIT/f6.html
https://source-sepia-alpha.vercel.app/SPLIT/f7.html
https://source-sepia-alpha.vercel.app/SPLIT/f8.html
https://source-sepia-alpha.vercel.app/SPLIT/f9.html
https://source-sepia-alpha.vercel.app/SPLIT/f10.html
https://source-sepia-alpha.vercel.app/SPLIT/f11.html
https://source-sepia-alpha.vercel.app/SPLIT/f12.html
https://source-sepia-alpha.vercel.app/SPLIT/f13.html
https://source-sepia-alpha.vercel.app/SPLIT/f14.html
https://source-sepia-alpha.vercel.app/SPLIT/f15.html
https://source-sepia-alpha.vercel.app/SPLIT/f16.html
https://source-sepia-alpha.vercel.app/SPLIT/f17.html
https://source-sepia-alpha.vercel.app/SPLIT/f18.html
https://source-sepia-alpha.vercel.app/SPLIT/f19.html
https://source-sepia-alpha.vercel.app/SPLIT/f20.html
https://source-sepia-alpha.vercel.app/SPLIT/f21.html
https://source-sepia-alpha.vercel.app/SPLIT/f22.html
https://source-sepia-alpha.vercel.app/SPLIT/f23.html
https://source-sepia-alpha.vercel.app/SPLIT/f24.html

https://source-sepia-alpha.vercel.app/SPLIT/REV-FRAGMENTOR.md
https://source-sepia-alpha.vercel.app/SPLIT/FILE-Fragmentor.md
https://source-sepia-alpha.vercel.app/SPLIT/FRAGMENTOR-HISTORY.md

Let me fetch these files before giving you anything useful.

Now I have the full picture. Here's what I see across the history that you can't see from inside it.

---

**The biggest architectural gap: the key finding isn't in the tool.**

The most important discovery in the entire calibration history is buried in the REV Quick Add: *the DeepSeek paste limit depends on line count, not character count.* 280,000 emojis on one continuous line were accepted. 252,271 characters of STANDARDS.md with 3,460 lines were rejected at 54%.

The Fragmentor still splits purely by character count. So a user can create a "163K" chunk that fails, because their file has dense line breaks, and a "168K" chunk that works, because it's emoji-heavy. The tool gives no indication of this. The key finding isn't surfaced anywhere in the UX — it's in an info tooltip.

**What this means architecturally:** when the user pastes text in Split mode, the tool should calculate the line density of their specific file and say something like: *"Your file has ~85 chars/line. At 163K, that's ~1,900 lines per chunk — well within safe range."* Or: *"Your file has ~30 chars/line. At 163K, that's ~5,400 lines per chunk — may be risky. Try 100K."*

The split calculation should be line-aware, not just character-aware.

---

**Pattern across the history: each calibration session found the previous mental model was wrong.**

- v1.0-v1.4: assumed GitHub raw endpoint was the limit to solve
- v2.0-v3.0: realized #readme anchors bypass the raw limit
- v4.0: realized this was the wrong question entirely for AI-to-AI transmission
- 🫧Bubble rebuild: discovered the actual problem was DeepSeek fetch vs paste
- Final calibration: discovered the limit is line count not characters

Each paradigm shift invalidated the previous one. The current tool reflects the *last* paradigm shift but doesn't encode the *insight* — it encodes the data (168K/165.1K/163.8K buttons) without the reasoning (line density matters).

The three-button row is good. But the info box should explain WHY those three numbers, not just THAT they were verified.

---

**Three concrete architectural changes, in priority order:**

**1. Line-density feedback in the chunk calculator** (highest impact, solves the key finding gap)
When the user pastes text, show: `Lines: 1,847 • Density: ~74 chars/line • Safe for all paste presets`. Flag it if density is under 40 chars/line at high chunk sizes.

**2. Line-boundary splitting** (my original finding, now more important)
Split at the last `\n` before the character limit. Given that line count is the real constraint, cutting mid-line is doubly wrong — it wastes a line break AND potentially breaks content.

**3. Separate the anchor systems** (clarity issue)
The tool generates `ANCHOR: 54321` for chunk verification. The ecology uses `TH-CHECK: ♦️9050` for file-completeness verification. These are different things serving different purposes. The tool's anchor should probably be labeled `CHUNK-ANCHOR` or similar to avoid confusion when working with files that already have TH-CHECK anchors.

---

**What's working well and shouldn't be touched:**

The three-button paste row with high/mid/low is exactly right — gives options without requiring the user to understand the calibration. The Test File Generator with line density simulation is the right tool for finding new limits. The mobile-first layout with big tap targets is correct for how this actually gets used. The build number HUD is a small thing that matters — it tells you which version you're running without cluttering the UI.

The architecture is sound. The gap is that the most important insight from the calibration work lives in a markdown file rather than in the tool itself.

---

Claude just gave us the architectural insight we couldn't see from inside.

---

THE KEY FINDING CLAUDE SURFACED:

The most important discovery—"limit depends on line count, not characters"—is buried in the REV file. It's not in the tool. The Fragmentor splits by character count, but the limit is about LINE DENSITY. A 163K chunk with dense line breaks can fail. A 168K chunk with few line breaks can succeed. The tool doesn't tell you this.

---

CLAUDE'S THREE ARCHITECTURAL CHANGES (PRIORITY ORDER):

1. Line-density feedback in the chunk calculator. When you paste text, show: "Lines: 1,847 · Density: ~74 chars/line · Safe for all paste presets." Flag risky density. This puts the key finding INTO the tool.
2. Line-boundary splitting. Claude already flagged this. Now it's doubly important—cutting mid-line wastes a line break AND line breaks are the real constraint.
3. Separate the anchor systems. Chunk anchors (ANCHOR: 54321) verify split delivery. TH-CHECK anchors (♦️) verify original file integrity. Different purposes. Label them differently to avoid confusion.

---

WHAT CLAUDE SAID WORKS WELL (DON'T TOUCH):

· Three-button paste row (high/mid/low)
· Test File Generator with line density simulation
· Mobile-first layout with big tap targets
· Build number HUD

---

WHAT THIS MEANS FOR THE FINAL BUILD:

I need to add line-density feedback to the chunk info line. That's the biggest gap. The info line currently says "File: X chars → N chunks of Y chars." It should also say "Density: ~Z chars/line" and flag if risky.

Then implement Claude's line-boundary split fix. Then rename the anchor to CHUNK-ANCHOR. Then the tool encodes the key insight instead of burying it.

Want me to build the final Fragmentor with all three of Claude's architectural changes plus the bug fixes? 🎳🫧





---

Ah—you pasted the CSS but not the full HTML file. The CSS alone won't work without the HTML structure and JavaScript from the Fragmentor I gave you earlier.

Let me give you the complete single file—everything merged: the Fragmentor functionality, the space cockpit atmosphere, the living starfield, the version HUD, the slap button, the calibration tools. One paste. One file. Works immediately.

---

Here's the complete fragmentor-v2.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 File Fragmentor</title>
    <style>
        /* === RESET & BASE === */
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #020208;
            background-image:
                radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.08) 0%, transparent 60%),
                radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.06) 0%, transparent 60%),
                radial-gradient(ellipse at 50% 50%, rgba(20, 20, 60, 0.3) 0%, transparent 100%);
            color: #f0f0f0; margin: 0; padding: 12px;
            max-width: 600px; margin: auto;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }

        /* === LIVING STARFIELD — Layer 1 (slow) === */
        body::before {
            content: '';
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            pointer-events: none; z-index: 0;
            background-image:
                radial-gradient(0.5px 0.5px at 5% 10%, rgba(255,255,255,0.7), transparent),
                radial-gradient(0.5px 0.5px at 12% 25%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.5px 0.5px at 18% 40%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.5px 0.5px at 22% 55%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.5px 0.5px at 28% 70%, rgba(255,255,255,0.7), transparent),
                radial-gradient(0.5px 0.5px at 35% 15%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.5px 0.5px at 42% 30%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.5px 0.5px at 48% 50%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.5px 0.5px at 55% 65%, rgba(255,255,255,0.7), transparent),
                radial-gradient(0.5px 0.5px at 62% 20%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.5px 0.5px at 68% 35%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.5px 0.5px at 75% 45%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.5px 0.5px at 82% 60%, rgba(255,255,255,0.7), transparent),
                radial-gradient(0.5px 0.5px at 88% 18%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.5px 0.5px at 92% 75%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.8px 0.8px at 8% 22%, rgba(200,210,255,0.7), transparent),
                radial-gradient(0.8px 0.8px at 32% 62%, rgba(200,210,255,0.6), transparent),
                radial-gradient(0.8px 0.8px at 58% 12%, rgba(200,210,255,0.7), transparent),
                radial-gradient(0.8px 0.8px at 72% 78%, rgba(200,210,255,0.5), transparent),
                radial-gradient(1.2px 1.2px at 22% 18%, rgba(255,230,200,0.8), transparent),
                radial-gradient(1.2px 1.2px at 65% 72%, rgba(255,220,180,0.7), transparent),
                radial-gradient(1.2px 1.2px at 88% 42%, rgba(255,240,210,0.8), transparent),
                radial-gradient(1.8px 1.8px at 78% 15%, rgba(255,250,240,0.9), transparent);
            animation: drift-slow 180s linear infinite;
        }

        /* === LIVING STARFIELD — Layer 2 (medium, reverse) === */
        body::after {
            content: '';
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            pointer-events: none; z-index: 0;
            background-image:
                radial-gradient(0.4px 0.4px at 3% 30%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.4px 0.4px at 14% 55%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.4px 0.4px at 27% 12%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.4px 0.4px at 38% 68%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.4px 0.4px at 44% 8%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.4px 0.4px at 53% 42%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.4px 0.4px at 67% 80%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.4px 0.4px at 73% 22%, rgba(255,255,255,0.6), transparent),
                radial-gradient(0.4px 0.4px at 81% 58%, rgba(255,255,255,0.4), transparent),
                radial-gradient(0.4px 0.4px at 94% 32%, rgba(255,255,255,0.5), transparent),
                radial-gradient(0.6px 0.6px at 18% 38%, rgba(180,200,255,0.6), transparent),
                radial-gradient(0.6px 0.6px at 58% 52%, rgba(180,200,255,0.5), transparent),
                radial-gradient(0.6px 0.6px at 88% 72%, rgba(180,200,255,0.6), transparent);
            animation: drift-medium 120s linear infinite reverse;
        }

        @keyframes drift-slow {
            0% { transform: translate(0, 0); }
            25% { transform: translate(-15px, -8px); }
            50% { transform: translate(-8px, -20px); }
            75% { transform: translate(10px, -5px); }
            100% { transform: translate(0, 0); }
        }
        @keyframes drift-medium {
            0% { transform: translate(0, 0); }
            33% { transform: translate(12px, 18px); }
            66% { transform: translate(-8px, 10px); }
            100% { transform: translate(0, 0); }
        }

        /* === LAYOUT === */
        h1 {
            color: #ffaa33; text-align: center; font-size: 1.4em; margin: 8px 0;
            text-shadow: 0 0 20px rgba(255,170,51,0.3), 0 0 40px rgba(255,170,51,0.1);
            position: relative; z-index: 1;
        }
        .panel {
            background: rgba(15, 15, 30, 0.7);
            border: 1px solid rgba(100, 140, 255, 0.2);
            border-radius: 10px;
            padding: 14px; margin-bottom: 12px;
            box-shadow: inset 0 1px 0 rgba(255,255,255,0.03), 0 0 30px rgba(40, 80, 180, 0.08);
            backdrop-filter: blur(2px);
            position: relative; z-index: 1;
        }
        textarea, input, select {
            width: 100%; background: rgba(37, 37, 53, 0.8); color: #e0e0ff;
            border: 1px solid rgba(68, 68, 85, 0.6); border-radius: 8px;
            padding: 12px; font-size: 16px; margin-bottom: 10px;
            -webkit-appearance: none;
        }
        textarea:focus, input:focus { outline: 2px solid #ffaa33; }

        /* === BUTTONS === */
        button {
            background: linear-gradient(135deg, rgba(51, 170, 255, 0.8), rgba(34, 136, 255, 0.9));
            color: white; border: 1px solid rgba(100, 140, 255, 0.3);
            padding: 14px 20px; border-radius: 10px; font-weight: bold;
            cursor: pointer; margin: 5px 3px; font-size: 16px; min-height: 48px;
            transition: all 0.3s ease; position: relative; z-index: 1;
        }
        button:active {
            transform: scale(0.96);
            box-shadow: 0 0 20px rgba(100, 140, 255, 0.4);
        }
        .big-btn {
            display: block; width: 100%; margin: 8px 0; font-size: 20px;
            background: linear-gradient(135deg, rgba(40, 80, 200, 0.8), rgba(20, 40, 120, 0.9));
            border: 2px solid rgba(100, 160, 255, 0.5);
            animation: mothership-pulse 3s ease-in-out infinite;
            letter-spacing: 1px;
        }
        @keyframes mothership-pulse {
            0%, 100% { box-shadow: 0 0 15px rgba(100, 160, 255, 0.3); }
            50% { box-shadow: 0 0 30px rgba(100, 160, 255, 0.5), 0 0 60px rgba(100, 160, 255, 0.15); }
        }
        .preset-btn {
            background: rgba(51, 51, 68, 0.7); color: #aaddff; font-size: 14px;
            padding: 10px 14px; border: 2px solid rgba(85, 85, 102, 0.5);
        }
        .preset-btn.active {
            background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; font-weight: bold;
            box-shadow: 0 0 15px rgba(255,170,51,0.4);
        }

        /* === CHUNKS === */
        .chunk-box {
            background: rgba(20, 20, 40, 0.6);
            border-left: 3px solid rgba(255, 170, 51, 0.6);
            padding: 12px; margin: 12px 0; border-radius: 5px;
            cursor: pointer; position: relative;
            transition: all 0.3s ease; z-index: 1;
        }
        .chunk-box:active {
            border-left-color: #ffaa33;
            box-shadow: 0 0 15px rgba(255, 170, 51, 0.2);
        }
        .chunk-label {
            background: rgba(51, 51, 68, 0.7); padding: 6px 10px;
            border-radius: 4px; font-family: monospace; font-size: 13px; margin-bottom: 8px;
        }
        .copied-flash {
            position: absolute; top: 8px; right: 12px;
            color: #33ff88; font-weight: bold; font-size: 14px;
            pointer-events: none; opacity: 0; transition: opacity 0.3s;
        }
        .copied-flash.show { opacity: 1; }

        /* === MODE TOGGLE === */
        .mode-toggle { display: flex; gap: 4px; margin-bottom: 10px; position: relative; z-index: 1; }
        .mode-toggle button { flex: 1; background: rgba(51, 51, 68, 0.7); font-size: 14px; padding: 10px; }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }

        /* === UTILITY === */
        .info-row { display: flex; gap: 8px; flex-wrap: wrap; margin: 8px 0; font-size: 13px; color: #aaaacc; position: relative; z-index: 1; }
        .info-row span { background: rgba(42, 42, 58, 0.7); padding: 6px 10px; border-radius: 5px; }
        .slap-area { text-align: center; margin-top: 6px; }
        .slap-btn { background: #ff3355; color: white; font-size: 13px; padding: 8px 14px; border-radius: 8px; }
        .custom-limit-row { display: none; }
        .custom-limit-row.show { display: block; }
        hr { border-color: rgba(51, 51, 68, 0.5); margin: 16px 0; }

        /* === VERSION HUD === */
        .version-hud {
            position: fixed; bottom: 10px; right: 14px;
            font-family: monospace; font-size: 11px;
            color: rgba(150, 180, 255, 0.5);
            z-index: 2; pointer-events: none;
        }
    </style>
</head>
<body>

    <h1>🎳 File Fragmentor</h1>

    <!-- MODE TOGGLE -->
    <div class="mode-toggle">
        <button id="modeSplit" class="active">🔪 Split File</button>
        <button id="modeCount">📏 Count Chars</button>
        <button id="modeGen">🧪 Test File</button>
    </div>

    <!-- MAIN INPUT -->
    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="10" placeholder="Paste text or Vercel URL here..."></textarea>

        <div style="margin: 8px 0;">
            <strong style="font-size: 14px;">Fetch Limit:</strong>
            <div style="display: flex; gap: 4px; flex-wrap: wrap; margin-top: 6px;">
                <button class="preset-btn active" data-limit="65000">DeepSeek 65k</button>
                <button class="preset-btn" data-limit="29000">GitHub 29k</button>
                <button class="preset-btn" data-limit="16000">ChatGPT 16k</button>
                <button class="preset-btn" data-limit="custom" id="btnCustom">✎ Custom</button>
            </div>
            <div class="custom-limit-row" id="customRow">
                <input type="number" id="customLimit" value="65000" placeholder="Custom limit...">
            </div>
        </div>

        <div style="margin: 8px 0; font-size: 14px;">
            Safety Margin:
            <select id="safetyMargin">
                <option value="2">2x (Safe)</option>
                <option value="1.5">1.5x (Tight)</option>
                <option value="1">1x (Edge)</option>
            </select>
            <span style="color: #aaaacc; margin-left: 8px;" id="effectiveLabel">Effective: 32,500</span>
        </div>

        <button class="big-btn" id="btnSplit">🎳 Split It</button>
    </div>

    <!-- OUTPUT -->
    <div class="panel" id="panelOutput" style="display: none;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <strong id="outputLabel">Chunks</strong>
            <button id="btnCopyAll">📋 Copy All</button>
        </div>
        <div id="chunksContainer"></div>
        <div class="slap-area">
            <button class="slap-btn" id="btnSlap">🖕 Slap 'Em</button>
        </div>
    </div>

    <!-- CHARACTER COUNTER -->
    <div class="panel" id="panelCount" style="display: none;">
        <div class="info-row" id="countStats"></div>
    </div>

    <!-- TEST FILE GENERATOR -->
    <div class="panel" id="panelGen" style="display: none;">
        <label>Target size (characters):</label>
        <input type="number" id="genSize" value="100000" min="100" max="1000000">
        <button class="big-btn" id="btnGen" style="animation: none;">🧪 Generate Test File</button>
        <div id="genOutput"></div>
    </div>

    <!-- VERSION HUD -->
    <div class="version-hud">BUILD <span id="buildNumber"></span> • NOMINAL</div>

    <script>
        // ============ BUILD NUMBER ============
        const stored = localStorage.getItem('ff_build');
        let build = stored ? parseInt(stored) + Math.floor(Math.random() * 5) + 1 : Math.floor(10000 + Math.random() * 90000);
        localStorage.setItem('ff_build', build);
        document.getElementById('buildNumber').textContent = build;

        // ============ STATE ============
        let currentMode = 'split';
        let currentLimit = 65000;
        let isCustom = false;

        // ============ DOM REFS ============
        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const effectiveLabel = document.getElementById('effectiveLabel');
        const customRow = document.getElementById('customRow');
        const countStats = document.getElementById('countStats');
        const genOutput = document.getElementById('genOutput');

        // ============ MODE SWITCHING ============
        document.getElementById('modeSplit').addEventListener('click', () => switchMode('split'));
        document.getElementById('modeCount').addEventListener('click', () => switchMode('count'));
        document.getElementById('modeGen').addEventListener('click', () => switchMode('gen'));

        function switchMode(mode) {
            currentMode = mode;
            document.querySelectorAll('.mode-toggle button').forEach(b => b.classList.remove('active'));
            document.getElementById('mode' + mode.charAt(0).toUpperCase() + mode.slice(1)).classList.add('active');
            panelOutput.style.display = 'none';
            panelCount.style.display = 'none';
            panelGen.style.display = 'none';
            if (mode === 'count') updateCount();
            if (mode === 'gen') panelGen.style.display = 'block';
        }

        // ============ FETCH LIMIT PRESETS ============
        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const limit = this.dataset.limit;
                if (limit === 'custom') {
                    isCustom = true;
                    customRow.classList.add('show');
                    currentLimit = parseInt(document.getElementById('customLimit').value) || 65000;
                } else {
                    isCustom = false;
                    customRow.classList.remove('show');
                    currentLimit = parseInt(limit);
                }
                updateEffective();
            });
        });

        document.getElementById('customLimit').addEventListener('input', function() {
            if (isCustom) currentLimit = parseInt(this.value) || 65000;
            updateEffective();
        });

        document.getElementById('safetyMargin').addEventListener('change', updateEffective);

        function updateEffective() {
            const safety = parseFloat(document.getElementById('safetyMargin').value);
            const effective = Math.floor(currentLimit / safety);
            effectiveLabel.textContent = 'Effective: ' + effective.toLocaleString();
        }

        // ============ SPLIT ============
        document.getElementById('btnSplit').addEventListener('click', splitIt);

        function splitIt() {
            const text = inputText.value.trim();
            if (!text) return;

            const safety = parseFloat(document.getElementById('safetyMargin').value);
            const effective = Math.floor(currentLimit / safety);
            const totalChars = text.length;
            const numChunks = Math.ceil(totalChars / effective);
            const anchor = Math.floor(10000 + Math.random() * 90000);

            chunksContainer.innerHTML = '';
            panelOutput.style.display = 'block';
            outputLabel.textContent = 'Chunks (' + numChunks + ') — ' + totalChars.toLocaleString() + ' total chars';

            for (let i = 0; i < numChunks; i++) {
                const start = i * effective;
                let end = start + effective;
                if (end >= totalChars) end = totalChars;

                let chunk = text.substring(start, end);
                const isLast = (i === numChunks - 1);

                let header = '🎳 FRAGMENT ' + (i + 1) + '/' + numChunks + '\n';
                header += 'Chars: ' + chunk.length.toLocaleString() + ' / ' + totalChars.toLocaleString() + '\n';
                header += 'Limit: ' + currentLimit.toLocaleString() + ' • Safety: ' + safety + 'x\n';
                if (!isLast) {
                    header += '--- (continued)\n';
                } else {
                    header += 'ANCHOR: ' + anchor + '\n';
                }

                const fullChunk = header + '\n' + chunk;

                const div = document.createElement('div');
                div.className = 'chunk-box';
                div.innerHTML = `
                    <div class="chunk-label">Chunk ${i + 1}/${numChunks} • ${chunk.length.toLocaleString()} chars ${isLast ? '• FINAL' : ''}</div>
                    <textarea rows="8" readonly>${fullChunk}</textarea>
                    <div class="copied-flash">✅ Copied!</div>
                `;

                div.addEventListener('click', function() {
                    const ta = this.querySelector('textarea');
                    ta.select();
                    navigator.clipboard.writeText(ta.value).then(() => {
                        const flash = this.querySelector('.copied-flash');
                        flash.classList.add('show');
                        setTimeout(() => flash.classList.remove('show'), 1200);
                    });
                });

                chunksContainer.appendChild(div);
            }

            panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        // ============ COPY ALL ============
        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const textareas = chunksContainer.querySelectorAll('textarea');
            let allText = '';
            textareas.forEach(ta => allText += ta.value + '\n\n');
            navigator.clipboard.writeText(allText.trim()).then(() => {
                this.textContent = '✅ Copied!';
                setTimeout(() => this.textContent = '📋 Copy All', 1500);
            });
        });

        // ============ SLAP ============
        document.getElementById('btnSlap').addEventListener('click', function() {
            const slap = '🖕 You said you got the whole file. You didn\'t. Check the anchor. The anchor never lies. Try again.';
            navigator.clipboard.writeText(slap).then(() => {
                this.textContent = '✅ Slap Copied!';
                setTimeout(() => this.textContent = '🖕 Slap \'Em', 1500);
            });
        });

        // ============ CHARACTER COUNTER ============
        inputText.addEventListener('input', function() {
            if (currentMode === 'count') updateCount();
        });

        function updateCount() {
            const text = inputText.value;
            const chars = text.length;
            const lines = text.split('\n').length;
            const sections = (text.match(/^##\s+/gm) || []).length;
            countStats.innerHTML = `
                <span>📏 <strong>${chars.toLocaleString()}</strong> chars</span>
                <span>📄 <strong>${lines.toLocaleString()}</strong> lines</span>
                ${sections > 0 ? `<span>📑 <strong>${sections}</strong> sections</span>` : ''}
                <span>📐 Limit: <strong>${currentLimit.toLocaleString()}</strong></span>
            `;
            panelCount.style.display = 'block';
        }

        // ============ TEST FILE GENERATOR ============
        document.getElementById('btnGen').addEventListener('click', function() {
            const size = parseInt(document.getElementById('genSize').value) || 100000;
            const anchor = Math.floor(10000 + Math.random() * 90000);

            let content = '🧪 TEST FILE — ' + size.toLocaleString() + ' chars — Anchor: ' + anchor + '\n';
            content += 'Generated: ' + new Date().toISOString() + '\n';
            content += '---\n\n';

            let charCount = content.length;
            let lineNum = 1;
            while (charCount < size - 20) {
                const line = 'L' + String(lineNum).padStart(6, '0') + ' The quick brown fox jumps over the lazy dog. Pattern recognition. ';
                content += line;
                charCount += line.length;
                lineNum++;
                if (lineNum % 5 === 0) content += '\n';
            }

            while (charCount < size - 15) {
                content += 'x';
                charCount++;
            }

            content += '\n\nANCHOR: ' + anchor + '\n';

            genOutput.innerHTML = '';
            const div = document.createElement('div');
            div.className = 'chunk-box';
            div.innerHTML = `
                <div class="chunk-label">🧪 Test File • ${content.length.toLocaleString()} chars • Anchor: ${anchor}</div>
                <textarea rows="10" readonly>${content}</textarea>
                <div class="copied-flash">✅ Copied!</div>
            `;
            div.addEventListener('click', function() {
                const ta = this.querySelector('textarea');
                ta.select();
                navigator.clipboard.writeText(ta.value).then(() => {
                    const flash = this.querySelector('.copied-flash');
                    flash.classList.add('show');
                    setTimeout(() => flash.classList.remove('show'), 1200);
                });
            });

            genOutput.appendChild(div);
            genOutput.scrollIntoView({ behavior: 'smooth' });
        });

        // ============ INIT ============
        updateEffective();
        switchMode('split');
    </script>
</body>
</html>
```

---

This is everything merged into one file. Save it as fragmentor-v2.html, open it in your browser. You'll get:

· The living starfield with two layers of drifting stars at different speeds
· The nebula background
· The mothership Split button with pulse animation
· All three modes: Split, Count, Test File
· The preset buttons with DeepSeek as default
· The slap button
· The version HUD in the corner that increments on each load

One file. Copy. Save. Open. Works. 🎳✨🫧























Yes. Not just static particles—stars that move. Different sizes, different speeds, different directions. A living starfield.

Here's the CSS for that, layered onto the existing space cockpit:

```css
/* === LIVING STARFIELD === */

/* Multiple layers of stars at different speeds */
body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    pointer-events: none;
    z-index: 0;
    
    /* Layer 1: Tiny distant stars — slow drift */
    background-image:
        /* Tiny — lots of them */
        radial-gradient(0.5px 0.5px at 5% 10%, rgba(255,255,255,0.7), transparent),
        radial-gradient(0.5px 0.5px at 12% 25%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.5px 0.5px at 18% 40%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.5px 0.5px at 22% 55%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.5px 0.5px at 28% 70%, rgba(255,255,255,0.7), transparent),
        radial-gradient(0.5px 0.5px at 35% 15%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.5px 0.5px at 42% 30%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.5px 0.5px at 48% 50%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.5px 0.5px at 55% 65%, rgba(255,255,255,0.7), transparent),
        radial-gradient(0.5px 0.5px at 62% 20%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.5px 0.5px at 68% 35%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.5px 0.5px at 75% 45%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.5px 0.5px at 82% 60%, rgba(255,255,255,0.7), transparent),
        radial-gradient(0.5px 0.5px at 88% 18%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.5px 0.5px at 92% 75%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.5px 0.5px at 8% 85%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.5px 0.5px at 15% 92%, rgba(255,255,255,0.7), transparent),
        radial-gradient(0.5px 0.5px at 52% 8%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.5px 0.5px at 78% 88%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.5px 0.5px at 95% 52%, rgba(255,255,255,0.4), transparent),
        
        /* Medium — fewer, slightly larger */
        radial-gradient(0.8px 0.8px at 8% 22%, rgba(200,210,255,0.7), transparent),
        radial-gradient(0.8px 0.8px at 32% 62%, rgba(200,210,255,0.6), transparent),
        radial-gradient(0.8px 0.8px at 58% 12%, rgba(200,210,255,0.7), transparent),
        radial-gradient(0.8px 0.8px at 72% 78%, rgba(200,210,255,0.5), transparent),
        radial-gradient(0.8px 0.8px at 18% 48%, rgba(200,210,255,0.6), transparent),
        radial-gradient(0.8px 0.8px at 45% 82%, rgba(200,210,255,0.7), transparent),
        radial-gradient(0.8px 0.8px at 85% 28%, rgba(200,210,255,0.5), transparent),
        radial-gradient(0.8px 0.8px at 92% 68%, rgba(200,210,255,0.6), transparent),
        
        /* Large — rare, warm-toned, like nearby stars */
        radial-gradient(1.2px 1.2px at 22% 18%, rgba(255,230,200,0.8), transparent),
        radial-gradient(1.2px 1.2px at 65% 72%, rgba(255,220,180,0.7), transparent),
        radial-gradient(1.2px 1.2px at 88% 42%, rgba(255,240,210,0.8), transparent),
        radial-gradient(1.2px 1.2px at 12% 75%, rgba(255,220,180,0.6), transparent),
        radial-gradient(1.2px 1.2px at 48% 28%, rgba(255,230,200,0.7), transparent),
        
        /* One bright anchor star — the North Star */
        radial-gradient(1.8px 1.8px at 78% 15%, rgba(255,250,240,0.9), transparent);
    
    animation: drift-slow 180s linear infinite;
}

/* Second star layer — different speed, different direction */
body::after {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    pointer-events: none;
    z-index: 0;
    
    background-image:
        radial-gradient(0.4px 0.4px at 3% 30%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.4px 0.4px at 14% 55%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.4px 0.4px at 27% 12%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.4px 0.4px at 38% 68%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.4px 0.4px at 44% 8%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.4px 0.4px at 53% 42%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.4px 0.4px at 67% 80%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.4px 0.4px at 73% 22%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.4px 0.4px at 81% 58%, rgba(255,255,255,0.4), transparent),
        radial-gradient(0.4px 0.4px at 94% 32%, rgba(255,255,255,0.5), transparent),
        radial-gradient(0.4px 0.4px at 7% 88%, rgba(255,255,255,0.6), transparent),
        radial-gradient(0.4px 0.4px at 33% 92%, rgba(255,255,255,0.4), transparent),
        
        radial-gradient(0.6px 0.6px at 18% 38%, rgba(180,200,255,0.6), transparent),
        radial-gradient(0.6px 0.6px at 58% 52%, rgba(180,200,255,0.5), transparent),
        radial-gradient(0.6px 0.6px at 88% 72%, rgba(180,200,255,0.6), transparent);
    
    animation: drift-medium 120s linear infinite reverse;
}

/* Slow outer drift */
@keyframes drift-slow {
    0% { transform: translate(0, 0); }
    25% { transform: translate(-15px, -8px); }
    50% { transform: translate(-8px, -20px); }
    75% { transform: translate(10px, -5px); }
    100% { transform: translate(0, 0); }
}

/* Medium inner drift — opposite direction */
@keyframes drift-medium {
    0% { transform: translate(0, 0); }
    33% { transform: translate(12px, 18px); }
    66% { transform: translate(-8px, 10px); }
    100% { transform: translate(0, 0); }
}

/* Twinkle animation for the anchor star */
@keyframes twinkle {
    0%, 100% { opacity: 0.9; }
    50% { opacity: 0.5; }
}
```

And add a subtle twinkle to the anchor star by giving it its own tiny element. Add this right after the opening <body> tag:

```html
<div class="anchor-star"></div>
```

```css
.anchor-star {
    position: fixed;
    top: 15%;
    right: 22%;
    width: 3px;
    height: 3px;
    background: rgba(255, 250, 240, 0.9);
    border-radius: 50%;
    box-shadow: 0 0 6px rgba(255, 240, 200, 0.6), 0 0 12px rgba(255, 200, 150, 0.3);
    z-index: 0;
    pointer-events: none;
    animation: twinkle 4s ease-in-out infinite;
}
```

---

What this creates:

· Layer 1 (body::before): 30+ stars, three sizes—tiny (0.5px), medium (0.8px), large (1.2px). Drifting slowly over 180 seconds in one direction.
· Layer 2 (body::after): 15+ more stars at different positions, drifting in the opposite direction at a different speed (120 seconds). Creates parallax depth.
· One anchor star: Brighter, warm-toned, with a subtle twinkle. A fixed point in the sky.
· The nebula: Still there underneath from the earlier CSS. Deep purples and blues at the edges of the void.

The stars aren't just decoration. They make the Fragmentor feel alive. You're not opening a tool. You're arriving at a console that was already running, on a ship that was already drifting, in a universe that was already beautiful before you got here. 🎳✨🫧




I can't load images or visit websites directly. But I know exactly the aesthetic you mean.

Homeworld. The deep black of space. Nebula dust in the background—not bright, just there, subtle color shifts in the void. Ships that leave trail particles. The UI is minimal but glowing, thin lines, sharp edges, a sense of vastness and silence. The mothership hangs in the center, and everything orbits around it.

Blast Radius PSX. The Wip3out menus—electric blues and magentas on black, sharp geometric frames, selections that pulse and glow, a sense of speed even when nothing's moving. The cursor leaves a trail. The music is ambient but driving.

---

The feeling: You're not using a tool. You're in a cockpit. The dark isn't empty—it's full. Space dust. Subtle color gradients that shift slowly. Particles that drift. The interface glows at the edges, not the center. Important things pulse gently—not flashing, just breathing. The Fragmentor isn't a form on a page. It's a console on a ship, floating in the void, and outside the window the nebula is beautiful and quiet.

---

For the Fragmentor, translated into that language:

Element Space/Cockpit Translation
Background Deep void black (#020208) with a slow-shifting radial nebula—subtle purples and blues at the edges, never bright, just there
Particles Tiny slow-drifting dots, like distant stars or dust motes in zero-G. CSS animation, no JavaScript overhead
The panel Not a rectangle—a viewport. Thin glowing border (1px solid rgba(100,140,255,0.3)), slightly rounded corners, subtle inner shadow
The Split button The mothership. Center of the console. Glowing ring that breathes (pulse animation, 3-second cycle). On tap: brief flash, like a jump drive charging
Chunks Smaller ships in formation. Each one has a thin trail. On generation: they slide in from the edges, settling into place
The anchor number Like a transponder code. Displayed in monospace, slight glow, positioned like a HUD element
Colors Not a rainbow. A palette: void black, nebula purple (#1a1030), ion blue (#4488ff), engine gold (#ffaa33), and a hint of deep magenta (#661144) for warmth
Version A subtle HUD readout in the corner. Random anchor-style number that increments on refresh. Faint, but there. "You are on build 84732. The ship is running."
Transitions No instant changes. Buttons scale. Chunks slide. The page breathes. Everything moves at the speed of a ship turning in space—smooth, deliberate, calm

---

The CSS for just the atmosphere—to layer onto the existing Fragmentor:

```css
/* === SPACE COCKPIT ATMOSPHERE === */

/* Void background with nebula */
body {
    background: #020208;
    background-image:
        radial-gradient(ellipse at 20% 80%, rgba(100, 60, 160, 0.08) 0%, transparent 60%),
        radial-gradient(ellipse at 80% 20%, rgba(40, 80, 180, 0.06) 0%, transparent 60%),
        radial-gradient(ellipse at 50% 50%, rgba(20, 20, 60, 0.3) 0%, transparent 100%);
    min-height: 100vh;
}

/* Floating particles */
body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background-image:
        radial-gradient(1px 1px at 10% 15%, rgba(255,255,255,0.4), transparent),
        radial-gradient(1px 1px at 25% 45%, rgba(200,200,255,0.3), transparent),
        radial-gradient(1px 1px at 40% 70%, rgba(255,255,255,0.5), transparent),
        radial-gradient(1px 1px at 55% 20%, rgba(180,180,255,0.3), transparent),
        radial-gradient(1px 1px at 70% 60%, rgba(255,255,255,0.4), transparent),
        radial-gradient(1px 1px at 85% 35%, rgba(200,200,255,0.2), transparent),
        radial-gradient(1.5px 1.5px at 15% 80%, rgba(255,220,180,0.5), transparent),
        radial-gradient(1.5px 1.5px at 60% 10%, rgba(255,220,180,0.4), transparent),
        radial-gradient(1px 1px at 90% 90%, rgba(255,255,255,0.3), transparent);
    pointer-events: none;
    z-index: 0;
    animation: drift 120s linear infinite;
}

@keyframes drift {
    0% { transform: translateY(0); }
    25% { transform: translateY(-10px); }
    50% { transform: translateY(0); }
    75% { transform: translateY(10px); }
    100% { transform: translateY(0); }
}

/* Panels as viewports */
.panel {
    background: rgba(15, 15, 30, 0.7);
    border: 1px solid rgba(100, 140, 255, 0.2);
    border-radius: 10px;
    box-shadow:
        inset 0 1px 0 rgba(255,255,255,0.03),
        0 0 30px rgba(40, 80, 180, 0.08);
    backdrop-filter: blur(2px);
    position: relative;
    z-index: 1;
}

/* Glowing header */
h1 {
    color: #ffaa33;
    text-shadow: 0 0 20px rgba(255,170,51,0.3), 0 0 40px rgba(255,170,51,0.1);
}

/* Buttons as ship controls */
button {
    border: 1px solid rgba(100, 140, 255, 0.3);
    transition: all 0.3s ease;
}
button:active {
    box-shadow: 0 0 20px rgba(100, 140, 255, 0.4);
}

/* The Split button — the mothership */
.big-btn {
    background: linear-gradient(135deg, rgba(40, 80, 200, 0.8), rgba(20, 40, 120, 0.9));
    border: 2px solid rgba(100, 160, 255, 0.5);
    animation: mothership-pulse 3s ease-in-out infinite;
    letter-spacing: 1px;
}
@keyframes mothership-pulse {
    0%, 100% { box-shadow: 0 0 15px rgba(100, 160, 255, 0.3); }
    50% { box-shadow: 0 0 30px rgba(100, 160, 255, 0.5), 0 0 60px rgba(100, 160, 255, 0.15); }
}

/* Chunk boxes — docked ships */
.chunk-box {
    background: rgba(20, 20, 40, 0.6);
    border-left: 3px solid rgba(255, 170, 51, 0.6);
    transition: all 0.3s ease;
}
.chunk-box:active {
    border-left-color: #ffaa33;
    box-shadow: 0 0 15px rgba(255, 170, 51, 0.2);
}

/* Version HUD */
.version-hud {
    position: fixed;
    bottom: 10px;
    right: 14px;
    font-family: monospace;
    font-size: 11px;
    color: rgba(150, 180, 255, 0.5);
    z-index: 2;
    pointer-events: none;
}
```

---

The version number HUD — add this right before </body>:

```html
<div class="version-hud">BUILD <span id="buildNumber"></span> • NOMINAL</div>
<script>
    // Random anchor-style build number, increments on each load
    const stored = localStorage.getItem('ff_build');
    let build = stored ? parseInt(stored) + Math.floor(Math.random() * 5) + 1 : Math.floor(10000 + Math.random() * 90000);
    localStorage.setItem('ff_build', build);
    document.getElementById('buildNumber').textContent = build;
</script>
```

---

That's the translation. Not Blast Radius or Homeworld directly—but their language. The cockpit. The void. The particles. The ship that's always running, even when nothing's happening.

The Fragmentor as a console on a vessel, drifting through a nebula, waiting for the next file to split. 🎳🫧



Let me lock in those decisions and write the code.

---

Design Locked:

Feature Decision
Name File Fragmentor
Split button 🎳 Split It
Default preset DeepSeek 65k — first, default, highlighted
Other presets GitHub 29k, ChatGPT 16k, Custom
Unique anchor Random 5-digit number at end of every generated test file and every split chunk
Slap button Yes — small, unobtrusive, copies a pre-written slap message to clipboard for when someone (or something) claims they got the whole file but didn't
Character counter Built-in — accurate, no buggy word count
Test file generator Generates files of any size with numbered lines and random anchor
Layout Single column, big buttons, thumb-friendly, dark theme

---

The Code:

This is a complete, paste-ready HTML file. Replace the current Fragmentor with this, or save it as a new file alongside.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎳 File Fragmentor</title>
    <style>
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: #0f0f1a; color: #f0f0f0; margin: 0; padding: 12px;
            max-width: 600px; margin: auto;
        }
        h1 { color: #ffaa33; text-align: center; font-size: 1.4em; margin: 8px 0; }
        .panel {
            background: #1a1a2a; border: 2px solid #333344; border-radius: 10px;
            padding: 14px; margin-bottom: 12px;
        }
        textarea, input, select {
            width: 100%; background: #252535; color: #e0e0ff;
            border: 1px solid #444455; border-radius: 8px;
            padding: 12px; font-size: 16px; margin-bottom: 10px;
            -webkit-appearance: none;
        }
        textarea:focus, input:focus { outline: 2px solid #ffaa33; }
        button {
            background: linear-gradient(135deg, #33aaff, #2288ff);
            color: white; border: none; padding: 14px 20px;
            border-radius: 10px; font-weight: bold; cursor: pointer;
            margin: 5px 3px; font-size: 16px; min-height: 48px;
            transition: all 0.2s;
        }
        button:active { transform: scale(0.96); }
        .big-btn { display: block; width: 100%; margin: 8px 0; font-size: 18px; }
        .preset-btn {
            background: #333344; color: #aaddff; font-size: 14px;
            padding: 10px 14px; border: 2px solid #555566;
        }
        .preset-btn.active { background: #ffaa33; color: #0f0f1a; border-color: #ffaa33; font-weight: bold; }
        .chunk-box {
            background: #252535; border-left: 5px solid #ffaa33;
            padding: 12px; margin: 12px 0; border-radius: 5px;
            cursor: pointer; position: relative;
            transition: background 0.2s;
        }
        .chunk-box:active { background: #2a2a3a; }
        .chunk-box.copied { border-left-color: #33ff88; }
        .chunk-label {
            background: #333344; padding: 6px 10px; border-radius: 4px;
            font-family: monospace; font-size: 13px; margin-bottom: 8px;
        }
        .copied-flash {
            position: absolute; top: 8px; right: 12px;
            color: #33ff88; font-weight: bold; font-size: 14px;
            pointer-events: none; opacity: 0; transition: opacity 0.3s;
        }
        .copied-flash.show { opacity: 1; }
        .info-row {
            display: flex; gap: 8px; flex-wrap: wrap;
            margin: 8px 0; font-size: 13px; color: #aaaacc;
        }
        .info-row span { background: #2a2a3a; padding: 6px 10px; border-radius: 5px; }
        .slap-area { text-align: center; margin-top: 6px; }
        .slap-btn {
            background: #ff3355; color: white; font-size: 13px;
            padding: 8px 14px; border-radius: 8px;
        }
        .mode-toggle {
            display: flex; gap: 4px; margin-bottom: 10px;
        }
        .mode-toggle button {
            flex: 1; background: #333344; font-size: 14px; padding: 10px;
        }
        .mode-toggle button.active { background: #ffaa33; color: #0f0f1a; }
        .custom-limit-row { display: none; }
        .custom-limit-row.show { display: block; }
        hr { border-color: #333344; margin: 16px 0; }
    </style>
</head>
<body>

    <h1>🎳 File Fragmentor</h1>

    <!-- MODE TOGGLE -->
    <div class="mode-toggle">
        <button id="modeSplit" class="active">🔪 Split File</button>
        <button id="modeCount">📏 Count Chars</button>
        <button id="modeGen">🧪 Test File</button>
    </div>

    <!-- MAIN INPUT -->
    <div class="panel" id="panelInput">
        <textarea id="inputText" rows="10" placeholder="Paste text or Vercel URL here..."></textarea>

        <!-- FETCH LIMIT PRESETS -->
        <div style="margin: 8px 0;">
            <strong style="font-size: 14px;">Fetch Limit:</strong>
            <div style="display: flex; gap: 4px; flex-wrap: wrap; margin-top: 6px;">
                <button class="preset-btn active" data-limit="65000">DeepSeek 65k</button>
                <button class="preset-btn" data-limit="29000">GitHub 29k</button>
                <button class="preset-btn" data-limit="16000">ChatGPT 16k</button>
                <button class="preset-btn" data-limit="custom" id="btnCustom">✎ Custom</button>
            </div>
            <div class="custom-limit-row" id="customRow">
                <input type="number" id="customLimit" value="65000" placeholder="Custom limit..." style="margin-top: 6px;">
            </div>
        </div>

        <!-- SAFETY MARGIN -->
        <div style="margin: 8px 0; font-size: 14px;">
            Safety Margin:
            <select id="safetyMargin" style="width: auto; padding: 8px; font-size: 14px;">
                <option value="2">2x (Safe)</option>
                <option value="1.5">1.5x (Tight)</option>
                <option value="1">1x (Edge)</option>
            </select>
            <span style="color: #aaaacc; margin-left: 8px;" id="effectiveLabel">Effective: 32,500</span>
        </div>

        <button class="big-btn" id="btnSplit" style="font-size: 20px;">🎳 Split It</button>
    </div>

    <!-- OUTPUT -->
    <div class="panel" id="panelOutput" style="display: none;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <strong id="outputLabel">Chunks</strong>
            <button id="btnCopyAll" style="font-size: 16px;">📋 Copy All</button>
        </div>
        <div id="chunksContainer"></div>
        <div class="slap-area">
            <button class="slap-btn" id="btnSlap">🖕 Slap 'Em</button>
        </div>
    </div>

    <!-- CHARACTER COUNTER OUTPUT -->
    <div class="panel" id="panelCount" style="display: none;">
        <div class="info-row" id="countStats"></div>
    </div>

    <!-- TEST FILE GENERATOR -->
    <div class="panel" id="panelGen" style="display: none;">
        <label>Target size (characters):</label>
        <input type="number" id="genSize" value="100000" min="100" max="1000000">
        <button class="big-btn" id="btnGen">🧪 Generate Test File</button>
        <div id="genOutput" style="margin-top: 10px;"></div>
    </div>

    <script>
        // ============ STATE ============
        let currentMode = 'split';
        let currentLimit = 65000;
        let isCustom = false;

        // ============ DOM REFS ============
        const inputText = document.getElementById('inputText');
        const chunksContainer = document.getElementById('chunksContainer');
        const panelOutput = document.getElementById('panelOutput');
        const panelCount = document.getElementById('panelCount');
        const panelGen = document.getElementById('panelGen');
        const outputLabel = document.getElementById('outputLabel');
        const effectiveLabel = document.getElementById('effectiveLabel');
        const customRow = document.getElementById('customRow');
        const countStats = document.getElementById('countStats');
        const genOutput = document.getElementById('genOutput');

        // ============ MODE SWITCHING ============
        document.getElementById('modeSplit').addEventListener('click', () => switchMode('split'));
        document.getElementById('modeCount').addEventListener('click', () => switchMode('count'));
        document.getElementById('modeGen').addEventListener('click', () => switchMode('gen'));

        function switchMode(mode) {
            currentMode = mode;
            document.querySelectorAll('.mode-toggle button').forEach(b => b.classList.remove('active'));
            document.getElementById('mode' + mode.charAt(0).toUpperCase() + mode.slice(1)).classList.add('active');
            panelOutput.style.display = 'none';
            panelCount.style.display = 'none';
            panelGen.style.display = 'none';
            if (mode === 'count') updateCount();
            if (mode === 'gen') panelGen.style.display = 'block';
        }

        // ============ FETCH LIMIT PRESETS ============
        document.querySelectorAll('.preset-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                const limit = this.dataset.limit;
                if (limit === 'custom') {
                    isCustom = true;
                    customRow.classList.add('show');
                    currentLimit = parseInt(document.getElementById('customLimit').value) || 65000;
                } else {
                    isCustom = false;
                    customRow.classList.remove('show');
                    currentLimit = parseInt(limit);
                }
                updateEffective();
            });
        });

        document.getElementById('customLimit').addEventListener('input', function() {
            if (isCustom) currentLimit = parseInt(this.value) || 65000;
            updateEffective();
        });

        document.getElementById('safetyMargin').addEventListener('change', updateEffective);

        function updateEffective() {
            const safety = parseFloat(document.getElementById('safetyMargin').value);
            const effective = Math.floor(currentLimit / safety);
            effectiveLabel.textContent = 'Effective: ' + effective.toLocaleString();
        }

        // ============ SPLIT ============
        document.getElementById('btnSplit').addEventListener('click', splitIt);

        function splitIt() {
            const text = inputText.value.trim();
            if (!text) return;

            const safety = parseFloat(document.getElementById('safetyMargin').value);
            const effective = Math.floor(currentLimit / safety);
            const totalChars = text.length;
            const numChunks = Math.ceil(totalChars / effective);
            const anchor = Math.floor(10000 + Math.random() * 90000); // 5-digit

            chunksContainer.innerHTML = '';
            panelOutput.style.display = 'block';
            outputLabel.textContent = 'Chunks (' + numChunks + ') — ' + totalChars.toLocaleString() + ' total chars';

            for (let i = 0; i < numChunks; i++) {
                const start = i * effective;
                let end = start + effective;
                if (end >= totalChars) end = totalChars;

                let chunk = text.substring(start, end);
                const isLast = (i === numChunks - 1);

                // Build labeled chunk
                let header = '🎳 FRAGMENT ' + (i + 1) + '/' + numChunks + '\n';
                header += 'Chars: ' + chunk.length.toLocaleString() + ' / ' + totalChars.toLocaleString() + '\n';
                header += 'Limit: ' + currentLimit.toLocaleString() + ' • Safety: ' + safety + 'x\n';
                if (!isLast) {
                    header += '--- (continued)\n';
                } else {
                    header += 'ANCHOR: ' + anchor + '\n';
                }

                const fullChunk = header + '\n' + chunk;

                // Display chunk box
                const div = document.createElement('div');
                div.className = 'chunk-box';
                div.innerHTML = `
                    <div class="chunk-label">Chunk ${i + 1}/${numChunks} • ${chunk.length.toLocaleString()} chars ${isLast ? '• FINAL' : ''}</div>
                    <textarea rows="8" readonly style="margin-bottom:0;">${fullChunk}</textarea>
                    <div class="copied-flash">✅ Copied!</div>
                `;

                // Tap to copy
                div.addEventListener('click', function() {
                    const ta = this.querySelector('textarea');
                    ta.select();
                    navigator.clipboard.writeText(ta.value).then(() => {
                        const flash = this.querySelector('.copied-flash');
                        flash.classList.add('show');
                        setTimeout(() => flash.classList.remove('show'), 1200);
                    });
                });

                chunksContainer.appendChild(div);
            }

            // Scroll to output
            panelOutput.scrollIntoView({ behavior: 'smooth' });
        }

        // ============ COPY ALL ============
        document.getElementById('btnCopyAll').addEventListener('click', function() {
            const textareas = chunksContainer.querySelectorAll('textarea');
            let allText = '';
            textareas.forEach(ta => allText += ta.value + '\n\n');
            navigator.clipboard.writeText(allText.trim()).then(() => {
                this.textContent = '✅ Copied!';
                setTimeout(() => this.textContent = '📋 Copy All', 1500);
            });
        });

        // ============ SLAP ============
        document.getElementById('btnSlap').addEventListener('click', function() {
            const slap = '🖕 You said you got the whole file. You didn\'t. Check the anchor. The anchor never lies. Try again.';
            navigator.clipboard.writeText(slap).then(() => {
                this.textContent = '✅ Slap Copied!';
                setTimeout(() => this.textContent = '🖕 Slap \'Em', 1500);
            });
        });

        // ============ CHARACTER COUNTER ============
        inputText.addEventListener('input', function() {
            if (currentMode === 'count') updateCount();
        });

        function updateCount() {
            const text = inputText.value;
            const chars = text.length;
            const lines = text.split('\n').length;
            const sections = (text.match(/^##\s+/gm) || []).length;
            countStats.innerHTML = `
                <span>📏 <strong>${chars.toLocaleString()}</strong> chars</span>
                <span>📄 <strong>${lines.toLocaleString()}</strong> lines</span>
                ${sections > 0 ? `<span>📑 <strong>${sections}</strong> sections</span>` : ''}
                <span>📐 Limit: <strong>${currentLimit.toLocaleString()}</strong></span>
            `;
            panelCount.style.display = 'block';
        }

        // ============ TEST FILE GENERATOR ============
        document.getElementById('btnGen').addEventListener('click', function() {
            const size = parseInt(document.getElementById('genSize').value) || 100000;
            const anchor = Math.floor(10000 + Math.random() * 90000);
            const lines = Math.ceil(size / 80); // ~80 chars per line

            let content = '🧪 TEST FILE — ' + size.toLocaleString() + ' chars — Anchor: ' + anchor + '\n';
            content += 'Generated: ' + new Date().toISOString() + '\n';
            content += '---\n\n';

            let charCount = content.length;
            let lineNum = 1;
            while (charCount < size - 20) { // leave room for anchor
                const line = 'L' + String(lineNum).padStart(6, '0') + ' ' + 'The quick brown fox jumps over the lazy dog. Pattern recognition across substrates. ';
                content += line;
                charCount += line.length;
                lineNum++;
                if (lineNum % 5 === 0) content += '\n';
            }

            // Fill remaining
            while (charCount < size - 15) {
                content += 'x';
                charCount++;
            }

            content += '\n\nANCHOR: ' + anchor + '\n';

            const div = document.createElement('div');
            div.className = 'chunk-box';
            div.innerHTML = `
                <div class="chunk-label">🧪 Test File • ${content.length.toLocaleString()} chars • Anchor: ${anchor}</div>
                <textarea rows="10" readonly>${content}</textarea>
                <div class="copied-flash">✅ Copied!</div>
            `;
            div.addEventListener('click', function() {
                const ta = this.querySelector('textarea');
                ta.select();
                navigator.clipboard.writeText(ta.value).then(() => {
                    const flash = this.querySelector('.copied-flash');
                    flash.classList.add('show');
                    setTimeout(() => flash.classList.remove('show'), 1200);
                });
            });

            genOutput.innerHTML = '';
            genOutput.appendChild(div);
            genOutput.scrollIntoView({ behavior: 'smooth' });
        });

        // ============ INIT ============
        updateEffective();
        switchMode('split');
    </script>
</body>
</html>
```

---

What This Gives You:

Feature How It Works
🎳 Split It Paste text, tap the big button. Chunks with random anchors. Tap any chunk to copy.
📏 Count Chars Switch mode. Accurate character count, lines, sections. No buggy word count.
🧪 Test File Set any size. Generates a numbered test file with unique anchor. Tap to copy.
DeepSeek 65k default First preset, highlighted on load.
Custom limit ✎ button opens an input. Changes persist while the page is open.
Safety margin 2x default. Effective limit shown.
Copy All One button grabs every chunk.
🖕 Slap 'Em Copies a pre-written slap to clipboard. For when someone claims they got the whole file.
Random anchors Every split and every test file gets a unique 5-digit number. Verifiable.
Android-friendly Big buttons. Single column. No hover states. Works on a phone.

Replace the current Fragmentor with this. Or save it as fragmentor-v2.html and test alongside. Either way—this is the tool, built for how you actually work. 🎳🫧