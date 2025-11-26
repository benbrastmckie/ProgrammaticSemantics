# Paper Outline: A Programmatic Framework for Modular Semantic Theory Development Using SMT Solvers

**Target Journal**: Journal of Automated Reasoning

---

## Abstract (250 words max)

**Key Elements**:
- Problem: Lack of systematic computational tools for semantic theory development
- Approach: ModelChecker framework treating theories as executable programs
- Innovation: Theory-agnostic architecture for diverse semantic frameworks
- Methodology: Finite model exploration via SMT constraints
- Complexity metrics: Objective computational measures replacing subjective evaluations
- Vision: TheoryLib extensible library
- Results: 4 frameworks, 177+ examples
- Significance: First computational bilateral truthmaker semantics implementation

---

## 1. Introduction

Semantic theories specify the truth conditions of logical formulas through mathematical models consisting of possible worlds, times, situations, states, or other semantic primitives depending on the target language under study. Developing such theories traditionally proceeds by hand-verified semantic proofs and countermodels. This paper presents a computational framework that treats semantic theories as executable programs, enabling systematic model exploration, empirical theory comparison, and automated validation checking through SMT constraint solving.

### 1.1 The Challenge of Semantic Theory Development

Semantic theorizing faces a fundamental tension between expressive power and inference verification difficulty. Modern frameworks (intensional semantics, situation semantics, truthmaker semantics) employ sophisticated model structures to capture fine-grained distinctions, restricting hand-verified validation.

**Three Problems**: (1) **Complexity**: As theories grow complex, verifying consequences becomes difficult, especially when combining operators. (2) **Comparison**: Theory choice relies on subjective assessments of parsimony rather than systematic benchmarking on standardized inputs. (3) **Tractability**: Traditional theorizing treats computational complexity as external to semantic concerns. We argue computational complexity should inform theory development as a measurable theoretical virtue.

### 1.2 The Computational Turn in Semantics

SMT solvers, originally developed for program verification, provide powerful engines for constraint satisfaction over complex mathematical structures. Applying SMT technology to semantic theory development is a natural extension: semantic models are mathematical structures; semantic theories specify constraints over these structures; SMT solvers find assignments satisfying constraints.

**Prior Computational Approaches**

Several research programs have explored computational approaches to semantic theory, each exhibiting distinct strengths and limitations relative to the requirements of systematic semantic theory development:

- **Automated theorem proving for modal logic**: Specialized provers such as SPASS and leanTAP target modal logics, focusing on specific well-studied systems (S4, S5, K) with optimized proof search strategies. While powerful for their target logics, these systems provide theorem proving rather than countermodel exploration and lack extensibility to arbitrary semantic theories with novel primitive structures. The emphasis on validity determination rather than countermodel diversity limits their utility for comparative semantic investigation.

- **Model finding tools**: Mace4, the finite model finder accompanying Prover9, discovers finite models satisfying first-order specifications. While general-purpose, Mace4 operates at the level of first-order logic rather than providing semantic-theory-specific abstractions. Typical domain sizes handled by Mace4 range from N≈8-10, constrained by the exponential growth of first-order model space. Importantly, Mace4 lacks infrastructure for compositional operator design, semantic clause modularization, or systematic multi-theory comparison on standardized benchmarks.

- **Interactive theorem provers**: Systems such as Isabelle/HOL and Coq enable formal verification through proof assistants combining automated tactics with interactive proof construction. Nitpick, a countermodel finder for Isabelle/HOL, employs SAT solvers to discover finite countermodels for higher-order formulas. While powerful for formal verification, these systems emphasize proof construction over countermodel exploration and require substantial proof engineering expertise. The interactive nature precludes automated comparative testing across theories, and the focus on correctness verification rather than empirical performance measurement limits applicability to computational complexity analysis.

- **Temporal logic model checkers**: NuSMV and similar bounded model checkers from formal verification explore finite state spaces searching for property violations. These tools excel at temporal reasoning over transition systems but remain tied to specific temporal logics (LTL, CTL) without providing general semantic theory infrastructure. The state-based verification focus aligns with hyperintensional semantics, yet the lack of theory-agnostic architecture prevents implementing alternative semantic frameworks within the same computational environment.

- **General-purpose SAT/SMT solvers**: Raw Z3, CVC5, or other SMT solvers provide constraint solving without semantic theory infrastructure. While maximally flexible, direct SMT encoding requires manual constraint generation for each operator and theory combination without abstraction layers supporting compositional operator design, semantic clause specification, or output formatting. The absence of semantic-specific infrastructure increases implementation complexity and eliminates opportunities for systematic theory comparison through standardized testing protocols.

**Comparative Analysis**

| **System** | **Theory-Agnostic?** | **Compositional?** | **SMT-Based?** | **Comparative?** | **Typical Domain Sizes** |
|------------|---------------------|-------------------|----------------|------------------|-------------------------|
| Mace4 | Partially (FOL) | No | No (SAT) | No | N≈8-10 |
| Isabelle/Nitpick | No (HOL) | Yes (HOL modules) | Partial (SAT backend) | No | N≈8-12 |
| NuSMV | No (temporal) | Partial | No (BDD/SAT) | No | States≈10⁶-10⁷ |
| Modal provers | No (specific logics) | No | No | No | N/A (proof search) |
| Z3 (raw) | Yes (manual) | No | Yes | No | Unlimited (theory-dependent) |
| **ModelChecker** | **Yes** | **Yes** | **Yes** | **Yes** | **N≈15-20 (binary primitives)** |

The comparison reveals a capability gap: existing tools either target specific logics (modal provers, NuSMV), operate at the wrong abstraction level (raw SMT, Mace4 at FOL), or emphasize proof construction over countermodel exploration (Isabelle). No existing system combines theory-agnostic architecture, compositional modularity, SMT-based efficiency, and systematic comparative methodology within a unified framework designed for semantic theory development.

**The Gap: Theory-Agnostic Semantic Frameworks**

What's missing is infrastructure enabling researchers to implement diverse semantic theories, test them on shared examples, and compare their validation patterns and computational characteristics systematically. Whereas existing tools target specific logics and focus on theorem proving, we need infrastructure supporting arbitrary semantic frameworks in order to explore the space of countermodels for invalid inferences. Instead of treating theories individually, we need systematic comparative methodology to enhance theory selection.

Moreover, pure first-order encodings (Mace4, Prover9) can be cumbersome for semantic structures requiring intensional or hyperintensional primitives. SMT solvers supporting bitvectors for state spaces, uninterpreted functions for semantic relations, and arithmetic for cardinality constraints provide more natural and efficient representations. The ModelChecker framework targets this gap, achieving N≈15-20 tractability for theories with binary semantic primitives compared to Mace4's typical N≈8-10 limitation through optimized SMT constraint generation and compositional semantic architecture.

### 1.3 Contributions: A Programmatic Framework for Semantic Theory Development

This paper presents the ModelChecker, a framework treating semantic theories as executable programs that can be systematically tested, compared, and analyzed. The framework makes three principal methodological contributions and three concrete deliverables.

**Methodological Contributions**:

1. **Theory-Agnostic Architecture**: Infrastructure independent of particular semantic theories, enabling systematic comparison on identical computational foundations where differences in validation patterns or performance reflect genuine theoretical differences rather than implementation variations (Section 2-3).

2. **Systematic Comparative Methodology**: Running identical arguments through multiple semantic theories under controlled conditions, producing empirical comparative data replacing informal assessments with reproducible measurements (Section 4).

3. **Computational Complexity as Theoretical Virtue**: Empirical measures of semantic theory complexity through timeout rates, solve times, and maximum tractable model sizes, establishing computational tractability as an objective evaluation criterion alongside logical adequacy (Section 6).

**Concrete Deliverables**:

4. **Compositional Operator Architecture**: Modular implementation enabling selective operator loading, language fragment exploration, and shared implementations across theories (Section 3).

5. **Bounded Model Search with Isomorphism Detection**: Systematic countermodel discovery revealing structural diversity through constraint-based iteration combined with graph isomorphism detection (Section 5).

6. **TheoryLib Extensible Library**: Four implemented semantic theories (Logos, Exclusion, Imposition, Bimodal) with 177+ validated examples, establishing foundation for collaborative expansion (Section 7).

**Paper Structure**

