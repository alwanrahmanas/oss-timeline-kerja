# 📊 Annual Timeline Dashboard

A **static, interactive web dashboard** for visualizing annual work activity timelines across multiple teams. Built with vanilla HTML/CSS/JavaScript — no framework, no build step, no server required.

> **Live example →** [fix-timeline-kerja.vercel.app](https://fix-timeline-kerja.vercel.app/)

![Dashboard Preview](https://raw.githubusercontent.com/alwanrahmanas/fix-timeline-kerja/main/preview.png)

---

## ✨ Features

| Feature | Description |
|---|---|
| **Gantt Chart** | Weekly activity bars across a 53-week year, scrollable vertically & horizontally |
| **Workload Heatmap** | Team-level intensity grid — instantly identify bottleneck weeks |
| **Calendar View** | Month-by-month activity chips, filterable by team |
| **KPI Strip** | Total activities, busiest month, busiest team, and overlap week count |
| **Team Filter** | Toggle individual teams on/off in the sidebar |
| **Month Filter** | Narrow down to a specific month |
| **Program Filter** | Filter by program/group across all views |
| **Global Search** | Debounced full-text search across activity and program names |
| **Zoom Levels** | Yearly · Quarterly · Monthly views |
| **Detail Panel** | Click any row to see full activity details + schedule overlap warnings |
| **Dark Mode** | Full light/dark theme toggle |
| **Responsive** | Hamburger sidebar on mobile screens |

---

## 🚀 Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/your-username/timeline-dashboard.git
cd timeline-dashboard
```

### 2. Open the dashboard

Just open `index.html` in your browser. The example data (`data_embed.js`) is already included — no setup needed.

```
timeline-dashboard/
├── index.html        ← Open this in your browser
├── data_embed.js     ← Example data (auto-generated)
├── convert_to_js.py  ← Run this to generate your own data
├── favicon.png       ← Replace with your own icon
├── example/          ← Contains a working example Excel file
└── timeline-data/    ← Put your .xlsx files here (gitignored)
```

---

## 📋 Using Your Own Data

### Step 1 — Prepare your Excel files

Each Excel file represents **one team**. The format is:

| Row | Content |
|---|---|
| Row 1 | Month headers: `Januari`, `Februari`, ..., `Desember` |
| Row 2 | Week labels: `W1`, `W2`, `W3`, ... |
| Row 3+ | Activity data (see below) |

**Activity row columns:**

| Column | Content |
|---|---|
| B (col 2) | Program / Group name |
| C (col 3) | Activity name |
| D (col 4) | Schedule text (optional, e.g. `"Jan – Mar 2026"`) |
| Week columns | Mark active weeks with any of: `■` `x` `X` `v` `1` `✓` `●` |

**Two week-label styles are supported:**
- **Style A (per-month):** Weeks reset each month → `W1 W2 W3 W4 | W1 W2 W3 W4 | ...`
- **Style B (global):** Weeks run globally → `W1 W2 ... W53`

The converter auto-detects which style you're using.

> ⚠️ **Typos in week labels are handled gracefully.** The converter uses positional column indexing rather than trusting the cell label text.

### Step 2 — Configure your teams

Edit `convert_to_js.py` and update these two sections:

```python
# Team detection: map filename patterns → team key
TIM_PATTERNS = {
    "team_a": r"team.?a|team_a",
    "team_b": r"team.?b|team_b",
}

# Team display settings
TIM_META = {
    "team_a": {"label": "Team A",  "color": "#01696f"},
    "team_b": {"label": "Team B",  "color": "#7a39bb"},
}
```

The `color` field accepts any CSS hex color. The `label` is shown in the sidebar and dashboard cards.

### Step 3 — Put Excel files in `timeline-data/`

```
timeline-data/
├── timeline_team_a_2026.xlsx
├── timeline_team_b_2026.xlsx
└── timeline_team_c_2026.xlsx
```

> **Testing the example:** we provided a working Excel template! Simply create the `timeline-data/` folder, copy `example/timeline_team_a_2026.xlsx` into it, and you're good to go.

> The `timeline-data/` folder is gitignored by default to protect sensitive organizational data.

### Step 4 — Run the converter

```bash
pip install openpyxl
python convert_to_js.py
```

Expected output:
```
Found 3 file(s):
  [timeline_team_a_2026.xlsx] → 76 activities [team_a]
  [timeline_team_b_2026.xlsx] → 95 activities [team_b]
  [timeline_team_c_2026.xlsx] → 34 activities [team_c]

Done! 205 activities from 3 team(s) written to data_embed.js
```

### Step 5 — Refresh the browser

Open (or refresh) `index.html` — your data will appear immediately.

---

## 🎨 Customization

### Change branding / title

Edit these lines in `index.html`:

```html
<title>Dashboard Timeline Kegiatan — Your Organization 2026</title>

<!-- Sidebar logo text -->
<strong>Your Organization</strong>
<span>Activity Timeline 2026</span>

<!-- Topbar -->
<h1 class="topbar-title">Dashboard Timeline Kegiatan <span>2026</span></h1>
```

### Change the accent color

In `index.html`, find and update:
```css
:root {
  --primary: #01696f;  /* Change this to your brand color */
}
```

### Change the year / week structure

In `convert_to_js.py`, update `MONTHS_CONFIG` to match your year's week counts:

```python
MONTHS_CONFIG = [
    (1,  "January",  5),  # 5 weeks in Jan
    (2,  "February", 4),
    ...
]
```

The total must sum to 53 weeks. Also update the matching `MONTHS` array in `index.html` (search for `const MONTHS = [`).

---

## 🌐 Deploying to Vercel / Netlify

Since this is a fully static site (pure HTML + JS), deployment is one step:

**Vercel:**
1. Push to GitHub
2. Import the repo in [vercel.com](https://vercel.com)
3. No build settings needed — Vercel will serve `index.html` directly

**Netlify:**
1. Push to GitHub
2. Import in [netlify.com](https://netlify.com) with Publish directory: `/` (root)

---

## 🔒 Data Privacy

- **Excel files are gitignored** — your actual organizational data never leaves your machine unless you explicitly add it.
- The included `data_embed.js` contains only **example/dummy data**.
- For team deployments, generate `data_embed.js` locally and push only that file (not the `.xlsx` sources).

---

## 🛠️ Technical Details

- **Zero dependencies** — vanilla HTML, CSS, JavaScript only
- **O(1) heatmap lookup** — pre-built index for instant cell rendering
- **CSS gradient Gantt bars** — no canvas, no SVG, scales to 400+ activities without lag
- **Fragment-based DOM rendering** — batched DOM writes for smooth performance
- **Positional week mapping** — robust Excel parsing that ignores typos in week cell labels

---

## 📁 File Reference

| File | Purpose |
|---|---|
| `index.html` | Complete dashboard (HTML + CSS + JS in one file) |
| `data_embed.js` | Generated data payload (commit this, not the .xlsx files) |
| `convert_to_js.py` | Excel → JS converter script |
| `favicon.png` | Browser tab icon (replace with your own) |
| `timeline-data/` | Place your .xlsx source files here (gitignored) |

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

*Built with ❤️ for teams managing complex annual work plans.*
