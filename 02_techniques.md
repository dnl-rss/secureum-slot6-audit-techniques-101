### 20. Specification analysis:

*Specification* describes in detail what (and sometimes why) the project and its various components are supposed to do functionally as part of their design and architecture.
- From a security perspective, it specifies:
    - what the assets are
    - where they are held
    - who are the actors
    - privileges of actors
    - who is allowed to access what and when
    - trust relationships
    - threat model
    - potential attack vectors
    - scenarios
    - mitigations
- Analysing the specification of a project provides auditors with the above details and lets them evaluate the assumptions made and indicate any shortcomings
- Very few smart contract projects have detailed specifications at their first audit stage. At best, they have some documentation about what is implemented. Auditors spend a lot of time inferring specification from documentation/implementation which leaves them with less time for vulnerability assessment.

### 21. Documentation analysis:

*Documentation* is a description of what has been implemented, based on the design and architectural requirements.
- answers *how* something has been designed/architected/implemented, without necessarily addressing the *why* and the design/requirement goals
- typically in the form of Readme files in the Github repository describing individual contract functionality combined with functional NatSpec and individual code comments
- often serves as a substitute for specification and provides critical insights into the assumptions, requirements, and goals of the project team
- Understanding the documentation before looking at the code helps auditors save time in inferring the architecture of the project, contract interactions, program constraints, asset flow, actors, threat model and risk mitigation measures
- mismatches between the documentation and the code could indicate stale/poor documentation, software defects, or security vulnerabilities
- auditors are expected to encourage the project team to document thoroughly so that they do not need to waste their time inferring this by reading code

### 22. Testing:

Software testing or validation is a well-known fundamental software engineering primitive to determine if software produces expected outputs when executed with different chosen inputs.

Smart contract testing has a similar motivation but is arguably more complicated despite their relatively smaller sizes (in lines of code) compared to Web2 software
- Smart contract development platforms (Truffle, Embark, Brownie, Waffle, Hardhat etc.) are relatively new with different levels of support for testing
- Projects usually have little testing done at the audit stage. Testing integrations and composability with mainnet contracts and state is non-trivial

Test coverage and test cases give a good indication of project maturity and also provide valuable insights to auditors into assumptions/edge-cases for vulnerability assessments

Auditors should expect a high-level of testing and test coverage because this is a must-have software-engineering discipline, especially when smart contracts that are by-design exposed to everyone on the blockchain end up holding assets worth tens of millions of dollars

> "Program testing can be used to show the presence of bugs, but never to show their absence!” - E.W. Dijkstra

### 23. Static analysis:

*Static analysis* is a technique of analyzing program properties *without actually executing* the program.
- typically is a combination of control flow and data flow analyses
- this is in contrast to software testing where programs are actually executed/run with different inputs

