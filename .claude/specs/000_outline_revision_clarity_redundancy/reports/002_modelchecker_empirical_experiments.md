# ModelChecker Empirical Experiments Research Report

## Metadata
- **Date**: 2025-11-26
- **Agent**: research-specialist
- **Topic**: Understanding ModelChecker framework for conducting empirical experiments (Section 6.4)
- **Report Type**: codebase analysis
- **Workflow Type**: research-and-revise
- **Research Complexity**: 2

## Executive Summary

The ModelChecker framework provides the necessary infrastructure to conduct the empirical experiments required for Section 6.4 of the paper. The three target theories (Logos, Exclusion, Imposition) are implemented with semantic primitives located in modular semantic packages, domain size is controlled via the `N` parameter in settings dictionaries, and the framework automatically collects solve time, timeout, and constraint count metrics. Experiments can be run programmatically via example files and results output to JSON format for analysis. The main challenge is that no existing benchmark suite exists, requiring creation of 15-20 representative test cases spanning validities, modal inferences, and invalid arguments.

## Findings

### 1. Theory Library Structure and Semantic Primitives

**Location**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/`

The theory library contains four semantic theories in separate subdirectories:
- `logos/` - Hyperintensional bilateral semantics (19+ operators across 5 subtheories)
- `exclusion/` - Unilateral exclusion semantics (witness-based negation)
- `imposition/` - Fine's counterfactual semantics (imposition-based counterfactuals)
- `bimodal/` - Temporal-modal logic (not needed for paper experiments)

#### Logos Theory Primitives

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/logos/semantic.py:70-87`

Logos defines **three binary primitives** as Z3 functions:
```python
self.verify = z3.Function(
    "verify",                   # Names the function 'verify'
    z3.BitVecSort(self.N),      # which maps a bitvector (state)
    syntactic.AtomSort,         # and a sentence letter
    z3.BoolSort()               # to a truth-value
)
self.falsify = z3.Function(
    "falsify",                  # Names the function 'falsify'
    z3.BitVecSort(self.N),      # which maps a bitvector (state)
    syntactic.AtomSort,         # and a sentence letter
    z3.BoolSort()               # to a truth-value
)
self.possible = z3.Function(
    "possible",                 # Names the function 'possible'
    z3.BitVecSort(self.N),      # which maps a bitvector (state)
    z3.BoolSort()               # to a truth-value
)
```

**Primitive Arity**: All three primitives have arity k=1 or k=1 with h=1 (binary: state × proposition or state alone).

#### Exclusion Theory Primitives

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/exclusion/semantic/core.py:116-128`

Exclusion inherits Logos primitives and adds **one binary primitive**:
```python
self.verify = z3.Function(
    "verify",
    z3.BitVecSort(self.N),
    syntactic.AtomSort,
    z3.BoolSort(),
)

self.excludes = z3.Function(
    "excludes",
    z3.BitVecSort(self.N),      # first state
    z3.BitVecSort(self.N),      # second state
    z3.BoolSort(),
)
```

**Primitive Arity**: Binary primitives (k=1 for verify, k=2 for excludes which is state × state).

#### Imposition Theory Primitives

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/imposition/semantic/core.py:129-135`

Imposition inherits Logos primitives and adds **one ternary primitive**:
```python
self.imposition = z3.Function(
    "imposition",
    z3.BitVecSort(self.N),  # state imposed
    z3.BitVecSort(self.N),  # world being imposed on
    z3.BitVecSort(self.N),  # outcome world
    z3.BoolSort()           # truth-value
)
```

**Primitive Arity**: Ternary primitive (k=3: state × world × world).

**Key Finding**: The three theories implement exactly the primitive arity tiers described in the paper:
- **Tier 1 (Binary)**: Logos (3 binary primitives), Exclusion (inherits Logos + 1 binary)
- **Tier 2-3 (Ternary)**: Imposition (inherits Logos + 1 ternary primitive)

### 2. Running Experiments and Setting Domain Size

**Domain Size Control**: The `N` parameter in settings dictionaries controls the size of the model space (number of bits in bitvectors representing states).

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/semantic.py:71`

Example settings structure for experiments:

```python
settings = {
    'N': 4,                  # Domain size (controls model space 2^N)
    'contingent': True,      # Require propositions neither necessary nor impossible
    'non_empty': True,       # Require non-empty subject matter
    'non_null': True,        # Require non-empty verifier sets
    'disjoint': False,       # Propositions have disjoint subject matters
    'max_time': 60,          # Timeout in seconds
    'iterate': 1,            # Number of distinct models to find
    'expectation': True,     # Expected result (True = valid, False = countermodel exists)
}
```

**Running Examples**: Experiments run via example files structured as:

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/exclusion/examples.py:82-100`

