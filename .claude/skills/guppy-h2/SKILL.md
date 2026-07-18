---
name: guppy-h2
description: Write, type-check, compile, and run correct Guppy programs targeting Quantinuum's H2 / Helios stack and its Selene emulator. Use whenever the task involves Guppy (guppylang), HUGR, Selene, qnexus job submission, or running quantum circuits on the H2 emulator — including the Quantathon CR 2026 project where Guppy is the recommended SDK. Trigger on mentions of Guppy, @guppy, .check(), .compile(), HUGR, Selene, run_shots, qnexus, "run on H2", or linear/ownership type errors on qubits. Also use when translating a Qiskit circuit to Guppy, since the ownership model and imports differ and are easy to get wrong from memory.
---

# Guppy on the H2 stack

Guppy is Quantinuum's Python-embedded quantum language for the H2/Helios generation. It differs from Qiskit in ways that break code written from stale memory, so verify against these mechanics rather than guessing. When an API detail is uncertain, consult the live docs at docs.quantinuum.com/guppy and docs.quantinuum.com/selene rather than inventing a method name.

## Core mental model

- A `@guppy` function describes a program (with measurement-dependent control flow), not a static circuit. This is more powerful than a Qiskit `QuantumCircuit` — you get real `if`/`for`/`while` on measurement outcomes.
- Compilation target is **HUGR** (Hierarchical Unified Graph Representation), an intermediate representation. You type-check with `.check()` and compile with `.compile()`.
- Qubits obey **linear / ownership typing**: a qubit is a resource that cannot be silently copied or dropped. It must be consumed by `measure(...)` (or explicitly `discard(...)`). The type checker enforces no-cloning and no-use-after-measure *at compile time* — a major reliability advantage over Qiskit, where those are runtime/logic errors.

## Minimal correct program (verified pattern)

```python
from guppylang import guppy
from guppylang.std.builtins import array, result
from guppylang.std.quantum import qubit, h, cx, measure, measure_array

@guppy
def main() -> None:
    qs = array(qubit() for _ in range(3))
    h(qs[0])
    for i in range(2):
        cx(qs[i], qs[i + 1])
    ms = measure_array(qs)        # consume all qubits
    result("out", ms)             # push to the results stream

main.check()                      # type-check first — cheap, catches ownership bugs
hugr = main.compile()             # produce HUGR
```

Key points a from-memory draft gets wrong:
- Imports live under `guppylang.std.quantum` (gates, `qubit`, `measure`, `measure_array`, `discard`) and `guppylang.std.builtins` (`array`, `result`, `comptime`). There is no monolithic `guppy.circuit`.
- Every allocated qubit must be measured or discarded, or `.check()` fails. This is a feature — do not "fix" it by suppressing the check.
- Output is reported via `result("tag", value)` into a results stream, not returned.

## Running: local Selene vs. Nexus emulator

**Local emulation (Selene, statevector via QuEST):**
```python
from selene_sim import build, Quest, SimpleRuntime
from hugr.qsystem.result import QsysResult

runner = build(hugr)
res = QsysResult(runner.run_shots(Quest(), n_qubits=3, runtime=SimpleRuntime(), n_shots=100))
res.collated_counts()
```
Use this for fast, noiseless development and for exact statevector checks. Selene also offers a `Stim` backend (Clifford) and error-model plugins for noisy emulation.

**H2/Helios emulator via Nexus (closer to hardware, noisy model):**
```python
import qnexus as qnx
job = qnx.start_execute_job(
    programs=[hugr_ref], n_shots=[100],
    backend_config=config,          # HeliosConfig / H2 emulator config
    name="unique-job-name",
)
qnx.job.wait_for(job)               # blocks up to ~900 s
```
Nexus routes through the same TKET-based compiler as the physical device. Capturing an emulator run here is what earns the project's "run on real quantum hardware" rubric points — label it as emulator, not hardware, in the report.

## H2 hardware facts that change how you write circuits

- **All-to-all connectivity.** Any two qubits interact directly via ion shuttling; there is **no SWAP/routing overhead** and no planar-embedding constraint. Do not insert SWAP networks or connectivity-aware layouts — they only add depth. (This is why a physical-connectivity-constraint checker is unnecessary for this project.)
- **Native two-qubit gate is ZZ / arbitrary-angle ZZ (RZZ).** A QAOA cost layer's ZZ interactions map directly to native gates — cheap. Prefer expressing cost terms as RZZ rather than CX–RZ–CX chains when possible.
- Native set also includes single-qubit rotations and a general SU(4) entangler; mid-circuit measurement and qubit reuse are supported (useful for measurement-conditioned programs).
- The hackathon emulator is capped for exact treatment (~26 qubits noiseless); keep instance sizes under that ceiling and treat larger sizes as classical-only scaling points.

## Common Guppy errors and fixes
- *"Linear variable used more than once" / "not used":* you copied or dropped a qubit. Route it through `measure`/`discard` exactly once.
- *Gate not found / wrong import:* check `guppylang.std.quantum`; some gates also have a functional form under `guppylang.std.quantum.functional` (e.g. `h` returning the qubit).
- *`.compile()` fails but logic looks fine:* run `.check()` alone first to isolate a type error from a compile/backend issue.
- *Optimization:* `main.compile(opt_level=...)` controls TKET-based optimization; intrusive quantum passes are off by default to preserve fault tolerance.

## SDK statement (project deliverable)
The project requires a ≤200-word note on the chosen SDK. Record honestly as you go: what worked in Guppy (ownership safety, native RZZ, real control flow), what did not (ecosystem maturity vs. Qiskit for optimization helpers), and what was missing. If Guppy proves infeasible in the time budget, documenting the attempt honestly still scores — do not fabricate a smooth experience.

## Reference
- `references/guppy_cheatsheet.md` — import map, gate list, ownership rules, and a Max-Cut cost-layer snippet in Guppy.
