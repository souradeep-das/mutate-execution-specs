# Mutate Execution Specs

`mutate-execution-specs` is a Mutation testing setup guide and toolset designed for the [Ethereum Execution Client Specifications](https://github.com/ethereum/execution-specs).


It uses [mutmut](https://github.com/boxed/mutmut) ([v3.3.0](https://pypi.org/project/mutmut/3.3.0/)) to systematically generate small changes (mutations) to the execution-spec and runs the existing test suite to check wether the changes are detected. The mutation tests are configured to run on a per fork-basis.

## Running Mutation tests for a specific Fork (Frontier)

### a. Setup

1. Clone the repo

```
$ git clone https://github.com/souradeep-das/mutate-execution-specs.git
$ cd mutate-execution-specs/
```

2. Clone the execution-specs repo

```
$ git clone https://github.com/ethereum/execution-specs.git
$ cd execution-specs/
```

3. Setup a virtual env

```
$ python -m venv venv
$ . venv/bin/activate
```

4. Install the package and test deps in executable mode

```
$ pip install -e .[test]
```

or, if you are using zsh

```
$ pip install -e '.[test]'
```

5. Install mutmut

```
pip install mutmut==3.3.0
```

### b. Configure

6. Update your `pyproject.toml` with the following section to configure `mutmut`

```
[tool.mutmut]
paths_to_mutate = [ "src/ethereum/frontier/" ]
tests_dir = ["tests/frontier/"]
debug=true
```

Here, paths_to_mutate and tests_dir have been setup to work for the `frontier` fork. Setting `debug=true` is optional. For running on a different fork - replace <frontier> with the specific fork name.

7. Copy other files and directories that the mutated code will depend on.


```
mkdir -p mutants/src && cp -r src/* mutants/src/
```

### c. Patch

8. Patch Mutmut

```
$ cp ../mutmut.patch .
$ patch venv/lib/python3.12/site-packages/mutmut/__main__.py < mutmut.patch
```

A patched version of `mutmut` is required to run the mutation tests. To find the differences refer [here](./docs/mutmut_diff.md).

9. Patch `tests/` to be able to work with `mutmut`

```
$ cp ../conftest.patch .
$ patch -p0 <conftest.patch
```

9. Patch `tests/frontier` to be able to work with `mutmut`

```
$ cp ../fork-patch/frontier/update_tests.patch .
$ patch -p0 < update_tests.patch
```

A patched version of the tests are required to run the mutation tests. To find the differences refer [here](./docs/tests_diff.md). For a different fork - replace <frontier> with the specific fork name.


### d. Run

10. Run mutation tests

```
$ mutmut run
```

or to run sequentially
```
$ mutmut run --max-children 1
```


## Last-known compatible versions

- [execution-specs #5bab4da](https://github.com/ethereum/execution-specs/commit/5bab4dace60bfd797e4a3cdb9a026446067f657b)
- [mutmut v3.3.0](https://github.com/boxed/mutmut/releases/tag/3.3.0)
