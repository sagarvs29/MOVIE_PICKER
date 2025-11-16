🍿 Gemini Cine-Pilot

A lightweight, client-side movie suggestion tool that eliminates "choice fatigue" by providing one high-quality pick per genre request. It uses the Gemini API with Google Search grounding for real-time data and dynamic visualization.

✨ Core Features & Technical Breakdown

Aspect

Technology/Output

Description

Data Engine

Gemini API (gemini-2.5-flash-preview-09-2025)

Fetches current, real-time movie context using Google Search grounding.

Output Contract

Structured Multi-part Prompt

Consistently returns Title, IMDb Rating, two summary reviews, and Streaming Availability.

Dynamic UI

Three.js

The background particle system dynamically adjusts its color, speed, and density to match the recommended movie's Mood Tag.

Frontend

HTML5, Tailwind CSS, Vanilla JavaScript

The entire application logic and UI are contained within a single index.html file.

🚀 How to Run Locally

Prerequisites

Modern browser (with ES6 module support).

Your own Gemini API Key.

A local web server (e.g., VS Code Live Server).

Setup Steps

Clone the Repository:

git clone [YOUR_REPO_URL]
cd gemini-cine-pilot


Insert API Key: Open index.html and replace the placeholder value with your key:

const apiKey = "YOUR_GEMINI_API_KEY_HERE";


Launch Server: Open the index.html file using your local web server tool.

🔐 Security Warning (Crucial)

This is a demo project. The current implementation stores the API key directly in the client-side JavaScript, which means it is exposed to anyone who views the page source.

Do NOT deploy this code to a public environment. For production, you must use a secure backend proxy to protect your API key.
