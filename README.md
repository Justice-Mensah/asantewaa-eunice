# Happy Birthday, Eunice

A one-page birthday site for **Asantewaa Eunice** — curtain intro, hero, a
heartfelt letter, three blessings, a wish-cake with confetti, and a warm OG
preview for WhatsApp/Twitter. It also **silently records each visitor's IP and
approximate location to a Google Sheet** you own (no database).

Live URL (once pushed):
**https://justice-mensah.github.io/asantewaa-eunice/**

---

## 1. Turn on visitor logging (Google Sheet — ~5 min)

The page is static (GitHub Pages), so visits are logged to a Google Sheet via a
tiny Apps Script Web App.

1. Create a new sheet: go to <https://sheets.new>, name it e.g. *Eunice Visits*.
2. **Extensions ▸ Apps Script**. Delete the sample, paste **all** of
   [`apps-script.gs`](apps-script.gs), click **Save**.
3. **Deploy ▸ New deployment ▸ (gear) Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - **Deploy**, then **Authorize access** → allow.
4. Copy the **Web app URL** (ends with `/exec`).
5. Open [`index.html`](index.html) and paste it into:
   ```js
   window.LOG_ENDPOINT = "https://script.google.com/macros/s/XXXX/exec";
   ```
6. Save + commit + push. Every visit now appends a row (Time, IP, City, Region,
   Country, Latitude, Longitude, a clickable map link, ISP, Device, etc.).

> Until you paste a real URL, logging stays **off** (the page still works fine).

### What's captured — two rows per visitor
Every visit logs one **IP** row automatically, and (if they allow it) one
**GPS** row. The `Source` column tells them apart:

- **IP** — automatic, no popup. City/ISP-level, so people on the same network
  share the same coordinates. Good for country/city, not exact.
- **GPS** — asked when the visitor taps **Enter** (a browser permission prompt).
  This is the **exact** location, with an accuracy in metres. If they decline,
  only the IP row is logged.

Both rows also capture: IP, city/region/country, latitude/longitude, a clickable
map link, ISP, device/user-agent, language, screen size, timezone, referrer, and
the page URL.

> Note: getting each person's *exact* spot requires the GPS prompt — there's no
> way to read precise location silently from a browser.

---

## 2. Deploy to GitHub Pages (its own repo)

```bash
cd /Applications/MAMP/htdocs/birthday/eunice
git init
git add .
git commit -m "Happy birthday, Eunice"
git branch -M main
git remote add origin https://github.com/Justice-Mensah/asantewaa-eunice.git
git push -u origin main
```

Then on GitHub → the **asantewaa-eunice** repo → **Settings → Pages** →
Source: `Deploy from branch`, Branch: `main` / `/ (root)` → **Save**.

In ~1 minute the site is live at:
**https://justice-mensah.github.io/asantewaa-eunice/**

---

## 3. WhatsApp / link preview

`og-image.jpg` (1200×630, ~41KB — well under WhatsApp's 300KB cap) is already
wired with absolute URLs. If WhatsApp shows a stale/blank preview, force a
re-scrape by adding a query string when sharing:

```
https://justice-mensah.github.io/asantewaa-eunice/?v=2
```

Test the preview:
- <https://www.opengraph.xyz/>
- <https://developers.facebook.com/tools/debug/> (Sharing Debugger → Scrape Again)

---

## Customize

- **The letter** → [`index.html`](index.html) → `.letter__body`
- **Hero + curtain wording** → [`index.html`](index.html) top sections
- **Her photo as the preview** → replace `og-image.jpg` (keep it ≤300KB, ideally
  1200×630 or portrait; update `og:image:width/height` if you change the size)
- **Palette** → [`styles.css`](styles.css) → `:root`

## Files

- `index.html` — structure + OG meta + the `LOG_ENDPOINT` config
- `styles.css` — all styling
- `script.js` — curtain, reveals, confetti, candle, copy-link, **visitor logging**
- `apps-script.gs` — the Google Apps Script you deploy (not served to visitors)
- `og-image.jpg` — the link preview
