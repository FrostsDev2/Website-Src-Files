# Website Source Files

The CDN-backed game library used by the site's catalog — every game lives in this repo as plain static files and is served straight from jsDelivr. No build step, no server, no dependencies: a game is a folder, and `catalog.json` is the list that tells the site what those folders are.

---

## At a glance

| | |
|---|---|
| **Games in the repo** | 485 folders |
| **Catalog entries** | 487 |
| **Added in the latest update** | **+335 games** (334 new catalog entries) |
| **Last updated** | **November 13, 2026** |
| **Host** | jsDelivr — `https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/` |
| **Build step** | none |

---

## Latest update — November 13, 2026

A large import from the **UGS-Assets** collection:

- **335 new game folders** added (the library grew from 150 → 485 folders).
- **334 new entries** written to `catalog.json` (153 → 487). `gunspin` arrived as a folder that already had a catalog entry waiting for it, so it was not duplicated.
- Each new entry got a proper `title`, a `category` from the existing category set, the **correct HTML entry file** (`index.html` when it exists, otherwise the game's real launcher such as `play.html`, `game.html` or `FUN.html`), and icon URLs wherever the game shipped an `icon` / `favicon` / `logo` file.
- **Nothing that already existed was overwritten.** Duplicate games — including the same game under a different folder name — were skipped on purpose (see [What was skipped](#what-was-skipped)).

Highlights from the import: *Hotline Miami*, *Half-Life*, *Quake*, *SUPERHOT*, *Mindustry*, *Antimatter Dimensions*, *AdVenture Capitalist*, *Super Auto Pets*, *Duck Life 6: Space*, *Slope 3*, *GrindCraft*, *Mr. Mine*, *Spelunky Classic HD*, *Super Smash Flash 2*, *Mari0*, *Geometry Dash*, *Retro Bowl College*, *Stumble Guys*, *Golf Orbit*, *Masked Forces Unlimited*, *Blockpost*, *Forward Assault*, *Texas Hold'em*, *Plinko*, *Strands*, *Kanye Zone* and a deep bench of horror titles (*Five Nights at Winston's*, *Granny 3*, *Evil Nun: School's Out*, *Baldi's Fun New School*, *The Backrooms 2D*, *Skinwalker*, *Slender Multiplayer*).

---

## How it works

1. A game sits in its own folder at the repo root, with an HTML file inside.
2. `catalog.json` maps a game slug to its display name, category, entry URL and icon URLs.
3. The site reads the catalog, renders the cards, and opens a game inside its player iframe.
4. jsDelivr serves every file from the GitHub repo — pushing to `main` is what makes a game live; there is nothing to compile.

Because everything is static and cache-friendly, adding a game costs one folder plus one catalog entry.

---

## Repo layout

```
Website-Src-Files/
├── catalog.json          ← the registry the site reads
├── 2048/                 ← one folder per game
│   └── index.html
├── superhot/
│   └── index.html
├── ztype/
│   └── play.html          ← entry file isn't always index.html
├── ruffle/               ← Flash runtime used by some titles
├── rufflethumbnails/     ← shared thumbnails for Flash content
├── clpixel.html          ← standalone client pages
└── clrocketleague.html
```

Folder names are used verbatim by the catalog, so they may contain capitals or spaces (for example `half life`, `BUMPER CARS SOCCER`, `Kindergarten-3`). Their URLs are percent-encoded in `catalog.json` (`half%20life/index.html`), and `icons` can be omitted when a game has no usable image.

---

## The catalog format

```json
{
  "generatedAt": "2026-11-13",
  "source": "old-dir.txt (host rewritten to cdnBase); UGS-Assets import",
  "titles": {
    "half life": {
      "title": "Half-Life",
      "category": "Shooters & Action",
      "entry": "https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/half%20life/index.html",
      "icons": [
        "https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/half%20life/favicon.ico"
      ]
    },
    "superhot": {
      "title": "SUPERHOT",
      "category": "Shooters & Action",
      "entry": "https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/superhot/index.html"
    }
  }
}
```

| Field | Meaning |
|---|---|
| key | Game slug — normally the folder name; stays stable so links keep working |
| `title` | Display name shown on the card |
| `category` | One of the categories in the table below |
| `entry` | Absolute CDN URL of the HTML file that launches the game |
| `icons` | Optional list of CDN image URLs, best thumbnail first |

---

## Categories

| Category | Games |
|---|---|
| Arcade | 101 |
| Shooters & Action | 65 |
| Platformers | 47 |
| Puzzle | 46 |
| Horror | 45 |
| Sports | 43 |
| Racing | 41 |
| Life & Sim | 32 |
| Idle & Clickers | 28 |
| Strategy & Defense | 23 |
| Board & Cards | 13 |
| Flash | 1 |

---

## Adding a game

1. Drop the game's folder into the repo root (keep its entry HTML inside the folder).
2. Add an entry to `catalog.json`:

```json
"my-new-game": {
  "title": "My New Game",
  "category": "Arcade",
  "entry": "https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/my-new-game/index.html",
  "icons": ["https://cdn.jsdelivr.net/gh/FrostsDev2/Website-Src-Files@main/my-new-game/icon.png"]
}
```

3. Commit and push to `main`. jsDelivr picks up the new files on the next request (a fresh push may take a few minutes to appear on the CDN).

**Before adding, check for the game in this catalog under any name** — many titles already exist as a numbered series (`slope`, `slope2`, `slope 3`) or inside a wrapper folder (`footballbros/footballbros.io/index.html`).

---

## What was skipped in the UGS import

To keep one copy of each game and leave existing folders untouched, 48 source folders were skipped:

- **21 duplicate folder names** already in the repo (`fnaf1`–`fnaf4`, `slope`, `vex3`–`vex6`, `cookieclicker`, `baldis-basics`, `doodle jump`, `bounce back`, `stickman hook`, `level devil`, `linerider`, `polytrack` and more).
- **23 same-game / duplicate copies:** *Basketball Stars* (`basket stars`), *Golf Battle* (`golf-battle`, repo has `golfbattel`), *Snow Rider 3D* (`snow-rider`), *Monkey Mart* (`monkey mart v6.3`), *Geometry Dash SubZero* (`gd-subzero`), *Friday Night Funkin'* (`fnf0.7.5`), *Moto X3M 2* (`moto2`), *PolyTrack* (`poly track`, `poly track old`), the extra Tung Sahur Horror builds, the 3rd BlightBorne copy, a debug build (`dungeon-degen-debug`) and an `escaperoad2city copy` folder.
- **4 folders with no HTML file at all**, so nothing could launch them: `Build`, `elasticman`, `moto winter`, `pico 8 games`.

Two source folders contained a different game than their name suggested, so they were added under honest names: `fall-guys` → **`stumbleguys`** (Stumble Guys) and `retro bowl` → **`retrobowlcollege`** (Retro Bowl College). A third, `UCDS other version`, turned out to be another *Top Speed Racing 3D* build and was removed after copying.

---

## Known gaps

- **283 of 487 entries have no icon**, so those cards show no thumbnail until an image is added. Adding an `icon.png` / `icon.jpg` to the folder and listing it in the entry is enough.
- A few legacy entries point at content that is not a game folder (`clpixel`, `clrocketleague`, `test`) or at folders that are no longer present (`escape-road-city-3`, `hardest-1`, `hardest-2`).
- Three folders exist without a catalog entry because they have no HTML entry point: `hillclimb`, `worldshardestgame`, `worldshardestgame2`.
- jsDelivr caches aggressively and enforces per-file size limits; very large single files can be rejected by the CDN even though they work locally.
- Game progress (saves, high scores) is stored by each game in the browser — nothing is stored server-side.

---

## Credits

None of these games are original work; they are HTML5 builds collected from the web and preserved for the site. The latest batch came from the **UGS-Assets** collection. Flash titles run through **Ruffle**. Rights belong to their respective authors — if something here is yours and you want it removed, open an issue.
