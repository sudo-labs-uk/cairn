---
name: brief
description: Work back from something the owner has to explain to a named audience by a date. Writes a brief (the deliverable, the questions that audience asked, drawn from transcripts and threads, and the topics needed to answer them, in prerequisite order, each with a closed-book explain-it test), then drives each topic through /cairn:study, /cairn:interview and a rehearsal in which Claude plays the audience and grades the answer against the sources. Use when they type /cairn:brief new <deliverable>, /cairn:brief <slug>, /cairn:brief rehearse <slug> [topic], say "I have to present X to Y", "I need to be able to explain X without help", "prepare me for the meeting with Y", or "rehearse X".
---

# Brief

Read `${CLAUDE_PLUGIN_ROOT}/shared/rules.md` and `${CLAUDE_PLUGIN_ROOT}/shared/note-types.md` before
anything else. "Briefs" in `rules.md` holds the rules this skill follows.

Study, interview and sweep start from a topic. A brief starts from a deliverable: who the owner has to
face, what they will ask, and what the owner has to be able to say without help. Every topic it lists
earns its place by answering one of those questions.

## Modes

- `/cairn:brief new <deliverable>`: write a brief. 20 minutes.
- `/cairn:brief <slug>`: show where the brief stands and start its next step.
- `/cairn:brief rehearse <slug> [topic]`: one rehearsal, on the named topic or the brief's first topic
  not yet `passed`. 10 minutes.
- No argument: list open briefs with their due date and next topic, and ask which one.

## New

1. **Open.** Run `date +%H:%M` and state the 20-minute limit with its end time. Read the config as
   "Config" in `rules.md` says. Pick a kebab-case slug unique in the vault, and decide whether the brief
   is about the employer's work (rule 6 in "Briefs"). Create `briefs/<slug>.md` from
   `templates/brief.md`, creating `briefs/` with a `.gitkeep` if it does not exist, with the company tag
   when it applies, and the queue item `queue/resume-brief-<slug>.md` from `templates/queue.md`, titled
   "resume brief: <slug>", listing the steps left. Done when both files exist.
2. **Deliverable.** Ask one question: who is the audience, by when, and what do they need to see or
   decide? Write `audience:` and `due:`, and draft the "Be able to" sentence (rule 1). Show it; apply the
   owner's edit. Done when the owner says yes to the sentence.
3. **Questions.** Ask where the audience's questions came from: a meeting, a thread, a review, or the
   owner's own list. Read each source in full: a meeting's transcript through the source the machine
   file configures (Granola, Meet notes, Zoom email), not its summary; a thread through Slack. List every
   question the audience asked or left unanswered, at most ten, each paraphrased in one line with a
   pointer (rule 2). Number them. Show the list and ask which to keep, edit or drop, and what is
   missing. Write only the confirmed list under "Questions they will ask". Done when the owner confirms
   the list.
4. **Topics.** For each question, name what the owner must understand to answer it. Match each to an
   existing note, queue item or study pack by slug before inventing one; give a new topic a type from
   `note-types.md`. Order the topics so each comes after every topic it builds on (rule 3). For each,
   draft one explain-it test in the audience's voice, list the question numbers it answers, and set its
   starting status (rule 4). Every question must be answered by at least one topic. More than eight
   topics means the deliverable is two briefs: say so and ask which half to keep. Show the table and
   apply the owner's edits. Done when the owner confirms the table and every question is covered.
5. **Commit.** Close the resume item, commit and push the vault as "Commit and push" in `rules.md` says,
   with message `brief: <slug>`, and end with one line: the brief's path, its first topic and the next
   command, `/cairn:brief <slug>`. Done when the commit lands and the line is said.

Check `date +%H:%M` after each step. At the limit, finish the step, commit with the resume item open,
and name `/cairn:brief new <slug>` to pick up.

## Next step

Read `briefs/<slug>.md`. Show the topic table with each status and the days left until `due:`. The next
topic is the first one, in table order, not yet `passed`. Its status names the next command:

| Status | Next command |
|---|---|
| `todo` | `/cairn:study <topic>` |
| `studied` | `/cairn:interview <topic>` |
| `noted` | `/cairn:brief rehearse <slug> <topic>` |

Ask one question: start it now? On yes, run that skill now. On no, end with the command. When every topic
is `passed`, close the brief (rule 7), commit with message `brief: close <slug>`, and say so.

## Rehearse

1. **Open.** Run `date +%H:%M` and state the 10-minute limit. Read the brief, the topic's note and its
   study pack. Create the transcript `interviews/YYYY-MM-DD-HHMM-rehearse-<topic-slug>.md`, with the
   brief's company tag, holding the brief, the topic, its test and the start time. A topic below `noted`
   can still be rehearsed as practice: say so now; it is graded and recorded, and its status does not
   change. Done when the transcript exists.
2. **Ask.** Tell the owner the rehearsal is closed book: no notes, no study pack, no agent. Then speak as
   the audience and ask the explain-it test, in their words. Append the owner's answer to the transcript
   word for word. Ask one follow-up as the audience would: at the weakest part of the answer, or the
   brief's nearest question the answer did not reach. Append it and the answer. During this step Claude
   only asks, as "Interview conduct" in `rules.md` says. Done when both answers are in the transcript.
3. **Grade.** Check both answers against the study pack's sources, and the code or documents behind
   them where the sources point there. Tell the owner plainly what was right, what was wrong and what
   was missing, citing the source for each. Append the grade. Done when every claim in the answers is
   marked right or wrong and every missing point is cited.
4. **Room check.** Ask one question: would you say it that way in the room? Apply rule 5. On a pass, set
   the topic's status to `passed`. On a fail, write a queue item for each gap, from `templates/queue.md`
   with the brief's company tag, named for the gap, linked under "Going deeper" in the topic's note.
   Either way add a line under the brief's "Rehearsals" heading: the date, the topic, pass or fail, and a
   link to the transcript. Done when the brief has the line.
5. **Commit.** Commit and push the vault with message `brief: rehearse <topic-slug>`, and end with one
   line: pass or fail, what was written, and the next command, `/cairn:brief <slug>`. Done when the commit
   lands and the line is said.
