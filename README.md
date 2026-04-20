# rls-audit

Claude Code skill for auditing Row Level Security (RLS) policies in PostgreSQL / Supabase databases.

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

```bash
claude install-skill ekhorkov/rls-audit
```

## Usage

In Claude Code, trigger the skill with any of these phrases:

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
