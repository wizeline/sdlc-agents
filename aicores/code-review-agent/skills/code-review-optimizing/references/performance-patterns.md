# Performance Patterns — Language-Specific Reference

## Python / Django / SQLAlchemy

**N+1 Fixes:**
```python
# Django ORM
User.objects.select_related('profile')       # JOIN for FK
User.objects.prefetch_related('tags')        # separate optimized query for M2M

# SQLAlchemy
session.query(User).options(joinedload(User.profile))
```

**Async:**
```python
# BAD — sequential
result_a = await fetch_a()
result_b = await fetch_b()

# GOOD — parallel (when independent)
result_a, result_b = await asyncio.gather(fetch_a(), fetch_b())
```

## JavaScript / TypeScript / Node.js

**N+1 Fixes:**
```js
// BAD
for (const user of users) {
  const profile = await db.profile.findOne({ userId: user.id });
}

// GOOD
const profiles = await db.profile.findMany({
  where: { userId: { in: users.map(u => u.id) } }
});
```

**Async Parallelism:**
```js
// BAD — sequential (2x latency)
const user = await getUser(id);
const orders = await getOrders(id);

// GOOD — parallel (max latency)
const [user, orders] = await Promise.all([getUser(id), getOrders(id)]);
```

**React Re-render Prevention:**
```jsx
// Memoize expensive computation
const sorted = useMemo(() => items.sort(compareFn), [items]);

// Stable callback reference
const handleClick = useCallback(() => doThing(id), [id]);
```

## Go

**Goroutine Fan-out:**
```go
// BAD — sequential
for _, id := range ids {
    result, _ := fetchItem(id)
    results = append(results, result)
}

// GOOD — parallel with WaitGroup or errgroup
g, ctx := errgroup.WithContext(ctx)
for _, id := range ids {
    id := id
    g.Go(func() error {
        result, err := fetchItem(ctx, id)
        // ...
    })
}
g.Wait()
```

## Database Index Heuristics

Flag missing indexes when you see:
- `WHERE` on a non-primary-key column in a large table
- `ORDER BY` on a non-indexed column
- `JOIN` on columns that aren't foreign keys
- Full-text search without a dedicated index (`LIKE '%term%'` — needs FTS index)

## Memory Leak Patterns

**JavaScript/Node:**
```js
// BAD — listener never removed
emitter.on('data', handler);

// GOOD
emitter.on('data', handler);
// Later:
emitter.off('data', handler);
// Or use: emitter.once() for single-fire
```

**Python:**
```python
# BAD — unbounded cache
cache = {}
def get(key):
    if key not in cache:
        cache[key] = expensive_compute(key)  # grows forever
    return cache[key]

# GOOD — use functools.lru_cache with maxsize, or TTLCache
from functools import lru_cache
@lru_cache(maxsize=1000)
def get(key): ...
```
