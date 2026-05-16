# Project: Anytime Fitness Equipment Discovery

## The Original Problem

You wanted to track equipment available at each Anytime Fitness gym you train at in Singapore. The original idea was to create a Google Sheet to record what machines are at each location so you could plan workouts based on available equipment.

**Original request (Claude chat):** *"I want to set up a Google Sheet to track equipment available at each Anytime Fitness gym I train at. Please create the sheet in my Google Drive and structure it so I can easily update it after each gym visit."*

## What This App Actually Solves

Instead of a static Google Sheet, this evolved into an **interactive web application** for discovering equipment across your gym locations. 

**Core Purpose:** Help you quickly find what equipment is available at each gym and discover which gyms have equipment that targets specific muscle groups.

**Key capabilities:**
- Browse equipment at any of 9 Singapore Anytime Fitness gyms
- Search for equipment by muscle group (chest, back, shoulders, arms, legs, etc.)
- See which gyms have the equipment you want to use
- Generate a pre-formatted prompt for AI to create custom workouts based on available equipment

## Scope: What's In, What's Out

### ✅ In Scope
- **Equipment inventory**: What machines are at each gym (brand, model, muscle groups targeted)
- **Gym status**: Which gyms are fully catalogued vs. still in progress
- **Search by muscle group**: Find all gyms/equipment that target a specific muscle
- **Responsive design**: Works on desktop and mobile
- **AI integration point**: "Copy Equipment" feature to generate workout prompts

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

## Why This Isn't a Google Sheet

A spreadsheet works for simple data entry, but a web app solves a different problem:
- **Discoverability**: Search by muscle group across all gyms at once
- **Visual hierarchy**: See equipment organized by category and gym
- **Mobile-friendly**: Easy to check on phone while at the gym
- **Copy-to-AI**: Quickly format equipment for AI workout generation
- **Expandability**: If you ever want to add filtering, analytics, or integrations later

The Sheet was the original ask, but the web app is the better solution to the underlying problem.

## Decision Framework for New Features

When evaluating a feature request, ask:

1. **Does it help discover equipment or find alternatives?** 
   - ✅ Add machine names → YES
   - ✅ Add search by brand → maybe (if equipment database gets large)

2. **Does it turn this into a different app?**
   - ❌ User accounts → NO (this is for you)
   - ❌ Workout logging → NO (that's a different problem)
   - ❌ Real-time availability → NO (out of scope, requires external API)

3. **Is it solving a problem specific to this gym discovery use case?**
   - ✅ Filter equipment by gym status → YES
   - ✅ Better mobile search → YES
   - ❌ Diet planning → NO

## Success Metrics

- Equipment database is complete for all 9 gyms
- You can quickly find which gym has what you want to use
- You can easily copy equipment to generate AI workouts
- App works smoothly on desktop and mobile
- Minimal maintenance (mostly just data updates)

## Future Considerations (Not Currently Planned)

- Other Anytime Fitness locations outside Singapore
- Equipment photos/videos (nice to have, low priority)
- Workout history integration (different app)
- Comparison with other gym chains (out of scope)

---

**See [CLAUDE.md](../CLAUDE.md) for development workflow, testing setup, and architectural patterns.**
