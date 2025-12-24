# Quantum Learning & Applied Algorithms

This repository documents my structured and hands-on learning journey in
**quantum computing**, with a strong focus on **practical algorithms,
hybrid quantum–classical approaches, and real-world applicability** rather
than purely theoretical treatments.

The work here is organized to progressively build intuition and capability,
starting from quantum fundamentals and moving toward applied optimization
and cryptographic impact. Core quantum concepts such as superposition,
interference, entanglement, and measurement are explored through small,
well-documented experiments before being applied to full algorithms.

The repository includes implementations of foundational quantum algorithms,
including **Deutsch–Jozsa, Bernstein–Vazirani, Simon, Grover’s search
algorithm, and an educational-scale implementation of Shor’s algorithm**.
These examples highlight how quantum computers extract global structure
from problems and why certain tasks benefit from quantum execution while
many others do not.

A key emphasis of this work is on **hybrid quantum–classical methods** that
are practical in the current **Noisy Intermediate-Scale Quantum (NISQ) era**.
This includes optimization experiments using **QAOA**, where quantum circuits
are combined with classical optimizers to solve combinatorial problems such
as MAX-CUT. Classical baselines are included where appropriate to provide
clear comparisons and realistic expectations.

To bridge theory with practice, selected circuits are executed on **real IBM
Quantum hardware**, with results compared against ideal simulators. These
experiments highlight the impact of noise, decoherence, and gate errors,
and demonstrate why error mitigation and hybrid workflows are essential for
near-term quantum applications.

The primary goal of this repository is to develop and demonstrate
**applied quantum literacy**—the ability to understand when quantum
computing offers meaningful advantage, how it integrates with classical
systems, and what limitations exist today. It is intended as a practical
reference for learners, engineers, and technology leaders exploring quantum
computing from an implementation-first, use-case-driven perspective.

**Tools & Technologies**
- Python, Qiskit
- IBM Quantum (simulators and real devices)
- Jupyter Notebooks
- Hybrid quantum–classical optimization workflows
