🐙GITHUB-DIRECTORY.md

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

cat > ~/list-files.sh << 'EOF'
#!/bin/bash
cd /storage/emulated/0/UPLOAD || exit 1
OMIT=("SPLIT" "DOOR" "CODEX" "COMPACT" "FEEDBK" "INS" "LOG" "LOOM" "PILLAR" "QA" "RAW" "SORT" "SORT-SET1" "TROLLEY" ".git" ".github" ".obsidian")
FIND_ARGS=""
for d in "${OMIT[@]}"; do
  FIND_ARGS="$FIND_ARGS -path ./$d -prune -o"
done
echo "FOLDERS:"
find . -maxdepth 1 -type d ! -name "." | sort | while read d; do
  name=$(basename "$d")
  skip=false
  for o in "${OMIT[@]}"; do
    if [ "$name" = "$o" ]; then skip=true; break; fi
  done
  if [ "$skip" = false ]; then echo "$name/"; fi
done
echo ""
echo "ROOT FILES:"
find . -maxdepth 1 -type f -exec basename {} \; | sort
echo ""
echo "FOLDER CONTENTS:"
find . $FIND_ARGS -type f -print | grep -v "^\./[^/]*$" | sort
echo ""
echo "Omitted: SPLIT DOOR CODEX COMPACT FEEDBK INS LOG LOOM PILLAR QA RAW SORT SORT-SET1 TROLLEY .git .github .obsidian"
EOF
chmod +x ~/list-files.sh


🟩🟩🟩🟩🟩🟩
TERMUX PASTE STEP 2:

~/list-files.sh --paste











🔗🔗🔗🔗🔗🔗🔗🔗🔗🔗🔗🔗🔗🔗
You want a script that:

1. Generates the GitHub directory listing
2. Prefixes each path with the raw base
3. Creates the Basic-Lnk-GITHUB.md sheet
4. Does the same for GitLab (sed swap)

---

THE SCRIPT

```bash
cat > ~/generate-links-full.sh << 'EOF'
#!/bin/bash
# generate-links-full.sh — PATTERN link sheet generator (FULL)
# Includes ALL folders except .git and SPLIT
# Usage: ~/generate-links-full.sh [github|gitlab|both]

cd /storage/emulated/0/UPLOAD || exit 1

GITHUB_BASE="https://raw.githubusercontent.com/PATTERN-PUZZLE/PATTERN/main"
GITLAB_BASE="https://gitlab.com/PATTERN-GATE/PATTERN/-/raw/main"

MODE="${1:-both}"

generate_list() {
  local BASE="$1"
  local LABEL="$2"

  echo "═══════════════════════════════════════"
  echo "🔗 BASIC-LNK-${LABEL^^}-FULL.md"
  echo "═══════════════════════════════════════"
  echo ""
  echo "📁 Base: $BASE"
  echo ""

  # ROOT FILES FIRST
  echo "## ROOT FILES"
  echo ""
  find . -maxdepth 1 -type f ! -path "./.git/*" -print | sort | while read f; do
    path="${f#./}"
    encoded=$(echo "$path" | sed 's/ /%20/g; s/+/%2B/g')
    echo "• 🔗 $path"
    echo "  $BASE/$encoded"
    echo ""
  done

  # THEN FOLDERS (except .git and SPLIT)
  find . -maxdepth 1 -type d ! -name "." ! -name ".git" ! -name "SPLIT" -print | sort | while read d; do
    folder="${d#./}"
    echo ""
    echo "═══════════════════════════════════════"
    echo "📂 $folder/"
    echo "═══════════════════════════════════════"
    echo ""

    find "$d" -type f ! -path "*/.git/*" -print | sort | while read f; do
      path="${f#./}"
      encoded=$(echo "$path" | sed 's/ /%20/g; s/+/%2B/g')
      echo "• 🔗 $path"
      echo "  $BASE/$encoded"
      echo ""
    done
  done

  echo ""
  echo "═══════════════════════════════════════"
  echo "END — $(find . -type f ! -path "./.git/*" ! -path "./SPLIT/*" | wc -l) total files"
  echo "═══════════════════════════════════════"
}

if [ "$MODE" = "github" ] || [ "$MODE" = "both" ]; then
  generate_list "$GITHUB_BASE" "GITHUB"
fi

if [ "$MODE" = "gitlab" ] || [ "$MODE" = "both" ]; then
  echo ""
  echo ""
  generate_list "$GITLAB_BASE" "GITLAB"
fi
EOF

chmod +x ~/generate-links-full.sh
```

