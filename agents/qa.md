# qa.md — Playbook QA & Testing

## Testing philosophy

User 1 is a real non-technical person on her phone. Her feedback is the most valuable QA. Watch where she hesitates. Don't explain anything in advance — let her go through it cold.

---

## First-use checklist (for developer's wife)

### FTUE

- [ ] Welcome screen loads correctly on iPhone Safari
- [ ] "Get started" tappable, keyboard comes up correctly on name step
- [ ] Continue disabled until family name entered
- [ ] Athlete form opens automatically on step 2
- [ ] Can add Jake (name, position, sport, team, color)
- [ ] Can add Wyatt without confusion
- [ ] Color picker works, selected color shows with border
- [ ] Continue disabled until at least one athlete added
- [ ] First to-do step: understands what it's asking
- [ ] Skip works
- [ ] All set screen shows correct summary
- [ ] "Open my Playbook" lands on Athletes tab with data

### Athletes tab

- [ ] Jake and Wyatt visible in sidebar
- [ ] Click Jake → his dashboard loads
- [ ] Stats tab shows default libero stats (sliders ready, no values yet)
- [ ] Drag a slider → value updates live
- [ ] Hit Save → "✓ Saved" appears then fades
- [ ] Stat appears on Overview with value (no arrow on first entry)
- [ ] Save again → Overview shows delta arrow
- [ ] History table shows entries with date + context
- [ ] Measurables: tap a value → can edit inline
- [ ] Schools tab: Add school → school appears with status pill
- [ ] Film tab: Add film link → appears in list
- [ ] Notes tab: Add note → appears with source badge
- [ ] "Edit stats →" on Overview → switches to Stats tab
- [ ] "View all →" on schools → switches to Schools tab
- [ ] "All notes →" → switches to Notes tab

### This Week tab

- [ ] Tasks from FTUE visible
- [ ] Urgency sorting: now → soon → later
- [ ] Check off a task → moves to completed section
- [ ] Uncheck → moves back
- [ ] Add to-do → appears in correct section
- [ ] Next event hero shows if events exist

### Calendar tab

- [ ] Empty state correct
- [ ] Add event → appears in chronological order
- [ ] Tournament typeahead works (type "SoCal" → suggests tournament)
- [ ] Past events at 45% opacity

### Vault tab

- [ ] Film / Coaches / Docs sub-tabs work
- [ ] Add coach → appears with athlete chip
- [ ] Add doc → appears in list

### Persistence

- [ ] Close browser tab completely
- [ ] Reopen URL → all data intact, FTUE not shown
- [ ] Returning visit lands on Athletes tab, not FTUE

### Context switcher (sidebar)

- [ ] Click Wyatt → his dashboard loads
- [ ] Click Jake → switches back
- [ ] Active dot shows on selected athlete

---

## Known limitations in v1 (not bugs)

- No edit/delete for tasks, events, film, coaches, docs (add only)
- No multi-device sync (localStorage only)
- No notifications
- No coach-facing tools
- Data lost if browser data cleared

---

## Regression checklist (run before every deploy)

- [ ] FTUE completes end-to-end on mobile Safari
- [ ] FTUE completes on desktop Chrome
- [ ] Returning visit skips FTUE, loads existing data
- [ ] All 4 top nav tabs navigate correctly
- [ ] All 5 athlete tabs work for each athlete
- [ ] Hot-swap navigation works (Edit stats →, View all →, All notes →)
- [ ] Stats: slider updates value live, Save writes history, Overview delta updates
- [ ] Default stats injected by position on athlete creation
- [ ] Default measurables injected on athlete creation
- [ ] Tournament typeahead works on calendar and stat context
- [ ] Context switching between athletes works
- [ ] localStorage key is `playbook_v2`
- [ ] App does not crash with 0 athletes, 0 tasks, 0 events
- [ ] Empty states show with correct action links

---

## Observation prompts (ask wife after testing)

1. Was there anything you couldn't figure out on your own?
2. What's the first thing you wanted that wasn't there?
3. What would make you open this every day?
4. What felt slow or annoying?
5. What would you tell another volleyball mom this does?

---

## Bug report format

1. What tab / screen were you on?
2. What did you tap or type?
3. What happened?
4. What did you expect?
5. Screenshot if possible
