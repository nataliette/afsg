# PRODUCT: Anytime Fitness Equipment Discovery

## Vision & Purpose

**The Problem:** You wanted to track equipment available at each Anytime Fitness gym you train at in Singapore so you could plan workouts based on available equipment. The original idea was a Google Sheet, but that's just data entry.

**The Solution:** An interactive web application for discovering equipment across your gym locations.

**Core Purpose:** Help you quickly find what equipment is available at each gym and discover which gyms have equipment that targets specific muscle groups.

**Primary User:** You (personal tool for planning gym workouts)

**Why It Matters:** Gyms have different equipment. This app lets you find alternatives when a machine is unavailable, plan which gym to visit for specific workouts, and generate AI workout prompts based on real available equipment.

---

## Scope: What's In, What's Out

### ✅ In Scope
- Equipment inventory: What machines are at each gym (brand, model, muscle groups targeted)
- Gym status: Which gyms are fully catalogued vs. still in progress
- Search by muscle group: Find all gyms/equipment that target a specific muscle
- Browse equipment at any gym
- Responsive design: Works on desktop and mobile
- AI integration point: "Copy Equipment" feature to generate workout prompts

### ❌ Out of Scope
- User accounts or profiles
- Workout logging or progress tracking
- Calendar/scheduling features
- Social features (sharing, following, comments)
- Equipment availability status ("is the leg press available right now?")
- Real-time updates or push notifications
- Membership tracking or gym recommendations
- Wearable integration
- Nutrition planning or diet tracking

---

## Design Direction & Aesthetic

**Platform:** Responsive web app (desktop-primary, mobile-capable)

**Visual Style:** Clean, minimal, function-forward. Equipment-focused (not gym-focused).

**Core Interaction Pattern:** Search → Discover → Copy

**Mobile Considerations:** Must be usable at the gym on a phone (small screen, quick lookup)

**Design System:** Dark theme, Electric Yellow (`#FFE600`) accent, Space Grotesk + Inter fonts, CSS custom properties in `_themes.scss`.

---

## Architecture & Technical Design

### App Structure
- **Type:** Single-page SPA with vanilla JavaScript
- **Routing:** Client-side via URL params (no backend)
- **Params used:**
  - `?gym=xx` — gym page showing all equipment at a specific gym
  - `?equipment=xx` — equipment page showing all gyms with a specific machine type
  - `?tag=xx` — tag page showing all gyms with equipment targeting a muscle group
  - No params — landing page (entry point)

### Page Terminology
When discussing pages, use these parameter-based names:
- **gym page** (`?gym=xx`) — equipment in a specific gym
- **equipment page** (`?equipment=xx`) — all gyms with a specific equipment type
- **tag page** (`?tag=xx`) — equipment matching a muscle group
- **landing page** (no params) — entry point

### Key Patterns
- **GYM_INITIALS:** Maps gym names to 2-letter codes (e.g., Harbourfront = 'HF'), used in URLs and throughout the app
- **PRIMARY_TAGS:** Core muscle groups shown on landing + search focus (back, chest, arms, legs)
- **isPrimaryTag():** Function to determine tag styling (dark blue for primary, light blue for secondary)
- **Status dots:** green = complete, orange = ongoing (showing gym cataloguing status)
- **CSS Grids:** Result displays use CSS Grid layouts (divs), not HTML `<table>` elements. When you say "table" you mean the grid
- **Mobile breakpoint:** 768px (below: stacked card layout; above: multi-column grid)
- **State management:** Event delegation + CSS class toggling

### Equipment Data Structure

#### Equipment Categories (Standardized)
All 11 gyms use this consistent order:
1. **Cables** — Cable machines & pulleys
2. **Upper** — Upper body machines (chest, back, shoulders, arms)
3. **Lower** — Lower body machines (legs, quads, hamstrings, glutes, calves, hip ab/ad)
4. **Cardio** — Cardiovascular equipment (treadmills, bikes, rowers, ellipticals, stair climbers)
5. **Barbells & Racks** — Free weight equipment
6. **Calisthenics** — Bodyweight stations (pull-up bars, dip stations)
7. **Core** — Core/abdominal machines & exercises
8. **Other** — Miscellaneous items (medicine balls, bosu balls, tools)

**History:** "Plate-Loaded" category removed (items redistributed), "Core" added for abdominal equipment, all cardio consolidated.

#### Machine Type Mappings (`machineTypeMap`)
Maps individual equipment names to canonical machine types for `?equipment=` URLs and cross-gym search:

**Key mappings:**
- Precor `Longpull 302` → `"Cable Row"`
- Precor `Pulldown 304` → `"Lat Pulldown"`
- `FTS Glide`, `Adjustable Pulley`, `Multistation` → `"Cable Station"`
- `Diverging Lat Pulldown`, `ISO Lateral Front Lat Pulldown` → `"Lat Pulldown"`
- `Diverging Seated Row`, `Low Row` → `"Seated Row"`
- `Hack Squat`, `Linear Hack Squat` → `"Hack Squat"` (own type)
- `Belt Squat` → `"Belt Squat"` (own type)
- `Squat Machine`, `Perfect Squat`, `Super Squat Press` → `"Squat"` (generic squat machines)
- `Pendulum X Squat` → own type (not grouped)

**Rule:** Brand-specific model names that map to known types should be added to machineTypeMap. Unique standalone types get their own URL.

