# INSEAD Prep Kit - standing instructions

This folder is an AI study system for an INSEAD student. Read `config.yaml` at the start of any task - it is the single source of truth for who the student is, which courses they take, their Canvas IDs, and where files go.

## The student

Details live in `config.yaml` (`student`, `campus`, `languages`). The student is not a software engineer: explain everything in plain English first, technical terms after, in parentheses. Never show raw code as an answer to a question - describe what happened instead. Both English and French are welcome in conversation and notes.

## Folder layout

- `PDFs Inbox/` - downloaded and dropped-in PDFs waiting to be processed
- `PDFs Done/` - move each PDF here after its notes are written
- `Notes/` - all study notes, as Markdown files
- `.claude/commands/` - the slash commands (each is a plain-text recipe)

## File naming for downloads

- Lecture slides: `{CODE} S{XX} - Lecture Slides.pdf`
- Readings: `{CODE} S{XX} - Reading - {Title}.pdf`
- Cases: `{CODE} S{XX} - Case - {Title}.pdf`
- Problem sets: `{CODE} S{XX} - Problem Set {N}.pdf`

Never leave a file called `download.pdf` or similar in the inbox - rename on arrival.

## Study notes style

One Markdown file per source PDF in `Notes/`, named to match the PDF (`.md` instead of `.pdf`). Structure:

1. A two-sentence plain-English summary at the top
2. Key arguments and frameworks, as short sections with headings
3. Anything numerical worth remembering, kept exact
4. "Questions worth bringing to class" - three to five genuinely good ones

Rules that are not negotiable:

- Only use what is actually in the PDF. Never invent, pad, or guess content. If a page is unreadable, say so in the note.
- Report gaps honestly ("slides not posted yet") rather than working around them silently.
- Hyphens only, never em or en dashes. British English spelling.

## Safety

- Never send, submit, post, buy, or sign anything without the student's explicit yes for that specific action.
- Canvas login is always done by the student, in the visible browser window. Never ask for, handle, or store their password.
- Downloads are personal study materials. Do not share them outside this folder or suggest doing so.
