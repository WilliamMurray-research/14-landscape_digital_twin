# 14.0 – Landscape Digital Twin (Julia, SWI-Prolog, Common Lisp, Python)

A symbolic-geometric digital twin framework modelling fragmented landscapes as
a fiber bundle manifold stack, with a dynamic DSL for describing ecological
entities, relations, and rewriting rules. Designed as the convergence point of
the tensor algebra (9.0), Prolog metamorphism (6.0), and Lisp homoiconicity
(7.0) workstreams.

`fiber bundles` `manifold stack` `dynamic DSL` `ecological connectivity`
`digital twin` `symbolic rewriting` `landscape restoration`

---

## Position in Programme

This project sits at **14.0**, immediately after:

- **7.0** – Lisp Metamorphism and Homoiconicity: the macro/rewriting substrate
  the DSL is built on
- **9.0** – Information Representation via Tensors: the mathematical
  representation layer the manifold stack is expressed through

It depends structurally on **6.0** (Prolog metamorphism) for symbolic invariant
reasoning, and on **3.0** (Knowledge Layer) for provenance and cross-domain
coherence. It is a candidate downstream consumer of the Unified Operator
Architecture corpus.

The Miyawaki corridor RL system (documented separately) is a natural downstream
application: this project provides the geometric and symbolic substrate that
system requires.

---

## Motivation

Fragmented landscapes — road-scarred, hydrologically disrupted, ecologically
isolated — are not well-described by flat tensor grids. Their structure is
inherently multi-layered: topography constrains hydrology, which constrains
vegetation, which shapes microclimate, which governs wind. These dependencies
are not independent channels; they are fibers over a common base.

Conventional simulation approaches stack these layers additively. A fiber bundle
formulation is more honest: each layer is a structured object attached to the
base manifold through a connection map that encodes real physical dependency.
The total space captures the coupled system; sections through it represent
ecological states; and the dynamics of restoration are paths through that space.

The Go analogy is instructive. Logging roads are cuts — they reduce the
automorphism group of the landscape graph, isolating patches the way a
ladder-cutting move isolates a Go group. Corridors restore liberties. The RL
agent's task is to find the vital points: the narrow gaps whose closure
maximises connected area per unit of intervention.

This project does not implement the RL agent. It builds the geometric and
symbolic object the agent would operate on.

---

## Architecture

### 1. Base Manifold — Topography

The base manifold **M** encodes the physical landscape: elevation, slope,
aspect, and road network geometry. It is the substrate over which all other
structure is defined. Represented as a Riemannian manifold discretised at 10m
resolution; curvature and gradient fields derived from LiDAR or DEM inputs.

### 2. Fiber Bundle Stack

Each ecological layer is a fiber **Fᵢ** attached to every point of **M**
through a projection πᵢ: Eᵢ → M. The total space Eᵢ = M × Fᵢ (in the trivial
case) carries the state of that layer across the landscape.

| Bundle        | Fiber content                                      | Key connection map        |
|---------------|----------------------------------------------------|---------------------------|
| Hydrology     | Soil moisture, drainage, infiltration capacity     | Slope + road compaction   |
| Vegetation    | Canopy density, successional stage, species mix    | Soil moisture + light     |
| Microclimate  | Temperature, humidity, wind shelter                | Canopy density + aspect   |
| Wind          | Speed, direction, turbulence                       | Topography + canopy       |
| Disturbance   | Fire risk, flood probability, invasion pressure    | All of the above          |

Connection forms between fibers encode how a change in one propagates to
another. These are the mathematically interesting objects: they determine which
restoration trajectories are reachable and which global states are accessible
from a given local intervention.

### 3. Dynamic DSL

A two-layer symbolic system for describing and evolving entities within the
twin.

**Prolog layer** — relational invariants and constraint reasoning:
- What is adjacent to what
- Which logical invariants must hold (connectivity, drainage, legal tenure)
- Queries over the patch adjacency graph
- Constraint propagation when actions are proposed

**Lisp layer** — transformational and rewriting dynamics:
- Macros that rewrite entity definitions as ecological thresholds are crossed
- Successional stage transitions trigger structural rewrites of adjacency
  relations and resistance values
