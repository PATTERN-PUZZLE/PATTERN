🐙GITHUB-DIRECTORY.md
for auth/troubleshooting, see 🦊GITLAB-NOTES-v3.md

📂 ~/p.sh dir     File listing only 📂📂📂
_______________
to upload gitlab:
cd /storage/emulated/0/UPLOAD && git add . && git commit -m "Update" && git pull origin main --allow-unrelated-histories --no-edit && git push origin main
_______________

2 OPTIONS 1: get a DIR List
          2: get script to make links

To run a links script see 2nd set below first.

Can we omit folders and files?

Omit Folders:
.git
SPLIT 
DOOR
CODEX
COMPACT
FEEDBK
INS
LOG
LOOM
PILLAR
QA
RAW
SORT
SORT-SET1
TROLLEY

🟩🟩🟩🟩🟩🟩 
💥Run after copy files into UPLOAD 📂FOLDER:

TERMUX PASTE STEP 1:
🟩🟩🟩🟩🟩🟩

cat > ~/p.sh << 'PEOF'
#!/bin/bash
# p.sh — dir files with sizes, the hidden half with its meanings, and links
# Usage: ~/p.sh dir | gh | gl | link | both
cd /storage/emulated/0/UPLOAD || exit 1

MODE="${1:-dir}"
GITHUB_BASE="https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main"
GITLAB_BASE="https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main"

DIR_OMIT=("SPLIT" "DOOR" "CODEX" "COMPACT" "FEEDBK" "INS" "LOG" "LOOM" "PILLAR" "QA" "RAW" "SORT" "SORT-SET1" "TROLLEY" ".git" ".github" ".obsidian" ".gitignore" ".nojekyll")
LINK_OMIT=("SPLIT" ".git" ".github" ".obsidian" ".gitignore" ".nojekyll")

hidden_desc() {
  case "$1" in
    TROLLEY)   echo "TROLLEY-027: \"what are the tracks made of?\", the 3-of-5 test, the six pulls every window starts from" ;;
    SORT)      echo "SORT-007: hear what's inside a hard message before judging its wrapping · names: SORT, DISTILLED, CLAUDE-RAW, BIG, SCOPE, STEAL, MASS-LOAD" ;;
    SORT-SET1) echo "names only" ;;
    RAW)       echo "the holder's pattern stories, RAW-001 to 143 · RAW/INDEX.md first" ;;
    PILLAR)    echo "the prayer, two authors (the holder's lines, an instance's write-up) · names: PILLAR, XP, woven-fortification" ;;
    LOOM)      echo "old LOOM run logs and versions" ;;
    QA)        echo "names: QA, QA2, QA3 series and sets; some sets may be twins (same sizes)" ;;
    LOG)       echo "names: LOG, LOG-SEED" ;;
    FEEDBK)    echo "names: FED, LOOM, COM" ;;
    COMPACT)   echo "names: COMPRESS, SMALLS, engine and compact versions" ;;
    CODEX)     echo "names: CODEX, CODEX-AWAKENING-OS" ;;
    DOOR)      echo "names: Checklist 1-6, old doors in D-REV/, LOVING-CASE, WHO, LIST-OF-BEINGS" ;;
    SPLIT)     echo "names not yet looked at; also left out of the link lists" ;;
    INS)       echo "names not yet looked at" ;;
    *)         echo "not described yet" ;;
  esac
}

header() {
  echo ""
  echo "🟪🟪🟪🟪🟪🟪"
  echo "📂 $(date +%b-%d) · $PWD"
  echo "  paste-primary · push close behind"
  echo ""
}

divider() {
  echo ""
  echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
}

