# Playbook

Private family command center for parents managing youth athletes through the college recruiting journey.

## What this is

A personal operating system for one family. Not a social network. Not a recruiting platform. A private binder that replaces the texts, notes, spreadsheets, and memory a parent currently uses to track their athlete's development and recruiting.

## First user

Developer's wife. Managing twin 16-year-old boys — Jake (Libero) and Wyatt (Setter/Opp) — both playing club volleyball (SCVA) and high school volleyball (Mira Costa HS). Class of 2028.

## Stack

Next.js 16 · TypeScript · Tailwind CSS · Vercel · localStorage (v1) · Supabase + Clerk (v2)

## Spec files

| File | Purpose |
|---|---|
| `claude.md` | Session context — read first every session |
| `agents/design.md` | Visual design system and component specs |
| `agents/frontend.md` | Layout, navigation, and UI behavior |
| `agents/data.md` | TypeScript interfaces and data model |
| `agents/qa.md` | Testing checklist and regression suite |
| `agents/product.md` | Product vision, principles, and user persona |
| `data/data-models.md` | Full data model with defaults and logic |
| `data/tournaments.json` | SoCal boys volleyball tournament schedule 2025–26 |
| `product/roadmap.md` | Phased feature roadmap |
| `product/vision.md` | Long-term product vision |
| `recruiting/ncaa-rules.md` | NCAA volleyball recruiting rules and calendar |
| `design-reference.html` | Visual source of truth — extract CSS from here |

## Key decisions

- No bottom nav — top nav + left sidebar
- Athletes are a dynamic list, not hardcoded nav items
- Stats use timestamped entry history — delta arrows compare current vs previous entry
- Default stats injected by position on athlete creation
- Tournament data in tournaments.json powers calendar typeahead and stat context suggestions
- localStorage only in v1 — Supabase migration path documented in agents/data.md
