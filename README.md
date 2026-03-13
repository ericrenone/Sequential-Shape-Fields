# Sequential Shape Fields
## A Markov Field Framework for Collective Geometric Interaction

> Minimal instructions can generate complex collective structure.  
> The goal of this framework is to infer the **local update rule by which strangers collectively generate spatial structure on a shared surface.**

This repository formalizes a progression from simple geometric prompts (e.g., **“draw a circle”**) to a shared-field experiment in which many strangers sequentially add shapes to the same surface.

The prompts themselves are not the primary object of study.  
Each prompt instead defines a **state space of possible responses**.

As the experimental design progresses, the system evolves from **independent geometric emissions** to a **collective stochastic field** whose dynamics can be modeled as a **spatial Markov process with a transition kernel**.

The final scientific objective is to estimate the **local decision rule** governing participant actions.

---

# Conceptual Overview

A participant receives an instruction, performs a decision process, and produces a geometric action.

That action updates the system state.

\[
X_t \rightarrow X_{t+1}
\]

Where:

- \(X_t\) is the system state before the participant acts  
- \(X_{t+1}\) is the system state after the participant acts  

For isolated drawing prompts, each response is an independent emission.

For the shared-surface experiment, the system evolves sequentially:

\[
X_0 \rightarrow X_1 \rightarrow X_2 \rightarrow \dots \rightarrow X_n
\]

Each participant observes the existing field and updates it.

This produces a **sequential stochastic process over spatial structure**.

---

# Experimental Grammar

The experimental progression introduces new latent variables in stages.

Each stage expands the state space of possible responses.

## Level 1 — Single-form generation

Participants generate one geometric object.

Examples:

- draw a line  
- draw a circle  
- draw a square  
- draw a triangle  

Purpose:

Estimate **single-form emission distributions**.

---

## Level 2 — Same-form relation

Participants generate two objects of the same class.

Examples:

- draw two circles  
- draw two squares  
- draw two triangles  

Purpose:

Estimate **relational geometry distributions**.

---

## Level 3 — Cross-form relation

Participants integrate objects of different classes.

Examples:

- draw a circle and a square  
- draw a circle and a triangle  
- draw a square and a triangle  

Purpose:

Measure **heterogeneous object integration strategies**.

---

## Level 4 — Choice without production

Participants no longer draw shapes.

Instead they choose from a predefined **shape bank**.

Example instruction:

> Choose one shape and add it to the paper.

Purpose:

Isolate

- categorical selection
- spatial placement

Motor-production noise is removed.

---

## Level 5 — Collective sequential field

Participants interact with the **same evolving sheet**.

Example instruction:

> Each participant adds exactly one shape to the shared paper.

Purpose:

Observe **collective field dynamics**.

---

# Stage 1 — Single Circle Baseline

The first baseline task establishes the distribution of circle production.

## Response representation

\[
s = (x, y, r, c, p, v, \theta, \epsilon)
\]

Where

- \(x,y\) — center position  
- \(r\) — radius  
- \(c\) — closure completeness  
- \(p\) — pen pressure  
- \(v\) — drawing velocity  
- \(\theta\) — starting angle  
- \(\epsilon\) — geometric deviation from an ideal circle  

## Emission model

\[
s \sim P_{\text{circle}}(\cdot)
\]

This stage estimates baseline tendencies such as:

- center bias
- radius distribution
- closure behavior
- drawing variance

These parameters define the **baseline emission distribution**.

---

# Stage 2 — Two-Circle Relation

The task now introduces **relational geometry**.

## Response representation

\[
s = (s_1, s_2, R)
\]

Where:

- \(s_1\), \(s_2\) describe each circle  
- \(R\) describes the geometric relationship.

### Relational parameters

\[
R = (d, \Delta r, o, t, n, a, \sigma)
\]

Where

- \(d\) — center distance  
- \(\Delta r\) — size difference  
- \(o\) — overlap  
- \(t\) — tangency  
- \(n\) — containment  
- \(a\) — alignment  
- \(\sigma\) — symmetry score  

The participant implicitly performs a short decision sequence:

\[
z_0 \rightarrow z_1 \rightarrow z_2
\]

Where

- \(z_0\): interpret prompt  
- \(z_1\): generate first circle  
- \(z_2\): position second circle relative to the first  

This stage introduces **conditional geometry**.

---

# Stage 3 — Square Baseline

Squares introduce a polygonal production process.

Unlike circles, squares require:

- vertex planning
- orthogonal edges
- angular closure
- rotation relative to page axes

## Emission model

\[
s \sim P_{\text{square}}(\cdot)
\]

Typical parameters include:

- center position
- side length
- orientation
- orthogonality error
- corner sharpness

This stage distinguishes **continuous curve production** from **polygonal construction**.

---

# Stage 4 — Triangle Baseline

Triangles represent the minimal polygon class.

## Emission model

\[
s \sim P_{\text{triangle}}(\cdot)
\]

Typical parameters:

- centroid position
- scale
- orientation
- side symmetry
- apex direction
- base selection

Triangles reveal tendencies such as:

- stability bias
- directional bias
- symbolic completion behavior

---