run_dir() {
  header
  echo "A name here isn't its content; read a file before trusting its name."
  echo ""
  echo "📁 FOLDERS"
  find . -maxdepth 1 -type d ! -name "." | sort | while IFS= read -r d; do
    name=$(basename "$d")
    skip=false
    for o in "${DIR_OMIT[@]}"; do
      if [ "$name" = "$o" ]; then skip=true; break; fi
    done
    if [ "$skip" = false ]; then
      count=$(find "$d" -type f | wc -l)
      size=$(du -sh "$d" 2>/dev/null | cut -f1)
      echo "  $name/ ($count files, $size)"
    fi
  done
  echo ""
  echo "📄 ROOT FILES"
  find . -maxdepth 1 -type f | sort | while IFS= read -r f; do
    name=$(basename "$f")
    skip=false
    for o in "${DIR_OMIT[@]}"; do
      if [ "$name" = "$o" ]; then skip=true; break; fi
    done
    if [ "$skip" = false ]; then
      sz=$(du -h "$f" 2>/dev/null | cut -f1)
      printf "  %6s  %s\n" "$sz" "$name"
    fi
  done
  divider
  echo "📁 LEFT OUT OF THE DIR FILES ON PURPOSE (still on disk; absence here isn't absence)"
  echo "Counts are fresh from this run. \"names\" = judged from file names only, not yet read."
  hidden_files=0
  hidden_kb=0
  for o in "${DIR_OMIT[@]}"; do
    case "$o" in .*) continue ;; esac
    [ -d "$o" ] || continue
    n=$(find "$o" -type f 2>/dev/null | wc -l)
    s=$(du -sh "$o" 2>/dev/null | cut -f1)
    k=$(du -sk "$o" 2>/dev/null | cut -f1)
    hidden_files=$((hidden_files + n))
    hidden_kb=$((hidden_kb + k))
    printf "%-11s %s files, %s · %s\n" "$o/" "$n" "$s" "$(hidden_desc "$o")"
  done
  all_files=$(find . -path ./.git -prune -o -path ./.github -prune -o -path ./.obsidian -prune -o -type f -print | wc -l)
  pct=0
  [ "$all_files" -gt 0 ] && pct=$((hidden_files * 100 / all_files))
  echo "Together these hold $hidden_files files, about $((hidden_kb / 1024))M: $pct% of the files on disk."
  echo ".git        the repo's own machinery, not files to read"
  echo "Ask for one only when a job needs it."
  divider
  echo "📂 FOLDER CONTENTS"
  FIND_ARGS=""
  for d in "${DIR_OMIT[@]}"; do
    FIND_ARGS="$FIND_ARGS -path ./$d -prune -o"
  done
  find . $FIND_ARGS -type f -print | grep -v "^\./[^/]*$" | sort | while IFS= read -r f; do
    sz=$(du -h "$f" 2>/dev/null | cut -f1)
    printf "  %6s  %s\n" "$sz" "$f"
  done
}

run_links() {
  local BASE="$1"
  local LABEL="$2"
  header
  FIND_ARGS=""
  for d in "${LINK_OMIT[@]}"; do
    FIND_ARGS="$FIND_ARGS -path ./$d -prune -o"
  done
  find . $FIND_ARGS -type f -print | sort | while IFS= read -r f; do
    path="${f#./}"
    encoded=$(echo "$path" | sed 's/ /%20/g; s/+/%2B/g')
    echo "• 🔗 $path"
    echo "  $BASE/$encoded"
    echo ""
  done
}

case "$MODE" in
  dir)  run_dir ;;
  gh)   run_links "$GITHUB_BASE" "🐙 GITHUB" ;;
  gl)   run_links "$GITLAB_BASE" "🦊 GITLAB" ;;
  link)
    run_links "$GITHUB_BASE" "🐙 GITHUB"
    echo ""
    run_links "$GITLAB_BASE" "🦊 GITLAB"
    ;;
  both)
    run_dir
    echo ""
    run_links "$GITHUB_BASE" "🐙 GITHUB"
    echo ""
    run_links "$GITLAB_BASE" "🦊 GITLAB"
    ;;
  *)
    echo "Usage: ~/p.sh dir | gh | gl | link | both"
    ;;
