# GameGuide 🎮 

**Discover your next favorite game with personalized recommendations.**

GameGuide is a game recommendation website that learns from the games you like, dislike, and play. Each recommendation includes a match score and an explanation of why the game may suit your taste.

## Features

- **Personalized recommendations** with match percentages and clear explanations.
- **Adaptive preferences** that change as you like or dislike games.
- **Game discovery** with search, genre and store filters, and sorting.
- **Game details** with store links and similar-game suggestions.
- **Personal library** for played games, likes, dislikes, and your wishlist.
- **Steam integration** for login and library synchronization.
- **Accounts** to keep your profile synced across browsers.
- **Friend matching** to find players with similar gaming tastes.
- **AI game chat** powered by Google Gemini, recommending games in the database.
- **Adult-content filtering** for automatically imported games.

## Tech Stack

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Node.js, Express, express-session
- **Storage:** JSON files and browser `localStorage`
- **Game data:** Steam Store API, IGDB, and optional RAWG
- **AI:** Google Gemini API

## Getting Started

### Requirements

- [Node.js](https://nodejs.org/) 18 or newer
- A modern web browser
- Optional API keys for Steam, Gemini, IGDB, or RAWG features

### Installation

```bash
npm install
cp .env.example .env
```

Edit `.env` and add the configuration values for the features you want to use. Keep your API keys and secrets private, and never commit your `.env` file.

### Populate the Game Catalog

The game catalog starts empty. You can add games using the available scripts:

```bash
npm run seed
npm run add-manual-games
```

To fetch real cover images, configure IGDB or RAWG and run:

```bash
npm run fetch-images
```

### Run the Website

```bash
npm start
```

Then open **http://localhost:3000** in your browser.

## Recommendation System

GameGuide scores games from 0 to 100 using:

- **40%** — Genre preference match
- **30%** — Similarity to games you like
- **20%** — Game rating
- **10%** — Novelty bonus for indie hidden gems

Games similar to ones you dislike can receive a penalty of up to 20 points. Your preferences adapt as you rate games, helping future recommendations better reflect your taste.

## Configuration

Configuration is managed through `.env`. Depending on which features you want to enable, you may configure:

- `SESSION_SECRET` — session security
- `STEAM_API_KEY` — Steam login and library sync
- `AI_API_KEY` and `AI_MODEL` — Gemini-powered chat
- `IGDB_CLIENT_ID` and `IGDB_CLIENT_SECRET` — game discovery and cover images
- `RAWG_API_KEY` — alternative source for cover images

The website can run without these optional API keys, but the related features will be limited.

## Project Documentation

For detailed setup instructions, available scripts, project structure, data handling, troubleshooting, and implementation details, see the full project documentation in the repository.

## Notes

- The game database is stored in `public/data/games.json`.
- Account data is stored in `users.json`; do not commit private user data.
- The JSON-file storage approach is intended for personal use or small groups and may not suit a large number of simultaneous users.
- Automatic content filtering may not catch every case.

## Data Sources & Credits

Game data and images may come from [Steam](https://store.steampowered.com/), [IGDB](https://www.igdb.com/), and [RAWG](https://rawg.io/). The AI chat uses [Google Gemini](https://ai.google.dev/). Follow each provider's terms and attribution requirements if you publish or distribute the project.

---

*Find games that fit your taste. Play something new.*
