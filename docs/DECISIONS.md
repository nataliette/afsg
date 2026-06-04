# DECISIONS.md — Equipment Gym Tracker

Log of session decisions, directional choices, and reasoning.

**Updated by Claude at the end of each session.** Never edited manually — only appended.

---

## Format per session entry:

```
## [DATE] Session [N] — [one-line focus]

**Built / decided:**
- 

**My reasoning (use your actual words where possible):**
- 

**Alternatives considered:**
- 

**Deferred:**
- 

**Patterns observed:**
[1–2 sentences, observational, about how you directed this session]
```

---

## 2026-05-21 Session 1 — Template sync

**Built / decided:**
- Synced project with latest template from ~/Sites/claude-dev-templates/sample-project/
- Created README.md with Quick Start instructions
- Created docs/DECISIONS.md for session logging
- Created docs/METHODOLOGY.md for work pattern observations
- Updated CLAUDE.md with new Git workflow format ("ok commit" vs "session close" ritual)
- Expanded .gitignore with standard entries (sass cache, build outputs, logs, etc.)

**My reasoning:**
- Template updates ensure consistency across projects and provide standardized documentation structure
- New session close ritual (docs updates + commit) enables better project continuity and pattern recognition
- README.md and docs structure support future onboarding and decision tracking

**Alternatives considered:**
- Manual piecemeal updates — would miss template improvements
- Keeping old CLAUDE.md format — wouldn't benefit from new session management system

**Deferred:**
- None — template sync complete

**Patterns observed:**
- You prefer explicit rituals (session close ceremony) over ad-hoc documentation, enabling better retrospection and pattern discovery across projects.
- During review, you flagged that METHODOLOGY.md should reference existing patterns from memory files, not just capture new ones. Expanded it to include 4 observed patterns: quality verification first, semantic markup preferences, code safety checks, and structured workflows.

---

## 2026-05-21 Session 2 — CSS grid layout completion

**Built / decided:**
- Added CSS Grid styling to `_layout.scss` for equipment display results (`.results-grid`, `.grid-header`, `.grid-row`, `.grid-cell`)
- Verified 4-column equipment grid displays correctly on gym pages (Category, Brand, Equipment, Muscle Groups)
- Confirmed tag+gym filtered view (`?gym=pp&tag=back`) works correctly with proper layout and contextual header
- Tested all three main workflows: gym equipment view, tag search with gym summary cards, and tag+gym filtered view
- Compiled SCSS and committed grid styling changes

**My reasoning:**
- Equipment grid layout was missing CSS rule definitions despite being in the compiled file — added the missing `.results-grid` and cell styling rules to complete the layout structure
- Grid styling uses CSS custom properties (`--grid-cols`) for flexibility across different page types (4 columns for gym, different for others)
- Verified functionality across all views before considering work complete

**Alternatives considered:**
- Injecting CSS dynamically via JavaScript — not maintainable long-term; fixed by properly adding styles to SCSS
- Using different layout approach — grid with `display: contents` for header/row wrappers is the established pattern; kept consistent

**Deferred:**
- None — CSS grid styling is complete and functional

**Patterns observed:**
- Preview server has CSS loading limitations (external stylesheet not fully accessible via CSSOM) but code works correctly in real browsers — will note for future testing to prioritize localhost over preview tool for CSS verification
- Grid layouts are a central pattern across the app; when one is fixed, verify all related views work together (gym page, tag page, tag+gym filtered view)

---

## 2026-05-21 Session 3 — Equipment brand name standardization

**Built / decided:**
- Standardized all equipment brand names across equipment.json for consistency
  - "LifeFitness" → "Life Fitness" (26 instances)
  - "Hammerstrength" → "Hammer Strength" (11 instances)
  - "Glutebuilder" → "Glute Builder" (2 instances)
  - "Technogym" already correct, no changes needed
- Updated CLAUDE.md brand abbreviations documentation to reflect new names
- Applied design token colors to clickable equipment styling (hardcoded hex → SCSS variables)

**My reasoning:**
- Brand names should be properly spaced and capitalized for professional appearance
- Updating documentation keeps future reference accurate
- Used design tokens instead of hardcoded colors for maintainability

**Alternatives considered:**
- None — straightforward data consistency work

**Deferred:**
- None

**Patterns observed:**
- You batch similar maintenance tasks together (brand standardization + design token cleanup) rather than splitting them across sessions, treating consistency improvements as part of normal work quality

---

## 2026-05-21 Session 4 — Equipment category audit and standardization

**Built / decided:**
- Completed comprehensive equipment category audit across all 10 gyms
- Standardized all gyms to use 8 consistent categories in fixed order: Cables, Upper, Lower, Cardio, Barbells & Racks, Calisthenics, Core, Other
- Removed "Plate-Loaded" category from Tanjong Pagar; redistributed items to Upper/Lower by muscle group
- Created "Core" category for abdominal/core equipment; added to Labrador View, Tanjong Pagar, Pasir Panjang, Depot Heights
- Moved all cardio equipment from "Other" to "Cardio" where it existed
- Added "Hip Abductor/Adductor" to machineTypeMap in index.html for proper equipment page discovery
- Verified all changes in browser preview across multiple gyms (Labrador View, Tanjong Pagar, Pasir Panjang)
- Updated CLAUDE.md with equipment category documentation
- Updated memory files with audit details for future reference

