# roadmap.md — Playbook Product Roadmap

Last updated: March 2026
Input: Developer vision + User 1 feedback (day zero)

---

## v1 — Live (current)

- FTUE onboarding (4 steps, zero seed data)
- Athletes tab: 5-tab dashboard per athlete
- Overview: stat cards with delta arrows, progress bars, measurables, schools, latest note
- Stats: sliders with timestamped history, contenteditable measurables
- Schools: cards with notes, next action, status, last contact
- Film: Hudl/YouTube link vault
- Notes: chronological with source label
- This Week: tasks by urgency + next event hero
- Calendar: events with tournament typeahead from tournaments.json
- Vault: Film / Coaches / Docs
- localStorage persistence
- Top nav + left sidebar layout
- Mobile responsive

Success metric: User 1 adds real data and returns the next day without being asked.

---

## v1.1 — Stabilization (after first week of real use)

Fix what User 1 finds broken or confusing. Do not ship until she has used v1 for at least a week.

- Edit and delete for all data types
- Recruiting timeline per athlete (visual milestones)
- **The Advisor** — AI recruiting advisor (see below, highest priority)
- Bug fixes from testing

---

## The Advisor — highest priority feature (v1.1)

User 1 identified this on day zero. She uses ChatGPT to ask recruiting questions, re-explaining Jake's full context every time.

The Advisor collapses that into one place. It knows the athlete's data already.

What it is: A tab on each athlete's dashboard. Parent types a question. System prompt is pre-loaded with that athlete's full profile — position, grade, stats, schools, upcoming events, coaching notes. Claude responds as a knowledgeable recruiting coach with full context.

What makes it different from ChatGPT: It already knows the kid. No re-explaining.

Tone requirement (from User 1's GPT response that she shared):
"I'm going to map this like a mini recruiting plan — not generic training, but what will actually move Jake up a tier by July."
The Advisor must sound like that. Specific, actionable, tied to real dates and events. Never generic.

System prompt structure:
```
You are an experienced college volleyball recruiting advisor.
You think in recruiting windows, tournament cycles, and development timelines.
You give specific actionable plans — not generic advice.

Athlete: [name]
Position: [position]
Class: [year] ([grade])
Sport: [sport] — [team]
Current stats: [stat name] [value], ...
Schools tracking: [name] ([status]), ...
Upcoming events: [name] [date], ...
Recent coach note: "[text]"

Answer the parent's question with specific, actionable advice tied to this athlete's actual situation.
```

Technical: Anthropic API call from client component. Same pattern as Social Studio. ANTHROPIC_API_KEY in Vercel env vars.

---

## v2 — Multi-device

When User 1 wants access on more than one device, or a second family wants Playbook.

- Supabase backend
- Clerk auth
- Multi-device sync
- Shared family access (mom + dad same account)
- localStorage data migration on first login

---

## v3 — Scale

Only if v2 validates with 10+ families.

- Multi-family SaaS + pricing
- Email digest ("Your week in Playbook")
- Push notifications
- NCAA recruiting calendar intelligence (auto-populate contact periods, dead periods)
- Document generation (auto-draft coach intro emails using athlete data)
- Scoutly integration — parent manages private data in Playbook, athlete manages public profile in Scoutly

---

## Not building (through v3)

- Coach-facing tools
- Public athlete profiles (that's Scoutly)
- Social features
- Video hosting
- Academic tracking
- NIL tools