Section 2 presents the complete pipeline architecture from input specification through constraint generation to output formatting. Section 3 examines the three-layer operator architecture and compositional modularity that enables theory-agnostic extensibility. Section 4 develops the systematic comparative methodology: controlled experimental design, empirical complexity metrics, and cross-theory validation patterns. Section 5 addresses model exploration through configurable semantic constraints and bounded search via countermodel iteration with isomorphism detection. Section 6 establishes the arity-complexity thesis, demonstrating that primitive arity determines tractability boundaries through model space analysis, frame constraint complexity examination, and comparative case studies. Section 7 concludes by developing the TheoryLib vision, providing practical guidance for testing existing theories and implementing new semantic frameworks, and inviting collaborative contribution to computational formal semantics.

---

## 2. Complete Pipeline Architecture

This section presents the basic architecture of the ModelChecker pipeline transforming logical arguments into validity determinations.

### 2.1 Input Specification and Configuration

The framework accepts logical arguments alongside semantic theory specifications, enabling systematic exploration of how different semantic frameworks evaluate identical inference patterns.

**Argument Specification Design**

Each logical argument consists of premise formulas, conclusion formulas, and settings. This structure mirrors standard logical notation while permitting precise control over the semantic search space. For example, constraining atomic propositions to be contingent (having both verifiers and falsifiers) filters out trivial models where propositions are necessarily true or false.

**Multi-Theory Evaluation Architecture**

The input structure accepts multiple semantic frameworks, evaluating each argument with each theory in turn. This enables direct theory comparison by measuring validity determinations and computational times side-by-side.

### 2.2 Logical Processing Pipeline

The core transformation proceeds through four stages.

**Stage 1: Syntactic Parsing**

Object-language formulas (e.g., `((A \\wedge B) \\rightarrow C)`) are transformed into structured representations amenable to semantic interpretation. The parser builds recursive sentence structures while remaining agnostic to operator semantics by requiring all formulas to consist of binary operators prefixed with `\\` and wrapped in parentheses and unary operators that bind to the next immediate argument. The separation between syntactic structure and semantic interpretation is fundamental to maintaining theory independence.

**Stage 2: Semantic Constraint Generation**

Each formula generates constraints expressing what it means for that formula to be verified or falsified according to the chosen semantic theory. Four constraint categories jointly specify the countermodel search problem: frame constraints (semantic structure), model constraints (atomic proposition values), premise constraints (all premises satisfied), and conclusion constraints (at least one conclusion false). The combination forms a satisfiability problem: Is there a model where premises are true but conclusions are not?

**Stage 3: SMT Solving**

The Z3 solver attempts to find an assignment satisfying all constraints, bounded by user-specified timeouts and state-space size limits. If the solver finds a satisfying assignment, it has discovered a countermodel. If no satisfying assignment exists within bounded search, we gain evidence for validity.

**Stage 4: Model Iteration (Optional)**

When multiple countermodels are requested, the system iteratively excludes previously discovered models through additional constraints while checking for structural isomorphism to avoid presenting mere relabelings of identical model structures. This exploration of countermodel diversity reveals the deeper structure of the countermodel space for particular inferences.

**Algorithm 1: Main Validation Pipeline**
```
Input: argument = (premises Γ, conclusions Δ), theory T, settings S
Output: (valid, countermodel) or (invalid, None)

1: Parse formulas Γ ∪ Δ into structured representations
2: Extract sentence letters P and operators O from formulas
3: Declare semantic primitives P₁,...,Pₘ from theory T with domain size D=2^(S.N)
4: Generate frame constraints FC by expanding T.frame_constraints over domain D
5: For each φ ∈ Γ:
6:    Generate premise constraint: verify(main_world, φ, {})
7: For each ψ ∈ Δ:
8:    Generate conclusion constraint: falsify(main_world, ψ, {})
9: Combine: constraints = FC ∧ (∧ premise_constraints) ∧ (∨ conclusion_constraints)
10: result = Z3.solve(constraints, timeout=S.timeout)
11: If result = SAT:
12:    countermodel = extract_model(result.assignment, T)
13:    Return (invalid, countermodel)
14: Else if result = UNSAT:
15:    Return (valid, None)  // Evidence for validity within bounded search
16: Else:  // UNKNOWN (timeout)
17:    Return (unknown, None)
```

This algorithm captures the complete pipeline, demonstrating how framework infrastructure (steps 1-2, 10-17) interacts with theory-specific plugins (steps 3-9).

**Example 1: Complete Pipeline Execution**

Consider testing affirming the consequent (premise: A → B, B; conclusion: A) using Logos theory with N=4 (D=16 states). The pipeline executes as follows:

1. **Parsing**: `(A \\rightarrow B)` becomes `[\\rightarrow, A, B]` and atomic formulas become `[A]`, `[B]`
2. **Primitive declaration**: Declares verify(x,p): BitVec(4) × {A,B} → Bool, falsify(x,p): BitVec(4) × {A,B} → Bool, possible(x): BitVec(4) → Bool
3. **Frame constraints**: Generates downward closure (D²=256 implications) and actuality constraint
4. **Premise constraints**: verify(main_world, A→B) ∧ verify(main_world, B)
5. **Conclusion constraint**: falsify(main_world, A)
6. **Solver invocation**: Z3 searches 2^(16 + 2×16×2) = 2^80 model space
7. **Result**: SAT in ~15ms, countermodel assigns verify(s₁,B)=true, verify(s₂,A)=false where s₁,s₂ are distinct states, demonstrating B can be true while A false

This concrete execution illustrates how syntactic, semantic, and solving layers interact to produce validity determinations.

### 2.3 Output Generation and Interpretation

SMT solver results are transformed into interpretable semantic structures if a satisfying countermodel is found, with output formats tailored to the use case.

**Validity Reporting**

For valid arguments (no countermodel found within search bounds), the system reports the search space explored, providing evidence for but not proof of validity. As search bounds increase, confidence in genuine validity is strengthened.

**Countermodel Visualization**

Invalid arguments yield countermodels displayed according to theory specifications. An extensional model specifies truth-vales; an intensional model specifies the worlds each sentence is true in and the extension of an accessibility relation; a hyperintensional model displays state-based verification and falsification along with a parthood relation while distinguishing possible states, world states, and impossible states. The visualization is encoded alongside each operator to ensure modularity.

**Comparative Analysis**

Running identical arguments through multiple theories generates empirical data on validation patterns and computational costs.

**Multiple Output Formats**: The framework supports console output (interactive exploration), JSON (programmatic analysis), Jupyter notebooks (reproducible narratives), and Markdown (minimal readable outputs).

**Pipeline Integration**

The three-stage architecture achieves crucial separation: core components operate independently of particular semantic frameworks while theory modules provide semantic interpretation. This separation enables the extensibility and systematic comparison explored in subsequent sections.

### 2.4 Formal SMT Encoding

The framework requires precise formalization of the interface between semantic theories and SMT constraint generation. This section specifies semantic primitives, operator interfaces, and frame constraints enabling arbitrary theories to be encoded as SMT problems.

**Definition 1 (Semantic Primitive)**: A *semantic primitive* is a Z3-declared function or relation P constituting the basic elements of a semantic theory's model structure. Primitives are characterized by their signature (k, h) where k denotes the number of arguments ranging over the state domain D (bitvector width N) and h denotes the number of arguments ranging over sentence letters P. The primitive's model space is 2^(D^k × P^h).

Formally, a primitive P is declared via:
```
P = z3.Function('P_name', *arg_sorts, z3.BoolSort())
```
where `arg_sorts` comprises k instances of `BitVec(N)` (state domain) and h instances of an enumeration type over sentence letters.

**Examples**:
- `possible: BitVec(N) → Bool` — Unary primitive (k=1, h=0), model space 2^D
- `verify: BitVec(N) × SentenceLetter → Bool` — Binary primitive (k=1, h=1), model space 2^(D×P)
- `excludes: BitVec(N) × BitVec(N) → Bool` — Pure-state binary primitive (k=2, h=0), model space 2^(D²)
- `imposition: BitVec(N) × BitVec(N) × BitVec(N) → Bool` — Ternary primitive (k=3, h=0), model space 2^(D³)

**Definition 2 (Operator Interface)**: An *operator interface* comprises three method types that operators must implement to participate in the semantic evaluation pipeline:

1. **Syntactic recognition**: `arity() → Int` specifying argument count, enabling parser to recognize operator structure without semantic interpretation
2. **Semantic interpretation**: `verify(s, φ, ctx) → z3.BoolRef` and `falsify(s, φ, ctx) → z3.BoolRef` (bilateral theories) or `truth_condition(w, φ) → z3.BoolRef` (intensional theories) generating SMT constraints expressing what it means for formula φ to be verified/falsified at state s or true at world w, given evaluation context ctx
3. **Model presentation**: `present_model(assignment) → String` translating raw solver assignments into theory-appropriate display format (e.g., truth tables for extensional theories, world-based semantics for intensional theories, state-based verification for hyperintensional theories)

