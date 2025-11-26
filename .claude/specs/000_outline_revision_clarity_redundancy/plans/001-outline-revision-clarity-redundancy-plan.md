# Implementation Plan: Paper Outline Revision for Clarity, Redundancy Reduction, and Narrative Arc Enhancement

## Metadata
- **Date**: 2025-11-26 (Revised: 2025-11-26)
- **Feature**: Systematic revision of paper outline to reduce redundancy, improve clarity and concision, strengthen narrative arc, and document experimental infrastructure requirements
- **Scope**: Complete revision of /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md (813 lines)
- **Estimated Phases**: 9
- **Estimated Hours**: 22-26
- **Standards File**: Journal of Automated Reasoning submission guidelines
- **Status**: [IN PROGRESS]
- **Complexity Score**: 138.5
- **Structure Level**: 0
- **Research Reports**:
  - [Outline Analysis Report](/home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/.claude/specs/000_outline_revision_clarity_redundancy/reports/001_outline_analysis.md)
  - [ModelChecker Empirical Experiments Report](/home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/.claude/specs/000_outline_revision_clarity_redundancy/reports/002_modelchecker_empirical_experiments.md)

## Overview

**SCOPE REVISION**: This plan now focuses on preparing experimental infrastructure documentation rather than completing empirical work directly. Based on the ModelChecker research report, Section 6.4's empirical validation requires using the separate ModelChecker codebase, which is outside this paper project's scope.

This plan addresses systematic revision of the paper outline targeting Journal of Automated Reasoning submission. The research analysis identified 38 instances of redundancy across 10 major concepts, 9 categories of clarity issues, and 7 structural problems affecting narrative arc. The plan will:

1. **Document experimental infrastructure** (NEW FOCUS): Create detailed experimental design, benchmark suite specifications, and data collection procedures based on ModelChecker capabilities
2. **Complete tractability analysis** (Section 6.5): Document expected performance characteristics based on model space calculations
3. **Expand related work** (Section 1.2): Add comparison with existing tools
4. **Add formal rigor**: Definitions and algorithmic specifications required for JAR
5. **Eliminate redundancy**: 18-25% reduction through consolidation
6. **Improve clarity**: Add examples, transitions, focused paragraphs
7. **Strengthen narrative arc**: Rebalance sections, compress peripheral content

The revision transforms a comprehensive but repetitive 813-line outline into a focused, experimentally-grounded, JAR-ready document of approximately 650-700 lines with documented experimental procedures (ready to execute), complete tractability analysis, reduced peripheral content, strengthened narrative flow, and enhanced technical rigor.

## Research Summary

The outline analysis report (001) provides comprehensive analysis establishing:

1. **Redundancy Analysis**: 38 locations across 10 concepts requiring consolidation
   - Theory-agnostic architecture (5 repetitions)
   - Comparative methodology (6 repetitions)
   - Primitive arity thesis (5 repetitions)
   - Model space calculations (2 repetitions)
   - Estimated reduction: 150-200 lines (18-25%)

2. **Clarity Issues**: 9 categories identified
   - Verbose section introductions
   - Unclear technical transitions
   - Overloaded paragraphs
   - Passive constructions
   - Delayed definitions
   - Complex nested clauses

3. **Narrative Arc Problems**: 7 structural issues
   - Front-loading contributions before motivation
   - Section 6 dominates (290 lines, 36% of paper)
   - Section 6.4 entirely TODO (47 lines)
   - Section 7 over-emphasizes future work
   - Missing concrete examples throughout
   - Weak build-up to primitive arity thesis

4. **JAR Requirements**: Critical gaps identified
   - Missing formal definitions section
   - No algorithmic specifications
   - Section 6.4 empirical validation incomplete (CRITICAL)
   - Related work needs expansion
   - 8 sections marked as NOTE (peripheral content)

5. **TODO Items**: 3 critical content gaps
   - Section 1.2: Related work expansion (HIGH priority)
   - Section 6.4: Empirical performance data (CRITICAL)
   - Section 6.5: Tractability numbers (CRITICAL)

The ModelChecker research report (002) provides experimental infrastructure analysis:

