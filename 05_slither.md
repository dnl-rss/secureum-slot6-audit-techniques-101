### 33. Slither

*Slither* is a Solidity static analysis framework written in Python 3.
- runs a suite of vulnerability detectors, prints visual information about contract details, and provides an API to easily write custom analyses.
- enables developers to find vulnerabilities, enhance their code comprehension, and quickly prototype custom analyses.
- implements 74 detectors in the publicly available free version (with trophies that showcase Slither findings in real-world contracts).

### 34. Slither features:

- *Detects vulnerable Solidity code* with low false positives
- *Identifies where the error condition occurs* in the source code
- Easily integrates into *continuous integration* and Truffle builds
- Built-in 'printers' quickly *report crucial contract information*
- Detector API to write *custom analyses* in Python
- Ability to analyze contracts written with `version >= 0.4`
- Intermediate representation (SlithIR) enables simple, high-precision analyses
- Correctly parses **99.9%** of all public Solidity code
- Average execution time of less than *1 second per contract*

### 35. Slither execution environment

Slither bugs and optimizations detection can run on a Truffle/Embark/Dapp/Etherlime/Hardhat application or on a single Solidity file.

Slither runs all its detectors by default.
- `--detect detector1,detector2`: run only selected detectors
- `--exclude detector1,detector2`: exclude selected detectors
- `--exclude-informational`: exclude `informational` severity detectors
- `--exclude-low`: exclude `low` severity detectors
- `--list-detectors`: lists available detectors

### 36. Slither printers

Slither printers allow printing contract information with `--print` and following options:
- `call-graph`: Export the call-graph of the contracts to a dot file
- `cfg`: Export the CFG of each functions
- `constructor-calls`: Print the constructors executed
- `contract-summary`: Print a summary of the contracts
- `data-dependency`: Print the data dependencies of the variables
- `echidna`: Export Echidna guiding information
- `evm`: Print the evm instructions of nodes in functions
- `function-id`: Print the keccack256 signature of the functions
- `function-summary`: Print a summary of the functions
- `human-summary`: Print a human-readable summary of the contracts
- `inheritance`: Print the inheritance relations between contracts
- `inheritance-graph`: Export the inheritance graph of each contract to a dot file
- `modifiers`: Print the modifiers called by each function
- `require`: Print the require and assert calls of each function
- `slithir`: Print the slithIR representation of the functions
- `slithir-ssa`: Print the slithIR representation of the functions          
- `variable-order`: Print the storage order of the state variables
- `vars-and-auth`: Print the state variables written and the authorization of the functions

### 37. Slither upgradability

Slither upgradeability checks helps review contracts that use the delegatecall proxy pattern using `slither-check-upgradeability` tool with following options:
- `became-constant`: Variables that should not be constant
- `function-id-collision`: Functions ids collision
- `function-shadowing`: Functions shadowing
- `missing-calls`: Missing calls to init functions
- `missing-init-modifier`: initializer() is not called
- `multiple-calls`: Init functions called multiple times
- `order-vars-contracts`: Incorrect vars order with the v2
- `order-vars-proxy`: Incorrect vars order with the proxy
- `variables-initialized`: State variables with an initial value
- `were-constant`: Variables that should be constant
- `extra-vars-proxy`: Extra vars in the proxy
- `missing-variables`: Variable missing in the v2
- `extra-vars-v2`: Extra vars in the v2
- `init-inherited`: Initializable is not inherited
- `init-missing`: Initializable is missing
- `initialize-target`: Initialize function that must be called
- `initializer-missing`: initializer() is missing

### 38. Slither code similarity

Slither code *similarity detector* (a research-oriented tool) uses state-of-the-art machine learning to detect similar (vulnerable) Solidity functions
- uses a pre-trained model from `etherscan_verified_contracts` with 60,000 contracts and more than 850,000 functions
- uses FastText, a vector embedding technique, to generate compact numerical representations of every function
- has four modes:
    1. `test`: finds similar functions to your own in a dataset of contracts
    2. `plot`: provide a visual representation of similarity of multiple sampled functions
    3. `train`: builds new models of large datasets of contracts
    4. `info`: inspects the internal information of the pre-trained model or the assessed code

### 39. Slither contract flattening

Slither contract flattening tool `slither-flat` produces a flattened version of the codebase with the following features:
- Supports three strategies:
    1. `MostDerived`: Export all the most derived contracts (every file is standalone)
    2. `OneFile`: Export all the contracts in one standalone file
    3. `LocalImport`: Export every contract in one separate file, and include import ".." in their preludes
- Supports circular dependency
- Supports all the compilation platforms (Truffle, embark, buidler, etherlime, ...).

### 40. Slither format

Slither format tool `slither-format` automatically generates git-compatible patches.

Patches should be carefully reviewed before applying.

Detectors supported with this tool are:
- `unused-state`
- `solc-version`
- `pragma`
- `naming-convention`
- `external-function`
- `constable-states`
- `constant-function`

### 41. Slither ERC conformance

Slither ERC conformance tool `slither-check-erc` checks the following for ERC's conformance for `ERC20`, `ERC721`, `ERC777`, `ERC165`, `ERC223` and `ERC1820`:
- [ ] All the functions are present
- [ ] All the events are present
- [ ] Functions return the correct type
- [ ] Functions that must be view are view
- [ ] Events' parameters are correctly indexed
- [ ] The functions emit the events
- [ ] Derived contracts do not break the conformance

### 42. Slither property generation

Slither property generation tool `slither-prop` generates code properties (e.g., invariants) that can be tested with unit tests or Echidna, entirely automatically.

The `ERC20` scenarios that can be tested are:
- `Transferable`: Test the correct tokens transfer
- `Pausable`: Test the pausable functionality
- `NotMintable`: Test that no one can mint tokens
- `NotMintableNotBurnable`: Test that no one can mint or burn tokens
- `NotBurnable`: Test that no one can burn tokens
- `Burnable`: Test the burn of tokens. Require the `burn(address) returns()` function

### 43. Slither new detectors  

Slither’s plugin architecture lets you integrate *new detectors* that run from the command line.

The skeleton for a detector has:
- `ARGUMENT`: lets you run the detector from the command line
- `HELP`: is the information printed from the command line
- `IMPACT`: indicates the impact of the issue.
    - Allowed values are `INFORMATIONAL`|`LOW`|`MEDIUM`|`HIGH`
- `CONFIDENCE`: indicates your confidence in the analysis.
    - Allowed values are `LOW`|`MEDIUM`|`HIGH`
- `WIKI`: constants are used to generate automatically the documentation.
- `_detect()`: is the function that implements the detector logic and needs to return a list of findings.
