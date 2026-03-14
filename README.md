# Collective Geometric Inscription
## A Marked Spatial Point Process Framework for Emergent Artifact Formation

> Uncoordinated participants, acting under a single minimal instruction,  
> generate collective spatial structure through sequential geometric decisions.  
> This framework infers the implicit rule governing those decisions.

---

## Overview

The experiment begins with one instruction:

> **Choose one shape and place it on the shared surface.**

Each participant arrives, observes a surface accumulating geometric marks, adds one shape, and leaves. No participant communicates with another. There is no shared goal. There is no coordination mechanism — except the artifact itself.

Over time, the surface fills. Structure appears.

The question is: **what rule produced it?**

This framework models the evolving artifact as a **sequential marked spatial point process** and treats participant decisions as draws from a **field-conditioned probability measure**. The scientific objective is to recover the **conditional intensity function** that governs how participants select shape type and placement location given the configuration they observe.

---

## Formal Setup

### Spatial Domain

Let

$$\mathcal{S} = [0, W] \times [0, H] \subset \mathbb{R}^2$$

denote the bounded planar domain — the physical surface on which participants act.

### Mark Space

Let

$$\mathcal{Q} = \{\,\circ,\; \square,\; \triangle\,\}$$

denote the finite categorical mark space — the set of available shape types.

### Configuration Space

A **marked configuration** is a finite set of located, typed events:

$$X = \{(q_i, \ell_i)\}_{i=1}^{n} \;\in\; \mathcal{N}_f(\mathcal{S} \times \mathcal{Q})$$

where $\mathcal{N}_f(\mathcal{S} \times \mathcal{Q})$ is the space of finite marked point configurations over $\mathcal{S} \times \mathcal{Q}$.

### The Construction Process

The artifact evolves as a **strictly additive sequential process**:

$$X_0 = \emptyset \qquad X_{t+1} = X_t \cup \{(q_{t+1},\, \ell_{t+1})\}$$

Each participant contributes exactly one event. The configuration at time $n$ encodes the complete history of $n$ sequential decisions. No element is ever removed or relocated. The process is irreversible.

---

## Markov Structure

Participants observe only the current visible field. They have no knowledge of the identity, sequence, or reasoning of prior participants — only the geometric configuration left on the surface.

This motivates the **first-order Markov assumption**:

$$P(X_{t+1} \mid X_0, X_1, \dots, X_t) \;=\; P(X_{t+1} \mid X_t)$$

**Justification.** The field $X_t$ is a complete spatial record of all prior actions. A participant reading the surface recovers — through perceived geometry — the aggregate signal of every preceding decision: density patterns, clustering, symmetry, empty regions, dominant shape types. The current configuration is therefore the natural sufficient statistic for predicting the next action.

Under this assumption, the construction process defines a **discrete-time Markov chain** on $\mathcal{N}_f(\mathcal{S} \times \mathcal{Q})$, with transition determined by participant behavior.

---

## The Conditional Intensity Function

Given field state $X_t$, a participant selects action $(q, \ell)$ according to a normalized density over $\mathcal{Q} \times \mathcal{S}$:

$$P(q, \ell \mid X_t)\, d\ell \;=\; \frac{\lambda(q, \ell \mid X_t)}{\mathcal{Z}(X_t)}\, d\ell$$

where the **partition function** normalizes over all possible actions:

$$\mathcal{Z}(X_t) \;=\; \sum_{q' \in \mathcal{Q}} \int_{\mathcal{S}} \lambda(q', \ell' \mid X_t)\, d\ell'$$

The function $\lambda : \mathcal{Q} \times \mathcal{S} \times \mathcal{N}_f \rightarrow \mathbb{R}_{+}$ is the **conditional intensity** — the fundamental object of inference.

Recovering $\lambda$ from observed trajectories is equivalent to identifying the behavioral rule by which participants read and respond to the field.

---

## Energy-Based Representation

The conditional intensity admits a **Gibbs energy representation**:

$$\lambda(q, \ell \mid X_t) \;=\; \exp\!\bigl(-\,\mathcal{H}(q, \ell;\, X_t)\bigr)$$

where $\mathcal{H}$ is a **local interaction energy** measuring the compatibility of placing shape $q$ at location $\ell$ given the current configuration $X_t$.

This formulation follows directly from the theory of Gibbs–Markov spatial processes established by Besag (1974): local conditional probability specifications that satisfy positivity and Markov conditions uniquely determine a globally consistent probability distribution over spatial configurations. The energy function encodes all interaction structure.

High $\mathcal{H}$ → low probability of that action.  
Low $\mathcal{H}$ → high probability of that action.

The energy formulation separates **what is measured** (spatial features of the configuration) from **how those measurements combine** (the parameter weights).

---

## Energy Decomposition

The interaction energy decomposes into a weighted sum of spatial feature functions:

$$\mathcal{H}(q, \ell;\, X_t) \;=\; \sum_{k=1}^{K} \beta_k\, f_k(q, \ell;\, X_t)$$

Parameters $\boldsymbol{\beta} = (\beta_1, \dots, \beta_K)$ are estimated from observed action sequences. Each $\beta_k$ quantifies the strength and direction of the corresponding behavioral tendency.

### Feature Functions

**Density interaction** — attraction or inhibition toward occupied regions:

$$f_{\text{density}}(q, \ell;\, X_t) \;=\; -\log\!\left(1 + \sum_{(q_j, \ell_j) \in X_t} \kappa_h(\|\ell - \ell_j\|)\right)$$

where $\kappa_h$ is an isotropic kernel with bandwidth $h$. Negative $\beta_{\text{density}}$ encodes clustering; positive encodes inhibition.

**Mark interaction** — categorical homophily or heterophily:

$$f_{\text{match}}(q, \ell;\, X_t) \;=\; \sum_{(q_j, \ell_j) \in X_t} \mathbf{1}[q = q_j]\cdot \kappa_h(\|\ell - \ell_j\|)$$

Negative $\beta_{\text{match}}$ draws same-type shapes together; positive disperses them.

**Symmetry gain** — preference for configurations with greater bilateral balance:

$$f_{\text{symmetry}}(q, \ell;\, X_t) \;=\; \Sigma\!\bigl(X_t \cup \{(q,\ell)\}\bigr) - \Sigma(X_t)$$

where $\Sigma(\cdot)$ is a bilateral symmetry functional over $\mathcal{S}$. Negative $\beta_{\text{symmetry}}$ encodes a tendency to complete perceived symmetry axes.

**Void attraction** — preference for spatially unoccupied regions:

$$f_{\text{void}}(\ell;\, X_t) \;=\; V(\ell;\, X_t)$$

where $V(\ell; X_t)$ is the area of the Voronoi cell containing $\ell$ under the configuration $X_t$. Negative $\beta_{\text{void}}$ encodes space-filling behavior.

**Boundary interaction** — avoidance or anchoring to the domain edge:

$$f_{\text{edge}}(\ell) \;=\; d(\ell,\, \partial\mathcal{S})$$

where $d(\ell, \partial\mathcal{S})$ is Euclidean distance from the boundary. Positive $\beta_{\text{edge}}$ encodes interior preference; negative encodes boundary anchoring.

**Scale coherence** — local size matching:

$$f_{\text{scale}}(q, \ell;\, X_t) \;=\; \sum_{(q_j, \ell_j) \in X_t} \bigl|r - r_j\bigr| \cdot \kappa_h(\|\ell - \ell_j\|)$$

where $r$ and $r_j$ are the characteristic scales of placed shapes.

---

## Mark-Induced Spatial Anisotropy

A structural distinction between this framework and standard marked point processes is that the marks carry **intrinsic geometry**.

Each placed shape is fully described by:

$$s = (q,\, \ell,\, r,\, \phi,\, \epsilon)$$

| Parameter | Meaning |
|---|---|
| $q \in \mathcal{Q}$ | categorical mark |
| $\ell \in \mathcal{S}$ | centroid location |
| $r \in \mathbb{R}_+$ | characteristic scale |
| $\phi \in [0, 2\pi)$ | orientation angle |
| $\epsilon$ | execution deviation from ideal form |

Marks are not dimensionless labels. They occupy space and possess symmetry structure:

- **Circles** $(\circ)$: continuous rotational symmetry, no preferred axis, isotropic overlap geometry
- **Squares** $(\square)$: fourfold symmetry, strong alignment tendency along axis $\phi$
- **Triangles** $(\triangle)$: single apex, directional asymmetry, bilateral symmetry about apex axis

This **mark-induced anisotropy** enters the interaction energy: the spatial relationship between two squares is not equivalent to the spatial relationship between two circles at the same distance, because the marks themselves structure the geometry of interaction. Feature functions that measure alignment, containment, or overlap must respect this.

---

## Stigmergic Coordination

This system exhibits a specific coordination structure: participants **never interact directly**. No participant observes another participant, communicates intent, or responds to agent behavior. Coordination is mediated entirely through the shared surface:

$$\text{participant}_t \;\longrightarrow\; \text{reads } X_t \;\longrightarrow\; \text{produces action} \;\longrightarrow\; X_{t+1}$$

This is **stigmergy** — indirect coordination through environmental modification — applied to human geometric production. The artifact is simultaneously:

1. the **record** of all prior decisions
2. the **stimulus** that structures the next decision
3. the **medium** through which strangers implicitly coordinate

Global spatial order emerges without communication, shared goals, or central authority. The artifact is not merely the output of the process — it is the mechanism of the process.

This distinguishes the framework from models in which agents respond to each other directly (Schelling segregation), in which structure is transmitted through imitation (iterated learning), or in which patterns emerge from physical rather than behavioral deposition processes.

---

## Parameter Estimation

Given an observed action sequence $\{(q_t, \ell_t)\}_{t=1}^{n}$ with associated field states $\{X_{t-1}\}_{t=1}^{n}$, parameters are estimated by **maximum pseudolikelihood**:

$$\hat{\boldsymbol{\beta}} \;=\; \underset{\boldsymbol{\beta}}{\arg\max}\; \sum_{t=1}^{n} \left[\log \lambda(q_t, \ell_t \mid X_{t-1};\, \boldsymbol{\beta}) \;-\; \log \mathcal{Z}(X_{t-1};\, \boldsymbol{\beta})\right]$$

The partition function $\mathcal{Z}(X_{t-1}; \boldsymbol{\beta})$ is approximated by numerical integration over $\mathcal{Q} \times \mathcal{S}$ using a discretization grid on $\mathcal{S}$.

Pseudolikelihood methods for spatial processes are established in Besag (1977) and fully developed in Baddeley, Rubak, and Turner (2015). Under mild regularity conditions, $\hat{\boldsymbol{\beta}}$ is consistent and asymptotically normal.

### Model Selection

Competing energy specifications are ranked by:

$$\text{AIC} = 2K - 2\hat{\ell} \qquad \text{BIC} = K \log n - 2\hat{\ell} \qquad \text{WAIC}$$

where $K$ is the number of free parameters and $\hat{\ell}$ is the maximized pseudolikelihood.

---

## Candidate Generative Mechanisms

Specific hypotheses about participant behavior correspond to specific energy specifications:

| Mechanism | Active Features | Sign | Predicted Field Structure |
|---|---|---|---|
| Complete Spatial Randomness | none | — | uniform, structureless |
| Clustering | $f_{\text{density}}$ | $\beta < 0$ | aggregated groups |
| Inhibition | $f_{\text{density}}$ | $\beta > 0$ | regular spacing |
| Shape segregation | $f_{\text{match}}$ | $\beta < 0$ | same-type clusters |
| Shape interleaving | $f_{\text{match}}$ | $\beta > 0$ | alternating types |
| Symmetry completion | $f_{\text{symmetry}}$ | $\beta < 0$ | bilateral balance |
| Space-filling | $f_{\text{void}}$ | $\beta < 0$ | even coverage |
| Edge anchoring | $f_{\text{edge}}$ | $\beta < 0$ | boundary concentration |
| Mixed | multiple | varied | composite spatial pattern |

The fitted model identifies which mechanism — or combination — governs collective behavior.

---

## Simulation and Field Comparison

Fitted parameters generate synthetic trajectories by sequential sampling from the estimated intensity:

$$X_0 = \emptyset, \qquad (q_{t+1}, \ell_{t+1}) \;\sim\; \frac{\exp\!\bigl(-\mathcal{H}(q,\ell;\, X_t;\, \hat{\boldsymbol{\beta}})\bigr)}{\mathcal{Z}(X_t;\, \hat{\boldsymbol{\beta}})}$$

Synthetic fields are compared to observed artifacts using spatial point process summary statistics:

| Statistic | Definition | Measures |
|---|---|---|
| $K(r)$ | $\lambda^{-1}\,\mathbb{E}[\text{events within distance } r \text{ of a typical event}]$ | overall clustering/inhibition |
| $g(r) = K'(r) / 2\pi r$ | pair correlation function | pairwise distance structure |
| $G(r)$ | nearest-neighbor distance distribution | local proximity |
| $M_{qq'}(r)$ | mark connection function | same-type co-occurrence by distance |
| $\Sigma(X)$ | symmetry index | bilateral balance |
| $\text{CV}(V)$ | coefficient of variation of Voronoi cell areas | spatial regularity |

Discrepancy between observed and simulated statistics, averaged over replicated trajectories, constitutes the primary validation criterion. The model that minimizes this discrepancy across all statistics identifies the most plausible generative mechanism.

---

## Experimental Design

### Progression of Inquiry

The experimental grammar isolates behavioral components through a structured sequence of tasks:

| Level | Task | Variable Isolated |
|---|---|---|
| 1 | Place a single shape | single-mark emission distribution |
| 2 | Place two shapes of the same type | same-class spatial relation |
| 3 | Place two shapes of different types | cross-class integration strategy |
| 4 | Select from a shape bank and place once | categorical choice without motor production |
| 5 | Sequential shared field — each participant adds one shape | collective transition kernel |

Each level adds one latent variable to the inference problem. Progression from Level 1 to Level 5 builds toward the full estimation of $\lambda(q, \ell \mid X_t)$.

### Minimum Viable Experiment

**Materials:**
- Shape bank: circle template, square template, triangle template
- One shared surface, initially blank
- Coordinate recording system (grid overlay, photograph, or tracking)

**Instruction delivered to each participant:**
> Choose one shape and place it anywhere on the surface.

**Record per action $t$:**

```
t       : order index
q_t     : shape selected ∈ {circle, square, triangle}
ℓ_t     : (x, y) placement coordinate
X_{t-1} : field state before action (photograph or coordinate snapshot)
```

**Proceed to estimation:**

$$\hat{\boldsymbol{\beta}} = \underset{\boldsymbol{\beta}}{\arg\max}\; \sum_t \log P(q_t, \ell_t \mid X_{t-1};\, \boldsymbol{\beta})$$

**Validate by simulation and field comparison.**

### Identifiability Conditions

Reliable kernel estimation requires:

- **Sample size:** $n \geq 30$ actions per experimental instance
- **Spatial coverage:** actions distributed across $\mathcal{S}$, not concentrated in one region
- **Replications:** multiple independent instances for out-of-sample validation
- **Field variation:** the field must evolve across observation indices for the Markov structure to be estimable

---

## Relationship to Prior Frameworks

| Framework | Connection | Structural Distinction |
|---|---|---|
| **Schelling (1971, 1978)** | minimal local rules generate global spatial structure | Schelling models agent *relocation*; this framework models *artifact accretion* — agents do not move, they append |
| **Besag (1974)** | Gibbs–Markov local conditional specification | Besag addresses abstract lattice systems; this framework addresses human behavioral decisions conditioned on a visible geometric field |
| **Ripley (1988); Møller & Waagepetersen (2004); Baddeley, Rubak & Turner (2015)** | spatial point process inference machinery | Classical SPP addresses natural event patterns; this framework introduces a behavioral generative process where each event is a deliberate human choice |
| **Kirby (2008)** | sequential human update systems producing emergent structure | Kirby studies symbolic transmission through imitation chains; this framework studies spatial geometric accumulation through sequential placement |
| **Grassé (1959), stigmergy** | indirect environmental coordination | Stigmergy addresses physical substance deposition by social insects; this framework addresses human aesthetic–geometric decision-making mediated by a shared surface |

The sharpest description of the system's position is:

> **Prior work gives either** emergence from local rules **(Schelling), spatial statistical inference (Ripley, Møller–Waagepetersen, Baddeley), or sequential human behavioral experiments (Kirby). This framework unifies all three around a new empirical object: the sequential construction of a collective geometric artifact by uncoordinated strangers under minimal instructions, modeled as a Gibbs-distributed marked spatial point process with a Markov update rule.**

---

## Canonical Foundations

| Domain | Key Works | Role in Framework |
|---|---|---|
| Markov processes | Markov (1906); Feller (1968); Norris (1998) | temporal structure of sequential updates |
| Spatial point processes | Cox & Isham (1980); Ripley (1988); Møller & Waagepetersen (2004); Baddeley, Rubak & Turner (2015) | marked spatial configuration modeling and inference |
| Gibbs–Markov spatial interaction | Besag (1974, 1977); Cressie (1993) | local interaction energy specification |
| Emergent spatial structure | Schelling (1971, 1978) | macro-from-micro conceptual template |
| Sequential human accumulation | Kirby (2008); Boyd & Richerson (1985) | sequential social update dynamics |
| Simulation-based inference | Metropolis et al. (1953); Robert & Casella (2004); Neal (2011) | trajectory simulation and parameter recovery |
| Decentralized emergence | Epstein (2006); Page (2011) | complex systems framing |

---

## Theoretical Summary

**Collective Geometric Inscription** is formally described as:

$$\boxed{X_t = \{(q_i, \ell_i)\}_{i=1}^{t} \qquad (q_{t+1},\, \ell_{t+1}) \;\sim\; \frac{\exp\!\bigl(-\sum_k \beta_k f_k(q, \ell;\, X_t)\bigr)}{\mathcal{Z}(X_t)}}$$

The object of inference is the energy decomposition $\{\beta_k, f_k\}$ that best accounts for the spatial structure of observed collective artifacts.

Understanding this decomposition reveals the **implicit decision rule by which strangers — acting independently, without communication, under a single minimal instruction — collectively produce complex spatial structure through the accumulation of geometric marks on a shared surface.**

---

## Conceptual Hierarchy

```
minimal instruction
    ↓
participant observes field X_t
    ↓
action (q_{t+1}, ℓ_{t+1}) drawn from λ(· | X_t)
    ↓
field updates: X_{t+1} = X_t ∪ {(q_{t+1}, ℓ_{t+1})}
    ↓
next participant observes X_{t+1}
    ↓
collective artifact X_n = emergent spatial structure
    ↓
inference: recover λ from {X_t} trajectory
    ↓
simulation: generate synthetic fields from λ̂
    ↓
validation: compare observed and simulated spatial statistics
    ↓
identification: which mechanism produced the structure?
```
