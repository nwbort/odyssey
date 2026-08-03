# Scheduled scraper

For https://www.eventcinemas.com.au/Cinema/IMAX-Sydney#date=2026-08-29

## Session watcher

`check.py` polls Event Cinemas hourly (`.github/workflows/watch.yml`) and opens an
assigned GitHub issue the first time a date in `watch.json` goes on sale.

Current state lives in [`sessions/README.md`](sessions/README.md).

### Why not scrape the page?

`#date=2026-08-29` is a URL fragment, so it is never sent to the server —
`./scrape.sh` always retrieves the default page, and that page only server-renders
a rolling three-day block of sessions. The date picker is backed by a JSON endpoint:

```
GET https://www.eventcinemas.com.au/Cinemas/GetSessions?cinemaIds=96&date=YYYY-MM-DD
```

`Data.Dates` is the cinema's full on-sale calendar; showtimes for the requested date
are under `Data.Movies[].CinemaModels[].Sessions[]`.

### Watching another date or movie

Edit `watch.json`. `cinemaId` and `movieId` are the ids used by that endpoint
(96 = IMAX Sydney, 19797 = The Odyssey). Dates already announced are recorded in
`sessions/alerts.json`; delete an entry to be notified about it again.
