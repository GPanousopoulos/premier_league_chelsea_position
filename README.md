# ⚽ Premier League Table retro view (with a Chelsea bias 💙)

This is my first take on a LLM-powered snapshot of the 2026-27 Premier League table. The script:
- scrapes the live table from [premierleague.com](https://www.premierleague.com/en/tables/premier-league/2026-27) with Selenium
- hands the page text to GPT-6-Luna model, and
- returns a clean, formatted summary with a focus on Chelsea FC (because why not).


## 📋 Requirements/Dependencies

| Requirement | Notes |
|---|---|
| Python 3.11+ |
| Google Chrome | Any recent version. Selenium Manager downloads the matching ChromeDriver automatically |
|You will need your LLM provider API key - in this case OpenAI, but you can use any LLM you want|
| Python packages | `selenium`, `beautifulsoup4`, `openai`, `python-dotenv`, `ipython` |

## 🚀 Setup

From the root of the repo:

```bash
uv sync
```

```bash
uv add selenium
```

Then create a `.env` file in the repo root with your key:

```env
OPENAI_API_KEY=sk-proj-...
```

## ▶️ Usage

Run it as a script:

```bash
uv run python week1/community-contributions/premier_league_scraper.py
```

## ⚙️ Configuration

`fetch_website_contents()` takes a few optional parameters:

| Parameter | Default | What it does |
|---|---|---|
| `url` | *(required)* | Page to scrape |
| `wait_for` | `"table tbody tr"` | CSS selector to wait for before reading the page |
| `timeout` | `20` | Seconds to wait for `wait_for`. On timeout it continues with whatever has loaded |
| `max_chars` | `10_000` | Maximum characters of page text returned |

**Want a different club?** Edit `system_prompt` and `user_prompt` near the top of the script and swap Chelsea for your team.

**Different season?** Change the season in the URL, e.g. `.../tables/premier-league/2025-26`.

## ⚠️ Notes & limitations

- **Run time:** Each call to `fetch_website_contents()` launches Chrome. A full run loads the page three times and calls the
  API twice (once for `print`, once for `display_summary`). Remove the calls you don't need to speed it up.
- **Next fixture:** The table page lists each club's next opponent, but not team news or line-ups. The model's answer is only as good as what's on the page.
- **Site changes:** If premierleague.com changes its markup, update `wait_for` to a selector that matches the new table.
- **Model:** Uses `gpt-6-luna` with `reasoning_effort="none"`. Swap in any Chat Completions model you have access to.

*Made with 💙 by George (and Claude :-D).*
