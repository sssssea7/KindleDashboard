# Kindle Dashboard

A single-page dashboard designed for the Kindle's Experimental Browser. Shows the time, date, weather, a 5-day forecast, the current month's calendar, and a rotating photo from a local folder. Pure HTML/CSS/JS — no build step, no dependencies, no API keys.

**Live:** https://sssssea7.github.io/KindleDashboard/

## Features

- **Clock** — tap to toggle between 12h (`9:42 PM`) and 24h (`21:42`).
- **Weather** — current "feels like" temperature and conditions for the selected city. Tap the temperature to toggle °F / °C.
- **5-day forecast** — high/low and conditions for the next five days.
- **Calendar** — current month with today highlighted.
- **Photos** — rotates through images in `photos/` every 10 minutes. Tap a photo to advance manually (the timer resets).
- **Cities** — preset dropdown, custom city search, or auto-detect via the browser's geolocation API.
- **Persistent** — selected city, clock format, and temperature unit are stored in `localStorage`, so your settings survive reloads.
- **E-ink friendly** — photos rendered in high-contrast grayscale; refresh intervals tuned to avoid unnecessary repaints.

## Customizing photos

1. Drop image files into the `photos/` folder.
2. List them in `photos/photos.json`:
   ```json
   [
     { "filename": "vacation.jpg", "caption": "Vacation" },
     { "filename": "sunset.jpg",   "caption": "Sunset"   }
   ]
   ```
3. Reload the dashboard.

If `photos.json` is missing or empty, the dashboard falls back to a default set of [Lorem Picsum](https://picsum.photos) images so it always has something to show.

**Tip:** strip EXIF metadata before committing personal photos — JPEGs from a phone often contain GPS coordinates. On macOS:

```bash
brew install exiftool
exiftool -all= -overwrite_original photos/*
```

## Customizing cities

Edit the `<option>` list inside `<select id="city-select">` in `index.html`. Each option's value is `lat,lon`:

```html
<option value="37.34,-121.89">San Jose</option>
```

The "Other..." option lets users search any city at runtime via Open-Meteo's geocoding API.

## Hosting

The dashboard is deployed via GitHub Pages from `main`. Any push to `main` triggers a rebuild (~30–60 seconds). For local testing:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

A local server (rather than `file://`) is required so the browser can fetch `photos/photos.json`.

## Loading on a Kindle

1. On the Kindle, open the **Experimental Browser** (Menu → Experimental Browser, or simply "Web" on newer models).
2. Navigate to the deployed URL and bookmark it.
3. Disable screen sleep if you want a permanent dashboard.

## APIs used

- [Open-Meteo Forecast API](https://open-meteo.com/en/docs) — weather and forecast.
- [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api) — custom city search.

Both are free and require no API key.