1. **Theory Locations**: Semantic primitives implemented in theory_lib/
   - Logos: 3 binary primitives (verify, falsify, possible)
   - Exclusion: Inherits Logos + excludes (binary: state × state)
   - Imposition: Inherits Logos + imposition (ternary: state × world × world)
   - Confirms paper's primitive arity classifications

2. **Domain Size Control**: N parameter controls model space (2^N states)
   - Settings dictionaries specify N, timeout, constraints
   - Existing tests validate N up to 16
   - Recommended test ranges: Logos/Exclusion N={4,5,6,8,10,12,15,18,20}, Imposition N={3,4,5,6,8,10}

3. **Metrics Collection**: Framework automatically captures
   - solve_time (z3_model_runtime)
   - timeout flags (boolean)
   - constraint_count (z3_model_assertions)
   - model_space_size (computed from N)
   - JSON export available

4. **Benchmark Suite Challenge**: No existing unified cross-theory test suite
   - Existing examples theory-specific (30+ per theory)
   - Need to create 15-20 cross-compatible benchmarks
   - Must use only common operators (¬, ∧, ∨, →, □, ◇)
   - Should include validities, modal inferences, countermodels

5. **Experimental Recommendations**: 7 tasks with time estimates
   - Create benchmark suite: 4-6 hours
   - Create experiment runner: 6-8 hours
   - Create data analysis script: 4-6 hours
   - Document experimental setup: 2-3 hours
   - Verify primitive arity: 1 hour
   - Memory profiling (optional): 3-4 hours
   - Statistical analysis prep: 2-3 hours

## Success Criteria

**PRIMARY CRITERIA (Experimental Infrastructure)**:
- [ ] Section 6.4 contains detailed experimental design specification (benchmark suite, N ranges, metrics)
- [ ] Benchmark suite design documented: 15-20 test cases with selection criteria
- [ ] Experiment runner specifications documented: theory iteration, N parameterization, metric collection
- [ ] Data analysis procedures documented: statistical methods, visualization requirements
- [ ] Experimental setup documented: hardware, Z3 version, timeout settings, reproducibility instructions
- [ ] Section 6.4 ready for execution (infrastructure documented, not completed)

**SECONDARY CRITERIA (Outline Completion)**:
- [ ] All 38 redundancy instances eliminated through consolidation
- [ ] Outline reduced from 813 to 650-700 lines (18-25% reduction)
- [ ] Section 6.5 TODO markers replaced with tractability analysis based on model space calculations
- [ ] Section 1.2 expanded with related work comparison
- [ ] Formal definitions section added (Section 2.4)
- [ ] 2-3 algorithmic specifications added
- [ ] 5-7 concrete worked examples distributed across sections
- [ ] 8 NOTE sections compressed or relocated
- [ ] Clarity improved: verbose introductions eliminated, transitions added, paragraphs focused
- [ ] Narrative arc strengthened: contributions restructured, conclusion rebalanced
- [ ] All TODO markers removed or marked as "EXECUTE: [experimental task]"
- [ ] Outline ready for JAR paper composition (pending experimental data collection)

## Technical Design

### Architecture Overview

**REVISED STRATEGY**: The plan now prioritizes documenting experimental infrastructure over executing experiments, since empirical work requires the separate ModelChecker codebase. The revision follows a phased approach:

1. **Experimental Infrastructure Documentation** (Phases 1-4): Document benchmark suite design, experiment runner specifications, data analysis procedures, and setup requirements based on ModelChecker research
2. **Tractability Analysis** (Phase 2): Complete Section 6.5 with model space calculations (not empirical results)
3. **Formal Rigor** (Phase 3): Add definitions and algorithms required for JAR
4. **Content Optimization** (Phases 5-7): Eliminate redundancy, improve clarity, add examples
5. **Structural Refinement** (Phase 8): Compress peripheral content, rebalance sections
6. **Final Polish** (Phase 9): Narrative arc optimization and verification

This prioritization ensures the paper is ready for experimental execution as a separate task while completing all outline work that can proceed independently.

### Revision Strategy

