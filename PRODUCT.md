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

**Design System:** Established with spacing scale (4px base), typography hierarchy, and neutral color palette. See CLAUDE.md for tokens.

---

## Success Metrics

- ✅ Equipment database is complete for all 10 gyms
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

---

**For development workflow, testing, and architectural patterns, see [CLAUDE.md](CLAUDE.md).**
