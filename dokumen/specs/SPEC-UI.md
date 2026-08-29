---
title: SPEC-UI — Spesifikasi Antarmuka Pengguna
document_id: SPEC-UI
version: 1.0
cb_reference: [CB §27], [CB §29]
status: DRAFT
owner: Design & Frontend Team
last_updated: 2026-08-29
---

# SPEC-UI — Spesifikasi Antarmuka Pengguna

Panduan desain UI yang konsisten.

## Referensi CB
- [CB §27] — UI/UX requirements
- [CB §29] — User experience goals

---

## Design Tokens

### Colors (Light Mode)

```json
{
  "primary": {
    "50": "#f0f9ff",
    "100": "#e0f2fe",
    "500": "#0ea5e9",
    "600": "#0284c7",
    "900": "#0c2d6b"
  },
  "success": { "500": "#10b981", "600": "#059669" },
  "warning": { "500": "#f59e0b", "600": "#d97706" },
  "danger": { "500": "#ef4444", "600": "#dc2626" },
  "neutral": {
    "50": "#f9fafb",
    "100": "#f3f4f6",
    "500": "#6b7280",
    "900": "#111827"
  }
}
```

### Dark Mode

```json
{
  "primary": {
    "50": "#0c2d6b",
    "500": "#0ea5e9",
    "900": "#f0f9ff"
  },
  "background": "#0f172a",
  "surface": "#1e293b",
  "text": "#e2e8f0"
}
```

### Sepia Mode (Legal Theme)

```json
{
  "primary": "#8b5a3c",
  "background": "#f5e6d3",
  "surface": "#ede5d9",
  "text": "#3e2723"
}
```

---

## Typography

### Font Stack
- **Heading:** Inter, -apple-system, BlinkMacSystemFont
- **Body:** Inter, -apple-system, BlinkMacSystemFont
- **Code:** Fira Code, monospace

### Scales

| Scale | Size | Weight | Line Height |
|-------|------|--------|-------------|
| H1 | 32px | 700 | 1.2 |
| H2 | 24px | 700 | 1.3 |
| H3 | 20px | 600 | 1.4 |
| Body | 16px | 400 | 1.5 |
| Small | 14px | 400 | 1.4 |
| Caption | 12px | 500 | 1.3 |
| Code | 14px | 500 | 1.6 |

---

## Spacing System

```
4px → 8px → 12px → 16px → 24px → 32px → 48px → 64px
(xs)   (s)   (sm)   (md)   (lg)   (xl)   (2xl)  (3xl)
```

---

## Component Library Specification

### Button Component

```jsx
<Button variant="primary" size="md" disabled={false}>
  Action
</Button>
```

**Variants:**
- `primary` — Main action
- `secondary` — Alternative action
- `tertiary` — Minimal action
- `danger` — Destructive action

**Sizes:**
- `sm` — Small (32px)
- `md` — Medium (40px, default)
- `lg` — Large (48px)

**States:**
- Default
- Hover
- Active
- Disabled
- Loading

### Input Component

```jsx
<Input 
  label="Search"
  placeholder="Type to search..."
  value={searchTerm}
  onChange={handleChange}
  error="Invalid format"
/>
```

**Variants:**
- Text input
- Password input
- Email input
- Number input
- Textarea

**States:**
- Default
- Focus
- Error
- Disabled
- Filled

### Card Component

```jsx
<Card title="Title" subtitle="Subtitle">
  <CardContent>...</CardContent>
  <CardActions>...</CardActions>
</Card>
```

### Modal/Dialog Component

```jsx
<Dialog open={isOpen} onClose={handleClose}>
  <DialogTitle>Confirm Action</DialogTitle>
  <DialogContent>Are you sure?</DialogContent>
  <DialogActions>
    <Button variant="secondary">Cancel</Button>
    <Button variant="primary">Confirm</Button>
  </DialogActions>
</Dialog>
```

---

## Layout Rules

### Responsive Breakpoints

| Breakpoint | Width | Target |
|-----------|-------|--------|
| Mobile | 320-640px | Phones |
| Tablet | 641-1024px | Tablets |
| Desktop | 1025-1440px | Computers |
| Large | 1441+px | Large monitors |

### Layout Grid

- **Mobile:** Single column, full width
- **Tablet:** 2 columns, 16px gutter
- **Desktop:** 3-4 columns, 24px gutter
- **Large:** 4-6 columns, 32px gutter

### Spacing Rules

```
┌─────────────────────────────────────┐
│ Header (60px)                       │
├─────────────────────────────────────┤
│ Sidebar │ Content Area              │
│ (240px) │ (max-width: 1000px)       │
│         │                           │
│         ├─ Padding: 24px            │
│         ├─ Gap: 16px between items  │
│         │                           │
│         └─ Footer (60px)            │
└─────────────────────────────────────┘
```

---

## State Visualizations

### Loading State

```
┌─────────────────────┐
│ [====>    ] Loading │
│ Please wait...      │
└─────────────────────┘
```

### Empty State

```
┌──────────────────────────────┐
│ 📋 No chats yet              │
│                              │
│ Start a new conversation     │
│ to get legal insights        │
│                              │
│  [Create New Chat] button    │
└──────────────────────────────┘
```

### Error State

```
┌──────────────────────────────┐
│ ⚠️  Something went wrong     │
│                              │
│ We couldn't process your     │
│ request. Please try again.   │
│                              │
│ Error ID: abc123def456       │
│  [Retry] [Report Issue]      │
└──────────────────────────────┘
```

