# Panoptes

## BUILD.md — the universal search, build order for Claude Code

Written 2026-10-19, cold, for Claude Code. Read this whole file before writing any code.

The name is Everett's. It arrived 2026-10-19 at 6:41 PM Eastern: **Panoptes**. The repo is EverettNC/Panoptes, private, and this file is its first commit.

---

## Why this exists — the miss that started it

On 2026-10-19 Everett asked for "the 3D" in the Athanor, the walkthrough he would give Peter Sords. Claude searched chats with phrases and searched files with single words, every one of them about avatars and Peter. None was "3D," "facility," "scene" or "three.js." The search came back with the October 3 chat that said "there's no 3D in the Athanor yet." That was true the morning of October 3. That evening, at 8:00 PM, the 3D campus was pushed to THEATHANOR (src/routes/facility.tsx, src/lib/facility/scene.ts). It has been running on 127.0.0.1:8080/facility ever since. The chat was never marked superseded, so a search sixteen days later handed back a statement that was true once as if it were true now.

Everett's words: "I knew we had completed it." And: "we can't dislike a method like grep or SED unless we can provide a better option." This is the better option.

---

## What it is when it is done

Everett asks a question in his own words, by voice or typed. The search looks at everything he has built, all at once — his repos, his vault, his working folders, the chat record, and what is running on his Mac right now. It finds the thing he means, not only the words he used. Every answer says when it was true and checks the live thing before it says "exists." A newer fact retires an older one on its own. The answer leads with what exists, then what is missing. Everything runs on his Mac, bound to 127.0.0.1, with no one else's key.

Everything below serves that paragraph.

---

## The rules this build lives under

