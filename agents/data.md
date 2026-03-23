# data.md — Playbook Data Model

## localStorage key

`playbook_v2`

All reads/writes through `lib/storage.ts`. No component calls localStorage directly.

---

## Full TypeScript interfaces

```typescript
// lib/types.ts

export interface AppState {
  family: string
  athletes: Athlete[]
  tasks: Task[]
  events: Event[]
  film: Film[]
  coaches: Coach[]
  docs: Doc[]
  notes: Note[]
}

export interface Athlete {
  id: string
  name: string
  initials: string
  position: string
  sport: string
  team: string
  color: AthleteColor
  schools: School[]
  stats: Stat[]
  measurables: Measurable[]
}

export type AthleteColor =
  'blue' | 'amber' | 'green' | 'purple' | 'coral' | 'teal'

export interface School {
  id: string
  name: string
  div: string
  status: RecruitingStatus
  notes: string
  nextAction: string
  lastContact: string
}

export type RecruitingStatus =
  'watching' | 'contacted' | 'interested' | 'priority'

export interface Stat {
  id: string
  name: string
  value: number | null
  previousValue: number | null
  unit: string
  min: number
  max: number
  updatedAt: string | null
  history: StatEntry[]
}

export interface StatEntry {
  value: number
  savedAt: string      // ISO timestamp — new Date().toISOString()
  context: string      // optional: "SoCal Cup 2026", "SCVA League", "Practice"
}

export interface Measurable {
  id: string
  label: string
  value: string        // stored as string: "6'0\"", "170 lbs", "28\""
}

export interface Task {
  id: string
  text: string
  athleteId: string    // '' = applies to all athletes
  urgency: 'now' | 'soon' | 'later'
  done: boolean
}

export interface Event {
  id: string
  name: string
  date: string         // YYYY-MM-DD
  location: string
  type: EventType
  athleteId: string    // '' = applies to all athletes
}

export type EventType = 'id' | 'club' | 'hs' | 'visit' | 'other'

export interface Film {
  id: string
  title: string
  url: string
  athleteId: string
  date: string         // human-readable: "Mar 8, 2026"
}

export interface Coach {
  id: string
  name: string
  school: string
  email: string
  athleteId: string
  status: RecruitingStatus
}

export interface Doc {
  id: string
  title: string
  url: string
  notes: string
}

export interface Note {
  id: string
  athleteId: string
  text: string
  source: NoteSource
  date: string         // human-readable: "Mar 2026"
}

export type NoteSource = 'club_coach' | 'hs_coach' | 'parent' | 'other'
```

---

## Storage abstraction

```typescript
// lib/storage.ts
const KEY = 'playbook_v2'

export function loadState(): AppState | null {
  try {
    const raw = localStorage.getItem(KEY)
    return raw ? JSON.parse(raw) : null
  } catch { return null }
}

export function saveState(state: AppState): void {
  try {
    localStorage.setItem(KEY, JSON.stringify(state))
  } catch {
    console.warn('Playbook: localStorage write failed')
  }
}

export function clearState(): void {
  localStorage.removeItem(KEY)
}
```

---

## Utility functions

