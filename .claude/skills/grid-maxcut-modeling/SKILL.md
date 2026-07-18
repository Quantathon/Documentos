---
name: grid-maxcut-modeling
description: Turn a real electrical grid (e.g. Costa Rica's ICE transmission network) into a weighted graph for fault-zone partitioning as Max-Cut, with defensible edge weights, physical constraints (generation-demand balance, critical-load protection), an honest H3 spatial-aggregation methodology, and articulated SDG 7/9/13 impact. Use whenever the task involves modeling a power grid for the Quantathon CR 2026 challenge, choosing what nodes/edges/weights mean, connecting the optimization to real-world grid resilience, or writing the SDG-impact section. Trigger on "ICE grid", "power grid graph", "fault-zone partitioning", "islanding", "critical loads", "H3 hexagons", "SDG impact", or "grid data to graph". Keep the modeling honest — document every simplification.
---

# Modeling the power grid as a Max-Cut graph

The point of this skill is a **defensible** grid model: every node, edge, and weight traceable to real data or a stated assumption, and every simplification documented. Real grid data lifts the SDG-impact score; fabricated or hand-waved topology is a red flag. When using ICE open data, cite the portal and record exactly what you kept and dropped.

## What the graph means (fix this first)

- **Nodes** = substations / regions / aggregated grid zones. Give them real names where possible.
- **Edges** = transmission lines that physically exist between those nodes. **Edges come from real line topology, not spatial proximity** — two geographically close nodes are only adjacent if a line connects them. This distinction is central and must be stated explicitly; conflating adjacency with topology is a common and penalizable error.
- **Weights** = line importance/capacity (e.g., derived from line capacity, load carried, or a documented proxy). State the derivation.
- **The cut** = a fault-zone partition: edges *cut* are the tie-lines that would open to island the network into self-contained zones. Max-Cut with these weights finds a partition that separates the network along a meaningful set of tie-lines.

## Data source
Costa Rican Electricity Institute (ICE) open-data portal: `datos-ice-se.opendata.arcgis.com`. Build a simplified transmission graph from it. Version the resulting graph file with: nodes, edges, weights, source URL, and **every simplification applied** (nodes merged, lines dropped, weight proxy used).

## Instance sizing (matches the quantum ceiling)
- Core instance: **8 nodes** (real named substations/regions) — small enough to brute-force verify, big enough to be non-trivial.
- Scaling instance: **12 nodes**; a larger **16–20 node** instance for scaling analysis, staying under the emulator's ~26-qubit exact-treatment ceiling.

## H3 spatial aggregation — a documented method, not a black box
Uber's H3 hexagonal grid is legitimate **only as an aggregation/visualization layer**, with two bounded roles:
1. **Aggregation:** grouping fine-grained grid points into supernodes for the 12- and 16–20-node instances — a reproducible territorial-binning method.
2. **Visualization:** the interactive map for the pitch (remove a line → network re-islands, per-island balance and renewable-share panel).

**Do not** let H3 hexagon adjacency define graph edges — that would replace real line topology with spatial neighbors and break the model's meaning. Edges always trace to lines. State this caveat wherever H3 appears.

## Physical constraints (as a measured extension, not the base formulation)
Keep the core Max-Cut pure (comparable to GW). Add constraints as a *separate* extension so the comparison stays clean:
- **Generation–demand balance:** each island should be able to roughly supply its own load. Encode as a penalty on partitions that isolate demand from generation.
- **Critical-load protection:** hospitals, water pumping, emergency services should stay energized — penalize partitions that strand them.
Report feasible-solution percentage vs. p alongside the approximation ratio, and compare the constrained partition against pure Max-Cut. This maps to the "constraint mixers / feasible subspaces" optional extension.

## SDG impact — articulate the causal chain (don't just name-drop)
The rubric rewards a *specific sub-target + causal chain*, not a generic mention. Concretely:
- **SDG 7 (affordable/clean energy):** fault-zone partitioning contains faults in defined zones, so outages hit smaller portions of the network → more consistent electricity access; islanded microgrids reduce renewable curtailment.
- **SDG 9 (infrastructure/innovation):** partitioning enables incremental, self-healing buildout — communities connect to a resilient segment without waiting for the whole national grid; drives grid-management software and protective-relay innovation.
- **SDG 13 (climate action):** a partition-and-self-heal grid absorbs variable renewables and shortens weather-driven outages, cutting reliance on diesel backup and protecting critical infrastructure during climate disasters.
Tie the *specific* partition your algorithm produces to one concrete outcome (e.g., "this cut keeps hospital X's island generation-balanced during a line-Y fault").

## Honest limitations to pre-write
- Instances are aggregated simplifications of the real ICE network; large-instance edges may use a documented connectivity proxy — label it as such.
- Static topology; no real-time load dynamics modeled.
- Max-Cut partition is a planning heuristic, not an operational relay scheme.

Route the graph file and any impact numbers through `quantum-code-audit`. Pair with `maxcut-qubo-qaoa` for the optimization and `classical-baselines-maxcut` for the reference.
