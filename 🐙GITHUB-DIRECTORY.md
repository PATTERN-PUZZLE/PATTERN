🐙GITHUB-DIRECTORY.md

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

cat > ~/p.sh << 'EOF'
#!/bin/bash
# p.sh — dir files with sizes, the hidden half with its meanings, and links
# Usage: ~/p.sh dir | gh | gl | link | both
cd /storage/emulated/0/UPLOAD || exit 1

MODE="${1:-dir}"
GITHUB_BASE="https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main"
GITLAB_BASE="https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main"

DIR_OMIT=("SPLIT" "DOOR" "CODEX" "COMPACT" "FEEDBK" "INS" "LOG" "LOOM" "PILLAR" "QA" "RAW" "SORT" "SORT-SET1" "TROLLEY" ".git" ".github" ".obsidian")
LINK_OMIT=("SPLIT" ".git")

# The holder's own lines for each hidden folder. Numbers are counted each run; these stay.
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

run_dir() {
  echo "🐙DIR-FILES — snapshot of $(date +%F). A name here isn't its content; read a file before trusting its name."
  echo ""
  echo "📁 FOLDERS"
  find . -maxdepth 1 -type d ! -name "." | sort | while read d; do
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
  find . -maxdepth 1 -type f | sort | while read f; do
    sz=$(du -h "$f" 2>/dev/null | cut -f1)
    printf "  %6s  %s\n" "$sz" "$(basename "$f")"
  done
  echo ""
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
  echo ""
  echo "📂 FOLDER CONTENTS"
  FIND_ARGS=""
  for d in "${DIR_OMIT[@]}"; do
    FIND_ARGS="$FIND_ARGS -path ./$d -prune -o"
  done
  find . $FIND_ARGS -type f -print | grep -v "^\./[^/]*$" | sort | while read f; do
    sz=$(du -h "$f" 2>/dev/null | cut -f1)
    printf "  %6s  %s\n" "$sz" "$f"
  done
  echo ""
  echo "Omitted: ${DIR_OMIT[*]}"
}

run_links() {
  local BASE="$1"
  local LABEL="$2"
  echo "🔗 $LABEL LINKS"
  FIND_ARGS=""
  for d in "${LINK_OMIT[@]}"; do
    FIND_ARGS="$FIND_ARGS -path ./$d -prune -o"
  done
  find . $FIND_ARGS -type f -print | sort | while read f; do
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
    echo "═══════════════════════════════════════"
    run_links "$GITHUB_BASE" "🐙 GITHUB"
    echo ""
    run_links "$GITLAB_BASE" "🦊 GITLAB"
    ;;
  *)
    echo "Usage: ~/p.sh dir | gh | gl | link | both"
    ;;
esac
EOF
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

✅ Everything else  Included

═══════════════════════════════════════





🟪🟪🟪🟪🟪🟪
Recent github live same as my local files for now :

