# README_NAREK

## Project Summary

In this project, we integrated and tested a G2Retro-like model within the `syntheseus` framework. We have made several code changes to integrate `G2RetroModel` as a backward reaction model and conducted benchmarks to measure its performance under various scenarios.

## What We Did So Far

1. **Model Integration**:  
   We created a `G2RetroModel` class that inherits from `ExternalReactionModel[InputType, ReactionType]`. This class wraps the G2Retro model to work as a backward reaction model within syntheseus.  
   - Implemented `_mols_to_batch()` and `_get_reactions()` methods to handle input molecules and return predictions.
   - Managed caching and duplicate filtering for reaction predictions.

2. **Benchmarking Setup**:  
   We introduced new benchmark tests (using `pytest-benchmark`) to measure the performance of the `G2RetroModel` under various input conditions.  
   - Tested scenarios with varying input sizes and complexity (simple molecules, complex SMILES, larger batches).
   - Added tests for scenarios with and without caching, varying `num_results`, and comparing different model versions.

3. **Definition of Concepts**:  
   - **BackwardReactionModel**: A model that predicts reactants given a product molecule.
   - **ForwardReactionModel**: A model that predicts products given reactants.
   - **Caching**: Storing previously computed predictions to speed up subsequent calls.
   - **Benchmarking**: Running tests to measure runtime performance, using different inputs and configurations.

## Where We Made Code Changes

- **`syntheseus/reaction_prediction/inference/g2retro.py`**:  
  Implemented the `G2RetroModel` class, overriding `_mols_to_batch()` and `_get_reactions()` to integrate G2Retro inference.
  
- **`syntheseus/interface/models.py`**:  
  Made sure our `ExternalReactionModel` inheritance and the reaction model interface are compatible.
  
- **`syntheseus/tests/interface/test_g2retro_model_bench.py`**:  
  Added benchmarking tests to measure performance under various scenarios.

- **`syntheseus/tests/reaction_prediction/inference/`** (general):  
  Integrated new test files or edited existing ones to ensure backward model tests pass correctly.

## How to Run Benchmarks

1. Install dependencies as described in the project's documentation. Ensure `pytest` and `pytest-benchmark` are installed:
   ```bash
   pip install pytest pytest-benchmark
   
2. Navigate to the root directory of the `syntheseus project`.

3. Run the benchmark tests:
   ```bash
   pytest --benchmark-only path/to/test_g2retro_model_bench.py
4. Check the output for performance results. By default, pytest-benchmark will print a summary table showing execution times, iterations, and statistics.

## Definitions Used
### InputType: 
A generic type representing the type of input the model receives (e.g., a single Molecule).

### ReactionType: 
A generic type representing the type of reaction output (e.g., SingleProductReaction).

### num_results: 
The number of reaction predictions requested from the model.

### Caching: 
Storing and retrieving results from previous model calls to reduce computation time.

### Duplicate filtering: 
Ensuring that if multiple identical reactions are predicted, they are collapsed into a single unique result.

## Future Directions
* Further integration with additional models.
* More complex benchmark scenarios (e.g., random SMILES generation, real-world datasets).
* Optimization of caching strategies and memory usage.