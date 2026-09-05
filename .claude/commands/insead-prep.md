Weekly INSEAD class prep. Work out which sessions are coming up, download all Canvas materials for them, turn every PDF into study notes, and report what is not posted yet.

$ARGUMENTS

Input: optional - course codes ("ETA"), a window ("this week", "Thursday"), or session numbers ("ETA S03"). With no input, cover every course in config.yaml for the coming week.

## Step 0: Read the settings

Read `config.yaml` in the kit root. It gives you the student, the courses with their Canvas IDs, and the three folders (`download_to`, `notes_to`, `done_to`). If a requested course has `canvas_id: 0000` (unset), stop and help the student fill it in first: have them open the course at learn.insead.edu and read the number out of the address bar.

## Step 1: Identify the target sessions

There is no calendar integration in this kit, so establish the sessions from Canvas itself plus the student:

1. Open Canvas (Step 2a) and look at each target course's home page and Modules section for upcoming session numbers and dates.
2. Present what you found as a short table (course, session, date if visible) and confirm with the student before downloading. If Canvas does not make the schedule obvious, simply ask: "Which sessions shall I prep?"

## Step 2: Download from Canvas

Use Playwright to open Canvas (https://learn.insead.edu) and download materials.

### 2a. Navigate and authenticate

- Navigate the browser to https://learn.insead.edu
- If the dashboard shows, you are logged in - continue.
- If a login page shows, tell the student to log in themselves in the visible window (never ask for their password), then wait and continue once the dashboard appears.

### 2b. For each course, use the Canvas API via browser evaluate

The browser's fetch carries the session cookies. Use this pattern for each course:

```javascript
// Get folder structure
fetch(`/api/v1/courses/{CANVAS_ID}/folders?per_page=50`).then(r => r.json())

// Get files from relevant folders (Handouts, Learning Materials, Session Slides)
fetch(`/api/v1/folders/{FOLDER_ID}/files?per_page=50`).then(r => r.json())
fetch(`/api/v1/folders/{FOLDER_ID}/folders?per_page=50`).then(r => r.json())
```

For each course, identify:

- **Handouts / Session Slides folder** - lecture slides (often named S{XX}.pdf or similar)
- **Learning Materials folder** - readings (often named S{XX}.{N}_{CODE}_{Title}.pdf)
- **Session-specific subfolders** if they exist

Filter to only the target session numbers. Also read module overview pages for reading lists, so you know what SHOULD exist (that is how gaps get reported).

### 2c. Download using Playwright download events

Use this pattern - it is the ONLY reliable method (curl with signed URLs does NOT work on Canvas):

```javascript
async (page) => {
  const downloadPromise = page.waitForEvent('download');
  await page.evaluate((fileId) => {
    const a = document.createElement('a');
    a.href = `https://learn.insead.edu/files/${fileId}/download?download_frd=1`;
    a.download = '';
    document.body.appendChild(a);
    a.click();
    a.remove();
  }, fileId);
  const download = await downloadPromise;
  await download.saveAs(destPath);
}
```

### 2d. Naming convention

Save into the `download_to` folder with the names defined in CLAUDE.md:

- Lecture slides: `{CODE} S{XX} - Lecture Slides.pdf`
- Readings: `{CODE} S{XX} - Reading - {Title}.pdf`
- Cases: `{CODE} S{XX} - Case - {Title}.pdf`
- Problem sets: `{CODE} S{XX} - Problem Set {N}.pdf`

## Step 3: Process every PDF into notes

For each downloaded PDF, follow the `/insead` command's recipe (`.claude/commands/insead.md`): read the PDF page by page, write one structured study note into the `notes_to` folder, then move the PDF to `done_to`. Process courses in parallel with sub-agents when there are several.

## Step 4: Report

Finish with a summary the student can trust:

```
## Downloaded and processed

| Course | Session | Slides | Readings | Cases | Notes created |
|--------|---------|--------|----------|-------|---------------|
| ETA    | S03     | yes    | 2        | 1     | 3             |

## Not posted yet (check closer to class)
- ETA S03 lecture slides

## Notes created
- ETA S03 - Reading - Search Funds.md
- ...
```

Report gaps honestly. Never pad the table, never invent a file that was not there.