1. **Experimental Design First** (Phases 1-4): Document what needs to be done to run experiments using ModelChecker infrastructure
2. **Formal Additions** (Phase 3): Add definitions and algorithms for JAR compliance
3. **Content Consolidation** (Phases 5-6): Eliminate redundancy and improve clarity
4. **Structural Refinement** (Phases 7-8): Compress peripheral content, rebalance sections
5. **Final Polish** (Phase 9): Narrative arc optimization and comprehensive verification

### File Structure

All changes target: `/home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md`

Backup created at: `/home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline_backup_2025-11-26.md`

### Key Principles

- **Preserve core contributions**: No substantive content loss, only consolidation
- **Maintain section structure**: Keep 7-section organization aligned with JAR expectations
- **Add before subtracting**: Complete TODOs before removing peripheral content
- **Evidence-based decisions**: All changes grounded in research report findings

## Implementation Phases

### Phase 1: Design Benchmark Suite Specification (Section 6.4) [COMPLETE]
dependencies: []

**Objective**: Document the design of a 15-20 test case benchmark suite for cross-theory comparison based on ModelChecker infrastructure analysis

**Complexity**: Medium

**Tasks**:
- [x] Document benchmark suite requirements: 15-20 representative inferences cross-compatible across Logos, Exclusion, Imposition
- [x] Specify operator restrictions: Use only common operators (¬, ∧, ∨, →, □, ◇) present in all three theories
- [x] Define test case categories with counts:
  - 5-7 propositional validities (tautologies, basic logical laws)
  - 3-4 modal inferences (S5 axioms, modal distribution)
  - 5-7 invalid arguments (countermodel examples)
  - 2-3 simple counterfactuals (if cross-compatible)
- [x] Document selection criteria: vary formula complexity (atomic → conjunctive → nested), avoid theory-specific features
- [x] Specify test case structure format: premises list, conclusions list, settings template (without N), expectation (valid/countermodel)
- [x] Document file location: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/benchmark_suite.py`
- [x] List example sources: Draw from existing theory examples (logos/examples.py, exclusion/examples.py, imposition/examples.py)
- [x] Add to Section 6.4: "Experimental Design" subsection before results (10-15 lines describing benchmark suite structure)

**Testing**:
```bash
# Verify Section 6.4 has Experimental Design subsection
grep -q "Experimental Design" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
# Verify benchmark suite specification documented
grep -c "benchmark" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
```

**Expected Duration**: 4-6 hours (documentation, not implementation)

### Phase 2: Document Experiment Runner & Complete Section 6.5, 1.2 [COMPLETE]
dependencies: [1]

**Objective**: Document experiment runner specifications, complete Section 6.5 tractability analysis (based on model space calculations, not empirical data), and expand Section 1.2 related work

**Complexity**: Medium

**Tasks**:
- [x] Document experiment runner specifications (Section 6.4 "Methodology" subsection, 10-15 lines):
  - Theory iteration: Run same benchmarks across Logos, Exclusion, Imposition
  - N parameterization: Logos/Exclusion N={4,5,6,8,10,12,15,18,20}, Imposition N={3,4,5,6,8,10}
  - Metrics collection: solve_time (z3_model_runtime), timeout flags, constraint_count, model_space_size
  - JSON export format with example structure
  - File location: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/run_experiments.py`
- [x] Complete Section 6.5 tractability analysis based on model space calculations:
  - Replace TODO line 635: "Unary primitives (k=1): Model space 2^D — Expected tractable to N≈20-30"
  - Replace TODO line 636: "Binary primitives (k=1, h=1): Model space 2^(D×P) — Expected tractable to N≈15-20 (Logos/Exclusion tier)"
  - Replace TODO line 637: "Pure-state binary primitives (k=2): Model space 2^(D²) — Expected tractable to N≈12-15"
  - Replace TODO line 638: "Ternary primitives (k=3): Model space 2^(D³) — Expected tractable to N≈8-10 (Imposition tier)"
  - Replace TODO line 650: "Memory usage expected to scale cubically for Imposition"
  - Replace TODO lines 661-662: "Empirical validation pending (see Experimental Design subsection)"