```python
EX_CM_1_premises = []
EX_CM_1_conclusions = []
EX_CM_1_settings = {
    'N': 2,
    'max_time': 2,
    'iterate': 1,
    'expectation': True,
}
EX_CM_1_example = [
    EX_CM_1_premises,
    EX_CM_1_conclusions,
    EX_CM_1_settings,
]
```

**CLI Invocation**:
```bash
# Run examples file with specific theory
cd /home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code
python dev_cli.py -l logos path/to/examples.py

# Compare multiple theories
python dev_cli.py --maximize path/to/examples.py

# Save results to JSON
python dev_cli.py --save json path/to/examples.py
```

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/__main__.py:118-122`

The `--maximize` flag enables theory comparison, running the same examples across multiple theories.

### 3. Collecting Performance Metrics

The framework automatically collects performance metrics during model solving:

#### Available Metrics

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/builder/runner_utils.py:45-82`

```python
def calculate_model_statistics(model_structure) -> Dict[str, Any]:
    stats = {
        'domain_size': 0,
        'relation_count': 0,
        'valuation_count': 0,
        'constraint_count': 0,
        'solve_time': 0.0
    }

    # Extract domain size
    if hasattr(model_structure, 'domain'):
        stats['domain_size'] = len(model_structure.domain)

    # Extract constraint count
    if hasattr(model_structure, 'z3_model_assertions'):
        stats['constraint_count'] = model_structure.z3_model_assertions

    # Extract solve time
    if hasattr(model_structure, 'z3_model_runtime'):
        stats['solve_time'] = model_structure.z3_model_runtime

    return stats
```

#### Timeout Detection

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/structure.py:66-92`

```python
self.max_time = self.settings.get("max_time", 5)  # Default 5 seconds timeout
self.timeout = False
self.z3_model_runtime = None

solver_results = self.solve(self.model_constraints, self.max_time)
```

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/structure.py:243-251`

Timeout is set in Z3 solver and detected:
```python
# Set timeout and solve
self.solver.set("timeout", int(max_time * 1000))  # Convert to milliseconds
check_result = self.solver.check()

if self.solver.reason_unknown() == "timeout":
    # Handle timeout case
```

#### JSON Output Format

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/output/formatters/json.py:25-44`

The JSON formatter preserves all model data including:
- Premises and conclusions
- Settings (including N and max_time)
- Model structure data
- Runtime statistics
- Timeout status

Example output structure:
```json
{
  "example_name": "EX_CM_1",
  "premises": [],
  "conclusions": [],
  "settings": {
    "N": 4,
    "max_time": 60,
    "contingent": true
  },
  "timeout": false,
  "solve_time": 0.452,
  "constraint_count": 156,
  "satisfiable": true
}
```

**Key Finding**: All metrics needed for Section 6.4 are automatically collected:
- ✓ Solve times (z3_model_runtime)
- ✓ Timeout rates (timeout flag)
- ✓ Constraint counts (z3_model_assertions)
- ✓ Model space sizes (computed from N: 2^N)
- ✗ Memory usage (not directly exposed but can be measured externally)

### 4. Test Suite Design and Existing Examples

**Challenge**: No existing unified benchmark suite for cross-theory comparison.

#### Existing Examples by Theory

**Logos Theory**:
- **File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/logos/examples.py:1-100`
- Aggregates examples from 5 subtheories (extensional, modal, constitutive, counterfactual, relevance)
- Example counts per subtheory stored in `example_range` dictionaries
- Naming convention: `EXT_*`, `MOD_*`, `CON_*`, `CF_*`, `REL_*`

**Exclusion Theory**:
- **File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/exclusion/examples.py:77-100`
- Categories: Countermodels (`EX_CM_*`), Theorems (`EX_TH_*`)
- Focuses on negation semantics, DeMorgan's laws, exclusion inferences
- ~30 examples total (exact count in `example_range` dict)

**Imposition Theory**:
- **File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/imposition/examples.py:61-100`
- Categories: Countermodels (`IM_CM_*`), Theorems (`IM_TH_*`)
- Focuses on counterfactual reasoning, antecedent strengthening, transitivity
- ~30 examples total

#### Representative Inference Types Present

Based on example file inspection:
1. **Propositional validities**: Tautologies, basic logical laws (in extensional subtheory)
2. **Modal inferences**: Necessity/possibility operators (in modal subtheory)
3. **Invalid arguments**: Countermodel examples showing failures (all `*_CM_*` examples)
4. **Counterfactual reasoning**: Would-conditionals and could-conditionals (in counterfactual/imposition)
5. **Complex nested formulas**: Multiple operators, nested negations

