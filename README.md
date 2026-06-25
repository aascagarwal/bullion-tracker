# Metals Macro Monitor — Deploy to GitHub Pages

A self-updating dashboard that auto-fetches daily Fed data (real yields, 2Y, 10Y,
breakeven inflation) and scores the gold/silver regime. Runs as a free webpage you
bookmark on your phone.

---

## What you get

- A live URL like `https://YOURNAME.github.io/metals-monitor/`
- Opens and auto-pulls the latest FRED data each time
- Green/amber/red regime scoring from the daily leading signals
- Manual fields for gold/silver ratio and the slow quarterly confirmers

---

## Deploy in ~10 minutes (no coding)

### 1. Create the repo
1. Go to https://github.com/new
2. Repository name: `metals-monitor` (or anything)
3. Set it to **Public** (required for free GitHub Pages)
4. Check "Add a README" — then click **Create repository**

### 2. Add the dashboard file
1. In your new repo, click **Add file → Upload files**
2. Upload `index.html` (the file from this set — the name `index.html` matters)
3. Click **Commit changes**

### 3. Turn on GitHub Pages
1. In the repo, go to **Settings** (top tab)
2. Left sidebar → **Pages**
3. Under "Build and deployment" → Source: **Deploy from a branch**
4. Branch: **main**, folder: **/ (root)** → click **Save**
5. Wait ~1 minute. The page URL appears at the top of that same Pages screen.

### 4. First open
1. Visit your new URL
2. Paste your FRED API key into the top bar
3. Click **Save & Fetch**
4. The yields load automatically. Bookmark it. Done.

---

## About the API key

Your FRED key is **read-only access to public economic data** — it can only look up
Fed statistics that are already public. In this setup the key is entered in the
browser each session (not stored in the repo), so it is never committed to GitHub.

If you'd prefer the page to remember the key so you don't paste it each time, you
have two choices:
- **Convenience:** hard-code it into `index.html` (one line near the bottom:
  `document.getElementById('apikey').value = 'YOURKEY';` before the load handler).
  Only do this if your repo is private OR you accept the key being visible — which
  is low-risk for a free read-only FRED key.
- **Hidden:** use the GitHub Action approach (ask me to build the Option B version)
  where a daily job fetches the data using the key as an encrypted repo secret and
  writes it into a data file the page reads. The key never appears in the page.

---

## What updates automatically vs manually

**Automatic (every business day, 1-day lag):**
- 10Y real yield (DFII10) — the master switch
- 2Y yield (DGS2) — Fed-hike proxy
- 10Y nominal (DGS10) — context
- 10Y breakeven inflation (T10YIE)

**Manual (you type when new data drops):**
- Gold/silver ratio — spot prices aren't free on FRED. Enter them, or wire your own
  feed into the `computeRatio()` function.
- Foreign Treasury holdings (TIC) — monthly
- Central bank gold % — quarterly

---

## Reading the signal

- **TAILWIND BUILDING** (green): daily signals net positive. Regime turning
  supportive — scale-in zone *if* confirmed on weekly closes.
- **MIXED / TRANSITION** (amber): signals diverging. Where bottoms form. Real
  yields are the tiebreaker.
- **HEADWIND INTACT** (red): rates/dollar still driving metals down. Wait for the
  rollover.

The single most important line: **a price low without a real-yield rollover is a
bear-market bounce.** Watch DFII10 turn down before acting with size.

Not investment advice.
