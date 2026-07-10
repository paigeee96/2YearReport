# Research report site — starter scaffold

A static site built around two "movements," each shown three ways, top to bottom:

1. **Full-length side by side** — the two complete recordings playing side by side, with a "play both, synced" button.
2. **Step side by side** — a shorter, more focused clip of each recording, also side by side with its own "play both, synced" button. These clips are expected to already have the moving graph baked into the video frame itself (e.g. rendered in MATLAB) — so there's no JavaScript syncing data to video here, they just play as ordinary video.
3. **3 static comparison charts, side by side** — one for hip, one for knee, one for ankle, each overlaying Video A vs Video B (teal vs coral) so the two trials are easy to compare at each joint.

No build tools, no backend — just HTML/CSS/JS, so it works directly on GitHub Pages.

## Swap in your own content

### Videos
Put your `.mp4` files in the `videos/` folder, then update the `<source src="...">` paths in `index.html`. There are 8 video slots, 4 per movement:

| Movement | Full-length pair | Step pair (graph baked in) |
|---|---|---|
| 1 | `#m1LongA`, `#m1LongB` | `#m1StepA`, `#m1StepB` |
| 2 | `#m2LongA`, `#m2LongB` | `#m2StepA`, `#m2StepB` |

Within each pair, the two clips sync best (for the "play both" button) if they're the same length as each other.

**On the MATLAB-rendered step videos:** since the graph is burned into the frame, just export those as normal `.mp4` files — nothing special needed on the web side. Keep files under ~50–100MB if possible for fast loading; for longer/larger video, consider hosting on YouTube/Vimeo and embedding an iframe instead (ask me and I can wire that up).

### Joint-angle comparison data
Open `js/main.js` and look at `SAMPLE_SERIES`. It's nested by movement → video → joint, and only feeds the 3 static comparison charts (not the videos):

```js
const SAMPLE_SERIES = {
  m1: {
    a: { hip: [...], knee: [...], ankle: [...] },   // Movement 1, Video A
    b: { hip: [...], knee: [...], ankle: [...] },   // Movement 1, Video B
  },
  m2: {
    a: { hip: [...], knee: [...], ankle: [...] },   // Movement 2, Video A
    b: { hip: [...], knee: [...], ankle: [...] },   // Movement 2, Video B
  },
};
```

Each joint's array is a time series shaped like `[{ t: 0, value: 42.1 }, { t: 0.5, value: 43.0 }, ...]`, where `t` is seconds and `value` is the joint angle in degrees.

If your data lives in CSVs, the easiest path is: export each of the 12 series (2 movements × 2 videos × 3 joints) to its own small `.json` file in that `{t, value}` shape, drop them in `data/`, and fetch them before the `DOMContentLoaded` handler runs — happy to help wire that up once you share a sample of your real CSV format.

### Titles & labels
- Replace "Name of movement 1" / "Name of movement 2" in `index.html` with your real movement names.
- Replace "Video A" / "Video B" labels throughout if you have better names (e.g. "Left leg" / "Right leg", "Pre" / "Post").

## Deploy to GitHub Pages

1. Create a new GitHub repo, e.g. `your-username.github.io` for a root-level site, or any name for a project site.
2. Upload this folder's contents (not the folder itself) via **Add file → Upload files** in the repo, then **Commit changes**.
3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: main / (root)**.
4. Your site will be live at the URL shown on that Pages settings screen.
