# GameGuide 1.0 🎮

**Find your next favorite game with recommendations tailored to your taste.**

GameGuide is a personalized game recommendation website. It learns from the games you like, dislike, and play, scores games with an explainable algorithm, and shows why each game was recommended.

Features include Steam login and library sync, email accounts, friend matching by gaming taste, a Google Gemini AI chat, and automatic adult-content filtering.

## Table of Contents

- [Features](#features)
- [Technologies and Services](#technologies-and-services)
- [Requirements](#requirements)
- [Installation and Setup](#installation-and-setup)
- [Configuration](#configuration)
- [Populate the Game Database](#populate-the-game-database)
- [Run GameGuide](#run-gameguide)
- [How Recommendations Work](#how-recommendations-work)
- [Data Storage and Privacy](#data-storage-and-privacy)
- [Project Structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Known Limitations](#known-limitations)
- [Data Sources and Credits](#data-sources-and-credits)

## Features

- **Personalized recommendations:** See a match percentage and a plain-English explanation for each recommendation.
- **Learns from your feedback:** Liking or disliking a game changes your genre preferences and future recommendations.
- **Onboarding wizard:** Choose favorite genres, mark games you have played, and rate games.
- **Discover page:** Search and filter by genre or store, and sort by recommendation, rating, release date, or name.
- **Game details:** View game information, store links, and similar-game suggestions.
- **My Games library:** Keep track of played, liked, disliked, and wishlisted games.
- **Steam integration:** Sign in with Steam and synchronize your owned games.
- **Email accounts:** Save your profile on the server and access it across browsers.
- **Friends:** Find and add players with similar gaming tastes.
- **GameGuide AI chat:** Uses Google Gemini to recommend games that exist in the project database.
- **Adult-content filter:** Automatically checks games imported into the catalog.

## Technologies and Services

| Component | Technology | Purpose |
|---|---|---|
| Frontend | HTML, CSS, JavaScript | Website interface; no frontend framework or build step |
| Backend | Node.js, Express, `express-session` | Server routes, sessions, and protected API requests |
| Storage | JSON files and browser `localStorage` | Game catalog, account data, and guest profiles |
| Game data | Steam Store API | Game details and Steam library features |
| Game discovery | IGDB via Twitch API | Additional game information and discovery |
| Optional image source | RAWG API | Alternative game cover images |
| AI | Google Gemini API | AI game chat |

## Requirements

Install these applications before starting:

1. **Node.js 18 or newer** — required to run the server and project scripts. Download it from [nodejs.org](https://nodejs.org/).
2. **A web browser** — Chrome, Firefox, Edge, Safari, or another modern browser.
3. **A terminal** — PowerShell or Windows Terminal on Windows, or Terminal on macOS/Linux. The terminal in Visual Studio Code also works.
4. **A code editor (recommended)** — for example, [Visual Studio Code](https://code.visualstudio.com/), to edit files and configure `.env`.
5. **Optional API keys** — needed only for features such as Steam integration, IGDB discovery, RAWG image fetching, and Gemini AI chat.

Check that Node.js and npm are installed:

```bash
node -v
npm -v
```

Node.js should report version 18 or newer.

## Installation and Setup

### 1. Get the project files

If the project is hosted on GitHub and you have Git installed, clone your repository:

```bash
git clone <YOUR-REPOSITORY-URL>
cd <YOUR-REPOSITORY-FOLDER>
```

Replace the placeholders with your actual repository URL and folder name.

Alternatively, on GitHub choose **Code → Download ZIP**, extract the ZIP file, and open the extracted project folder in your terminal or editor.

Make sure you are in the main project folder containing `package.json` before running the commands below.

### 2. Install dependencies

```bash
npm install
```

This installs the Node.js packages the project needs.

### 3. Create your environment file

Copy `.env.example` to `.env`.

**Windows PowerShell:**

```powershell
Copy-Item .env.example .env
```

**macOS/Linux:**

```bash
cp .env.example .env
```

Open `.env` in your editor and add the settings you need. Keep this file private: **never commit `.env`, API keys, or secrets to GitHub.**

If your project folder does not contain `.env.example`, create `.env` manually and add the configuration variables listed below.

## Configuration

All server configuration is read from `.env`. The website can start without optional API keys, but features that depend on those keys will be unavailable or limited.

| Variable | Used for | Required? |
|---|---|---|
| `SESSION_SECRET` | Protecting login sessions; use a long, random value | Recommended for account sessions |
| `PORT` | Server port; defaults to `3000` | Optional for local use |
| `BACKEND_URL` | Backend address; defaults to `http://localhost:3000` | Usually optional locally |
| `STEAM_API_KEY` | Steam login and library synchronization | Only for Steam features |
| `AI_API_KEY` | Google Gemini AI chat | Only for AI chat |
| `AI_MODEL` | Selects the Gemini model; the project provides a default | Optional |
| `IGDB_CLIENT_ID` | IGDB access | Only for IGDB features |
| `IGDB_CLIENT_SECRET` | IGDB access | Only for IGDB features |
| `RAWG_API_KEY` | Alternative source for cover images | Only for RAWG image fetching |

Get keys from the relevant providers:

- Steam: [Steam Web API key](https://steamcommunity.com/dev/apikey)
- Google Gemini: [Google AI Studio](https://aistudio.google.com/apikey)
- IGDB: [Twitch Developer Console](https://dev.twitch.tv/console/apps)
- RAWG: [RAWG API](https://rawg.io/apidocs)

Provider requirements and access rules may change. Follow each provider's current instructions. If `AI_API_KEY` is not configured, the AI page can still show top matches with a notice that AI is not connected.

## Populate the Game Database

The catalog at `public/data/games.json` starts empty. Add games before using the discovery and recommendation pages.

Run these commands from the project folder:

```bash
npm run seed
npm run add-manual-games
```

| Command | What it does | API key needed? |
|---|---|---|
| `npm run seed` | Imports a batch of popular games from Steam, including details | No |
| `npm run discover` | Imports a batch of highly rated games from IGDB | Yes, IGDB credentials |
| `npm run add-manual-games` | Adds a set of manually defined games from platforms without a public API integration | No |
| `npm run fetch-images` | Replaces placeholder covers with real images where possible | IGDB or RAWG may be needed |
| `npm run clean-content` | Re-checks saved games for adult content | No |
| `npm run add-test-friend` | Creates a test account for trying the Friends page | No |

You can run import commands again later to add more games. Existing games are skipped by the seed process.

A recommended first setup is:

```bash
npm run seed
npm run add-manual-games
npm run fetch-images
```

The last command may need IGDB or RAWG credentials. You can also connect Steam from the **Profile** page and select **Sync Steam Library** to import games you own.

## Run GameGuide

Start the local server:

```bash
npm start
```

Then open this address in your browser:

**http://localhost:3000**

Keep the terminal open while using the website. To stop the server, press **Ctrl+C** in the terminal.

After editing frontend HTML, CSS, or JavaScript files, refresh the browser. After changing `.env`, restart the server.

### First-time use

1. Open `http://localhost:3000`.
2. Complete onboarding by selecting favorite genres and rating or marking games you have played.
3. Visit **Discover** or **Recommendations** to explore games.
4. Like or dislike games to personalize future suggestions.
5. Optionally register an email account and connect Steam from the **Profile** page.
6. Configure `AI_API_KEY` in `.env` and restart the server if you want to use the Gemini AI chat.

## How Recommendations Work

The recommendation algorithm is implemented in `public/js/recommend.js`. Each game receives a score from 0 to 100 based on:

| Factor | Weight | Meaning |
|---|---:|---|
| Genre match | 40% | How well the game's genres match saved preferences |
| Similarity to liked games | 30% | Genre overlap with games you liked |
| Game rating | 20% | The game's own rating |
| Novelty | 10% | A bonus for indie “hidden gems” |

A penalty of up to 20 points can be applied to games similar to ones you disliked. The penalty is capped so disliking one game does not eliminate an entire genre from consideration.

Genre preferences start at a neutral value of 20. Genres selected during onboarding start at 85; each like increases related preferences by 8, and each dislike decreases them by 4. Games marked as already played are excluded from the Home and Recommendations pages.

## Content Filtering

Automatic imports from Steam, IGDB, and the project's import scripts apply an adult-content filter. The filter checks available content descriptors, warnings, genres, categories, keywords, and age requirements, depending on the data source.

No automatic filter is perfect. If a game is incorrectly included or excluded, review its entry in `public/data/games.json`. Run `npm run clean-content` to re-check the saved catalog.

## Data Storage and Privacy

| Data | Stored in |
|---|---|
| Game catalog | `public/data/games.json` on the server |
| Accounts, password hashes, and friends | `users.json` on the server |
| Guest profile preferences | Browser `localStorage` |
| Logged-in profile | Browser `localStorage` and the user's server account |

Passwords are hashed using `scrypt` with a random salt per account; they are not stored as plain text.

Steam login and AI chat require secret keys, so those requests are handled by the backend rather than exposing the keys in browser code.

**Security reminders:**
- Never commit `.env`, `users.json`, or other private account data.
- Do not publish API keys or session secrets.
- Use a unique, long `SESSION_SECRET` for your own setup.

## Project Structure

```text
server.js                  Express server and static files
auth.js                    Email registration, login, and logout
steam.js                   Steam login, library fetching, content filter
friends.js                 Taste similarity and friends list
ai.js                      Gemini chat
gamesDb.js                 Reads/writes public/data/games.json
usersDb.js                 Reads/writes users.json and hashes passwords

seed-steam-games.js        npm run seed
discover-games.js          npm run discover
manual-games.js            npm run add-manual-games
fetch-real-images.js       npm run fetch-images
clean-adult-content.js     npm run clean-content
add-test-friend.js         npm run add-test-friend

public/
  index.html
  discover.html
  game.html
  recommendations.html
  mygames.html
  genres.html
  profile.html
  onboarding.html
  ai.html
  login.html
  friends.html
  404.html
  css/style.css
  data/games.json
  js/
    store.js
    recommend.js
    gamecard.js
    nav.js
    home.js
    discover.js
    game.js
    onboarding.js
    profile.js
    ai.js
```

For a plain-English explanation of individual files, see [`PROJECT_GUIDE.md`](PROJECT_GUIDE.md) if it is included in your repository.

## Troubleshooting

| Problem | Possible fix |
|---|---|
| `node` or `npm` is not recognized | Install Node.js 18 or newer and reopen your terminal. |
| The website has no games | Run `npm run seed` and `npm run add-manual-games`. |
| Steam sync returns no games | Set Steam **Game details** privacy to **Public** and check your API key. |
| Steam login fails | Check `STEAM_API_KEY` and confirm `BACKEND_URL` matches the address you use. |
| The AI says it is not connected | Set `AI_API_KEY` in `.env` and restart the server. |
| `npm run discover` fails | Check IGDB credentials and the Twitch Developer account requirements. |
| Cover images are placeholders | Configure IGDB or RAWG, then run `npm run fetch-images`. Some games may need manual fixes. |
| The Friends page is empty | Add another account or use the test-friend script. |
| Registration says the email already exists | Log in with that account, or remove local test data if you intentionally want to reset it. |

## Known Limitations

- Epic Games Store login and library sync are not supported; Epic-only games can be added manually.
- Some manually added non-Steam game details may need to be checked.
- Data is stored in JSON files, which is suitable for personal use or small groups but may not scale to many simultaneous users.
- Adult-content filtering is automatic and may occasionally make mistakes.

## Data Sources and Credits

Game data and images may come from the [Steam Store](https://store.steampowered.com/), [IGDB](https://www.igdb.com/), and [RAWG](https://rawg.io/). The AI chat uses [Google Gemini](https://ai.google.dev/).

If you publish or distribute this project, follow each provider's terms and attribution requirements.

---

**GameGuide — find games that fit your taste and discover something new.**
