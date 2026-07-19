# School Schedules Database

**An almanac of how many students are in session in each US district — an AI-agent-ready API.**

Observed where we collect it, imputed where we don't, and *every value is labeled* so your models can trust the feature.

---

### Try it — one call, public sample

```bash
curl https://schoolschedulesdatabase.com/sample.json
```

```json
{
  "district": "Houston ISD",
  "nces_id": "4823640",
  "state": "TX",
  "days": [
    { "date": "2026-08-24", "in_session": true,  "students_in_session": 183460, "source": "official_calendar_pdf_verified" },
    { "date": "2026-11-26", "in_session": false, "students_in_session": 0, "break_name": "Thanksgiving", "source": "official_calendar_pdf_verified" },
    { "date": "2027-03-15", "in_session": false, "students_in_session": 0, "break_name": "Spring Break", "source": "imputation", "estimated": true }
  ]
}
```

---

### What it is

- **Day-level, every US district** — is school in session, and how many students, for any date.
- **A historical archive + the upcoming year** — not a single snapshot.
- **Honest by design** — each value carries its `source`; imputed values are flagged `estimated: true`. Never guessed silently.
- **AI-agent-ready** — clean JSON, stable schema, one call. Drop it into any pipeline that needs to know when kids are in school.

### Who it's for

Data scientists and AI agents building anything sensitive to the school calendar — demand forecasting, traffic and mobility models, staffing, retail, energy, public health.

---

### Links

- Website — https://schoolschedulesdatabase.com
- API sample — https://schoolschedulesdatabase.com/sample.json
- X — https://x.com/SchoolSchedDB
- Instagram — https://instagram.com/SchoolSchedDB

*Confirmed dates are printed solid. Estimated dates are always labeled. Weekends are always certain.*