### Success State

```
┌──────────────────────────────┐
│ ✓ Document exported!         │
│                              │
│ [Open] [Save As] [Copy Link] │
└──────────────────────────────┘
```

---

## Animation Guidelines

### Transitions

- **Fast:** 150ms (micro-interactions)
- **Normal:** 300ms (standard transitions)
- **Slow:** 500ms (attention-grabbing)

### Easing Functions

- **Ease-out:** For elements entering view
- **Ease-in:** For elements leaving view
- **Ease-in-out:** For continuous transitions

### Examples

```css
/* Button hover */
button {
  transition: background-color 150ms ease-out;
}

/* Modal entrance */
@keyframes modalEnter {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

/* Loading spinner */
@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
animation: spin 1s linear infinite;
```

---

## Icon Library

### Icon Set

- **Library:** Heroicons
- **Size:** 16px, 20px, 24px
- **Color:** Inherit from text color
- **Weight:** 1.5px stroke

### Common Icons

| Icon | Usage |
|------|-------|
| 📝 | Chat, compose |
| 🔍 | Search, magnifier |
| ⚙️ | Settings |
| 📊 | Analytics, insights |
| 💾 | Save, export |
| 🔑 | License, security |
| 📚 | Knowledge base |
| 🌐 | Web research |
| ✓ | Success, confirmation |
| ✕ | Close, cancel |
| ! | Warning, attention |
| ? | Help, info |

---

## Dark Mode & Theme Variants

### Theme Switching

```javascript
// System preference detection
const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

// User override
localStorage.setItem('theme', 'dark'); // or 'light', 'sepia'

// Apply theme
document.documentElement.setAttribute('data-theme', theme);
```

### CSS Variables

```css
:root {
  --color-primary: #0ea5e9;
  --color-background: #ffffff;
  --color-text: #111827;
}

[data-theme="dark"] {
  --color-primary: #0ea5e9;
  --color-background: #0f172a;
  --color-text: #e2e8f0;
}

[data-theme="sepia"] {
  --color-primary: #8b5a3c;
  --color-background: #f5e6d3;
  --color-text: #3e2723;
}
```

---

## Focus Management

### Keyboard Navigation

- **Tab:** Move to next focusable element
- **Shift + Tab:** Move to previous focusable element
- **Enter:** Activate button or submit form
- **Escape:** Close modal/dropdown/popover
- **Space:** Toggle checkbox/radio
- **Arrow keys:** Navigate menu/list items

### Visual Focus Indicator

```css
*:focus {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

---

## Accessibility Features (WCAG 2.1 AA)

### Built-in Features

- [ ] Semantic HTML (buttons, links, headings)
- [ ] ARIA labels for interactive elements
- [ ] Color contrast ratio ≥ 4.5:1
- [ ] Focus indicators visible
- [ ] Keyboard navigation complete
- [ ] Screen reader support
- [ ] Form labels associated
- [ ] Error messages clear and actionable

### Accessibility Modes

1. **High Contrast Mode**
   - Increased color contrast (7:1+)
   - Thicker borders
   - Larger focus indicators

2. **Reduced Motion Mode**
   - Disable animations
   - Instant transitions
   - Static elements

3. **Large Text Mode**
   - 120% font size scaling
   - Adjusted spacing
   - Responsive layout

---

## Common Layouts

### Chat Interface

```
┌─────────────────────────────────────┐
│ PAUGERAN │ Chat Title   │ ⚙️ ⋮     │
├────────┬─────────────────────────────┤
│ Chats  │ Messages                    │
│ ─────  │ ────────────                │
│ [1]    │ You: Query?                 │
│ [2]    │                             │
│ [3]    │ Assistant: Response...      │
│        │                             │
│ + New  │ [Citation] "Pasal 12"       │
│        │                             │
│        │ You: Follow-up?             │
│        │                             │
│        │ Assistant: Yes, because...  │
│        │                             │
│        ├─────────────────────────────┤
│        │ Type message... │ [Send ➜] │
└────────┴─────────────────────────────┘
```

### Export Dialog

```
┌───────────────────────────────────┐
│ Export Chat                   [✕] │
├───────────────────────────────────┤
│ Format: [PDF ▾] [DOCX] [MARKDOWN] │
│                                   │
│ Template:                         │
│ [🔘] Standard                     │
│ [ ] Formal                        │
│ [ ] Memorandum                    │
│ [ ] Opinion Letter                │
│                                   │
│ Options:                          │
│ [☑] Include citations             │
│ [☑] Include metadata              │
│ [☐] Highlight changes             │
│                                   │
│        [Cancel] [Export ✓]        │
└───────────────────────────────────┘
```

---

## Checklist Implementasi

- [ ] Design tokens finalized
- [ ] Typography system implemented
- [ ] Spacing system consistent
- [ ] Component library created
- [ ] Responsive layouts tested
- [ ] State visualizations implemented
- [ ] Animations smooth and performant
- [ ] Icons integrated
- [ ] Theme switching working
- [ ] Accessibility compliance verified
- [ ] Focus management complete
- [ ] Keyboard navigation tested

---

## Referensi Tambahan

- [SolidJS Components](https://docs.solidjs.com)
- [Heroicons](https://heroicons.com)
- [Web Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
