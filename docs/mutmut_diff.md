1. Add tests_dir to mutmut config to be able to selectively pick test files

2. Set PYTHONPATH to mutmut src files

3. Disable timeout checker thread in mutmut to avoid automatic SIGXCPU for long-running mutants.

4. Run the whole test suite for each mutation, since a specific unit test for the mutated code cannot be mapped.