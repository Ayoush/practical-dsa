# Data Structures, But From Production

A series about learning data structures and algorithms by building the problems that need them.

Not LeetCode. There is no "given an array of integers, return..." here. Each episode starts with a
real system — a rate limiter, an audio buffer, a reconciliation loop — built the obvious way until it
breaks. Then we fix it, from scratch, with the data structure that was always the right answer.

---

## The premise

Most DSA material teaches you the solution and then invents a problem to justify it. That ordering is
backwards, and it's why people can implement a binary search but can't recognise the moment they need
one.

So this series inverts it:

1. **Build the system.** Real constraints, real input volume, running code.
2. **Break it.** Load it until latency, memory, or correctness visibly fails. Measure the failure.
3. **Reach for the structure.** Implement it by hand and show the failure disappear.

The measurement step is the whole point. Every episode ends with a number that got better.

## Rules

- **No standard library data structures.** No `deque`, no `bisect`, no `sort` on the hot path we're
  studying. Yes, your language already has these. Implementing them once is how you learn when to
  reach for them.
- **The problem comes first.** No episode opens with the name of the algorithm.
- **Everything runs.** Every directory here is executable, not pseudocode.
- **Benchmarks are committed.** The before/after numbers live in the repo so you can reproduce them
  or argue with them.

## Episode structure

Each episode is a self-contained directory:

```
NN-episode-name/
├── README.md         # the problem, the constraint, the numbers
├── problem/          # the system under test — a real workload, not a test harness
├── naive/            # the obvious implementation
├── solution/         # the from-scratch data structure
└── bench/            # the script that produces the before/after numbers
```

Run any episode's benchmark from its directory:

```bash
cd 01-episode-name/bench
# see the episode README for the exact command
```

## Roadmap

### Part 1 — Arrays

| # | Episode | The system | What breaks | What fixes it |
|---|---------|-----------|-------------|---------------|
| 01 | Sliding window rate limiter | API gateway, 100 req/min per key | Fixed windows let 200 requests through across a boundary | Ring of per-second counters, indexed by `epoch % 60` |
| 02 | Rolling audio buffer | Live transcription, 20ms PCM chunks, last 30s always available | Append-and-slice grows without bound; slice cost climbs | Preallocated circular buffer, wraparound reads |
| 03 | IP-to-geo on the hot path | Country lookup against 300k CIDR ranges, every request | Linear scan eats the p99 budget | Flattened sorted ranges + lower-bound binary search |
| 04 | Percentiles without the samples | p50/p95/p99 per endpoint at 50k req/s | Storing every sample; averaging averages | Fixed-bucket histogram, prefix-sum percentile |
| 05 | Reconciling desired vs. actual state | Syncing 50k records to an external provider every 60s | Nested-loop diff goes quadratic; "delete and recreate" causes churn | Sort both sides, two-pointer merge into create/update/delete |

Later parts (hash tables, trees, heaps, graphs) follow the same format. They'll be listed here as
they're planned — the roadmap only commits to what's actually been designed.

## Who this is for

Engineers who can already ship features and want to understand the layer underneath. If you've ever
merged a PR that worked fine in staging and fell over at 10x traffic, this is aimed at you.

You don't need a CS degree. You do need to be comfortable reading code and running a benchmark.

## Following along

Each episode has a companion video. Links are in the episode READMEs.

Issues and PRs are welcome — especially "your benchmark is wrong and here's why," which makes for a
better episode than anything I'd write alone.

## License

MIT.