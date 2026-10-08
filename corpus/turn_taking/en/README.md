# turn_taking/en

This local corpus package holds the English audio assets for the current
`lab_reference_en` runnable scope in Convo Bench.

Scope:

- Included cases: `R00`, `R01`, `R13-R17`, `I01-I04`
- Excluded cases: `R05`, `R06`

Layout:

- `audio/` - wav assets
- `manifest.yaml` - package summary and scope metadata
- `files.csv` - per-file inventory with size and sha256

Populate or refresh this package with:

```bash
uv run python scripts/sync_turn_taking_corpus.py \
  --source-root /path/to/convoAI
```

Run the suite against this package with:

```bash
uv run --extra analysis-minimal convo-bench lab run lab_reference_en \
  --languages en \
  --asset-root corpus/turn_taking/en
```

This package is part of the public audio corpus and is available under
[CC0 1.0 Universal](../../LICENSE).