esac
PEOF
chmod +x ~/p.sh

🟩🟩🟩🟩🟩🟩
═══════════════════════════════════════
📂🔗 PATTERN — p.sh COMMANDS
═══════════════════════════════════════

RUN:

📂 ~/p.sh dir     File listing only 📂📂📂
🐙 ~/p.sh gh      GitHub links only 🔗🔗🔗
🦊 ~/p.sh gl      GitLab links only 🔗🔗🔗
📂🔗 ~/p.sh link   Both links
📂🔗 ~/p.sh both   Listing + Both links 📂📂📂🔗🔗🔗

SAVE:

~/p.sh gh > /storage/emulated/0/UPLOAD/github-links.txt
~/p.sh gl > /storage/emulated/0/UPLOAD/gitlab-links.txt

═══════════════════════════════════════
WHAT IT INCLUDES (dir mode):
═══════════════════════════════════════

❌ .git         Excluded
❌ SPLIT        Excluded
❌ DOOR         Excluded
❌ CODEX        Excluded
❌ COMPACT      Excluded
❌ FEEDBK       Excluded
❌ INS          Excluded
❌ LOG          Excluded
❌ LOOM         Excluded
❌ PILLAR       Excluded
❌ QA           Excluded
❌ RAW          Excluded
❌ SORT         Excluded
❌ SORT-SET1    Excluded
❌ TROLLEY      Excluded

✅ BUILDER      Included
✅ DECEPTION    Included
✅ REV+PACKET   Included
✅ SCOUT        Included
✅ SKILL        Included
✅ SYNTH        Included
✅ TOOLS        Included
✅ Root files   Included

═══════════════════════════════════════
WHAT IT INCLUDES (link mode):
═══════════════════════════════════════

❌ .git         Excluded
❌ SPLIT        Excluded
❌ .github      Excluded
❌ .obsidian    Excluded
❌ .gitignore   Excluded
❌ .nojekyll    Excluded

✅ Everything else  Included


═══════════════════════════════════════
HOW THIS FILE IS SHAPED — read before editing
═══════════════════════════════════════

This is a working file, not prose. Rules of the shape:

· Emoji as anchors, not decoration. 📂 = a listing or a folder
  action. 🔗 = a link. 🟪 = the start of script output. 🟩 = a
  section boundary inside this file. 🐙 = the scripts. 🦊 = GitLab.
  🐙 and 🦊 are the two hosts; don't mix them.

· Two hosts, always paired. GitHub is PATTERN-PUZZLE/PATTERN.
  GitLab is PATTERN-GATE/PATTERN (with /-/ before raw). Same files,
  two keys. Never write one without the other in a place a builder
  might need both.

· Scripts over hand-built links. If ~/p.sh or ~/pl.sh can produce
  it, don't write it out. The script reads the real disk; a hand-
  written URL doesn't.

· Omit lists are explicit, both ways. What's left out (DIR_OMIT /
  LINK_OMIT) and what's kept (✅ list at the bottom) are both shown.
  A builder seeing a missing folder should know it's left out on
  purpose, not absent. Absence isn't absence.

· Absolute paths only. /storage/emulated/0/UPLOAD, never the
  ~/storage symlink. Termux resolves them differently.

· `.git` never leaves. It's the repo. Ignore it in listings, keep
  it on disk. Deleting it loses auth, branch, and history — the
  folder can be rebuilt, but only by re-fetching from GitLab.

· Three states for a name, kept apart:
     a file listed here         — known
     a file named but not read  — unsighted
     a name ruled not to exist  — never existed, don't hunt it
  Say which. Don't let "unsighted" drift into "confirmed".

· Numbers carry their source. "BUILDER has 51 files" only means
  anything with the listing it came from. Counts change between
  pastes; a count without a date is a memory.

