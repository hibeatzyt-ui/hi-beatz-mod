# Hi Beatz — your song library

This folder *is* your library. Hi Clone reads it entirely from static files —
there's no server, no database, nothing to install. Every song is one folder
in here, and `songs.json` (in this folder) lists which folders to load.

## 1. Folder naming

```
Hi Beatz/
  7rings_ST/
  7rings_DIAMOND/
  shapeofyou_GOLD/
```

Pattern: `<anything>_<DIFFCODE>`, where `<DIFFCODE>` is one of:

| Suffix     | Shown as  |
|------------|-----------|
| `_ST`      | Standard  |
| `_GOLD`    | Gold      |
| `_DIAMOND` | Diamond   |

Songs are grouped in-game by their `title` + `artist` (from `info.json`), so
`7rings_ST` and `7rings_DIAMOND` show up as one song card in Song Select with
two selectable difficulties. The folder suffix is just a naming convention —
if `info.json` includes an explicit `"difficulty"` field, that wins.

## 2. What goes inside each song folder

```
7rings_DIAMOND/
  info.json      required
  chart.chart    required
  audio.mp3      required (mp3/ogg/wav all work — just match the "audio" field)
  artwork.jpg    optional but recommended (jpg/png)
```

A ready-to-copy starting point is in `Hi Beatz/_template/`.

## 3. info.json

```json
{
  "title": "7 rings",
  "artist": "Ariana Grande",
  "difficulty": "Diamond",
  "bpm": 140,
  "audio": "audio.mp3",
  "artwork": "artwork.jpg",
  "chart": "chart.chart"
}
```

Don't want to hand-write this? Open Hi Clone → **Add a Song** on the home
screen. Fill in the title, artist, difficulty and BPM, and it generates this
JSON for you (and shows you the folder name to create).

## 4. chart.chart — note types

The file is plain JSON with a `.chart` extension (to match your other chart
tooling) — the content format is exactly the same as `.json`, just saved
with a different name.

```json
{
  "bpm": 120,
  "offset": 0,
  "notes": [ ... ]
}
```

- `bpm` — informational / used for authoring.
- `offset` — seconds to shift every note relative to the audio file, in case
  your export has lead-in silence.
- `notes` — an array of note objects, each with a `t` (time in seconds from
  the start of the audio) and a `type`:

| type              | meaning                                             | required fields |
|-------------------|------------------------------------------------------|------------------|
| `"tap"`           | plain tap note                                       | `lane` (1–4)     |
| `"l1"`,`"l2"`,`"l3"`,`"l4"` | swipe **left** across 1–4 lanes, starting at `lane`  | `lane` (1–4)     |
| `"r1"`,`"r2"`,`"r3"`,`"r4"` | swipe **right** across 1–4 lanes, starting at `lane` | `lane` (1–4)     |
| `"h<a>><b>"` e.g. `"h2>3"`, `"h3>4"` | noodle hold — press-and-hold lane `a`, dragged to lane `b` | `dur` (seconds) |

Example:

```json
{ "t": 1.50, "lane": 2, "type": "tap" }
{ "t": 3.50, "lane": 1, "type": "l1" }
{ "t": 4.00, "lane": 4, "type": "r4" }
{ "t": 5.00, "type": "h2>3", "dur": 0.6 }
{ "t": 6.00, "type": "h3>4", "dur": 0.6 }
```

Notes must be listed in any order — Hi Clone sorts them by time on load.

## 5. Adding the song to your library

Open `Hi Beatz/songs.json` and add the folder name to the array:

```json
["7rings_ST", "7rings_DIAMOND", "shapeofyou_GOLD"]
```

That's it — reload Hi Clone and the song shows up in Song Select.

## 6. A note on audio files

Hi Clone doesn't ship with any songs. Add your own audio files locally —
just make sure you have the right to use whatever you put in here, especially
if you publish your GitHub Pages site.
