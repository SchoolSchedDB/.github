# School Schedules Database

**Day-level US school calendars — is school in session, for every district, on any date.**

Observed where we collect it, estimated where we can't, and *every row carries the method it
came from* so your models know how much to trust the feature.

---

### What it is

- **Day-level, every US district** — ~12,000 districts across 49 states + DC, one row per
  district per day.
- **Three school years** — 2024–25 through 2026–27, accumulating. School calendars vanish from
  the web when the year ends; this is the archive of what they said.
- **Honest by design** — every row carries `source_method` and a `confidence` score. Estimates
  are never presented as observations.
- **Built for pipelines** — clean JSON and CSV, stable schema, NCES IDs on every row so it joins
  straight onto Census, enrollment, or your own geodata.

### Who it's for

Data teams who need a school-calendar signal: demand forecasting, traffic and mobility models,
staffing, retail, energy, public health.

---

### Try it — free sample, no key

```bash
curl https://api.hazeydata.ai/ssd/v1/sample/districts
```

Returns the 100 largest districts with their IDs, states and enrollment. This is the district
index — calendar days need a key.

### A calendar row

```bash
curl -H "Authorization: Bearer ssd_live_..." \
  "https://api.hazeydata.ai/ssd/v1/days?district_id=TX_4823640&school_year=2026-2027"
```

```json
{
  "district_id": "TX_4823640",
  "district_name": "HOUSTON ISD",
  "state": "TX",
  "enrollment": 184109,
  "date": "2027-03-08",
  "school_year": "2026-2027",
  "is_in_session": 0,
  "day_type": "BREAK",
  "break_name": "Spring Recess",
  "confidence": 0.98,
  "source_method": "official_calendar_pdf_verified"
}
```

District IDs are `{STATE}_{NCES_ID}`. `is_in_session` is `1` (full day), `0.5` (half day),
`0` (no school) — or **`-1`, the no-basis sentinel**: no basis has been established for that
day, which is *we don't know*, never *closed*. For in-session rows, students in session is
`enrollment × is_in_session`; **never sum `is_in_session` across days** — count days where it
is `> 0` instead, or `-1` rows will subtract from your total.

---

### Telling observed from estimated

**There is no `estimated` boolean.** Derive it from `source_method`:

| `source_method` | Meaning |
|---|---|
| `observed` · `official_calendar_pdf_verified` · `r1_extract_driver` · `annotation_extract` · `human_anchor_email` | Read from the district's own published calendar |
| `deterministic` | Derived by rule — weekends and fixed holidays |
| `legacy` | Carried forward from an earlier collection — treat as estimated |
| `inferred` · `state_median_imputation` | **Estimated.** Treat as a guess and filter on `confidence` |
| `unresolved` | **No basis** — always `is_in_session = -1`, `day_type = "UNKNOWN"`, `confidence = 0.0`. An honest "we don't know" |

Most 2026–27 dates are currently estimated. That is stated plainly rather than hidden, because a
silently wrong calendar is worse for your model than a labelled uncertain one.

### Bulk export

```bash
curl -H "Authorization: Bearer ssd_live_..." \
  "https://api.hazeydata.ai/ssd/v1/export?format=csv&state=TX&school_year=2026-2027"
```

---

### Access

Full access is **$99/month flat** — every district, every day, JSON and CSV, updated daily.
Price locked at signup.

### Links

- Website — https://schoolschedulesdatabase.com
- API docs — https://schoolschedulesdatabase.com/api/
- For agents — https://schoolschedulesdatabase.com/llms.txt
- X — https://x.com/SchoolSchedDB
- Instagram — https://instagram.com/SchoolSchedDB

*Not affiliated with any school district or the Department of Education.*