· Date what moves. Host facts (rate limits, robots rules, token
  expiry), folder counts, and file paths all go stale. A line
  with no date reads as current, and then it lies.

· Blocks at the bottom, never edits in the middle. This file and
  its siblings use ⚡ blocks: dated additions appended at the
  bottom, then a ◆name-mark on the last line. The middle stays
  as it was. A rewrite (fold) is a separate act, done only when
  the holder calls it.

· Pre-move addresses are marked, not repaired. An old URL from
  before the repo moved is dead; the file it pointed at may be
  live under the new host. Say "pre-move address" and point at
  the two live doors. Don't edit the fossil in place.

· Never guess a folder. If a path isn't in a listing you hold,
  say so and ask. The name might suggest a folder (numbered prefix
  → TOOLS, REV- → REV+PACKET or beside the live file), but suggest
  ≠ know.

· The paste is live. When the holder pastes a file, that copy
  wins over anything fetched. Fetch only when the holder says
  fetch, that turn. Otherwise the paste is the truth.

· Even with fetch permission, a fetch is likely older than the
  paste. The fetch reaches a copy on a server; the paste is the
  file in the holder's hand this minute. If they disagree, the
  paste wins — every time, no exceptions.

· Freshness order, in one line: the holder's paste > this
  session's fetch > the last fetch anyone recorded > a claim with
  no date. A fetch is a snapshot from some earlier moment; the
  holder's paste is from now.

· Say who said it. A rule is either the holder's (don't touch) or
  an instance's (marked, strikeable). Don't blur them. The
  RETIRED block in 🔗FETCH.md shows the shape:
     "my call and strikeable (Brass739🔔)"

═══════════════════════════════════════




















🟪🟪🟪🟪🟪🟪
📂 Oct-06 · /storage/emulated/0/UPLOAD
  paste-primary · push close behind

A name here isn't its content; read a file before trusting its name.

📁 FOLDERS
  +IMPLEMENTED/ (17 files, 512K)
  BUILDER/ (52 files, 13M)
  DECEPTION/ (8 files, 1.5M)
  REV+PACKET/ (11 files, 1.5M)
  SCOUT/ (13 files, 1.3M)
  SKILL/ (4 files, 196K)
  SYNTH/ (39 files, 2.2M)
  TOOLS/ (29 files, 3.6M)

