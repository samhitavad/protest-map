# Protestor: The Backbone of a Democracy

An interactive D3.js map of every one of the ~267,000 real US protest events from 2017–March
2026, sourced from the Crowd Counting Consortium (Harvard Kennedy School Nonviolent Action Lab
& UConn). No sampling — every event for the selected year is rendered.

## Viewing it locally

This **cannot** be opened by double-clicking anymore. It needs to fetch a separate JSON data
file for whichever year is selected, and browsers block that kind of request from a bare local
file for security reasons. Use a real local server:

1. Open this folder in VS Code (`File > Open Folder`).
2. Install the **Live Server** extension (search it in the Extensions panel) if you haven't already.
3. Right-click `index.html` → **Open with Live Server**.
4. It opens at `http://127.0.0.1:5500/index.html`.

## Deploying it for real

- **GitHub Pages** — push this whole folder (including `data/`) to a repo, enable Pages.
- **Cloudflare Pages** — connect the repo or drag-and-drop deploy. Each data file is under
  10MB — comfortably under Cloudflare's 25MB per-file limit.

## How it's built

- `index.html` — the app. State outlines and territory insets render as SVG; the individual
  event dots render on an HTML canvas layer on top, since a normal year can have 30,000–40,000
  events — too many for the browser to track as individual SVG elements.
- `data/events_YYYY.json` — one file per year, fetched only when that year is viewed. The
  browser caches each year after the first load, so revisiting a year is instant.
- Click/hover on the canvas uses a spatial index (`d3.quadtree`) to find the nearest point,
  since canvas has no built-in per-shape mouse events the way SVG does.
- All sidebar counts (the big number, per-state/per-topic totals) come from a separate,
  pre-computed exact lookup table — accurate regardless of what's currently rendered.