# Same-Shape Relational Families

Estimate relational distributions separately for each class:

\[
P(R \mid \text{circle-circle})
\]

\[
P(R \mid \text{square-square})
\]

\[
P(R \mid \text{triangle-triangle})
\]

Central research question:

Do participants organize identical objects differently depending on geometric class?

---

# Mixed-Shape Relations

Tasks:

- square + circle
- circle + triangle
- square + triangle

## Response representation

\[
s = (s_A, s_B, R, O)
\]

Where

- \(R\) is relational geometry
- \(O\) is order and role assignment

Variables include:

- which shape appears first
- relative scale
- containment
- anchor vs satellite roles

These tasks measure **heterogeneous integration strategies**.

---

# Shape Bank Simplification

To isolate categorical choice, introduce a shape bank:

- circle
- square
- triangle

Instruction:

> Choose one shape and add it to the paper.

Each action becomes:

\[
a_t = (q_t, \ell_t)
\]

Where:

- \(q_t\) — shape category
- \(\ell_t\) — placement location

This removes variation due to drawing execution.

---

# Shared Field Representation

Let the field state at step \(t\) be

\[
X_t = \{(q_1,\ell_1), (q_2,\ell_2), \dots, (q_t,\ell_t)\}
\]

Each participant observes the current field and adds one shape:

\[
X_{t+1} = X_t \cup \{(q_{t+1},\ell_{t+1})\}
\]

This produces a **sequential spatial process**.

---

# Markov Field Assumption

The update process is modeled as:

\[
P(X_{t+1} \mid X_t, X_{t-1}, \dots) \approx P(X_{t+1} \mid X_t)
\]

Participant decisions depend on the **visible current field**, which encodes prior history.

This yields a **Markov field representation**.

---

# Transition Kernel

The central object of study is the transition kernel:

\[
(q_{t+1},\ell_{t+1}) \sim K(\cdot \mid X_t)
\]

This kernel describes how the next participant selects:

- a shape category
- a spatial placement

given the current field configuration.

---

# Conditional Intensity Model

A flexible parametric form for the kernel is:

\[
P(q,\ell \mid X_t)
\propto
\exp\left(
\beta_1 f_{\text{density}}(\ell, X_t)
+
\beta_2 f_{\text{symmetry}}(\ell, X_t)
+
\beta_3 f_{\text{match}}(q, X_t)
+
\beta_4 f_{\text{novelty}}(q, X_t)
+
\beta_5 f_{\text{edge}}(\ell)
\right)
\]

Example features:

| Feature | Meaning |
|---|---|
density | attraction or avoidance of existing shapes |
symmetry | tendency to balance visible asymmetries |
match | category alignment with neighbors |
novelty | contrast against existing categories |
edge | boundary preferences |

Parameters \(\beta_i\) control behavioral tendencies.

---

# Parameter Estimation

Parameters are estimated using **maximum pseudolikelihood** for spatial point processes.

Procedure:

1. record all actions \(a_t\)
2. compute local field statistics
3. estimate coefficients \(\beta_i\)
4. compare candidate models using

- AIC
- BIC
- WAIC

Model comparison identifies the transition rule that best explains the observed field evolution.

---

# Monte Carlo Simulation

Monte Carlo simulation generates synthetic fields from candidate kernels.

Simulated trajectories:

\[
X_0 \rightarrow X_1 \rightarrow \dots \rightarrow X_n
\]

allow comparison between:

- observed fields
- null models
- alternative behavioral rules.

Example models:

| Model | Description |
|---|---|
Random | independent uniform placement |
Cluster | attraction to existing shapes |
Homophily | shape-category matching |
Symmetry | balance completion |

Simulations reveal which mechanisms reproduce the observed structure.

---

# Experimental Pipeline

## Data collection

For each participant record:

- shape category
- coordinates
- order index

## Model fitting

Estimate parameters of candidate kernels.

## Simulation

Generate synthetic trajectories.

## Model comparison

Identify which transition rule best explains the empirical data.

---

# Conceptual Summary

The experimental progression follows this ladder:

\[
\text{single form}
\rightarrow
\text{relation}
\rightarrow
\text{mixed relation}
\rightarrow
\text{choice}
\rightarrow
\text{placement}
\rightarrow
\text{collective sequential field}
\rightarrow
\text{transition kernel}
\rightarrow
\text{Monte Carlo inference}
\]

The prompts initiate the system.

The field dynamics reveal the **local rules governing collective spatial structure**.

---

# Minimum Viable Experiment

1. Prepare a shape bank:

- circle
- square
- triangle

2. Instruction:

> Choose one shape and add it anywhere on the paper.

3. Each passerby adds exactly one shape.

4. Record:

- shape type
- coordinates
- order index

5. Fit the transition kernel

\[
X_{t+1} \sim K(\cdot \mid X_t)
\]

6. Run Monte Carlo simulations to evaluate candidate mechanisms.

---

# Core Research Goal

Estimate the transition rule:

\[
K((q,\ell)\mid X_t)
\]

This kernel describes how strangers collectively generate spatial structure under minimal instructions.

Understanding this rule reveals how simple local decisions produce complex collective artifacts.
