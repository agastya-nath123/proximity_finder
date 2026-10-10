# Nearest Locations API

Returns the 10 closest locations of a category, where "closest" = shortest road
distance through the links in `link.txt`, and only locations inside the
straight-line search radius are allowed.

## Endpoint

`GET or POST  /search/`  with fields `lat, long, cat, rad, link`

```
/search/?lat=0.5&long=0.5&cat=bank&rad=0.2&link=link.txt
```

`link` can be: a URL to a .txt file, a file name on the server (`link.txt`),
the file contents as text, or an uploaded file:

```
curl -X POST https://<your-app>/search/ -F lat=0.5 -F long=0.5 -F cat=bank -F rad=0.2 -F link=@link.txt
```

If `link` is left out, the provided `data/link.txt` is used.

Response: `{"ids": [10 IDs], "results": [details...], ...}`.
Add `&format=ids` to get only the array of IDs.
Optional: `k` (number of results, default 10), `tie=id|euclid` (ordering of
locations at equal road distance, default `id`; server-wide default can be set
with env var `TIE_BREAK`).

Interactive docs: `/docs` — the POST /search/ entry has a "Choose File" button
for uploading a link file (Try it out -> fill fields -> choose file -> Execute).

## Run locally

```
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

