# Desktop handoff — publish this repository

## Final repository identity

- Owner: `TarunKurella`
- Repository: `agent-runtime-notes`
- Full name: `TarunKurella/agent-runtime-notes`
- Visibility: public
- Default branch: `main`
- Description: `A searchable engineering fieldbook tracing agent-runtime decisions from problem to code, failure modes, and reusable implementation lessons.`

The name is intentionally plain and professional. It describes the material honestly without making claims about autonomy, security bypasses, or production readiness.

## Files that must stay at the repository root

```text
agent-runtime-notes/
├── README.md
├── AGENTS.md
├── DESKTOP-HANDOFF.md
├── BUILD-REPORT.md
├── LICENSE
├── 01-book/
├── 02-notes/
└── 03-agent-context/
```

Do not place the extracted `agent-runtime-notes/` folder inside another repository subdirectory. That folder is the repository root.

## Publish from the extracted folder

Run these commands in PowerShell, Git Bash, or a terminal where GitHub CLI is installed:

```bash
gh auth status
git init -b main
git add -A
git commit -m "Publish agent runtime notes"
gh repo create TarunKurella/agent-runtime-notes --public --source=. --remote=origin --push --description "A searchable engineering fieldbook tracing agent-runtime decisions from problem to code, failure modes, and reusable implementation lessons."
```

The PDF is about 76.5 MB. It is below GitHub's 100 MB hard file limit, although GitHub may display its normal large-file warning. Do not recompress or rewrite the PDF unless its page count and hash are checked again.

## Add GitHub topics

```bash
gh repo edit TarunKurella/agent-runtime-notes --add-topic agent-systems --add-topic agent-runtime --add-topic ai-engineering --add-topic harness-engineering --add-topic coding-agents --add-topic llm-agents --add-topic rust --add-topic tokio --add-topic context-management --add-topic software-architecture --add-topic engineering-decisions --add-topic technical-writing
```

## Verify after publishing

```bash
gh repo view TarunKurella/agent-runtime-notes --web
git status --short
```

Expected checks:

- `git status --short` prints nothing.
- GitHub renders `README.md` at the repository front page.
- `02-notes/` contains 687 Markdown notes.
- `03-agent-context/` contains 687 numbered context files.
- `01-book/DeepSeek-Harness-Judgment-Edition.pdf` opens and has 3,050 pages.

## Prompt for the desktop agent

> Open `DESKTOP-HANDOFF.md` and publish this extracted folder exactly as `TarunKurella/agent-runtime-notes`. Use the stated public visibility, description, commit message, and GitHub topics. Do not rewrite generated notes or the PDF. Before pushing, confirm that the repository root contains the three numbered folders and that `git status` includes only this package. After pushing, verify the rendered README, file counts, repository topics, and clean working tree. Report the repository URL and commit SHA.
