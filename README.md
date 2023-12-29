# Using S2C binary docker image
- Combine the docker image files => `cat s2c-docker-image.tar.bz2.00 s2c-docker-image.tar.bz2.01 | tee s2c-docker-image.tar.bz2 > /dev/null`
- Extract the docker image => `bunzip2 s2c-docker-image.tar.bz2`
- Load the docker image => `docker load < s2c-docker-image.tar`
- Start a docker container & enter shell => `docker run -it s2c:1.0`
- Open README.md for details on how to run tests => `vim README.md`
- Detach from shell without stopping image => `ctrl+p ctrl+q`
- Reattach to container shell => `docker attach <container-name-or-id>`

# Running S2C tests
## Structure
Tests are organized within the directory defined by `$SUPEROPT_SPEC_TESTS_DIR`.

These tests are divided into two directories:
  - `samples` => contain simple testcases
  - `benchmarks` => contain testcases used in evaluation of S2C

Each test requires the following 3 files:
  - `.spec` => Specification
  - `.c` => Implementation
  - `.iospecs` => I/O (Pre,Post) Specification

## How to run tests
The root as well as the two subdirectories contain makefiles to run tests.
Execute `make run` to run tests under that directory.
Once finished, each testcase prints its final result (equivalence found / failed) and execution time.
Each testcase can also be run individually, refer to `suite.sh` scripts inside `samples` and `benchmarks` for this.

## Manual run
The executable `s2c` can be manually called with custom inputs as well.
However, it is recommended to use `s2c_sample_{run|debug}` functions defined in `run.sh` scripts inside `samples` and `benchmarks` for the appropriate arguments.

The following are the most relevant arguments `s2c` accepts:
- `unroll-factor` => unroll factor (\mu)
- `speceq-solver-pre-expansion-depth` => unrolling parameter used for categorization of proof obligations (k)
- `speceq-solver-weakening-depth` => over-approximation depth (d_o)
- `speceq-solver-strengthening-depth` => under-approximation depth (d_u)

## Stat collection
For evaluating `s2c`, we also collect additional statistics such as # of type I queries that were proven or disproven.
To do this, run `make` in the `scripts` directory to build the executable `extract_stats`.
`extract_stats` requires two arguments:
- `path to the testcase run stdout` => by default, these outputs are generated inside `{samples|benchmarks}/tmpdir` directories
- `stats filename` => the file to save the results formatted as csv

Assuming it is run from the `scripts` directory to extract stat for all `benchmarks` => `./extract_stats ../benchmarks/tmpdir benchmark_stats.csv`
