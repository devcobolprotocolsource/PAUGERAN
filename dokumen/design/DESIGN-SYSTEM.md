---
title: DESIGN-SYSTEM — Design System
document_id: DESIGN-SYSTEM
version: 1.0
cb_reference: [CB §27], [CB §29]
status: DRAFT
owner: Design Team
last_updated: 2026-08-29
---

# DESIGN-SYSTEM — Design System

Sistem desain yang konsisten.

## Referensi CB
- [CB §27] — Design consistency
- [CB §29] — User experience

---

## Design System Overview

PAUGERAN Design System adalah comprehensive collection of components, patterns, dan guidelines untuk membangun konsisten UI across web dan desktop platforms.

---

## Design Tokens

### Color System

#### Primary Palette
```json
{
  "primary": {
    "50": "#f0f9ff",
    "100": "#e0f2fe", 
    "200": "#bae6fd",
    "300": "#7dd3fc",
    "400": "#38bdf8",
    "500": "#0ea5e9",
    "600": "#0284c7",
    "700": "#0369a1",
    "800": "#075985",
    "900": "#0c2d6b"
  }
}
```

#### Semantic Colors
- **Success:** #10b981
- **Warning:** #f59e0b
- **Danger:** #ef4444
- **Info:** #06b6d4
- **Neutral:** #6b7280

### Spacing Scale

```
4px (xs)
8px (s)
12px (sm)
16px (md) ← Default
24px (lg)
32px (xl)
48px (2xl)
64px (3xl)
```

### Border Radius

```
0px (none)
2px (xs)
4px (sm)
6px (md) ← Default
8px (lg)
12px (xl)
16px (2xl)
9999px (full/pill)
```

### Shadows

```
None
SM: 0 1px 2px 0 rgba(0,0,0,0.05)
MD: 0 4px 6px -1px rgba(0,0,0,0.1)
LG: 0 10px 15px -3px rgba(0,0,0,0.1)
XL: 0 20px 25px -5px rgba(0,0,0,0.1)
```

### Typography

| Name | Size | Weight | Line Height | Letter Spacing |
|------|------|--------|-------------|----------------|
| Display | 48px | 700 | 1.1 | -0.02em |
| H1 | 36px | 700 | 1.2 | -0.01em |
| H2 | 28px | 700 | 1.3 | 0 |
| H3 | 24px | 600 | 1.4 | 0 |
| H4 | 20px | 600 | 1.5 | 0 |
| Body | 16px | 400 | 1.5 | 0 |
| Body-SM | 14px | 400 | 1.43 | 0.25px |
| Caption | 12px | 500 | 1.33 | 0.4px |
| Mono | 14px | 500 | 1.5 | 0 |

---

## Component Library

### Core Components

1. **Button** — Primary action
   - Variants: primary, secondary, tertiary, danger
   - Sizes: sm, md, lg
   - States: default, hover, active, disabled, loading

2. **Input** — Text input
   - Types: text, email, password, number, tel, url
   - States: default, focus, filled, error, disabled

3. **Textarea** — Multi-line text
   - Auto-resize option
   - Char counter (optional)

4. **Select** — Dropdown selection
   - Single & multi-select
   - Searchable option
   - Grouped options

5. **Checkbox** — Boolean toggle
   - Indeterminate state
   - Disabled state
   - Group coordination

6. **Radio** — Exclusive selection
   - Button group variant
   - Icon + text variant

7. **Toggle** — On/off switch
   - Loading state
   - Disabled state

8. **Card** — Content container
   - Header, body, footer sections
   - Hover elevation
   - Interactive state

9. **Badge** — Label tag
   - Variants: solid, outline
   - Color options
   - Size options

10. **Link** — Text link
    - Underline variants
    - External icon
    - Loading state

### Complex Components

11. **Modal/Dialog** — Overlay content
    - Alert, confirm, custom
    - Scrollable content
    - Keyboard handling

12. **Toast/Notification** — Transient message
    - Types: success, error, warning, info
    - Auto-dismiss
    - Action button

13. **Dropdown Menu** — Context menu
    - Keyboard navigation
    - Icon support
    - Divider groups

14. **Tooltip** — Inline hint
    - Position: top, bottom, left, right
    - Delay options

15. **Popover** — Contextual panel
    - Anchored to element
    - Dismissable

16. **Table** — Data display
    - Sortable columns
    - Selectable rows
    - Pagination

17. **Pagination** — Page navigation
    - Previous/next buttons
    - Page numbers
    - Disabled states

18. **Breadcrumb** — Navigation hierarchy
    - Separator customizable
    - Current page highlight

19. **Tabs** — Content panels
    - Horizontal & vertical
    - Icon + text
    - Keyboard navigation

20. **Accordion** — Collapsible sections
    - Single & multi-expand
    - Lazy loading support

---

## Icon Library

### Icon Set: Heroicons

**Categories:**
- Navigation (arrow-left, arrow-right, menu)
- Communication (chat, bell, mail)
- Media (camera, image, play)
- Editor (link, bold, italic)
- Documents (document, archive, folder)
- Objects (chart-bar, briefcase, calendar)
- Settings (cog, slider, lock)

### Usage

```jsx
import { ChevronRightIcon } from 'heroicons';

<ChevronRightIcon className="w-5 h-5" />
```

