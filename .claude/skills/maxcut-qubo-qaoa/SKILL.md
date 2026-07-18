---
name: maxcut-qubo-qaoa
description: Build and verify the full Max-Cut → QUBO → QAOA pipeline correctly, with fixed sign conventions, brute-force verification, approximation-ratio computation, multi-seed statistics, and p-scaling. Use whenever the task is formulating a weighted-graph Max-Cut as a QUBO/Ising problem, running QAOA over it, computing r = E_QAOA/E_optimal, plotting r vs. p, or reporting QAOA results — the core of the Quantathon CR 2026 fault-zone-partitioning challenge. Trigger on "Max-Cut", "QUBO", "approximation ratio", "r vs p", "QAOA scaling", or any weighted-graph partitioning-as-optimization request. Use this to get the formulation and statistics right, then route the artifacts through quantum-code-audit.
---

# Max-Cut → QUBO → QAOA, done rigorously

This skill enforces the exact pipeline the project rubric rewards: a verified formulation, honest statistics, and ratios (never raw cost). The failure modes are known and cheap to prevent — build defensively.

## Fix conventions up front (write them down)

Before any code, commit in writing:
- **Objective:** Max-Cut *maximizes* total weight of cut edges. QUBO solvers *minimize* xᵀQx. Decide how you bridge (minimize the negative, or maximize the cut objective) and keep it fixed everywhere.
- **Variables:** x_i ∈ {0,1} = which side of the partition node i is on. Cut edge (i,j) contributes w_ij iff x_i ≠ x_j.
- **Ising map:** x_i = (1 − z_i)/2, z_i ∈ {−1,+1}. Track the constant offset — it matters when comparing to a brute-force optimum.

The single most common bug is a sign flip that silently returns the *worst* cut. The defense is the brute-force check below, run on day 1.

## Pipeline steps (each with its verification)

### 1. Graph
Weighted graph, 6–12 nodes for the core instance. Store nodes, edges, weights, and provenance in a versioned data file. For the project, derive from real ICE grid data (see the `grid-maxcut-modeling` skill) — real data lifts the SDG score.

### 2. QUBO / Ising formulation
Max-Cut objective: maximize Σ_{(i,j)∈E} w_ij (x_i + x_j − 2 x_i x_j). Expand to xᵀQx form.
**Verify:** on a 6–8 node instance, brute-force all 2^n assignments; confirm the QUBO energy ordering matches the direct cut-value ordering, up to the offset. Document each Q term.

### 3. Classical baselines (same instance)
Compute brute-force optimum (enables exact r), greedy (~0.5 floor), and Goemans–Williamson (~0.878, the strongest classical reference the rubric demands). Use the `classical-baselines-maxcut` skill. These are day-1 deliverables, not afterthoughts.

### 4. QAOA
Depth-p circuit, 2p parameters, standard mixer + uniform-superposition start. Cost layer = RZZ per edge (native on H2); mixer = RX(2β). Optimize (γ,β) classically with COBYLA/SPSA; warm-start from a grid search if it stalls.

### 5. Approximation ratio — the only correct metric
```
r = E_QAOA / E_optimal        # E_optimal from brute force where feasible
```
Never report raw cost. Report r for each p.

### 6. Statistics — mandatory
For each p ∈ {1,2,3}, run **≥5 seeds** with different initializations. Report **mean ± std** of r. A single run, or best-of-N, is cherry-picking (a rubric red flag). Plot r vs. p **with error bars**, with GW and greedy as horizontal reference lines.

### 7. Scaling
Repeat across ≥2 instance sizes (e.g., 8 and 12 nodes; a larger size for classical-only extrapolation). Label extrapolation as extrapolation. Stay under the emulator's exact-treatment ceiling (~26 qubits) for quantum runs.

## Approximation-ratio guarantees — state them honestly

- The **0.6924 p=1 guarantee is for 3-regular graphs only**. A grid graph is generally neither 3-regular nor unweighted, so do **not** claim that bound for it. Report the *empirical* ratio you measure, and cite the 3-regular result only as context.
- QAOA does not beat GW for Max-Cut on any known instance; at p=1 its guaranteed ratio is strictly below GW's 0.878. Put this in the (mandatory) limitations section, with your measured gap and error bars — do not hide it.
- Warm-start reframing (project-specific): treat warm-start as a *controlled experiment* — "how much does the QAOA–GW gap narrow at fixed depth when seeded with a classical solution?" — not as a claim of beating GW. Report the effect whether or not it helps. Implementation mechanics and the full framing rules live in the `warm-start-qaoa` skill.

## Deliverable checklist (maps to the rubric)
- [ ] Graph data file (nodes, edges, weights, source, simplifications).
- [ ] QUBO with documented penalty/offset terms, brute-force-verified.
- [ ] r-vs-p plot with error bars; GW + greedy reference lines.
- [ ] Mean ± std over ≥5 seeds per p.
- [ ] Scaling across ≥2 sizes with honest extrapolation.
- [ ] Honest-limitations section (the QAOA<GW gap, emulator≠hardware, aggregated-instance caveats).
- [ ] Single entry point regenerating every figure from fixed seeds (repo architecture in `reproducibility-pipeline`).

Route the final formulation, numbers, and claims through `quantum-code-audit` before submission; write them up per `report-pitch-deliverables`.

## Reference
- `references/pipeline_recipe.md` — worked brute-force verifier, ratio computation, and the statistics/plotting pattern.