Tools:
- [Slither](https://github.com/crytic/slither) for Solidity
- [Mythril](https://github.com/ConsenSys/mythril) for EVM bytecode

### 24. Fuzzing:

[Fuzz testing](https://en.wikipedia.org/wiki/Fuzzing) is an automated software testing technique that involves:
1. *providing invalid, unexpected, or random data* as inputs to a computer program
2. *monitoring the program for exceptions* such as crashes, failing built-in code assertions, or potential memory leaks

Fuzzing is especially relevant to smart contracts because anyone can interact with them on the blockchain with random inputs without necessarily having a valid reason or expectation (arbitrary byzantine behaviour)

Tools:
- [Echidna](https://github.com/crytic/echidna)
- [Harvey](https://mariachris.github.io/Pubs/FSE-2020-Harvey.pdf)

### 25. Symbolic checking:

[Symbolic checking](https://en.wikipedia.org/wiki/Model_checking#Symbolic_model_checking) is a technique of *checking for program correctness*, i.e. proving/verifying, by using symbolic inputs to represent set of states and transitions instead of enumerating individual states/transitions separately
- Model checking or property checking is a method for checking whether a finite-state model of a system meets a given specification (also known as correctness)
- In order to solve such a problem algorithmically, both the model of the system and its specification are formulated in some precise mathematical language. To this end, the problem is formulated as a task in logic, namely to check whether a structure satisfies a given logical formula.
- A simple model-checking problem consists of verifying whether a formula in the propositional logic is satisfied by a given structure
- Instead of enumerating reachable states one at a time, the state space can sometimes be traversed more efficiently by considering large numbers of states at a single step. When such state space traversal is based on representations of a set of states and transition relations as logical formulas, binary decision diagrams (BDD) or other related data structures, the model-checking method is symbolic.
- Model-checking tools face a combinatorial blow up of the state-space, commonly known as the state explosion problem, that must be addressed to solve most real-world problems
- Symbolic algorithms avoid explicitly constructing the graph for the finite state machines (FSM); instead, they represent the graph implicitly using a formula in quantified propositional logic

### 26. Formal verification:

[Formal verification](https://en.wikipedia.org/wiki/Formal_verification) is the act of proving or disproving the correctness of intended algorithms underlying a system with respect to a certain formal specification or property, using formal methods of mathematics
- effective at detecting complex bugs which are hard to detect manually or using simpler automated tools
- needs a specification of the program being verified and techniques to translate/compare the specification with the actual implementation

Tools:
- [Certora](https://www.certora.com/) Prover
- ChainSecurity [VerX](http://verx.ch/)
- Runtime Verification Inc [KEVM](https://github.com/kframework/evm-semantics) models EVM semantics.

Specifications and Results from VerX:

```
// Only the owner can change the supply of tokens
property only_owner_mints {
    always((SampleToken._totalSupply > prev(SampleToken._totalSupply))
            ==> (prev(SampleToken._owner) == msg.sender));
}

// Only users may change their own balances of tokens
property only_msg_sender_lower_balance {
    always((prev(SampleToken._balances[0x123]) > SampleToken._balances[0x123])
            ==> (msg.sender == 0x123));
}

// The sum of user balances equals the supply of tokens
property balances_sum_equals_total_supply {
    always(SUM(SampleToken._balances) == SampleToken._totalSupply);
}

// Only the owner can elect who is the new owner
property only_owner_changes_owner {
    always((prev(SampleToken._owner) != SampleToken._owner)
            ==> (msg.sender == prev(SampleToken._owner)));
}

// Only the mint function can change the supply of tokens
property only_mint_changes_supply {
    always((SampleToken._totalSupply != prev(SampleToken._totalSupply))
            ==> (FUNCTION == SampleToken.mint(address,uint256)));
}

// The supply of tokens is constant
property constant_token_supply {
    always(SampleToken._totalSupply == prev(SampleToken._totalSupply));
}

// The name of the token cannot be changed
property constant_name {
    always(SampleToken._name == prev(SampleToken._name));
}

// The symbolc of the token cannot be changed
property constant_symbol {
    always(SampleToken._symbol == prev(SampleToken._symbol));
}

// The decimals of the token cannot be changed
property constant_decimals {
    always(SampleToken._decimals == prev(SampleToken._decimals));
}
```

```
Property: constant_token_supply
Failed to verify property, possible violation trace: Deployer.init(), SampleToken.mint(address,uint256)
```

### 27. Manual analysis:

*Manual analysis* is complimentary to *automated analysis* using tools and serves a critical need in smart contract audits
- *Automated analysis* using tools is cheap (typically open-source free software), fast, deterministic, and scalable (varies depending on the tool being semi-/fully-automated) but is only as good as the properties it is made aware of, which is typically limited to Solidity and EVM related constraints
- *Manual analysis* with humans, in contrast, is expensive, slow, non-deterministic, and not scalable because human expertise in smart contact security is a rare/expensive skill set today and we are slower, prone to error, and inconsistent.
- *Manual analysis* is the only way to infer and evaluate business logic and application-level constraints where a majority of the serious vulnerabilities are being found
