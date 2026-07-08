<div align="center">

# ⚡ Axiom

*GraphRAG reads the map. Axiom navigates it.*

[![Status](https://img.shields.io/badge/status-in%20development-7b2d8b)](https://github.com/Pralishatripathy000/Axiom)
[![Python](https://img.shields.io/badge/python-3.10+-blue)](https://python.org)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

</div>

---

## 🔮 What is Axiom?

GraphRAG introduced knowledge graphs to retrieval augmented generation. But it left two fundamental problems unsolved:

- **Binary edges** — relationships either exist or they don't. No notion of strength, confidence, or semantic weight.
- **Opaque reasoning** — it answers your question but cannot show you *how* it got there through the graph.

Axiom fixes both.

By introducing **dual-signal weighted edges** and **K-shortest path traversal**, Axiom transforms a static knowledge graph into a navigable reasoning system — one that doesn't just retrieve, but *explains*.

---


| Problem | Axiom's Solution |
|:--------|:----------------|
| Binary edges in GraphRAG | Dual-signal weighted edges — semantic similarity + relationship type strength |
| Unexplainable reasoning paths | K-shortest path traversal with plain language explanation of each reasoning chain |

---

## ⚡ Architecture
