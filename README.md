# NEBULA CDN

### A free, open-source CDN of **2,790+ browser-playable HTML games** served via [jsDelivr](https://www.jsdelivr.com/).

> Built & maintained by **GoatTech Industries**

---

## Quick Links

| Resource | URL |
|----------|-----|
| **Game Catalog (JSON API)** | `https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json` |
| **Player (HTML)** | `https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/player.html` |
| **CDN Base URL** | `https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/` |
| **Single Game Example** | `https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/doom.html` |

---

## What Is This?

NEBULA CDN is a collection of **2,790 self-contained HTML games** — each a single `.html` file that runs entirely in the browser. Every game is:

- **Minified** — comments and tracking code stripped, whitespace collapsed
- **Tagged** — with a NEBULA CDN header comment identifying the source
- **Cataloged** — in a machine-readable `games.json` with full metadata
- **Served Free** — via jsDelivr's global CDN (no rate limits, no API keys)

### Stats

| Metric | Value |
|--------|-------|
| Total Games | 2,790 |
| Total Size | ~1.63 GB |
| Categories | 16 |
| File Format | Single-file HTML |
| CDN | jsDelivr (GitHub-backed) |

### Categories

| Category | Count | Category | Count |
|----------|-------|----------|-------|
| Other | 1,802 | Platformer | 105 |
| Pokemon | 134 | Mario | 104 |
| Sonic | 74 | Racing | 72 |
| Strategy | 72 | Fighting | 71 |
| Sports | 68 | Shooter | 51 |
| Horror | 51 | Puzzle | 46 |
| Minecraft | 41 | Zelda | 37 |
| RPG | 33 | Simulation | 29 |

---

## Using the CDN

### Direct Game URLs

Every game can be loaded directly via jsDelivr:

```
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/{slug}.html
```

**Examples:**

```
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/doom.html
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/2048.html
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/fnaf.html
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/minecraft-tower-defense.html
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/cookie-clicker.html
```

### Embed a Game

```html
<iframe 
  src="https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/2048.html"
  width="800" 
  height="600"
  sandbox="allow-scripts allow-same-origin allow-popups allow-forms allow-modals allow-pointer-lock"
  allowfullscreen>
</iframe>
```

### Open in about:blank (Stealth Mode)

```javascript
const win = window.open('about:blank', '_blank');
win.document.open();
win.document.write(`
  <!DOCTYPE html>
  <html><head><title>Document</title></head>
  <body style="margin:0;overflow:hidden">
    <iframe src="https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/doom.html"
      style="width:100%;height:100vh;border:none"
      sandbox="allow-scripts allow-same-origin allow-popups allow-forms allow-modals allow-pointer-lock"
      allowfullscreen></iframe>
  </body></html>
`);
win.document.close();
```

---

## Scraping the CDN (`games.json` API)

The `games.json` file at the root of this repository is the **single source of truth** for the entire catalog. It's designed to be fetched and consumed by external websites, apps, or scripts.

### Fetch the Catalog

**JavaScript:**

```javascript
const CDN = 'https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main';

async function loadCatalog() {
  const res = await fetch(`${CDN}/games.json`);
  const data = await res.json();
  
  console.log(`Total games: ${data.stats.totalGames}`);
  console.log(`Categories: ${data.categories.join(', ')}`);
  
  // Access all games
  for (const game of data.games) {
    console.log(`${game.name} [${game.category}] - ${CDN}/${game.file}`);
  }
  
  return data;
}
```

**Python:**

```python
import requests

CDN = "https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main"

def load_catalog():
    r = requests.get(f"{CDN}/games.json")
    data = r.json()
    
    print(f"Total games: {data['stats']['totalGames']}")
    
    for game in data["games"]:
        url = f"{CDN}/{game['file']}"
        print(f"{game['name']} [{game['category']}] - {url}")
    
    return data
```

**cURL:**

```bash
curl -s https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json | python3 -m json.tool
```

### Catalog Schema (`games.json`)