- The DSL evolves with the landscape; a patch at stage 3 is a different
  symbolic object than at stage 1

The DSL is dynamic in the sense that its grammar is not fixed: Lisp macros
extend it in response to system state, and Prolog rules are asserted and
retracted as the landscape changes. This is the direct application of Projects
6.0 and 7.0 to a concrete physical domain.

**Entity types expressible in the DSL:**

```
patch       — habitat unit with state (canopy, soil, stage, area)
road        — linear barrier with resistance and compaction profile
corridor    — planted section with growth dynamics and connectivity contribution
node        — high-density planting anchor; microclimate emitter
gap         — unplanted interval between corridor segments; resistance value
adjacency   — directed relation with movement probability
flow        — hydrological or microclimatic propagation path
```

**Example rewriting rule (conceptual):**

When a corridor node reaches canopy closure threshold → assert new adjacency
relations to neighbouring patches → retract high-resistance gap entries →
propagate humidity spillover to adjacent cells → update ECA estimate.

### 4. Connectivity Metric Layer

Equivalent Connected Area (ECA) computed over the patch adjacency graph:

```
ECA = ΣᵢΣⱼ aᵢ · aⱼ · pᵢⱼ*
```

where pᵢⱼ* is the maximum-product movement probability across all paths, and
resistance values rₖ are derived from the fiber bundle state at each cell.

The isoperimetric ratio P²/4πA penalises elongated corridor geometries. Both
metrics update dynamically as the DSL rewrites in response to ecological change.

### 5. Sensor Integration Interface

The twin maintains correspondence with a real landscape through a structured
update interface. Field observations — LiDAR canopy height, soil moisture
sensors, camera trap detections, microclimate loggers — are ingested as section
updates on the relevant fiber. The Prolog layer validates consistency against
invariants before committing updates. Provenance tracked through the Knowledge
Layer (3.0).

---

## Research Questions

1. What are the algebraic properties of the connection forms between ecological
   fibers, and do they constrain the set of reachable restoration trajectories?

2. Does the automorphism group of the patch adjacency graph provide useful
   information about optimal corridor placement — specifically, do high-symmetry
   landscapes admit simpler RL policies?

3. Can the dynamic DSL express successional dynamics faithfully enough that
   Prolog queries over it reproduce known ecological outcomes from published
   Miyawaki field data?

4. What is the minimal fiber bundle structure sufficient to capture the
   microclimate spillover effects that make Miyawaki corridors ecologically
   effective beyond their physical footprint?

---

## Implementation Sequence

| Phase | Scope                                                          | Language            |
|-------|----------------------------------------------------------------|---------------------|
| 14.1  | Base manifold construction from DEM/LiDAR; curvature fields    | Julia               |
| 14.2  | Fiber bundle stack; connection maps; section representation    | Julia + Python      |
| 14.3  | Prolog relational layer; patch adjacency graph; invariants     | SWI-Prolog          |
| 14.4  | Lisp DSL; entity grammar; macro rewriting rules                | Common Lisp         |
| 14.5  | ECA computation; connectivity dashboard                        | Julia + Python      |
| 14.6  | Sensor integration interface; provenance pipeline to 3.0       | Python + PostgreSQL |
| 14.7  | Full twin integration; synthetic landscape validation          | All                 |

---

## Relationship to Wider Programme

| Project | Dependency type                                              |
|---------|--------------------------------------------------------------|
| 3.0     | Knowledge Layer — provenance, semantic indexing              |
| 5.0–13.0| Hashimoto / spectral chain — graph structure foundations     |
| 6.0     | Prolog metamorphism — DSL invariant layer                    |
| 7.0     | Lisp homoiconicity — DSL rewriting layer                     |
| 9.0     | Tensor algebra — manifold representation                     |
| UOA     | Theoretical spine — local-to-global operator framework       |

Downstream: Miyawaki corridor RL system (unnumbered, post-14.0).

---

## Standards

Instantiates the Project Template Framework (0.0) in full. All artefacts
registered in the Unified Asset Registry. Documentation in Australian Government
Style Manual + AGLC-compliant English (see 10.0).
