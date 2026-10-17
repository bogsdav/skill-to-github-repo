# Skill to GitHub Repo

A Claude skill. Use when the user creates or saves a new Claude skill, or asks to back up their skills: log it in their skills spreadsheet, then publish it as its own private GitHub repo.

## What's inside

- `skill-to-github-repo/SKILL.md`

## Install

### Claude app (claude.ai, desktop, mobile)

1. Download this repo as a ZIP (**Code > Download ZIP**) and unzip it.
2. Zip the `skill-to-github-repo` folder on its own, so the ZIP contains `skill-to-github-repo/SKILL.md`.
3. In Claude's settings, open the Skills section, upload that ZIP, and turn the skill on.

### Claude Code

```bash
git clone https://github.com/bogsdav/skill-to-github-repo.git
mkdir -p ~/.claude/skills
cp -r skill-to-github-repo/skill-to-github-repo ~/.claude/skills/
```

Restart Claude Code. The skill loads automatically when a request matches its description.

## Try it

- "I just saved a new skill. Log it and back it up to GitHub."
- "Sync all my skills to their GitHub repos."

## License

MIT. See [LICENSE](LICENSE).
