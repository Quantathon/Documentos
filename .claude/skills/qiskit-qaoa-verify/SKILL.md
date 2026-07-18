---
name: qiskit-qaoa-verify
description: Write and verify idiomatic, non-deprecated Qiskit code for QAOA / Max-Cut / QUBO optimization, and catch stale-API errors before they run. Use whenever the task uses Qiskit, qiskit-optimization, qiskit-algorithms, primitives (Sampler/Estimator), SparsePauliOp cost Hamiltonians, or transpilation for a QAOA workflow — including the Quantathon CR 2026 project where Qiskit is the fallback/primary SDK. Trigger on any Qiskit import, on "QAOA in Qiskit", on optimizer setup (COBYLA/SPSA/BFGS), and especially whenever code imports from qiskit.algorithms, since that path is removed and is the top failure mode. Prefer verifying import paths and primitive versions over trusting recalled snippets.
---

# Qiskit QAOA — idiomatic and version-correct

Qiskit's optimization stack was restructured; a large fraction of QAOA code online and in model memory is **broken against current Qiskit**. This skill's first job is to stop stale imports from shipping. When unsure of the current path, web-search the qiskit-optimization / qiskit-algorithms release notes rather than trusting a remembered snippet.

## The deprecation traps (verified) — check these first

1. **`qiskit.algorithms` is gone.** `from qiskit.algorithms import QAOA` / `VQE` no longer works. Current options:
   - `from qiskit_algorithms import QAOA` (separate `qiskit-algorithms` package), or
   - use **qiskit-optimization ≥ 0.7**, which migrated the eigensolvers (VQE, QAOA, SamplingVQE, NumPyMinimumEigensolver) and optimizers (COBYLA, SPSA, SciPyOptimizer) in-house and **no longer depends on qiskit-algorithms**.
2. **V1 primitives are deprecated; use V2.** `BaseSamplerV2` / `BaseEstimatorV2`. For exact, noiseless statevector work use `from qiskit.primitives import StatevectorSampler`. V1 `Sampler`/`Estimator` still run but emit deprecation warnings and should not go in fresh code.
3. **QAOA on a diagonal (Max-Cut) Hamiltonian uses a Sampler**, not an Estimator — the cost operator is diagonal in the computational basis, so sampling suffices (`SamplingVQE`/`QAOA` are sampler-based). `reps` is the depth parameter p.
4. **Transpilation for hardware/simulators.** With V2 primitives and real backends you must transpile against the target (pass manager) before running; the primitive will not silently route for you. For all-to-all targets (H2) no routing is needed, but the ISA/basis-gate translation still applies.

## Canonical correct skeleton (verify versions in your environment)

```python
import networkx as nx
from qiskit.quantum_info import SparsePauliOp
from qiskit.primitives import StatevectorSampler
from qiskit_optimization.applications import Maxcut          # builds the QuadraticProgram
# QAOA + optimizer: from qiskit_optimization (>=0.7) or qiskit_algorithms — confirm which is installed
from qiskit_algorithms import QAOA
from qiskit_algorithms.optimizers import COBYLA

G = nx.Graph(); G.add_weighted_edges_from([(0,1,1.0),(1,2,1.0),(0,2,1.0)])
qp = Maxcut(nx.to_numpy_array(G)).to_quadratic_program()
qubit_op, offset = qp.to_ising()          # SparsePauliOp cost Hamiltonian (+ constant offset)

sampler = StatevectorSampler(seed=42)     # V2, exact statevector
qaoa = QAOA(sampler, COBYLA(), reps=1)    # reps = p ; 2p parameters
result = qaoa.compute_minimum_eigenvalue(qubit_op)
```

Notes:
- Keep the `offset` from `to_ising()` — the true objective is eigenvalue + offset. Dropping it corrupts the reported cut value (a Layer-1 audit failure).
- Building the cost Hamiltonian by hand? Represent each edge ZZ term as a `SparsePauliOp` Pauli string with the correct qubit indices and check endianness (Qiskit is little-endian: qubit 0 is the rightmost character).

## Code-technique review (idiomatic Qiskit)

- Use `SparsePauliOp` (not dense matrices) for cost Hamiltonians — dense scales as 2^n and defeats the point.
- Build graphs with NetworkX and convert; do not hand-index adjacency matrices error-prone.
- Fix `seed` on the sampler/optimizer and log it; QAOA is stochastic.
- Wrap the optimizer: COBYLA or SPSA are robust starts; if it stalls, warm-start from a grid search over (γ, β) — this is explicitly recommended for the project.
- Separate concerns: graph construction, Hamiltonian build, QAOA run, and result post-processing in distinct functions so each is testable and the single-entry-point reproducibility requirement is easy to meet.

## Verification pass before trusting Qiskit output
1. Import check: no `qiskit.algorithms` imports anywhere.
2. Primitive version: V2 (or explicitly justified V1).
3. Offset retained and added back to the eigenvalue.
4. Endianness: bitstring order matches the graph's node indexing (spot-check one known cut).
5. `reps == p`; parameter count == 2p.
6. Statevector sampler for exact checks; noisy backend/emulator clearly separated and labeled.

Hand any nontrivial derivation or claim produced here to the `quantum-code-audit` skill before it enters a report.

## Reference
- `references/qiskit_migration.md` — old→new import table and a hand-built cost-Hamiltonian example with endianness spelled out.
