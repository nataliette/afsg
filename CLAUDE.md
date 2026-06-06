# CLAUDE.md — Equipment Gym Tracker

**📖 Product & Architecture:** See [PRODUCT.md](PRODUCT.md) for vision, scope, architecture, data structures, and design system. This file covers Claude's workflow only.

---

## Planning & Execution

**For any structural change or non-trivial task:**
1. Claude proposes a plan first
2. You approve or redirect before implementation begins
3. Do not implement until you explicitly say "go ahead" or similar

This prevents wasted work and ensures alignment on approach.

**Before making structural changes, step back and review the entire structure holistically—don't hack together a function to solve the immediate problem.** Consider:
- How does this fit into the overall app flow and layout?
- Are there existing patterns or components I should reuse?
- Can I refactor other parts to support this change cleanly?
- What's the simplest, most maintainable solution?

**After implementing, audit the page structure:** HTML hierarchy is clean, CSS classes match element structure, no duplicate IDs or unused classes, mobile and desktop layouts are intact.

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
- Claude updates CLAUDE.md and/or PRODUCT.md with any new patterns, decisions, or gotchas
- Claude appends a session entry to docs/DECISIONS.md
- Claude updates README.md if the stack, quick-start steps, or doc list changed
- Claude creates the commit automatically

**Commit message format (for "ok commit"):**
- Use conventional commits: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`
- Example: `feat: add equipment search filter`
- Session close commits use: `docs: session close — [brief description]`

**Do NOT push manually** — you handle that via GitHub Desktop

**When committing, check:**
- Any new patterns or gotchas are captured in CLAUDE.md or PRODUCT.md (not external memory files)
- If product scope has evolved, PRODUCT.md is updated
- New features align with PRODUCT.md's decision framework

## Version Control & Rollback

This project uses **git tags** (v1, v2, etc.) to preserve versioned snapshots published as GitHub Releases.

**To view or rollback:**

**Option 1: Browse on GitHub**
1. Go to repo → **Releases** tab → click the version you want

**Option 2: Switch local code**
```bash
git checkout v1          # Switch to v1 (original state)
git checkout main        # Switch back to latest
git checkout -b branch-name v1  # Create new branch from v1 to experiment
```

**Option 3: Create new version tags**
```bash
git tag v3              # Create tag v3 at current commit
git push origin v3      # Push tag to GitHub
# Then create release on GitHub for discoverability
```

---

## SCSS & Design System

**Compilation only** — for design tokens, see PRODUCT.md.

- All styles in `styles.scss` (compiled output) and `_*.scss` partials
- **Never edit `styles.css` directly** — modify SCSS partials and recompile
- Organized as: `_variables.scss` → `_base.scss` → `_components.scss` → `_layout.scss` → `_responsive.scss`

**Compile with:**
```bash
sass styles.scss styles.css       # One-time compile
sass --watch styles.scss:styles.css  # Watch mode
```

**Gotcha — `darken()` compiles to invalid CSS:** Dart Sass compiles `darken()` to floating-point RGB values (e.g. `rgb(11.538...)`) which breaks the CSS parser. Use hardcoded hex instead:
- `darken(#f5f5f5, 5%)` → `#ececec`
- `darken(#2196f3, 10%)` → `#1976d2`

---

## Adding New Gyms

**When adding a gym to equipment.json, immediately add it to `GYM_INITIALS` in index.html.** This is critical:
- **GYM_INITIALS** maps gym names to 2-letter codes and is used throughout the app when generating URLs
- Without an entry, any code calling `GYM_INITIALS[gymName].toLowerCase()` throws a crash error
- Always follow the pattern: `'Gym Name': 'GN'` (first letter of each word)

**Example:** When adding "Depot Heights", add `'Depot Heights': 'DH'` to GYM_INITIALS immediately.

---

## Updating Equipment Data

**When adding or modifying equipment:**
1. Add/modify equipment in equipment.json
2. Update the gym's `lastUpdated` field to today's date (format: "DD MMM YYYY" with 3-letter month, e.g., "21 May 2026")
3. Commit with: `feat: update [Gym Name] equipment — 21 May 2026`
4. After committing, share the local preview link: **[http://gymcat.test/](http://gymcat.test/)** so the user can verify the changes

**See PRODUCT.md for:**
- Equipment category definitions
- Machine type mappings (machineTypeMap)
- Equipment brand abbreviations (C2, LF, HS, etc.)
- How lastUpdated is tracked and displayed

---

## Testing

### Local preview (Caddy)
Static site — Caddy serves the files directly, so just open **http://gymcat.test/** (no dev server needed).

**Port-80 caveat:** `.test` works only while Caddy is running and LocalWP is quit (both want port 80). Toggle on with `osascript -e 'quit app "Local"'` then `sudo brew services start caddy`; reverse to return to WordPress.

### Server Setup
```bash
python -m http.server 8000  # Start HTTP server (project root)
```

**If port busy:**
```bash
lsof -i :8000     # Find process
kill -9 <PID>     # Kill it
```

### Testing Workflow
**Desktop (Claude Code Preview):** Opens `http://localhost:8000/` automatically when configured

**Mobile (iPhone/Brave):**
1. Find machine IP: `ifconfig | grep "inet " | grep -v 127.0.0.1`
2. Visit `http://<LOCAL_IP>:8000` (e.g., `http://192.168.1.50:8000`)
3. Both desktop and mobile share the same server — refresh both after changes

**Key notes:**
- equipment.json fetch works correctly with server
- Mobile layout testing: rotate iPhone to verify 768px breakpoints
- **Clipboard API on HTTP:** `navigator.clipboard` is unavailable on local network IPs (requires HTTPS). The Copy to Clipboard button uses a `document.execCommand('copy')` fallback — always keep that fallback in place.

---

## No Shortcuts

- Don't skip structural audits
- Don't commit without approval
- Don't edit compiled CSS directly
- Don't use feature flags or backwards-compat shims for unused code

---

## Quick Reference

```bash
git status                    # Check what changed
git diff                      # See changes before commit
sass styles.scss styles.css  # Compile SCSS
python -m http.server 8000   # Start dev server
```
