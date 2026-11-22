# Cairn

Cairn helps a person capture what they understand, in their own words, and prove they can explain it. This glossary fixes the words the cairn skills use for that work and the record it leaves. Every skill reads it before anything else.

## The owner and the vault

**Owner**:
The person whose notes these are, and the only person cairn works for.
_Avoid_: User, learner, student

**Vault**:
The owner's one private collection of notes and everything cairn writes beside them.
_Avoid_: Brain, knowledge graph, repo

**Employer**:
The company the owner works for now. An owner may have none.
_Avoid_: Company, org, client

**Knowledge base**:
Notes the owner had before cairn, which cairn reads and never writes to.
_Avoid_: Imported vault, old notes, source

## Notes

**Topic**:
One thing the owner can learn and explain, and the subject of at most one note.
_Avoid_: Stone, node, item

**Note**:
One topic written in the owner's words.
_Avoid_: Page, document, article, study pack

**Note type**:
The kind of topic a note covers (concept, system, project, synthesis or person), which fixes the questions it is built from.
_Avoid_: Category, template

**Synthesis**:
A note on how several topics combine in one setting, such as deletion requests on a lakehouse.
_Avoid_: Summary, overview

**Prerequisite**:
A topic that another topic directly builds on. The link has a direction: Parquet is a prerequisite of Iceberg snapshots, never the reverse.
_Avoid_: Dependency, parent, upstream

**Company topic**:
A topic whose note would stop being true once the owner leaves the employer: its systems, projects and people, and a synthesis set there. Any other topic is general.
_Avoid_: Work topic, internal topic

**Company tag**:
The mark on every file about a company topic, so the owner can keep or delete one employer's files as a set.
_Avoid_: Label, privacy rule

**Draft**:
The status of a note that is missing a required answer, has a review error parked, or was not confirmed at its last read-back.
_Avoid_: Loose, unverified, incomplete

**Defensible**:
The status of a note the owner can explain and defend: every required answer is in, nothing is parked, and the owner confirmed it at its last read-back.
_Avoid_: Solid, verified, mastered

**Queue item**:
One open question, linked from every note it touches, and closed by an interview.
_Avoid_: Gap, proposal, task, todo

**Going deeper**:
The part of a note that lists the open queue items touching it, and nothing else.
_Avoid_: Next steps, open questions

## Sessions

**Session**:
One sitting in which the owner works on a topic with Claude.
_Avoid_: Chat, conversation

**Study**:
The session in which Claude teaches one topic hands-on, starting from a probe and ending in a reading list and a hand-off to an interview.
_Avoid_: Lesson, tutorial, course

**Probe**:
The question that opens a study, asking what the owner thinks the topic is and what it is for, so the teaching can aim at their misconceptions.
_Avoid_: Assessment, quiz, placement

**Study pack**:
The reading material a study leaves behind: its sources, its walkthrough and what the owner's commands produced. Claude writes it, so it is never a note.
_Avoid_: Note, lesson notes, summary

**Lab**:
The hands-on material for one study, in whatever shape its tools need, which the owner works through by hand. It may use real or mock data, and a later lab may build on an earlier one.
_Avoid_: Exercise, sandbox, environment, demo

**Interview**:
The session in which Claude asks a note type's questions and writes only the owner's answers into the note.
_Avoid_: Lesson, study, quiz

**Transcript**:
The word-for-word record of an interview or a rehearsal.
_Avoid_: Log, summary, minutes

**Read-back**:
Showing the owner something they wrote and asking whether it still sounds like them.
_Avoid_: Review, retest, refresh

## Briefs

**Brief**:
One deliverable the owner must be able to explain to a named audience by a date, with the questions that audience will ask and the topics needed to answer them.
_Avoid_: Plan, project, goal

**Explain-it test**:
One question per brief topic that the owner answers aloud, without notes, as they would in front of the brief's audience.
_Avoid_: Exam, quiz, test

**Rehearsal**:
The session in which Claude plays the brief's audience and asks one topic's explain-it test, closed book.
_Avoid_: Mock, practice run, dry run

## Bringing work in

**Sweep**:
The scheduled review that reads the owner's recent work, finds topics with no note, and hands the top few to a study or an interview.
_Avoid_: Scan, digest

**Source**:
A place the sweep may read, such as Claude Code sessions, meeting notes or a chat workspace.
_Avoid_: Connector, feed, integration, reference

**Cadence**:
How often the sweep runs: daily, weekly or monthly.
_Avoid_: Schedule, frequency

**Slot**:
The time the owner set aside for one cadence's sweep, and how long it lasts.
_Avoid_: Block, appointment