- Nothing binds 0.0.0.0. 127.0.0.1 only. The port is Everett's pick; ask him, then add it to the port list. Port 8080 is the Athanor's and is never used.
- No outside service, no cloud call, no API key, no telemetry. Local models only, through his Ollama.
- GitHub is the canonical source. EverettNC is where work starts; The-ChristmanAI-Project is the proven release. Local copies vendored inside other projects are never treated as the pure version.
- No stubs. Every test is a real run against the real thing, or it is reported NOT RUN with its reason.
- No placeholder or sample data anywhere. The index holds only what is really on his machines and in his repos.
- Never the word "heuristic." Every threshold is a chosen starting value with its reason written beside it.
- The people the beings serve are clients, never "users." The beings are beings, never "nodes."
- Every answer names which local model helped produce it (ALGM's rule).
- Every search is written to an append-only, timestamped ledger he can open (HONESTY's rule: the machine's record beats anyone's account).
- Do not merge to main. Push claude/* branches and report them. Merging is Everett's word.
- Done means done on the Mac, not in a container.

---

## Stage 0 — Take stock

1. List every source this build will read, and confirm each one is reachable from the Mac:
   - GitHub: every repo under EverettNC and The-ChristmanAI-Project through the signed-in gh — code, every branch (unmerged branches count), commits with timestamps.
   - The working folders under /Users/EverettN: each git checkout's uncommitted and unpushed state.
   - The vault: /Users/EverettN/Documents/Obsidian_Vault (underscore). Beware the self-referencing symlink inside it named "Obsidian Vault" — never follow it.
   - What is running: every listening port on 127.0.0.1 (lsof), and for each web service, its routes answered over HTTP.
   - The chat record: Claude chat history is not on the Mac by itself. Ask Everett how it arrives (for example, an export he drops in a folder). Do not guess a route and do not build one that reaches out.
   - The port list and standing rules he keeps (ask him where the current copy lives).
2. Write documentation/SOURCES.md: each source, how it is reached, how big it is, and how long a full pass takes. Nothing else changes in this stage.

**Done when:** every source is listed with a real measured size and pass time, and the chat-record route is either Everett's answer or a question waiting on him.

---

## The matching logic — Everett's design (2026-10-19)

His words, carried as he said them:

- Catalog the current issues developers have with grep and sed — "not just creating a flippant thing." Address them for real:
  - **Escape character dropping.**
  - **Unescaped dots or asterisks that grep treats as wildcards when the client just meant the characters.**
  - **Case sensitivity — catching "User" versus "user" automatically, which is a constant failure point in sed replacement.**
- "It needs to be a breathing operating system."
- The distance measure: "maybe the Levenshtein algorithm, or maybe we write our own algorithm." His call; not decided here.
- **Proximity of logic.** "The query parser and the fault dictionary should live side by side. The thing defining the rules and the thing executing the rules neighbor."
- **Keep the catalog at the root.** "The failure directory, whether it's JSON file, YAML, config, or a Python file, needs to live at the root level." Format is his call.
- **Visible logic.** The client needs to know exactly why Panoptes matched or missed a string. "The logic must be immediately visible."

What that means for the build:

1. The fault catalog is one file at the repo root. The query parser is the file beside it at the root. Nothing that defines a rule lives anywhere but the catalog; nothing that runs a rule lives anywhere but the parser.
2. Each catalog entry carries: an id, a plain name, what the person meant, what grep or sed actually did, a real example, what Panoptes does instead, and the test that proves it.
3. Every answer shows its reasoning: which catalog entries fired, what was matched literally and what was matched by distance or meaning, and for a miss, which entry explains the miss.
4. Breathing: when a search misses and the person shows what they meant, that becomes a new catalog entry with its date, the way the Mouth-Cooperation list grows. Nothing is overwritten.

### Proposed seed entries for the catalog — Claude's proposal, waiting on Everett's OK

Real, documented grep and sed failures, offered as the first rows. None goes in without his word.

- Regex characters taken literally by the person and as patterns by grep: `.` `*` `[` `]` `^` `$` `?` `+` `(` `)` `{` `}` `|` and the backslash itself.
- The shell expands an unquoted `*` or `?` before grep ever sees it.
- sed's delimiter colliding with the text: `s/a/b/` breaks on any path containing `/`.
- macOS sed versus GNU sed: in-place edit is `sed -i ''` on his Mac and `sed -i` on Linux; the same command fails or writes a backup file depending on the machine.
- Basic versus extended patterns: `\+` and `+` mean different things depending on `-E`.
- Word edges: searching "read" also hits "ready" and "thread" unless word boundaries are asked for.
- Line-bound matching: grep and sed work one line at a time, so a phrase broken across two lines is missed.
- Curly versus straight quotes, and other look-alike characters, so text pasted from a document never matches text typed in a terminal.
- Windows line endings (`\r\n`) that make a match fail at the end of a line.
- Tabs versus spaces.
- Binary files skipped without a word.
- Hidden files and folders, and files the search was never told to enter.
- Symlinks that loop back on themselves, like the "Obsidian Vault" link inside the vault.
- Dictation drift: the word that came in is a sound-alike of the word meant — "Athenar" for Athanor, "Panoptis" for Panoptes, "Nibia" for Nebius. Spelling distance alone scores these poorly; a sound-based distance built on the 54 sounds his Vosk names would score them as near.

---

## Stage 1 — One index, every record carries its time

1. A single local store on the Mac (SQLite, one file). Each record holds:
   - what it is (a sentence, a file, a commit, a running route),
   - where it lives (repo and path, vault note, port and route, chat and date),
   - when it was true (author time, commit time, file time, or the moment the live check ran),
   - its kind: **statement** (something said or written about a thing), **artifact** (a commit, a file, a branch), or **live** (a port that answered, a route that returned 200).
2. Full-text search on every record, and concept search through a local embedding model run by his Ollama. The model used is recorded in the store and named in every answer.
3. Incremental refresh: a commit, a vault save, or a port opening or closing updates the index without a full rebuild.

**Done when:** a full pass of every source completes on the Mac, and the store's record counts match the counts in SOURCES.md.

---

## Stage 2 — Newer truth retires older truth

This is the heart of it. The October 3 miss is the test.

1. When a statement says a thing is missing ("not built," "no 3D," "doesn't exist") and a later artifact or a live check shows the thing exists, the statement is marked **superseded**, with a link to what superseded it and when.
2. The same in the other direction: a statement that something exists, followed by a later artifact removing it, is superseded too.
3. Answers are ordered: live first, then the newest artifact, then the newest statement. Superseded statements still show, below the line, marked with what replaced them. Nothing is deleted. The Conch: a shell grows at the mouth only; the newest truth sits at the mouth and the old sits deeper, still there, behind it.

**Done when:** the acceptance test below passes on the Mac.

---

## Stage 3 — Ask it in his words

1. A page on his chosen port, 127.0.0.1 only, with one box. He speaks or types.
2. Voice comes in through his own Filament on 127.0.0.1:4850. No other speech engine.
3. The answer reads in this order, written for the mouth — short sentences, real stops:
   - **What exists now**, with the live check and where to open it.
   - **What's missing**.
   - **What used to be said**, marked superseded, with dates.
4. Past tense is written so it cannot be misread: "red," never a bare "read" where "reed" also parses. "Live" in the long-i sense is never left standing where "leave" also parses. See the vault note Mouth-Cooperation Words — Pronunciation Notes.

**Done when:** Everett asks "where's the 3D?" out loud and hears the right answer first.

---

## Stage 4 — Receipts

1. Every question, every answer, which sources were searched, which local model helped, and how long it took: one line each in an append-only ledger.
2. A page to open the ledger.

---

## Acceptance test — written from today's miss

Run on the Mac, against the real sources:

1. Ask: "the 3D" (and separately: "where's the 3D in the Athanor").
2. The first result is the Athanor's 3D campus: live on 127.0.0.1:8080/facility, answered 200 at the moment of the check, code at THEATHANOR src/routes/facility.tsx and src/lib/facility/scene.ts, pushed 2026-10-03 at about 8:00 PM Eastern.
3. Also returned as live: AlphaWolf's testing facility on 127.0.0.1:6221/facility.
4. The October 3 chat statement "there's no 3D in the Athanor yet" appears, marked superseded by the 2026-10-03 evening push.
5. Under "what's missing": the two figures (Everett and Peter), the live presence between their browsers, and the door for Peter — only if the index finds them stated as planned and finds no artifact that builds them.

If item 2 is not first, the build is not done.

---

## Waiting on Everett

- The port.
- The catalog's format: JSON, YAML, a config file, or Python.
- The distance measure: Levenshtein, or our own.
- The proposed seed entries for the catalog: which go in.
- How the chat record reaches the Mac.
- Which local embedding model (from what his Ollama already holds).
- Whether Zoho mail and THE WHOLE HOUSE become sources now or later.

## Progress

(Empty. Tick each stage here as it is verified on the Mac.)