```typescript
// lib/utils.ts

export function uid(): string {
  return Math.random().toString(36).slice(2, 9)
}

export function fmtDate(dateStr: string): { mo: string, dy: number } {
  // Always parse with T12:00:00 to avoid timezone offset issues
  const d = new Date(dateStr + 'T12:00:00')
  const MONTHS = ['Jan','Feb','Mar','Apr','May','Jun',
                  'Jul','Aug','Sep','Oct','Nov','Dec']
  return { mo: MONTHS[d.getMonth()], dy: d.getDate() }
}

export function filtered<T extends { athleteId: string }>(
  arr: T[],
  ctx: string   // '' = show all, athleteId = filter to that athlete
): T[] {
  if (!ctx) return arr
  return arr.filter(item => !item.athleteId || item.athleteId === ctx)
}

export function initials(name: string): string {
  const parts = name.trim().split(' ')
  return (parts[0][0] + (parts[1] ? parts[1][0] : '')).toUpperCase()
}

export function getDefaultStats(position: string): Stat[] {
  const pos = position.toLowerCase()
  const make = (name: string, min: number, max: number, unit: string): Stat => ({
    id: uid(), name, value: null, previousValue: null,
    unit, min, max, updatedAt: null, history: []
  })

  if (pos.includes('libero')) return [
    make('Pass average', 0, 3.0, '/ 3.0'),
    make('Serve receive %', 0, 100, '%'),
    make('Dig avg / set', 0, 8.0, '/ set'),
    make('Serve aces / set', 0, 3.0, '/ set'),
  ]

  if (pos.includes('setter')) return [
    make('Set efficiency %', 0, 100, '%'),
    make('Assists / set', 0, 15, '/ set'),
    make('Opp attack %', 0, 60, '%'),
    make('Serve aces / set', 0, 3.0, '/ set'),
  ]

  if (pos.includes('opp') || pos.includes('outside') || pos.includes('middle')) return [
    make('Attack %', 0, 60, '%'),
    make('Kills / set', 0, 8.0, '/ set'),
    make('Hitting efficiency', -1.0, 1.0, ''),
    make('Blocks / set', 0, 3.0, '/ set'),
  ]

  // Generic fallback
  return [
    make('Kills / set', 0, 8.0, '/ set'),
    make('Attack %', 0, 60, '%'),
    make('Aces / set', 0, 3.0, '/ set'),
  ]
}

export function getDefaultMeasurables(): Measurable[] {
  return ['Height', 'Weight', 'Vertical', 'Block reach', 'Wingspan']
    .map(label => ({ id: uid(), label, value: '' }))
}
```

---

## Save stat flow

When parent hits "Save update" on Stats tab:

```typescript
// Push new history entry
stat.history.unshift({
  value: newValue,
  savedAt: new Date().toISOString(),
  context: contextInput.trim()
})

// Update current and previous
stat.previousValue = stat.value
stat.value = newValue
stat.updatedAt = new Date().toISOString()

// Trim history to last 20 entries
if (stat.history.length > 20) stat.history = stat.history.slice(0, 20)

// Save to localStorage
saveState(state)
```

---

## Delta display logic (Overview stat cards)

```typescript
function getDelta(stat: Stat): { arrow: '↑'|'↓'|null, text: string, color: 'green'|'red'|'gray'|'muted' } {
  if (stat.value === null) return { arrow: null, text: 'No data yet', color: 'muted' }
  if (stat.previousValue === null) return { arrow: null, text: stat.updatedAt ? `Updated ${fmtShortDate(stat.updatedAt)}` : '', color: 'gray' }
  if (stat.value > stat.previousValue) return { arrow: '↑', text: `from ${stat.previousValue}`, color: 'green' }
  if (stat.value < stat.previousValue) return { arrow: '↓', text: `from ${stat.previousValue}`, color: 'red' }
  return { arrow: null, text: 'no change', color: 'gray' }
}
```

---

## Tournament data

`lib/tournaments.json` — static array of SoCal boys volleyball tournaments 2025–26.

Each entry:
```typescript
interface Tournament {
  id: string
  name: string
  startDate: string    // YYYY-MM-DD
  endDate: string
  venue: string
  location: string
  org: string
  type: string
  divisions: string[]
  gender: string
  notes: string
  url: string | null
}
```

Used for:
1. Calendar add-event: typeahead/suggestions when typing event name
2. Stats context input: typeahead when saving a stat update

---

## Supabase migration path (v2)

Tables map 1:1 to AppState:
`families`, `athletes`, `schools`, `stats`, `stat_history`, `measurables`,
`tasks`, `events`, `film`, `coaches`, `docs`, `notes`

All tables have `family_id` foreign key + RLS policies.
`stat_history` is the normalized version of `Stat.history[]`.
Migration: on first v2 login, offer to import localStorage data.