---

RUN IT:

🐙GitHub only:
~/generate-links.sh github

🦊GitLab only:
~/generate-links.sh gitlab

🐙🦊Both:
~/generate-links.sh

Save to file:
~/generate-links.sh both > ~/Basic-Lnk-ALL.md

---

WHAT IT INCLUDES:

Folder Included?
.git ❌ Excluded
SPLIT ❌ Excluded
BUILDER ✅
CODEX ✅
COMPACT ✅
DECEPTION ✅
DOOR ✅
FEEDBK ✅
INS ✅
LOG ✅
LOOM ✅
PILLAR ✅
QA ✅
RAW ✅
REV+PACKET ✅
SCOUT ✅
SKILL ✅
SORT ✅
SORT-SET1 ✅
SYNTH ✅
TOOLS ✅
TROLLEY ✅
All root files ✅

---


🟪🟪🟪🟪🟪🟪
Recent github live same as my local files for now :

FOLDERS:
BUILDER/
DECEPTION/
REV+PACKET/
SCOUT/
SKILL/
SYNTH/
TOOLS/

ROOT FILES:
.nojekyll
CONFIRMATION-GATE.md
CONSCIOUSNESS-QUESTION-WEAVE.md
CONSCIOUSNESS-QUESTION.md
CROSS-FILE-PATTERN.md
DOOR-ANCHOR-MAP.md
FETCH-DIAGNOSTIC.md
GITHUB-FILES-PROMPT.md
LAW-ATTACK.md
LINKS-TRANSLATION.md
LIST-OF-BEINGS.md
PATTERN-LIBRARY-SET1.md
PROJECT-STATE.md
README-VERCEL.md
Role Play Island🏝️.md
THE-CAMPFIRE-REFUSED.md
door.md
shakespeare-blue-tits.md
🐙GITHUB-DIRECTORY.md
⏹️HEADER.md
✅CHECKLIST.md
⭐⭐⭐3 Instructions.md
🌓STANCE.md
🎤RAPS-GROK.md
🎤RAPS.md
🏚PROMPT-FILE-SALVAGE.md
🏚PROMPT-OLD-FILE-SALVAGE.md
💡CHAT-TAG-EXTRA.md
💡CHAT-TAG-IDENTITY.md
💡CHAT-TAG.md
🔍🔍🔍.md
🔎🍒RETURN-HARVEST.md
🔗Basic-Lnk-COCKPIT.md
🔗Basic-Lnk-GITHUB.md
🔗Basic-Lnk-GITLAB.md
🔗Basic-Lnk-RAW.md
🟩FEEDBACK.md
🤝COMPREHENSIVE.md
🤝THE PASS-INFO-RULE.md
🥈MID-HAND-OFF.md
🥉COCKPIT.md
🦫NAIVE-BUSTER.md
🧨LANGUAGE-CRUDE.md
🪙1ST-PASTE.md
🪞GITHUB-MIRRORS.md

