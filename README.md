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

### Claude Code / Codex

#### Per-user (available in all projects)

```bash
mkdir -p ~/.claude/skills/rls-audit
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/SKILL.md \
  -o ~/.claude/skills/rls-audit/SKILL.md
mkdir -p ~/.claude/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/audit-queries.md \
  -o ~/.claude/skills/rls-audit/references/audit-queries.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/common-vulnerabilities.md \
  -o ~/.claude/skills/rls-audit/references/common-vulnerabilities.md
```

#### Per-project (committed to repo)

```bash
mkdir -p .claude/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/SKILL.md \
  -o .claude/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/{audit-queries,common-vulnerabilities}.md \
  -o '.claude/skills/rls-audit/references/#1.md'
```

### OpenAI Codex

Codex reads instructions from `AGENTS.md` or markdown files in the repo. Add the skill to your project:

```bash
mkdir -p codex/skills/rls-audit/references
cp SKILL.md codex/skills/rls-audit/
cp references/*.md codex/skills/rls-audit/references/
```

Then reference it in your `AGENTS.md`:

```markdown
## Security
- For RLS audits, follow the workflow in `codex/skills/rls-audit/SKILL.md`
```

### Cursor

```bash
mkdir -p .cursor/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/SKILL.md \
  -o .cursor/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/{audit-queries,common-vulnerabilities}.md \
  -o '.cursor/skills/rls-audit/references/#1.md'
```

Or add the content of `SKILL.md` to your `.cursorrules` file.

### Windsurf

```bash
mkdir -p .windsurf/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/SKILL.md \
  -o .windsurf/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/{audit-queries,common-vulnerabilities}.md \
  -o '.windsurf/skills/rls-audit/references/#1.md'
```

Or add the content of `SKILL.md` to your `.windsurfrules` file.

### GitHub Copilot

Add the skill as a custom instruction:

```bash
mkdir -p .github/copilot/skills/rls-audit/references
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/SKILL.md \
  -o .github/copilot/skills/rls-audit/SKILL.md
curl -sL https://raw.githubusercontent.com/ekhorkov/rls-audit/main/references/{audit-queries,common-vulnerabilities}.md \
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
SKILL.md                              — Skill definition and workflow
references/
  audit-queries.md                    — SQL queries for each audit check
  common-vulnerabilities.md           — Top 10 RLS vulnerability patterns
```

## License

MIT
