# Accessibility Reviewer Skill

A reusable accessibility audit skill focused on WCAG 2.1 AA with practical fixes.

## What this repo contains

- `SKILL.md`: Core skill instructions and audit workflow
- `references/wcag-criteria.md`: WCAG criteria reference used during audits
- `references/fix-patterns.md`: Copy-paste fix patterns for common issues
- `agents/openai.yaml`: OpenAI/Codex skill metadata
- `assets/portable-prompts/`: Prompt files for non-native skill runtimes

## Use with Codex/OpenAI Skills

1. Load this skill directory in your skills environment.
2. Trigger `accessibility-reviewer` for a11y reviews and WCAG checks.

## Use with Other LLMs (Claude, Gemini, etc.)

1. Use `assets/portable-prompts/system-prompt.md` as the system/instruction prompt.
2. Use `assets/portable-prompts/task-template.md` as the user prompt template.
3. Include context from:
   - `references/wcag-criteria.md`
   - `references/fix-patterns.md`

## Typical input

- A URL
- A component or page snippet (HTML/JSX/CSS)
- A full app section to audit

## Output produced by this skill

- Prioritized findings (Critical, Serious, Moderate, Advisory)
- WCAG success criteria citations per issue
- User impact explanation
- Concrete code-level fixes
- Passed checks, recommendations, and manual test checklist
