# Trading Journal Data

Public JSON store of every signal fired by the SMT + QT bots.

- `signals.json` — chronological append-only log (last 5000 entries)
- `.github/workflows/append_signal.yml` — the workflow bots dispatch to add entries

Consumed by the trading-journal React dashboard.
