# Accessibility Reviewer

Accessibility audit skill for web UIs, focused on WCAG 2.1 AA with actionable code fixes.

## Quick Start

1. Install the skill (Codex/OpenAI skills runtime).
2. Invoke it on a URL or code snippet.
3. Get a prioritized audit report with WCAG citations and fixes.

## Installation

### Option 1: Install from GitHub (recommended)

```bash
npx skills add https://github.com/YKehinde/accessibility-reviewer
```

### Option 2: Manual install

1. Clone this repository:
```bash
git clone https://github.com/YKehinde/accessibility-reviewer.git
```
2. Copy the folder to your skills directory (for example, `$CODEX_HOME/skills/accessibility-reviewer`).
3. Restart your agent session so it reloads available skills.

## How It Works

The skill is triggered when the user asks for accessibility review/audit help, including:
- "a11y audit"
- "check WCAG compliance"
- "review keyboard navigation"
- "fix screen reader issues"

When triggered, it follows this workflow:

1. Scope the review target (URL/component/snippet), stack, and conformance target.
2. Audit using six pillars:
   - Perceivable
   - Operable
   - Understandable
   - Robust
   - Structure & Semantics
   - Interactive Patterns
3. Classify findings by severity:
   - Critical
   - Serious
   - Moderate
   - Advisory
4. Output a structured report with:
   - WCAG success criterion per issue
   - User impact
   - Location/element
   - Copy-paste fix code
   - Passed checks
   - Recommendations
   - Manual testing checklist

## Use Outside Codex Skills (Claude, Gemini, etc.)

Use the portable prompts in `assets/portable-prompts/`:

1. Paste `system-prompt.md` into the model's system/custom instructions.
2. Use `task-template.md` as your user prompt and fill it in.
3. Provide references as context:
   - `references/wcag-criteria.md`
   - `references/fix-patterns.md`

This reproduces the same audit behavior even where native Skills are not supported.

## Repository Structure

```text
.
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── wcag-criteria.md
│   └── fix-patterns.md
└── assets/portable-prompts/
    ├── system-prompt.md
    └── task-template.md
```

## Example Prompts

- "Run an accessibility audit of this checkout page against WCAG 2.1 AA."
- "Review this React form for keyboard and screen reader issues."
- "Find contrast and focus problems in this CSS and suggest fixes."

