# 🍳 FridgeAI

> **Snap your fridge. Get dinner. Zero food waste.**

FridgeAI is an AI-powered cooking assistant that turns whatever you have in your fridge into actual dinner recipes — with calorie counts, step-by-step instructions, and an auto-generated shopping list for missing ingredients.

Built with React + Claude API. No more "I have ingredients but nothing to cook."

---

## ✨ Features

- ⌨️ **Type your ingredients** — Quick text input, comma-separated or one per line
- 🥄 **Smart pantry check** — Asks about staples you probably have (salt, oil, ginger, garlic, spices)
- 🍲 **3 personalized recipes** — Ranked by how much of the ingredients you already have
- 📊 **Real nutrition data** — Calories, protein, time, and difficulty per recipe
- 🛒 **Auto-generated shopping list** — One tap to share via WhatsApp or copy
- 💰 **Cost estimate** — Approximate total for missing ingredients
- 📸 **Photo recognition** — *Coming in V2* — snap your fridge for auto-detection

---

## 🎬 Demo Flow

```
Home → Type Ingredients → Pantry Check → 3 Recipes → Detail View → Shopping List
```

| Screen | What happens |
|--------|-------------|
| **Home** | User starts the flow (photo input shown as "Coming soon" — V2) |
| **Text input** | User types comma-separated ingredient list |
| **Pantry check** | App asks about 12 common staples — tap what you have |
| **Recipes** | 3 AI-generated recipes ranked by ingredient match |
| **Recipe detail** | Full ingredients (✓ have / 🛒 missing), step-by-step, calorie info |
| **Shopping list** | Missing items, cost estimate, copy or share to WhatsApp |

---

## 🛠️ Tech Stack

- **Frontend:** React 18 + Tailwind CSS
- **Icons:** Lucide React
- **AI:** Anthropic Claude API (`window.claude.complete` for in-browser inference)
- **Architecture:** Single-file React component, fully client-side, no backend required

---

## 🚀 Quick Start

### Option 1: Run in Claude Artifacts (easiest)
Copy `FridgeAI.jsx` into a Claude artifact. It runs immediately — no setup.

### Option 2: Run locally with Vite

```bash
# Clone the repo
git clone https://github.com/simranbathija/FridgeAI.git
cd fridgeai

# Install
npm install

# Run dev server
npm run dev
```

Open `http://localhost:5173` in your browser.

### Option 3: Deploy to Vercel
```bash
npm run build
vercel deploy
```

---

## 🧠 How It Works

### 1. Ingredient Input
The user types their available ingredients (comma-separated). The text is parsed, normalized (capitalization, whitespace), and converted to a structured list. *Photo-based ingredient detection is planned for V2 — see roadmap.*

### 2. Pantry Staples Check
Instead of forcing the user to list every spice, FridgeAI asks: *"Do you have these basics?"* — salt, oil, garlic, ginger, cumin, etc. This dramatically improves recipe quality without input fatigue.

### 3. Recipe Generation
A structured prompt sends Claude:
- All confirmed ingredients (detected + manual + pantry staples)
- Request for 3 diverse recipes
- Required JSON schema with match percentage, missing ingredients, calories, steps

Claude returns recipes ranked by `match_percent` — recipes using only what you have score 95-100%; recipes needing 1-2 extra items score 75-90%.

### 4. Shopping List
For any recipe with missing ingredients, one tap builds a clean shopping list with quantity estimates and approximate cost. Users can copy it or share via WhatsApp.

---

## 📁 Project Structure

```
fridgeai/
├── src/
│   ├── FridgeAI.jsx          # Main component (single-file app)
│   ├── App.jsx               # Entry wrapper
│   └── main.jsx              # React DOM mount
├── public/
│   └── ...
├── package.json
├── tailwind.config.js
├── README.md
└── LICENSE
```

---

## 🎯 What I Learned Building This

- **Scope is a feature**: I started with photo-based ingredient detection but ran into reliability issues in sandboxed environments. Rather than ship something flaky, I scoped it to V2 and made text input rock-solid first. Better to ship something small that works than something big that doesn't.
- **Prompt engineering for structured output**: Forcing Claude to return strict JSON (no markdown, no preamble) requires careful prompt design + a `replace(/```json|```/g, '')` cleanup step.
- **State machine UX**: A multi-screen flow needs clear state transitions. `screen` state + conditional rendering > React Router for prototype apps.
- **Reducing input fatigue**: The pantry-staples step came out of testing — users hated typing "salt, oil, pepper" every time. Tap-to-confirm is 10x faster.
- **Match scoring**: Sorting recipes by ingredient overlap (high to low) is the difference between "useful" and "annoying" — recipes you can cook *now* should always be on top.
- **Clipboard API gotchas**: `navigator.clipboard.writeText()` silently fails in sandboxed iframes. Built a 3-tier fallback chain (modern API → `execCommand` → alert) to handle every browser environment.

---

## 🗺️ Roadmap

- [ ] **Photo recognition (V2)** — Snap your fridge, Claude vision identifies ingredients automatically. Currently scoped out due to vision API constraints in sandboxed environments.
- [ ] Save favorite recipes locally
- [ ] Voice mode for hands-free cooking
- [ ] Multi-day meal planning
- [ ] Real grocery API integration (Zepto / Blinkit / Instacart)
- [ ] Dietary preference profiles (veg, vegan, jain, halal, keto)

---

## 🤝 Contributing

PRs welcome! Please open an issue first for any major changes.

---

## 📜 License

MIT — do whatever you want with it.

---

## 🙋 About

Built by Simran Bathija(https://www.linkedin.com/in/simran-bathija-1b9317123/) as part of an exploration into AI-augmented product workflows.

If this helped you, a ⭐ on GitHub means a lot.
