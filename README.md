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
      "submitted_by": "github-username",
      "vehicle_class": "deprecated, superseded by category + manual_type below -- kept for now so existing consumers (registry-browse.js's type filter) don't break; removed once they're migrated",
      "category": "one of manual-types.json's category ids (garage/marina/hangar/farm/home/hobby)",
      "manual_type": "one of that category's own type ids in manual-types.json -- always includes an \"other\" fallback, never free text"
    }
  ]
}
```

## Manual types

`manual-types.json` is the single source of truth for every valid
`category`/`manual_type` pair -- see `blayde-manual`'s `ROADMAP.md` for
the full design (why categories exist, the size cap and reasoning
behind each category's type list, and how something filed under
`"other"` gets promoted to a real type or recategorized later). Every
consumer (the indexer's confirm-vehicle step, `registry-browse.js`'s
filter, mosaic template selection) reads this file directly rather than
hardcoding its own copy, so adding a type to an existing category is a
one-line data change here, not a code change anywhere else.

Each category's `types` array holds `{id, label}` objects, not bare
strings -- `id` is the value stored on a `registry.json` entry's
`manual_type`, `label` is what's shown in dropdowns and filters. Kept
separate because a handful of ids (`atv-utv`, `ham-radio`) don't read
well when mechanically title-cased.
