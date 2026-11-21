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
- `/cairn:brief <slug>`: when `queue/resume-brief-<slug>.md` is open, finish writing the brief from the
  step it names; otherwise show where the brief stands and start its next step.
- `/cairn:brief rehearse <slug> [topic]`: one rehearsal, on the named topic or the brief's first topic
  not yet `passed`. 10 minutes.
- No argument: list open briefs with their due date and next topic, and ask which one.

Every mode reads the config as "Config" in `rules.md` says, and runs `date +%H:%M` at its start and
after each step. At its limit it finishes the current step, commits with its resume item open, and ends
with the line Session state rule 3 asks for, naming the command that picks up.

## New

1. **Open.** State the 20-minute limit and its end time. Pick a kebab-case slug unique in the vault, and
   decide whether the brief is about the employer's work (rule 6 in "Briefs"). Create `briefs/<slug>.md`
   from `templates/brief.md`, creating `briefs/` with a `.gitkeep` if it does not exist, with the company
   tag when it applies, and the queue item `queue/resume-brief-<slug>.md` from `templates/queue.md`,
   titled "resume brief: <slug>", listing steps 2 to 5. Take each step off it as it finishes. Done when
   both files exist.
2. **Deliverable.** Ask one question: who is the audience, by when, and what do they need to see or
   decide? Write `audience:` and `due:`, and draft the "Be able to" sentence (rule 1). Show it; apply the
   owner's edit. Done when the owner says yes to the sentence.
3. **Questions.** Sweep everything available on the deliverable before drafting a question: every
   meeting with the audience or on the topic (transcripts, as rule 2 says), the channels and threads
   where it is discussed, the tracker's project and tickets, design docs, and the code and notebooks the
   owner built. Use every source the machine file configures and every connector the session has, and
   ask the owner which channel, project and docs hold it. Append one line per source read, or skipped and
   why, to the resume item. Then list at most ten questions, numbered, each with its pointer. Show the
   sources read and the list, and ask which questions to keep, edit or drop, and what is missing. Write
   only the confirmed list. Done when every named source has its line and the owner confirms the list.
4. **Topics.** For each question, name what the owner must understand to answer it. Match each to an
   existing note, queue item or study pack by slug before inventing one; give a new topic a type from
   `note-types.md`. Order the topics as rule 3 says, draft each one's explain-it test, fill "Answers"
   with the question numbers, and set each starting status (rule 4). Every question must appear in some
   topic's "Answers". More than eight topics means the deliverable is two briefs: say so and ask which
   half to keep. Show the table and apply the owner's edits. Done when the owner confirms the table and
   every question is answered.
5. **Commit.** Close the resume item, commit and push the vault as "Commit and push" in `rules.md` says,
   with message `brief: <slug>`, and end with one line: the brief's path, its first topic and the next
   command, `/cairn:brief <slug>`. Done when the commit lands and the line is said.

## Next step

Read `briefs/<slug>.md`. Show the topic table with each status and the days left until `due:`. The next
topic is the first one, in table order, not yet `passed`; rule 4 names its next command. Ask one
question: start it now? On yes, run that skill now. On no, end with one line: the brief's path, its next
topic and that command.

When every topic is `passed`, close the brief (rule 7), commit with message `brief: <slug>`, and end
with one line: the brief's path, that it is done, and `/cairn:sweep` as the next command.

## Rehearse

1. **Open.** State the 10-minute limit and its end time. Read the brief, the topic's note, its study
   pack when one exists, and the note's sources. Create the transcript
   `interviews/YYYY-MM-DD-HHMM-rehearse-<topic-slug>.md`, with the brief's company tag, holding the brief,
   the topic, its test and the start time, and the queue item `queue/resume-brief-rehearse-<topic-slug>.md`
   from `templates/queue.md`, titled "resume rehearsal: <topic-slug>", listing steps 2 to 5 and linking
   the transcript; take each step off it as it finishes. A topic below `noted` can still be rehearsed
   as practice: say so now; it is graded and recorded, and its status does not change. Done when both
   files exist.
2. **Ask.** Tell the owner the rehearsal is closed book: no notes, no study pack, no agent. Then speak as
   the audience and ask the explain-it test, in their words. Append the owner's answer to the transcript
   word for word. Ask one follow-up as the audience would: at the weakest part of the answer, or the
   brief's nearest question the answer did not reach. Append it and the answer. During this step Claude
   only asks, as "Interview conduct" in `rules.md` says. Done when both answers are in the transcript.
3. **Grade.** Grade both answers against the sources rule 5 names. Tell the owner plainly what was
   right, what was wrong and what was missing, citing the source for each. Append the grade. Done when
   every claim in the answers is marked right or wrong and every missing point is cited.
4. **Room check.** Ask one question: would you say it that way in the room? Apply rule 5, with the gap
   queue items following rule 6, and on a pass set the topic to `passed`. Add a line under the brief's
   "Rehearsals" heading: the date, the topic, pass or fail, and a link to the transcript. Done when the
   brief has the line.
5. **Commit.** Close the resume item, commit and push the vault with message `brief: <slug>`, and end
   with one line: pass or fail, what was written, and the next command, `/cairn:brief <slug>`. Done when
   the commit lands and the line is said.
