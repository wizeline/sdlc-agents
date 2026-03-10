# Concurrency Patterns — Reference

## JavaScript / Node.js

**Race Condition (TOCTOU):**
```js
// BAD — check-then-act, not atomic
const exists = await db.user.findUnique({ where: { email } });
if (!exists) {
  await db.user.create({ data: { email } }); // another request may have created it
}

// GOOD — upsert or unique constraint + catch
await db.user.upsert({
  where: { email },
  create: { email },
  update: {},
});
```

**State after unmount (React):**
```js
// BAD
useEffect(() => {
  fetchData().then(data => setState(data)); // fires after unmount
}, []);

// GOOD
useEffect(() => {
  let cancelled = false;
  fetchData().then(data => {
    if (!cancelled) setState(data);
  });
  return () => { cancelled = true; };
}, []);
```

## Python

**Threading — shared state:**
```python
# BAD
counter = 0
def increment():
    global counter
    counter += 1  # not atomic in CPython under some conditions

# GOOD
import threading
lock = threading.Lock()
def increment():
    with lock:
        counter += 1
```

**Async — partial failure:**
```python
# BAD — second operation may fail after first succeeds
await db.update_balance(user_id, -amount)
await db.create_transaction(user_id, amount)  # if this fails, balance is gone

# GOOD — wrap in transaction
async with db.transaction():
    await db.update_balance(user_id, -amount)
    await db.create_transaction(user_id, amount)
```

## Go

**Map concurrent access:**
```go
// BAD — concurrent read/write to map causes panic
var cache = map[string]string{}
go func() { cache["key"] = "value" }()
go func() { _ = cache["key"] }()

// GOOD — use sync.Map or mutex
var mu sync.RWMutex
mu.Lock()
cache["key"] = "value"
mu.Unlock()
```

**Deadlock pattern:**
```go
// BAD — inconsistent lock order
// goroutine 1: Lock(A) → Lock(B)
// goroutine 2: Lock(B) → Lock(A)  ← deadlock

// GOOD — always acquire in same order: A → B
```

## Database Transactions

**Idempotency via optimistic locking:**
```sql
-- BAD: two concurrent updates both read version=1
UPDATE orders SET status='shipped', version=2
WHERE id=42 AND version=1;  -- first succeeds, second silently fails

-- Handle: check rows_affected == 0 → retry or surface conflict
```

## Error Propagation Anti-patterns

```python
# BAD — swallowed error
try:
    result = risky_operation()
except Exception:
    pass  # caller has no idea something went wrong

# BAD — lost context
try:
    result = risky_operation()
except Exception as e:
    raise RuntimeError("something went wrong")  # original traceback lost

# GOOD — preserve context
try:
    result = risky_operation()
except SpecificError as e:
    logger.error("Failed during X: %s", e)
    raise  # re-raise preserving original traceback
```
