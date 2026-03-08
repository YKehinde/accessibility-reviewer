# WCAG 2.1 Success Criteria Reference

Quick reference for all WCAG 2.1 criteria relevant to web application audits. Organised by principle.

## Table of Contents
1. [Perceivable](#perceivable)
2. [Operable](#operable)
3. [Understandable](#understandable)
4. [Robust](#robust)
5. [WCAG 2.2 Additions](#wcag-22-additions)

---

## Perceivable

| SC | Name | Level | Notes |
|---|---|---|---|
| 1.1.1 | Non-text Content | A | Alt text for images, `aria-label` for icon buttons |
| 1.2.1 | Audio-only / Video-only | A | Provide transcript or audio description |
| 1.2.2 | Captions (Prerecorded) | A | Captions for video with audio |
| 1.2.3 | Audio Description / Media Alternative | A | |
| 1.2.4 | Captions (Live) | AA | |
| 1.2.5 | Audio Description (Prerecorded) | AA | |
| 1.3.1 | Info and Relationships | A | Semantic HTML, table headers, list markup |
| 1.3.2 | Meaningful Sequence | A | DOM order = reading order |
| 1.3.3 | Sensory Characteristics | A | Don't rely on shape/colour/position alone |
| 1.3.4 | Orientation | AA | Don't lock to portrait/landscape |
| 1.3.5 | Identify Input Purpose | AA | `autocomplete` attributes on personal data fields |
| 1.4.1 | Use of Colour | A | Don't use colour as the only visual means |
| 1.4.2 | Audio Control | A | Pause/stop/mute for auto-playing audio |
| 1.4.3 | Contrast (Minimum) | AA | 4.5:1 normal text, 3:1 large text |
| 1.4.4 | Resize Text | AA | Text resizable to 200% without assistive tech |
| 1.4.5 | Images of Text | AA | Avoid text in images |
| 1.4.10 | Reflow | AA | No horizontal scroll at 320px width |
| 1.4.11 | Non-text Contrast | AA | 3:1 for UI components and focus indicators |
| 1.4.12 | Text Spacing | AA | No loss of content when spacing overridden |
| 1.4.13 | Content on Hover or Focus | AA | Hoverable, dismissible, persistent tooltips |

---

## Operable

| SC | Name | Level | Notes |
|---|---|---|---|
| 2.1.1 | Keyboard | A | All functionality available via keyboard |
| 2.1.2 | No Keyboard Trap | A | Can always navigate away via keyboard |
| 2.1.4 | Character Key Shortcuts | A | Single-char shortcuts can be turned off/remapped |
| 2.2.1 | Timing Adjustable | A | Turn off, adjust, or extend time limits |
| 2.2.2 | Pause, Stop, Hide | A | Control for moving/blinking content |
| 2.3.1 | Three Flashes or Below | A | No content flashes >3 times/sec |
| 2.4.1 | Bypass Blocks | A | Skip navigation link |
| 2.4.2 | Page Titled | A | Descriptive `<title>` on each page/view |
| 2.4.3 | Focus Order | A | Logical focus sequence |
| 2.4.4 | Link Purpose (In Context) | A | Link text describes destination |
| 2.4.5 | Multiple Ways | AA | Multiple ways to find pages |
| 2.4.6 | Headings and Labels | AA | Descriptive headings and labels |
| 2.4.7 | Focus Visible | AA | Keyboard focus indicator is visible |
| 2.5.1 | Pointer Gestures | A | Multi-point gestures have single-pointer alternative |
| 2.5.2 | Pointer Cancellation | A | Up-event for activation |
| 2.5.3 | Label in Name | A | Accessible name contains visible label text |
| 2.5.4 | Motion Actuation | A | Functions with motion have button alternative |

---

## Understandable

| SC | Name | Level | Notes |
|---|---|---|---|
| 3.1.1 | Language of Page | A | `<html lang="en">` |
| 3.1.2 | Language of Parts | AA | `lang` attribute on content in other languages |
| 3.2.1 | On Focus | A | No context change on focus alone |
| 3.2.2 | On Input | A | No unexpected context change on input |
| 3.2.3 | Consistent Navigation | AA | Navigation in same relative order across pages |
| 3.2.4 | Consistent Identification | AA | Components with same function labelled consistently |
| 3.3.1 | Error Identification | A | Errors described in text |
| 3.3.2 | Labels or Instructions | A | Labels/instructions for user input |
| 3.3.3 | Error Suggestion | AA | Suggestions for correcting errors |
| 3.3.4 | Error Prevention (Legal, Financial, Data) | AA | Confirmation/review for important submissions |

---

## Robust

| SC | Name | Level | Notes |
|---|---|---|---|
| 4.1.1 | Parsing | A | No duplicate IDs, complete start/end tags |
| 4.1.2 | Name, Role, Value | A | Correct ARIA name, role, state for all UI components |
| 4.1.3 | Status Messages | AA | `aria-live` / `role="status"` for dynamic updates |

---

## WCAG 2.2 Additions (Flag as Advisory unless client targets 2.2)

| SC | Name | Level | Notes |
|---|---|---|---|
| 2.4.11 | Focus Appearance (Minimum) | AA | Focus indicator: 2px outline, 3:1 contrast |
| 2.4.12 | Focus Appearance (Enhanced) | AAA | Larger focus indicators |
| 2.4.13 | Focus Appearance | AA | Area of focus indicator ≥ perimeter × 2px |
| 2.5.7 | Dragging Movements | AA | Single pointer alternative for drag actions |
| 2.5.8 | Target Size (Minimum) | AA | 24×24px minimum target size |
| 3.2.6 | Consistent Help | A | Help mechanisms in consistent location |
| 3.3.7 | Redundant Entry | A | Don't ask for same info twice in a process |
| 3.3.8 | Accessible Authentication (Minimum) | AA | No cognitive tests for login |

---

## Key ARIA Roles Quick Reference

```
Landmark roles: banner, navigation, main, complementary, contentinfo, search, form, region
Widget roles: button, checkbox, combobox, dialog, gridcell, link, listbox, menuitem, 
              option, progressbar, radio, scrollbar, searchbox, slider, spinbutton, 
              switch, tab, tabpanel, textbox, treeitem
Live region roles: alert, log, marquee, status, timer
```

## ARIA State/Property Cheatsheet

```
aria-expanded    — open/closed state (accordions, dropdowns)
aria-haspopup    — indicates popup (menu, listbox, tree, grid, dialog)
aria-controls    — IDs of controlled element(s)
aria-owns        — IDs of owned elements (when DOM structure can't represent ownership)
aria-labelledby  — IDs of labelling element(s)
aria-describedby — IDs of describing element(s)
aria-live        — off | polite | assertive
aria-atomic      — whether to announce entire region or just change
aria-relevant    — additions | removals | text | all
aria-busy        — true while content is loading
aria-disabled    — disabled state (still in focus order, unlike HTML disabled)
aria-hidden      — remove from accessibility tree
aria-invalid     — true | false | grammar | spelling
aria-required    — required field
aria-checked     — true | false | mixed
aria-selected    — true | false (listbox, tree, grid options)
aria-pressed     — true | false | mixed (toggle buttons)
aria-current     — page | step | location | date | time | true
aria-modal       — true (dialogs — reduces background noise in screen readers)
```
