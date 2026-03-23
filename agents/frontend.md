# frontend.md — Playbook Layout & UI Behavior

## App shell

```
TopNav (48px, sticky, z-index 100)
────────────────────────────────────
Sidebar (200px) | Main content (flex:1, overflow-y:auto, padding 22px 24px)
```

Mobile (<768px): sidebar hidden, athlete chip row shown below TopNav.

---

## TopNav

- Height: 48px, `--card` bg, border-bottom 0.5px `--border`
- Left: "Playbook" wordmark — 15px/600, "book" in `--blue`
- Center: nav links — Athletes · This Week · Calendar · Vault
  - 12px/500, `--text3` inactive, `--bg2` + `--text` active
  - 6px padding, 6px border-radius, 2px gap
- Right: "Family Name · Year" — 11px DM Mono `--text3`

---

## Sidebar

Width: 200px. `--card` bg. Border-right 0.5px `--border`. Hidden on mobile.

### Athletes section

- Section label: 9px uppercase DM Mono `--text3`
- Athlete item: flex row, 7px 8px padding, 7px radius
  - 26px avatar circle (colored initials, athlete color class)
  - Name: 12px/500
  - Role: 10px `--text3`
  - Active state: `--bg2` bg + 5px `--blue` dot on right
- Add athlete: dashed border add-row at bottom

### Open to-dos section

- 4 most urgent open tasks
- Each: 5px urgency dot (red=now, amber=soon, gray=later) + 11px `--text2` text
- Truncate to fit

### Next event

- `--bg2` rounded card (8px radius), margin 0 12px 14px
- Eyebrow: "MMM D · N days" — 9px DM Mono `--text3` uppercase
- Name: 12px/500
- Sub: 10px `--text3`

---

## Athletes tab — Athlete header

Above the 5-tab bar:
- 44px avatar circle (athlete color, large)
- Name: 20px/600, letter-spacing -0.4px
- Subtitle: position · height · weight · class (11px DM Mono `--text3`)
- Tags: sport, team(s), grade — 9px pill tags

---

## Athletes tab — 5 tab bar

See design.md for tab bar styling. 5 equal tabs: Overview · Stats · Schools · Film · Notes.

---

## Overview tab

### Stat cards (4-column grid, full width)

- 4 cards: background `--bg2`, 10px radius
- Each: label (9px uppercase mono), value (22px/500), delta (10px)
- Delta logic: null value → "No data"; null prev → value only; up → green ↑; down → red ↓; same → gray

Empty state: "No stats yet — add them in the Stats tab →" (switches to Stats tab)

### 2-column layout below stat cards

Left column:
1. Season progress card — bar chart per stat
2. Measurables card — 3-column grid

Right column:
1. Top schools card — 3 schools max with next action + status pill
2. Latest note card — most recent note

All card actions hot-swap tabs:
- "Edit stats →" → Stats tab
- "Edit →" on measurables → Stats tab
- "View all →" on schools → Schools tab
- "All notes →" → Notes tab

Empty states per section (when no data):
- No stats: message + "Add in Stats tab →"
- No schools: message + "Add school →" opens AddSchoolModal
- No notes: message + "Add note →" opens AddNoteModal
- No measurables: message + "Add in Stats tab →"

---

## Stats tab

### Performance stats section

Header: "Performance stats — drag to update" (9px uppercase `--text3` on `--bg2`)
Intro: "Each update saves a timestamp. Arrows on Overview show movement from your last entry."

Each stat entry row:
- Top: stat name (13px/500) + current value (18px/500 DM Mono) + delta pill
- Slider: full width, accent-color `--blue`, min/max labels in 9px DM Mono
- "Last updated: [date] · previous: [value]" — 10px DM Mono `--text3`
- Context input (optional): "Where? e.g. SoCal Cup 2026" — typeaheads from tournaments.json
- Save button: blue, 11px, "Save update"
- After save: "✓ Saved" green pill, disappears after 2s

Stat history table (below each entry):
- Columns: Date · Value · Change · Context
- Most recent first, max 5 rows
- 11px DM Mono `--text3`
- No borders — subtle separation only
- Omit Change column if only 1 entry

### Measurables section

Header: "Measurables — tap to edit"
3-column grid. Each value is contenteditable="true" — tap to edit inline.
"Add stat field" add-row at bottom.

---

## Schools tab

Each school row:
- Name (13px/500) + division (10px DM Mono `--text3`)
- Notes (11px `--text2`, line-height 1.4)
- Next action (10px `--blue` "→ ...")
- Status pill (right) + last contact (9px DM Mono `--text3` below pill)

---

## Film tab

Each film row:
- 26px icon square (`--bg2`, play symbol)
- Title (12px/500) + date (10px DM Mono `--text3`)
- "View ↗" in `--blue`

---

## Notes tab

Chronological, most recent first:
- Source + date: 9px DM Mono `--text3`
- Note text: 12px `--text2`, line-height 1.55

Source labels: "Club coach" / "HS coach" / "Parent note" / "Other"

---

## This Week tab

- "This week" heading (18px/500)
- Next event hero: `--blue` bg, month/day, name, location + days away
- "Do this now": urgency:now tasks
- "Coming up": urgency:soon + urgency:later
- "Completed": done tasks at 45% opacity
- Task item: urgency dot + text + athlete chip + checkbox (tap to toggle)

---

## Calendar tab

- Chronological list, past events 45% opacity
- Each row: date block (month + day) + name + location + type tag
- Add event: modal with tournament typeahead from tournaments.json

Type tags: ID Camp (purple), Club (blue), HS (amber), Visit (green), Other (gray)

---

## Vault tab

3 sub-tabs: Film · Coaches · Docs

Coach rows: initials avatar + name/school/email + athlete chip + status pill
Doc rows: icon + title + notes + "View ↗"

---

## FTUE

Full-screen, z-index 300. Progress bar (3px, `--blue`, 4 steps).

Step 0 — Welcome: logo, headline, 3 value props, CTA
Step 1 — Family name: text input, Continue disabled until value entered
Step 2 — Add athletes: inline form (name, position, sport, team, color picker). Continue disabled until ≥1 athlete. Default stats + measurables injected on save.
Step 3 — First to-do: text + athlete + urgency. Skippable.
Step 4 — All set: checkmark, personalized title, summary

Color picker: 6 swatches. Selected: dark border + scale(1.18).

---

## Mobile athlete picker (replaces sidebar on mobile)

Horizontal chip row below TopNav:
- Each chip: athlete avatar + first name
- Active: `--blue-bg` bg, `--blue` border, `--blue-text` color
- Scrollable if many athletes
