# CLAUDE.md — Equipment Gym Tracker

**📖 Project Documentation:** See [docs/project.md](docs/project.md) for project purpose, scope, and decision framework. Reference this when evaluating new features to avoid feature creep.

---

## Planning & Execution

**For any structural change or non-trivial task:**
1. Claude proposes a plan first
2. You approve or redirect before implementation begins
3. Do not implement until you explicitly say "go ahead" or similar

This prevents wasted work and ensures alignment on approach.

---

## Quick Commands

- **"ok commit"** → Simple git commit with conventional message (no doc updates)
- **"session close"** → Comprehensive ritual: update all docs, then commit

---

## Git & Commit Workflow

**"ok commit":** Use this when you've made changes that should be saved but don't represent a complete session:
- Run `git add [files]` and commit with a conventional commit message
- No doc updates — just the commit
- Example: `feat: add dark mode toggle`

**"session close":** Use this at the end of a work session to wrap up and document:
- Claude updates CLAUDE.md, docs/DECISIONS.md, docs/METHODOLOGY.md, and docs/project.md
- Claude creates the commit automatically
- This increments the template review counter (at 20 session closes, you'll be reminded to run `/review-workflow`)

**Commit message format (for "ok commit"):**
- Use conventional commits: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`
- Example: `feat: add equipment search filter`
- Session close commits use: `docs: session close — [brief description]`

**Do NOT push manually** — you handle that via GitHub Desktop

**Before committing:**
- Update CLAUDE.md and memory files (in `.claude/projects/-Users-natalie-Sites-afsg/memory/`) with any new context, patterns, or decisions discovered during the session.
- If the project's scope or purpose has evolved, update `docs/project.md` to reflect the current state.
- When implementing new features, consult `docs/project.md`'s decision framework to ensure alignment with core purpose and avoid scope creep.

## Structural Changes
**Before making any structural change, step back and review the entire structure at a high level.** Optimize holistically—don't hack together a function to solve the immediate problem. Consider:
- How does this fit into the overall app flow and layout?
- Are there existing patterns or components I should reuse?
- Can I refactor other parts to support this change cleanly?
- What's the simplest, most maintainable solution?

**After implementing, audit the page structure.** Don't use shortcut hacks. Verify:
- HTML hierarchy is clean
- CSS classes match element structure
- No duplicate IDs or unused classes
- Mobile and desktop layouts are intact

## SCSS & Design System
- All styles live in `styles.scss` (compiled output) and individual `_*.scss` partials
- **Never edit `styles.css` directly** — always modify the SCSS partials and recompile
- Design tokens in `_variables.scss`: colors, spacing (4px scale), typography, transitions
- Organized as: `_variables.scss` → `_base.scss` → `_components.scss` → `_layout.scss` → `_responsive.scss`
- Compile with: `sass styles.scss styles.css`
- Or watch mode: `sass --watch styles.scss:styles.css`
- **Note on CSS Grid styling:** Equipment result displays use `.results-grid` with CSS Grid layout (4 columns on gym page, managed via `--grid-cols` CSS custom property). Gym summary cards use `.gym-summary-row` with `display: grid`. Both are fully styled and functional in real browsers; preview tool has CSS loading limitations but all CSS is correct in the compiled file.

## Page Terminology
When discussing pages, use these parameter-based names for clarity:
- **gym page**: `?gym=xx` — shows equipment in a specific gym
- **equipment page**: `?equipment=xx` — shows all gyms with a specific equipment type
- **tag page**: `?tag=xx` — shows equipment matching a muscle group tag
- **landing page**: no params — entry point with gym and tag browsing

## App Architecture
- Single-page SPA with vanilla JavaScript
- Client-side routing via URL params: `?gym=xx` for gym view, `?tag=xx` for search, `?equipment=xx` for equipment lookup
- Landing page shows when no params (entry point)
- Event delegation, CSS class toggling for state management
- Equipment data from `equipment.json` with mock fallback for preview

## Key Patterns
- **GYM_INITIALS**: Maps gym names to 2-letter codes (e.g., Harbourfront = 'HF')
- **PRIMARY_TAGS**: Core muscle groups shown on landing + search focus
- **isPrimaryTag()**: Determines tag styling (dark blue for primary, light blue for secondary)
- Status dots: green = complete, orange = ongoing
- Mobile breaks at 768px; tables convert to card grid layout
- **CSS Grids:** Result displays (gym page, tag page, equipment page) use CSS Grid layouts (divs), not HTML `<table>` elements. When user says "table" they mean the CSS Grid result display.

## Equipment Brand Abbreviations
When entering equipment quickly, these abbreviations expand to full brand names:
- **C2** = Concept2 (rowing/ski machines)
- **LF** = LifeFitness (machines, cardio)
- **HS** = Hammerstrength (iso-lateral machines, racks)
- **HA** = Hammerstrength (typo variant of HS)
- **LG** = LifeFitness (typo variant of LF)

## Adding New Gyms
**When adding a new gym to equipment.json, immediately add it to `GYM_INITIALS` in index.html.** This is critical:
- **GYM_INITIALS** maps gym names to 2-letter codes and is used throughout the app when generating URLs and processing gym data
- Without an entry, any code calling `GYM_INITIALS[gymName].toLowerCase()` throws `"Cannot read properties of undefined"` error
- The app will load but crash when trying to interact with the gym
- Always follow the pattern: `'Gym Name': 'GN'` (first letter of each word)

**Example:** When adding "Depot Heights", add `'Depot Heights': 'DH'` to the GYM_INITIALS object immediately.

## Tracking Equipment Update Dates
**Every gym in equipment.json has a `lastUpdated` field** (format: `"DD MMM YYYY"`, e.g., `"21 May 2026"`). This is derived from git commit history:
- When you provide updated equipment data, capture today's date in human-readable format with 3-letter month abbreviation (e.g., "21 May 2026", "15 Jan 2026", "03 Dec 2025")
- Update the gym's `lastUpdated` field in equipment.json to this date
- The UI displays this as "✓ Complete — updated 21 May 2026" or "⚠ Partial — updated 15 May 2026"

**Process when adding/updating equipment:**
1. Add or modify equipment data in equipment.json
2. Update the gym's `lastUpdated` field to today's date (format: "DD MMM YYYY" with 3-letter month)
3. Commit with the gym name and date in message: `feat: update [Gym Name] equipment — 21 May 2026`

**Dates are tracked via:**
- Git commit history (primary source of truth)
- `lastUpdated` field in equipment.json (displayed to users)

## Testing

### Server Setup
- Start HTTP server: `python -m http.server 8000` (in project root)
- If port 8000 busy: check `lsof -i :8000` and kill process as needed
- If port 8080 busy with unrelated site: close that to avoid confusion

### Testing Workflow
**Claude Code Preview Panel (Desktop):** Opens `http://localhost:8000/` automatically when configured

**Mobile Device (iPhone/Brave):**
1. Find your machine's local IP: Run `ifconfig | grep "inet " | grep -v 127.0.0.1` in Terminal
   - Or check System Preferences > Network
2. Visit `http://<LOCAL_IP>:8000` on your iPhone (e.g., `http://192.168.1.50:8000`)
3. Both desktop and mobile use the same server instance — changes visible on both after refresh

### Key Notes
- equipment.json fetch works correctly with server (no mock fallback)
- After code changes: refresh both devices to see updates
- Mobile layout testing: rotate iPhone to verify responsive breakpoints (768px)

## No Shortcuts
- Don't skip structural audits
- Don't commit without approval
- Don't edit compiled CSS directly
- Don't use feature flags or backwards-compat shims for unused code

---

## Accessibility & Security

✓ **Keyboard navigation:** All interactive elements have keyboard focus states (visible outlines)
✓ **External links:** All have `target="_blank" rel="noopener noreferrer"`
✓ **Images:** All have meaningful `alt` text for screen readers
✓ **Responsive design:** Adapts gracefully at breakpoints (mobile, tablet, desktop)
✓ **WCAG AA minimum:** Color contrast, text sizing, interactive target sizes meet accessibility standards

---

## Quick Reference

```bash
# Check what changed before commit
git status
git diff

# Compile SCSS (if applicable)
sass styles.scss styles.css

# Start dev server
python -m http.server 8000
```

---

## Important Notes

- Edit CLAUDE.md as you learn more about the project — keep it up to date
- Design tokens are universal; only compilation method varies by stack
- When in doubt, ask yourself: "Is this aligned with the vision / scope / aesthetic I defined?"
