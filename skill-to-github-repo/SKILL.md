---
name: "skill-to-github-repo"
description: "Use when the user creates or saves a new Claude skill, or asks to back up their skills: log it in their skills spreadsheet, then publish it as its own private GitHub repo."
---

# Skill to GitHub Repo

Every skill the user makes gets (1) a row in their tracker sheet and (2) its own GitHub repo. Run this whenever the user creates, saves, or updates one of their own skills, or asks to sync them.

## Fixed settings

- Tracker: Google Sheet "Claude Skills to GitHub Repos", ID `1DDSgyYCWylbG9PDxTFFs8a0ADYfFizmBaTdf3zfxWt0`, tab `Skills`.
- Columns: A Skill | B What it does | C Proposed repo name | D Visibility | E Status | F License | G Priority | H Repo URL | I Notes.
- Defaults unless the user says otherwise: Visibility `Private`, License `MIT`, repo name = skill name.
- Status values: Not started, Reviewing, Preparing repo, Repo created, Published, Skipped.
- Only the user's own skills qualify (ListSkills ids starting `skill_` with no plugin prefix). Never publish Anthropic built-in or plugin skills.

## Steps

1. **Find the skill.** Use ListSkills to confirm it is the user's own. Locate its folder (search the synced skills directory for a folder named after it). Read every file before publishing.
2. **Log it.** Read `Skills!A:I` with get_values. If the skill has no row, append one: name, a one-line description from its frontmatter, repo name, `Private`, `Preparing repo`, `MIT`. If it has a row, honor any Visibility, License or repo name the user filled in. Status `Skipped` means leave it alone.
3. **Vet.** Scan the files for secrets (API keys, tokens, passwords) and personal identifiers; stop and ask if any turn up. Note any upstream project it builds on so the README can credit it.
4. **Package.** Repo layout:
   - `<skill-name>/` containing the skill files exactly as saved (SKILL.md plus any references, scripts, assets).
   - `README.md`: title, one-paragraph description, file list, install steps (Claude app: zip the `<skill-name>` folder and upload it in Claude's Skills settings; Claude Code: clone, then `cp -r <name>/<name> ~/.claude/skills/`), 2-3 example prompts, Credits (only if it builds on another project), License line.
   - `LICENSE`: MIT with the current year and the user's GitHub display name (fall back to their login).
5. **Publish.** Check GitHub access with `gh api user`. If it is not linked, give the user the connect link from the error and stop after packaging. Otherwise:
   - New repo: `gh repo create <owner>/<repo> --private --description "<one line>"`, then `git init`, commit, push to `main`.
   - Existing repo: clone it, replace the `<skill-name>/` folder and README, commit with a message naming what changed, push.
   - Never delete or force-push over a repo without the user asking.
6. **Update the sheet.** Set Status `Repo created` (or `Published` if public) and Repo URL to `https://github.com/<owner>/<repo>`. Leave Priority and Notes to the user.
7. **Verify.** `gh repo view <owner>/<repo> --json visibility,url` and list its files; re-read the sheet row. Report the repo link and what changed in one or two lines.