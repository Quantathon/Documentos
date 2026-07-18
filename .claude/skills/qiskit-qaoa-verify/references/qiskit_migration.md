# Qiskit old → new (verify current state in release notes; APIs move fast)

| Stale / broken | Current |
|---|---|
| `from qiskit.algorithms import QAOA` | `from qiskit_algorithms import QAOA` **or** qiskit-optimization ≥0.7 in-house QAOA |
| `from qiskit.algorithms.optimizers import COBYLA` | `from qiskit_algorithms.optimizers import COBYLA` (or qiskit-optimization migrated optimizers) |
| V1 `from qiskit.primitives import Sampler` (deprecated) | `from qiskit.primitives import StatevectorSampler` (V2, exact) or a V2 backend sampler |
| `QuantumInstance` | removed — use primitives |
| `qiskit.opflow` | removed — use `qiskit.quantum_info.SparsePauliOp` |
| dense matrix Hamiltonian | `SparsePauliOp` from Pauli strings |

qiskit-optimization 0.7 highlights (verified): eliminates the qiskit-algorithms dependency by migrating VQE/QAOA/SamplingVQE/NumPyMinimumEigensolver and optimizers (COBYLA, NELDER_MEAD, SciPyOptimizer, SPSA); supports V2 primitives with automatic V1/V2 detection; supports Qiskit 2.x and Python 3.12/3.13.

## Endianness (Qiskit little-endian)
A measured bitstring `'q_{n-1} ... q_1 q_0'` has qubit 0 as the **rightmost** character. When mapping a bitstring back to node assignments for a cut, index from the right. Spot-check: build a trivial 2-node graph, compute the cut by hand for `'01'` and `'10'`, confirm the code agrees.

## Hand-built cost Hamiltonian (Max-Cut, edges (i,j,w))
```python
from qiskit.quantum_info import SparsePauliOp

def maxcut_cost_op(n, edges):
    terms = []
    for i, j, w in edges:
        z = ['I'] * n
        z[n - 1 - i] = 'Z'      # little-endian placement
        z[n - 1 - j] = 'Z'
        terms.append((''.join(z), 0.5 * w))   # (w/2)(Z_i Z_j - I); sign/convention per your derivation
    return SparsePauliOp.from_list(terms)
```
Verify the sign and the constant offset against the brute-force cut on a tiny instance before use (see quantum-code-audit).
