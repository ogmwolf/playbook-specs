# design.md — Playbook Visual Design System

## Source of truth

`design-reference.html` in the project root contains the complete CSS design system. When styling any component, open that file first and extract from it. This document describes intent — the HTML file shows exactly what it should look like.

---

## Typography

| Use | Font | Weight | Size range |
|---|---|---|---|
| All UI / body | DM Sans | 400, 500, 600 | 11–20px |
| Labels, stats, metadata, timestamps | DM Mono | 400, 500 | 9–13px |

Import via next/font/google. Never use system fonts, Inter, Roboto, or Arial.

---

## Color system (CSS variables — never hardcode hex)

```css
--bg:        #FAFAF8   /* page background */
--bg2:       #F3F2EE   /* surfaces, stat cards, sidebar hover */
--bg3:       #ECEAE4   /* pressed states */
--card:      #FFFFFF   /* card background */
--border:    rgba(0,0,0,0.08)   /* default border */
--border2:   rgba(0,0,0,0.14)  /* emphasis border */
--text:      #1A1916   /* primary */
--text2:     #6B6960   /* secondary */
--text3:     #9E9C94   /* tertiary / muted */

--blue:      #2563EB   --blue-bg:   #EFF4FF   --blue-text: #1D4ED8
--amber:     #D97706   --amber-bg:  #FFFBEB   --amber-text:#92400E
--green:     #059669   --green-bg:  #ECFDF5   --green-text:#065F46
--red:       #DC2626   --red-bg:    #FEF2F2
--purple:    #7C3AED   --purple-bg: #F5F3FF   --purple-text:#4C1D95

--r:  10px   /* standard radius */
--rl: 14px   /* large radius (cards) */
--font: 'DM Sans', system-ui, sans-serif
--mono: 'DM Mono', monospace
```

---

## Athlete color system

Each athlete has a color assigned at creation. Used for avatar background and chips.

| Key | Background | Text |
|---|---|---|
| blue | #EFF4FF | #0C447C |
| amber | #FAEEDA | #633806 |
| green | #ECFDF5 | #065F46 |
| purple | #F5F3FF | #4C1D95 |
| coral | #FFF0EB | #9A3412 |
| teal | #ECFEFF | #155E75 |

CSS classes: `.av-blue`, `.av-amber`, `.av-green`, `.av-purple`, `.av-coral`, `.av-teal`

---

## Cards

```
background: --card
border: 0.5px solid --border
border-radius: --rl (14px)
overflow: hidden
```

Card header:
```
padding: 11px 14px 9px
border-bottom: 0.5px solid --border
card-title: 10px uppercase DM Mono --text3
card-action: 11px --blue, font-weight 500, cursor pointer
```

---

## Stat cards (4-up grid on Overview)

```
background: --bg2
border-radius: --r (10px)
padding: 12px 14px

label:  9px uppercase DM Mono --text3
value:  22px/500 letter-spacing -0.5px
delta:  10px — green (up), --text3 (flat), red (down)
```

---

## Status pills

| Status | Background | Text |
|---|---|---|
| priority | --purple-bg | #4C1D95 |
| interested | --green-bg | #065F46 |
| contacted | --amber-bg | #92400E |
| watching | --blue-bg | #1D4ED8 |

Font: 9px DM Mono, 2px 7px padding, 20px border-radius.

---

## Tab bar

```
container: --bg2 bg, 8px radius, 3px padding, 0.5px border
active tab: --card bg, --text color, 0.5px border, 6px radius
inactive: no bg, --text3 color
font: 11px/500 DM Sans
all tabs equal width (flex: 1)
```

---

## Progress bars

```
track: 4px height, --bg2, 2px radius
fill: --blue / --amber / --green depending on stat
```

---

## Measurables grid

3-column grid. Each cell:
```
padding: 10px 14px
border-right: 0.5px solid --border (removed on 3rd column)
border-bottom: 0.5px solid --border (removed on last row)
label: 9px uppercase DM Mono --text3
value: 16px/500 letter-spacing -0.3px
```

---

## Modals (bottom sheet)

```
backdrop: rgba(0,0,0,0.4)
sheet: --card, border-radius 20px 20px 0 0
handle: 36px × 4px, --border2, centered, margin-bottom 18px
animation: slide up 220ms
max-height: 88vh
```

---

## Empty states

```
padding: 24px 16px
text-align: center
color: --text3
font-size: 12px
action link: --blue, font-weight 500, display block, margin-top 6px
```

---

## Animations

- Tab/page transitions: fadeUp (opacity 0→1, translateY 10px→0, 180ms)
- Modal entrance: slideUp from bottom (220ms)
- Stat save confirm: show instantly, fade after 2s
- No heavy animations, no parallax, no scroll effects

---

## What not to do

- No gradients on UI elements
- No drop shadows (except subtle on active sub-tab)
- No emoji in UI
- No Inter, Roboto, Arial, or system fonts
- No hardcoded hex — always CSS variables
- No bottom nav (old design — top nav + sidebar is current)
- No seed data in components