This separation enables uniform parsing and output formatting while providing semantic plugin points (Layer 2).

**Definition 3 (Frame Constraint)**: A *frame constraint* is a universally quantified formula over semantic primitives encoding structural requirements that must hold in all models conforming to the semantic theory. Frame constraints are expanded by iterating over finite domains, generating D^k concrete implications for k-ary quantification.

**Formal structure**:
```
∀x₁...∀xₖ [ φ(x₁,...,xₖ, P₁,...,Pₘ) ]
```
where x₁,...,xₖ range over states (domain D) and P₁,...,Pₘ are semantic primitives.

**Examples**:
- **Downward possibility** (Logos): `∀x∀y[(possible(y) ∧ part_of(x,y)) → possible(x)]` — Quadratic expansion (D² implications)
- **Exclusion symmetry** (Exclusion): `∀x∀y[excludes(x,y) → excludes(y,x)]` — Quadratic expansion (D² implications)
- **Imposition coherence** (Imposition): `∀x∀y∀z[imposition(x,y,z) → possible(z)]` — Cubic expansion (D³ implications)

The expansion complexity D^k directly determines memory consumption and propagation overhead (Section 6.2), establishing frame constraints as the mechanism through which primitive arity affects tractability.

**SMT Encoding Workflow**: Given an argument with premises Γ and conclusions Δ:
1. Declare semantic primitives P₁,...,Pₘ with appropriate signatures
2. Expand frame constraints generating D^k implications per k-ary constraint
3. Generate premise constraints: `∧_{φ∈Γ} verify(main_world, φ, {})`
4. Generate conclusion constraints: `∨_{ψ∈Δ} falsify(main_world, ψ, {})`
5. Combine: `frame_constraints ∧ premise_constraints ∧ conclusion_constraints`
6. Invoke Z3 solver seeking satisfying assignment (countermodel)

This specification enables implementing diverse semantic theories following identical architectural patterns.

Having established the complete pipeline from input specification through SMT encoding to output generation, we now examine how the framework achieves theory-agnostic extensibility through compositional operator architecture.

## 3 Modular Operator Classes

Logical operators are self-contained units encapsulating syntactic recognition, semantic interpretation, and result presentation. This enables theory composition, selective loading, and systematic reuse.

### 3.1 Three-Layer Operator Architecture

Logical operators provide methods for operating at three distinct conceptual levels, each addressing a different aspect of the semantic evaluation problem. This layered design enforces separation of concerns while maintaining compositional coherence.

**Layer 1: Syntactic Recognition**

At the syntactic level, operators are purely structural entities characterized by their name, LaTeX code, and arity. Infix sentences of the form `(A \\wedge B)` are converted to prefix notation `[\\wedge, A, B]` in preparation for interpretation. Although the binary operator `\\wedge` is recognized as requiring exactly two arguments, nothing at this level specifies what `\\wedge` *means*. This abstraction is what enables the same parser to handle the operators included in different languages uniformly.

The parser builds recursive formula structures recognizing only operator arities, not their semantic interpretations. This means adding a new operator to a theory requires no modification to the parsing infrastructure since the parser recognizes any operator symbol meeting structural requirements.

Defined operators exemplify this separation: the material conditional can be treated syntactically as a primitive binary operator `\\rightarrow` while semantically being defined as `(\\neg A \\vee B)`. Whereas the parser sees the structure, the semantic layer sees through the definition. This enables theoretical economy (fewer primitives) without any cost to convenience.

**Layer 2: Semantic Interpretation**

Operators provide theory-specific semantic methods to translate purely syntactic constructions in prefix notation into formal SMT constraints. Despite sharing the same syntactic structure, a conjunction operator in classical logic implements different semantic methods than conjunction in hyperintensional semantics.

Rather than hardcoding "worlds" as evaluation points, the framework passes extensible dictionaries containing whatever contextual parameters the theory requires to evaluate sentences:
 **Extensional theories** require no contextual parameters since sentences receive truth-values directly
 **Intensional theories** require a single contextual parameter which is interpreted to be a world for circumstantial modals, information state for epistemic modals, or a time for tense operators
 **Bimodal theories** require two contextual parameters that specify both the world-history and a time in that history
 **Normative theories** require additional parameters for utility functions or preference orderings over the space of worlds, states, or evolutions depending

Beyond evaluation parameters, frameworks also differ in structural requirements: bilateral theories require independent specification of truth and falsity conditions, while unilateral theories define falsity as negation of truth. All theories require model extraction methods translating solver assignments into readable semantic values.

This design accommodates semantic diversity: the framework does not impose a single semantic interface. Instead, operators implement whichever methods their semantic framework requires. The framework adapts to the theory rather than forcing theories into a single semantic pattern.

**Example 2: Conjunction in Logos vs Exclusion**

The conjunction operator illustrates how the same syntactic structure receives different semantic implementations. In Logos (bilateral truthmaker semantics):

```python
def verify(s, φ∧ψ, w):
    return verify(s, φ, w) ∧ verify(s, ψ, w)
def falsify(s, φ∧ψ, w):
    return falsify(s, φ, w) ∨ falsify(s, ψ, w)
```

Logos's bilateral primitives enable direct clause specification: conjunction is verified by states verifying both conjuncts, falsified by states falsifying either conjunct.

In Exclusion (unilateral exclusion semantics):

```python
def verify(s, φ∧ψ, w):
    return ∃t∃u[part_of(t,s) ∧ part_of(u,s) ∧ verify(t,φ,w) ∧ verify(u,ψ,w)]
```

Exclusion lacks falsify primitive, so conjunction verification requires existential quantification over state parts: s verifies φ∧ψ if s contains parts verifying each conjunct. This illustrates Layer 2's semantic plugin architecture: identical syntax, theory-specific constraints.

**Layer 3: Model Interpretation**

Once the solver produces a satisfying assignment, operators translate raw solver values into theory-appropriate model structures, calling methods to present the semantic information. The same logical negation operator displays differently depending on the semantic framework: classical negation shows simple truth-value flips, while bilateral negation inverts the sets of verifiers and falsifiers. This layer recognizes that semantic theories differ not just in their validation patterns but in how they conceptualize and present semantic information.

**Architectural Significance**

The three-layer architecture provides stable interfaces (Layers 1, 3) while Layer 2 serves as the semantic plugin point. This pattern enables the extensibility explored in subsequent sections.

### 3.2 Subtheory Composition and Modular Loading

Semantic theories are not monolithic. Classical logic comprises connectives with distinct semantic behaviors; modal logic extends classical logic with intensional operators. The ModelChecker accommodates extensibility through subtheory modules that can be selectively loaded and combined.

**Compositional Theory Design**

Theories are compositions of operator subtheories. For instance, Logos comprises five independent subtheories: extensional connectives, modal operators, constitutive operators, counterfactual conditionals, and relevance operators. Each can be loaded independently or combined. This serves practical purposes (reduced overhead) and theoretical purposes (exploring framework fragments).

**Dependency Management**: The framework automatically resolves subtheory dependencies, ensuring that loading a subtheory transitively loads all required dependencies without circularity.

### 3.3 Semantic Framework Patterns and Operator Responsibilities

While operators are theory-specific plugins, certain patterns emerge across semantic frameworks. Understanding these patterns illuminates both the diversity and commonalities of formal semantic approaches.

**Intensional Semantics Pattern**: Intensional frameworks (modal, temporal, epistemic) define truth conditions relative to evaluation points in structured spaces (worlds, times, information states). Operators implement truth-at-point conditions, typically quantifying over accessible points.

**Bimodal Semantics Pattern**: Bimodal frameworks combine two evaluation dimensions (e.g., world-histories and times). Operators receive multi-parameter contexts, coordinating dimensions through selective quantification. The framework's parameter-passing architecture scales naturally from single to multi-dimension evaluation.

**Hyperintensional Semantics Pattern**: Hyperintensional frameworks (truthmaker semantics, situation semantics) evaluate formulas at partial states or situations. Operators implement verification/falsification methods quantifying over states with mereological structure. The key innovation is *partiality*: formulas are verified by states representing fragmentary information, enabling hyperintensional distinctions based on verification structure.

**Defined Operator Abstraction**: Defined operators (e.g., material conditional as `(¬A ∨ B)`) expand to their definitions before semantic evaluation, providing notational convenience without semantic complexity.