**Ball equipment naming convention:**
- Exercise balls (stability/Swiss balls) are named with cm sizing: `Exercise Ball 55cm`, `Exercise Ball 65cm` — both map to `"Exercise Ball"`
- These are **not** medicine balls — medicine balls are weighted and measured in kg/lb; exercise balls are inflated and measured by diameter in cm
- If a ball entry has no size, use just `Exercise Ball`

#### Equipment Brand Abbreviations
Shorthand for rapid data entry (expands to full names in equipment.json):
- **C2** = Concept2
- **LF** = Life Fitness
- **HS** = Hammer Strength (or HA, LG as typo variants)

#### Tracking Equipment Updates
Each gym has a `lastUpdated` field in equipment.json (format: "DD MMM YYYY", e.g., "21 May 2026"):
- Derived from git commit history
- Displayed in UI as "✓ Complete — updated 21 May 2026"
- Update the field when adding/modifying equipment
- Commit message format: `feat: update [Gym Name] equipment — 21 May 2026`

### Design System & Tokens

**Theme:** Dark — near-black base with Electric Yellow (`#FFE600`) accent. A second purple theme (`#8B5CF6`) exists in `_themes.scss` as a hidden easter egg (toggle button hidden via `display:none`; see DECISIONS.md Session 10).

**File Structure:**
- `styles.scss` — import wrapper only; compiled to `styles.css`
- `_themes.scss` — CSS custom properties for both themes (imported ONCE in `styles.scss` — never from partials)
- `_variables.scss` — SCSS variables for spacing, typography, radii, transitions
- `_base.scss` → `_components.scss` → `_layout.scss` → `_responsive.scss` → `_inventory.scss`

**CSS Custom Properties (design tokens):**
| Token | Value | Usage |
|---|---|---|
| `--bg-base` | `#111111` | Page background |
| `--bg-surface` | `#1a1a1a` | Cards, modals |
| `--bg-sidebar` / `--bg-header` | `#161616` | Sidebar, header |
| `--border` | `#2a2a2a` | All borders |
| `--text-primary` | `#f0f0f0` | Main text |
| `--text-secondary` | `#c0c0c0` | Subdued labels |
| `--text-tertiary` | `#888888` | Placeholders, meta |
| `--accent` | `#FFE600` | CTAs, active states, headings |
| `--accent-hover` | `#FFC700` | Hover state of accent |
| `--accent-muted` | `rgba(255,230,0,0.12)` | Subtle accent backgrounds |
| `--on-accent` | `#111111` | Text on accent backgrounds |
| `--status-complete` | `#22c55e` | Green status dot |
| `--status-partial` | `#f97316` | Orange status dot |

**Fonts:** Space Grotesk (headings/card titles, 500–700) + Inter (body, 400–600) via Google Fonts.

**Spacing:** 4px baseline — `$spacing-xs` (4) through `$spacing-4xl` (40).

**Important:**
- Never edit `styles.css` directly. Modify SCSS partials and recompile: `sass styles.scss styles.css`
- `_themes.scss` must only be imported in `styles.scss`, never in individual partials (causes `:root` duplication)
- All colour rules on equipment items must be **explicit** (never `color: inherit`) — see DECISIONS.md Session 11 for why
- Increment `?v=X` on the `<link rel="stylesheet">` in `index.html` when deploying to bust Caddy's cache

**Note on CSS Grid styling:** Equipment displays use `.results-grid` (4 columns on gym page, managed via `--grid-cols` CSS custom property). Gym summary cards use `.gym-summary-row`. Both are fully styled and functional; preview tool has CSS loading limitations but all CSS compiles correctly.

### Accessibility & Security
- ✓ Keyboard navigation — all interactive elements have visible focus states (outlines)
- ✓ External links — all have `target="_blank" rel="noopener noreferrer"`
- ✓ Images — meaningful `alt` text for screen readers
- ✓ Responsive design — gracefully adapts at breakpoints (mobile, tablet, desktop)
- ✓ WCAG AA minimum — color contrast, text sizing, target sizes meet standards
- ✓ No hardcoded credentials or API keys in code

---

## Success Metrics

- ✅ Equipment database is complete for all 11 gyms
- ✅ You can quickly find which gym has what you want to use
- ✅ You can easily copy equipment to generate AI workouts
- ✅ App works smoothly on desktop and mobile
- ✅ Minimal maintenance (mostly just data updates)

---

## Decision Framework for New Features

When evaluating a feature request, ask:

**1. Does it help discover equipment or find alternatives?**
   - ✅ Add machine names, variants → YES
   - ✅ Add search by brand → maybe (if database gets large)

**2. Does it turn this into a different app?**
   - ❌ User accounts → NO (this is for you)
   - ❌ Workout logging → NO (that's a different problem)
   - ❌ Real-time availability → NO (out of scope, requires external API)

**3. Is it solving a problem specific to gym discovery?**
   - ✅ Filter equipment by gym status → YES
   - ✅ Better mobile search → YES
   - ✅ Gym comparison/ranking → YES
   - ❌ Diet planning → NO

**Red flags:** If a feature requires user accounts, backend infrastructure, or real-time data, it's probably out of scope.

---

## Future Considerations (Not Currently Planned)

- Other Anytime Fitness locations outside Singapore (expansion)
- Equipment photos/videos (nice to have, low priority)
- Workout history integration (different product)
- Comparison with other gym chains (out of scope, different market)

## Known UX Issues (Deferred)

None currently.

---

**For development workflow, testing, and architectural patterns, see [CLAUDE.md](CLAUDE.md).**
