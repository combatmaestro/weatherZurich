# Climate Lifehacks Zurich

A small, static web app that displays Zurich weather and filters student tips from `tips.csv`.

## Run locally

The app loads `tips.csv` with `fetch`, so serve the project over HTTP rather than opening `index.html` directly.

With Python 3:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000> in a browser. Alternatively, open the project in VS Code and use the Live Server extension.

The app fetches current and daily weather data from Open-Meteo and refreshes it hourly. Heat days use the daily maximum temperature (30 °C or higher), and ice days use a daily maximum below 0 °C. If the weather API is unavailable, the app displays an error instead of substituting sample weather.

## Tip list

`tips.csv` must contain the columns `id`, `season`, `condition`, `extreme`, and `tip`. Standard comma-separated CSV and tab-separated files are accepted. Seasonal tips are filtered by the current season (or the selected season); tips marked extreme are shown only when their matching weather event applies and are not restricted by season. The app currently includes the 70 tips supplied with the revision.

## Optional AI tips

AI tip generation is optional and requires a server-side `POST /api/tips` endpoint returning JSON in this shape:

```json
{
  "tips": [
    { "text": "A useful tip", "condition": "general", "season": "all", "extreme": false }
  ]
}
```

The endpoint should call Gemini from a backend and keep its API key in a server-side environment variable. `.env.example` is only a placeholder; do not put a real key in this repository or in browser code. A key sent to a browser is visible to anyone using the app. If no backend endpoint is configured or its request fails, the app falls back to the CSV tip list.

This repository is a static prototype and does not include a backend. The private-key approach requires a small serverless function or backend service; if that is outside the project budget, leave AI generation unconfigured and use the CSV list as the default.