- [x] Expand Section 1.2 related work (lines 43-59): research 3-5 specific projects (Isabelle/HOL, Prover9/Mace4, NuSMV, Nitpick)
- [x] Add comparison table: features (theory-agnostic? compositional? SMT-based? comparative? domain sizes?)
- [x] Add qualitative comparison: "Mace4 typically handles N≈8-10; our approach targets N≈15-20 for binary primitives"

**Testing**:
```bash
# Verify TODO markers resolved (except "EXECUTE:" markers)
grep -n "TODO" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md | grep -v "EXECUTE:"
# Check Section 1.2 expansion (should be 40-60 lines)
sed -n '/^### 1.2/,/^### 1.3/p' /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md | wc -l
# Verify experiment runner documented
grep -q "run_experiments.py" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
```

**Expected Duration**: 5-7 hours

### Phase 3: Document Data Analysis & Experimental Setup [COMPLETE]
dependencies: [2]

**Objective**: Document data analysis procedures and experimental setup requirements for reproducibility

**Complexity**: Medium

**Tasks**:
- [x] Document data analysis specifications (Section 6.4 "Analysis" subsection, 10-15 lines):
  - Statistical methods: Pearson correlation (model space exponent vs timeout rate), linear regression (log(solve_time) ~ N)
  - Summary statistics: timeout rate by theory × N, average solve time, max tractable N (threshold: <25% timeout)
  - Visualization requirements: Table 1 (timeout rates), Figure 1 (solve time vs N log scale), bar chart (max tractable N)
  - LaTeX export format for tables and figure data
  - File location: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/analyze_results.py`
- [x] Document experimental setup (Section 6.4 "Setup" subsection, 8-12 lines):
  - Hardware specifications: [To be filled during execution]
  - Z3 version: [To be filled - check ModelChecker dependencies]
  - Timeout settings: 60 seconds per inference
  - Reproducibility instructions: File paths, command invocations
  - Expected runtime estimates: ~2-4 hours per theory (45 benchmarks × 9-12 N values × 60s timeout)
  - File location: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/README.md`
- [x] Add "EXECUTE:" markers for experimental tasks:
  - Line 578 (Section 6.4 start): "EXECUTE: Run benchmark suite using documented methodology"
  - Line 624 (Section 6.4 end): "EXECUTE: Generate tables and figures using analysis script"

**Testing**:
```bash
# Verify all analysis subsections documented
grep -q "Analysis" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
grep -q "Setup" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
# Verify EXECUTE markers present
grep -c "EXECUTE:" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
# Verify file paths documented
grep -q "analyze_results.py" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
```

**Expected Duration**: 4-6 hours

### Phase 4: Add Formal Rigor - Definitions & Algorithms [COMPLETE]
dependencies: [3]

**Objective**: Add formal definitions section and algorithmic specifications required for JAR

**Complexity**: Medium

**Tasks**:
- [x] Create new Section 2.4: "Formal SMT Encoding" (after Section 2.3, before Section 3)
- [x] Add Definition 1: Semantic Primitive (formal specification with Z3 function declaration)
- [x] Add Definition 2: Operator Interface (methods required: syntactic recognition, semantic interpretation, model presentation)
- [x] Add Definition 3: Frame Constraint (quantified formula over semantic primitives)
- [x] Show concrete Z3 constraint encoding example for one semantic clause
- [x] Add Algorithm 1 (Section 2.2): Main validation pipeline (input: argument + theory → output: valid/countermodel)
- [x] Add Algorithm 2 (Section 5.3): Iterative countermodel discovery with isomorphism detection
- [x] Use LaTeX algorithm environment notation (pseudocode format)

**Testing**:
```bash
# Verify definitions section exists
grep -q "^### 2.4 Formal SMT Encoding" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
# Verify algorithms added
grep -c "^Algorithm [0-9]:" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md
```

**Expected Duration**: 4-6 hours

### Phase 5: Eliminate Redundancy [COMPLETE]
dependencies: [4]

**Objective**: Systematically remove 38 redundancy instances across 10 major concepts (18-25% reduction)

