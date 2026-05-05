# FridgeAI — Product Requirements Document

> **Snap your fridge. Get dinner.**

**Version:** 1.0  
**Status:** Draft  
**Owner:** Product  
**Last Updated:** April 30, 2026

---

## 1. Executive Summary

FridgeAI is a mobile-first application that turns the contents of a user's fridge or pantry into personalized dinner recipes in under 30 seconds. The user takes one photo (or types ingredients); AI identifies what's available; an LLM generates 3 recipe options matched to the user's dietary preferences, time available, and skill level. Each recipe includes calorie counts, macros, and a shopping list for any missing items.

The product solves the most common and most ignored daily friction in cooking: **deciding what to make**. We're not building another recipe app. We're building a decision-removal tool.

### Problem Statement

78% of household cooks report *"deciding what to cook"* as more stressful than the cooking itself (NPD Group, 2024). The average household wastes ~$1,500/year in food because ingredients expire before being used (USDA). Existing recipe apps require the user to first know what they want, then check what they have, then search — exactly backwards.

### Vision

> Anyone with a phone and a fridge should be able to make a good dinner in 30 minutes, without thinking, without waste, and without ordering takeout.

---

## 2. Goals & Non-Goals

### ✅ Goals (V1)

- Reduce time-to-decision (open app to chosen recipe) to under **60 seconds**
- Generate at least **3 viable recipe options** for any input with 5+ ingredients
- Surface accurate calorie counts (within ±10% of USDA values) for every recipe
- Auto-generate a **shopping list** for missing ingredients with one tap
- Reach **10,000 MAU** within 6 months of public launch

### ❌ Non-Goals (V1)

- **Photo-based ingredient detection** — cut to V2. Computer vision adds significant complexity (model training, edge cases, packaging OCR) and the text input flow is faster to validate. Once we have product-market fit signal, we'll invest in CV.
- Grocery delivery integration (V2)
- Social features — sharing recipes, following users, comments (V3)
- Meal planning across multiple days (V2)
- Smart fridge / IoT hardware integration (out of scope)
- Restaurant recommendations or takeout integration

---

## 3. Target Users & Personas

### Primary: "Tired Tara" (60% of users)

> 28-40 years old. Working professional. Cooks 4-5 dinners a week. Decision-fatigued by 7pm. Has groceries but "nothing to cook." Will order Swiggy if she can't decide in 5 minutes.

**Job to be done:** *"When I get home tired, I want to know what to cook with what's already there, so I can avoid ordering in."*

### Secondary: "Health-Conscious Hari" (25% of users)

> 25-35 years old. Tracking macros or weight goals. Cooks deliberately. Wants visibility into calories and protein per meal. Currently using MyFitnessPal + Google for recipes — two apps, two workflows.

**Job to be done:** *"When I plan dinner, I want to see calorie and macro info upfront, so I can stay on my goals without guessing."*

### Tertiary: "New-to-Cooking Neha" (15% of users)

> 20-26 years old. Recently moved out / started cooking. Has ingredients but doesn't know what to do with them. Intimidated by recipe complexity.

**Job to be done:** *"When I want to cook but feel overwhelmed, I want simple matched-to-my-skill recipes, so I can build confidence in the kitchen."*

---

## 4. Solution Overview

**Three-tap flow:**

- **Tap 1**: Open app, type ingredients (or snap fridge in V2)
- **Tap 2**: Confirm pantry staples (15s)
- **Tap 3**: Choose one of 3 recipe cards (30s, 45s, 60s options)

**Total time:** under 60 seconds from app open to cooking.

### Core User Flow

| Step | User Action | System Action | Time Budget |
|------|-------------|---------------|-------------|
| 1 | Opens app, taps Type Ingredients | Text input launches | 3 sec |
| 2 | Types ingredient list | Parsed to chips | 15 sec |
| 3 | Taps pantry staples they have | Stored in state | 10 sec |
| 4 | Taps Generate Recipes | LLM call returns 3 recipes with macros | 8 sec |
| 5 | Picks one recipe, starts cooking | Step-by-step view, timer, shopping list | 30 sec to decide |

---

## 5. Key Features (V1)

### F1. Ingredient Input
Text-based input with smart parsing (comma-separated or newline). Auto-capitalization, deduplication, normalization.

### F2. Pantry Staples Check
12 common staples (salt, oil, garlic, ginger, etc.) shown as a tappable grid. Reduces input fatigue dramatically — users don't need to type "salt" every time.

