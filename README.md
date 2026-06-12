# 🥂 Champagne Deal Checker

Monitors Tesco (Clubcard) and Sainsbury's (Nectar) for multi-bottle Champagne
deals on Veuve Clicquot, G.H. Mumm, and Lanson. Sends a Pushover notification
to your iPhone when a qualifying deal is found.

---

## Setup — 20 minutes start to finish

### 1. Get Pushover on your iPhone (£5 one-time)

1. Buy [Pushover](https://pushover.net) on the App Store (£5, one-time)
2. Log in at https://pushover.net and note your **User Key** on the dashboard
3. Click **Create an Application/API Token** → name it "Champagne Checker"
4. Note the **API Token/Key** it gives you

---

### 2. Create a GitHub repo

1. Go to https://github.com/new
2. Create a **private** repo — e.g. `champagne-checker`
3. Upload both files:
   - `champagne_checker.py` → root of the repo
   - `champagne-checker.yml` → into `.github/workflows/` folder

---

### 3. Add your Pushover secrets to GitHub

In your repo: **Settings → Secrets and variables → Actions → New repository secret**

Add these two secrets:

| Name | Value |
|------|-------|
| `PUSHOVER_USER_KEY` | Your Pushover user key |
| `PUSHOVER_APP_TOKEN` | Your app API token |

---

### 4. Test it manually

Go to **Actions → Champagne Deal Checker → Run workflow**

Check the logs. You'll see which pages were fetched and whether any deal
patterns were detected. If a deal is found, your phone buzzes within seconds.

---

## Adjusting the watchlist

Edit `WATCHLIST` in `champagne_checker.py`. Each bottle takes:

```python
Bottle(
    name="Your Champagne Name",
    normal_price=27.00,          # RRP, used to calculate % off
    tesco_url="https://...",     # product or search page URL
    sainsburys_url="https://...",
)
```

To get the best URLs: search for the bottle on Tesco/Sainsbury's, open the
product page, and copy the URL. Product page URLs are more reliable than
search URLs.

---

## Adjusting the thresholds

In `champagne-checker.yml`, edit the env block:

```yaml
MIN_DISCOUNT_PCT: "25"   # alert at 25% off or more
MIN_BOTTLES:      "6"    # alert when buying 6+ bottles
```

---

## Schedule

Runs daily at 09:00 UK time (08:00 UTC). To change, edit the cron line:

```yaml
- cron: "0 8 * * *"   # every day at 08:00 UTC
- cron: "0 8 * * 1"   # Mondays only
- cron: "0 8,14 * * *" # 08:00 and 14:00 UTC daily
```

---

## Caveats

- Tesco and Sainsbury's use bot-detection. The script uses a mobile
  user-agent to reduce blocking — but it can still be rate-limited.
- If the scraper stops working, it's usually because the site layout changed.
  Open the product URL in a browser and check if the page still loads normally.
- GitHub Actions free tier gives 2,000 minutes/month — this script uses
  roughly 2 minutes per run, so ~60 runs/month, well within the free limit.