**Complexity**: High

**Tasks**:
- [x] Consolidate theory-agnostic architecture (5 instances → 1): Single comprehensive explanation in Section 2.1, cross-references only elsewhere (lines 66-69, 105-106, 121-122, 168-169, 206-207)
- [x] Consolidate compositional modularity (4 instances → 1): Merge Sections 3 intro and 3.2 discussions (lines 72-76, 172-174, 209-212, 250-252)
- [x] Consolidate comparative methodology (6 instances → 1): Full explanation in Section 4.1 only, brief mentions in 1.3 and 2.1 (lines 78-83, 115-117, 159-162, 255-257, 261-268, 295-300)
- [x] Consolidate bounded model exploration (3 instances → 1): Primary explanation in Section 2.3 or 5.1 (lines 84-86, 141-143, 323-324)
- [x] Consolidate SMT constraint categories (3 instances → 1): State once in Section 2.2, reference elsewhere (lines 132-136, 174-176, Sections 5.1-5.2)
- [x] Consolidate primitive arity thesis (5 instances → 2): Remove from Contribution 6, eliminate Section 4.4 preview, state clearly in Section 6 intro, conclude in 6.5 (lines 93-96, 313-318, 402-407, 456-459, 625-641)
- [x] Consolidate TheoryLib vision (4 instances → 1): Detailed explanation in Section 7.1 only, brief forward references (lines 88-92, 695-699, 697-704, 807-808)
- [x] Remove duplicate model space calculations: Keep in 6.1, reference from 6.3 (lines 432-453, 554-567)
- [x] Consolidate three-layer operator architecture: Single explanation in 3.1, cross-references only (lines 175-180, 192-204, 731-734)
- [x] Consolidate validation vs proof distinction: State once in Section 2.2 or 2.3 (lines 84-86, 151-154, 396-400)

**Testing**:
```bash
# Measure line reduction
ORIGINAL_LINES=813
CURRENT_LINES=$(wc -l < outline/paper_outline.md)
REDUCTION=$((ORIGINAL_LINES - CURRENT_LINES))
PERCENT=$((100 * REDUCTION / ORIGINAL_LINES))
echo "Reduced by $REDUCTION lines ($PERCENT%)"
# Target: 150-200 line reduction (18-25%)
[ $REDUCTION -ge 150 ] || echo "WARNING: Insufficient reduction"
```

**Expected Duration**: 6-8 hours (careful editing required)

### Phase 6: Add Concrete Examples & Improve Clarity [NOT STARTED]
dependencies: [5]

**Objective**: Add 5-7 concrete worked examples and improve transitions, paragraph structure, and language clarity

**Complexity**: Medium

**Tasks**:
- [ ] Add Example 1 (Section 2.2): Complete inference through all 4 pipeline stages (5-10 lines)
- [ ] Add Example 2 (Section 3.1): Concrete operator implementation showing conjunction in Logos vs Exclusion (5-10 lines)
- [ ] Add Example 3 (Section 4.3): Actual validation pattern table for 2-3 theories on same inference (5-10 lines)
- [ ] Add Example 4 (Section 5.3): 2-3 structurally distinct countermodels with descriptions (5-10 lines)
- [ ] Add Example 5 (Section 6.3): Detailed Logos vs Exclusion performance comparison table (5-10 lines)
- [ ] Add 1-2 sentence transitions between major concepts (Stage 1→2, intensional→hyperintensional pattern shift): 6-8 locations
- [ ] Break overloaded paragraphs: Section 2.3 Layer 2 (line 192-199), Section 6.2 frame constraints (461-483), Section 6.3 argument domains (554-569)
- [ ] Convert passive constructions to active voice: 10-15 locations
- [ ] Define technical terms at first use: "contingent" (line 111), "frame constraints" (line 132), "semantic primitives" (line 410)

**Testing**:
```bash
# Count examples added
grep -c "^\*\*Example [0-9]" outline/paper_outline.md
# Verify transitions added (look for "Having established" pattern)
grep -c "Having established" outline/paper_outline.md
```

