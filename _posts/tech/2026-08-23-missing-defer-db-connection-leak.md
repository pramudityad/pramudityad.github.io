---
title: "One Missing defer: How a Go Transaction Leak Exhausted Our DB Connection Pool"
tags: Tech
---

A production incident that started with a single missing `defer` and ended with ~1,300 customers unable to see any products. The root cause was embarrassingly small; the detection took embarrassingly long. Here's the anatomy.

## What happened

The service started returning empty product listings. Underneath, every request was failing with `error acquiring db connection` — the PostgreSQL connection pool was fully exhausted. The service had been degraded for about two hours before mitigation, but the first trigger fired roughly **8 hours earlier** and went unnoticed.

## The root cause chain (5 whys)

1. Product catalog missing → homepage showed empty listings.
2. Service returned "error acquiring db connection" → **connection pool exhausted**.
3. Oldest-transaction age spiked → a DB connection was **never released** after a transaction.
4. In the create-order endpoint, the transaction was opened **before** validation completed. When payment-option validation failed, the error path returned **without rolling back** the transaction.
5. **Root cause:** improper transaction lifecycle management — no `defer` for rollback, so failure paths didn't guarantee cleanup.

## The anti-pattern

```go
// BAD: transaction opened before validation
tx, _ := db.Begin()
if err := validatePaymentOptions(); err != nil {
    return err // tx NOT rolled back — connection leaks
}
// ... work ...
tx.Commit()
```

The fix is the standard Go idiom — roll back immediately after begin:

```go
// GOOD: defer rollback right after Begin
tx, err := db.Begin()
if err != nil {
    return err
}
defer tx.Rollback() // no-op after Commit
if err := validatePaymentOptions(); err != nil {
    return err // defer handles rollback
}
tx.Commit()
```

`tx.Rollback()` after a successful `Commit()` is a harmless no-op, so the `defer` costs nothing and covers every error path, including ones you haven't thought of yet.

## Why it stayed hidden for 8 hours

The code bug was only half the story:

- **No pool metrics on any dashboard** — active vs idle connections and the max threshold were invisible.
- **The alert was too generic**, so it got dismissed instead of investigated.
- **No proactive alert** at, say, >80% pool usage.

And during response: pod auto-restart didn't help because Kubernetes couldn't cleanly restart pods holding open DB connections, and manual restarts released connections temporarily — masking the root cause while the leak refilled the pool.

## Takeaways

1. **Always `defer tx.Rollback()` right after `db.Begin()`** — cheap insurance that covers unknown error paths.
2. **Open transactions as late as possible** — after validation, not before.
3. **Monitor your pool** — active/idle connections, oldest transaction age, and alert before exhaustion, not after.
4. **Beware fixes that mask symptoms** — a restart that "works" is a clue, not a resolution.

A one-line omission, a two-hour outage, and an 8-hour detection gap. Connection lifecycle is boring until it isn't.
