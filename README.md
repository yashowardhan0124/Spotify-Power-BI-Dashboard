# 🎧 SPOTIFY POWER BI DASHBOARD

An interactive, 4-page Power BI dashboard that turns the raw Spotify **Top 50 World** chart data into clear KPIs, trends, and artist/song drill-downs. It is built for music analysts, playlist managers, and marketing teams.

> **Tool:** Power BI Desktop · **Data:** `spotify-top-50-world.csv` · **Canvas:** 1920 × 1080 (16:9) · **Custom visual:** Advance Card

---

## 🔗 Quick Links to Dashboard Photos

| Page | Preview file | Open full-resolution image |
|---|---|---|
| 🏠 Home | ![Home page](images/home.png)
| 📊 Overview | `images/overview.png` | [**View Overview →**](images/overview.png) |
| 🎤 Artists | `images/artists.png` | [**View Artists →**](images/artists.png) |
| 🎵 Songs | `images/songs.png` | [**View Songs →**](images/songs.png) |

> Click any screenshot below to open it at full HD resolution (4833 × ~2715 px).

---

## 📸 Dashboard Preview

### 🏠 Home
Landing page with the Spotify branding, an album-cover mosaic, and navigation buttons to every page.

[![Home Page](images/home.png)](images/home.png)

### 📊 Overview
KPIs, now-playing album card, album-type and explicit breakdowns, and monthly trends.

[![Overview Page](images/overview.png)](images/overview.png)

### 🎤 Artists
Artist rankings by distinct songs, total popularity, and Position #1 hits, with a drill-down table.

[![Artists Page](images/artists.png)](images/artists.png)

### 🎵 Songs
Song rankings by popularity, songs per artist, and Position #1 hits, with a detailed table.

[![Songs Page](images/songs.png)](images/songs.png)

---

## 📌 Business Requirement

Spotify stakeholders (music analysts, playlist managers, and marketing teams) need a **consolidated dashboard** to monitor song and artist performance across different dimensions.

## ❓ Problem Statement

Spotify's raw "Top 50" dataset is limited to lists and rankings, which makes it hard to see patterns and take insights quickly. This dashboard addresses:

| Problem | How the dashboard solves it |
|---|---|
| No clear KPI monitoring | Summary cards for total songs, artists, popularity, and duration |
| No explicit vs non-explicit analysis | Side-by-side comparison with share of total |
| Hard to track song/album distribution | Breakdowns by album type and release year |
| Trend visibility missing | Monthly and yearly popularity and song-count trends |
| Artist and song insights not connected | Drill-down pages for Artists and Songs linked from the overview |
| Decision-making gaps | Marketing and curation teams can see which artists and songs to promote |

---

## 🗂️ Dashboard Pages

### 1. 🏠 Home
- Spotify logo and album-cover mosaic background
- Navigation buttons: **Home · Overview · Artists · Songs**

### 2. 📊 Overview — [view image](images/overview.png)
- **KPI cards:** Distinct Songs (**789**), Count of Artists (**342**), Avg Duration (**3.28 min**), Avg Popularity (**89.62**)
- **Now-playing style card** showing the album cover for the selected track
- **Songs by Album Type** (single 269, album 562)
- **Explicit vs Non-Explicit** song counts
- **Songs by Year** (2023: 423, 2024: 452)
- **Avg Popularity by Album Type**
- **Avg Popularity by Month** (area chart, from about 86.9 in Oct to 92.5 in Jan) with a **Month / Quarter** toggle
- **Distinct Songs by Month** (column chart, peaking at 220 in Oct)
- **Songs by Artist** and **Songs by Popularity** ranking bars
- **"Songs & Artist" slicer panel** with album covers on the left. Selecting an item cross-filters the page.

### 3. 🎤 Artists — [view image](images/artists.png)
- **Distinct Songs by Artist** (Taylor Swift leads with 85)
- **Total Popularity by Artist**
- **Position 1 Hits per Artist**, to spot artists with consistent #1 positions
- **Drill-down table:** Year, Quarter, Month, Day, Avg / Max / Min Popularity, Avg Duration, Avg Tracks per Album, Distinct Songs
- Now-playing album card and "Songs & Artist" slicer