#### Creating the Benchmark Suite

**Recommended approach** for Section 6.4:

1. **Select 5 examples from each theory's existing tests** (15 total):
   - 3 validities (theorems)
   - 2 invalidities (countermodels)

2. **Ensure cross-theory compatibility**:
   - Use common operators present in all three theories
   - Avoid theory-specific operators (e.g., essence, grounding)
   - Focus on: ¬, ∧, ∨, →, □, ◇

3. **Create new `benchmark_suite.py`** file with structure:
```python
# Benchmark 1: Simple tautology
BM_01_premises = []
BM_01_conclusions = ['(A \\vee \\neg A)']
BM_01_settings_template = {
    'contingent': True,
    'non_null': True,
    'non_empty': True,
    'disjoint': False,
    'max_time': 60,
    'iterate': 1,
    'expectation': True,
}

# Run with varying N
for N in [4, 5, 6, 8, 10, 12, 15, 18, 20]:
    settings = BM_01_settings_template.copy()
    settings['N'] = N
    # Run test...
```

**File Location**: Create at `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/benchmark_suite.py`

### 5. Domain Size Parameterization and Model Space Control

**Model Space Calculation**:

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/semantic.py:71-73`

The `N` parameter controls bitvector size:
```python
self.N = combined_settings['N']
# Creates bitvectors of size N, yielding 2^N possible states
```

**Relationship to Paper's Model Space Analysis**:

From paper outline Section 6.1:
- **Logos/Exclusion**: Model space O(2^(D² + D·P)) where D = domain size, P = propositions
- **Imposition**: Model space O(2^(D³))

The framework's `N` parameter corresponds to a combined domain/proposition encoding. For experiments:
- `N=4` → 16 states → explores space of size 2^4
- `N=10` → 1024 states → explores space of size 2^10
- `N=20` → ~1 million states → explores space of size 2^20

**Performance Testing Examples**:

**File**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/tests/integration/test_performance.py:67-87`

Existing performance tests show scaling behavior:
```python
@pytest.mark.parametrize("n,max_time", [
    (2, 0.5),
    (4, 1.0),
    (8, 3.0),
    (16, 10.0),
])
def test_scaling_with_n(self, n, max_time):
    """Test performance scales reasonably with N."""
    settings = {'N': n}
    model = self.create_model(settings)
    elapsed = time.time() - start
    assert elapsed < max_time
```

**Key Finding**: The framework is already tested for N values up to 16, with expected solve times increasing roughly exponentially. For paper experiments:
- **Logos/Exclusion**: Test N = {4, 5, 6, 8, 10, 12, 15, 18, 20}
- **Imposition**: Test N = {3, 4, 5, 6, 8, 10} (expect failures beyond N=10-12)

## Recommendations

### 1. Create Unified Benchmark Suite (CRITICAL - 4-6 hours)

**Action**: Create `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/benchmark_suite.py`

**Structure**:
```python
# 15-20 representative inferences
# Each with:
# - Premises list
# - Conclusions list
# - Settings template (without N)
# Include:
# - 5-7 propositional validities (tautologies, basic laws)
# - 3-4 modal inferences (S5 axioms, modal distribution)
# - 5-7 invalid arguments (countermodel tests)
# - 2-3 simple counterfactuals (if compatible across theories)
```

**Selection criteria**:
- Use only operators present in all three theories
- Vary formula complexity (atomic → conjunctive → nested)
- Cover different inference types (validity, invalidity, modal)
- Avoid theory-specific features

### 2. Create Automated Experiment Runner (HIGH - 6-8 hours)

**Action**: Create `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/run_experiments.py`

**Functionality**:
- Load benchmark suite
- For each theory in [logos, exclusion, imposition]:
  - For each N value in appropriate range:
    - For each benchmark:
      - Run example with timeout
      - Collect metrics (solve_time, timeout, constraint_count)
      - Save to JSON
- Generate summary CSV with columns: theory, N, benchmark_id, solve_time, timeout, constraint_count, model_space_size

