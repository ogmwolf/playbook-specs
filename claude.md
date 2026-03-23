# CLAUDE.md — Playbook Session Context

Read this file at the start of EVERY session before touching any code.
Then read agents/design-agent.md before touching any CSS or JSX.

---

## ⚠️ MANDATORY DESIGN RULE

**Every component MUST use CSS classes from globals.css.**
**Never use inline styles on non-SVG elements.**
**Never use Tailwind utility classes.**
**Never hardcode hex colors — always CSS variables.**

CSS-first rule: Add ALL classes to globals.css BEFORE writing any JSX.
If a component renders as unstyled text, classes are missing from globals.css.

---

## What is Playbook

Private family command center for parents managing youth athletes through college recruiting. Not a social network. A personal operating system for one family.

First user: developer's wife managing two boys — Jake Wolf (Libero, class of 2029) and Wyatt Wolf (Setter/Oppo, class of 2029) — both playing club volleyball (Rockstar VBC) and high school volleyball (Mira Costa HS).

---

## Stack

- Next.js 16, App Router, TypeScript, Tailwind CSS
- Vercel deployment
- localStorage key: `playbook_v2`
- Supabase + Clerk planned for v2

---

## Core principles

1. Mobile-first. Every layout works on a phone.
2. Task-first. Home is the daily command center.
3. Private. No social, no sharing, no public profiles.
4. Simple. Non-technical parent understands every screen in 10 seconds.
5. Multi-athlete. Athletes are a dynamic list, not hardcoded.
6. Multi-sport. Only Volleyball active in v1.

---

## Navigation (4 top nav tabs)

**Home · Calendar · Stats · Vault**

Default landing: Home

**Sidebar:**
- Athletes list with colored avatar (auto-assigned ROYGBIV colors)
- Progress bar under active athlete (% complete)
- Open tasks listed under each athlete (max 3, with colored dot)
- Next event card at bottom
- No sub-tabs under athletes — removed as redundant

---

## Home screen layout (WeekView.tsx)

3-column layout:
- LEFT: TASKS widget (shared across all athletes)
- CENTER: Athlete cards stacked vertically (one per athlete)
- RIGHT: NOTES widget (shared across all athletes)

**Athlete card content:**
- Name (15px/600)
- Team · High School (10px mono --text3)
- Position · Sport · Grade (10px mono --text3)
- Performance stats line (up to 3 stats joined with ·)
- Physical stats line (Height · Weight · Vertical)
- Rows: NEXT EVENT, WINDOW, RECRUITING (colleges count · film count)

**TASKS widget (shared, left column):**
- All tasks across all athletes
- Each row: colored dot (athlete color) + athlete name (9px mono) + task text + circle checkbox
- Gray dot for tasks with no specific athlete
- Tap checkbox to complete — fades and disappears after 1 second
- "+ Add to-do" at bottom

**NOTES widget (shared, right column):**
- All notes across all athletes, most recent first
- Each row: colored dot + "Athlete · Source:" prefix (9px mono --text3) + note text (12px --text2)
- "+ Add note" at bottom

---

## Athlete colors (auto-assigned, no picker)

1st: #E63946 (red) → .av-red
2nd: #2563EB (blue) → .av-blue
3rd: #16A34A (green) → .av-green
4th: #D97706 (amber) → .av-amber
5th: #4F46E5 (indigo) → .av-indigo
6th: #7C3AED (violet) → .av-violet

Colored dots in tasks/notes match athlete color.

---

## FTUE flow (3 steps)

Step 0: Welcome — headline "Youth sports command center", 3 value props (icon LEFT of text, horizontal layout)
Step 1: Family name
Step 2: Add athletes — sport carousel (3 cards, Volleyball auto-selected center, wraps infinitely), position dropdown, grad year (2026-2032), gender (Boy/Girl toggle, required), team, high school, first name + last name fields. Colors auto-assigned.
Step 3: All set — shows "Your first steps" checklist

No color picker. No first to-do step. No first note step.

---

## Volleyball positions (dropdown only, no free text)

Libero, Setter, Setter/Oppo, Opposite, Outside Hitter, Middle Blocker, Defensive Specialist

