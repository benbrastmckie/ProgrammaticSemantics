# Programmatic Semantics

A programmatic methodology using the [model-checker](https://pypi.org/project/model-checker/) software to rapidly prototype semantic theories using SMT solvers.

## Overview

This project presents a computational framework that treats semantic theories as executable programs, enabling systematic model exploration, empirical theory comparison, and automated validation checking through SMT constraint solving. The ModelChecker framework leverages the Z3 SMT solver to rule out finite countermodels of a user-specified size, providing evidence for logical consequences.

**Target Journal**: Journal of Automated Reasoning

**Authors**: 
- Benjamin Brast-McKie (brastmck@mit.edu)
- Miguel Buitrago (mfbuit46@mit.edu)

## Repository Structure

```
ProgrammaticSemantics/
├── paper/              # Main paper draft
├── outline/            # Paper outline and planning documents
├── research/           # Technical research background
├── talks/              # Conference and workshop presentations
│   ├── truthmaker_advances/  # Advances in Truthmaker Semantics II
│   ├── topos_talk/           # Topos Institute presentation
│   └── mit_wip/              # MIT WIP talk
└── handout/            # Presentation handout
```

## Main Documents

### Paper

- **[Main Paper](paper/sn-article.tex)** - Draft paper on programmatic semantics using the Springer Nature template

### Planning & Outline

- **[Paper Outline](outline/paper_outline.md)** - Comprehensive 7-section outline for the Journal of Automated Reasoning paper, including:
  - Introduction: challenges in semantic theory development
  - Complete pipeline architecture
  - Modular operator classes
  - Systematic comparative methodology
  - Model exploration and bounded search
  - Computational complexity and primitive arity
  - TheoryLib and collaborative development

- **[Paper Overview](outline/paper_overview.md)** - Compilation of topics extracted from the project repository

- **[TODO List](outline/TODO.md)** - Current project tasks and checklist

### Research

- **[SMT Solver Mechanisms](research/smt_solver_mechanisms.md)** - Technical background on:
  - Boolean Constraint Propagation (BCP)
  - Theory modularity and independent reasoning
  - Coupling and propagation cascades
  - DPLL(T) architecture
  - Clause learning and memory pressure
  - Quantifier expansion vs. instantiation

### Conference Presentations

#### Advances in Truthmaker Semantics II (July 29, 2025)
- **[Abstract](talks/truthmaker_advances/abstract.tex)** ([PDF](talks/truthmaker_advances/abstract.pdf))
- Joint presentation with Miguel Buitrago
- Case studies: Fine's imposition semantics and Champollion's unilateral semantics
- Introduces both model-checker and proof-checker

#### Topos Institute (June 17, 2025)
- **[Abstract](talks/topos_talk/abstract.tex)** ([PDF](talks/topos_talk/abstract.pdf))
- Focus on programmatic methodology for prototyping semantic theories
- Includes live demonstration of the workflow

#### MIT WIP
- **[Abstract](talks/mit_wip/abstract.tex)** ([PDF](talks/mit_wip/abstract.pdf))
- Earlier version focusing on truthmaker semantics applications

### Handout

- **[Handout](handout/handout.tex)** ([PDF](handout/handout.pdf)) - Presentation handout material

## Key Contributions

The ModelChecker framework makes six principal contributions:

1. **Theory-Agnostic Architecture** - Infrastructure independent of particular semantic theories
2. **Compositional Modularity** - Operators as self-contained units enabling systematic reuse
3. **Systematic Comparative Methodology** - Running identical arguments through multiple theories
4. **Bounded Model Exploration** - Systematic countermodel discovery with isomorphism detection
5. **TheoryLib** - Extensible library with 4 semantic theories and 177+ examples
6. **Computational Complexity as Theoretical Virtue** - Empirical measures of semantic theory tractability

## Implemented Theories

TheoryLib currently includes four semantic frameworks:

- **Logos** - Bilateral truthmaker semantics
- **Exclusion** - Unilateral truthmaker semantics
- **Imposition** - Fine's imposition semantics for counterfactuals
- **Bimodal** - Temporal-modal logic

## Software

- **[model-checker](https://pypi.org/project/model-checker/)** - Python package for semantic theory validation using Z3 SMT solver
- **proof-checker** - Complementary utility for developing proof theory in LEAN (in development)

## Current Status

- Paper outline created
- Research of related programs pending
- Main paper draft in early stages
- Testing of Logos, Imposition, and Exclusion theories pending
- Architecture diagrams to be created

## Related Work

This project builds on:
- SMT solver technology (Z3) for constraint satisfaction
- Fine's modalized state spaces and truthmaker semantics
- Champollion's unilateral semantics
- Automated reasoning approaches to modal and temporal logic

## License & Contact

For questions or collaboration inquiries, contact Benjamin Brast-McKie at brastmck@mit.edu
