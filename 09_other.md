### 65. Scribble

*Scribble* is a verification language and runtime verification tool that translates high-level specifications into solidity code.

It allows you to annotate a solidity smart contract with properties (See here).

Principles/Goals:
1. Specifications are easy to understand by developers and auditors
2. Specifications are simple to reason about
3. Specifications can be efficiently checked using off-the-shelf analysis tools
4. A small number of core specification constructs are sufficient to express and reason about more advanced constructs

Transforms annotations in the Scribble specification language into concrete assertions

With these instrumented but equivalent contracts, one can then use Mythril, Harvey, MythX

### 66. Fuzzing-as-a-Service:

*Fuzzing-as-a-Service* enables project to submit smart contracts along with embedded inlined specifications or properties written using the Scribble language.
- These contracts are run through the Harvey fuzzer which uses the specified properties to optimize fuzzing campaigns.
- Any violations from fuzzing are reported back from the service for the project to fix.

ConsenSys Diligence

### 67. Karl

*Karl* is a monitor for smart contracts that checks for security vulnerabilities using the Mythril detection engine.
- It can be used to monitor the Ethereum blockchain for newly deployed vulnerable smart contracts in real-time.

### 68. Theo

*Theo* is an exploitation tool with a Metasploit-like interface, drops you into a Python REPL console, where you can use the available features to do smart contract reconnaissance, check the storage, run exploits or frontrun or backrun transactions targeting a specific smart contract.

Features:
- Automatic smart contract scanning which generates a list of possible exploits
- Sending transactions to exploit a smart contract
- Transaction pool monitor
- Web3 console
- Frontrunning and backrunning transactions
- Waiting for a list of transactions and sending out others
- Estimating gas for transactions means only successful transactions are sent
- Disabling gas estimation will send transactions with a fixed gas quantity.

### 69. Visual Auditor

Visual Auditor is a Visual Studio Code extension that provides security-aware syntax and semantic highlighting for Solidity and Vyper.
- **Syntax Highlighting**: access modifiers (external, public, payable, …), security relevant built-ins, globals, methods and user/miner-tainted information, (address.call(), tx.origin, msg.data, block.*, now), storage access modifiers (memory, storage), developer notes in comments (TODO, FIXME, HACK, …), custom function modifiers, contract creation / event invocations, easily differentiate between arithmetics vs. logical operations, make Constructor and Fallback function more prominent
- **Semantic Highlighting**: highlights StateVars (constant, inherited), detects and alerts about StateVar shadowing, highlights function arguments in the function body
- **Review Features**: audit annotations/bookmarks - @audit - <msg> @audit-ok - <msg> (see below), generic interface for importing external scanner results - cdili json format (see below), codelens inline action: graph, report, dependencies, inheritance, parse, ftrace, flatten, generate unit test stub, function signature hashes, uml
- **Graph and Reporting Features**: access your favorite Sūrya features from within vscode, interactive call graphs with call flow highlighting and more, auto-generate UML diagrams from code to support your threat modelling exercises or documentation
- **Code Augmentation**: Hover over Ethereum Account addresses to download the byte-code, source-code or open it in the browser, Hover over ASM instructions to show their signatures, Hover over keywords to show basic Security Notes, Hover over StateVar's to show declaration information
- **Views**: Cockpit vs Outline

### 70. Surya

*Surya* aids auditors in understanding and visualizing Solidity smart contracts by providing information about the contracts’ structure and generates call graphs and inheritance graphs.
- It also supports querying the function call graph in multiple ways to aid in the manual inspection of contracts.
- Integrated with Visual Auditor
- Commands: graph, ftrace, flatten, describe, inheritance, dependencies, parse, mdreport

### 71. SWC Registry:

