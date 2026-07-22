# Common Workflows with Claude Code — skills

Repo-specific recipes for working on this **Anthropic Skills repository** with
Claude Code. For the general, product-agnostic guidance see the upstream
[Common workflows](https://code.claude.com/docs/en/common-workflows) page; this
file adapts those patterns to a skills repo that ships both example skills and
the production document skills, plus a Playwright test harness.

## Get oriented fast

The repo is organised into a few top-level areas (see `README.md`):

- [`skills/`](./skills) — the skill examples (Creative & Design, Development &
  Technical, Enterprise & Communication) and the Document Skills
  (`docx`, `pdf`, `pptx`, `xlsx`).
- [`spec/`](./spec) — the Agent Skills specification
  (`agent-skills-spec.md`).
- [`template/`](./template) — a skill template to start from.
- Playwright config + `tests/` — the web-testing harness.

```text
give me an overview of this repo — how skills/, spec/, template/ and the Playwright harness relate
```

Each skill is self-contained in its own folder with a `SKILL.md` file
(YAML frontmatter + instructions) that Claude loads dynamically.

## Create a new skill

Start from the template:

```text
copy template/ to skills/my-new-skill and fill in its SKILL.md following the agent-skills spec in spec/
```

A basic skill is just a folder with a `SKILL.md` containing YAML frontmatter
(`name`, `description`) and instructions — see the "Creating a Basic Skill"
section of `README.md`.

## Register and use skills in Claude Code

This repo is a Claude Code **plugin marketplace**:

```text
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

After installing, invoke a skill just by mentioning it, e.g. "Use the PDF skill
to extract the form fields from `path/to/file.pdf`".

## Run the Playwright tests

Setup and run (see `QUICK_START.md` and `PLAYWRIGHT_GUIDE.md`):

```bash
npm install
npx playwright install        # browsers — first time only

npm test                      # all tests (playwright test)
npm run test:ui               # interactive UI, best for development
npm run test:headed           # headed browser
npm run test:chrome           # single engine (also :firefox, :webkit)
npx playwright show-report    # HTML report
```

> Note: in this remote environment Chromium is pre-installed and Playwright is
> configured to find it — do **not** run `playwright install` here; launch the
> pre-installed browser instead.

Write new tests under `tests/` as `*.spec.ts` following the pattern in
`QUICK_START.md`.

## Fix a bug in a skill

```text
the xlsx skill's formula example in SKILL.md is wrong — here's what breaks. fix it and add a Playwright test that covers the corrected behaviour.
```

## Edit an existing skill

Keep edits surgical and keep the `SKILL.md` description honest — the description
is what decides when the skill triggers:

```text
tighten the frontend-design SKILL.md description so it triggers on UI/component work but not on backend tasks
```

## Create a PR

```text
summarize my changes to the docx skill and create a draft pr
```

## Delegate research

```text
use a subagent to list every skill under skills/ with its name and one-line description from SKILL.md frontmatter
```

## Licensing note

Skills here carry different licences — many are Apache 2.0, but the document
skills (`docx`, `pdf`, `pptx`, `xlsx`) are **source-available, not open
source**. Check `THIRD_PARTY_NOTICES.md` and each folder's `LICENSE` before
reusing code elsewhere.
