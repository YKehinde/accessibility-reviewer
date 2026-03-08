# Accessibility Fix Patterns

Copy-pasteable templates for common accessibility fixes. Adapt to the user's tech stack.

---

## Focus Ring Restoration

Never remove focus outlines without a replacement. Restore them for keyboard users only:

```css
/* Option 1: Restore native focus ring for keyboard users only */
:focus-visible {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
}

/* Hide for mouse users */
:focus:not(:focus-visible) {
  outline: none;
}

/* Option 2: Custom high-visibility focus ring */
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 3px;
  border-radius: 2px;
  box-shadow: 0 0 0 5px rgba(0, 95, 204, 0.25);
}
```

---

## Skip Navigation Link

```html
<!-- Place as first element in <body> -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<style>
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  z-index: 9999;
  padding: 0.5rem 1rem;
  background: #000;
  color: #fff;
  font-weight: 600;
  text-decoration: none;
  transition: top 0.1s;
}

.skip-link:focus {
  top: 0;
}
</style>

<main id="main-content" tabindex="-1">
  <!-- page content -->
</main>
```

React version:
```jsx
const SkipLink = () => (
  <>
    <a href="#main-content" className="skip-link">
      Skip to main content
    </a>
    {/* CSS from above in your stylesheet or CSS-in-JS */}
  </>
);
```

---

## ARIA Live Region

```html
<!-- Polite: announces after user finishes current action (most cases) -->
<div role="status" aria-live="polite" aria-atomic="true" class="sr-only" id="status-message">
  <!-- Dynamically update textContent via JS -->
</div>

<!-- Assertive: interrupts immediately (only for critical errors) -->
<div role="alert" aria-live="assertive" aria-atomic="true" class="sr-only" id="error-message">
</div>
```

```css
/* Screen reader only utility class */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

React hook for announcements:
```jsx
import { useEffect, useRef } from 'react';

export function useAnnounce() {
  const ref = useRef(null);

  const announce = (message, priority = 'polite') => {
    if (!ref.current) return;
    ref.current.setAttribute('aria-live', priority);
    // Clear then set to ensure re-announcement of same message
    ref.current.textContent = '';
    setTimeout(() => { ref.current.textContent = message; }, 50);
  };

  const LiveRegion = () => (
    <div
      ref={ref}
      role="status"
      aria-live="polite"
      aria-atomic="true"
      className="sr-only"
    />
  );

  return { announce, LiveRegion };
}
```

---

## Accessible Modal / Dialog

```html
<!-- Trigger button -->
<button type="button" aria-haspopup="dialog" id="open-modal">Open dialog</button>

<!-- Dialog -->
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
  id="my-dialog"
  tabindex="-1"
>
  <h2 id="dialog-title">Dialog Title</h2>
  <p id="dialog-desc">Optional description of what this dialog does.</p>

  <!-- dialog content -->

  <button type="button" id="close-modal" aria-label="Close dialog">×</button>
</div>
```

```javascript
// Focus management
const dialog = document.getElementById('my-dialog');
const trigger = document.getElementById('open-modal');
const focusableSelectors = 'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])';

function openDialog() {
  dialog.removeAttribute('hidden');
  dialog.focus(); // Move focus into dialog
}

function closeDialog() {
  dialog.setAttribute('hidden', '');
  trigger.focus(); // Return focus to trigger
}

// Trap focus within dialog
dialog.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') return closeDialog();

  if (e.key === 'Tab') {
    const focusable = [...dialog.querySelectorAll(focusableSelectors)];
    const first = focusable[0];
    const last = focusable[focusable.length - 1];

    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  }
});
```

---

## Form Label Association

```html
<!-- Method 1: Implicit label (wrapping) -->
<label>
  Email address
  <input type="email" name="email" autocomplete="email" />
</label>

<!-- Method 2: Explicit label (for/id) — preferred -->
<label for="email">Email address</label>
<input type="email" id="email" name="email" autocomplete="email" />

<!-- Method 3: aria-label (no visible label — use sparingly) -->
<input type="search" aria-label="Search products" placeholder="Search…" />

<!-- Method 4: aria-labelledby (label is elsewhere in DOM) -->
<h2 id="billing-heading">Billing Address</h2>
<input type="text" aria-labelledby="billing-heading" name="billing-street" />

<!-- Error association -->
<label for="email">
  Email address
  <span aria-hidden="true">*</span>
  <span class="sr-only">(required)</span>
</label>
<input
  type="email"
  id="email"
  name="email"
  aria-required="true"
  aria-describedby="email-error"
  aria-invalid="true"
/>
<p id="email-error" role="alert">
  Please enter a valid email address.