**Expected Duration**: 5-7 hours

### Phase 7: Compress Peripheral Content (NOTE Sections) [NOT STARTED]
dependencies: [6]

**Objective**: Compress or relocate 8 NOTE sections identified as non-essential (130 line reduction)

**Complexity**: Low

**Tasks**:
- [ ] Compress Section 2.3 output formats: 15 lines → 3 lines (lines 163-166)
- [ ] Compress Section 3.2 dependency management: Keep 1-2 sentences only (lines 219-222)
- [ ] Compress Section 5.1 configuration hierarchy: 25 lines → 5 lines (lines 326-340)
- [ ] Compress Section 5.4 isomorphism optimization: Brief mention only (lines 369-375)
- [ ] Move Section 7.3 implementation guide to supplementary note: Add marker "See supplementary guide for detailed implementation steps" (lines 722-752)
- [ ] Compress Section 7.4 educational applications: 15 lines → 3 lines (lines 771-774)
- [ ] Compress Section 7.5 future work: 60 lines → 20 lines, keep 2-3 key directions only (lines 791-808)
- [ ] Add NOTE markers to compressed sections indicating non-essential status

**Testing**:
```bash
# Verify compression targets met
sed -n '/^### 7.5/,/^### 7.6/p' outline/paper_outline.md | wc -l | grep -q "^2[0-9]$" || echo "Section 7.5 not compressed"
# Check total line count approaching target
CURRENT_LINES=$(wc -l < outline/paper_outline.md)
[ $CURRENT_LINES -le 700 ] || echo "WARNING: Still above 700 lines"
```

**Expected Duration**: 3-4 hours

### Phase 8: Structural Reorganization [NOT STARTED]
dependencies: [7]

**Objective**: Rebalance sections, restructure contributions presentation, improve narrative arc

**Complexity**: Medium

**Tasks**:
- [ ] Restructure Section 1.3 contributions: Keep 2-3 high-level contributions only, move detailed explanations to Section 7
- [ ] Rebalance Section 7: Expand Section 7.1 or 7.4 with concrete methodology examples (from compressed 7.5 space)
- [ ] Consider adding Section 7.6: "Design Principles" or "Lessons Learned" extracting practical insights
- [ ] Evaluate Section 6 length (currently 290 lines): Determine if detailed calculations should move to appendix (optional, only if page limits require)
- [ ] Strengthen build-up to primitive arity thesis: Restructure Section 6 as discovery narrative (observations → pattern → explanation → thesis)
- [ ] Add forward-looking preview sentences at end of Sections 2, 3, 4, 5 linking to next section
- [ ] Verify problem → solution → validation → discussion arc is clear and well-connected

**Testing**:
```bash
# Verify Section 1.3 is concise
sed -n '/^### 1.3/,/^##/p' outline/paper_outline.md | wc -l | grep -q "^[3-5][0-9]$" || echo "Section 1.3 still too long"
# Verify conclusion rebalanced (7.5 no longer dominant)
sed -n '/^### 7.5/,/^##/p' outline/paper_outline.md | wc -l
```

**Expected Duration**: 3-4 hours

### Phase 9: Final Polish & Verification [NOT STARTED]
dependencies: [8]

**Objective**: Comprehensive quality check, consistency verification, final line count confirmation

**Complexity**: Low

**Tasks**:
- [ ] Verify all 38 redundancy instances eliminated (check each line number from research report)
- [ ] Verify experimental infrastructure documented (Phases 1-3 complete):
  - Benchmark suite design (Section 6.4 "Experimental Design")
  - Experiment runner specifications (Section 6.4 "Methodology")
  - Data analysis procedures (Section 6.4 "Analysis")
  - Experimental setup (Section 6.4 "Setup")
  - All file paths and procedures documented
