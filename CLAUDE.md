# CLAUDE.md

Working guide for AI assistants (and humans) in the **skills** repository.

## What this repo is

A fork of [github.com/anthropics/skills](https://github.com/anthropics/skills) —
Anthropic's collection of **Agent Skills** for Claude — customised with a few
CCELAM NK & Associates quantity-surveying skills and a Playwright test harness.

A **skill** is a self-contained folder with a `SKILL.md` (YAML frontmatter +
instructions) plus any bundled scripts, references, and assets. Claude loads a
skill dynamically when a task matches its `description`, so the description is
what decides when the skill triggers — keep it honest and specific.

The repo doubles as a **Claude Code plugin marketplace** (see
`.claude-plugin/marketplace.json`).

## Repository layout

```
skills/
├── skills/                     # the skills themselves, one folder each
│   ├── docx/ pdf/ pptx/ xlsx/  # Document Skills (source-available, NOT open source)
│   ├── bill-of-quantities/     # CCELAM addition — BoQ from drawings/specs
│   ├── construction-project-management/  # CCELAM addition — schedules, risk, EVA
│   ├── skill-creator/ mcp-builder/ webapp-testing/ frontend-design/ …  # examples
│   └── … (algorithmic-art, brand-guidelines, canvas-design, internal-comms,
│         theme-factory, slack-gif-creator, web-artifacts-builder, doc-coauthoring, claude-api)
├── spec/agent-skills-spec.md   # the Agent Skills specification
├── template/SKILL.md           # minimal skill template to start from
├── tests/                      # Playwright specs (*.spec.ts)
├── playwright.config.ts        # Playwright config
├── package.json                # @playwright/test dev dep + test scripts
├── tsconfig.json
├── .claude-plugin/marketplace.json   # plugin marketplace (document-skills, example-skills)
├── BILL_OF_QUANTITIES_SETUP.md          # setup guide for the BoQ skill
├── CONSTRUCTION_PROJECT_MANAGEMENT_SETUP.md
├── QUICK_START.md / PLAYWRIGHT_GUIDE.md # Playwright harness docs
├── THIRD_PARTY_NOTICES.md               # per-skill licences — check before reuse
└── COMMON_WORKFLOWS.md                  # repo-specific Claude Code recipes
```

`.agent.md` defines a **skills-author agent** persona for working in this repo.
`kimi` and `llm wiki` are stray scratch files, not skills.

## Skill structure & conventions

- Each skill lives in its own folder under `skills/` with a `SKILL.md`.
- `SKILL.md` frontmatter is at minimum `name` (a lowercase slug matching the
  folder) and `description` (what it does **and when** to use it). Some skills
  add `compatibility` (e.g. the CCELAM skills note Python/openpyxl needs).
- Keep detail that isn't needed for triggering out of `SKILL.md` — put it in
  `reference.md`, `scripts/`, or `assets/` within the skill folder.
- Follow the **spec** in `spec/agent-skills-spec.md` and start new skills by
  copying `template/`.

## Creating / editing a skill

```text
copy template/ to skills/my-new-skill and fill in its SKILL.md per spec/agent-skills-spec.md
```

Keep edits **surgical** and keep the `SKILL.md` description accurate — a vague
description makes the skill fire on the wrong tasks (or never fire). Don't touch
files outside the skill you're working on unless asked.

## Plugin marketplace

`.claude-plugin/marketplace.json` exposes two plugins — `document-skills`
(`docx`, `pdf`, `pptx`, `xlsx`) and `example-skills` (the creative/technical
examples). Register and install in Claude Code:

```
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

If you add or rename a skill that belongs in a plugin, update its entry in
`marketplace.json` too.

## Playwright test harness

```bash
npm install
npx playwright install        # browsers — first run only (LOCAL machines)
npm test                      # all tests (playwright test)
npm run test:ui               # interactive UI runner (best for development)
npm run test:headed           # headed browser
npm run test:chrome           # single engine (also :firefox, :webkit)
npx playwright show-report    # HTML report
```

> **In this remote environment** Chromium is pre-installed and Playwright is
> configured to find it (`PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers`). Do **not**
> run `playwright install` here — launch the pre-installed browser instead.

New tests go under `tests/` as `*.spec.ts`, following `example.spec.ts` and the
pattern in `QUICK_START.md`.

## Licensing (important)

Skills carry **different licences**. Many examples are Apache 2.0, but the
Document Skills (`docx`, `pdf`, `pptx`, `xlsx`) are **source-available, not open
source**. Check `THIRD_PARTY_NOTICES.md` and any per-folder `LICENSE` before
reusing code elsewhere.

See `COMMON_WORKFLOWS.md` for more Claude Code recipes.