---

## Grade calculation

graduationYear - currentYear:
1 = Senior, 2 = Junior, 3 = Sophomore, 4 = Freshman, 5 = 8th grade, 6+ = Middle school

---

## Data model (current — matches lib/types.ts)

AppState: family, athletes[], tasks[], events[], film[], coaches[], docs[], notes[]

Athlete: id, name, initials, position, sport, team, highSchool, color, gender: 'male'|'female'|'', graduationYear?: number, schools[], stats[], measurables[]

Stat: id, name, value: number|null, previousValue: number|null, unit, min, max, updatedAt, seeded: boolean, history: StatEntry[]
StatEntry: value, savedAt (ISO), context

Measurable: id, label, value: string, history: Array<{value, savedAt}>

Task: id, text, athleteId ('' = all), urgency: 'soon' (default, not shown to user), done: boolean

Note: id, athleteId ('' = all), text, source: 'club_coach'|'hs_coach'|'parent'|'other', date

School status labels (athlete perspective):
- watching → "On my radar"
- contacted → "I reached out"
- interested → "They're interested"
- priority → "Top choice"

---

## Default stats by position (seeded: true, value = midpoint)

Libero: Pass average (0–3.0), Serve receive % (0–100), Dig avg/set (0–8.0), Serve aces/set (0–3.0)
Setter: Set efficiency % (0–100), Assists/set (0–15), Opp attack % (0–60), Serve aces/set (0–3.0)
Setter/Oppo + Opposite + Outside + Middle: Attack % (0–60), Kills/set (0–8.0), Hitting efficiency (-1.0–1.0), Blocks/set (0–3.0)

Block reach NOT included for Libero or Defensive Specialist.

---

## Default measurables

Male: Height "6'0\"", Weight "170 lbs", Vertical "30\"", Block reach "7'5\"", Wingspan "6'2\""
Female: Height "5'8\"", Weight "140 lbs", Vertical "24\"", Block reach "7'0\"", Wingspan "5'10\""
Libero/DS: omit Block reach

Slider ranges:
- Height: 54–102 inches → display feet/inches
- Weight: 80–400 lbs
- Vertical: 0–55 inches
- Block reach: 66–144 inches → display feet/inches
- Wingspan: 54–108 inches → display feet/inches

---

## Tournament data (lib/tournaments.json)

12 verified tournaments. Gender field confirmed against official sources:
- Boys: AIM league, SoCal Cup Equinox, USAV Boys West Coast Qualifier, Big West ID Camp, SoCal Cup Showcase, USAV Boys Jr Nationals Wave 1+2
- Girls: SCVA Las Vegas Classic, Red Rock Rave #1, Red Rock Rave #2, SCVA Summer Soiree

Gender filter: male athletes see boys tournaments, female athletes see girls tournaments.

---

## Coding conventions

- All components: "use client"
- uid() = Math.random().toString(36).slice(2,9)
- Never call localStorage directly — use lib/storage.ts
- DM Sans for UI, DM Mono for labels/stats/metadata
- CSS variables only — NEVER hardcode hex
- Design source of truth: ~/playbook-specs/agents/design-agent.md

---

## Spec files location

~/playbook-specs/
├── claude.md               ← this file
├── README.md
├── design-reference.html   ← visual source of truth
├── agents/
│   ├── design-agent.md     ← Apple design principles — read before any CSS
│   ├── design.md
│   ├── frontend.md
│   ├── data.md
│   ├── product.md
│   └── qa.md
├── data/
│   ├── tournaments.json    ← 12 verified tournaments, correct gender tags
│   ├── roster-needs.json   ← womens DI programs (girls only)
│   ├── ncaa-calendar.json  ← recruiting periods and milestones
│   └── di-programs.json    ← DI programs with questionnaire URLs
├── product/
│   ├── roadmap.md
│   └── vision.md
└── recruiting/
    └── ncaa-rules.md

---

## Do not touch without asking

- localStorage key (playbook_v2)
- Data model field names (breaks existing data)
- Default stat field names
- lib/tournaments.json gender fields (verified against official sources)
- FTUE step sequence
