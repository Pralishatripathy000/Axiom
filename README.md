<div align="center">

# ⚡ Axiom

*GraphRAG reads the map. Axiom navigates it.*

[![Status](https://img.shields.io/badge/status-in%20development-7b2d8b)](https://github.com/Pralishatripathy000/Axiom)
[![Python](https://img.shields.io/badge/python-3.10+-blue)](https://python.org/)
[![Research](https://img.shields.io/badge/research-GraphRAG%20%7C%20Graph%20Traversal-5b5bd6)](#)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

**A research experiment in turning knowledge graphs from structures we retrieve from into spaces we can meaningfully navigate.**

</div>

---

## 🔮 What is Axiom?

Axiom is a research framework exploring **semantic, explainable multi-path retrieval over knowledge graphs**, built on the GraphRAG paradigm.

GraphRAG demonstrated the value of representing unstructured knowledge as a graph. Axiom begins with the question that naturally followed:

> **Once knowledge is represented as a graph, why stop at retrieving from it? Why not navigate it?**

Axiom treats the knowledge graph as a **semantically typed, weighted and navigable retrieval space**.

The proposed system maps a query to relevant concepts, traverses meaningful relationships between them, retrieves one or more evidence paths, traces those paths back to the scientific literature, and provides the resulting evidence to an LLM for grounded generation.

```text
Query
  ↓
Relevant Concepts
  ↓
Ranked Evidence Paths
  ↓
Supporting Sources
  ↓
Grounded Answer + Evidence Paths + Citations
```

Axiom is currently under active development as a **Master's thesis research project**.

---

## 💭 How Axiom Happened

Axiom did not start as an attempt to build another RAG system.It started with two things I found interesting : **modern retrieval systems** and **classical algorithms**. RAG showed how external knowledge could augment an LLM. GraphRAG went further by organizing that knowledge structurally.

But once the knowledge became a graph, an older part of computer science suddenly became relevant. Graphs already have decades of algorithms for finding routes through them. So the first question was simple :

> **Could Dijkstra's algorithm be useful inside GraphRAG?**

Which immediately produced a much less simple question :

> **What does a "shortest" path mean when the graph contains knowledge instead of roads?**

A road has distance. Knowledge has **meaning**. That shifted the idea from ordinary shortest-path traversal toward **semantic graph traversal**. Relationships would need to express not only that two concepts are connected, but *how*:

```text
causes
requires
supports
extends
contradicts
```

Those relationships could then contribute to a numerical traversal cost.

And then came another problem.

Scientific questions rarely have only one valid explanation.

One shortest path was no longer enough.

That brought in **Yen's K-shortest paths**, alternative evidence routes, path diversity, contradictory evidence and source provenance.

So Axiom evolved roughly like this:

```text
RAG
 ↓
GraphRAG
 ↓
Why not traverse the graph?
 ↓
Dijkstra
 ↓
What does "shortest" mean for knowledge?
 ↓
Semantic + Relationship-Aware Edges
 ↓
Multiple Valid Explanations
 ↓
K-Shortest Paths
 ↓
Evidence Diversity + Provenance
 ↓
Axiom
```

At its core, Axiom asks what happens when **classical graph algorithms meet semantically enriched modern retrieval systems**.

---

## 🎯 Research Question

> **Can explicit semantic relationship typing combined with weighted multi-path graph traversal improve the relevance, interpretability and provenance of retrieval compared with conventional RAG and standard GraphRAG for machine-learning failure-mode diagnosis?**

Axiom is not intended to expose an LLM's internal reasoning.

Its focus is **retrieval-level explainability**: making the route from question to evidence inspectable.

---

## 🧩 The Idea

Modern GraphRAG research already includes weighted relationships, graph traversal, relational paths and multi-hop retrieval.

Axiom therefore does not simply ask whether graphs can be traversed.

It investigates a particular combination:

| Component | Role |
|---|---|
| **Semantically typed edges** | Represent relationships such as `causes`, `requires`, `supports`, `extends` and `contradicts` |
| **Multi-signal edge scoring** | Combines semantic similarity with relationship information |
| **Query-aware weighting** | Allows relationship importance to vary with query intent |
| **Dijkstra** | Retrieves the single preferred evidence path |
| **Yen's K-shortest paths** | Retrieves alternative loopless evidence paths |
| **Path diversity** | Reduces redundant alternatives |
| **Counter-paths** | Surfaces supporting and contradictory evidence where available |
| **Edge-level provenance** | Connects retrieved relationships back to source passages and papers |
| **Ablation experiments** | Tests whether each mechanism actually contributes |

The hypothesis is not that classical algorithms are better than modern AI.

It is that **classical graph algorithms may remain surprisingly useful when the graph they navigate has been semantically enriched**.

---

## ⚙️ Proposed Architecture

Axiom consists of an **offline knowledge-construction pipeline** and an **online retrieval pipeline**.

### Offline — Build the Knowledge Space

```text
Scientific Literature
        ↓
Document Preprocessing
        ↓
Entity & Relation Extraction
        ↓
Entity Resolution
        ↓
Semantic Edge Scoring
        ↓
Traversal-Cost Construction
        ↓
Typed, Cost-Weighted Knowledge Graph
```

Source provenance is retained while relationships are extracted.

### Online — Navigate It

```text
User Query
    ↓
Query Understanding & Entity Mapping
    ↓
Weighted Path Retrieval
   ↙                       ↘
Dijkstra              Yen's K-Shortest
Best Path             Alternative Paths
   ↘                       ↙
        Ranked Evidence Paths
                 ↓
        Source Passage Retrieval
                 ↓
       Path-Aligned Context
                 ↓
       Grounded LLM Generation
                 ↓
Answer + Evidence Paths + Citations
```

The pre-built knowledge graph supports both query-to-entity mapping and path retrieval.

Dijkstra and Yen are **alternative retrieval mechanisms**, not sequential stages.

---

## 📐 Semantic Edge Scoring

For an edge `e`, the current proposed formulation is:

```text
S(e) = αS_semantic(e) + (1 − α)S_relation(e)
```

where:

- `S_semantic(e)` represents semantic similarity,
- `S_relation(e)` represents relationship-type information,
- `α` controls their relative contribution.

Because shortest-path algorithms minimize cost:

```text
C(e) = 1 − S(e)
```

assuming normalized scores in `[0,1]`.

Therefore:

```text
Stronger Relationship
        ↓
Higher Semantic Score
        ↓
Lower Traversal Cost
        ↓
Preferred Evidence Route
```

The weighting is a **research hypothesis**, not a hard-coded claim of optimality. Different weighting strategies will be compared experimentally.

---

## 🛣️ Because One Path Is Rarely Enough

Dijkstra can find the preferred path.

But scientific literature may contain:

```text
Explanation A
Explanation B
Explanation C
```

and occasionally:

```text
Paper D: Actually, no.
```

Axiom therefore investigates **multiple evidence paths** rather than assuming a single route contains the entire explanation.

Yen's K-shortest-path algorithm provides alternative paths, while additional diversity logic is intended to prevent Top-K retrieval from becoming:

```text
Path 1: A → B → C → D
Path 2: A → B → C → E → D
Path 3: A → B → C → F → D
```

and pretending those are three dramatically different discoveries.

Where supported by the graph, contradictory relationships can also form **counter-paths**, allowing disagreement in the literature to remain visible rather than being flattened into one answer.

---

## 🔗 Evidence, Not Just Edges

Every important relationship should owe us a source.

Axiom therefore retains provenance at the edge level:

```text
Concept
   ↓
Typed Relationship
   ↓
Concept
   ↓
Supporting Passage
   ↓
Scientific Paper
```

This allows a retrieved path to function as an **evidence chain**, not merely a sequence of graph nodes.

---

## 🧪 Initial Research Domain

The initial experimental domain is **machine-learning failure-mode diagnosis**.

Example reasoning chains may connect:

```text
Observed Failure
      ↓
Underlying Mechanism
      ↓
Theoretical Explanation
      ↓
Supporting / Contradictory Evidence
      ↓
Potential Mitigation
```

Candidate topics include:

- vanishing gradients
- overfitting
- GAN mode collapse
- catastrophic forgetting
- optimization instability
- generalization failures
- architecture-specific failure modes

The initial corpus is expected to contain approximately **150–200 curated scientific papers**.

---

## 🔬 Proposed Evaluation

Axiom will be evaluated beyond final-answer quality.

**Retrieval**

`Precision@K` · `Recall@K` · `MRR` · `NDCG`

**Path Quality**

Validity · Semantic coherence · Diversity · Redundancy · Coverage

**Provenance**

Citation accuracy · Source alignment · Edge-to-source traceability

**Generation**

Correctness · Groundedness · Hallucination rate · Interpretability

Experiments will also compare variants such as:

```text
Unweighted
vs
Semantic-only
vs
Relationship-only
vs
Dual-signal weighting
```

and:

```text
Dijkstra
vs
Yen Top-K
vs
Diverse Top-K
```

Because if removing a component changes nothing, it probably did not deserve its architecture box.

---

## 🛠️ Proposed Technical Stack

| Area | Technology |
|---|---|
| Language | Python |
| Development | VS Code · Jupyter |
| Literature | arXiv · OpenAlex / Semantic Scholar |
| PDF Processing | PyMuPDF |
| Data | pandas · NumPy |
| NLP | spaCy |
| Embeddings | Sentence-Transformers |
| Similarity / Entity Resolution | scikit-learn |
| Knowledge Graph | **NetworkX** |
| Graph Traversal | Dijkstra · Yen's K-Shortest Paths |
| Path Logic | Custom Python |
| Provenance | JSON / Parquet / SQLite |
| LLM | Open-weight or API-hosted model |
| Evaluation | scikit-learn · SciPy |
| Visualization | PyVis · NetworkX · Matplotlib |
| Prototype | Streamlit |
| Compute | Local · Google Colab · Kaggle |
| Optional Cloud | AWS / Azure |

A high-end local GPU is **not required**. Most graph construction, traversal, scoring and evaluation can run on CPU, with Colab/Kaggle available for heavier embedding or LLM workloads.

---

## 📁 Proposed Repository Structure

```text
Axiom/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── evaluation/
│
├── corpus/
├── extraction/
├── entity_resolution/
│
├── graph/
│   ├── construction/
│   ├── schemas/
│   └── provenance/
│
├── scoring/
│   ├── semantic/
│   ├── relation/
│   └── query_conditioning/
│
├── traversal/
│   ├── dijkstra/
│   ├── yen_k_shortest/
│   ├── diversity/
│   └── counter_paths/
│
├── retrieval/
├── generation/
├── baselines/
├── evaluation/
├── experiments/
├── visualization/
├── app/
├── notebooks/
├── tests/
│
├── requirements.txt
├── LICENSE
└── README.md
```

The structure will evolve with the research.

A thesis repository should follow the experiments — not force the experiments to follow a collection of beautifully empty folders.

---

## 🗺️ Current Roadmap

- [x] Research direction
- [x] Initial literature review
- [x] Axiom concept formulation
- [x] Proposed architecture
- [x] Semantic edge formulation
- [x] Dijkstra + Yen retrieval design
- [x] Provenance design
- [x] Initial evaluation strategy
- [x] Expanded related-work search
- [ ] Deep comparison with closest recent research
- [ ] Finalize novelty statement
- [ ] Build scientific corpus
- [ ] Entity & relation extraction
- [ ] Entity resolution
- [ ] Knowledge graph construction
- [ ] Query-to-graph mapping
- [ ] Weighted path retrieval
- [ ] Diverse / counter-path retrieval
- [ ] Baseline implementation
- [ ] Evaluation & ablation studies
- [ ] Prototype
- [ ] Publication experiments
- [ ] Thesis

---

## ⚠️ Still Unsolved — Intentionally

Axiom is research, so several boxes are supposed to remain open.

One of the biggest is **query-to-endpoint selection**.

Dijkstra and Yen expect graph endpoints.

Natural-language questions, rather inconveniently, do not arrive as:

```text
source_node = X
target_node = Y
```

Determining those candidates robustly is therefore part of the methodology still being developed.

Relationship weighting, semantic path diversity and counter-path selection likewise remain experimental questions rather than assumptions disguised as results.

---

## 📚 Research Positioning

Axiom builds on research across:

**RAG → GraphRAG → KG-guided retrieval → graph traversal → path-based retrieval → multi-path retrieval → provenance-aware generation**

Microsoft GraphRAG is an important reference architecture and baseline.

Axiom is not intended to be:

```text
GraphRAG + Azure
```

Its research contribution is intended to remain independent of the deployment platform.

The underlying question is more interesting:

> **What changes when a knowledge graph stops being merely a retrieval structure and becomes a semantically meaningful space that can be navigated, compared and inspected?**

---

## 🚧 Status


Expect changing assumptions, ablations, graphs that occasionally refuse to cooperate, and — if all goes well — a few results worth defending.

---

The axioms are being laid. The paths are being carved. Come back when the graph speaks. ⚡
