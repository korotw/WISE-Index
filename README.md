# WISE-Index
<p/>A lightweight scoring tool for fire and EMS technology systems, built around the W.I.S.E. evaluation framework. </p>
<p/>Think of it as IMDB for fire/EMS software: rate systems on 10 weighted criteria, see how they stack up by category, and export the results.</p>

---

## What's in this repo

| File | What it is |
|------|------------|
| `wise_tool.html` | The scoring tool. Single file. Open it in a browser. |
| `WISE_v1.1_Reference_Guide.docx` | The methodology and rubric reference. |
| `WISE_Tool_User_Guide.docx` | Step-by-step user guide. |
| `README.md` | This file. |

That's it. No build step, no dependencies, no install.

---

## Quick start

### Use it locally
1. Download `wise_tool.html`.
2. Double-click. Your default browser opens it.
3. Click **Load sample data** on the Systems screen, or **Add system** to start fresh.
4. Score, weight, and export.

### Host it
- **SharePoint**: Upload to a document library. Some tenants block inline scripts; if so, use a library that allows scripts or pick another option below.
- **GitHub Pages**: Drop `wise_tool.html` in your repo, enable Pages, and the URL is live.
- **Static host**: Netlify, Cloudflare Pages, Azure Static Web Apps, S3+CloudFront. Upload the one file.
- **Shared drive**: Drop it in a network folder. Anyone who can read the folder can use it.

All data lives in the browser's local storage. Nothing leaves the user's machine.

---

## How scoring works

10 criteria, each scored 1 to 10, each with an editable weight (0 to 3). Default weight is 1.0.

```
Weighted Score = sum(score × weight) / sum(weight)   for criteria with weight > 0 and a 1-10 score
Overall Score  = the weighted score across all scored criteria
Category Rank  = descending sort of Overall Score within a category
```

A weight of 0 excludes the criterion. Missing scores are excluded from the denominator (a missing score does not drag the total down).

The math is documented in Section 6 of the User Guide and in the Help screen of the tool.

---

## What's included in the tool

- **Systems screen** — add, list, and remove systems. See scoring progress and QA flags.
- **Score a System** — enter 1-to-10 scores and evidence for each criterion.
- **Weights** — adjust criterion weights. Four built-in presets (Integration, Security, Innovation, Cost focus).
- **Dashboard** — summary metrics, per-category leaderboards, full matrix, radar chart for comparison.
- **QA Checks** — flags missing evidence, out-of-range values, and stale entries (>180 days).
- **Rubric** — on-screen 1-to-10 scale and criteria reference.
- **Import / Export** — JSON for full backup, CSV for the master matrix.
- **Help** — quick start and score math.

---

## Data model

Stored in browser localStorage under the key `wise_state_v1`. Schema:

```json
{
  "systems": [
    {
      "id": "sys_abc123",
      "name": "ESO Fire RMS",
      "vendor": "ESO",
      "category": "Records Management (RMS, Fire)",
      "scores": {
        "integration": { "value": 8, "evidence": "...", "rater": "WJR", "updated": 1714000000000 }
      },
      "created": 1714000000000,
      "updated": 1714000000000
    }
  ],
  "weights": { "integration": 1.0, "data_access": 1.0, "...": "..." },
  "currentRater": "WJR",
  "lastCompare": ["sys_abc123", null, null]
}
```

Export the JSON for backup. Import to restore. Two devices, two separate datasets.

---

## Browser support

Modern browsers only. Tested on:
- Chrome 100+
- Edge 100+
- Safari 15+
- Firefox 100+

Uses CSS Grid, ES6, `fetch`, `localStorage`, and Canvas. No frameworks.

---

## Privacy

- All data lives in the browser's local storage for the page's origin.
- No network calls. No analytics. No tracking.
- Clearing the browser cache or using incognito mode erases data. Export first.
- Two devices have two separate datasets; sync via JSON export/import.

---

## Reference framework

The W.I.S.E. methodology is documented in `WISE_v1.1_Reference_Guide.docx`:

- Section 1: Purpose
- Section 2: Governance and ownership
- Section 3: Scoring rubric (anchored 1-to-10, evidence requirement, price normalization)
- Section 4: Criteria
- Section 5: How to use W.I.S.E. (workflow, score math, multi-rater protocol)
- Section 6: Categories (13 categories with representative systems)
- Section 7: Workbook structure
- Section 8: Next steps

---

## Roadmap

Possible future work, not committed:

- Multi-rater per-criterion tracking with median calculation
- Emerging Capability column for Categories 1 to 12
- Scenario snapshots (named weight configurations)
- Direct Power BI push via web connector
- Optional SharePoint list sync
- Glossary tooltips for acronyms (FHIR, NEMSIS, CJIS, etc.)
- Pre-populated vendor catalog
- Edit history / audit log
- Light/dark mode toggle

---

## Contributing

Fork. Edit `wise_tool.html`. Open a pull request with a description of what changed and why.

For the docx files, edit through the source `.js` build scripts (not the .docx directly) so changes are reproducible. The build scripts use the [`docx`](https://www.npmjs.com/package/docx) package via Node.

```bash
npm install docx
node make_docx.js     # builds the spec
node make_guide.js    # builds the user guide
```

---

## License

TBD by AFESA.

---

## Contact

Maintained by AFESA Data and Technology Workgroup. For methodology questions, vendor corrections, or scoring disputes, see Section 2 of the Reference Guide.
