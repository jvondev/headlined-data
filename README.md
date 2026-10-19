# Headlined Open Data Lake (`headlined-data`)

> Public Edge-Replicated Binary News Stream Repository for Headlined.

This repository serves as the public static data store for [Headlined](https://headlined.app). It stores daily news streams compressed in binary GZIP Tab-Separated Value (`.tsv.gz`) format.

---

## Architecture & Schema

Each file is stored under its corresponding calendar year:
```text
rss-data-YYYY/YYYY-MM-DD.tsv.gz
```

### Data Schema (TSV)
When decompressed using standard GZIP, each file contains a header row followed by tab-delimited rows:

| Field | Type | Description |
| :--- | :--- | :--- |
| `title` | string | Sanitized headline without HTML or tab characters |
| `description` | string | Executive semantic summary (truncated to top paragraphs) |
| `url` | string | Canonical source article URL |
| `image` | string | Validated high-resolution thumbnail URL |
| `topic` | string | Primary resolved taxonomy (Tech, Finance, Auto, Health, Politics, etc.) |

---

## Client Usage Example (Browser / Node)

### In Browser via `DecompressionStream`:
```javascript
const res = await fetch('https://raw.githubusercontent.com/jvondev/headlined-data/main/rss-data-2026/2026-10-19.tsv.gz');
const ds = new DecompressionStream('gzip');
const text = await new Response(res.body.pipeThrough(ds)).text();
const lines = text.trim().split('\n');
console.log(`Loaded ${lines.length - 1} stories.`);
```

---

## Cryptographic Verification

Every file in this repository is cryptographically tracked in [`manifest.sha256`](./manifest.sha256). You can verify data integrity using:
```bash
sha256sum -c manifest.sha256
```

---

## License
The data feed indices are provided under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/). All underlying articles remain the intellectual property of their original publishers.
