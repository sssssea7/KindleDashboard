# Kindle Dashboard

A single-page dashboard designed for the Kindle's Experimental Browser. Shows the time, date, weather, a 5-day forecast, the current month's calendar, and a rotating photo from a local folder. Pure HTML/CSS/JS — no build step, no dependencies, no API keys.

**Live:** https://sssssea7.github.io/KindleDashboard/

## Features

- **Clock** — tap to toggle between 12h (`9:42 PM`) and 24h (`21:42`).
- **Weather** — current temperature and conditions for the selected city. Tap the temperature to toggle °F / °C.
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
<option value="37.77,-122.42">San Francisco</option>
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

### Keeping the screen always on

By default the Kindle sleeps after ~10 minutes of inactivity. To prevent this, type `~ds` in the home screen's search bar and submit. The Kindle will stop auto-sleeping; restart the device to re-enable sleep. Availability varies by model and firmware, but it works on many older Kindles.

### Will this damage the screen?

E-ink is a good fit for permanent display:

- **No burn-in.** Unlike OLED, e-ink particles are physically moved between refreshes, so static content doesn't degrade pixels. A static image is actually easier on e-ink than constant change.
- **Mild ghosting.** Repeated partial refreshes can leave faint outlines; the dashboard's slow refresh cadence (clock every 30s, weather every 30 min) keeps this minimal.
- **Frontlight wear.** Frontlight LEDs eventually dim with continuous use, but they're rated for tens of thousands of hours.

The real concern is the battery, not the screen. Keeping the Kindle plugged in 24/7 will gradually degrade the lithium-ion cell (capacity loss, possible swelling). Common workarounds: power via USB with the battery disconnected, or accept the battery as a sacrificial part. The screen will likely outlast the battery.

## APIs used

- [Open-Meteo Forecast API](https://open-meteo.com/en/docs) — weather and forecast.
- [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api) — custom city search.

Both are free and require no API key.
