# Blayde Manual -- registry

Maps a PDF's SHA-256 fingerprint to its approved vehicle repo. Read by
every patcher/indexer client directly from this file at
`raw.githubusercontent.com/BlaydeManual/registry/main/registry.json` --
no API, no auth, unauthenticated public reads only.

Starts empty on purpose. The first entry here comes from a real
vehicle's real org approval, not a seed. See the main
[blayde-manual](https://github.com/BlaydeManual/blayde-manual) repo's
`ROADMAP.md`/`LEGAL.md` for the full architecture and reasoning.

## Schema

```json
{
  "vehicles": [
    {
      "vehicle_slug": "make-model-year-range",
      "edition_id": "OEM",
      "vehicle_display_name": "Make Model (Year-Range)",
      "source_identifier": "where the submitter found this manual",
      "source_pdf_sha256": "the fingerprint patchers/indexers match against",
      "repo_url": "https://github.com/BlaydeManual/<vehicle-repo>",
      "status": "approved",
      "submitted_by": "github-username"
    }
  ]
}
```