### F3. Personalized Recipe Generation
LLM call (Claude) generates 3 recipes given: ingredients, dietary preferences, cuisine preference, time available, skill level.

### F4. Calorie & Macro Display
Per-serving calories, protein, carbs, fat — derived from ingredient quantities and a USDA-backed nutrition database. Displayed on every recipe card before user commits.

### F5. Cook Mode
Step-by-step view with built-in timer. Hands-free voice mode (V1.5).

### F6. Missing Ingredient Shopping List
If a recipe needs 1-2 items the user doesn't have, generate a shopping list. Tappable export to Notes/WhatsApp. Future: integration with Zepto/Blinkit/Instacart.

---

## 6. User Stories & Acceptance Criteria

11 stories across 4 epics, sprint-ready for Jira / Linear import.

### EPIC 1: Ingredient Input

**FR-101: Quick text input flow** *(P0, 3 pts)*
> *As a* hungry user, *I want to* open the app and immediately type my ingredients, *so that* I can get to recipes in under 60 seconds.

Acceptance: App opens to input screen in < 2s • Comma-separated or newline parsing • Auto-capitalization & deduplication • Edits persist if user navigates back.

**FR-102: Editable ingredient chips** *(P0, 5 pts)*
> *As a* user, *I want to* edit my ingredients before generating recipes, *so that* I can fix typos and add anything missed.

Acceptance: Tap chip to remove • Add button opens text input • Smart pairing suggestions ("You added paneer — also have onion?") • Edits persist across navigation.

### EPIC 2: Pantry Check

**FR-201: Tap-to-confirm pantry staples** *(P0, 5 pts)*
> *As a* user, *I want to* tell the app which common staples I have without typing each one, *so that* I get better recipes without input fatigue.

Acceptance: 12 staple items shown as tappable grid • Visual confirmation on selection • Selections feed into recipe generation prompt.

### EPIC 3: Recipe Generation

**FR-301: Generate 3 recipes via LLM** *(P0, 8 pts)*
> *As a* user, *I want* 3 recipe options based on my ingredients, *so that* I have choice without being overwhelmed.

Acceptance: Exactly 3 recipes returned • Each includes name, time, difficulty, calories, protein, full ingredient list, missing ingredients, step-by-step • Response P50 < 8s, P95 < 15s • Hard-rules layer blocks unsafe combos.

**FR-302: Calorie & macro display** *(P0, 5 pts)*
> *As a* health-conscious user, *I want to* see calories and protein for each recipe before committing, *so that* I can stay on my goals.

Acceptance: Per-serving calories on every card • Macro breakdown on detail view • Within ±10% of USDA values • "Estimate" label visible.

**FR-303: Recipe match score & ranking** *(P1, 3 pts)*
> *As a* user, *I want to* see recipes ranked by what I already have, *so that* I can pick the easiest option fast.

Acceptance: Match % calculated from ingredient overlap • No-missing recipes score 95-100% • 1-2 missing items score 75-90% • Recipes sorted high-to-low by match.

### EPIC 4: Cook & Shop

**FR-401: Step-by-step recipe view** *(P0, 5 pts)*
> *As a* user actively cooking, *I want* the recipe broken into clear steps, *so that* I can follow along without scrolling and getting lost.

Acceptance: Numbered steps with clear typography • Ingredient list with have/missing indicators • Chef's tip displayed • Back navigation preserves state.

**FR-402: Auto-generated shopping list** *(P0, 5 pts)*
> *As a* user, *I want* a shopping list when a recipe needs items I don't have, *so that* I can grab them on the way home.

Acceptance: One-tap "Build shopping list" CTA on recipes with missing items • Shows item + quantity • Estimated total cost displayed.

**FR-403: Copy / share shopping list** *(P0, 3 pts)*
> *As a* user, *I want to* send my shopping list to WhatsApp or copy it, *so that* I can use it outside the app.

Acceptance: Copy button works in all browsers (incl. sandboxed iframes via 3-tier fallback) • WhatsApp share opens with pre-filled text • Visual "Copied!" confirmation for 2s.

**FR-404: Built-in cooking timers (V1.5)** *(P1, 5 pts)*
> *As a* user, *I want* timers for steps that need them, *so that* I don't burn the food while glancing at my phone.

Acceptance: Timer auto-suggested when step text mentions duration • Persists if user backgrounds the app • Audio + haptic notification on completion.

**FR-405: Voice mode for hands-free cooking (V2)** *(P2, 13 pts)*
> *As a* user with messy hands, *I want to* advance steps and start timers with voice, *so that* I don't touch the phone with onion-coated fingers.

