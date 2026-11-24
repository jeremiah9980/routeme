# Favor route planner

Upload index.html to the root of the main branch. In the repository settings, open Pages, choose Deploy from a branch, select main and /(root), then save. Open the site address shown in Pages after deployment.

Choose Favor screenshots, check the pickup and each delivery address, and build the route. The planner compares round trips for up to eight deliveries and uses a nearest-stop estimate for larger batches. Links with more than three intermediate waypoints are split into consecutive legs for mobile browsers.

The app needs an internet connection for OCR resources, Esri address lookup, OSRM road times and Google Maps. Screenshots are processed in the browser.

Review each suggested customer name before routing. Type the name when Favor shows it on another screen. The numbered map labels and stop list use the confirmed names. Google Maps navigation and the Share route button use addresses only. The page uses Leaflet with OpenStreetMap tiles to display the stops.
