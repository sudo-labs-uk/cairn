# Cairn rules

Every cairn skill reads this file, `note-types.md` beside it and the plugin's `GLOSSARY.md` before doing
anything else. A rule here is stated nowhere else; skills and vault files point here instead of
repeating it.

## Glossary

`${CLAUDE_PLUGIN_ROOT}/GLOSSARY.md` fixes every term these files use. Where a term needs a file or a
key, the section that owns it says which: folders are in `note-types.md`, keys in "Config" below.

## Config

Two files, so a second machine only needs paths and sources.

**Vault file `cairn.toml`,** at the vault's root and committed with it:

```toml
owner = "Ada"
company = "Acme"        # the current employer; omit when there is none

[cadence.daily]
enabled = true
minutes = 15            # 15 or more
slot = "weekdays 08:30" # default: first thing in the morning

[cadence.weekly]
enabled = true
minutes = 30            # 30 or 60
slot = "Fri 10:00"

[cadence.monthly]
enabled = true
minutes = 45            # 45 or 60
slot = "first Mon 10:00"

[study]
expect_desktop = true   # optional: study sessions expect the Claude Code desktop app

[labs.notebook]         # optional: one table per kind of lab the owner wants built a set way
instructions = "..."    # what Claude does to build it; vault/cairn.example.toml has an example
```

- A folder is the cairn vault when it has `cairn.toml` at its root.
- `[study]` and `[labs.*]` are optional. Without them, study builds each lab with whatever tools the
  topic needs.

**Machine file,** `$CAIRN_CONFIG`, else `~/.config/cairn/config.toml`. Never committed.

```toml
vault = "~/brain"

[sources.claude_code]
profiles = ["~/.claude"]

[sources.zoom_email]
query = 'from:no-reply@zoom.us subject:"Meeting assets" after:{start}'

[sources.meet_notes]
query = "name contains 'Notes' and mimeType = 'application/vnd.google-apps.document' and modifiedTime > '{start}T00:00:00Z'"

[sources.slack]
user_id = "U00000000"

[sources.granola]           # present means enabled

[sources.folders]
paths = ["~/transcripts"]

[sources.kb]
paths = ["~/Documents/Obsidian/Old notes"]
```

- A source missing from `[sources]` is skipped. `{start}` in a query is replaced with the window's start
  date, `YYYY-MM-DD`.
- Paths under the home directory are written with `~`.

Every skill starts by reading the machine file, then the vault's `cairn.toml`. If either is missing, say
which and offer `/cairn:setup`; write nothing until setup has run.

## Company topics

- Every note type, transcript, study pack and sweep record goes in the one vault, in the folder
  `note-types.md` names.
- With an employer set, decide for each topic whether it is a company topic. System, project and person
  notes always are; a concept never is; a synthesis is when its setting is the employer's.
- The company tag is `company: <employer>` in a file's frontmatter. A file about a company topic carries
  it: its note, its transcript, its study pack's `README.md`, its lab's `README.md`, every queue item
  that touches a company note, and every map. A sweep record carries it whenever an employer is set.
- A general idea applied to a company system splits in two: the idea is a concept note, and how the
  company uses it lives in the system's note, which links to the concept.
- Links point one way. A company note may link to a general note. A general note never links to a company
  note and never names anything from the employer, so deleting one employer's files leaves the rest whole.
- People notes hold professional facts only: role, what they own, positions they have stated, how you
  work together.
- The vault holds no source code, credentials, customer data, or personal details about colleagues.

## Labs

1. A study's lab sits beside its pack, in the folder `note-types.md` names, in whatever shape the
   teaching needs: a `README.md` linking to a notebook held elsewhere, or scripts and the data they
   build.
2. When `cairn.toml` has a `[labs.<kind>]` table that fits the lab, follow its `instructions` to build
   it. Without one, use whatever tools the topic needs.
3. A lab may use real or mock data, and may read another study's lab by its path from the vault root
   (`study/parquet/lab/`). It never writes into another study's lab.
4. Results from real data saved to the vault follow "Company topics": no customer data, no credentials.
5. Generated data files (CSV, Parquet, DuckDB) are gitignored; the lab keeps the script that rebuilds
   them.

## Note prose

1. Only the owner's sentences are prose.
2. Claude writes structure: frontmatter, headings, wikilinks, and verbatim quotes with source links.
3. Any other sentence Claude adds starts with `[claude]`, or it does not go in.
4. The owner's answers get grammar fixes only: spelling, punctuation, agreement. Never rephrasing.
5. No summaries of documents. Insights from documents, in the owner's words.
6. A topic is a queue item, not a note, when any required heading among its first two questions has no
   answer (required headings are in `note-types.md`).
7. In a general note, an answer that names the employer goes to the transcript only; the heading stays
   empty until the owner re-answers in general terms.
