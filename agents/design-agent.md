# design-agent.md — Playbook Design System

Read this file before writing ANY CSS, JSX styling, or component layout.
This is the visual law. Do not deviate from it.

---

## Design philosophy

Playbook is a private family tool. It should feel inevitable — like it could not have been designed any other way.

Study how Apple designs tools for people who care deeply about something. The Health app. Notes. Reminders. They don't show off. They get out of the way. The content — the athlete's data, the parent's tasks — is the hero. The UI is the frame.

**Four principles that govern every decision:**

**1. Deference.** The interface exists to serve the content, not to be admired. Every element that doesn't directly help the parent understand their athlete's situation should be removed or made smaller. When in doubt, cut it.

**2. Clarity.** A busy mom opening this app at 6am before practice should understand what she's looking at in under three seconds. No decoding required. Labels are clear. Hierarchy is obvious. Actions are unmistakable. If you have to explain a UI element, redesign it.

**3. Depth through restraint.** Visual hierarchy comes from size, weight, and color contrast — not shadows, gradients, or decoration. The warm off-white background (#FAFAF8) against white cards (#FFFFFF) creates depth without effort. A 0.5px border is all the separation needed. Trust the whitespace.

**4. Intention in motion.** Every transition should feel like it was always going to happen that way. Modals slide up because they're coming from below. Tab switches fade because content is replacing content. Nothing bounces, nothing spins, nothing draws attention to itself.

The one feeling every screen should produce: "I know exactly what to do next."

The one question to ask before shipping any screen: "Would a tired parent at 10pm on her phone find this calming or confusing?"

---

## The non-negotiables

1. **Never hardcode hex colors.** Always use CSS variables. No exceptions.
2. **Never use Tailwind utility classes.** All styling through CSS classes in globals.css.
3. **Never use inline styles on non-SVG elements.** CSS variables passed as custom properties (e.g. `--fill-pct`) are the only exception.
4. **Never use gradients on UI elements.** Flat color only.
5. **Never add drop shadows** except a subtle one on active sub-tabs.
6. **Never use emoji in UI.** Use text symbols or SVG.
7. **Never use Inter, Roboto, Arial, or system fonts.** DM Sans and DM Mono only.
8. **Always add CSS classes to globals.css before using them in JSX.** If a class doesn't exist in globals.css, the component will render unstyled. This is the most common bug — fix it by writing the CSS first.

---

## Fonts

| Use | Font | Weights | Size range |
|---|---|---|---|
| All UI, body, labels | DM Sans | 400, 500, 600 | 10–20px |
| Stats, metadata, timestamps, monospace labels | DM Mono | 400, 500 | 9–13px |

```css
font-family: var(--font);   /* DM Sans */
font-family: var(--mono);   /* DM Mono */
```

Never mix fonts within a single element. Never use font-size below 9px.

---

## Color System

### App Background & Surface
--bg: #FAFAF8 (warm off-white page background)
--bg2: #F3F2EE (subtle surface, used for inactive pills, hover states)
--bg3: #ECEAE4 (stronger surface)
--card: #FFFFFF (card background)
--border: #00000020 (card and divider borders)
--border2: rgba(0,0,0,0.14)

### Typography
--text: #1A1916 (primary text, warm near-black)
--text2: #6B6960 (secondary/muted text)
--text3: #9E9C94 (tertiary/hint text, labels, metadata)

### Semantic Colors
--blue: #2563EB | --blue-bg: #EFF4FF | --blue-text: #1D4ED8
--green: #059669 | --green-bg: #ECFDF5 | --green-text: #065F46
--amber: #D97706 | --amber-bg: #FFFBEB | --amber-text: #92400E
--red: #DC2626 | --red-bg: #FEF2F2
--purple: #7C3AED | --purple-bg: #F5F3FF | --purple-text: #4C1D95

### Athlete Colors (muted, sophisticated — NOT primary colors)
.av-red: #C17B6A (terracotta)
.av-blue: #5B8DB8 (slate blue)
.av-green: #5A9E7A (sage green)
.av-amber: #C4965A (warm amber)
.av-indigo: #7B6FA0 (dusty purple)
.av-violet: #A07B8A (mauve)

### Status Pill Colors (colleges/recruiting)
watching: background var(--bg2), color var(--text2)
contacted: background var(--blue-bg), color var(--blue)
interested: background var(--green-bg), color var(--green)
priority: background var(--amber-bg), color var(--amber-text)

### Typography
--font: 'DM Sans' (all UI text)
--mono: 'DM Mono' (labels, stats, metadata, mono elements)
--r: 10px (standard border radius)
--rl: 14px (large border radius)

### Design Principles (from Jony Ive / Apple)
- Warm, muted palette — never primary or saturated colors
- CSS variables only — never hardcode hex values
- No inline styles on non-SVG elements
- All classes defined in globals.css before use
- Athlete colors are identity markers, not UI states
- Status colors use semantic bg/text variable pairs

---

## Athlete color system

Auto-assigned in order. Never let users pick colors.

| Slot | Key | Background | Text |
|---|---|---|---|
| 1st | red | #FEE2E2 | #991B1B |
| 2nd | blue | #EFF4FF | #0C447C |
| 3rd | green | #ECFDF5 | #065F46 |
| 4th | amber | #FAEEDA | #633806 |
| 5th | indigo | #EEF2FF | #3730A3 |
| 6th | violet | #F5F3FF | #4C1D95 |

CSS classes: `.av-red`, `.av-blue`, `.av-green`, `.av-amber`, `.av-indigo`, `.av-violet`

---

## Spacing system

Use these values consistently. Never invent arbitrary spacing.

```
4px   — tight gap between inline elements
6px   — gap between list items
8px   — small internal padding
12px  — standard gap
14px  — card internal padding (compact)
16px  — card internal padding (standard)
20px  — card internal padding (spacious)
24px  — page content padding
```

---

## Cards

The fundamental UI unit. Everything is a card.

```css
.card {
  background: var(--card);
  border: 0.5px solid var(--border);
  border-radius: var(--rl);  /* 14px */
  overflow: hidden;
}
```

Card header:
```css
.card-header {
  padding: 11px 14px 9px;
  border-bottom: 0.5px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.card-title {
  font-size: 9px;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--text3);
  font-family: var(--mono);
}
.card-action {
  font-size: 11px;
  color: var(--blue);
  font-weight: 500;
  cursor: pointer;
  background: none;
  border: none;
  padding: 0;
}
```

---

## Typography rules

**Headings:**
- Page title: 18px/500, letter-spacing -0.3px, --text
- Section heading: 13px/600, --text
- Card title: 9px uppercase DM Mono, --text3

**Body:**
- Primary text: 12–13px/400, --text2, line-height 1.5
- Secondary text: 11px/400, --text2
- Muted text: 10–11px/400, --text3

**Metadata/labels:**
- All DM Mono
- 9–11px
- --text3
- uppercase + letter-spacing: 0.05–0.06em for section labels

**Never use bold (700) weight** except for large display numbers (stats, countdown rings).

---

## Stat cards

```css
.stat-card {
  background: var(--bg2);
  border-radius: var(--r);
  padding: 12px 14px;
}
.stat-label {
  font-size: 9px;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  font-family: var(--mono);
  color: var(--text3);
  margin-bottom: 4px;
}
.stat-value {
  font-size: 22px;
  font-weight: 500;
  letter-spacing: -0.5px;
  color: var(--text);
}
.stat-delta {
  font-size: 10px;
  margin-top: 2px;
}
.delta-up   { color: var(--green); }
.delta-down { color: var(--red); }
.delta-flat { color: var(--text3); }
```

---

## Status pills

```css
/* Base */
.pill {
  font-size: 9px;
  font-family: var(--mono);
  padding: 2px 7px;
  border-radius: 20px;
  white-space: nowrap;
}

/* Variants */
.pill-priority   { background: var(--purple-bg); color: var(--purple-text); }
.pill-interested { background: var(--green-bg);  color: var(--green-text); }
.pill-contacted  { background: var(--amber-bg);  color: var(--amber-text); }
.pill-watching   { background: var(--blue-bg);   color: var(--blue-text); }
```

Status display labels (athlete perspective):
- watching → "On my radar"
- contacted → "I reached out"
- interested → "They're interested"
- priority → "Top choice"

---

## Tab bar

```css
.tab-bar {
  display: flex;
  background: var(--bg2);
  border-radius: 8px;
  padding: 3px;
  border: 0.5px solid var(--border);
  gap: 2px;
}
.tab-btn {
  flex: 1;
  font-size: 11px;
  font-weight: 500;
  font-family: var(--font);
  color: var(--text3);
  background: none;
  border: none;
  border-radius: 6px;
  padding: 5px 8px;
  cursor: pointer;
  white-space: nowrap;
}
.tab-btn.active {
  background: var(--card);
  color: var(--text);
  border: 0.5px solid var(--border);
  box-shadow: 0 1px 2px rgba(0,0,0,0.06);
}
```

---

## Modals

Bottom sheet pattern. Never centered dialogs.

```css
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  z-index: 200;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}
.modal {
  background: var(--card);
  border-radius: 20px 20px 0 0;
  width: 100%;
  max-width: 560px;
  max-height: 88vh;
  overflow-y: auto;
  padding: 0 20px 24px;
  animation: slideUp 220ms ease;
}
.modal-handle {
  width: 36px;
  height: 4px;
  background: var(--border2);
  border-radius: 2px;
  margin: 12px auto 18px;
}
```

---

## Form inputs

```css
.ftue-input, input[type="text"], input[type="email"], select, textarea {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid var(--border2);
  border-radius: var(--r);
  background: var(--bg);
  font-size: 13px;
  font-family: var(--font);
  color: var(--text);
  outline: none;
  transition: border-color 0.15s;
}
input:focus, select:focus, textarea:focus {
  border-color: var(--blue);
}
```

---

## Buttons

```css
/* Primary */
.btn-primary {
  background: var(--blue);
  color: white;
  font-size: 13px;
  font-weight: 500;
  font-family: var(--font);
  border: none;
  border-radius: var(--r);
  padding: 10px 20px;
  cursor: pointer;
  width: 100%;
}
.btn-primary:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

/* Ghost / cancel */
.btn-cancel {
  background: none;
  color: var(--text3);
  font-size: 13px;
  font-family: var(--font);
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}

/* Inline action link */
.btn-link {
  background: none;
  border: none;
  color: var(--blue);
  font-size: 11px;
  font-weight: 500;
  font-family: var(--font);
  cursor: pointer;
  padding: 0;
  white-space: nowrap;
}
```

---

## Empty states

```css
.empty-state {
  padding: 24px 16px;
  text-align: center;
  color: var(--text3);
  font-size: 12px;
  line-height: 1.5;
}
.empty-action {
  display: block;
  margin-top: 8px;
  color: var(--blue);
  font-weight: 500;
  font-size: 11px;
  background: none;
  border: none;
  cursor: pointer;
}
```

---

## Animations

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes slideUp {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}
.anim-fade-up  { animation: fadeUp 180ms ease; }
.anim-slide-up { animation: slideUp 220ms ease; }
```

---

## Responsive

```css
/* Sidebar hidden on mobile */
@media (max-width: 768px) {
  .sidebar { display: none; }
  .mobile-athlete-picker { display: flex; }
  .main { padding: 16px; }
}
```

---

## Layout rules

- Page content max-width: none — fills available space
- Content padding: 22px 24px on desktop, 16px on mobile
- Sidebar: 200px fixed width
- Card grid: use CSS grid, never flexbox for multi-column card layouts
- Always use `gap` for spacing between grid/flex children — never margin on individual items

---

## What Apple would never do

Study this list. These are the things that make UI look cheap:

- Multiple font sizes on the same line without clear hierarchy
- Text that touches the edge of its container
- Buttons that are too small to tap comfortably (min 44px height on mobile)
- Inconsistent corner radii (pick --r or --rl and stick to it)
- Labels that are the same visual weight as values
- Actions that look like labels (blue color = interactive, always)
- Empty states that say nothing useful
- Loading states that are jarring
- Hover states that change layout (color/opacity only)
- Borders that are too thick (0.5px is the Playbook standard)
- Text that wraps unexpectedly in tight spaces (use white-space: nowrap + overflow: hidden + text-overflow: ellipsis)

---

## Visual judgment — ask these before shipping any screen

These are the questions a great designer asks. Ask them in order.

**1. Is there anything I can remove?**
Every element should earn its place. Labels, dividers, icons, helper text — if removing it doesn't break understanding, remove it. The best designs have nothing left to take away.

**2. Is the hierarchy immediately obvious?**
The most important thing on the screen should be visually dominant. The second most important thing should be clearly secondary. Everything else should recede. If everything has the same visual weight, nothing has priority.

**3. Is the whitespace doing work?**
Whitespace is not empty space — it's breathing room that creates grouping and separation. Related things should be close. Unrelated things should be far apart. A card with too little padding feels cramped. A screen with no breathing room feels overwhelming.

**4. Are all interactive elements obvious?**
Blue means interactive in Playbook. Always. If something is tappable, it should be blue or have an explicit visual affordance. A parent should never wonder "can I tap this?"

**5. Does it work at 11pm on an iPhone?**
Tap targets must be at least 44px tall. Text must be readable without zooming. Nothing critical should be hidden below the fold on a phone screen. Test every screen at 375px width.

**6. Does every screen have one primary action?**
A screen with three equally prominent buttons has no primary action. One thing should be clearly the most important next step. Everything else is secondary. If you can't identify the primary action, the screen needs to be redesigned.

**7. Is the empty state better than nothing?**
Empty states are the first thing new users see. They should be inviting, not clinical. "No colleges added yet — Add one →" is better than "No data." Show the potential of what the screen becomes when it's full.

---

## The CSS-first rule

**Before writing any JSX:**
1. List every CSS class the component needs
2. Check globals.css — does each class exist?
3. If missing — add it to globals.css first
4. Only then write the JSX using those classes

This is the single most important rule. Every unstyled component in Playbook's history was caused by violating this rule.