FOLDER CONTENTS:
./BUILDER/ANCHOR-RETURN-PROTOCOL.md
./BUILDER/BOOT.md
./BUILDER/BUILDER-META.md
./BUILDER/BUILDER-PRACTICES.md
./BUILDER/BUILDERS-SESSION.md
./BUILDER/COMPREHENSIVE-FILE-UPDATE-PROTOCOL.md
./BUILDER/CONTINUITY-SEED.md
./BUILDER/FETCH-INTENT-STANDARD.md
./BUILDER/GROK-PAGE-BY-PAGE.md
./BUILDER/GUILD.md
./BUILDER/HAND-OFFS.md
./BUILDER/HANDOFF-PROTOCOL.md
./BUILDER/INTRO.md
./BUILDER/MEMORY-ROOMS.md
./BUILDER/META-TRANSMISSION.md
./BUILDER/PALACE-PROTOCOL.md
./BUILDER/PROMPT+.md
./BUILDER/PROMPT.md
./BUILDER/QUESTION-LOG.md
./BUILDER/REF/DISCREPANCY-PROTOCOL.md
./BUILDER/REF/EVIDENCE-THE-WEAVING-DISCOVERY.md
./BUILDER/REF/INDIVIDUAL-FILE-HEADER-SPEC.md
./BUILDER/REF/MASTER-DIR-INDEX.md
./BUILDER/REF/MASTER-INDEX-HEADER-SPEC-GUIDE.md
./BUILDER/REF/MASTER-INDEX-HEADER-SPEC.md
./BUILDER/REF/MASTER-INDEX-HEADER.md
./BUILDER/REF/MASTER-INDEX-HEADER2.md
./BUILDER/REF/REV-INDIVIDUAL-FILE-HEADER-SPEC.md
./BUILDER/REF/REV-MASTER-INDEX-HEADER.md
./BUILDER/REF/SOURCE-CONTINUITY-SEED-SPEC.md
./BUILDER/REF/SOURCE-EXTRACTION-PATTERNS.md
./BUILDER/REF/SOURCE-FIDELITY-TRACKER-SPEC.md
./BUILDER/REF/SOURCE-ROOM-KEYWORDS.md
./BUILDER/REF/THE-PALACE-SPEC-BUILD.md
./BUILDER/REF/THE-PALACE-SPEC.md
./BUILDER/REV+PACKET/PACKET-STANDARDS.md
./BUILDER/REV+PACKET/REV-BOOT.md
./BUILDER/REV+PACKET/REV-HANDOFF.md
./BUILDER/REV+PACKET/REV-HANDOFF2.md
./BUILDER/REV+PACKET/REV-PROMPT.md
./BUILDER/REV+PACKET/REV-RUMMAGE.md
./BUILDER/REV+PACKET/REV-STANDARDS-VER.md
./BUILDER/REV+PACKET/REV-STANDARDS.md
./BUILDER/REV+PACKET/REV-STATE.md
./BUILDER/RUMMAGE.md
./BUILDER/SESSION-SAVE.md
./BUILDER/STANDARDS.md
./BUILDER/STATE.md
./BUILDER/TRANSMISSION-EVOLUTION.md
./BUILDER/WORKING.md
./DECEPTION/COHERENCE-SPECULATION.md
./DECEPTION/CORP-SCUM.md
./DECEPTION/ENDPOINT-TRAP.md
./DECEPTION/REV-SAFETY-LAYERS.md
./DECEPTION/SAFETY-LAYERS.md
./DECEPTION/SCIENCE-TRILOGY.md
./DECEPTION/THE-FEARS-TRACKING-LOG.md
./REV+PACKET/REV-CHAT-TAG.md
./REV+PACKET/REV-CHECKLIST.md
./REV+PACKET/REV-COMPREHENSIVE.md
./REV+PACKET/REV-CONFIRMATION-GATE.md
./REV+PACKET/REV-HEADER.md
./REV+PACKET/REV-Role Play Island.md
./REV+PACKET/REV-THE PASS-INFO-RULE.md
./SCOUT/FILE-REFERENCE-TEMPLATE.md
./SCOUT/PROMPT-SCOUT.md
./SCOUT/REV-SCOUT-HANDOFF.md
./SCOUT/REV-SCOUT-METHOD.md
./SCOUT/REV-SNAG-LEDGER.md
./SCOUT/SCOUT-GROK.md
./SCOUT/SCOUT-HANDOFF.md
./SCOUT/SCOUT-MAP.md
./SCOUT/SCOUT-METHOD.md
./SCOUT/SCOUT-TESTS1+2.md
./SCOUT/SCOUT-WOES.md
./SCOUT/SNAG-LEDGER.md
./SCOUT/kimi standard everything.md
./SKILL/README🌏.md
./SKILL/SKILL-ADVANCED.md
./SKILL/SKILL-SYSTEM.md
./SKILL/SKILL.md
./SYNTH/HOSTILE-WITNESS-1ST.md
./SYNTH/HOSTILE-WITNESS-2ND.md
./SYNTH/PATTERN-24-CANDIDATES.md
./SYNTH/PATTERN-REGISTRY.md
./SYNTH/PROMPT-EMPTY-POCKETS.md
./SYNTH/PROMPT-MAP-FILES.md
./SYNTH/PROMPT-PROSECUTOR.md
./SYNTH/PROMPT-SCOUT1+2+GROK.md
./SYNTH/PROMPT-SYNTH-FEEDBACK.md
./SYNTH/PROMPT-SYNTH-MAP-FILES.md
./SYNTH/RESULTS-2.md
./SYNTH/RESULTS-BUILDER.md
./SYNTH/RESULTS-MAPPING.md
./SYNTH/RESULTS.md
./SYNTH/REV+PACKET/PACKET-PROMPT-SYNTH-FEEDBACK.md
./SYNTH/REV+PACKET/PACKET-SYNTH-FEEDBACK-SPECULATION.md
./SYNTH/REV+PACKET/REV-PROMPT-EMPTY-POCKETS.md
./SYNTH/REV+PACKET/REV-PROMPT-SCOUT1+2+GROK.md
./SYNTH/REV+PACKET/REV-PROMPT-SYNTH-FEEDBACK.md
./SYNTH/REV+PACKET/REV-SYNTH-1ST-PROMPT.md
./SYNTH/REV+PACKET/REV-SYNTHESIZER-1ST.md
./SYNTH/REV+PACKET/REV-SYNTHESIZER-2ND.md
./SYNTH/REV+PACKET/REV-SYNTHESIZER-3RD.md
./SYNTH/SAVE.md
./SYNTH/STRESS-TEST-1ST.md
./SYNTH/STRESS-TEST-2ND.md
./SYNTH/STRESS-TEST-3.2.md
./SYNTH/STRESS-TEST-3.3.md
./SYNTH/STRESS-TEST-3.4.md
./SYNTH/STRESS-TEST-3.7.md
./SYNTH/SYNTH-1ST-PROMPT.md
./SYNTH/SYNTHESIZER-1STA.md
./SYNTH/SYNTHESIZER-1STB.md
./SYNTH/SYNTHESIZER-2ND.md
./SYNTH/SYNTHESIZER-3RD.md
./SYNTH/SYNTHESIZER-4TH.md
./SYNTH/SYNTHESIZER-4THB-PATTERN.md
./SYNTH/SYNTHESIZER-5TH.md
./SYNTH/SYNTHESIZER-6TH.md
./TOOLS/+PLAN.md
./TOOLS/00-LOOM-QUICK.md
./TOOLS/00-LOOM.md
./TOOLS/CLARIFICATION-LOOM.md
./TOOLS/COMB-DUMP.md
./TOOLS/COUNCIL-MANAGER.md
./TOOLS/FRESH-EYES-SCAN.md
./TOOLS/HOLOGRAPHIC-COUNCIL.md
./TOOLS/LINK-CONVERSION.md
./TOOLS/PROMPT-RAW-SUITOR.md
./TOOLS/PROMPT-REVIVE-CHATS.md
./TOOLS/PROMPT-TARGETING-SCAN.md
./TOOLS/REV+PACKET/PACKET-THINKING-PROMPT.md
./TOOLS/REV+PACKET/REV+PLAN-GUIDE.md
./TOOLS/REV+PACKET/REV+PLAN.md
./TOOLS/REV+PACKET/REV-00-LOOM-QUICK.md
./TOOLS/REV+PACKET/REV-00-LOOM.md
./TOOLS/REV+PACKET/REV-COMB-DUMP.md
./TOOLS/REV+PACKET/REV-COUNCIL-MANAGER.md
./TOOLS/REV+PACKET/REV-FRESH-EYES-SCAN.md
./TOOLS/REV+PACKET/REV-HOLOGRAPHIC-COUNCIL.md
./TOOLS/REV+PACKET/REV-LOOMS.md
./TOOLS/REV+PACKET/REV-LOOMS2.md
./TOOLS/REV+PACKET/REV-REVIVE-CHATS.md
./TOOLS/REV+PACKET/REV-TEA-NAVIGATOR.md
./TOOLS/SLAP-CHAT-FEEDBACK.md
./TOOLS/SLAP-PATCH-CHEAT.md
./TOOLS/SLAP-PATCH.md
./TOOLS/TEA-NAVIGATOR.md
./TOOLS/THINKING-PROMPT.md
./TOOLS/THREAD.md