### Sizes

- 16px (xs) — Small icons in text
- 20px (sm) — Standard in buttons
- 24px (md) — Default
- 32px (lg) — Large standalone
- 48px (xl) — Hero/featured

---

## Layout Patterns

### Main Layout

```
┌─────────────────────────────────────┐
│ Header (60px)                       │
├─────────────────────────────────────┤
│ │                                   │
│ │ Sidebar (240px)  │ Content Area   │
│ │                  │ (flex-grow)    │
│ │                  │                │
│ └──────────────────┴─────────────────┤
│ Footer (60px)                       │
└─────────────────────────────────────┘
```

### Grid System

- Desktop: 12-column grid
- Tablet: 8-column grid
- Mobile: 4-column grid
- Gutter: 16px/24px/32px

### Responsive Stacks

```jsx
<Stack responsive>
  <Item responsive cols={{ xs: 12, md: 6, lg: 4 }} />
  <Item responsive cols={{ xs: 12, md: 6, lg: 4 }} />
  <Item responsive cols={{ xs: 12, md: 6, lg: 4 }} />
</Stack>
```

---

## Motion & Animation

### Transitions

```css
/* Micro-interactions (150ms) */
button:hover {
  transition: all 150ms ease-out;
}

/* Standard transitions (300ms) */
.card {
  transition: transform 300ms ease-in-out;
}

/* Attention-grabbing (500ms) */
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
```

### Animation Principles

- **Purpose:** Every animation should serve a purpose
- **Subtle:** Avoid distracting
- **Responsive:** Respect prefers-reduced-motion
- **Performant:** Use transform & opacity
- **Consistent:** Use defined durations and easing

---

## Accessibility

### WCAG 2.1 AA Compliance

✅ **Requirements:**
- Color contrast ≥ 4.5:1 for text
- Focus indicators visible
- Semantic HTML
- ARIA labels where needed
- Keyboard navigation
- Screen reader support

### Accessibility Testing

- Automated: axe, Pa11y
- Manual: Screen reader testing, keyboard navigation
- User testing: People with disabilities

---

## Dark Mode

### Implementation

```css
/* Light mode (default) */
:root {
  --bg-primary: #ffffff;
  --text-primary: #111827;
  --border-primary: #e5e7eb;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #0f172a;
    --text-primary: #e2e8f0;
    --border-primary: #334155;
  }
}

/* User override */
[data-theme="dark"] {
  /* Dark mode colors */
}

[data-theme="light"] {
  /* Light mode colors */
}

[data-theme="sepia"] {
  /* Sepia mode colors */
}
```

### Automatic Detection

```javascript
// Detect system preference
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)');

// Listen for changes
prefersDark.addEventListener('change', (e) => {
  updateTheme(e.matches ? 'dark' : 'light');
});
```

---

## Storybook Integration

### File Structure

```
@paugeran/ui/
├── src/
│   ├── components/
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.module.css
│   │   │   └── Button.stories.tsx
│   │   └── Input/
│   │       ├── Input.tsx
│   │       ├── Input.module.css
│   │       └── Input.stories.tsx
│   └── tokens/
│       └── tokens.json
└── .storybook/
    ├── main.ts
    ├── preview.ts
    └── theme.ts
```

### Story Example

```tsx
// Button.stories.tsx
import { Meta, StoryObj } from '@storybook/solidjs';
import { Button } from './Button';

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  argTypes: {
    variant: { 
      options: ['primary', 'secondary', 'tertiary', 'danger'],
      control: { type: 'select' }
    },
    size: { 
      options: ['sm', 'md', 'lg'],
      control: { type: 'select' }
    },
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: {
    variant: 'primary',
    size: 'md',
    children: 'Click me',
  },
};

export const Disabled: Story = {
  args: {
    variant: 'primary',
    disabled: true,
    children: 'Disabled',
  },
};
```

---

## Brand Guidelines

### Logo Usage
- Logo with margin (1x logo size)
- Minimum size: 120px
- Color & white versions

### Typography
- Headings: Inter 700
- Body: Inter 400
- Code: Fira Code 500

### Color
- Primary: #0ea5e9
- Secondary: #8b5a3c (legal theme)
- Supporting colors in palette

---

## Documentation

### Component Documentation

Each component should have:

```markdown
# Button

Brief description

## Usage

```tsx
<Button variant="primary">Click me</Button>
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | string | 'primary' | Visual style |
| size | string | 'md' | Size variant |
| disabled | boolean | false | Disabled state |

## Accessibility

- Uses semantic button element
- Focus visible by default
- Supports aria-label

## Examples

### Primary Button
### Secondary Button
### Disabled State
### Loading State
```

---

## Checklist Implementasi

- [ ] Design tokens defined
- [ ] Component library 20+ components
- [ ] Icon library integrated
- [ ] Layout patterns documented
- [ ] Motion guidelines established
- [ ] Accessibility compliance verified
- [ ] Dark mode implemented
- [ ] Storybook setup complete
- [ ] Brand guidelines documented
- [ ] Component examples provided

---

## Referensi Tambahan

- [Storybook Documentation](https://storybook.js.org/)
- [SolidJS Design Patterns](https://docs.solidjs.com)
- [Figma File](https://figma.com/file/paugeran-design)
