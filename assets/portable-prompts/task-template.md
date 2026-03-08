Use this template as the user prompt in any LLM tool:

---
Run an accessibility audit using the `accessibility-reviewer` process.

Target:
- Type: [full app | page | component | snippet | URL]
- Stack: [React/Vue/HTML/etc.]
- Conformance target: [WCAG 2.1 AA default, or specify]
- Assistive tech target (optional): [e.g., NVDA + Chrome]

Input to audit:
```[language]
[paste code, markup, or URL context]
```

Output requirements:
- Use this structure exactly:
  1. Accessibility Audit Report
  2. Summary
  3. Critical Issues
  4. Serious Issues
  5. Moderate Issues
  6. Passed Checks
  7. Recommendations
  8. Testing Checklist
- For each issue include:
  1. Issue title
  2. WCAG SC and level
  3. Impact
  4. Element/location
  5. Problem
  6. Fix with corrected code
---
