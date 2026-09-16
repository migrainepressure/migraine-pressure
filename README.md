# Migraine Pressure

A small installable web app (PWA) that shows barometric pressure from 72 h back to 72 h ahead, with the rate of change made explicit, for tracking pressure shifts as migraine triggers.

## What is in the folder

| File | Purpose |
|---|---|
| `index.html` | The whole app: markup, styles and logic in one file. No build step, no dependencies. |
| `manifest.webmanifest` | Lets the phone install it to the home screen as "Migraine Pressure". |
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

All differences are trailing: the value at hour *t* minus the value at hour *t* minus the window. Each hour is then given a tier from whichever of the 12 h or 24 h change scores higher against its own thresholds, and the tier colours the trace, the rate strip under the chart and the shift flags.

| Tier | 12 h change | 24 h change | Shown as |
|---|---|---|---|
| Steady | under 1.5 | under 2.5 | grey trace, empty strip |
| Mild | 1.5 to 3 | 2.5 to 5 | light blue (rise) or light red (fall) |
| Moderate | 3 to 5 | 5 to 8 | mid tone, flag at onset with the peak change |
| Large | 5 and over | 8 and over | strong tone, flag |

Mild starts at half the moderate value. The moderate and large values for both windows are editable in Settings, as is the 3 h threshold used by the columns on the Tendency tab. Thresholds are in hPa whatever display unit is chosen. They are starting points drawn from the migraine literature, not clinical advice.

## Gestures on the main chart

Pinch to zoom, drag to pan, tap to read a value (tap again to clear), double tap or "Reset" to return to the default window. On a computer, the mouse wheel zooms.

## Not in this version

Migraine episode logging, push notifications (these need a server), and the phone's own barometer.
