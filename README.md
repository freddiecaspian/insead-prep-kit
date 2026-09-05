# INSEAD Prep Kit

A complete, working AI set-up for an INSEAD student, starting from a totally blank slate. By the end of this document you will type one command and watch your Mac log into Canvas as you, download the week's slides, readings and cases for your courses, file them with proper names, and turn every PDF into structured study notes - while you watch it happen in a real Chrome window.

This is an open-source copy of the system Freddie Chambers runs every week for his own classes. Nothing in it requires you to write or read code. It was built for Nathan Best to prep his ETA elective (Entrepreneurship through Acquisition), but it works for any INSEAD course - just add a course to the settings file.

**This README is the master document. Everything you need is in here, in order. Do the parts top to bottom.**

## What is in the kit

```
insead-prep-kit/
  README.md                       <- you are here, the master document
  CLAUDE.md                       <- standing instructions Claude reads every session
  config.yaml                     <- YOUR settings: courses, Canvas IDs, folders
  .claude/commands/
    insead-prep.md                <- the main slash command: /insead-prep
    insead.md                     <- process any single PDF: /insead
  PDFs Inbox/                     <- downloads land here
  PDFs Done/                      <- processed PDFs are moved here
  Notes/                          <- your study notes are written here
```

Two ideas explain the whole kit:

1. **A slash command is a recipe.** Each file in `.claude/commands/` is plain text: a description of a job, written in English. When you type `/insead-prep` in Claude, it reads that file and follows it. You can open the files and read exactly what they do.
2. **One settings file makes it yours.** `config.yaml` holds your name, your courses and their Canvas IDs. The commands read it every time. Change the file, and the whole kit follows.

## Part 1 - the blank slate hour (do this solo)

This assumes nothing is installed and no accounts exist. It takes under an hour, and you can spread it over a few days.

### 1a. Two accounts (ten minutes)

