# SpidyCrawler - Synthetic Web Traffic Agent v1.2

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey)
![License](https://img.shields.io/badge/License-Educational-green)
![Status](https://img.shields.io/badge/Status-Active-success)

*"Simulate real users, understand real behavior, optimize real performance."*

A Python-based web traffic bot that simulates human-like browsing. **All bots start at once** — no batching, no cap on count (10, 100, 10k+). Uses proxies for different IPs/locations; optional free proxy auto-fetch.

## 📁 Project Structure

```
SpidyCrawler/
├── run.py                    # One-file launcher (setup + run)
├── runBot.bat                # Double-click launcher (Windows)
├── delete_logs.bat           # Force-delete Logs folder (run after closing IDE/bot)
├── source/
│   ├── SC_BOT.py             # Core bot + human-like behavior
│   ├── bot_manager.py         # Multi-bot runner (unlimited scale)
│   ├── theme.py              # Console theme + log prefixes
│   ├── proxy_fetcher.py      # Fetch free proxies (optional)
│   └── requirements.txt      # Python dependencies
├── Customize/
│   ├── urls.txt              # Target URLs (one per line)
│   ├── spend_time.txt        # Seconds per page (60–86400)
│   ├── bot_count.txt         # Number of bots (1 = single, 2+ = all start together)
│   ├── proxies.txt           # Proxies (optional; empty = auto-fetch or no proxy)
│   └── proxies.txt.example   # Example format
├── .github/workflows/
│   └── seo-bot.yml           # GitHub Action – manual run only (no auto on push)
└── Logs/                     # Created per run; table + step logs per bot
    ├── bot_1/
    │   ├── bot_activity.log  # Table: TIMESTAMP | BOT | MESSAGE
    │   ├── steps.log         # Step | Time | What happened
    │   ├── ip_country.txt    # IP and country for this bot
    │   └── session_final.json
    ├── bot_2/
    └── ...
```

## 🚀 Quick Start

1. **Double-click** `runBot.bat` or run `python run.py` from the project folder.
2. First run will automatically:
   - Create `Customize/` and default config files if missing
   - Upgrade pip and install dependencies (`source/requirements.txt`)
   - Pin `blinker==1.7.0` and install `selenium-wire` (for proxies)
   - Fetch free proxies if `Customize/proxies.txt` is empty
   - Remove old `Logs/` (or skip if files in use)
   - Start the bot(s)

No manual setup. Edit `Customize/urls.txt`, `spend_time.txt`, and `bot_count.txt` after the first run.

### Run on Another PC (auto setup, auto config, auto download)

1. Copy the SpidyCrawler folder (or clone the repo).
2. Install **Python 3.8+** ([python.org](https://python.org)) and **Chrome**. Add Python to PATH.
3. Double-click **runBot.bat** (or run `python run.py`).
4. First run: upgrade pip, install deps (with retry), pin blinker, create config if missing, fetch proxies if empty, then start bots. No manual pip/config steps.

## ⚙️ Configuration

### 1. Bot Count (`Customize/bot_count.txt`)

**Unlimited** — all bots start at once. No cap, no confirmation.

```
1     # Single bot
20    # 20 bots at once
100   # 100 bots at once
5000  # As many as your machine can handle
```

- All bots start together (no batching).
- ~0.2 GB RAM per bot (estimate).
- Console shows first 50 bots live; all write to `Logs/bot_*/`.

### 2. Visit Duration (`Customize/spend_time.txt`)

Seconds per page (60 to 86400):

```
100
600
```

### 3. URLs (`Customize/urls.txt`)

One URL per line (with or without `https://`):

```
https://maizan.me/
https://example.com/
https://www.wikipedia.org/
```

### 4. Proxies (`Customize/proxies.txt`) – Optional

- **Auto (free):** Leave file missing or empty → first run fetches free proxies and saves to `proxies.txt`.
- **Manual:** Add one proxy per line: `http://ip:port` or `http://user:pass@host:port`, `socks5://…`. Auth supported via selenium-wire.
- **No proxy:** Empty file and skip fetch, or remove file → bots run with your IP.
- **Retry:** On proxy failure (e.g. tunnel error), each bot tries up to 3 different proxies from the list.

## 📊 Logs

- **`Logs/bot_N/bot_activity.log`** — Table format: `TIMESTAMP | BOT | MESSAGE`.
- **`Logs/bot_N/steps.log`** — Step summary: `Step | Time | What happened`.
- **`Logs/bot_N/ip_country.txt`** — IP and country for that bot.
- **`Logs/bot_N/session_final.json`** — Full session summary.
- **`Logs/SC_BOT.log`** — Main process log (table style).

To clear logs: close Cursor/IDE and any bot window, then run **`delete_logs.bat`** or delete the `Logs` folder manually.

## 🎯 Features

- **Human-like behavior** — Random delays, mouse movement, scrolling, clicking.
- **Unlimited scale** — All bots start at once; set any count in `bot_count.txt`.
- **Proxies** — Different IP/location per bot; auto-fetch free list or use your own.
- **Anti-detection** — Headless Chrome with realistic viewport, `navigator.webdriver` masked, CDP scripts.
- **Retry on proxy failure** — Up to 3 different proxies per bot if one fails.
- **0 GUI** — Console only; no browser windows.

## 🔧 Requirements

- **Windows 10/11** (runBot.bat is Windows-oriented; run.py works on other OS with Python + Chrome).
- **Python 3.8+**.
- **Chrome** (for Selenium/headless browsing).

## 📋 Dependencies (auto-installed by run.py)

- `selenium` — Browser automation
- `fake-useragent` — Random user agents
- `blinker==1.7.0` — Required for selenium-wire (do not upgrade to 1.8+)
- `selenium-wire` — Proxy support (different IP per bot)

## 🐙 GitHub Action (Manual Only)

- **Workflow:** `SEO Bot Launcher` (`.github/workflows/seo-bot.yml`).
- **Trigger:** **Manual only** — Actions → SEO Bot Launcher → Run workflow.
- **Does not run** on push, PR, or any automatic event.
- Uses `run.py` for full setup and bot run; uploads `Logs` as an artifact.

## 🆘 Troubleshooting

| Issue | What to do |
|-------|------------|
| Python not found | Install Python 3.8+, add to PATH; or use `py -3 run.py`. |
| selenium-wire / blinker error | run.py pins blinker; if it still fails, run `pip install blinker==1.7.0` then `pip install selenium-wire --no-deps`. |
| Proxy tunnel failed | Normal with free proxies. Bot retries with next proxy; or run without proxies (empty `proxies.txt`) to test. |
| Logs folder won’t delete | Close Cursor and any `python run.py` window, then run `delete_logs.bat`. |
| Chrome/driver issues | Install or update Chrome. |

Log files: `Logs/` (and `Logs/bot_*/` for per-bot details).

## 📄 License

Educational and legitimate testing only. Use only on sites you own or have permission to test. Respect terms of service and robots.txt.

---

**Remember:** Use responsibly. All bots start at once with no limit on count — scale according to your hardware and network.