The Smart Contract Weakness Classification Registry (SWC Registry) is an implementation of the weakness classification scheme proposed in EIP-1470.
- It is loosely aligned to the terminologies and structure used in the Common Weakness Enumeration (CWE) while overlaying a wide range of weakness variants that are specific to smart contracts
- The goals of this project are as follows:
    1. Provide a straightforward way to classify security issues in smart contract systems.
    2. Define a common language for describing security issues in smart contract systems' architecture, design, or code.
    3. Serve as a way to train and increase performance for smart contract security analysis tools.
- This repository is maintained by the team behind MythX and currently contains 37 entries

### 72. Securify:

Security is a security scanner for Ethereum smart contracts which Implements static analysis written in Datalog and supports 38 vulnerabilities

### 73. VerX:

VerX is a verifier that can automatically prove temporal safety properties of Ethereum smart contracts.

The verifier is based on a careful combination of three ideas:
1. reduction of temporal safety verification to reachability checking,
2. an efficient symbolic execution engine used to compute precise symbolic states within a transaction
3. delayed abstraction which approximates symbolic states at the end of transactions into abstract states

### 74. SmartCheck:

SmartCheck is an extensible static analysis tool for discovering vulnerabilities and other code issues in Ethereum smart contracts written in the Solidity programming language.

It translates Solidity source code into an XML-based intermediate representation and checks it against XPath patterns.

### 75. K-Framework

K-framework based analysis, modelling and verification tools from Runtime Verification (RV):

provides KEVM which is a model of EVM in the K-Framework.

It is the first executable specification of the EVM that completely passes official test-suites and serves as a platform for building a wide range of analysis tools and other semantic extensions for EVM.

### 76. Certora Prover:

Certora checks that a smart contract satisfies a set of rules written in a language called Specify.
- Each rule is checked on all possible transactions, though of course this is not done by explicitly enumerating transactions, but rather through symbolic techniques.
- Provides complete path coverage for a set of safety rules provided by the user.
    - For example, a rule might check that only a bounded number of tokens can be minted in an ERC20 contract. The prover either guarantees that a rule holds on all paths and all inputs or produces a test input that demonstrates a violation of the rule.
- The problem addressed by the Certora Prover is known to be undecidable which means that there will always be pathological programs and rules for which the Certora prover will time out without a definitive answer
- Takes the smart contract as input (either as EVM bytecode or Solidity source code) and a set of rules, written in Certora’s specification language.
    - The Prover then automatically determines whether or not the contract satisfies all the rules using a combination of two computer science techniques: abstract interpretation and constraint solving

### 77. Hevm

DappHub’s Hevm is an implementation of the EVM made specifically for unit testing and debugging smart contracts.
- It can run unit tests, property tests, interactively debug contracts while showing the Solidity source, or run arbitrary EVM code.

### 78. CTF

Capture the Flag (CTF) games are fun and educational challenges where participants have to hack different (dummy) smart contracts that have vulnerabilities in them. They help understand the complexities around how vulnerabilities may be exploited in the wild.

Popular ones include:
- **Capture The Ether**: is a set of twenty challenges created by Steve Marx which test knowledge of Ethereum concepts of contracts, accounts and math among other things.
- **Ethernaut**: is a Web3/Solidity based war game from OpenZeppelin that is played in the Ethereum Virtual Machine. Each level is a smart contract that needs to be ‘hacked'. The game is 100% open source and all levels are contributions made by other players
- **Damn Vulnerable DeFi v2**: is a set of 12 DeFi related challenges created by tinchoabbate. Depending on the challenge, you should either stop the system from working, steal as much funds as you can, or do some other unexpected things.
- **Paradigm CFT**: is a set of seventeen challenges created by samczsun at Paradigm.

### 79. Pitfalls of tools

Smart contract security tools are useful in assisting auditors while reviewing smart contracts.
- They automate many of the tasks that can be codified into rules with different levels of coverage, correctness and precision.
- They are fast, cheap, scalable and deterministic compared to manual analysis. But they are also susceptible to false positives.
- They are especially well-suited currently to detect common security pitfalls and best-practices at the Solidity and EVM level.
- With varying degrees of manual assistance, they can also be programmed to check for application-level, business-logic constraints.
