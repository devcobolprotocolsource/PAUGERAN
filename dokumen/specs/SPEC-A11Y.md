---
title: SPEC-A11Y — Spesifikasi Aksesibilitas
document_id: SPEC-A11Y
version: 1.0
cb_reference: [CB §28]
status: DRAFT
owner: Design & QA Team
last_updated: 2026-08-29
---

# SPEC-A11Y — Spesifikasi Aksesibilitas

Panduan implementasi aksesibilitas WCAG 2.1 AA.

---

## WCAG 2.1 AA Compliance Checklist

### Perceivable
- [x] Color contrast ≥ 4.5:1
- [x] Alternative text for images
- [x] Captions for video (future)
- [x] Adjustable text size (100-200%)

### Operable
- [x] Keyboard navigation (Tab, Arrow, Enter, Escape)
- [x] Focus indicators visible (outline ≥ 2px)
- [x] No keyboard traps
- [x] Links and buttons have clear purpose
- [x] No flashing (> 3x/second)

### Understandable
- [x] Language declared (lang="id")
- [x] Clear labels for form fields
- [x] Error messages clear and actionable
- [x] Consistent navigation
- [x] Reading order logical

### Robust
- [x] Valid HTML
- [x] Semantic HTML (<button>, <nav>, <main>)
- [x] ARIA labels where needed
- [x] Screen reader support tested

---

## Keyboard Navigation Map

| Key | Action |
|-----|--------|
| Tab | Next element |
| Shift+Tab | Previous element |
| Enter | Activate button/link |
| Space | Toggle checkbox/radio |
| Arrow Keys | Navigate menu/list |
| Escape | Close modal/dropdown |
| Cmd/Ctrl+K | Command palette |

---

## ARIA Labels

```jsx
// Button
<button aria-label="Close dialog">✕</button>

// Navigation
<nav aria-label="Main navigation">...</nav>

// Live region
<div aria-live="polite" aria-atomic="true">
  Status updates
</div>

// Describedby
<input 
  aria-describedby="error-msg"
  type="email"
/>
<div id="error-msg">Invalid email format</div>
```

---

## Screen Reader Testing

**Tools:**
- NVDA (Windows, free)
- JAWS (Windows, commercial)
- VoiceOver (macOS, built-in)
- TalkBack (Android, built-in)

**Test Scenarios:**
- [ ] Can navigate with keyboard only
- [ ] Content hierarchy clear
- [ ] Form labels announced
- [ ] Errors clearly communicated
- [ ] All functionality accessible

---

## Color Contrast

### Requirements

| Use | Min Ratio | Level AA |
|-----|-----------|----------|
| Normal text | 4.5:1 | ✅ Required |
| Large text | 3:1 | ✅ Required |
| Graphics | 3:1 | ✅ Required |
| Focus indicator | 3:1 | ✅ Required |

### Color Combinations to Avoid
- Red + Green (colorblind-unfriendly)
- Rely on color alone (always add pattern/text)

---

## Focus Management

### Focus Indicators
```css
*:focus {
  outline: 2px solid #0ea5e9;
  outline-offset: 2px;
}

*:focus-visible {
  /* Only show for keyboard navigation */
}
```

### Focus Trap (Modal)
```javascript
// Trap focus within modal
function trapFocus(modal) {
  const focusableElements = modal.querySelectorAll(
    'button, a, input, select, textarea'
  );
  const firstElement = focusableElements[0];
  const lastElement = focusableElements[focusableElements.length - 1];

  document.addEventListener('keydown', (e) => {
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === firstElement) {
        lastElement.focus();
        e.preventDefault();
      } else if (!e.shiftKey && document.activeElement === lastElement) {
        firstElement.focus();
        e.preventDefault();
      }
    }
  });
}
```

---

## Accessibility Modes

### High Contrast Mode
- Increase border thickness (2-3px)
- Higher color saturation
- Larger focus indicators (3-4px)
- Simplified animations

### Reduced Motion Mode
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Large Text Mode
- Max 200% scaling
- Responsive layouts that reflow
- No horizontal scrolling
- Sufficient touch target size (44x44px min)

---

## Common Accessibility Pitfalls

❌ **Don't:**
- Remove focus outlines
- Use color alone to convey info
- Disable zoom (viewport-fit)
- Create keyboard traps
- Nest heading levels incorrectly
- Use placeholder as label
- Forget alt text on images
- Auto-play audio/video

✅ **Do:**
- Provide keyboard alternatives
- Use semantic HTML
- Test with screen readers
- Ensure sufficient contrast
- Make interactive elements large enough
- Provide clear error messages
- Use proper heading hierarchy
- Label form fields clearly

---

## Testing Tools

| Tool | Type | Purpose |
|------|------|---------|
| axe DevTools | Automated | Find violations |
| Pa11y | CLI | Batch testing |
| WebAIM Contrast | Online | Check colors |
| WAVE | Browser ext | Visual feedback |
| Lighthouse | Chrome | Audit |

---

## Checklist Implementasi

- [ ] WCAG 2.1 AA checklist completed
- [ ] Keyboard navigation tested
- [ ] Screen reader tested (NVDA/JAWS/VoiceOver)
- [ ] Color contrast verified
- [ ] Focus indicators visible
- [ ] Accessibility modes implemented
- [ ] Automated testing in CI
- [ ] Manual testing documented
- [ ] Users with disabilities consulted

---

## Referensi

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM Resources](https://webaim.org/)
- [ARIA Authoring](https://www.w3.org/WAI/ARIA/apg/)