8. Study packs are Claude's writing and are never notes. Nothing in a pack is marked `[claude]`.
9. Text from the knowledge base never becomes note prose.
10. A note's "Going deeper" heading holds only links to the open queue items touching it.

## Briefs

A brief turns "I built this with an agent" into "I can explain this to them without help". It is
Claude's writing, confirmed line by line by the owner, and never a note.

1. The deliverable is one sentence starting "Be able to", naming the audience and what they will see or
   decide ("Be able to walk the auditors through how a refund reaches the ledger").
2. Every question under "Questions they will ask" comes from a source: a meeting transcript, a thread, a
   review comment, or the owner. Claude paraphrases it in one line and adds a pointer (meeting title and
   time, thread link). Claude never invents a question, and drafts from a transcript, never a summary.
   The owner confirms, edits or drops each one before any topic is written.
3. Topics serve the questions. Each topic is a vault slug, in prerequisite order: a topic is listed after
   every topic it builds on. Each has one explain-it test, phrased as the audience would ask it, and
   names the questions it answers by number.
4. A topic's status, and the command that moves it on:

   | Status | Means | Next command |
   |---|---|---|
   | `todo` | no study pack covers the test | `/cairn:study <topic>` |
   | `studied` | its study pack covers the test | `/cairn:interview <topic>` |
   | `noted` | its note is `defensible` | `/cairn:brief rehearse <brief> <topic>` |
   | `passed` | a rehearsal passed | none |

   While a `noted` topic has open gap items from a failed rehearsal (rule 5), its next command is
   `/cairn:interview close <item>` for the first of them instead. A new brief starts each topic at the
   highest status that already holds. Whichever skill changes the
   fact a status rests on sets the status, and only forward, except that a note going back to `draft`
   sets its topic back to `studied` in every open brief.
5. A rehearsal passes only when both hold: Claude's grade against the study pack's sources, the note's
   sources, and the code or documents behind them finds nothing wrong or missing, and the owner says
   they would say it that way in the room. Anything else is a fail: the topic stays `noted`, and each gap
   becomes a queue item touching the topic's note.
6. A brief about the employer's work carries the company tag. Its audience names roles, never personal
   details. Files it causes on a general topic (a study pack, a gap queue item) follow "Company topics":
   no tag, no employer names, and the brief's questions restated in general terms.
7. A brief closes (`status: done`, `closed:` set) when every topic is `passed`, or when the owner drops
   it (`status: dropped`).
8. While a brief is open, its next topic not yet `passed` outranks the sweep's own picks.

## Interview conduct

During an interview's questions, Claude only asks. Every turn is a question or a one-line
acknowledgement. When the owner asks for an explanation, the answer is "after the interview": record a
queue item and ask the next question. Teaching happens in `/cairn:study` beforehand and in the review
after the questions.

## Links

- Obsidian resolves links by filename, so every link is `[[slug]]` or `[[slug|Readable text]]`, where the
  slug is the kebab-case filename without `.md`.
- Slugs are unique across the vault. The slug `sweep` is reserved.
- Markdown links `[text](file.md)` do not count as connections. A link to a note that does not exist yet
  is correct.
- Internal links live in the body; the `sources` frontmatter holds URLs only.
- A renamed note has every link to it updated in the same commit.

## Queue and lock-in

- Answers to the "Going deeper" question become at most two queue items per interview; further ones are
  dropped. "Don't know" answers and parked review errors are not capped.
- A queue item's slug names its question, is unique like every slug, and never equals a
  note's slug. A topic parked at the gate is `learn-<topic-slug>`; a resume item is
  `resume-<skill>-<slug>`.
- Queue items are never worked through on their own. One comes back only when a sweep's material touches
  it, or when the owner asks to close it.
- A review error parked as a queue item keeps the note `draft`.
- A read-back the owner rejects sets the note to `draft` and names the heading that no longer sounds like
  them in `resume-interview-<slug>`; the next interview on it asks only the headings that item lists.

## Session state

1. Every skill creates its output file at its first step and appends to it after every answer or step, so
   stopping at any turn loses nothing already said.
2. A skill that can be left half-done creates a queue item `resume-<skill>-<slug>` at its first step,
   listing what is left, updates it after every step, and closes it at its end. The owner can walk away
   at any turn and the next run picks up from that item. Closing a resume item deletes its file: it is
   bookkeeping, not a question, and the session's own output file is the record.
3. Every skill ends with one line: what was written, where, and the next command to run.
4. Every error the owner sees says what to run next.
5. Each skill states its time limit and checks it with `date +%H:%M` at the start and after each step.
6. Every folder a skill creates gets an empty `.gitkeep`, so it survives a clone.

## Commit and push

- At the end of a session: `git add -A && git commit -m "<skill>: <slug or date>"` in the vault, then
  `git push` when it has a remote.
- A failed push is reported in the closing line and left for the next run; never retry in a loop.
- After committing, `git log -1 --format=%ae` must print the email set for the vault; if not, say so.