- [ ] Verify Section 6.5 tractability analysis complete (based on model space calculations, not empirical data)
- [ ] Verify Section 1.2 expanded with related work comparison
- [ ] Verify all 8 NOTE sections compressed/relocated
- [ ] Verify 5-7 examples added and distributed appropriately
- [ ] Verify formal definitions section (2.4) and algorithms (2 total) present
- [ ] Check terminology consistency: "semantic primitives" vs "primitives", "model space" vs "search space"
- [ ] Verify cross-references updated after restructuring
- [ ] Confirm line count: 650-700 lines (18-25% reduction from 813)
- [ ] Verify EXECUTE markers present for experimental tasks (not TODO markers)
- [ ] Check section balance: No section >40% of total (Section 6 was 36%)
- [ ] Proofread for clarity: verbose introductions eliminated, transitions smooth, paragraphs focused
- [ ] Verify JAR readiness: formal rigor, experimental design documented, related work, algorithmic specs all present

**Testing**:
```bash
# Comprehensive verification suite
echo "=== EXECUTE Marker Check (should have markers, not TODOs) ==="
grep -n "EXECUTE:" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md || echo "WARNING: No EXECUTE markers"
grep -n "TODO" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md | grep -v "EXECUTE:" && echo "WARNING: Unresolved TODOs" || echo "All TODOs resolved"

echo "=== Experimental Infrastructure Check ==="
grep -c "Experimental Design\|Methodology\|Analysis\|Setup" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md | xargs -I {} echo "Found {} experimental subsections (target: 4)"
grep -c "benchmark_suite.py\|run_experiments.py\|analyze_results.py" /home/benjamin/Documents/Philosophy/Papers/ProgrammaticSemantics/outline/paper_outline.md | xargs -I {} echo "Found {} file path references (target: 3)"

echo "=== Line Count ==="
FINAL_LINES=$(wc -l < outline/paper_outline.md)
REDUCTION=$((813 - FINAL_LINES))
PERCENT=$((100 * REDUCTION / 813))
echo "Final: $FINAL_LINES lines (reduced by $REDUCTION, $PERCENT%)"
[ $FINAL_LINES -ge 650 ] && [ $FINAL_LINES -le 700 ] && echo "✓ Target range achieved"

echo "=== Section Balance ==="
for i in 1 2 3 4 5 6 7; do
  SECTION_LINES=$(sed -n "/^## $i\./,/^## [0-9]\./p" outline/paper_outline.md | wc -l)
  SECTION_PERCENT=$((100 * SECTION_LINES / FINAL_LINES))
  echo "Section $i: $SECTION_LINES lines ($SECTION_PERCENT%)"
  [ $SECTION_PERCENT -le 40 ] || echo "  WARNING: Section $i exceeds 40%"
done

echo "=== Required Elements ==="
grep -q "^### 2.4" outline/paper_outline.md && echo "✓ Formal definitions section" || echo "✗ Missing definitions"
grep -q "^Algorithm [0-9]:" outline/paper_outline.md && echo "✓ Algorithms present" || echo "✗ Missing algorithms"
grep -c "^\*\*Example [0-9]" outline/paper_outline.md | xargs -I {} echo "✓ {} examples added"

echo "=== Redundancy Elimination ==="
# Spot check key redundancy removals
grep -c "theory-agnostic architecture" outline/paper_outline.md | xargs -I {} echo "Theory-agnostic mentions: {} (target: 3-4)"
grep -c "primitive arity" outline/paper_outline.md | xargs -I {} echo "Primitive arity mentions: {} (target: 8-12)"
```

**Expected Duration**: 2-3 hours

## Testing Strategy

### Phase-Level Testing

Each phase includes specific test commands verifying completion criteria. Tests focus on:
- TODO marker elimination
- Line count reduction tracking
- Required element presence (definitions, algorithms, examples)
- Section balance verification
- Redundancy reduction confirmation

### Integration Testing

After Phase 8, comprehensive verification suite ensures:
- All research report findings addressed
- JAR submission requirements met
- Narrative arc coherent
- No regression in content quality

### Validation Approach

Manual review of revised outline against research report checklist:
- [ ] 38 redundancy instances eliminated
- [ ] 9 clarity categories addressed
- [ ] 7 narrative arc issues resolved
- [ ] 3 TODO sections completed
- [ ] 8 NOTE sections compressed
- [ ] JAR requirements satisfied

## Documentation Requirements

### Progress Tracking