```jsonc
{
  "name": "NEBULA CDN",
  "description": "A free, open-source CDN of browser-playable HTML games.",
  "version": "1.0.0",
  "author": "GoatTech Industries",
  "repository": "https://github.com/GoatTech-42/NEBULA-CDN",
  "cdn_base": "https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main",
  "license": "MIT",
  "generated": "2026-04-16T00:00:00Z",
  "stats": {
    "totalGames": 2790,
    "totalSizeBytes": 1709388255,
    "totalSizeMB": 1630.2,
    "categories": {
      "Other": 1802,
      "Pokemon": 134,
      // ...
    }
  },
  "categories": ["Fighting", "Horror", "Mario", "Minecraft", ...],
  "games": [
    {
      "id": "doom",                          // Unique ID (same as slug)
      "name": "doom",                        // Display name
      "slug": "doom",                        // URL-safe slug
      "file": "games/doom.html",             // Relative path from repo root
      "filename": "doom.html",               // Just the filename
      "category": "Shooter",                 // Primary category
      "tags": ["offline","browser","html5","singleplayer"],
      "description": "Play doom - ...",      // Short description
      "size": 123456,                        // File size in bytes
      "originalSize": 234567,                // Original size before minification
      "hash": "abc123def456"                 // SHA-256 hash prefix (16 chars)
    }
    // ... 2790 entries
  ]
}
```

### Game Object Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Unique game identifier (same as slug) |
| `name` | `string` | Human-readable display name |
| `slug` | `string` | URL-safe identifier used in filenames |
| `file` | `string` | Relative path to game file (`games/{slug}.html`) |
| `filename` | `string` | Just the filename (`{slug}.html`) |
| `category` | `string` | Primary category (one of 16 categories) |
| `tags` | `string[]` | Array of tags (e.g., `offline`, `browser`, `retro`) |
| `description` | `string` | Short description of the game |
| `size` | `number` | File size in bytes (after minification) |
| `originalSize` | `number` | Original file size before processing |
| `hash` | `string` | First 16 chars of SHA-256 hash |

---

## Build Your Own Game Portal