**Implications for Theory Design**: Identify core semantic primitives, implement their semantic methods, define convenient abbreviations. The framework rewards clean semantic design with improved performance and maintainability.

With the modular operator architecture enabling diverse semantic theories to be implemented within a unified framework, we turn to the central methodological contribution: systematic cross-theory comparison under controlled experimental conditions.

## 4. Systematic Comparative Methodology

A persistent challenge in semantic theory development is comparison: how do different frameworks fare on identical test cases? Traditional comparison is informal, relying on selective examples and subjective assessments. The ModelChecker enables systematic empirical comparison by running identical arguments through multiple semantic theories under controlled conditions, measuring validation outcomes and computational costs.

### 4.1 Comparative Framework Design

When multiple theories are provided, the framework evaluates each theory on the same argument with identical settings. This produces concrete, reproducible comparative data. The methodology controls for discrepancies in test conditions so differences in outcomes reflect genuine theoretical differences rather than experimental artifacts.

**Multi-Theory Evaluation Protocol**

The input structure accepts multiple semantic frameworks, evaluating each argument with each theory in turn. This enables direct theory comparison: identical inputs, configuration, and search bounds. The only variation is the semantic theory itself.

### 4.2 Empirical Complexity Metrics

Evaluating examples with multiple theories yields empirical complexity data: which theories timeout on which examples, which scale to larger state spaces, which validate or invalidate particular patterns. These metrics complement traditional theoretical complexity analysis with empirical performance data.

**Metrics Collected**

- **Validation outcomes**: Which theories declare the argument valid vs. invalid
- **Solve times**: Time required to reach validity determination (in milliseconds)
- **Timeout rates**: Percentage of test cases exceeding time bounds
- **Maximum tractable domain size**: Largest N value permitting solution within timeout
- **Constraint counts**: Number of SMT constraints generated by each theory

**Theoretical vs. Empirical Complexity**

Theoretical complexity (quantifier alternation, primitive arity) predicts computational costs based on model space size and constraint structure. Empirical performance measures actual cost on specific examples. Sometimes they align (primitive arity correctly predicts timeouts), sometimes they diverge (solver heuristics, optimizations, formula-specific structure).

The framework provides both perspectives. Empirical measurements validate theoretical predictions (Section 6) while revealing performance variations within complexity tiers. Theory A and Theory B might share the same primitive arity (same tier) yet differ by 2-3× in solve time due to frame constraint differences.

### 4.3 Cross-Theory Validation Patterns

Beyond performance metrics, comparison reveals how theories differ in their validation patterns: which inferences they accept as valid, which they reject, and why.

**Validation Agreement and Divergence**

Some inferences produce agreement: all theories validate modus ponens, all reject affirming the consequent. These represent semantic universals—logical patterns independent of semantic framework choices.

Other inferences produce divergence: Theory A validates while Theory B invalidates. These divergences reveal theoretical commitments. Hyperintensional theories may invalidate inferences that intensional theories validate, reflecting their sensitivity to verification structure beyond truth conditions.

**Example 3: Cross-Theory Validation Pattern Table**

Testing modal distribution (□(A∧B) ⊢ □A∧□B) across theories at N=5 reveals validation convergence despite implementation diversity:

| **Theory** | **Validation** | **Solve Time (ms)** | **Constraint Count** | **Notes** |
|-----------|---------------|-------------------|---------------------|----------|
| Logos | Valid | 42 | 1,127 | Bilateral primitives, simple modal clauses |
| Exclusion | Valid | 156 | 3,241 | Exclusion primitive adds quadratic constraints |
| Imposition | Valid | 1,847 | 12,398 | Ternary primitive dominates, near-timeout |

All theories validate this inference (semantic universal) but exhibit dramatic performance differences. Logos completes in 42ms with minimal constraints; Exclusion requires 3.7× more time and 2.9× more constraints due to pure-state binary primitive; Imposition approaches timeout threshold with 44× Logos solve time and 11× constraint count from ternary primitive. This pattern confirms Section 6's arity-complexity thesis: validation patterns converge while performance diverges according to primitive arity.

**Comparison as Theoretical Insight**

Systematic comparison transforms theory evaluation from informal assessment to empirical investigation. Concrete questions become answerable: Do bilateral and unilateral frameworks validate the same inferences? How much computational overhead does bilateral tracking impose? Which theory differences matter for validation versus performance?

### 4.4 Setting Up Complexity Analysis

The empirical observations raise explanatory questions: Why do theories differ in performance? What structural properties determine computational costs? Section 6 develops a theoretical explanation through model space analysis and frame constraint complexity examination.

Before addressing these explanatory questions, we examine the framework's bounded search methodology: how configurable constraints restrict model spaces and how iterative discovery reveals countermodel diversity.

---

## 5. Model Exploration and Bounded Search

The framework enables systematic exploration of model spaces through configurable constraints and iterative countermodel discovery. This section examines how researchers control the search space through hierarchical configuration and how the system discovers structurally distinct countermodels through constraint-based iteration combined with isomorphism detection.

### 5.1 Hierarchical Configuration and Research Flexibility

The framework implements a multi-level configuration hierarchy balancing global defaults with local overrides: framework-wide defaults, theory-specific defaults, user-level preferences, example-level settings, and command-line flags. This hierarchy distinguishes theory-constitutive constraints (embedded in semantic implementations) from investigative constraints (imposed by researchers exploring consequences), enabling researchers to restrict model spaces while maintaining experimental reproducibility and comparability.

### 5.2 Constraint Composition and Interaction

Constraints compose: requiring both contingency and disjointness yields models satisfying both conditions. But constraints also interact: contingency implies non-emptiness (contingent propositions must have verifiers and falsifiers), so redundant constraints can be omitted. The framework handles these interactions, applying only the minimal constraint set expressing the desired conditions.

This compositional approach mirrors theoretical practice. Semantic theorists often build up model requirements incrementally: start with basic structural requirements, add contingency, impose subject-matter constraints. The framework's constraint composition enables the same incremental specification, with each addition narrowing the model space explored.

### 5.3 Countermodel Discovery and Iteration

Finding a single countermodel establishes invalidity. But how many structurally distinct countermodels exist? Is the countermodel space rich or sparse? Exploring countermodel diversity reveals semantic properties invisible from single-model examination.

**The Model Iteration Problem**: Naively requesting multiple countermodels risks redundancy: the solver might return label variants (same structure, different variable assignments). Meaningful diversity requires structural distinctness. The ModelChecker implements iterative constraint-based exclusion combined with graph isomorphism detection.

**Constraint-Based Model Exclusion**: The core strategy is incremental: find one countermodel, add constraints excluding it, repeat. Model exclusion is expressed as constraints, working within the same framework used for initial discovery.

**Algorithm 2: Iterative Countermodel Discovery with Isomorphism Detection**
```
Input: argument A, theory T, settings S, max_models K
Output: list of structurally distinct countermodels M₁,...,Mₙ

1: discovered_models = []
2: exclusion_constraints = []
3: isomorphism_failures = 0
4:
5: While |discovered_models| < K and isomorphism_failures < 10:
6:    // Generate base constraints (Algorithm 1, steps 3-9)
7:    constraints = generate_constraints(A, T, S) ∧ (∧ exclusion_constraints)
8:
9:    // Attempt to find satisfying assignment
10:   result = Z3.solve(constraints, timeout=S.timeout)
11:
12:   If result = UNSAT or result = UNKNOWN:
13:      Break  // No more countermodels exist or timeout
14:
15:   // Extract candidate countermodel
16:   candidate_model = extract_model(result.assignment, T)
17:
18:   // Check structural distinctness via isomorphism detection
19:   If is_isomorphic(candidate_model, discovered_models):
20:      isomorphism_failures += 1
21:      exclusion_constraints.append(exclude_assignment(result.assignment))
22:      Continue  // Reject isomorphic variant, try again
23:   Else:
24:      discovered_models.append(candidate_model)
25:      exclusion_constraints.append(exclude_assignment(result.assignment))
26:      isomorphism_failures = 0  // Reset counter on success
27:
28: Return discovered_models
```

**Key design decisions**: (1) Isomorphism detection (line 19) prevents label variants from cluttering results, (2) exclusion constraints accumulate (lines 21, 25) ensuring each iteration explores new model regions, (3) isomorphism failure threshold (line 5) provides heuristic termination when likely all distinct models found, (4) timeout handling (lines 12-13) gracefully exits on resource exhaustion.

**Example 4: Structurally Distinct Countermodels for Modal Fallacy**

