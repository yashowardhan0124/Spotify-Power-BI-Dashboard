# 🎧 SPOTIFY POWER BI DASHBOARD

An interactive, 4-page Power BI dashboard that turns the raw Spotify "Top 50 World" chart data into clear KPIs, trends, and artist/song drill-downs, built for music analysts, playlist managers, and marketing teams.

> **Tool:** Power BI Desktop · **Data:** `spotify-top-50-world.csv` · **Canvas:** 1920 × 1080 (16:9) · **Custom visual:** Advance Card

---

## 📸 Dashboard Preview

| Home | Overview |
|:---:|:---:|
| ![Home](images/home.png) | ![Overview](images/overview.png) |

| Artists | Songs |
|:---:|:---:|
| ![Artists](images/artists.png) | ![Songs](images/songs.png) |

> To export HD screenshots: open each `.pbit` in Power BI Desktop → **File → Export → Export to PDF**, then convert each page to PNG (or use **Windows + Shift + S** on the full-screen view). Save them as `home.png`, `overview.png`, `artists.png`, `songs.png` inside an `images/` folder.

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
| Artist and song insights not connected | Drill-down pages linked from the overview |
| Decision-making gaps | Marketing and curation teams can see which artists and songs to promote |

---

## 🗂️ Dashboard Pages

### 1. 🏠 Home
Landing page with navigation buttons to **Overview**, **Artists**, and **Songs**.

### 2. 📊 Overview
- **KPI cards:** Distinct Songs, Distinct Artists, Avg Popularity, Avg Duration (minutes)
- **Now-playing style card** showing the album cover for the selected track
- **Explicit vs Non-Explicit** comparison and share
- **Songs by Album Type** (single / album / compilation) donut charts
- **Distinct Songs and Avg Popularity by Year**
- **Monthly trends:** Avg Popularity (area chart) and Distinct Songs (column chart)
- **Month / Quarter toggle** (advanced slicer) for the time-based charts
- **Top Songs** and **Top Artists** by popularity
- Song / artist / cover **slicer panel** on the left

### 3. 🎤 Artists
- **Top Artists by Popularity**
- **Songs by Artist** (distinct song count)
- **Position #1 hits per artist**, to spot artists with consistent hits
- **Drill-down table:** release date, distinct songs, avg/min/max popularity, avg tracks per album, avg duration

### 4. 🎵 Songs
- **Top Songs by Popularity**
- **Distinct Artists per Song**
- **Position #1 hits per song**
- **Detail table:** album count, explicit vs non-explicit songs, avg/min popularity, avg duration, position-1 hits
- Popularity gauge (100% stacked bar) and album-cover card for the selected song

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
4. When prompted, **point the data source to your local `spotify-top-50-world.csv`**. The template currently references a local file path, so it must be updated on first open.
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
├── images/                        # Dashboard screenshots
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
