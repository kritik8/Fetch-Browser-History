# Browser History Extractor

Compact project to extract and summarize browser history from Chrome/Chromium, Firefox, and Edge profiles into a simple timeline and top-domains report. Designed as a small, honest personal project you can complete in a day and link from your CV.

Overview
- Purpose: quickly produce a user activity timeline (timestamps, URL, title, browser, profile) and a short triage summary (top domains, top visited pages) from local browser history files.
- Scope: works on local browser profile SQLite files (Chrome/Chromium/Edge `History`, Firefox `places.sqlite`). Does not analyze cookies, passwords, or synced cloud data.
- Deliverables: a small Python script (browser_timeline.py), example outputs (CSV, simple Markdown/HTML report), and screenshots you can include in your repo.

Why this is useful
- Fast way to show user web activity during a timeframe, helpful for basic triage and demonstrating practical forensic skills.
- Small, reproducible, and safe when using only your own data or public sample files.

Safety & legal note (read this)
- Only analyze browser data you own or are explicitly permitted to analyze. Do not extract or share someone else’s private browsing history without consent. If you use sample data, link its source.
- Close the browser (or copy the SQLite file while browser is closed) before reading Chrome/Firefox history to avoid locks/corruption.

What’s included (suggested repo structure)
- browser_timeline.py — main extraction & export script (Python)
- requirements.txt — minimal Python deps (e.g., pandas, python-dateutil)
- sample_data/ — (optional) small example history files or links to public test files
- outputs/ — example outputs: timeline.csv, top_domains.md, screenshot.png
- README.md — this file

Prerequisites
- Python 3.8+
- pip
- Recommended Python packages (install from requirements.txt):
  - pandas
  - python-dateutil
  - tldextract (optional, for domain extraction)
- sqlite3 (usually available by default on Linux/macOS/Windows via Python)

Quick install
```bash
git clone https://github.com/<your-username>/browser-history-extractor.git
cd browser-history-extractor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

How browser history is located (common paths)
- Chrome / Edge (while browser closed):
  - macOS: ~/Library/Application Support/Google/Chrome/Default/History
  - Linux: ~/.config/google-chrome/Default/History
  - Windows: %LOCALAPPDATA%\Google\Chrome\User Data\Default\History
- Firefox:
  - profiles usually under: ~/.mozilla/firefox/*.default-release/places.sqlite
- Note: profiles other than `Default` will have a different folder name. Copy the file to a working folder before running the script.

Usage (example)
- Basic:
  ```bash
  python3 browser_timeline.py \
    --input /path/to/copied/History \
    --browser chrome \
    --output outputs/timeline.csv
  ```
- Multiple inputs (Chrome + Firefox):
  ```bash
  python3 browser_timeline.py \
    --inputs /path/to/History /path/to/places.sqlite \
    --output outputs/timeline.csv \
    --report outputs/top_domains.md
  ```
- The script will:
  - Read supported SQLite schemas and normalize timestamps to UTC.
  - Produce `timeline.csv` with fields: timestamp_utc, browser, profile, url, title, visit_count, domain.
  - Produce a short `top_domains.md` showing top N domains and top visited URLs.

Notes on timestamps & normalization
- Chrome/Chromium/Edge use WebKit/Chrome timestamps (large integers). The script converts these to standard UTC datetimes.
- Firefox uses Unix timestamps in `places.sqlite`; these are also normalized to UTC.
- All outputs are in UTC to avoid ambiguity; local timezone conversions can be performed in the notebook or by the consumer.

Example output (CSV row)
timestamp_utc, browser, profile, url, title, visit_count, domain
2025-11-08T14:23:10Z, chrome, Default, https://example.com/article, "Example Article", 3, example.com

Simple analysis ideas to add
- Events per hour / day (activity heatmap)
- Top 20 domains and top 20 URLs
- Filter timeline by keyword (e.g., "login", "bank", "github")
- Produce a small findings snippet: “Most activity between 09:00–11:00 UTC; top domain: example.com; suspicious visit: ...” (if applicable)

How to present this on your CV (honest examples)
- “Personal project — Browser history extractor: small Python tool to parse Chrome/Firefox history sqlite files and produce a normalized timeline and top-domains report. Repo: https://github.com/<your-username>/browser-history-extractor (Python, pandas, SQLite).”
- One-liner: “Built a browser history timeline extractor (Python) to quickly triage user web activity from local profile files.”

Suggestions to make it CV-ready (1–2 day work)
- Include a runnable script + requirements and a sample run in outputs/
- Add a short example findings file and 1–2 screenshots of visualizations (graph of events/hour and top domains)
- Add a short test script (scripts/test_run.sh) that runs the extractor on sample_data and checks outputs exist

Next steps / optional improvements
- Add a Jupyter notebook for visualization (events-per-hour, heatmap)
- Integrate with browser-history libraries to support more browsers
- Add domain categorization (work, social, finance) or simple risk scoring

License & attribution
- Use an OSI-approved license if you plan to publish (MIT is common for small personal projects).
- If you include any sample data, cite the source.

Questions or want a starter script?
- I can generate a minimal `browser_timeline.py` script and `requirements.txt` now (ready-to-commit). Tell me whether you want it to:
  - Read Chrome/Firefox files directly, or
  - Assume pre-copied files in a `sample_data/` folder

```