Here's a complete example of a minimal external site that scrapes NEBULA CDN and displays a game library:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Game Site - Powered by NEBULA CDN</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: system-ui, sans-serif; background: #111; color: #eee; padding: 2rem; }
    h1 { margin-bottom: 1rem; }
    #search { width: 100%; padding: 0.75rem; margin-bottom: 1rem; background: #222; border: 1px solid #333; color: #eee; border-radius: 8px; font-size: 1rem; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 1rem; }
    .card { background: #1a1a2e; border: 1px solid #333; border-radius: 8px; padding: 1rem; cursor: pointer; transition: border-color 0.2s; }
    .card:hover { border-color: #7c5cfc; }
    .card h3 { font-size: 0.95rem; margin-bottom: 0.3rem; }
    .card p { font-size: 0.8rem; color: #888; margin-bottom: 0.5rem; }
    .card .cat { color: #7c5cfc; font-size: 0.75rem; font-weight: 600; }
    .btns { display: flex; gap: 0.5rem; margin-top: 0.5rem; }
    .btns button { flex: 1; padding: 0.4rem; border: none; border-radius: 6px; cursor: pointer; font-size: 0.8rem; }
    .btn-play { background: #7c5cfc; color: #fff; }
    .btn-stealth { background: #333; color: #ccc; }
    #player { position: fixed; inset: 0; background: rgba(0,0,0,0.9); display: none; z-index: 100; }
    #player iframe { width: 100%; height: 100%; border: none; }
    #close { position: fixed; top: 1rem; right: 1rem; z-index: 101; background: #e74c3c; color: #fff; border: none; padding: 0.5rem 1rem; border-radius: 6px; cursor: pointer; font-weight: 700; }
  </style>
</head>
<body>

<h1>My Game Portal</h1>
<input type="text" id="search" placeholder="Search games...">
<div class="grid" id="grid"></div>
<div id="player"><iframe id="frame"></iframe></div>
<button id="close" style="display:none" onclick="closeGame()">✕ Close</button>

<script>
const CDN = 'https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main';
let games = [];

// 1. Fetch the catalog
fetch(`${CDN}/games.json`)
  .then(r => r.json())
  .then(data => {
    games = data.games;
    render(games);
  });

// 2. Render game cards
function render(list) {
  document.getElementById('grid').innerHTML = list.map(g => `
    <div class="card">
      <div class="cat">${g.category}</div>
      <h3>${esc(g.name)}</h3>
      <p>${esc(g.description)}</p>
      <div class="btns">
        <button class="btn-play" onclick="playGame('${g.slug}')">▶ Play</button>
        <button class="btn-stealth" onclick="stealthGame('${g.slug}')">🔗 Stealth</button>
      </div>
    </div>
  `).join('');
}

// 3. Search
document.getElementById('search').addEventListener('input', e => {
  const q = e.target.value.toLowerCase();
  render(games.filter(g => (g.name + g.category + g.tags.join(' ')).toLowerCase().includes(q)));
});

// 4. Play in iframe overlay
function playGame(slug) {
  const game = games.find(g => g.slug === slug);
  document.getElementById('frame').src = `${CDN}/${game.file}`;
  document.getElementById('player').style.display = 'block';
  document.getElementById('close').style.display = 'block';
}

function closeGame() {
  document.getElementById('frame').src = '';
  document.getElementById('player').style.display = 'none';
  document.getElementById('close').style.display = 'none';
}

// 5. Open in about:blank (stealth mode)
function stealthGame(slug) {
  const game = games.find(g => g.slug === slug);
  const url = `${CDN}/${game.file}`;
  const win = window.open('about:blank', '_blank');
  win.document.write(`<!DOCTYPE html><html><head><title>Document</title></head><body style="margin:0;overflow:hidden"><iframe src="${url}" style="width:100%;height:100vh;border:none" sandbox="allow-scripts allow-same-origin allow-popups allow-forms allow-modals allow-pointer-lock" allowfullscreen></iframe></body></html>`);
  win.document.close();
}

// Escape HTML
function esc(s) { return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

// Close on Escape
document.addEventListener('keydown', e => { if (e.key === 'Escape') closeGame(); });
</script>
</body>
</html>
```

Save this as a single HTML file and open it — it will automatically fetch the catalog from jsDelivr and display all 2,790 games.

---

## Advanced Scraping Examples

### Filter by Category (JavaScript)

```javascript
const data = await fetch('https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json').then(r => r.json());

// Get all Pokemon games
const pokemon = data.games.filter(g => g.category === 'Pokemon');
console.log(`${pokemon.length} Pokemon games found`);

// Get all horror games under 1MB
const smallHorror = data.games.filter(g => g.category === 'Horror' && g.size < 1048576);

// Get all games with 'retro' tag
const retro = data.games.filter(g => g.tags.includes('retro'));

// Get random game
const random = data.games[Math.floor(Math.random() * data.games.length)];
console.log(`Random: ${random.name} (${random.category})`);
```

### Build a Category Index (Python)

```python
import requests
from collections import defaultdict

data = requests.get("https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json").json()

by_category = defaultdict(list)
for game in data["games"]:
    by_category[game["category"]].append(game)

for cat, games in sorted(by_category.items()):
    print(f"\n{cat} ({len(games)} games):")
    for g in games[:3]:
        print(f"  - {g['name']}")
```

### Download All Games (bash)

```bash
# Download the catalog
curl -sL https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json -o games.json

# Download all game files using jq + wget
CDN="https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main"
mkdir -p games
jq -r '.games[].file' games.json | while read file; do
  wget -q "${CDN}/${file}" -O "${file}"
  echo "Downloaded: ${file}"
done
```

### Sync with Changes (bash)

```bash
# Use the hash field to detect changes
CDN="https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main"
curl -sL "${CDN}/games.json" | jq -r '.games[] | "\(.hash) \(.file)"' > remote_hashes.txt

# Compare with local hashes and download only changed files
while read hash file; do
  local_hash=$(sha256sum "$file" 2>/dev/null | cut -c1-16)
  if [ "$hash" != "$local_hash" ]; then
    wget -q "${CDN}/${file}" -O "${file}"
    echo "Updated: ${file}"
  fi
done < remote_hashes.txt
```

---

## File Structure

```
NEBULA-CDN/
├── games/                    # All game HTML files (2,790 files)
│   ├── 2048.html
│   ├── doom.html
│   ├── fnaf.html
│   ├── minecraft-tower-defense.html
│   ├── cookie-clicker.html
│   └── ... (2,785 more)
├── games.json                # Full catalog with metadata (machine-readable)
├── player.html               # Single-file game player/browser (human-friendly)
├── process_games.py          # Build script (rename, minify, catalog)
└── README.md                 # This file
```

---

## How Games Are Named

All game files follow a consistent naming convention:

1. **Prefix stripped** — `cl` prefix from original filenames removed
2. **Lowercased** — all filenames are lowercase
3. **Slugified** — spaces, underscores, and special characters replaced with hyphens
4. **Deduplicated** — duplicate files (e.g., `(1)`, `(2)` suffixes) resolved
5. **Extension normalized** — all files saved as `.html`

**Examples:**

| Original | Renamed |
|----------|---------|
| `clDoom.html` | `doom.html` |
| `cl2048.html` | `2048.html` |
| `clFNAF.html` | `fnaf.html` |
| `clcrashbandicoot (1).html` | `crashbandicoot.html` |
| `MINECRAFTTOWERDEFENSE.html` | `minecraft-tower-defense.html` |
| `clPokémon Emerald Rush Edition (2.0).html` | `pokemon-emerald-rush-edition-2-0.html` |

---

## How Games Are Processed

Each game file goes through this pipeline:

1. **Comment Removal** — HTML comments (`<!-- -->`) are stripped
2. **Tracker Removal** — Google Analytics, Google Tag Manager, and ad scripts removed
3. **Ad Removal** — Sidebar ad divs and related CSS removed  
4. **Whitespace Collapse** — Excessive whitespace, blank lines, and inter-tag spaces collapsed
5. **Header Injection** — NEBULA CDN attribution comment added to line 1
6. **Cataloging** — Metadata (name, category, tags, size, hash) computed and written to `games.json`

### Header Format

Every game file starts with:

```html
<!-- NEBULA CDN | GoatTech Industries | Game: {name} | File: {filename} | https://github.com/GoatTech-42/NEBULA-CDN -->
```

---

## jsDelivr CDN Details

This project uses [jsDelivr's GitHub integration](https://www.jsdelivr.com/features) which:

- Mirrors this GitHub repo at `cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/`
- Provides a global CDN with 750+ PoPs worldwide
- Has **no rate limits** and **no API keys** required
- Supports **permanent versioned URLs** (pin to a commit or tag)
- Caches files for up to 7 days (use `@main` for latest or `@{commit}` for pinned)

### URL Patterns

```
# Latest (follows main branch, cached ~7 days)
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games/{slug}.html

# Pinned to specific commit (permanent, never changes)
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@{commit_hash}/games/{slug}.html

# Minified by jsDelivr (adds .min to supported formats)
https://cdn.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json
```

### Purge Cache

If you need fresh data after an update:

```
https://purge.jsdelivr.net/gh/GoatTech-42/NEBULA-CDN@main/games.json
```

---

## CORS & Security

- All files are served with proper CORS headers by jsDelivr
- Games run in a browser sandbox — use the `sandbox` attribute on iframes:
  ```
  sandbox="allow-scripts allow-same-origin allow-popups allow-forms allow-modals allow-pointer-lock"
  ```
- The about:blank stealth technique opens games in a clean tab with no referrer

---

## Contributing

1. Fork this repository
2. Add game HTML files to the `games/` directory
3. Run `python3 process_games.py` to rename, minify, and rebuild the catalog
4. Submit a pull request

### Game File Requirements

- Must be a **single, self-contained HTML file**
- Should work offline (no external dependencies, or use CDN-hosted assets)
- No malware, crypto miners, or malicious code
- Files larger than 50MB should be avoided

---

## License

MIT License — See [LICENSE](LICENSE) for details.

---

<p align="center">
  <strong>NEBULA CDN</strong> — by <a href="https://github.com/GoatTech-42">GoatTech Industries</a><br>
  Serving 2,790+ games to the world, for free.
</p>
