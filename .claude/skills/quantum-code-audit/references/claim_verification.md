# Verified-facts table (check claims against this; re-verify if stale)

These were confirmed against primary/official sources. Treat them as the
reference the project's claims must match. Dates matter — device specs and APIs
change; re-verify with a live source before a final submission.

## Algorithmic guarantees
| Claim | Correct scope | Source |
|---|---|---|
| QAOA p=1 ratio ≥ 0.6924 | **3-regular graphs only** — NOT arbitrary/weighted graphs | Farhi, Goldstone, Gutmann (2014), arXiv:1411.4028 |
| GW ratio ≈ 0.87856 | Max-Cut with **non-negative** edge weights; SDP relaxation + random-hyperplane rounding; general guarantee | Goemans & Williamson (1995), JACM 42(6) |
| 0.878 essentially optimal for Max-Cut | Under the **Unique Games Conjecture** (conditional, not unconditional) | Khot et al. (2007), SIAM J. Comput. 37(1) |
| QAOA does not beat GW for Max-Cut | True on all known instances; at p=1 the *guaranteed* ratio is strictly below GW | consensus; state as measured gap |

Common LLM error to catch: quoting 0.6924 as a general Max-Cut guarantee, or
quoting GW's 0.878 as the ratio "achieved" without actually running GW on the
instance.

## Quantinuum H2 device facts
| Fact | Value | Source |
|---|---|---|
| Connectivity | **All-to-all** via ion shuttling; no SWAP/routing overhead, no planar-embedding constraint | docs.quantinuum.com H2 data sheet; Quantinuum blog (2024–2025) |
| Native two-qubit gate | ZZ / arbitrary-angle ZZ (RZZ); also general SU(4) entangler | H2 Product Data Sheet |
| Current physical qubit count | H2 upgraded 32 → **56 qubits** (2024); a fully exact "encompassing" emulator is no longer offered at full size | Quantinuum blog, "H-Series hits 56 physical qubits" |
| Hackathon emulator ceiling | Brief states exact treatment up to **~26 qubits** — this is the provided emulator's cap, not the device; RE-VERIFY the exact target in official hackathon docs | Quantathon brief (verify) |
| Mid-circuit measurement + qubit reuse | Supported | H2 Product Data Sheet |

Implication verified: because H2 is all-to-all, a physical-connectivity-constraint
checker is **not needed** for this project — do not insert SWAP networks.

## Guppy / Selene mechanics
| Step | Correct API | Source |
|---|---|---|
| Type-check (linear/ownership) | `fn.check()` | docs.quantinuum.com/guppy getting-started |
| Compile to HUGR | `fn.compile()` | docs; CQCL/selene README |
| Local emulation | `selene_sim.build(hugr)` → `runner.run_shots(Quest(), n_qubits=N, runtime=SimpleRuntime(), n_shots=K)` | docs.quantinuum.com/selene |
| Record outputs | `result("tag", value)` into results stream (not `return`) | Guppy getting-started |
| Backends | `Quest` (statevector), `Stim` (Clifford), error-model plugins for noise | selene docs |
| Imports | gates/qubit/measure under `guppylang.std.quantum`; `array`/`result` under `guppylang.std.builtins` | Guppy docs |

## Qiskit API state (moves fast — re-verify release notes)
| Stale / broken | Current | Source |
|---|---|---|
| `from qiskit.algorithms import QAOA` | `from qiskit_algorithms import QAOA`, or qiskit-optimization ≥0.7 in-house | qiskit-optimization 0.7 release notes; IBM migration guide |
| V1 `Sampler`/`Estimator` | V2 primitives (`BaseSamplerV2`/`BaseEstimatorV2`); `StatevectorSampler` for exact | qiskit-optimization release notes |
| implicit routing | explicit transpilation / pass manager against the target | IBM migration guide |
| `reps` = ? | `reps` is the QAOA depth `p` | qiskit-algorithms QAOA docs |
