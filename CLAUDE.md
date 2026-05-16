# CLAUDE.md — Project Guidelines

**📖 Project Documentation:** See [docs/project.md](docs/project.md) for project purpose, scope, and decision framework. Reference this when evaluating new features to avoid feature creep.

## Planning & Execution
**For any structural change or non-trivial task: propose a plan first, then wait for explicit user approval before executing.** Use EnterPlanMode when appropriate. Don't implement until the user says "go ahead" or similar.

## Git Workflow
**Only commit when the user explicitly says "commit".** Do not commit automatically. User will handle pushing to remote manually—never push. Wait for explicit "commit" before running any git commands.

**Before committing:** 
- Update CLAUDE.md and memory files (in `.claude/projects/-Users-natalie-Sites-afsg/memory/`) with any new context, patterns, or decisions discovered during the session. This ensures future sessions have up-to-date guidance and understanding of the project.
- If the project's scope or purpose has evolved, update `docs/project.md` to reflect the current state. The decision framework there guides future feature evaluations.
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