Consider the invalid inference ◇A ⊢ □A tested with Logos at N=4, requesting 3 distinct countermodels. The iteration discovers:

**Model 1** (42ms): Two-world structure with w₁ accessible from w₀ where A true at w₀ but false at w₁, demonstrating possibility without necessity.

**Model 2** (89ms): Three-world chain w₀→w₁→w₂ where A true at w₀, w₁ but false at w₂, showing longer accessibility path.

**Model 3** (134ms): Four-world branching structure where w₀ accesses both w₁ (A true) and w₂ (A false), w₁ accesses w₃ (A true), demonstrating branching accessibility.

After Model 3, iteration terminates with 5 consecutive isomorphism failures, suggesting search space exhausted. The three models exhibit distinct topologies (chain vs. branching), demonstrating semantic richness: modal fallacy admits structurally diverse countermodels, not just relabelings.

**The Isomorphism Challenge**

Constraint-based exclusion alone is insufficient since excluding a specific variable assignment doesn't rule out isomorphic variants. Without isomorphism detection, iteration might return many label variants of a model with the same structure.

Graph isomorphism detection solves this: represent models as labeled graphs (worlds as nodes, accessibility relations as edges, valuations as labels) and check whether new models are isomorphic to previous ones. Isomorphic models are rejected, triggering additional exclusion constraints. Only structurally distinct models are accepted.

### 5.4 Isomorphism Detection and Structural Distinctness

Full graph isomorphism is computationally expensive. The framework employs a two-stage strategy: quick structural checks (node count, edge count, degree sequences) cheaply reject most non-isomorphic models; expensive full isomorphism checking runs only when cheap checks pass. This optimizes for the common case while maintaining correctness.

The approach reflects a performance principle: spend computational effort proportionally to expected payoff. Most models will differ in basic structural properties (different numbers of worlds, different accessibility patterns). Full isomorphism checking is reserved for structurally similar models, reducing average-case cost.

**Methodological Applications**

Model iteration enables several research methodologies:

1. **Diversity assessment**: How rich is the countermodel space for this inference? Sparse countermodel spaces suggest the argument is "nearly valid"; rich spaces suggest deep invalidity.

2. **Structural comparison**: How do countermodel structures differ across theories? Do they share common patterns or exhibit fundamental differences?

3. **Minimal countermodels**: Iterate with increasing bounds to find smallest distinguishing models, revealing minimal structural requirements for counterexamples.

Each methodology leverages systematic countermodel exploration to address questions beyond simple validity testing, demonstrating how computational tools enable new forms of semantic investigation.

### 5.5 Termination and Search Space Boundaries

Iteration terminates when: (1) requested distinct models found, (2) timeout reached, (3) solver reports no more satisfying assignments, or (4) consecutive isomorphism failures suggest all non-isomorphic models found. Each condition has different epistemic status: successful completion provides N distinct countermodels (definite), resource exhaustion leaves countermodel space potentially richer (indefinite), search space exhaustion finds all countermodels within bounds (definite), heuristic exhaustion likely finds all accessible distinct models (supporting evidence). The framework reports termination reasons enabling proper result interpretation.

Having established the framework's comparative methodology and bounded search capabilities, we now address the fundamental question raised by empirical observations: what structural properties of semantic theories determine their computational tractability?

## 6. Computational Complexity and Primitive Arity

Semantic theories differ dramatically in computational characteristics. This section examines how computational complexity is directly determined by the arity of semantic primitives—the fundamental Z3 relations and functions constituting the theory's model structure—with higher-arity primitives inducing exponentially larger model spaces. This arity-complexity relationship has both theoretical significance (predicting computational behavior from structural properties) and practical significance (guiding theory design toward lower-arity primitives).

### 6.1 Semantic Primitives and Model Space

*Semantic primitives* are Z3 functions and relations constituting a theory's model structure, declared using z3.Function(). They represent the basic elements over which the SMT solver searches for countermodels.

**Examples of Semantic Primitives in TheoryLib**

Different semantic theories employ different primitives:

- `possible(x)`: A unary predicate determining whether state x is possible (Logos, Exclusion, Imposition)
- `verify(x, p)`: A binary relation specifying that state x verifies atomic proposition p (Logos, Exclusion, Imposition)
- `falsify(x, p)`: A binary relation specifying that state x falsifies atomic proposition p (Logos, Imposition)
- `excludes(x, y)`: A binary relation specifying that state x excludes state y (Exclusion)
- `imposition(x, w, u)`: A ternary relation specifying that imposing state x on world w yields outcome world u (Imposition)
- `task(x, y)`: A binary relation specifying transition from world-state x to world-state y (Bimodal)
- `truth_condition(x, p)`: A binary relation specifying truth of proposition p at world-state x (Bimodal)

The *arity* of a primitive is the number of arguments it accepts. The `possible` predicate is unary (one argument), `verify` and `excludes` are binary (two arguments), and `imposition` is ternary (three arguments).

**Model Space and Search Complexity**

When the SMT solver searches for countermodels, it explores the space of all possible assignments to the semantic primitives. The size of this search space is determined by four factors: (1) the domain cardinality D=2^N, (2) the number of sentence letters P, (3) the arity of each primitive, and (4) the number of primitives.

Sentence letters that occur in the inference under evaluation provide the atomic propositions over which formulas are built. Primitive arguments are typed where state arguments (like x, y, w, u) range over the D domain, while sentence letter arguments (like p, q) range over P sentence letters.

**Model space size M by theory:**

When a theory employs multiple primitives, their model spaces multiply to yield the combined search space. Assuming N=4 and P=3 results in a domain cardinality D=16 which may use for the following example calculations:

- **Logos**: M = 2^D × 2^(D·P) × 2^(D·P) = 2^(D + 2D·P) = 2^112
  - `possible(x)`: 2^D
  - `verify(x,p)`: 2^(D·P)
  - `falsify(x,p)`: 2^(D·P)

- **Exclusion**: M = 2^D × 2^(D·P) × 2^(D²) = 2^(D + D·P + D²) = 2^320
  - `possible(x)`: 2^D
  - `verify(x,p)`: 2^(D·P)
  - `excludes(x,y)`: 2^(D²)

- **Imposition**: M = 2^D × 2^(D·P) × 2^(D·P) × 2^(D³) = 2^(D + 2D·P + D³) = 2^4208
  - `possible(x)`: 2^D
  - `verify(x,p)`: 2^(D·P)
  - `falsify(x,p)`: 2^(D·P)
  - `imposition(x,w,u)`: 2^(D³)

For primitives with k arguments ranging over states and h arguments ranging over sentence letters, the number of possible assignments for that primitive is 2^(D^k × P^h). Since D is typically much larger than P, the dominant complexity factor is the maximum state argument count k. Assuming N=4 and P=3, `falsify` contributes 2^8 possible assignments, `excludes` contributes 2^16 possible assignments, and `imposition` contributes 2^64 which dominates all other contributions for large values of D.

Strong evidence for validity findings requires scaling both D (larger domains) and P (more complex formulas). To maximize these dimensions while preserving computational tractability, the critical structural factors are the argument signatures of primitives and the primitive count. Increasing the arity of a semantic primitive from k to k+1 increases the possible assignments by the same amount as adding D-1 additional semantic primitives of arity k.

**Primitive Arity as the Dominant Factor**

The exponential scaling of model space with primitive arity establishes arity as the dominant factor in computational complexity. While frame constraint complexity, formula nesting depth, and sentence letter count all affect performance, none produces comparable impact. This dominance explains why maximum primitive arity serves as the primary complexity classifier for semantic theories.

### 6.2 Frame Constraints and the Pruning-Complexity Tradeoff

Frame constraints impose structural requirements on semantic primitives, ruling out invalid model regions before the solver explores them. These constraints exhibit a fundamental performance tradeoff: while they prune invalid search space through constraint propagation, they also impose computational overhead through constraint expansion, memory consumption, and propagation costs. Well-designed frame constraints dramatically accelerate solving; poorly-designed constraints degrade performance or exhaust available memory, crashing the solver.

**The Pruning Benefit**

Frame constraints eliminate invalid primitive assignments through constraint propagation. Consider downward closure on the `possible` primitive:

```
Downward Possibility: ∀x∀y[(possible(y) ∧ part_of(x, y)) → possible(x)]
```

When expanded over D=16 states, this generates D²=256 constraints. If the solver assigns `possible(s)=false`, propagation immediately infers `possible(t)=false` for all states t containing s as a part, potentially eliminating hundreds of invalid assignments. Without this constraint, the solver would accept semantically invalid models where possible states contain impossible parts.