Acceptance: "Hey Fridge, next step" advances • Wake word reliable in kitchen noise • Toggleable on/off.

---

### Definition of Done (all stories)
Code reviewed • Unit tests written (>80% coverage) • Manual QA on iOS + Android • Analytics events instrumented • Accessibility audited • Feature flag in place for gradual rollout.

### Sprint Plan (8-week MVP build)

| Sprint | Stories | Theme |
|--------|---------|-------|
| 1 | FR-101, FR-102 | Input flow |
| 2 | FR-201 | Pantry UX |
| 3 | FR-301, FR-302 | LLM pipeline + nutrition |
| 4 | FR-303, FR-401 | Match score + recipe view |
| 5 | FR-402, FR-403 | Shopping list + sharing |
| 6 | FR-404 | Timers |
| 7 | Polish, performance, analytics |
| 8 | Beta release prep, instrumentation review |

---

## 7. Success Metrics

| Metric | Target (6 months post-launch) | Why it matters |
|--------|------------------------------|----------------|
| Time-to-decision | Under 60 sec (P50) | Core value prop is speed |
| D7 retention | 35%+ | Indicates real utility, not novelty |
| Recipes-cooked / user / week | 2.5+ | Cooked != just viewed; tracked via Cook Mode |
| Recipe satisfaction (post-cook rating) | 4.2/5 average | LLM hallucinations would tank this fast |
| Free-to-Pro conversion | 4-6% | Sustains unit economics at scale |

---

## 8. Risks & Mitigations

| Risk | Likelihood / Impact | Mitigation |
|------|---------------------|------------|
| LLM hallucinates unsafe recipes (raw chicken pairings, allergen miss) | Medium / High | Hard rules layer over LLM output; allergen/safety check pass before display; user-reported flagging |
| LLM API costs scale with usage | High / Medium | Caching for common ingredient combos; smaller model for free tier; batch processing |
| Calorie data inaccurate | Medium / High | USDA-backed DB; explicit "estimate" labeling; per-100g not per-serving for transparency |
| CV model misidentifies ingredients (V2 photo feature) | High / High | Editable chip UI; confidence threshold tuning; rapid retraining loop with user-corrected data |
| User photographs ingredients still in packaging (V2) | High / Medium | OCR layer for branded packaging; onboarding teaches "unpack first" behavior |

---

## 9. Open Questions

- Should V2 photo mode support video input (pan across the fridge) or only single photos? Video is richer but heavier.
- Pricing model: ad-supported free vs freemium with $4.99/mo Pro? Pro unlocks unlimited recipes, voice mode, shopping list export.
- Do we ship with grocery integration day 1 (Zepto API) or wait for retention validation?
- Indian-first or global-first launch? Indian gives sharper differentiation; global gives faster scale.

---

## 10. Timeline (Indicative)

| Phase | Duration | Deliverables |
|-------|----------|--------------|
| Discovery | 2 weeks | User interviews (n=20), competitive analysis, technical feasibility spike |
| Design + Prototype | 3 weeks | Hi-fi Figma prototype, interactive testing with 8 target users |
| Build (MVP) | 8 weeks | LLM pipeline, iOS + Android apps, calorie DB |
| Closed Beta | 3 weeks | 100 users, instrument all 6 success metrics, daily standups on data |
| Public Launch | Week 17 | App Store + Play Store, ProductHunt launch, content marketing kickoff |

---

## 11. Appendix

### Competitive Landscape

- **SuperCook** (web, US) — text-based ingredient input, dated UX, no calorie data, no mobile-first
- **Yummly** (acquired by Whirlpool) — recipe discovery first, not pantry-first; dying product
- **ChatGPT / Claude direct prompting** — works for power users but no CV, no persistence, no calorie accuracy
- **Tasty / NYT Cooking** — content-first, not utility-first; different problem

> **FridgeAI's wedge:** pantry-first + computer vision (V2) + speed. No competitor combines all three for mobile.

---

## Project Outputs

This PRD is part of a complete PM workflow. Other artifacts in this repo:

- 🍳 [Working MVP](./FridgeAI.jsx) — Single-file React app
- 📖 [README](./README.md) — Project overview
- 🗺️ [Roadmap](./README.md#-roadmap) — V2+ planned features

---

*Built as part of an exploration into AI-augmented product management workflows. PRD generated as part of a 90-minute end-to-end PM process.*
