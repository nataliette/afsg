# CLAUDE.md — Project Guidelines

## Planning & Execution
**For any structural change or non-trivial task: propose a plan first, then wait for explicit user approval before executing.** Use EnterPlanMode when appropriate. Don't implement until the user says "go ahead" or similar.

## Git Workflow
**Only commit when the user explicitly says "commit".** Do not commit automatically. User will handle pushing to remote manually—never push. Wait for explicit "commit" before running any git commands.

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

## App Architecture
- Single-page SPA with vanilla JavaScript
- Client-side routing via URL params: `?gym=xx` for gym view, `?tag=xx` for search
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
- Dev server not used; open `index.html` directly in browser
- Claude preview panel: equipment.json fetch may fail, falls back to mock data (intentional for preview)
- Mobile testing: resize browser or use device preview tools

## No Shortcuts
- Don't skip structural audits
- Don't commit without approval
- Don't edit compiled CSS directly
- Don't use feature flags or backwards-compat shims for unused code
