# Zenith Audio | Modern Web-Based Music Library Manager

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Made with Vanilla JS](https://img.shields.io/badge/Made%20with-Vanilla%20JS-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)

**[🔴 View the Live Demo Here](https://erfan2255.github.io/zaudio/)**

Zenith Audio is a beautiful, responsive, and highly customizable web platform for managing and curating your personal music library. Built with performance in mind, it leverages Vanilla JavaScript and Firebase to deliver seamless cloud syncing, advanced filtering, and a modern user interface without the overhead of heavy front-end frameworks.

## 🚀 Key Features

* **Real-Time Cloud Sync:** Integrated Firebase Authentication and Firestore to save user history, favorites, and library statistics across all devices.
* **Advanced Curation & Filtering:** Instantly sort tracks by newest arrivals, oldest releases, global popularity, or personal favorites.
* **Custom UI/UX:** Features a sleek glassmorphism design with seamless toggling between Dark and Light modes.
* **Dual View Modes:** Switch between an immersive Grid View for album art and a compact List View for quick scanning.
* **Seamless Playback Integration:** One-click redirection to YouTube Music or standard YouTube for instant audio playback.
* **Global Statistics:** Track global song popularity and view personal library metrics (total tracks, loved songs, and listening history).
* **Data Export:** Easily export your curated favorite lists and listening history directly to a `.txt` file.

## 🕷️ Data Pipeline & Web Scraping

Zenith Audio uses an automated Python pipeline (designed for Google Colab) to scrape new tracks from radio stations (Capital FM 🇬🇧 and Pulse 87 🇺🇸), update play counts, and fetch metadata via the iTunes API. The scripts are included in this repository as `.txt` files.

### The 3-Step Update Workflow:

**Step 1: Scrape & Merge (Script A)**
1. Copy the code from `Script A (Weekly Listener).txt` into a Google Colab cell. 
2. Upload your current `global_music_library_COMPLETE.json`. 
3. Run the script. It will auto-clean duplicates, update play counts, scrape fresh radio data, and output a `new_songs_to_review.txt` and a `global_music_library_PENDING.json`.

**Step 2: AI Metadata Cleaning**
Take the contents of `new_songs_to_review.txt` and use an LLM (like ChatGPT or Gemini) to clean the formatting. **Use the following exact prompt:**

> **Role:** You are an expert music metadata cleaner and formatter. I have a list of songs that needs to be processed for a specific script (Script B) used in my Zenith Audio Player.
> 
> **Task:** Clean, deduplicate, and format the provided list of songs while strictly preserving the pipe symbol (|) and country flags at the end of each line.
> 
> **Instructions & Rules:**
> 1. **Strip Remix Tags:** Remove all remix, edit, or version tags from the titles. Delete anything like (Radio Edit), (Ryan Riback Remix), [Pete Down Remix), etc. Leave only the base artist and the original song title.
> 2. **Fix Formatting & Typos:** Ensure the format is strictly `Artist - Title | [Flag]`. If an entry is backwards, flip it. Correct minor, obvious typos.
> 3. **Merge Duplicates:** Consolidate any duplicate tracks into a single line.
> 4. **The "Secret Bridge" Rule (CRITICAL):** Do NOT delete the `| 🇬🇧` or `| 🇺🇸` at the end of the line. Everything before the pipe searches iTunes; everything after attaches the correct flag in the Zenith Audio Player.
> 5. **Combine Flags on Duplicates:** If merging duplicate songs that have different flags, combine the flags on the final line like this: `| 🇬🇧 🇺🇸`.
> 6. **Alphabetical Sort:** Sort the final output alphabetically by the Artist's name.
> 
> **Output Format:** Output the final list. Do not use bullet points, numbers, or any extra punctuation. Give me just the raw list.
> 
> **Example of the Secret Bridge Rule:**
> ❌ *Original:* The Weeknd (Radio Edit) - Starboy | 🇬🇧 -> *Bad Fix:* The Weeknd - Starboy 
> ✅ *Original:* The Weeknd (Radio Edit) - Starboy | 🇬🇧 -> *Good Fix:* The Weeknd - Starboy | 🇬🇧 
> 
> Here is the list to process: 
> [PASTE YOUR UNFORMATTED SONG LIST HERE]

**Step 3: Fetch Metadata & Finalize (Script B)**
1. Copy the code from `Script B (New Release Publisher).txt` into Google Colab. 
2. Upload your `global_music_library_PENDING.json`. 
3. Paste the AI-cleaned song list directly into the script variable. 
4. Run the script to fetch iTunes metadata (album art, release dates) and generate your new, finalized `global_music_library_COMPLETE.json`.

## 🛠️ Tech Stack

* **Front-End:** HTML5, CSS3 (Custom Properties, CSS Grid/Flexbox), Vanilla JavaScript (ES6 Modules)
* **Back-End & Database:** Firebase v11 (Auth, Firestore)
* **Icons & Fonts:** FontAwesome 6.4.2, Google Fonts (Outfit)
* **Data Pipeline:** Python, BeautifulSoup4, Requests, iTunes Search API

## ⚙️ Setup and Installation

To run this project locally or deploy it to your own server, follow these steps:

1. **Clone the repository:**
    git clone [https://github.com/erfan2255/zaudio.git](https://github.com/erfan2255/zaudio.git)
    cd zaudio

2. **Configure Firebase:**
    Open `index.html` and locate the `firebaseConfig` object. Replace the placeholder values with your own Firebase project credentials:
    const firebaseConfig = {
        apiKey: "YOUR_API_KEY",
        authDomain: "YOUR_AUTH_DOMAIN",
        projectId: "YOUR_PROJECT_ID",
        storageBucket: "YOUR_STORAGE_BUCKET",
        messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
        appId: "YOUR_APP_ID"
    };

3. **Provide Music Data:**
    Ensure you have a valid `global_music_library_COMPLETE.json` file in the root directory to populate the application's music grid.

4. **Launch the Application:**
    Because the app fetches local JSON data and uses ES6 modules, you must serve it via a local web server (e.g., VS Code Live Server or Python's `http.server`).
    python -m http.server 8000

## ⌨️ Keyboard Shortcuts

* `/` : Focus search bar
* `L` : Toggle between Grid and List view

## 👨‍💻 Author

**Erfan Shariati**
* GitHub: [@erfan2255](https://github.com/erfan2255)
* Live Project: [Zenith Audio](https://erfan2255.github.io/zaudio/)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
