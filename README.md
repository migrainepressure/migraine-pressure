# Barometer

A small installable web app (PWA) that shows barometric pressure from 72 h back to 72 h ahead, with the rate of change made explicit, for tracking pressure shifts as migraine triggers.

## What is in the folder

| File | Purpose |
|---|---|
| `index.html` | The whole app: markup, styles and logic in one file. No build step, no dependencies. |
| `manifest.webmanifest` | Lets the phone install it to the home screen as "Barometer". |
| `sw.js` | Service worker. Caches the app shell so it opens offline; the last pressure data is kept in local storage. |
| `icons/` | Home screen icons. |

## Hosting (needs HTTPS for install and geolocation)

Any static host will do. Two that take no setup:

1. GitHub Pages: create a repository, upload these files to its root, then Settings, Pages, deploy from the main branch. The app appears at `https://<user>.github.io/<repo>/`.
2. Netlify Drop (app.netlify.com/drop): drag the folder onto the page. You get a URL in a few seconds.

Opening `index.html` straight from disk also works for a look around, but without a location search over HTTPS the browser may block geolocation; the town search still works.

## Installing on the phone

Open the URL in the phone's browser, set a location in Settings (search a town, or "Use my position"), then:

- iPhone (Safari): Share, "Add to Home Screen".
- Android (Chrome): the three dot menu, "Install app" or "Add to Home screen".

## Data

Hourly sea level pressure (or station pressure, switchable) from Open-Meteo: 7 days of observations and 7 days of forecast, refreshed on open when the cached copy is more than 30 minutes old. No account, no key, no tracking. Data licence CC BY 4.0.

## How the rate of change is worked out

All differences are trailing: the value at hour *t* minus the value at hour *t* minus the window.

| Measure | Where it shows | Default threshold |
|---|---|---|
| Change over 3 h | Deltas beside the hero number; the column strip under the main chart (re-binned to per hour when zoomed to 30 h or less) | ±1.0 hPa |
| Change over 12 h | Hatched shift spans on the main chart; the Shifts list on the Tendency tab. A span runs from the start of the first 12 h window that exceeds the threshold to the end of the last consecutive one | ±3.0 hPa |
| Change over 24 h | Trailing line on the Tendency tab; the Tendency hero shows the largest 24 h change in the forecast | ±5.0 hPa |

Thresholds are in hPa whatever display unit is chosen, and are editable in Settings. They are starting points drawn from the migraine literature, not clinical advice.

## Gestures on the main chart

Pinch to zoom, drag to pan, tap to read a value (tap again to clear), double tap or "Reset" to return to the default window. On a computer, the mouse wheel zooms.

## Not in this version

Migraine episode logging, push notifications (these need a server), and the phone's own barometer.