### 4. 🎵 Songs — [view image](images/songs.png)
- **Songs by Artist**
- **Songs by Popularity**
- **Songs Hits per Artist** (Position 1 hits)
- **Detail table:** Avg Popularity, Avg Duration, Min Popularity, Distinct Songs, Position 1 Hits, Songs per Artist, Songs per Year, Total Songs, Albums Count, Non-Explicit Songs
- Now-playing album card and "Songs & Artist" slicer

---

## 🧱 Data Model

**Source table:** `Top-50-World` (loaded from CSV, 11 columns)

| Column | Type | Description |
|---|---|---|
| `date` | Date | Chart date |
| `position` | Integer | Chart rank (1–50) |
| `song` | Text | Track name |
| `artist` | Text | Artist name |
| `popularity` | Integer | Spotify popularity score |
| `duration_ms` | Integer | Track length in milliseconds |
| `album_type` | Text | single / album / compilation |
| `total_tracks` | Integer | Tracks in the album |
| `release_date` | Date | Track release date |
| `is_explicit` | Boolean | Explicit-content flag |
| `album_cover_url` | Text | Album art URL |

**Calculated columns:** `Month`, `Qtr`, `MonthIndex`, `Year` (derived from `date`)

**Supporting tables:**
- `_Measures`: a dedicated table that holds all DAX measures
- `Slicer_Option`: a field parameter for the Month / Quarter toggle
- Auto date tables for `date` and `release_date`

### Key DAX Measures

| Measure | Logic |
|---|---|
| Total Songs | `COUNTROWS('Top-50-world')` |
| Distinct Songs / Artists | `DISTINCTCOUNT` on `song` / `artist` |
| Avg Popularity | `AVERAGE(popularity)` |
| Avg Duration Minutes | `AVERAGE(duration_ms) / 60000` |
| Avg Position | `AVERAGE(position)` |
| Explicit / Non-Explicit Songs | `COUNTROWS` filtered on `is_explicit` |
| Pct Explicit Songs | `DIVIDE([Explicit Songs], [Total Songs], 0)` |
| Avg Popularity Explicit / NonExplicit | `AVERAGE(popularity)` filtered on `is_explicit` |
| Position 1 Songs / Artists | Rows or distinct artists where `position = 1` |
| Singles Count / Albums Count | `COUNTROWS` filtered on `album_type` |
| Avg Tracks per Album | `AVERAGE(total_tracks)` |
| Min / Max Popularity, Min / Max Duration | `MIN` / `MAX` aggregates |

---

## 🚀 Getting Started

1. **Clone the repo**
```bash
   git clone https://github.com/<your-username>/<your-repo>.git
```
2. **Install the Advance Card custom visual** if Power BI prompts for it (AppSource: *Advance Card*).
3. **Open any `.pbit` file** in Power BI Desktop. Each file contains the full 4-page report and opens on a different page.
4. When prompted, **point the data source to your local `spotify-top-50-world.csv`**. The template references a local file path, so it must be updated on first open.
5. Click **Load** and explore.

> **Note:** `.pbit` files are templates and ship without data. You need the CSV to populate the visuals.

---

## 📁 Repository Structure

```
├── Home.pbit                      # Report template (opens on Home)
├── Overiew.pbit                   # Report template (opens on Overview)
├── Artists.pbit                   # Report template (opens on Artists)
├── Songs.pbit                     # Report template (opens on Songs)
├── Bussiness_Requirements.docx    # Business requirement & problem statement
├── images/
│   ├── home.png                   # Home page screenshot
│   ├── overview.png               # Overview page screenshot
│   ├── artists.png                # Artists page screenshot
│   └── songs.png                  # Songs page screenshot
└── README.md
```

---

## 🛠️ Tech Stack

- **Power BI Desktop** (report, data model, DAX)
- **Power Query (M)** for CSV ingestion and type transformation
- **DAX** for measures and calculated columns
- **Advance Card** custom visual

---

## 📬 Author

**Yashowardhan Shete**
Feel free to connect and share feedback!
