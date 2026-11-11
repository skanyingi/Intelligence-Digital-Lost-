# Intelligent Digital Lost & Found: Matching and Recovery System

A prototype for the **Kenya Airways Innovation Challenge 2026**. It replaces manual lost-and-found handling with automated matching, human-verified release and a full audit trail.

**Author:** Sammy, final-year Computer Science student (CUEA), ALX Software Engineering graduate
**GitHub:** github.com/skanyingi · **Portfolio:** portfolio-five-eosin-10.vercel.app
**Demo video:** [add link]

> **Status:** Prototype built and tested on **synthetic data only**. No real customer or KQ data is used. See [Status](#status) for what is built and what is planned.

---

## The problem

Lost and found items at airports are logged and stored manually by security teams. There is no reliable way to link a found item to its owner, so:

- Unclaimed items pile up in storage.
- Staff spend a lot of time searching manually when customers make claims.
- Items are handed to the authorities after 2-3 months, even when the owner could have been found.

## The solution

1. **Intake:** staff log found items (photo, category, description, location, time). Customers file lost reports through a public form.
2. **Matching:** the engine compares each lost report with the found inventory and returns a ranked shortlist with confidence scores.
3. **Verification and release:** staff review the top matches, ask the customer a verification question, then approve and release the item.
4. **Monitoring:** a dashboard shows inventory, pending matches and the days left before each item's disposal deadline.

Staff always make the final decision. The system only suggests matches.

## Architecture

```
Customer portal ──► Lost reports ──┐
                                   ├──► Matching engine ──► Ranked matches + confidence
Staff app ───────► Found items ────┘                              │
                                                                  ▼
                                              Staff verification & release workflow
                                                                  │
                                                                  ▼
                                            Audit log  ·  Notifications  ·  Dashboard
```

*Replace this with a diagram image once you have one: `docs/architecture.png`.*

## Matching approach

Each lost report is scored against each candidate found item:

| Signal | How it is used |
|---|---|
| Category | Hard filter (a lost phone is never compared with a found jacket) |
| Location | Plausible station or area for the item |
| Time | The item must have been found after the loss |
| Description similarity | [TF-IDF / sentence embeddings, update to what you used] |
| Distinguishing details | Brand, colour and unique marks get extra weight |
| Photo similarity | *Planned (stretch goal)* |

The signals are combined into a single confidence percentage: `[describe your weighting]`.

## Results (synthetic data)

Tested on a synthetic set of **[N]** lost and found pairs.

| Metric | Result |
|---|---|
| Correct item ranked #1 | [X]% |
| Correct item in top 3 | [X]% |
| Average staff review time per claim | [X], if measured |

These results come from synthetic data and show that the method works. They do not predict accuracy on real KQ data. Tuning on real item categories is part of the proposed pilot.

## Screenshots

| Found item logging | Lost report form | Match shortlist | Dashboard |
|---|---|---|---|
| ![](docs/found-item.png) | ![](docs/lost-report.png) | ![](docs/matches.png) | ![](docs/dashboard.png) |

## Proposed pilot

- **Scope:** one airport station, co-built with KQ's security and baggage teams.
- **Duration:** 4-6 weeks.
- **Activities:** adapt item categories and workflow to KQ's process, tune the matching on real descriptions, train staff.
- **Success metrics** (from the KQ brief): recovery rate, backlog reduction, staff time saved, share of items matched before the disposal deadline.

## Tech stack

- **Backend:** Django, Django REST Framework
- **Frontend:** React
- **Database:** [SQLite / PostgreSQL]
- **Matching:** [scikit-learn / sentence-transformers]

## Getting started

```bash
git clone https://github.com/skanyingi/[repo-name].git
cd [repo-name]

# Backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py loaddata sample_data    # synthetic data
python manage.py runserver

# Frontend (in a second terminal)
cd frontend
npm install
npm start
```

*Update the commands to match your actual project.*

## Status

- [ ] Found item logging
- [ ] Lost report portal
- [ ] Text-based matching with confidence scores
- [ ] Staff verification and release workflow
- [ ] Audit trail
- [ ] Dashboard with disposal-deadline indicator
- [ ] Email/SMS notifications
- [ ] Photo-based matching (stretch goal)

*Tick each box as you finish it, so the README only claims what exists.*

## Limitations and next steps

- Built and evaluated on synthetic data only.
- No integration with KQ systems yet. Integration would be scoped during the pilot.
- Matching quality on real, messy descriptions still needs to be validated.

## Contact

Sammy: samuelkanyingi2016@gmail.com · github.com/skanyingi