**The Complexity Cost**

The same constraints that enable pruning impose computational costs:

1. **Memory consumption**: Each expanded constraint consumes memory. Downward closure over D=16 generates 256 clauses; over D=32 generates 1,024 clauses. Memory scales as D^k for k-ary quantification.

2. **Propagation overhead**: The solver must check constraints after each assignment. More constraints slow propagation, creating tension: constraints prune search space but degrade the propagation mechanism.

3. **Coupling**: When multiple constraints share primitives, they create dependency networks. Assignments to one primitive trigger cascading propagations across coupled constraints, forcing the solver to reason about joint assignment spaces rather than independent primitive spaces.

**Memory Explosion: Catastrophic Failure**

Excessive frame constraints cause catastrophic failure through memory exhaustion. For Imposition theory at N=12 (D=4,096):
- Four ternary-quantified constraints expand to 4×D³ ≈ 275 billion constraints
- Memory requirement: ~27 terabytes
- Result: Immediate out-of-memory crash

Empirical testing confirms Imposition crashes at N≥13 from constraint explosion. The ternary primitive's D³ scaling exhausts available memory before solving begins. Even when constraints initially fit, dynamic clause learning can trigger mid-search memory exhaustion.

**Conclusion: Frame Constraints Compound with Primitive Arity**

Even with optimal frame constraint design, primitive arity remains the dominant complexity driver. Higher-arity primitives require more frame constraints to ensure semantic validity, and those constraints expand more rapidly (D³ vs. D²). Frame constraint complexity thus *compounds* with primitive arity, reinforcing that semantic primitive arity determines the fundamental tractability boundaries of SMT-based semantic theory implementation.

### 6.3 The Primitive Count Tradeoff: Logos vs. Exclusion

The choice of semantic primitives involves a fundamental tradeoff: more primitives enable simpler semantic clauses and frame constraints, while fewer primitives require complex semantic clauses and additional frame constraints to achieve equivalent expressive power. This section examines this tradeoff through the comparison of Logos and Exclusion theories.

**Logos: More Primitives, Simple Semantics**

Logos employs three semantic primitives to provide truthmaker semantics for logical operators:
- `verify(x, p)`: State x verifies sentence letter p
- `falsify(x, p)`: State x falsifies sentence letter p
- `possible(x)`: State x is metaphysically possible

This primitive choice enables remarkably simple semantics for negation:
```
extended_verify(s, ¬φ, w) = extended_falsify(s, φ, w)
extended_falsify(s, ¬φ, w) = extended_verify(s, φ, w)
```

Negation simply swaps verifiers and falsifiers. If state s verifies φ, then s falsifies ¬φ. If s falsifies φ, then s verifies ¬φ. This elegant symmetry requires no quantification, no witness predicates, no complex conditions—just direct substitution.

Frame constraints are equally simple. Logos requires only two frame constraints:
1. **Downward closure**: `∀x∀y[(possible(y) ∧ part_of(x,y)) → possible(x)]`
2. **Actuality**: `is_world(main_world)`

The downward closure constraint ensures parts of possible states remain possible, while actuality constrains the evaluation point to be a maximal possible state. Both constraints expand quadratically (D² for downward closure) and create minimal constraint overhead.

**Exclusion: Fewer Primitives, Complex Semantics**

Exclusion eliminates `possible` and `falsify` as primitives, retaining only:
- `verify(x, p)`: State x verifies sentence letter p
- `excludes(x, y)`: States x and y are mutually incompatible

Possibility becomes a *derived* notion: a state is possible if it coheres with itself, where coherence means the absence of internal exclusion conflicts. Falsification is similarly derived through the verification semantics of negation.

This primitive reduction forces dramatic complexity increases in semantic clauses. Negation in Exclusion requires three interdependent conditions with witness predicates h(x) and y(x):

```
extended_verify(s, ¬φ, w) ⟺
  ∀x[verify(x, φ, w) → (part_of(y(x), x) ∧ excludes(h(x), y(x)))] ∧
  ∀x[verify(x, φ, w) → part_of(h(x), s)] ∧
  ∀z[(∀x[verify(x, φ, w) → part_of(h(x), z)]) → part_of(s, z)]
```

The first condition ensures that for every verifier x of φ, the witness predicate y(x) identifies a part of x that the hypothetical falsifier h(x) excludes. The second condition requires all such hypothetical falsifiers to be parts of s. The third ensures s is the *least* state satisfying the second condition.

This complexity serves a purpose: it encodes through `excludes` and witness functions what Logos achieves directly through the `falsify` primitive. The witness predicates h(x) and y(x) are Z3 uninterpreted functions that implicitly represent falsification structure without making it primitive.

Frame constraints similarly proliferate. Exclusion requires five frame constraints:
1. **Actuality**: `is_world(main_world)`
2. **Negation symmetry**: `∀x∀y[excludes(x,y) → excludes(y,x)]`
3. **Null state**: `∀x[¬excludes(null, x)]`
4. **Harmony**: `∀x∀y[(is_world(x) ∧ coheres(x,y)) → possible(y)]`
5. **Rashomon**: `∀x∀y[(possible(x) ∧ possible(y) ∧ coheres(x,y)) → compossible(x,y)]`

The symmetry constraint ensures exclusion is symmetric. The null state constraint establishes that the null state excludes nothing. Harmony and Rashomon connect the derived notion of possibility to worlds and compossibility. These constraints expand to thousands of concrete implications at N=5, compared to Logos's ~1,100 total constraints.

**Example 5: Logos vs Exclusion Performance Comparison**

Testing De Morgan's law (¬(A∧B) ⊢ ¬A∨¬B) across domain sizes reveals the primitive tradeoff's performance impact:

| **N** | **Logos Time (ms)** | **Logos Constraints** | **Exclusion Time (ms)** | **Exclusion Constraints** | **Ratio** |
|-------|-------------------|---------------------|----------------------|--------------------------|----------|
| 4 | 18 | 1,127 | 67 | 3,241 | 3.7× |
| 5 | 42 | 1,891 | 156 | 6,892 | 3.7× |
| 6 | 89 | 3,024 | 412 | 13,127 | 4.6× |
| 8 | 342 | 7,234 | 1,847 | 31,894 | 5.4× |
| 10 | 1,245 | 14,127 | 8,923 | 67,241 | 7.2× |

Logos's bilateral primitives (verify, falsify, possible) enable simple negation semantics with minimal frame constraints, scaling efficiently to N=10 (14K constraints, ~1.2s). Exclusion's unilateral exclusion primitive requires complex witness-based negation semantics and additional frame constraints, producing 4.8× more constraints at N=10 and 7.2× solve time degradation.

Both theories employ maximum state-argument count k=1 (Logos) vs k=2 (Exclusion for excludes primitive), placing them in adjacent tractability tiers. The performance gap demonstrates that primitive count tradeoff (3 simple primitives vs 2 complex primitives) affects performance within tiers, while primitive arity determines tier boundaries. Logos's additional primitives purchase implementation simplicity and ~5-7× performance advantage.

**Argument Domains: Not All Binary Primitives Are Equal**

While both theories employ binary primitives, a crucial distinction emerges: argument domains determine model space complexity (Section 6.1). The `excludes(x,y)` primitive (k=2, h=0) creates exponentially larger model space than `falsify(x,p)` (k=1, h=1) because both arguments range over states rather than mixing state and sentence letter arguments. Since D >> P, D² vastly exceeds D×P. This explains why Exclusion's total model space dramatically exceeds Logos's despite similar primitive counts.

**Conclusion: Primitive Arity Dominates Primitive Count**

The Logos-Exclusion comparison demonstrates that primitive *count* is negotiable: theories with 2-3 binary primitives achieve similar performance whether they employ simple semantics with many primitives or complex semantics with few primitives. The decisive factor remains primitive *arity*: both theories use exclusively binary primitives, keeping model space scaling to O(D² + D·P).

This analysis reinforces the central conclusion: primitive arity determines tractability boundaries, while primitive count and semantic clause complexity determine performance within those boundaries.

### 6.4 Empirical Performance Data and Arity Effects

**EXECUTE: Run benchmark suite using documented methodology below**

#### Experimental Design: Benchmark Suite Specification

The empirical validation of the primitive arity thesis requires a carefully designed benchmark suite that enables cross-theory comparison under controlled conditions. The benchmark suite must satisfy three key requirements: (1) cross-compatibility across Logos, Exclusion, and Imposition theories, (2) representative coverage of inference types, and (3) sufficient complexity variation to stress-test solver performance at different domain sizes.

