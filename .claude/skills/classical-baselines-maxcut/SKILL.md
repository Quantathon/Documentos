---
name: classical-baselines-maxcut
description: Implement and verify the classical Max-Cut baselines that a quantum result must be benchmarked against — brute force (exact), greedy, simulated annealing, and Goemans-Williamson SDP rounding via CVXPY. Use whenever a QAOA/Max-Cut project needs a valid classical reference, the "strongest available classical method", or a fair quantum-vs-classical comparison — the 15% baseline criterion of the Quantathon CR 2026 rubric. Trigger on "Goemans-Williamson", "GW", "SDP relaxation", "greedy Max-Cut", "classical baseline", "CVXPY", or "benchmark against classical". These must run on the SAME instance as QAOA, so build them alongside the quantum pipeline, not after.
---

# Classical Max-Cut baselines

The rubric requires benchmarking against the **strongest available classical method**, cited. For Max-Cut that is Goemans–Williamson. A quantum result with no honest classical baseline scores near zero on that criterion, so these are day-1 deliverables. When quoting a guarantee, confirm its scope with a live source rather than from memory.

## The four baselines and what each is for

1. **Brute force (exact optimum).** Enumerate 2^n assignments for n ≤ ~20. This provides E_optimal, which is what makes the approximation ratio r = E/E_optimal meaningful. Without it you have no exact denominator.
2. **Greedy (~0.5 floor).** Assign each node to the side that maximizes marginal cut given already-placed nodes. Cheap, weak — it's the floor a quantum method should clear.
3. **Simulated annealing.** A strong stochastic classical heuristic; useful as a "what good classical effort achieves" point. Report mean ± std over seeds like any stochastic method.
4. **Goemans–Williamson (≈0.878).** SDP relaxation + random-hyperplane rounding — the citable strong baseline. This is the one the rubric's "strongest method" language points at.

## Goemans–Williamson via CVXPY (verified approach)

GW solves an SDP relaxation, then rounds by a random hyperplane. The guarantee **0.87856 holds for non-negative edge weights**; state that scope.

```python
import cvxpy as cp
import numpy as np

def goemans_williamson(W, rounds=100, seed=0):
    n = W.shape[0]
    X = cp.Variable((n, n), symmetric=True)          # X = v_i · v_j Gram matrix
    constraints = [X >> 0] + [X[i, i] == 1 for i in range(n)]   # PSD, unit vectors
    # maximize (1/2) sum_{i<j} W_ij (1 - X_ij)  == expected cut of the relaxation
    obj = cp.Maximize(0.25 * cp.sum(cp.multiply(W, (1 - X))))
    cp.Problem(obj, constraints).solve()
    # recover unit vectors from X, round with random hyperplanes, keep best cut
    L = np.linalg.cholesky(X.value + 1e-9 * np.eye(n))
    rng = np.random.default_rng(seed)
    best_val, best_assign = -np.inf, None
    for _ in range(rounds):
        r = rng.standard_normal(n)
        assign = (L.T @ r >= 0).astype(int)
        val = sum(W[i, j] for i in range(n) for j in range(i + 1, n) if assign[i] != assign[j])
        if val > best_val:
            best_val, best_assign = val, assign
    return best_val, best_assign
```

Verification: on a tiny instance, confirm GW's returned cut never exceeds the brute-force optimum and typically reaches ≥0.878× it over enough rounding rounds. If GW exceeds the exact optimum, there is a bug (usually a factor or sign in the objective).

## Fair-comparison rules
- **Same instance.** GW, greedy, SA, and QAOA all run on the identical node/edge/weight file. Different instances make the comparison meaningless.
- **Same metric.** Report every method's approximation ratio against the same E_optimal, not raw cuts.
- **Compute GW yourself.** Report the ratio *your* GW implementation achieves on the instance, not the literature bound 0.878 quoted in place of a measurement.
- **Statistics** for stochastic methods (GW rounding, SA): mean ± std over seeds.

## Honest framing for the report
- GW's 0.878 is the strongest general guarantee; QAOA does not currently beat it for Max-Cut. State the measured gap.
- Under the Unique Games Conjecture, 0.878 is essentially the best efficiently-achievable ratio for Max-Cut (Khot et al.) — useful context for why GW is the bar, but verify before citing.
- Brute force is exponential; note the instance sizes where it stops being feasible and where you switch to GW/SA as the reference.

References: Goemans & Williamson (1995), JACM 42(6); Diamond & Boyd (2016) CVXPY, JMLR 17(83). Route final numbers through `quantum-code-audit`.