</p>
```

---

## Icon Button Labelling

```html
<!-- SVG icon button — always needs a label -->
<button type="button" aria-label="Close">
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="M18 6L6 18M6 6l12 12" />
  </svg>
</button>

<!-- Icon + visible text — hide icon from AT -->
<button type="button">
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="..." />
  </svg>
  Delete item
</button>
```

React component:
```jsx
const IconButton = ({ icon: Icon, label, onClick, ...props }) => (
  <button
    type="button"
    aria-label={label}
    onClick={onClick}
    {...props}
  >
    <Icon aria-hidden="true" focusable="false" />
  </button>
);
```

---

## Accessible Accordion

```html
<div class="accordion">
  <h3>
    <button
      type="button"
      aria-expanded="false"
      aria-controls="section1-content"
      id="section1-header"
    >
      Section 1
    </button>
  </h3>
  <div
    id="section1-content"
    role="region"
    aria-labelledby="section1-header"
    hidden
  >
    <p>Content here</p>
  </div>
</div>
```

```javascript
document.querySelectorAll('.accordion button').forEach(btn => {
  btn.addEventListener('click', () => {
    const expanded = btn.getAttribute('aria-expanded') === 'true';
    btn.setAttribute('aria-expanded', String(!expanded));
    document.getElementById(btn.getAttribute('aria-controls'))
      .toggleAttribute('hidden');
  });
});
```

---

## Accessible Tabs

```html
<div role="tablist" aria-label="Settings">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1" tabindex="0">General</button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2" tabindex="-1">Security</button>
  <button role="tab" aria-selected="false" aria-controls="panel-3" id="tab-3" tabindex="-1">Notifications</button>
</div>

<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">General settings…</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>Security settings…</div>
<div role="tabpanel" id="panel-3" aria-labelledby="tab-3" hidden>Notification settings…</div>
```

Arrow key navigation (required for tablist):
```javascript
const tabs = document.querySelectorAll('[role="tab"]');

tabs.forEach((tab, index) => {
  tab.addEventListener('keydown', (e) => {
    let newIndex;
    if (e.key === 'ArrowRight') newIndex = (index + 1) % tabs.length;
    if (e.key === 'ArrowLeft') newIndex = (index - 1 + tabs.length) % tabs.length;
    if (e.key === 'Home') newIndex = 0;
    if (e.key === 'End') newIndex = tabs.length - 1;

    if (newIndex !== undefined) {
      tabs[newIndex].click();
      tabs[newIndex].focus();
    }
  });
});
```

---

## Reduced Motion

```css
/* Disable transitions and animations for users who prefer reduced motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

React hook:
```jsx
import { useEffect, useState } from 'react';

export function usePrefersReducedMotion() {
  const [prefersReducedMotion, setPrefersReducedMotion] = useState(false);

  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    setPrefersReducedMotion(mq.matches);
    const handler = (e) => setPrefersReducedMotion(e.matches);
    mq.addEventListener('change', handler);
    return () => mq.removeEventListener('change', handler);
  }, []);

  return prefersReducedMotion;
}
```

---

## Colour Contrast Quick Reference

WCAG 2.1 AA minimums:
- Normal text (< 18pt / 14pt bold): **4.5:1**
- Large text (≥ 18pt / 14pt bold): **3:1**
- UI components & focus indicators: **3:1**

Common compliant pairings:
```
#000000 on #ffffff  → 21:1  ✓
#1a1a1a on #f5f5f5  → 16.1:1  ✓
#595959 on #ffffff  → 7:1  ✓
#767676 on #ffffff  → 4.54:1  ✓ (barely passes AA)
#949494 on #ffffff  → 2.85:1  ✗ FAILS
#005fcc on #ffffff  → 5.9:1  ✓ (accessible blue)
#d0021b on #ffffff  → 5.1:1  ✓ (accessible red)
```

---

## Page Structure Template

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Page Title — Site Name</title>
</head>
<body>

  <!-- Skip link (must be first) -->
  <a href="#main-content" class="skip-link">Skip to main content</a>

  <header role="banner">
    <nav aria-label="Primary navigation">
      <ul>
        <li><a href="/" aria-current="page">Home</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <main id="main-content" tabindex="-1">
    <h1>Page Heading</h1>
    <!-- page content -->
  </main>

  <aside aria-label="Related content">
    <!-- sidebar -->
  </aside>

  <footer role="contentinfo">
    <nav aria-label="Footer navigation">
      <!-- footer links -->
    </nav>
  </footer>

  <!-- Screen reader announcements -->
  <div role="status" aria-live="polite" aria-atomic="true" class="sr-only" id="announcements"></div>

</body>
</html>
```
