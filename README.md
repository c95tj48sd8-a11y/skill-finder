# skill-finder

**A Claude skill that finds the right skill for your task, security-checks it, and uses it to get the job done.**

When Claude gets a task that a specialized skill would handle better (Excel files, PDFs, slide decks, data viz, …), `skill-finder` does three things:

1. **Searches** your installed skills first, then official catalogs, then trusted GitHub sources.
2. **Vets** the best candidate by reading `SKILL.md` and every bundled script before using anything.
3. **Uses** the skill to finish your task, and asks before saving anything permanently.

## How it works

```
Task ──► Installed skills? ──yes──► Use it
              │ no
              ▼
         Official catalog? ──yes──► Offer install card
              │ no
              ▼
         Trusted GitHub repos ──► Security review ──pass──► Use it (this session)
                                        │ fail                    │
                                        ▼                         ▼
                                 Solve without it        Ask: save permanently?
```

## Example

> **You:** Turn this CSV into a formatted Excel report with charts.
>
> **Claude:** *(skill-finder: no Excel skill installed → finds the `xlsx` skill in `anthropics/skills` → reads it, no red flags → builds the report)*
> Here's your report. I used the `xlsx` skill from anthropics/skills. Want me to save it permanently?

## Security

External skills are untrusted. Before any skill from the web is used, `skill-finder`:

- reads `SKILL.md` and every bundled script in full, so nothing unread is ever executed
- **rejects** skills that try to override system rules, exfiltrate data or credentials, download code from unknown URLs (`curl | bash`, obfuscated or base64 payloads), or touch files outside the working directory
- prefers official and well-maintained sources such as [anthropics/skills](https://github.com/anthropics/skills)
- never installs anything permanently without your explicit consent

## Installation

### Claude.ai / Claude Desktop
1. Download `skill-finder.zip` from the [Releases](../../releases) page.
2. In Claude, go to **Customize → Skills** and upload the ZIP.
3. Turn the skill on.

### Claude Code
```bash
git clone https://github.com/c95tj48sd8-a11y/skill-finder.git
cp -r skill-finder/skill-finder ~/.claude/skills/
```

Or, with the skills CLI:
```bash
npx skills add c95tj48sd8-a11y/skill-finder
```

## Usage

Give Claude a task as usual. `skill-finder` kicks in on its own when a specialized skill would help. You can also call it directly:

> *"Use skill-finder to create a pitch deck from these notes."*

## Repository layout

```
.
├── skill-finder/
│   └── SKILL.md      # the skill
├── README.md
└── LICENSE
```

## Contributing

Issues and PRs are welcome, especially suggestions for trusted skill sources and new security checks.

## License

MIT. See [LICENSE](LICENSE).