**My reasoning:**
- Equipment categories were inconsistent across gyms (some had Plate-Loaded, some didn't; cardio scattered in Other)
- Standardization improves UX discoverability and makes gym pages predictable
- Moving Plate-Loaded items to Upper/Lower preserves functionality while eliminating unnecessary category
- Creating Core category consolidates abdominal equipment that was previously hidden in Other
- Verifying in browser ensures changes work in real environment, not just code inspection

**Alternatives considered:**
- Keep Plate-Loaded for Tanjong Pagar only — inconsistency would be confusing for users
- Create separate Abs category instead of Core — "Core" is more inclusive (exercises that target stabilizer muscles)
- Leave cardio in Other — less discoverable; dedicated Cardio category better aligns with gym layout logic

**Deferred:**
- None — audit complete and verified

**Patterns observed:**
- You approach systematic maintenance (like category audits) with a bias toward completeness and consistency rather than partial fixes. When identifying one issue (missing equipment mapping), you expanded to a full audit (category standardization across all gyms). This suggests you value holistic improvement over localized patches, preventing future inconsistencies.

---

## 2026-05-22 Session 5 — Version control and rollback setup

**Built / decided:**
- Created git tag `v1` pointing to the original online state (commit 1fe39a3, last pushed before today's work)
- Created git tag `v2` pointing to current local state (commit 8326c37, after 15 new commits from today's sessions)
- Pushed both tags to GitHub
- Added comprehensive "Version Control & Rollback" section to CLAUDE.md with three concrete methods:
  1. Browse versions on GitHub Releases
  2. Switch local code with `git checkout v1/v2`
  3. Create experimental branches from past versions with `git checkout -b experiment-v1 v1`
- Documented how to create future version tags for preservation

**My reasoning:**
- Version snapshots enable easy rollback and retroactive reference to specific development points without losing work
- Having v1 as a baseline before major session work provides a safety net for comparison or emergency rollback
- Documentation ensures the versioning workflow is repeatable and doesn't get forgotten by next session
- Three access methods (GitHub browse, local checkout, branching) cover different use cases (quick reference, local experimentation, advanced debugging)

**Alternatives considered:**
- Automated tagging on every session — too granular; explicit tagging on major milestones is cleaner
- Keeping version info only in git log — less discoverable; GitHub Releases are a standard way to surface versions

**Deferred:**
- GitHub Releases UI creation (created tags and pushed, but user will create releases manually via web interface for visibility)

**Patterns observed:**
- You value preservation and retroactive documentation; willing to set up infrastructure (version tags + documented rollback methods) to support future reference and debugging
- This aligns with existing preference for structured rituals — versioning is treated as a repeatable, documented process, not an ad-hoc action
- You think ahead about future scenarios (needing to look back, comparing versions) and want systems in place to support them

---

## 2026-05-28 Session 6 — City Hall equipment, cable grouping, cross-gym reorganization

**Built / decided:**
- Added complete equipment list for City Hall (formerly AF Cityhall) with 70+ pieces across all 8 categories
- Added `group` field to cable JSON items for modal grouping; updated `buildEquipmentIndex()` and `updateEquipmentModalText()` to display grouped cables under a heading (e.g., "Cable Station:") with indentation
- Expanded cable items from single grouped entries into individual lines (each Precor cable station item as its own entry)
- Reorganized equipment across three gyms:
  - **LV (Labrador View):** Moved Precor Multistation from Upper → Cables; fixed pre-existing duplicate Cables section bug in JSON
  - **PP (Pasir Panjang):** Moved Precor FTS Glide from Core → Cables
  - **TP (Tanjong Pagar):** Added `"group": "Cable Station"` to all cable items; moved Chin/Dip from Cables → Calisthenics; moved Captain's Chair from Calisthenics → Core
- Renamed gym "AF Cityhall" to "City Hall" in both `equipment.json` and `GYM_INITIALS` in `index.html`
- Added `machineTypeMap` entries: `Longpull 302` → `"Cable Row"`, `Pulldown 304` → `"Lat Pulldown"`
- Added Machine Type Groupings documentation to `PRODUCT.md`
- CLAUDE.md significantly restructured by user: reference content (equipment categories, machine type mappings, brand abbreviations) consolidated into PRODUCT.md; CLAUDE.md now covers workflow only

**My reasoning:**
- Separating cable station items individually makes equipment pages useful (e.g. `?equipment=Lat_Pulldown` now includes Pulldown 304) while grouping in the modal preserves the "Cable Station" context for AI workout prompts
- Cross-gym reorganization ensures consistent categorization: FTS Glide and Multistation are cable machines and belong under Cables; Chin/Dip is calisthenics; Captain's Chair is core
- PRODUCT.md as single reference for data structure decisions reduces duplication and makes CLAUDE.md focused on workflow

**Alternatives considered:**
- Keep Chin/Dip under Cables (where it physically lives) — rejected; it's categorized by movement type, not location
- Map Longpull 302 to "Seated Row" instead of "Cable Row" — rejected; user specified "Cable Row" as the distinct machine type

**Deferred:**
- None

**Patterns observed:**
- You treat individual equipment items as first-class searchable objects: when a cable station has multiple machines, each gets its own entry and URL, not just a group summary. Grouping is a display concern (modal/copy feature), not a data concern.
- You proactively rationalize naming across the whole dataset when renaming (gym name, GYM_INITIALS) rather than patching just the visible string.

---

## 2026-06-03 Session 7 — Workflow: replace memory files with project docs

**Built / decided:**
- Changed session close ritual: no longer writes to `~/.claude/projects/.../memory/` files
- All institutional knowledge now lives in version-controlled project docs (CLAUDE.md, PRODUCT.md, docs/DECISIONS.md)
- Migrated three memory-file entries into project docs:
  - SCSS `darken()` compiler bug → CLAUDE.md SCSS section
  - Clipboard API HTTP fallback requirement → CLAUDE.md Testing section
  - Tooltip UX deferred issue → PRODUCT.md Known UX Issues section
- Updated CLAUDE.md session close instructions accordingly

**My reasoning:**
- External memory files (`~/.claude/`) are not version-controlled — knowledge captured there is invisible to git history and can't be reviewed alongside the code that prompted it
- Project docs (CLAUDE.md, PRODUCT.md) are committed with the code, making institutional knowledge portable, reviewable, and durable across sessions

**Alternatives considered:**
- Keep both memory files and project docs in sync — too much duplication and friction; one source of truth is better

**Deferred:**
- None

**Patterns observed:**
- You prefer knowledge to live where you can see and control it — version-controlled docs over opaque tool-managed files. This is consistent with the preference for structured rituals: documentation as a first-class artifact of the work, not a side effect.

---

## Session — 03 Jun 2026

**Changes:**
- Completed equipment data for New Queensway and Grandtral Complex; both marked `complete`
- All exercise balls (previously labelled "Medicine Ball") unified under `Exercise Ball` naming convention with cm sizing (e.g. `Exercise Ball 55cm`, `Exercise Ball 65cm`)
- Added `machineTypeMap` entries to group `Exercise Ball 55cm` and `Exercise Ball 65cm` under `?equipment=Exercise_Ball`
- Removed hover tooltip feature from equipment items

**Removed feature — hover tooltip on equipment items:**
The equipment item row had a `mouseenter`/`mouseleave` tooltip that cloned the hidden `.equipment-tags` div and displayed it as a floating overlay. The tooltip appeared to the right of the item on hover, styled with a left-pointing arrow caret. It was removed because it disappeared on `mouseleave`, preventing the user from interacting with the tags inside it.

To reimplement: see git history for `setupEquipmentTooltips()` in `index.html` and `.equipment-tooltip` in `_layout.scss`. Key fix needed: use `pointer-events: auto` and switch from `mouseleave` to a click-toggle or a delayed hide so the user can reach the tooltip before it closes.

**Deferred:**
- None

---

## 2026-06-04 Session 9 — Buona Vista added, squat machine types split, City Hall initials fixed

**Built / decided:**
- Added complete equipment entry for Buona Vista (11th gym, status: complete, 34 items across 7 categories)
- Fixed City Hall `GYM_INITIALS` from `'AC'` to `'CH'` (URL changes from `?gym=ac` to `?gym=ch`)
- Split `machineTypeMap` squat types that were previously all grouped under `"Squat"`:
  - `Hack Squat`, `Linear Hack Squat` → `"Hack Squat"` (own type)
  - `Belt Squat` → `"Belt Squat"` (own type)
  - `Squat Machine`, `Perfect Squat`, `Super Squat Press` → `"Squat"` (generic machines)
  - `Pendulum X Squat` — left as its own standalone type (not grouped)

**My reasoning:**
- Buona Vista is a large gym with significant free weight infrastructure (HS racks, mini racks, deadlift platforms) alongside a full LF machine suite — needed a complete data entry pass
- City Hall initials `AC` were incorrect; `CH` is the obvious correct code
- Hack Squat and Belt Squat are mechanically distinct enough from generic squat machines to warrant their own `?equipment=` pages; previously they were all lumped together making the page misleading
- Pendulum X Squat is mechanically unique (pendulum arc vs fixed track) so it stays standalone

**Alternatives considered:**
- Grouping Pendulum X Squat under Hack Squat — rejected; it's a distinct movement pattern
- Grouping Super Squat Press under Leg Press — rejected; it's a squat-pattern machine

**Deferred:**
- None

**Patterns observed:**
- You audit machine type groupings when adding new gyms reveals gaps — adding BV's Hack Squat and Belt Squat alongside City Hall's Squat Machine made the existing flat grouping obviously wrong.