Maintain revision log documenting:
- Lines added/removed per phase
- TODO completion dates
- Example locations
- Compression decisions

### Backup Strategy

Create timestamped backup before each major phase:
```bash
cp outline/paper_outline.md "outline/paper_outline_backup_phase_N_$(date +%Y%m%d).md"
```

### Final Deliverable

Revised outline at original location with:
- All TODO sections completed
- 18-25% length reduction
- JAR-ready structure and content
- Accompanying revision summary document

## Dependencies

### External Dependencies
- Research report findings (completed: reports 001, 002)
- Journal of Automated Reasoning submission guidelines
- ModelChecker codebase (separate project for future experimental execution)
- Access to ModelChecker documentation for infrastructure specifications

### Phase Dependencies
- Phase 2 depends on Phase 1 (experiment runner builds on benchmark suite design)
- Phase 3 depends on Phase 2 (analysis procedures document processing of experiment results)
- Phase 4 depends on Phase 3 (formal additions independent, but builds on experimental context)
- Phase 5 can proceed after Phase 4 (redundancy elimination independent of prior work)
- Phase 6 benefits from Phase 5 (examples easier to place after redundancy removed)
- Phase 7-8 depend on Phase 6 (structural changes after content stable)
- Phase 9 requires all prior phases complete

### Resource Requirements
- 22-26 hours total effort across 9 phases (reduced from 24-28 by documenting experiments instead of running them)
- LaTeX knowledge for algorithm/definition formatting
- Access to ModelChecker research report (002) for infrastructure details
- No computational resources needed (experiments documented, not executed)

## Notes

### Critical Path

**REVISED**: Phases 1-3 (experimental infrastructure documentation) are on the critical path as they prepare the paper for future empirical work. Unlike the original plan which required 2-3 days of testing, these phases now focus on documenting procedures based on ModelChecker research (13-19 hours total). The actual experimental execution becomes a separate task outside this plan's scope.

### Flexibility Points

- Phase 8, Task 4 (Section 6 compression) is optional and should only be pursued if page limits require it
- Example count (5-7) can be adjusted based on available space after redundancy elimination
- Section 7.6 addition is optional enhancement, not required for JAR submission
- Data analysis script specifications (Phase 3) can be simplified if statistical analysis requirements are reduced

### Quality Guardrails

- Preserve all substantive contributions (no content loss)
- Maintain technical accuracy (no oversimplification during compression)
- Keep cross-references valid (update after restructuring)
- Test readability after each major edit (read aloud or peer review)
- Ensure experimental procedures are executable (clear file paths, parameter specifications, reproducible instructions)

### Success Indicators

The revision succeeds if:
1. Experimental infrastructure fully documented (Phases 1-3: benchmark suite, experiment runner, analysis procedures, setup)
2. Section 6.5 tractability analysis complete (based on model space calculations)
3. Section 1.2 related work expanded
4. All TODO markers replaced with EXECUTE markers for experimental tasks
5. Length reduced to 650-700 lines (18-25% reduction achieved)
6. Research report's completion criteria met (adapted for documentation focus)
7. Outline ready for JAR paper composition (pending separate experimental execution)
8. Technical rigor increased (formal definitions, algorithms, experimental design present)
9. Narrative arc strengthened (coherent problem → solution → validation design → discussion flow)

### Estimated Timeline

**REVISED TIMELINE**:
- **Week 1**: Phases 1-3 (experimental infrastructure documentation: 13-19 hours)
- **Week 2**: Phases 4-6 (formal rigor + redundancy + clarity: 15-21 hours)
- **Week 3**: Phases 7-9 (compression + structure + polish: 9-13 hours)
- **Total**: 3 weeks, 37-53 hours (includes testing/debugging time)

**Key Change**: Reduced from "3-4 weeks of focused work" (with 2-3 days experimental execution) to "2-3 weeks of focused documentation work" (experimental execution moved to separate task). Total time slightly increased due to additional documentation phases but overall timeline compressed by removing experimental execution dependency.

Realistic completion estimate: **2-3 weeks of focused work** (was 3-4 weeks).