1. **Claude Pro** - go to [claude.ai](https://claude.ai), create an account, and take the Pro plan (US$20 a month). While you are there, install the desktop app and the iPhone app. New habit from day one: questions go to Claude before they go to Google.
2. **GitHub** (free) - go to [github.com](https://github.com) and create an account. GitHub is where people share folders of code and text publicly. This kit lives there, and the link Freddie sends you points to it.

### 1b. Speak, don't type - Wispr Flow

Wispr Flow is a dictation app that sits behind everything on your Mac and iPhone. Hold one key, talk normally, and it types clean, punctuated text into whatever your cursor is in - email, WhatsApp, Claude, anything. It handles English and French in the same sentence and strips out the ums. It comes first in this plan because everything else gets faster once you stop typing.

1. Download it from [wisprflow.ai](https://wisprflow.ai). Allow microphone access when asked.
2. Pick your hold-to-talk key (the `fn` key works well) and do the two-minute tutorial.
3. The week-one rule: anything longer than one line gets dictated. Everywhere, in both languages. It feels odd for two days, then becomes permanent.

It is free to try, about US$15 a month after.

### 1c. Claude Code - Claude in a terminal

Claude Code is Claude running in a terminal window: you type (or dictate) instructions in plain English and it does the work directly on your Mac - reading files, writing notes, running tools. You never write or read code.

1. Open Terminal: press `cmd + space`, type `terminal`, press enter. The plain text window that opens is all a terminal is - a place where you type instructions.
2. Paste this line and press enter (it installs Claude Code):

   ```
   curl -fsSL https://claude.ai/install.sh | bash
   ```

3. Close the Terminal window, open a new one, and type:

   ```
   claude
   ```

4. It will ask you to sign in with your Claude account. Do that once. You are now talking to Claude in the terminal - say hello.

### 1d. Playwright - give Claude hands

Playwright is the piece that sounds like science fiction until you watch it. It lets Claude open a **real Chrome window** on your Mac and use it exactly the way you would: click, scroll, read, type, download. Because it is a real browser, it works on sites you are logged into - Canvas included - and every step happens visibly on your screen.

1. In Terminal (not inside Claude - open a fresh window if unsure), paste:

   ```
   claude mcp add playwright -- npx @playwright/mcp@latest
   ```

2. Test drive: start `claude` again and ask, in your own words, something like *"open the INSEAD website in a browser and tell me what is on the homepage"*. Watch the Chrome window appear and do it.

That is the whole foundation. Everything from here is the kit itself.

## Part 2 - install the kit (with Freddie, coffee one)

1. In Terminal, download your copy of the kit:

   ```
   git clone [the GitHub link Freddie sends you]
   ```

   (`git clone` means "download this shared folder and keep it connected to the original".)

2. Step inside the folder and start Claude there:

   ```
   cd insead-prep-kit
   claude
   ```

   Starting Claude *inside* the kit folder matters: that is how it finds `CLAUDE.md`, the slash commands and your settings.

## Part 3 - your settings file, config.yaml

Everything personal lives in one small file, `config.yaml`, written in YAML - a format designed to be readable by humans first and machines second. Open it in any text editor (or ask Claude to edit it for you). It looks like this:

```yaml
student: Nathan Best
campus: Fontainebleau
languages: [english, french]

courses:
  - code: ETA
    name: Entrepreneurship through Acquisition
    canvas_id: 0000        # see below for how to find this

  # Add every other course as three more lines, same shape:
  # - code: XYZ
  #   name: Course Name
  #   canvas_id: 0000

download_to: PDFs Inbox/
notes_to: Notes/
done_to: PDFs Done/
```

**Finding a Canvas ID (the only number you need):** open the course at [learn.insead.edu](https://learn.insead.edu). The address bar shows something like `learn.insead.edu/courses/9876` - the number at the end is the Canvas ID. Copy it into the file. Five minutes covers your whole timetable, once per period.

If you are unsure, do this part with Freddie at coffee one - or simply ask Claude: *"help me fill in config.yaml, my courses are..."*.

## Part 4 - the first run

In the kit folder, with Claude running, type:

```
/insead-prep ETA
```

Here is what happens on screen, in order:

1. Claude reads `config.yaml` and works out which ETA sessions are coming up (it will ask you if the week is ambiguous).
2. A Chrome window opens at learn.insead.edu. **The first time, you log in yourself** with your INSEAD single sign-on. Claude never sees or stores your password - you type it into the real INSEAD page, and the browser remembers the session from then on.
3. Claude walks the course's folders the way you would - handouts, learning materials, session slides - using Canvas's own filing system, and picks out just the sessions you need.
4. Each file downloads into `PDFs Inbox/` with a proper name: `ETA S03 - Reading - Search Funds.pdf`, not `download(4).pdf`.
5. Claude then reads every PDF - actually reads it, page by page - and writes structured study notes into `Notes/`: key arguments, frameworks, and the questions worth bringing to class.
6. It reports the gaps honestly: *"S03 slides not posted yet - check again Wednesday."* Nothing is invented.

Run it again any week, for any course in your settings file, or several at once: `/insead-prep ETA this week`.

## Part 5 - beyond the first run

- **`/insead`** - drop any PDF into `PDFs Inbox/` (a case someone AirDropped you, a handout, an article) and type `/insead`. Same structured notes, on demand.
- **No command at all** - slash commands are shortcuts, not the ceiling. *"Compare these three readings and tell me what the professor is setting up"* works in plain English, in either language.
- **Copy a command, own it** - open any file in `.claude/commands/`, change the words, save it under a new name. You have just written software. This is how the kit grows beyond INSEAD: board papers summarised before a meeting, research sweeps that come back as shortlist memos, company briefs when recruiting starts. Same engine, different target.

## Guardrails

- Watch it work the first few times. Stay in the room.
- Nothing gets sent, submitted or bought without your explicit yes - the kit's standing instructions (`CLAUDE.md`) say so, and you should keep it that way.
- On logged-in sites Claude acts **as you**. Treat it like handing your laptop to a very fast, very literal intern: clear instructions, and your eyes on the screen.
- Downloads are course materials for your own study. Keep them in these folders, not shared onwards - normal INSEAD copyright rules apply.

## Troubleshooting

| Symptom | What it means | Fix |
|---|---|---|
| `claude: command not found` | Terminal has not picked up the install yet | Close the Terminal window, open a new one, try again |
| Chrome opens on the INSEAD login page again | The Canvas session expired (it does every so often) | Log in in the window, then tell Claude "logged in, carry on" |
| "Slides not posted yet" | Professors upload late; this is Canvas, not a bug | Run `/insead-prep` again the day before class |
| A download fails or a file is empty | Canvas download links are fussy | Tell Claude "that download failed, try it again" - the command knows the reliable method |
| Claude seems to be inventing content | It should never - the instructions forbid it | Say "only use what is actually in the PDF" and mention it to Freddie |

## Costs

| Thing | Cost |
|---|---|
| Claude Pro | US$20 a month |
| Wispr Flow | free to try, about US$15 a month after |
| GitHub, Claude Code, Playwright, this kit | free |

About US$35 a month, all running on your own Mac.

## Credits

Built by Freddie Chambers (INSEAD 26D) from the system he runs weekly, packaged for Nathan Best. Questions, bugs, or a coffee to set it up: ask Freddie.