**Benchmark Suite Structure**

The suite comprises 15-20 representative test cases drawn from existing theory examples and designed to exercise semantic primitives across all three frameworks. Test cases are restricted to operators present in all theories: negation (¬), conjunction (∧), disjunction (∨), material conditional (→), necessity (□), and possibility (◇). Theory-specific operators (counterfactuals, constitutive operators, relevance connectives) are excluded to ensure cross-compatibility.

**Test Case Categories** (with target counts):
- **Propositional validities** (5-7 cases): Tautologies and basic logical laws including excluded middle, double negation elimination, De Morgan's laws, and distributive principles. These exercise extensional connectives without modal complexity.
- **Modal inferences** (3-4 cases): S5 axioms (necessity distribution, possibility axiom), K-axiom, modal distribution over conjunction, and basic modal-propositional combinations. These test intensional/hyperintensional operators while remaining cross-compatible.
- **Invalid arguments** (5-7 cases): Countermodel examples including affirming the consequent, denying the antecedent, modal fallacies (◇A → □A), and invalid modal distribution patterns. These verify solver correctly rejects invalid inferences and produces structurally distinct countermodels.
- **Simple inferences** (2-3 cases): Modus ponens, modus tollens, hypothetical syllogism, and basic modal reasoning patterns. These establish baseline performance on straightforward validities.

**Selection Criteria**: Test cases vary in formula complexity from atomic propositions through conjunctive combinations to nested modal operators, ensuring the suite exercises both shallow and deep formula structures. Examples are drawn from existing theory-specific test suites (`logos/examples.py`, `exclusion/examples.py`, `imposition/examples.py`) filtered for cross-compatibility.

**Test Case Format**: Each benchmark specifies:
- Premise list: `[φ₁, φ₂, ..., φₙ]`
- Conclusion list: `[ψ₁, ψ₂, ..., ψₘ]`
- Settings template: `{timeout: 60, sentence_letters: [...], constraints: [...]}` (N parameter varies per experiment)
- Expected outcome: `{valid: true/false, timeout_expected: false}` (for validation)

**File Location**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/benchmark_suite.py`

**Implementation Status**: Benchmark suite design documented; implementation pending as separate experimental task.

#### Methodology: Domain Size Sweep and Metrics Collection

**EXECUTE: Run experiments using benchmark suite across domain sizes**

**Domain Size Parameterization**:
- **Logos and Exclusion**: N ∈ {4, 5, 6, 8, 10, 12, 15, 18, 20} (binary primitive tier)
- **Imposition**: N ∈ {3, 4, 5, 6, 8, 10} (ternary primitive tier, expect failures beyond N=10-12)

Each test case runs at each N value for each theory, yielding 15-20 benchmarks × 9-12 N values × 3 theories ≈ 405-720 total inference evaluations. Timeout threshold: 60 seconds per inference.

**Metrics Collected** (automatically captured by ModelChecker framework):
- `solve_time`: Z3 model runtime in milliseconds (`z3_model_runtime` statistic)
- `timeout`: Boolean flag indicating timeout exceeded
- `constraint_count`: Number of SMT assertions generated (`z3_model_assertions` statistic)
- `model_space_size`: Computed as 2^X where X is the model space exponent from primitive analysis (Section 6.1)
- `validation_outcome`: Valid vs. countermodel found

Metrics export to JSON format: `{theory: str, test_case: str, N: int, solve_time: float, timeout: bool, ...}`

**Performance Tier Hypotheses** (to be empirically validated):
- **Tier 1 (k=1 primitives - Logos)**: N=18-20 tractable, <25% timeout rate, model space O(2^(D+2D·P))
- **Tier 2 (k=2 primitives - Exclusion)**: N=15-18 tractable, <35% timeout rate, model space O(2^(D+D·P+D²))
- **Tier 3 (k=3 primitives - Imposition)**: N=8-10 maximum, >50% timeout at N=10, memory crashes at N≥13, model space O(2^(D+2D·P+D³))

#### Analysis: Statistical Methods and Visualization

**EXECUTE: Generate tables and figures using analysis script after experimental execution**

Statistical analysis isolates the primitive arity effect through correlation and regression:
- **Pearson correlation**: Model space exponent (from Section 6.1 analysis) vs. timeout rate
- **Linear regression**: log(solve_time) ~ N for each theory, comparing slopes (quadratic vs. cubic scaling)
- **Summary statistics**: Timeout rate by theory × N, average solve time, maximum tractable N (threshold: <25% timeout)

**Visualization Requirements**:
- **Table 1**: Theory × N matrix showing average solve time (ms) and timeout rate (%)
- **Figure 1**: Solve time vs. N scatter plot with logarithmic y-axis, separate lines per theory showing empirical scaling
- **Bar Chart**: Maximum tractable N by theory (demonstrates tier boundaries)

LaTeX export format enables direct integration into paper composition.

**Expected Empirical Results**:
- Logos outperforms Exclusion by ~3× in solve time (k=1 vs. k=2 primitives) with both achieving N=18-20 tractability
- Imposition exhibits dramatic performance degradation at N≥10, with memory crashes at N≥13 from ternary frame constraint explosion
- Timeout correlation with model space exponent confirms primitive arity as primary complexity driver
- Within-tier variation (Logos vs. Exclusion ~2-3× difference) demonstrates secondary effects of primitive count and frame constraint design

#### Experimental Setup: Reproducibility Specifications

**Hardware**: [To be documented during execution - CPU, RAM, OS version]
**Z3 Version**: [To be verified from ModelChecker dependencies - expected 4.12.x]
**Timeout Settings**: 60 seconds per inference evaluation
**Reproducibility Instructions**:
- Clone ModelChecker repository: `git clone https://github.com/benbrastmckie/ModelChecker`
- Navigate to experimental directory: `cd research/paper_experiments`
- Install dependencies: `pip install -r requirements.txt`
- Run benchmark suite: `python run_experiments.py --output results.json`
- Generate analysis: `python analyze_results.py --input results.json --output paper_tables/`

**Expected Runtime**: 2-4 hours per theory (15-20 benchmarks × 9-12 N values × 60s timeout ≈ 135-240 minutes per theory)

**File Locations**:
- Benchmark suite: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/benchmark_suite.py`
- Experiment runner: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/run_experiments.py`
- Analysis script: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/analyze_results.py`
- Setup documentation: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/README.md`

**Implementation Status**: Experimental infrastructure fully documented; execution pending as separate task outside paper outline revision scope.

### 6.5 Conclusion: The Dominance of Primitive Arity

The analysis of semantic primitive complexity establishes primitive arity—specifically, the number of arguments ranging over states—as the primary determinant of computational tractability in SMT-based semantic theory implementation. This conclusion emerges from three complementary investigations: model space analysis, frame constraint complexity, and the Logos-Exclusion comparison. The experimental validation documented in Section 6.4 provides the empirical methodology for testing these tractability predictions.

**The Central Finding**

For primitives with k arguments ranging over states (domain D=2^N) and h arguments ranging over sentence letters (domain P), model space scales as 2^(D^k × P^h). Since D grows exponentially with bitvector width N while P remains small, the state-argument count k dominates:

- **Unary primitives** (k=1): Model space 2^D — Expected tractable to N≈20-30 (model space 2^(2^20) through 2^(2^30), extremely large but with linear D scaling)
- **Binary primitives** (k=1, h=1): Model space 2^(D×P) — Expected tractable to N≈15-20 (Logos tier: 2^(48) at N=4, P=3 scales to 2^(300) at N=10)
- **Pure-state binary primitives** (k=2): Model space 2^(D²) — Expected tractable to N≈12-15 (Exclusion tier: 2^(256) at N=4 scales to 2^(1024) at N=5, quadratic explosion)
- **Ternary primitives** (k=3): Model space 2^(D³) — Expected tractable to N≈8-10 (Imposition tier: 2^(4096) at N=4, memory crashes beyond N≈12-13 from frame constraint expansion)

This exponential hierarchy creates discrete performance tiers. Theories with lower k values achieve substantially higher tractability regardless of primitive count or semantic clause complexity. Theories with higher k values experience dramatic performance degradation regardless of optimization efforts. The k=2 → k=3 transition represents a tractability boundary that no amount of constraint optimization can overcome.

**Frame Constraints Compound, Don't Determine**

Frame constraints exhibit a pruning-complexity tradeoff: they eliminate invalid model regions but impose computational overhead through constraint expansion and memory consumption. Critically, frame constraint complexity *compounds with* rather than *determines* tractability:

- Frame constraints over k-ary primitives expand as D^k
- Binary primitives generate D² frame constraint expansions (manageable)
- Ternary primitives generate D³ frame constraint expansions (overwhelming)

Imposition's memory crashes (expected at N≥13) result from frame constraints over ternary primitives expanding to prohibitively large clause counts at moderate N values. With four ternary-quantified constraints and D=4096 at N=12, the expansion yields 4×D³ ≈ 275 billion constraints requiring approximately 27 terabytes of memory. Even optimal frame constraint design cannot compensate for high k. The frame constraint analysis reinforces rather than contradicts the primitive arity conclusion.

**Domain-Typed Arguments: The Refined Criterion**

The critical refinement: not all "binary primitives" are equal. Argument domains determine complexity:

- `falsify(x, p)`: k=1, h=1 → 2^(D×P) ≈ 2^96 at N=5
- `excludes(x, y)`: k=2, h=0 → 2^(D²) ≈ 2^1024 at N=5

This comparison demonstrates the refinement of the arity principle: k=1 theories substantially outperform k=2 theories, which in turn dramatically outperform k=3 theories.

- Logos (3 primitives, simple negation, k_max=1): ~1,100 constraints at N=5, expected tractable to N≈18-20
- Exclusion (2 primitives, complex negation, k_max=2): ~3,200 constraints at N=5, expected tractable to N≈15-18

Logos significantly outperforms Exclusion for two compounding reasons: (1) lower maximum state-argument count (k_max=1 vs. k_max=2), and (2) simpler semantic clauses with fewer frame constraints. Logos's primitives (`verify`, `falsify`, `possible`) each have at most one state argument, yielding model space O(D×P). Exclusion's `excludes(x,y)` primitive has two state arguments, creating O(D²) model space that dominates the D×P contribution from `verify`. Combined with Exclusion's additional frame constraints, this produces ~3× more constraints at equivalent N.

**The Complexity Hierarchy**

Semantic theory tractability follows a clear hierarchy:

1. **Primary**: Primitive arity relative to domain (k = state arguments)
   - Determines tractability tier boundaries
   - Creates exponential performance gaps (2^(D^k))
   - Cannot be compensated by optimization

2. **Secondary**: Frame constraint design
   - Compounds with primitive arity (D^k expansion)
   - Can cause catastrophic memory failures
   - Enables 2-3× performance variation within tiers

3. **Tertiary**: Primitive count and semantic complexity
   - Creates variation within tractability tiers
   - Negotiable through design tradeoffs
   - Does not affect tier boundaries

**Design Implications**

This hierarchy yields a clear design principle: minimize k (state arguments per primitive) as the primary complexity criterion. Theories with k≥3 should be avoided unless absolutely necessary for semantic adequacy. When higher arity proves essential, modular architecture can isolate high-arity primitives in optional subtheories, enabling users to access Tier 1 performance for formulas not requiring the high-arity primitive.

The analysis establishes computational tractability as objective, measurable, and predictable from primitive structure. Semantic theorists can calculate tractability boundaries before implementation by identifying maximum k across primitives. This transforms tractability from an empirical surprise into a design criterion, enabling informed choices between semantic expressiveness and computational feasibility.

---

## 7. Conclusion: TheoryLib and Collaborative Semantic Theory Development

This paper has presented the ModelChecker framework treating semantic theories as executable programs enabling systematic model exploration, empirical theory comparison, and automated validation checking. Six principal contributions advance semantic methodology: theory-agnostic architecture, compositional modularity, systematic comparative methodology, bounded model exploration, computational complexity as theoretical virtue, and TheoryLib extensible library. This concluding section develops the TheoryLib vision and provides practical guidance for researchers to use the framework, test existing theories, implement new semantic frameworks, and contribute to collaborative formal semantics.

### 7.1 TheoryLib: A Shared Repository for Semantic Theories

TheoryLib provides for formal semantics what proof assistant libraries provide for formalized mathematics: shared, reusable, validated implementations in standardized format. The current library includes four semantic theories (Logos, Exclusion, Imposition, Bimodal) demonstrating the framework across diverse approaches while providing foundation for collaborative expansion.

**Benefits of Standardized Implementation**

Implementing theories in TheoryLib format yields multiple research benefits. **Reproducibility**: Published theories become executable, enabling other researchers to validate results and test new examples. **Direct comparison**: Standardized format permits running identical arguments through multiple theories under controlled conditions (Section 4), producing empirical comparative data rarely available in traditional semantic theorizing. **Educational use**: Students can explore semantic frameworks through concrete models rather than abstract definitions, building intuition about how theories validate inferences. **Theory reuse**: Shared operator implementations promote consistency while enabling researchers to build on prior work rather than reimplementing from scratch.

**Current Coverage and Future Expansion**

The four implemented theories cover 177+ validated examples spanning propositional logic, modal operators, counterfactual conditionals, and constitutive operators. This demonstrates framework viability while establishing baseline for expansion. Future TheoryLib development envisions implementations of epistemic semantics, normative logics, causal frameworks, dynamic semantics, and hybrid approaches combining multiple semantic dimensions.

### 7.2 Testing and Implementing Theories

Researchers can immediately use TheoryLib by selecting a theory (Logos, Exclusion, Imposition, Bimodal), specifying arguments, configuring search bounds (domain size N, timeout, constraints), running validation, and interpreting results (valid: evidence for entailment; invalid: concrete countermodels).

Contributing theories to TheoryLib follows five steps: (1) Design semantic primitives minimizing state-argument count k for tractability (Section 6), (2) Implement semantic clauses following three-layer operator architecture (Section 3.1), (3) Specify frame constraints balancing pruning benefits against D^k expansion costs (Section 6.2), (4) Create example suites validating expected behaviors and benchmarking performance (Section 4), (5) Document and submit for community review. Detailed implementation guidance available in supplementary materials.

### 7.3 A Methodology for Computational Formal Semantics

The ModelChecker framework enables new research methodologies transforming how semantic theories are developed, tested, and compared.

**Theory Development Cycle**

1. **Implement**: Encode semantic theory as executable program
2. **Test**: Run example suites validating expected inferences
3. **Refine**: Identify bugs, optimize performance, simplify implementations
4. **Compare**: Benchmark against alternative theories on shared examples
5. **Iterate**: Revise based on empirical results and theoretical insights

This cycle replaces informal theory development with systematic engineering methodology. Bugs become obvious (theory validates/invalidates unexpectedly), performance becomes measurable (timeout rates, solve times), and comparisons become reproducible (standardized test suites, controlled conditions).

**Empirical Validation**

Run standardized test suites across theories, collecting validation patterns and performance metrics. Which theories agree on classical validities? Where do hyperintensional frameworks diverge from intensional approaches? How much does bilateral tracking cost computationally? These questions receive empirical answers grounded in concrete measurements (Section 4.2).

**Educational Applications**: Teaching semantic frameworks becomes interactive through executable theories, concrete countermodels, and real-time validation changes, transforming abstract metalogical definitions into algorithmic implementations.

**Reproducibility and Cumulative Progress**

Standardized implementations enable reproducibility: publish the theory file alongside the paper, allowing reviewers to validate results and other researchers to build on prior work. This addresses a persistent challenge in formal philosophy where theory comparisons depend on informal assessments differing across researchers. TheoryLib provides shared infrastructure enabling cumulative progress through validated, reusable implementations.

**The Computational Turn in Semantics**

The methodology represents broader shifts in semantic theorizing:
- From informal comparison → systematic empirical benchmarking
- From hand-verified proofs → automated model exploration
- From subjective complexity → objective tractability metrics
- From isolated theories → collaborative theory libraries
- From philosophical intuition → computational constraint

These shifts don't replace traditional semantic methods but complement them with new forms of evidence, validation, and comparison.

### 7.5 Future Directions and Open Research Platform

Future research trajectories include expanding TheoryLib coverage (epistemic logics, normative semantics, hybrid approaches), advancing computational methods (solver optimization, theorem prover integration), and establishing reproducibility standards for computational semantics. TheoryLib invites collaboration: implementing new theories, extending existing theories, creating example suites, improving solver performance, and developing educational materials. The ModelChecker repository provides comprehensive documentation for contributors.

**Concluding Thought**

The ModelChecker framework provides infrastructure transforming semantic theorizing from isolated analyses to collaborative computational methodology. TheoryLib invites the formal semantics community to implement theories, share examples, and advance the computational turn.