**Pseudocode**:
```python
import json
import time
from model_checker.theory_lib import logos, exclusion, imposition

theories = {
    'logos': (logos.LogosSemantics, [4,5,6,8,10,12,15,18,20]),
    'exclusion': (exclusion.WitnessSemantics, [4,5,6,8,10,12,15,18,20]),
    'imposition': (imposition.ImpositionSemantics, [3,4,5,6,8,10,12]),
}

results = []
for theory_name, (SemanticClass, N_values) in theories.items():
    for N in N_values:
        for benchmark_id, benchmark in enumerate(benchmark_suite):
            settings = benchmark['settings'].copy()
            settings['N'] = N
            settings['max_time'] = 60

            # Run experiment
            start = time.time()
            try:
                semantics = SemanticClass(settings)
                model = build_model(semantics, benchmark)

                results.append({
                    'theory': theory_name,
                    'N': N,
                    'benchmark': benchmark_id,
                    'solve_time': model.z3_model_runtime,
                    'timeout': model.timeout,
                    'constraint_count': model.z3_model_assertions,
                    'model_space_size': 2**N,
                    'satisfiable': model.satisfiable,
                })
            except Exception as e:
                results.append({
                    'theory': theory_name,
                    'N': N,
                    'benchmark': benchmark_id,
                    'error': str(e),
                })

# Save results
with open('experiment_results.json', 'w') as f:
    json.dump(results, f, indent=2)
```

### 3. Create Data Analysis Script (MEDIUM - 4-6 hours)

**Action**: Create `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/analyze_results.py`

**Functionality**:
- Load experiment results JSON
- Calculate summary statistics:
  - Timeout rate by theory × N
  - Average solve time by theory × N
  - Maximum tractable N per theory (threshold: <25% timeout rate)
  - Constraint count scaling
- Generate visualizations:
  - Table 1: Timeout rates (theory × N)
  - Figure 1: Solve time vs N (log scale, separate lines per theory)
  - Bar chart: Max tractable N by theory
- Export LaTeX tables and figure data

**Output**: Ready-to-insert content for Section 6.4 of paper.

### 4. Document Experimental Setup (MEDIUM - 2-3 hours)

**Action**: Create `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/research/paper_experiments/README.md`

**Content**:
- Hardware specifications
- Z3 version
- ModelChecker version
- Timeout settings (60s)
- Benchmark suite description
- Instructions to reproduce experiments
- Expected runtime estimates

### 5. Verify Primitive Arity Classifications (LOW - 1 hour)

**Action**: Double-check semantic primitive definitions match paper's classifications.

**Verification checklist**:
- ✓ Logos: verify (state × prop), falsify (state × prop), possible (state) → Binary
- ✓ Exclusion: + excludes (state × state) → Binary
- ✓ Imposition: + imposition (state × world × world) → Ternary

**Confirm**: All classifications in paper match implementation.

### 6. Memory Usage Measurement (OPTIONAL - 3-4 hours)

**Action**: Add memory profiling to experiment runner using `tracemalloc` module.

**File reference**: `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/tests/integration/test_performance.py:90-150`

**Implementation**:
```python
import tracemalloc

tracemalloc.start()
# Run experiment
current, peak = tracemalloc.get_traced_memory()
tracemalloc.stop()

peak_mb = peak / (1024 * 1024)
# Record peak memory usage
```

**Note**: Paper outline mentions memory usage but doesn't emphasize it. Only implement if time permits.

### 7. Statistical Analysis Preparation (LOW - 2-3 hours)

**Action**: Prepare statistical tests for correlation between model space exponent and timeout rate.

**Metrics to compute**:
- Pearson correlation coefficient: model_space_exponent vs timeout_rate
- Linear regression: log(solve_time) ~ N
- Scaling factor: solve_time(N+1) / solve_time(N)
- Threshold analysis: minimum N where timeout_rate > 50%

**Python libraries**: scipy.stats, numpy, pandas

## References

### Theory Library Files
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/README.md:1-100` - Theory library overview
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/logos/semantic.py:70-87` - Logos primitives
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/exclusion/semantic/core.py:116-128` - Exclusion primitives
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/imposition/semantic/core.py:129-135` - Imposition primitives

### Example Files
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/logos/examples.py:1-100` - Logos examples
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/exclusion/examples.py:77-100` - Exclusion examples
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/theory_lib/imposition/examples.py:61-100` - Imposition examples

### CLI and Configuration
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/__main__.py:118-122` - CLI maximize flag
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/semantic.py:71` - N parameter definition
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/structure.py:66-92` - Timeout implementation
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/models/structure.py:243-251` - Z3 timeout detection

### Metrics Collection
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/builder/runner_utils.py:45-82` - Statistics calculation
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/src/model_checker/output/formatters/json.py:25-44` - JSON output format

### Performance Testing
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/tests/integration/test_performance.py:67-87` - N scaling tests
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/tests/integration/test_performance.py:90-150` - Memory profiling examples

### Documentation
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/README.md:1-322` - Main project README
- `/home/benjamin/Documents/Philosophy/Projects/ModelChecker/Code/README.md` - Package documentation
