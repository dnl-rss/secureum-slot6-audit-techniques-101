### 44. Manticore

*Manticore* is a symbolic execution tool for analysis of Ethereum smart contracts (besides Linux binaries & WASM modules). See tutorial for details.
- **Program Exploration**: executes a program with symbolic inputs and explores all the possible states it can reach
- **Input Generation**: automatically produces concrete inputs that result in a given program state
- **Error Discovery**: detects crashes and other failure cases in binaries and smart contracts
- **Instrumentation**: provides fine-grained control of state exploration via event callbacks and instruction hooks
- **Programmatic Interface**: exposes programmatic access to its analysis engine via a Python API

### 45. Echidna

*Echidna* is a Haskell program designed for *fuzzing/property-based testing* of Ethereum smart contracts.

It uses sophisticated grammar-based fuzzing campaigns based on a contract ABI to falsify user-defined predicates or Solidity assertions.

### 46. Echidna Features:

- Generates inputs tailored to your actual code
- Optional corpus collection, mutation and coverage guidance to find deeper bugs
- Powered by Slither to extract useful information before the fuzzing campaign
- Source code integration to identify which lines are covered after the fuzzing campaign
- Curses-based retro UI, text-only or JSON output
- Automatic test case minimization for quick triage
- Seamless integration into the development workflow
- Maximum gas usage reporting of the fuzzing campaign
- Support for a complex contract initialization with Etheno and Truffle

### 47. Echidna Usage

(see tutorial for details):

- **Executing the test runner**: The core Echidna functionality is an executable called `echidna-test`.
    - `echidna-test` takes a contract and a list of invariants (properties that should always remain true) as input.
    - For each invariant, it generates random sequences of calls to the contract and checks if the invariant holds.
    - If it can find some way to falsify the invariant, it prints the call sequence that does so.
    - If it can't, you have some assurance the contract is safe.
- **Writing invariants**: Invariants are expressed as Solidity functions with names that begin with `echidna_`, have no arguments, and return `bool`.
- **Collecting and visualizing coverage**: After finishing a campaign, Echidna can save a coverage maximizing corpus in a special directory specified with the `corpusDir config` option.
    - This directory will contain two entries:
        1. a directory named coverage with JSON files that can be replayed by Echidna
        2. a plain-text file named `covered.txt`, a copy of the source code with coverage annotations.