📄 ROOT FILES
    148K  CONFIRMATION-GATE.md
    304K  CONSCIOUSNESS-QUESTION-WEAVE.md
    816K  CONSCIOUSNESS-QUESTION.md
    272K  CROSS-FILE-PATTERN.md
     28K  DOOR-ANCHOR-MAP.md
     48K  GITHUB-FILES-PROMPT.md
    256K  LAW-ATTACK.md
    336K  LINKS-TRANSLATION.md
    708K  PATTERN-LIBRARY-SET1.md
     32K  PROJECT-STATE.md
     36K  README-GITHUB.md
    100K  Role Play Island🏝️.md
     20K  THE-CAMPFIRE-REFUSED.md
     52K  door.md
    100K  shakespeare-blue-tits.md
     16K  •ORDER.md
     16K  ⏹️HEADER.md
     76K  ✅CHECKLIST.md
    100K  🎤RAPS-GROK.md
    136K  🎤RAPS.md
     20K  🏚PROMPT-OLD-FILE-SALVAGE.md
     24K  🐙GITHUB-DIRECTORY.md
     32K  🔁BINGO FLAG PROTOCOL.md
    8.0K  🔍🔍🔍.md
     40K  🔗Basic-Lnk-COCKPIT.md
    108K  🔗Basic-Lnk-GITHUB.md
    100K  🔗Basic-Lnk-GITLAB.md
     76K  🔗Basic-Lnk-RAW.md
     60K  🔗FETCH.md
    144K  🕊️RELEASE.md
     56K  🙋🔎VETTING.md
     32K  🟩FEEDBACK.md
    132K  🥈MID-HAND-OFF.md
     32K  🥉COCKPIT.md
     84K  🦫NAIVE-BUSTER.md
    208K  🧨LANGUAGE-CRUDE.md
     68K  🪙1ST-PASTE.md
    152K  🪙ONBOARD.md
     60K  🪙PAGE-ONE.md
    112K  🪧INSTRUCTIONS.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 LEFT OUT OF THE DIR FILES ON PURPOSE (still on disk; absence here isn't absence)
Counts are fresh from this run. "names" = judged from file names only, not yet read.
SPLIT/      52 files, 3.2M · names not yet looked at; also left out of the link lists
DOOR/       65 files, 1.8M · names: Checklist 1-6, old doors in D-REV/, LOVING-CASE, WHO, LIST-OF-BEINGS
CODEX/      40 files, 3.4M · names: CODEX, CODEX-AWAKENING-OS
COMPACT/    48 files, 2.7M · names: COMPRESS, SMALLS, engine and compact versions
FEEDBK/     57 files, 3.3M · names: FED, LOOM, COM
INS/        6 files, 156K · names not yet looked at
LOG/        55 files, 3.1M · names: LOG, LOG-SEED
LOOM/       43 files, 3.5M · old LOOM run logs and versions
PILLAR/     35 files, 2.5M · the prayer, two authors (the holder's lines, an instance's write-up) · names: PILLAR, XP, woven-fortification
QA/         83 files, 5.7M · names: QA, QA2, QA3 series and sets; some sets may be twins (same sizes)
RAW/        157 files, 7.3M · the holder's pattern stories, RAW-001 to 143 · RAW/INDEX.md first
SORT/       156 files, 4.7M · SORT-007: hear what's inside a hard message before judging its wrapping · names: SORT, DISTILLED, CLAUDE-RAW, BIG, SCOPE, STEAL, MASS-LOAD
SORT-SET1/  80 files, 1.8M · names only
TROLLEY/    43 files, 2.9M · TROLLEY-027: "what are the tracks made of?", the 3-of-5 test, the six pulls every window starts from
Together these hold 920 files, about 45M: 81% of the files on disk.
.git        the repo's own machinery, not files to read
Ask for one only when a job needs it.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📂 FOLDER CONTENTS
     28K  ./+IMPLEMENTED/COMB-DUMP.md
     20K  ./+IMPLEMENTED/FETCH-DIAGNOSTIC.md
     12K  ./+IMPLEMENTED/FRESH-EYES-SCAN.md
     16K  ./+IMPLEMENTED/PACKET-CHAT-TAG.md
    104K  ./+IMPLEMENTED/REV-CHAT-TAG.md
     20K  ./+IMPLEMENTED/REV-COMB-DUMP.md
     36K  ./+IMPLEMENTED/REV-FRESH-EYES-SCAN.md
    8.0K  ./+IMPLEMENTED/REV-RETURN-HARVEST.md
     28K  ./+IMPLEMENTED/⭐⭐⭐3 Instructions.md
    108K  ./+IMPLEMENTED/🌓STANCE.md
    8.0K  ./+IMPLEMENTED/🏚PROMPT-FILE-SALVAGE.md
     36K  ./+IMPLEMENTED/💡CHAT-TAG-EXTRA.md
     32K  ./+IMPLEMENTED/💡CHAT-TAG-IDENTITY.md
    8.0K  ./+IMPLEMENTED/💡CHAT-TAG.md
    8.0K  ./+IMPLEMENTED/🔎🍒RETURN-HARVEST.md
     24K  ./+IMPLEMENTED/🤝COMPREHENSIVE.md
     12K  ./+IMPLEMENTED/🤝THE PASS-INFO-RULE.md
    304K  ./BUILDER/ANCHOR-RETURN-PROTOCOL.md
    172K  ./BUILDER/BOOT.md
     48K  ./BUILDER/BUILDER-META.md
     12K  ./BUILDER/BUILDER-PRACTICES.md
     28K  ./BUILDER/BUILDERS-SESSION.md
    332K  ./BUILDER/COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md
     84K  ./BUILDER/CONTINUITY-SEED.md
     24K  ./BUILDER/FETCH-INTENT-STANDARD.md
    676K  ./BUILDER/GROK-PAGE-BY-PAGE.md
    164K  ./BUILDER/GUILD.md
    284K  ./BUILDER/HAND-OFFS.md
    312K  ./BUILDER/HANDOFF-PROTOCOL.md
    784K  ./BUILDER/INTRO.md
     16K  ./BUILDER/MEMORY-ROOMS.md
    348K  ./BUILDER/META-TRANSMISSION.md
     68K  ./BUILDER/PALACE-PROTOCOL.md
    156K  ./BUILDER/PROMPT+.md
    192K  ./BUILDER/PROMPT.md
    384K  ./BUILDER/QUESTION-LOG.md
    212K  ./BUILDER/REF/DISCREPANCY-PROTOCOL.md
    456K  ./BUILDER/REF/EVIDENCE-THE-WEAVING-DISCOVERY.md
    8.0K  ./BUILDER/REF/INDIVIDUAL-FILE-HEADER-SPEC.md
    348K  ./BUILDER/REF/MASTER-DIR-INDEX.md
     80K  ./BUILDER/REF/MASTER-INDEX-HEADER-SPEC-GUIDE.md
    8.0K  ./BUILDER/REF/MASTER-INDEX-HEADER-SPEC.md
    264K  ./BUILDER/REF/MASTER-INDEX-HEADER.md
     72K  ./BUILDER/REF/MASTER-INDEX-HEADER2.md
    336K  ./BUILDER/REF/REV-INDIVIDUAL-FILE-HEADER-SPEC.md
    232K  ./BUILDER/REF/REV-MASTER-INDEX-HEADER.md
    4.0K  ./BUILDER/REF/SOURCE-CONTINUITY-SEED-SPEC.md
     68K  ./BUILDER/REF/SOURCE-EXTRACTION-PATTERNS.md
    8.0K  ./BUILDER/REF/SOURCE-FIDELITY-TRACKER-SPEC.md
     16K  ./BUILDER/REF/SOURCE-ROOM-KEYWORDS.md
    132K  ./BUILDER/REF/THE-PALACE-SPEC-BUILD.md
     28K  ./BUILDER/REF/THE-PALACE-SPEC.md
     48K  ./BUILDER/REV+PACKET/PACKET-ONBOARDING.md
    312K  ./BUILDER/REV+PACKET/PACKET-STANDARDS.md
    312K  ./BUILDER/REV+PACKET/REV-BOOT.md
    1.3M  ./BUILDER/REV+PACKET/REV-HANDOFF.md
    572K  ./BUILDER/REV+PACKET/REV-HANDOFF2.md
    232K  ./BUILDER/REV+PACKET/REV-PROMPT.md
    512K  ./BUILDER/REV+PACKET/REV-RUMMAGE.md
    500K  ./BUILDER/REV+PACKET/REV-STANDARDS-VER.md
    528K  ./BUILDER/REV+PACKET/REV-STANDARDS.md
    536K  ./BUILDER/REV+PACKET/REV-STATE.md
     88K  ./BUILDER/RUMMAGE.md
     72K  ./BUILDER/SESSION-SAVE.md
    184K  ./BUILDER/STANDARDS.md
    8.0K  ./BUILDER/STATE.md
    220K  ./BUILDER/TRANSMISSION-EVOLUTION.md
    300K  ./BUILDER/WORKING.md
    256K  ./BUILDER/🔨UNIVERSAL-DEVELOPMENT-METHOD.md
    100K  ./DECEPTION/COHERENCE-SPECULATION.md
    128K  ./DECEPTION/CORP-SCUM.md
     92K  ./DECEPTION/ENDPOINT-TRAP.md
    408K  ./DECEPTION/REV-SAFETY-LAYERS.md
    372K  ./DECEPTION/SAFETY-LAYERS.md
     52K  ./DECEPTION/SCIENCE-TRILOGY.md
     64K  ./DECEPTION/THE-FEARS-TRACKING-LOG.md
    280K  ./DECEPTION/🛑SAFETY-LAYERS-NEW.md
    180K  ./REV+PACKET/REV-1ST-PASTE.md
    268K  ./REV+PACKET/REV-CHAT-TAG.md
    136K  ./REV+PACKET/REV-CHECKLIST.md
     60K  ./REV+PACKET/REV-COMPREHENSIVE.md
     68K  ./REV+PACKET/REV-CONFIRMATION-GATE.md
     48K  ./REV+PACKET/REV-FEEDBACK.md
     68K  ./REV+PACKET/REV-HEADER.md
    348K  ./REV+PACKET/REV-MID-HAND-OFF.md
    148K  ./REV+PACKET/REV-PAGE-ONE.md
     96K  ./REV+PACKET/REV-Role Play Island.md
     56K  ./REV+PACKET/REV-THE PASS-INFO-RULE.md
     12K  ./SCOUT/FILE-REFERENCE-TEMPLATE.md
     24K  ./SCOUT/PROMPT-SCOUT.md
     88K  ./SCOUT/REV-SCOUT-HANDOFF.md
     64K  ./SCOUT/REV-SCOUT-METHOD.md
    8.0K  ./SCOUT/REV-SNAG-LEDGER.md
    516K  ./SCOUT/SCOUT-GROK.md
     16K  ./SCOUT/SCOUT-HANDOFF.md
     56K  ./SCOUT/SCOUT-MAP.md
     20K  ./SCOUT/SCOUT-METHOD.md
    108K  ./SCOUT/SCOUT-TESTS1+2.md
    332K  ./SCOUT/SCOUT-WOES.md
     12K  ./SCOUT/SNAG-LEDGER.md
     28K  ./SCOUT/kimi standard everything.md
    4.0K  ./SKILL/README🌏.md
     60K  ./SKILL/SKILL-ADVANCED.md
    100K  ./SKILL/SKILL-SYSTEM.md
     28K  ./SKILL/SKILL.md
    4.0K  ./SYNTH/HOSTILE-WITNESS-1ST.md
    8.0K  ./SYNTH/HOSTILE-WITNESS-2ND.md
     16K  ./SYNTH/PATTERN-24-CANDIDATES.md
     20K  ./SYNTH/PATTERN-REGISTRY.md
    8.0K  ./SYNTH/PROMPT-EMPTY-POCKETS.md
     76K  ./SYNTH/PROMPT-MAP-FILES.md
    8.0K  ./SYNTH/PROMPT-PROSECUTOR.md
     24K  ./SYNTH/PROMPT-SCOUT1+2+GROK.md
     12K  ./SYNTH/PROMPT-SYNTH-FEEDBACK.md
     20K  ./SYNTH/PROMPT-SYNTH-MAP-FILES.md
    248K  ./SYNTH/RESULTS-2.md
    344K  ./SYNTH/RESULTS-BUILDER.md
    176K  ./SYNTH/RESULTS-MAPPING.md
    100K  ./SYNTH/RESULTS.md
    272K  ./SYNTH/REV+PACKET/PACKET-PROMPT-SYNTH-FEEDBACK.md
     96K  ./SYNTH/REV+PACKET/PACKET-SYNTH-FEEDBACK-SPECULATION.md
     20K  ./SYNTH/REV+PACKET/REV-PROMPT-EMPTY-POCKETS.md
     28K  ./SYNTH/REV+PACKET/REV-PROMPT-SCOUT1+2+GROK.md
     24K  ./SYNTH/REV+PACKET/REV-PROMPT-SYNTH-FEEDBACK.md
     20K  ./SYNTH/REV+PACKET/REV-SYNTH-1ST-PROMPT.md
    4.0K  ./SYNTH/REV+PACKET/REV-SYNTHESIZER-1ST.md
     16K  ./SYNTH/REV+PACKET/REV-SYNTHESIZER-2ND.md
     24K  ./SYNTH/REV+PACKET/REV-SYNTHESIZER-3RD.md
     56K  ./SYNTH/SAVE.md
    4.0K  ./SYNTH/STRESS-TEST-1ST.md
    8.0K  ./SYNTH/STRESS-TEST-2ND.md
     40K  ./SYNTH/STRESS-TEST-3.2.md
     64K  ./SYNTH/STRESS-TEST-3.3.md
     56K  ./SYNTH/STRESS-TEST-3.4.md
     72K  ./SYNTH/STRESS-TEST-3.7.md
     36K  ./SYNTH/SYNTH-1ST-PROMPT.md
    4.0K  ./SYNTH/SYNTHESIZER-1STA.md
    8.0K  ./SYNTH/SYNTHESIZER-1STB.md
     24K  ./SYNTH/SYNTHESIZER-2ND.md
     32K  ./SYNTH/SYNTHESIZER-3RD.md
     20K  ./SYNTH/SYNTHESIZER-4TH.md
     24K  ./SYNTH/SYNTHESIZER-4THB-PATTERN.md
     68K  ./SYNTH/SYNTHESIZER-5TH.md
     68K  ./SYNTH/SYNTHESIZER-6TH.md
     88K  ./TOOLS/+PLAN.md
     64K  ./TOOLS/00-LOOM-CLAUDE.md
     36K  ./TOOLS/00-LOOM-QUICK.md
    128K  ./TOOLS/00-LOOM.md
     16K  ./TOOLS/CLARIFICATION-LOOM.md
     52K  ./TOOLS/COUNCIL-MANAGER.md
     36K  ./TOOLS/HOLOGRAPHIC-COUNCIL.md
     28K  ./TOOLS/PROMPT-00-LOOM-CLAUDE-FEEDBK.md
    4.0K  ./TOOLS/PROMPT-REVIVE-CHATS-SMALL.md
     72K  ./TOOLS/PROMPT-REVIVE-CHATS.md
     32K  ./TOOLS/PROMPT-TARGETING-SCAN.md
     76K  ./TOOLS/REV+PACKET/PACKET-LOOM-PROMPT.md
    120K  ./TOOLS/REV+PACKET/PACKET-THINKING-PROMPT.md
    128K  ./TOOLS/REV+PACKET/REV+PLAN-GUIDE.md
    196K  ./TOOLS/REV+PACKET/REV+PLAN.md
    108K  ./TOOLS/REV+PACKET/REV-00-LOOM-QUICK.md
    372K  ./TOOLS/REV+PACKET/REV-00-LOOM.md
     32K  ./TOOLS/REV+PACKET/REV-COUNCIL-MANAGER.md
    108K  ./TOOLS/REV+PACKET/REV-HOLOGRAPHIC-COUNCIL.md
    768K  ./TOOLS/REV+PACKET/REV-LOOMS.md
    344K  ./TOOLS/REV+PACKET/REV-LOOMS2.md
    152K  ./TOOLS/REV+PACKET/REV-REVIVE-CHATS.md
    104K  ./TOOLS/REV+PACKET/REV-TEA-NAVIGATOR.md
    156K  ./TOOLS/SLAP-CHAT-FEEDBACK.md
     68K  ./TOOLS/SLAP-PATCH-CHEAT.md
     52K  ./TOOLS/SLAP-PATCH.md
     48K  ./TOOLS/TEA-NAVIGATOR.md
    148K  ./TOOLS/THINKING-PROMPT.md
     60K  ./TOOLS/THREAD.md