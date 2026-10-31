# factory-kit

The prompts and agents from [Software Factory](https://github.com/owainlewis/software-factory), as plain files you copy into your repo. Give it a work ticket and get back a reviewed pull request.

Use this if you want to read and edit every prompt in your own repo, or share one setup with your team through git. For the installable plugin, Codex support, updates and the evals, use [Software Factory](https://github.com/owainlewis/software-factory). Both run the same prompts.

To plan the work first (requirements, a spec per feature, and small tickets), use [Blueprint](https://github.com/owainlewis/blueprint). A Blueprint spec can go straight to `/factory`.

## What's in here

```
.claude/
├── settings.json            # the git, gh and read-only commands the agents run
├── commands/
│   ├── factory.md           # /factory <ticket>   ticket to reviewed PR
│   └── review.md            # /review [branch]    review a branch, change nothing
└── agents/
    ├── triage.md            # checks the ticket against the code, writes a verified spec
    ├── implementer.md       # builds to the spec on a local branch, never pushes
    ├── reviewer.md          # reviews code, commits and PR text before anything is pushed
    ├── ux-reviewer.md       # uses page changes in a real browser
    ├── security-reviewer.md # proves each security hole with an input (/review)
    └── classifier.md        # rates the change low, medium or high risk (/review)
```

## Install

You need Claude Code and the [GitHub CLI](https://cli.github.com), logged in. For Linear tickets, connect a Linear MCP server.

```bash
git clone https://github.com/ai-developer-community/factory-kit
cp -R factory-kit/.claude/agents factory-kit/.claude/commands /path/to/your-repo/.claude/
```

Then merge `factory-kit/.claude/settings.json` into your repo's `.claude/settings.json` and add your own test and lint commands. Don't overwrite agents or settings you already have. Make sure `CLAUDE.md` lists your build, lint and test commands, because the agents run whatever it says.

## Use

From the root of your repo:

```
/factory #42
/factory https://linear.app/acme/issue/ENG-123
/factory "Add a --json flag to the export command"
/factory #42 --merge
/review
```

Triage writes a spec into the ticket: problem, behavior, decisions, assumptions, an acceptance table, and out of scope. The implementer builds it on a local branch with tests first. The reviewer checks the code, the commits and the PR description, and proves new tests fail when the code is reverted. Only reviewed work is pushed. The PR is yours to merge unless you pass `--merge`.

A ticket too big for one PR stops with a suggested split.

## Updating

Don't edit these files here. They are generated from `src/` in Software Factory:

```bash
python3 scripts/build.py --kit /path/to/factory-kit
```
