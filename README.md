# 14.0 – Landscape Digital Twin  
*A symbolic–geometric framework for modelling fragmented landscapes as a fiber‑bundle manifold stack, with a dynamic DSL for ecological entities, relations, and rewriting rules.*

---

## Position in Programme

Project **14.0** integrates three upstream workstreams:

- **7.0 – Lisp Homoiconicity**: macro‑driven rewriting substrate for the DSL  
- **9.0 – Tensor Algebra**: manifold and fiber representation  
- **6.0 – Prolog Metamorphism**: symbolic invariants and constraint reasoning

It also consumes provenance and semantic structure from **3.0 – Knowledge Layer**, and provides the geometric/symbolic substrate required by the Miyawaki corridor RL system.

---

## Motivation

Fragmented landscapes — road‑cut, hydrologically disrupted, ecologically isolated — cannot be faithfully represented by flat tensor grids. Their structure is inherently hierarchical:

- topography shapes hydrology  
- hydrology shapes vegetation  
- vegetation shapes microclimate  
- microclimate shapes wind  

These are not independent channels but **fibers over a shared base manifold**. A fiber‑bundle formulation captures these dependencies explicitly: connection maps encode how changes propagate between layers, and sections represent ecological states.

The Go analogy is instructive: roads act as cuts that reduce the automorphism group of the landscape graph, isolating patches the way ladder‑cuts isolate Go groups. Corridors restore liberties. The RL agent’s task is to identify vital points; this project builds the geometric object that agent operates on.

---

## Architecture

### 1. Base Manifold — Topography

The base manifold **M** encodes elevation, slope, aspect, and road geometry. It is represented as a discretised Riemannian manifold (10 m resolution), with curvature and gradient fields derived from LiDAR/DEM inputs.

### 2. Fiber Bundle Stack

Each ecological layer is a fiber **Fᵢ** attached to **M** via projection πᵢ: Eᵢ → M. The total space Eᵢ = M × Fᵢ carries the distributed state of that layer.

| Bundle        | Fiber content                                      | Key connection map                |
|---------------|----------------------------------------------------|-----------------------------------|
| Hydrology     | Soil moisture, drainage, infiltration              | Slope + road compaction           |
| Vegetation    | Canopy density, successional stage, species mix    | Soil moisture + light             |
| Microclimate  | Temperature, humidity, shelter                     | Canopy density + aspect           |
| Wind          | Speed, direction, turbulence                       | Topography + canopy               |
| Disturbance   | Fire risk, flood probability, invasion pressure    | All of the above                  |

Connection forms encode how perturbations propagate between fibers, determining reachable restoration trajectories and global ecological states.

### 3. Dynamic DSL

A two‑layer symbolic system:

**Prolog layer** — relational invariants  
- adjacency, drainage, tenure  
- constraint propagation  
- patch‑graph queries

**Lisp layer** — rewriting dynamics  
- macros rewrite entity definitions as thresholds are crossed  
- successional transitions update adjacency and resistance  
- grammar evolves with landscape state

Entity types:

```
patch, road, corridor, node, gap,
adjacency, flow
```

Example conceptual rewrite:  
Corridor node reaches canopy‑closure → assert new adjacencies → retract high‑resistance gaps → propagate humidity spillover → update ECA.

### 4. Connectivity Metric Layer

Equivalent Connected Area (ECA):

```
ECA = ΣᵢΣⱼ aᵢ · aⱼ · pᵢⱼ*
```

where pᵢⱼ* is the maximum‑product movement probability across all paths, with resistance derived from fiber‑bundle state.  
Isoperimetric ratio P²/4πA penalises elongated corridor geometries.  
Both metrics update as DSL rewrites.

### 5. Sensor Integration Interface

Field observations (LiDAR canopy height, soil moisture sensors, camera traps, microclimate loggers) update the relevant fiber sections. Prolog validates invariants before committing updates. Provenance flows through the Knowledge Layer.

---

## Research Questions

1. Algebraic properties of ecological connection forms and their constraints on restoration trajectories  
2. Whether the automorphism group of the patch adjacency graph informs optimal corridor placement  
3. Whether the dynamic DSL reproduces known successional dynamics from Miyawaki field data  
4. Minimal fiber‑bundle structure required to capture microclimate spillover effects

---

## Implementation Sequence

| Phase | Scope                                                          | Language            |
|-------|----------------------------------------------------------------|---------------------|
| 14.1  | Base manifold construction; curvature fields                   | Julia               |
| 14.2  | Fiber bundle stack; connection maps                            | Julia + Python      |
| 14.3  | Prolog relational layer; invariants                            | SWI‑Prolog          |
| 14.4  | Lisp DSL; grammar; rewriting                                   | Common Lisp         |
| 14.5  | ECA computation; connectivity dashboard                        | Julia + Python      |
| 14.6  | Sensor integration; provenance pipeline                        | Python + PostgreSQL |
| 14.7  | Full twin integration; synthetic landscape validation          | All                 |

---

## Relationship to Wider Programme

| Project | Dependency type                                              |
|---------|--------------------------------------------------------------|
| 3.0     | Knowledge Layer — provenance, semantic indexing              |
| 5.0–13.0| Hashimoto / spectral chain — graph foundations               |
| 6.0     | Prolog metamorphism — invariant layer                        |
| 7.0     | Lisp homoiconicity — rewriting layer                         |
| 9.0     | Tensor algebra — manifold representation                     |
| UOA     | Unified Operator Architecture — local‑to‑global operators    |

Downstream: Miyawaki corridor RL system.

---

## Standards

Implements the Project Template Framework (0.0).  
All artefacts registered in the Unified Asset Registry.  
Documentation follows Australian Government Style Manual + AGLC (see **10.0 – Documentation Standards**).

---

## Relation to *Reclaiming the Scar*

This project is directly aligned with my paper [*Reclaiming the Scar: Reinforcement Learning - Optimised Miyawaki Corridors for Defragmenting Logged Landscapes*](https://williammurrayrisk.substack.com/p/reclaiming-the-scar). That work develops interdisciplinary frameworks integrating restoration ecology, machine learning, and landscape topology to address road‑driven forest fragmentation.

Project **14.0** provides the formal substrate for those ideas:

- the **fiber‑bundle manifold** models the multi‑layer ecological dependencies explored in the paper  
- the **dynamic DSL** encodes successional dynamics, microclimate spillover, and patch‑graph evolution  
- the **connectivity metrics** (ECA, isoperimetric ratio) match the graph‑theoretic tools used in the RL corridor optimisation  
- the **sensor integration layer** supports the data‑driven restoration workflows the paper describes.  

