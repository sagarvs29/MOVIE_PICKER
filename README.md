# 🍿 Gemini Cine-Pilot

A lightweight, single-page, client‑side movie suggestion tool that reduces “choice fatigue” by giving you **one** high‑quality pick per genre request. It uses the **Gemini 2.5 Flash Preview** model with **Google Search grounding** to keep recommendations fresh (title + year, IMDb-style rating string, compact review sentiments, mood tag, and streaming availability).

> ⚠️ This is a demo / learning project. The current implementation exposes an API key in client code; don’t deploy as‑is to production. Use a server or proxy to protect secrets.

---
## Table of Contents
1. [Features](#-features)
2. [How It Works](#-how-it-works)
3. [Mood → Visual Mapping](#-mood--visual-mapping)
4. [Prerequisites](#-prerequisites)
5. [Local Setup](#-local-setup)
6. [Configuration](#-configuration)
7. [Security Notes](#-security-notes)
8. [Project Structure](#-project-structure)
9. [Extensibility Ideas / Roadmap](#-extensibility-ideas--roadmap)
10. [Troubleshooting](#-troubleshooting)
11. [Disclaimers](#-disclaimers)

---
## ✨ Features
- **Real-Time Grounded Suggestions:** Gemini model + Google Search tool for current streaming availability & ratings context.
- **Structured Output Contract:**
  1. Movie Title (with Year)
  2. IMDb-style Rating string (e.g. `8.4/10`)
  3. Two-sentence synthesized positive review sentiment
  4. Primary Mood Tag (`Action | Romance | Horror | Sci-Fi | Fantasy | Comedy | Drama`)
  5. Streaming availability summary (e.g. “Available on Netflix and Max”)
- **Dynamic 3D Background:** Three.js particle field adapts color palette, rotation speed, and particle size to the Mood Tag.
- **Single-File Simplicity:** Everything lives in `index.html` for fast iteration.
- **Retry Logic Placeholder:** Exponential backoff framework ready for handling transient API errors / rate limits.
- **Follow-Up Suggestion Logic:** Subsequent clicks request a *different* movie from the prior result.

---
## 🧠 How It Works
1. User enters a genre (free text).
2. A structured system prompt instructs Gemini to return exactly five parts separated by double newlines.
3. Response text is parsed into title, rating, review sentences, mood tag, and streaming availability.
4. Mood tag drives Three.js scene mutations (colors, speed, particle size).
5. Grounding metadata (if present) is turned into clickable source links.

---
## 🎨 Mood → Visual Mapping
| Mood Tag | Particle Color | Background | Speed | Size |
|----------|----------------|------------|-------|------|
| Action   | Orange-Red     | Deep Crimson | Fast ↑ | Large |
| Romance  | Light Salmon   | Soft Violet  | Very Slow ↓ | Small |
| Horror   | Green Glow     | Black        | Minimal | Medium |
| Sci-Fi   | Cyan           | Dark Navy    | Medium | Medium |
| Fantasy  | Goldenrod      | Deep Purple  | Medium | Larger |
| Comedy   | Yellow         | Dark Gray    | Fast | Medium |
| Drama    | Light Gray     | Dark Gray    | Slow | Medium |
| Default / Unknown | Blue | Dark Blue | Normal | Medium |

---
## ✅ Prerequisites
- Modern browser (Chrome, Edge, Firefox, Safari) with ES6 module support.
- Gemini API key from Google AI Studio.
- (Optional) Local web server for cleaner module handling (VS Code Live Server, `python -m http.server`, etc.).

---
## 🚀 Local Setup
1. **Clone:**
   ```powershell
   git clone <YOUR_REPO_URL>
   cd "movie picker"
   ```
2. **Get API Key:** From Google AI developer portal.
3. **Insert Key:** In `index.html`, find `const apiKey = "YOUR_GEMINI_API_KEY_HERE"` and replace with your key.
4. **Open Locally:**
   - Option A: Just double-click `index.html` (may work; some browsers enforce stricter module rules).
   - Option B (recommended): Serve via a local server.
     ```powershell
     # Example using VS Code Live Server extension (GUI)
     # Or using Node's 'serve' if installed:
     npx serve .
     ```
5. Enter a genre (e.g. `Sci-Fi`, `Romantic Comedy`, `Horror`) and click **Pick a Movie!**

---
## 🔧 Configuration
| Setting | Location | Purpose |
|---------|----------|---------|
| `apiKey` | `index.html` | Gemini authentication (replace placeholder) |
| System Prompt | `systemPrompt` constant | Enforces output format contract |
| Model Endpoint | `apiUrl` | Chooses model + includes key |
| Particle Defaults | Three.js init block | Base visual style before mood adaptation |

**Changing the Model:** Update `gemini-2.5-flash-preview-09-2025` in `apiUrl` to a stable or newer released model when available.

---
## 🔐 Security Notes
- **Do NOT expose production keys client-side.** Anyone can extract them from shipped JS.
- Use a backend proxy that injects the API key server-side and rate limits requests.
- Consider adding request signing, per-user quotas, and logging.
- Firebase initialization is optional—if credentials absent, app gracefully proceeds without persistence.

---
## 📁 Project Structure
```
movie picker/
├── index.html    # Main application (UI, logic, Three.js scene, API calls)
└── README.md     # Project documentation
```

---
## 🧩 Extensibility Ideas / Roadmap
| Idea | Benefit | Complexity |
|------|---------|-----------|
| Local Storage cache | Avoid repeat API calls for same genre | Low |
| Server proxy for API key | Security & usage metrics | Medium |
| Genre suggestions dropdown | UX guidance | Low |
| Accessibility improvements (ARIA, focus states) | Inclusivity | Medium |
| Unit tests (parsing, mood mapping) | Reliability | Medium |
| Streaming API integration (JustWatch / TMDB) | More accurate availability | Medium/High |
| Watchlist & history (Firebase/IndexedDB) | Personalization | Medium |
| Internationalization (i18n) | Global reach | Medium |
| Sentiment analysis validation | Output trustworthiness | High |

---
## 🛠️ Troubleshooting
| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| 403 Forbidden | Invalid or missing API key | Replace placeholder with real key |
| Empty suggestion | Parsing failed due to unexpected model output | Re-try; adjust system prompt formatting |
| Mood not changing | Mood tag not recognized | Ensure model returns one of specified tags |
| Slow loading | Network latency / model cold start | Retry; add spinner feedback (already present) |
| Grounding links missing | Model didn’t provide attribution metadata | This is normal for some responses |

---
## 📜 Disclaimers
- Ratings and streaming availability may be approximate; always verify on official platforms.
- The app synthesizes “Top Reviews” sentiment—it does **not** quote verbatim critic text.
- Mood visualization is aesthetic only and not an authoritative genre classification.
- Demo code provided *as-is* without warranty; suitability for production requires additional hardening.

---
## ✅ Quick Command Recap (Optional)
```powershell
# Clone
git clone <YOUR_REPO_URL>
cd "movie picker"

# (Optional) Serve locally
npx serve .
```
Open the served URL in your browser, enter a genre, enjoy the suggestion.

---
## ❓ FAQ
**Q: Why only one movie per query?** To reduce decision fatigue; you can ask for another.
**Q: Can I extend for multiple results?** Yes—modify parsing to accept a list and redesign the UI grid.
**Q: Why is the API key inline?** Simplicity for local demos—replace with a secure proxy for real use.

---
Feel free to customize, fork, and experiment. If you add a backend proxy or new visual modes, document them here.

<!-- License intentionally omitted (unspecified). Add one (e.g., MIT) if you plan to share publicly. -->
