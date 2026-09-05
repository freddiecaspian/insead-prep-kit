Process one or more PDFs from the inbox into structured study notes.

$ARGUMENTS

Input: optional - a filename or course code to process just that; with no input, process everything currently in the inbox folder.

## Step 0: Read the settings

Read `config.yaml` for the folders (`download_to` = the inbox, `notes_to`, `done_to`) and the course list (to recognise course codes in filenames).

## Step 1: Read the PDF properly

For each target PDF in the inbox:

1. Read it page by page, including charts, tables and exhibits. If pages are image-heavy, render them as images and read them visually - do not skip visual content.
2. If a page is genuinely unreadable, note that in the output rather than guessing what it said.

## Step 2: Write the study note

One Markdown file per PDF in the notes folder, named to match the PDF (`.md` instead of `.pdf`). Structure, per CLAUDE.md:

1. A two-sentence plain-English summary at the top.
2. Key arguments and frameworks, as short sections with headings.
3. Anything numerical worth remembering, kept exact.
4. "Questions worth bringing to class" - three to five genuinely good ones.

Style: plain English first, technical terms explained in parentheses. Hyphens only, never em or en dashes. British spelling. Only use what is actually in the PDF.

## Step 3: File and report

1. Move each processed PDF from the inbox to the done folder.
2. Report a short list: note created, source PDF, anything unreadable or missing.
