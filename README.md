# rls-audit

[Agent Skills](https://agentskills.io)-compatible skill for auditing Row Level Security (RLS) policies in PostgreSQL / Supabase databases.

Works with Claude Code, OpenAI Codex, Cursor, Windsurf, GitHub Copilot, Gemini CLI, Roo Code, Junie (JetBrains), and any other agent that supports the Agent Skills standard.

## What it does

- Finds tables without RLS enabled
- Detects tables with RLS but no policies (silently broken)
- Flags overly permissive policies (`USING (true)`)
- Checks for missing `FORCE ROW LEVEL SECURITY`
- Identifies incomplete operation coverage (SELECT without INSERT/UPDATE/DELETE)
- Finds `SECURITY DEFINER` functions that bypass RLS
- Detects grants without matching RLS policies
- Checks for missing `WITH CHECK` on write policies
- Reports tenant isolation leaks in multi-tenant setups

## Install

### Claude Code / Codex (plugin)

Add the marketplace and install:

```
/plugin marketplace add ekhorkov/rls-audit
/plugin install rls-audit@ekhorkov-rls-audit
```

Or one-liner via CLI:

```bash
claude plugin marketplace add ekhorkov/rls-audit && claude plugin install rls-audit@ekhorkov-rls-audit
```

#### Manual install (per-user)

```bash
mkdir -p ~/.claude/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o ~/.claude/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/audit-queries.md \
  -o ~/.claude/skills/rls-audit/references/audit-queries.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/common-vulnerabilities.md \
  -o ~/.claude/skills/rls-audit/references/common-vulnerabilities.md
```

#### Manual install (per-project)

```bash
mkdir -p .claude/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o .claude/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/{audit-queries,common-vulnerabilities}.md \
  -o '.claude/skills/rls-audit/references/#1.md'
```

### OpenAI Codex

Codex reads instructions from `AGENTS.md` or markdown files in the repo:

```bash
mkdir -p codex/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o codex/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/{audit-queries,common-vulnerabilities}.md \
  -o 'codex/skills/rls-audit/references/#1.md'
```

Then reference it in your `AGENTS.md`:

```markdown
## Security
- For RLS audits, follow the workflow in `codex/skills/rls-audit/SKILL.md`
```

### Cursor

```bash
mkdir -p .cursor/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o .cursor/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/{audit-queries,common-vulnerabilities}.md \
  -o '.cursor/skills/rls-audit/references/#1.md'
```

Or add the content of `SKILL.md` to your `.cursorrules` file.

### Windsurf

```bash
mkdir -p .windsurf/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o .windsurf/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/{audit-queries,common-vulnerabilities}.md \
  -o '.windsurf/skills/rls-audit/references/#1.md'
```

Or add the content of `SKILL.md` to your `.windsurfrules` file.

### GitHub Copilot

```bash
mkdir -p .github/copilot/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/SKILL.md \
  -o .github/copilot/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/skills/rls-audit/references/{audit-queries,common-vulnerabilities}.md \
  -o '.github/copilot/skills/rls-audit/references/#1.md'
```

Or paste the content into `.github/copilot-instructions.md`.

### Other agents

The skill uses the open [Agent Skills](https://agentskills.io) format (`SKILL.md` + references). Copy the files into your agent's skills/instructions directory — the format is the same everywhere.

## Usage

Trigger the skill with any of these phrases:

- `/rls-audit`
- "аудит RLS"
- "проверка политик"
- "supabase security audit"
- "RLS gaps"

The skill will scan your Supabase migrations, run audit queries, classify findings by severity (CRITICAL / HIGH / MEDIUM / LOW), and generate a fix migration file.

## Structure

```
.claude-plugin/
  plugin.json                         — Plugin manifest (name, version, author)
  marketplace.json                    — Marketplace index for /plugin install
skills/
  rls-audit/
    SKILL.md                          — Skill definition and workflow
    references/
      audit-queries.md                — SQL queries for each audit check
      common-vulnerabilities.md       — Top 10 RLS vulnerability patterns
```

## License

MIT
