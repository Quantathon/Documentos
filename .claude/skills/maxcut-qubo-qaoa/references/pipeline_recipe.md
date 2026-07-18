# Pipeline recipe: verifier, ratio, statistics

## Brute-force verifier (day-1, catches sign bugs)
```python
import itertools, numpy as np

def cut_value(assignment, edges):
    return sum(w for (i, j, w) in edges if assignment[i] != assignment[j])

def brute_force_maxcut(n, edges):
    best_val, best_x = -np.inf, None
    for bits in itertools.product([0, 1], repeat=n):
        v = cut_value(bits, edges)
        if v > best_val:
            best_val, best_x = v, bits
    return best_val, best_x

def qubo_energy(x, Q):                      # x: 0/1 vector, Q: n x n
    x = np.asarray(x); return float(x @ Q @ x)

# CHECK: over several random bitstrings, confirm that ordering by cut_value
# is the reverse (or same, per your convention) of ordering by qubo_energy,
# and that argmax(cut) corresponds to the intended QUBO optimum.
```

## Approximation ratio
```python
def approx_ratio(e_measured, e_optimal):
    return e_measured / e_optimal          # both as CUT VALUES, same instance
```
Compute `e_measured` as the expected cut over the QAOA output distribution (or the mean cut of sampled bitstrings), not a single lucky bitstring.

## Multi-seed statistics + plot
```python
import numpy as np
seeds = [0, 1, 2, 3, 4]                     # >= 5
ratios_by_p = {}                            # p -> list of r over seeds
for p in [1, 2, 3]:
    rs = []
    for s in seeds:
        r = run_qaoa_once(instance, p=p, seed=s)   # returns approximation ratio
        rs.append(r)
    ratios_by_p[p] = (np.mean(rs), np.std(rs))

# plot: x = p, y = mean r, yerr = std; add horizontal lines for GW and greedy ratios
```

## Reproducibility wiring
- One `main.py` (or master notebook) regenerates every figure and number.
- Fix and log all seeds. Freeze `requirements.txt`. Version the graph data file with its source.
- The reproducibility criterion multiplies across the whole rubric (failure deducts everywhere), so treat it as a day-1 design constraint.
