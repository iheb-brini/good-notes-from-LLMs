# good-notes-from-LLMs

Useful lessons learned from conversations with LLMs (Claude, ChatGPT, Gemini, …), rewritten as short, practical notes.

## Why this repo exists

LLM conversations often produce a good answer: the right tool, a safe sequence of commands, a gotcha you'd never have found alone. Then it disappears into chat history. This repo keeps those answers. Each one is checked, cleaned up and written so that someone else can use it without the original conversation.

A good note is:

- **Practical**: it solves a real problem you had, with commands or steps you can copy.
- **Verified**: you actually ran it, or checked it against the official docs. LLMs can be confidently wrong, so a note is only as good as the testing behind it.
- **Self-contained**: it explains the context (what problem, what environment) and doesn't depend on the chat it came from.
- **Short**: one topic per note, and just enough "why" to understand the choice.

## Notes

### rclone
- [Copy a large local folder to Google Drive](rclone/copy-large-folder-to-gdrive.md): set up a Drive remote (including on a headless server), then copy, check and delete safely.

## Repository layout

```
.
├── README.md              # this file: purpose, index, how to contribute
├── CLAUDE.md              # instructions for Claude Code when it works in this repo
├── .claude/settings.json  # Claude Code permissions for this repo
└── <topic>/               # one folder per tool or topic (rclone/, git/, docker/, …)
    └── <short-kebab-case-title>.md
```

## Contributing

Contributions are welcome. You can add a new lesson, fix a note, or add a missing tip.

### Adding a note

1. **Pick or create a topic folder**, named after the tool or area, in lowercase (e.g. `rclone/`, `git/`, `python/`).
2. **Create the note** as `<topic>/<short-kebab-case-title>.md`. Start from the template below.
3. **Test what you write.** Run the commands yourself, or check them against the official docs, and remove anything you couldn't confirm.
4. **Remove anything private**: tokens, emails, hostnames, internal paths and customer data. Use placeholders like `<your-remote>` or `/path/to/data`.
5. **Add the note to the index** in the [Notes](#notes) section above, under its topic heading, with a one-line description.
6. **Open a pull request** with a short description of the lesson.

### Note template

````markdown
# <What you did, as a title>

**Context:** the problem, the environment (OS, versions, sizes), and what you were trying to achieve.

## Why <this approach>

- Why this is the right tool or approach
- Alternatives you ruled out, and why

## Steps

```bash
# commands, in order, with a short comment on anything that isn't obvious
```

## Tips / gotchas

- Limits, pitfalls, version quirks, how to undo or verify
````

### Style

- Write in plain English and keep sentences short.
- Put commands first and explanation after.
- Prefer safe defaults: verify before you delete, suggest `--dry-run` where it exists, and flag anything destructive.
- Mention the version when it matters (e.g. "rclone ≥ 1.60").
- You don't need to credit the model or paste the chat. Only the lesson matters.

### Commit messages

Use one line of 5–10 words that describes the change, e.g. `Add rclone note for Google Drive uploads`.

## License

No license has been chosen yet. Until one is added, please ask before reusing the content outside this repo.
