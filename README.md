# Favor route planner

A single-page planner for Favor runs out of H-E-B. It reads the addresses off your Favor screenshots, lets you check them, works out the quickest order to drive them, and hands each leg to Google Maps.

## Put it on the site

Upload `index.html` and `README.md` to the root of the `main` branch, replacing the old ones. In the repository settings, open Pages, choose Deploy from a branch, select `main` and `/(root)`, then save. Open the site address shown in Pages after deployment. If the old page still shows, reload once; phones cache it.

## Using it

1. Choose your Favor screenshots, or skip this and type the addresses.
2. Check the pickup and every drop-off. Names read from a screenshot are highlighted until you check them. Type the name when Favor shows it on another screen.
3. Tap **Find quickest order**. If an address cannot be found, only that row is marked, with near matches to tap.
4. Use the route:
   - The green button at the bottom always navigates to the next stop that is not delivered.
   - **Mark delivered** crosses a stop off. The arrows move a stop earlier or later and show how much time your order adds, with one tap to go back to the quickest order.
   - **Open in Google Maps** gives the whole route. With more than three in-between stops it is split into consecutive links, because a phone browser keeps only three. A whole-route link is also offered for up to nine in-between stops; it only works when it opens in the Google Maps app.
   - Untick **Return to H-E-B after the last delivery** for a one-way run.

Two orders going to the same address become one stop.

## How the order is worked out

Up to 15 drop-offs, every possible order is compared and the quickest is used. Above 15, the planner starts from nearest-stop routes and keeps improving them; the result is close but not guaranteed quickest, and the page says so. Times are ordinary driving speeds with no live traffic.

## Settings

The top of the script in `index.html` has the values you might change:

- `KNOWN_STORES`: Favor often shows a store by name with no street address. Add each store you run from as a name to look for and the address to use. Greenlawn Crossing is already there.
- `EXACT_LIMIT`: how many drop-offs get the compare-everything treatment.
- `MIN_SCORE`: how strict address matching is.

## What leaves the phone

| Data | Goes to | When |
|---|---|---|
| Screenshots | Nowhere. They are read in the browser. | |
| Customer names | Nowhere. They only label the list and the map. | |
| Each address you confirm | Esri (address lookup) | Find quickest order |
| The map points of the stops | OSRM demo server (drive times, route line) | Find quickest order, reordering |
| The addresses, in order | Google Maps | When you open or share a link |
| The map area around the stops | OpenStreetMap tile servers | When the map shows |

The only thing saved on the phone is the pickup store, and only while **Remember this store on this phone** is ticked. Do not commit real screenshots, customer names or addresses to this repository: it is public, and Favor's Runner guidelines require delivery details to stay confidential.

## Services it depends on

The page needs a connection for all of these, and none comes with a guarantee.

- **Esri address lookup**, without a key. Esri's current documentation asks for a key; the keyless address still works but could stop. If lookups start failing for every address, this is the first thing to check.
- **OSRM demo server** for drive times. Its policy is non-commercial use, at most one request a second, no uptime promise. The page spaces its requests to respect that and credits OSRM and OpenStreetMap.
- **OpenStreetMap tiles** for the map, **Google Maps** for navigation.
- **jsDelivr** for the map library (Leaflet 1.9.4), the text reader (Tesseract.js 7.0.0) and the Overpass typeface. Versions are pinned and the two scripts loaded by the page carry integrity checks.

## If something stops working

- **Screenshots will not read, on one phone only.** The page has a security policy near the top of `index.html` (the line starting `<meta http-equiv="Content-Security-Policy"`). Very old browsers can refuse the text reader under it. Deleting that one line removes the policy.
- **You add a new service or library.** Add its address to that same policy line or the browser will block it.
- **The map is missing but the route is there.** The map library did not load. The route and links still work.

## Tested

Checked in desktop Chromium at phone size with stand-ins for Esri, OSRM and the map tiles: reading light and dark sample screenshots with the real text reader, building, reordering, marking delivered, one-way routes, 18-stop batches, wrong addresses, a service outage, being offline, and the CDN being unreachable. Not yet checked: Safari on an iPhone, real Favor screenshots, and the live services. Run one real batch before relying on it.