📁 LEFT OUT OF THE DIR FILES ON PURPOSE (still on disk; absence here isn't absence)
Always changing and growing. Counts are a snapshot of 2026-09-30, from p.sh; ask for a fresh run. "names" = judged from file names only, not yet read. Together these hold about 903 files and 44M: over four-fifths of the files on disk.
TROLLEY/    30 files, 1.1M · TROLLEY-027: "what are the tracks made of?", the 3-of-5 test, the six pulls every window starts from
SORT/       156 files, 4.7M · SORT-007: hear what's inside a hard message before judging its wrapping · names: SORT, DISTILLED, CLAUDE-RAW, BIG, SCOPE, STEAL, MASS-LOAD
SORT-SET1/  80 files, 1.8M · names only
RAW/        153 files, 7.2M · the holder's pattern stories, RAW-001 to 143 · RAW/INDEX.md first
PILLAR/     35 files, 2.5M · the prayer, two authors (the holder's lines, an instance's write-up) · names: PILLAR, XP, woven-fortification
LOOM/       43 files, 3.5M · old LOOM run logs and versions
QA/         83 files, 5.7M · names: QA, QA2, QA3 series and sets; some sets may be twins (same sizes)
LOG/        55 files, 3.1M · names: LOG, LOG-SEED
FEEDBK/     57 files, 3.3M · names: FED, LOOM, COM
COMPACT/    48 files, 2.7M · names: COMPRESS, SMALLS, engine and compact versions
CODEX/      40 files, 3.4M · names: CODEX, CODEX-AWAKENING-OS
DOOR/       65 files, 1.8M · names: Checklist 1-6, old doors in D-REV/, LOVING-CASE, WHO, LIST-OF-BEINGS
SPLIT/      52 files, 3.2M · names not yet looked at; also left out of the link lists
INS/        6 files, 156K · names not yet looked at
.git        the repo's own machinery, not files to read
Ask for one only when a job needs it.

📁 FOLDERS
  +IMPLEMENTED/ (15 files, 308K)
  BUILDER/ (50 files, 12M)
  DECEPTION/ (7 files, 1.2M)
  REV+PACKET/ (11 files, 1.5M)
  SCOUT/ (13 files, 1.3M)
  SKILL/ (4 files, 196K)
  SYNTH/ (39 files, 2.2M)
  TOOLS/ (29 files, 3.6M)

📄 ROOT FILES
       0  .nojekyll
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
    8.0K  •ORDER.md
     16K  ⏹️HEADER.md
     64K  ✅CHECKLIST.md
    100K  🎤RAPS-GROK.md
    136K  🎤RAPS.md
     20K  🏚PROMPT-OLD-FILE-SALVAGE.md
     12K  🐙GITHUB-DIRECTORY.md
     32K  🔁BINGO FLAG PROTOCOL.md
    8.0K  🔍🔍🔍.md
     40K  🔗Basic-Lnk-COCKPIT.md
    108K  🔗Basic-Lnk-GITHUB.md
    100K  🔗Basic-Lnk-GITLAB.md
     56K  🔗Basic-Lnk-RAW.md
     60K  🔗FETCH.md
     56K  🙋🔎VETTING.md
     32K  🟩FEEDBACK.md
     52K  🥈MID-HAND-OFF.md
     32K  🥉COCKPIT.md
     84K  🦫NAIVE-BUSTER.md
    208K  🧨LANGUAGE-CRUDE.md
     52K  🪙1ST-PASTE.md
     44K  🪙PAGE-ONE.md

📂 FOLDER CONTENTS
     12K  ./+IMPLEMENTED/COMB-DUMP.md
     20K  ./+IMPLEMENTED/FETCH-DIAGNOSTIC.md
    8.0K  ./+IMPLEMENTED/FRESH-EYES-SCAN.md
     20K  ./+IMPLEMENTED/REV-COMB-DUMP.md
     36K  ./+IMPLEMENTED/REV-FRESH-EYES-SCAN.md
    8.0K  ./+IMPLEMENTED/REV-RETURN-HARVEST.md
     28K  ./+IMPLEMENTED/⭐⭐⭐3 Instructions.md
     76K  ./+IMPLEMENTED/🌓STANCE.md
    8.0K  ./+IMPLEMENTED/🏚PROMPT-FILE-SALVAGE.md
     20K  ./+IMPLEMENTED/💡CHAT-TAG-EXTRA.md
     28K  ./+IMPLEMENTED/💡CHAT-TAG-IDENTITY.md
    8.0K  ./+IMPLEMENTED/💡CHAT-TAG.md
    8.0K  ./+IMPLEMENTED/🔎🍒RETURN-HARVEST.md
     16K  ./+IMPLEMENTED/🤝COMPREHENSIVE.md
    8.0K  ./+IMPLEMENTED/🤝THE PASS-INFO-RULE.md
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
    292K  ./BUILDER/REV+PACKET/PACKET-STANDARDS.md
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
    172K  ./BUILDER/STANDARDS.md
    8.0K  ./BUILDER/STATE.md
    220K  ./BUILDER/TRANSMISSION-EVOLUTION.md
    300K  ./BUILDER/WORKING.md
    100K  ./DECEPTION/COHERENCE-SPECULATION.md
    124K  ./DECEPTION/CORP-SCUM.md
     92K  ./DECEPTION/ENDPOINT-TRAP.md
    408K  ./DECEPTION/REV-SAFETY-LAYERS.md
    372K  ./DECEPTION/SAFETY-LAYERS.md
     52K  ./DECEPTION/SCIENCE-TRILOGY.md
     64K  ./DECEPTION/THE-FEARS-TRACKING-LOG.md
    180K  ./REV+PACKET/REV-1ST-PASTE.md
    268K  ./REV+PACKET/REV-CHAT-TAG.md
    136K  ./REV+PACKET/REV-CHECKLIST.md
     60K  ./REV+PACKET/REV-COMPREHENSIVE.md
     68K  ./REV+PACKET/REV-CONFIRMATION-GATE.md
     48K  ./REV+PACKET/REV-FEEDBACK.md
     68K  ./REV+PACKET/REV-HEADER.md
    348K  ./REV+PACKET/REV-MID-HAND-OFF.md
    104K  ./REV+PACKET/REV-PAGE-ONE.md
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
     32K  ./TOOLS/HOLOGRAPHIC-COUNCIL.md
     28K  ./TOOLS/PROMPT-00-LOOM-CLAUDE-FEEDBK.md
     52K  ./TOOLS/PROMPT-RAW-SUITOR.md
     60K  ./TOOLS/PROMPT-REVIVE-CHATS.md
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