### 80. Audit Process

Audit Process could be thought of as a ten-step process as follows:
1. Read specification/documentation of the project to understand the requirements, design and architecture
2. Run fast automated tools such as linters or static analyzers to investigate common Solidity pitfalls or missing smart contract best-practices
3. Manual code analysis to understand business logic and detect vulnerabilities in it
4. Run slower but more deeper automated tools such as symbolic checkers, fuzzers or formal verification analyzers which typically require formulation of properties/constraints beforehand, hand holding during the analyses and some post-run evaluation of their results
5. Discuss (with other auditors) the findings from above to identify any false positives or missing analyses
6. Convey status to project team for clarifying questions on business logic or threat model
7. Iterate the above for the duration of the audit leaving some time for report writing
8. Write report summarizing the above with details on findings and recommendations
9. Deliver the report to the project team and discuss findings, severity and potential fixes
10. Evaluate fixes from the project team and verify that they indeed remove the vulnerabilities identified in findings.

### 81. Reading specification/documentation:

For projects that have a specification of the design and architecture of their smart contracts, this is the recommended starting point. Very few new projects have a specification at the audit stage. Some of them have documentation in parts. Some key points:
- Specification starts with the project’s technical and business goals and requirements. It describes how the project’s design and architecture help achieve those goals.
- The actual implementation of smart contracts is a functional manifestation of the goals, requirements, specification, design and architecture, understanding of which is critical in evaluating if the implementation indeed meets the goals and requirements
- Documentation is a description of what has been implemented based on the design and architectural requirements.
- Specification answers ‘why’ something needs to be designed/architected/implemented the way it has been done. Documentation answers ‘how’ something has been designed/architected/implemented without necessarily addressing the ‘why’ and leaves it up to the auditors to speculate on the reasons.
- Documentation is typically in the form of Readme files describing individual contract functionality combined with functional NatSpec and individual code comments. Encouraging projects to provide a detailed specification and documentation saves a lot of time and effort for the auditors in understanding the project goals/structure and prevents them from making the same assumptions as the implementation which is a leading cause of vulnerabilities.
- In the absence of both specification and documentation, auditors are forced to infer goals, requirements, design and architecture from reading code and using tools such as Surya and Slither printers. This takes up a lot of time leaving less time for deeper/complex security analyses.

### 82. Running static analyzers:

Automated tools such as linters or static analyzers help investigate common Solidity pitfalls or missing smart contract best-practices
- Tools such as Slither and MythX perform control-flow and data-flow analyses on the smart contracts in the context of their detectors which encode common security pitfalls and best-practices.
- Evaluating their findings, which are usually available in seconds/minutes, is a good starting point to detect common vulnerabilities based on well-known constraints/properties of Solidity language, EVM or Ethereum blockchain.
- False positives are possible among some of the detector findings and need to be verified manually if they are true/false positives

### 83. Manual code review:

is required to understand business logic and detect vulnerabilities in it.
- Automated analyzers do not understand application-level logic and their constraints. They are limited to constraints/properties of Solidity language, EVM or Ethereum blockchain.
- Manual analysis of the code is required to detect security-relevant deviations in implementation vis-a-vis the specification or documentation.
- Auditors may need to infer business logic and their implied constraints directly from the code or from discussions with the project team and thereafter evaluate if those constraints/properties hold in all parts of the codebase.

### 84. Running deeper automated tools:

Deeper automated tools such as fuzzers e.g. Echidna, symbolic checkers such as Manticore, tool suite such as MythX and formally verifying custom properties with Scribble or Certora Prover takes more setup and preparation time but helps run deeper analyses to discover edge-cases in application-level properties and mathematical errors, among other things.
- Given these require understanding of the project’s application logic, they are recommended to be used at least after an initial manual code review or sometimes after deeper discussion about the specification/implementation with the project team
- Analyzing the output of these tools requires significant expertise with the tools themselves, their domain-specific language and sometimes even their inner workings
- Evaluating false-positives is sometimes challenging with these tools but the true positives they discover are significant and extreme corner cases missed even by the best manual analyses

### 85. Brainstorming with other auditors:

> "Given enough eyeballs, all bugs are shallow" - Linus's Law

- While some audit firms encourage active/passive discussion, there are others whose approach is to let auditors separately perform the assessment to encourage independent thinking instead of group thinking. The premise is that group thinking might bias the audit team to focus on certain aspects while missing some vulnerabilities.
- A hybrid approach might be interesting where the audit team initially brainstorms to discuss the project’s goals, specification/documentation and implementation but later firewall themselves to independently pursue the assessments and finally come together to compile their findings.

### 86. Discussion with project team:

Having an open communication channel with the project team is  useful to clarify any assumptions in specification, documentation, implementation, or discuss interim findings.
- Findings may also be shared with the project team immediately on a private repository to discuss impact, fixes and other implications.
- If the audit spans multiple weeks, it may help to have a weekly sync up call. A counterpoint to this is to independently perform the entire assessment so as to not get biased by the project team’s inputs and opinions.

### 87. Report writing:

The audit report is a final compilation of the entire assessment and presents all aspects of the audit including the audit scope/coverage, timeline, team/effort, summaries, tools/techniques, findings, exploit scenarios, suggested fixes, short-/long-term recommendations and any appendices with further details on tools and rationale.
- An executive summary typically gives an overview of the audit report with highlights/lowlights illustrating the number/type/severity of vulnerabilities found and an overall assessment of risk. It may also include a description of the smart contracts, (inferred) actors, assets, roles, permissions, access control, interactions, threat model and existing risk mitigation measures
- The bulk of the report focuses on the findings from the audit, their type/category, likelihood/impact, severity, justifications for these ratings, potential exploit scenarios, affected parts of smart contracts and potential remediations
- It may also address subjective aspects of code quality, readability/auditability and other software-engineering best practices related to documentation, code structure, function/variable naming conventions, test coverage etc. that do not pose an imminent security risk but are indicators of anti-patterns and processes influencing the introduction and persistence of security vulnerabilities

### 88. Report delivery:

The delivery of the report to the project team is a critical deliverable and milestone. Unless interim findings/status is shared, this will be the first time the project team will have access to the assessment details.
- The delivery typically happens via a shared online document and is accompanied with a readout where the auditors present the report highlights to the project team for discussion and any debate on findings and their severity ratings
- The project team typically takes some time to review the audit report and respond back with any counterpoints on findings, severities or suggested fixes
- Depending on the prior agreement, the project team and audit firm might release the audit report publicly (after all required fixes have been made) or the project may decide to keep it private for some reason

### 89. Evaluating fixes:

Post audit, the project team may work on any required fixes for reported findings and request the audit firm for reviewing their responses
- Fixes may be applied for a majority of the findings and the review may need to confirm that applied fixes (could be different from audit’s recommended fixes) indeed mitigate the risk reported by the findings
- Findings may be contested as not being relevant, outside the project’s threat model or simply acknowledged as being within the project’s acceptable risk model
- Audit firms may evaluate the specific fixes applied and confirm/deny their risk mitigation. Unless it is a fix/retainer type audit, this phase typically takes not more than a day because it would usually be outside the agreed upon duration of the audit.

### 90. Manual review approaches:

Auditors have different approaches to manual reviewing smart contract code for vulnerabilities.
- Starting with access control
- Starting with asset flow
- Starting with control flow
- Starting with data flow
- Inferring constraints
- Understanding dependencies
- Evaluating assumptions
- Evaluating security checklists

### 91. Starting with access control:

Access control is the most fundamental security primitive which addresses ‘who’ has authorised access to ‘what.’ (In a formal access control model, the ‘who’ refers to subjects, ’what’ refers to objects and an access control matrix indicates the permissions between subjects and objects.)
- While the overall philosophy might be that smart contracts are permissionless, in reality, they do indeed have different permissions/roles for different actors who interact/use them.
- The general classification is that of users and admin(s). For purposes of guarded launch or otherwise, many smart contracts have an admin role that is typically the address that deployed the contract. Admins typically have control over critical configuration and application parameters including (emergency) transfers/withdrawals of contract funds.
- Starting with understanding the access control implemented by the smart contracts and checking if they have applied correctly, completely and consistently is a good approach to understanding access flow and detecting violations

### 92. Starting with asset flow:

Assets are Ether or ERC20/ERC721/other tokens managed by smart contracts. Given that exploits target assets of value, it makes sense to start evaluating the flow of assets into/outside/within/across smart contracts and their dependencies.
- Who: Assets should be withdrawn/deposited only by authorised/specified addresses as per application logic
- When: Assets should be withdrawn/deposited only in authorised/specified time windows or under  authorised/specified  conditions as per application logic (when)
- Which: Assets, only those authorised/specified types, should be withdrawn/deposited as per application logic
- Why: Assets should be withdrawn/deposited only for authorised/specified reasons as per application logic
- Where: Assets should be withdrawn/deposited only to authorised/specified addresses as per application logic
- What type: Assets, only of authorised/specified types, should be withdrawn/deposited as per application logic
- How much: Assets, only in authorised/specified amounts, should be withdrawn/deposited as per application logic

### 93. Evaluating control flow:

Control flow analyzes the transfer of control, i.e. execution order, across and within smart contracts.
- Interprocedural (procedure is just another name for a function) control flow is typically indicated by a call graph which shows which functions (callers) call which other functions (callees), across or within smart contracts
- Intraprocedural (i.e. within a function) control flow is dictated by conditionals (if/else), loops (for/while/do/continue/break) and return statements.
- Both intra and interprocedural control flow analysis help track the flow of execution and data in smart contracts

### 94. Evaluating data flow:

Data flow analyzes the transfer of data across and within smart contracts
- Interprocedural data flow is evaluated by analyzing the data (variables/constants) used as argument values for function parameters at call sites
- Intraprocedural data flow is evaluated by analyzing the a assignment and use of (state/memory/calldata) variables/constants along the control flow paths within functions.
- Both intra and interprocedural data flow analysis help track the flow of global/local storage/memory changes in smart contracts

### 95. Inferring constraints:

Program constraints are basically rules that should be followed by the program. Language-level and EVM-level security constraints are well-known because they are part of the language and EVM specification. However, application-level constraints are rules that are implicit to the business logic implemented and may not be explicitly described in the specification e.g. mint an ERC-721 token to the address when it makes a certain deposit of ERC-20 tokens to the smart contract and burn it when it withdraws the earlier deposit. Such constraints may have to be inferred by the auditors while manually analyzing the smart contract code.
- One approach to inferring program constraints is to evaluate what is being done on most program paths related to a particular logic and treat it as a constraint. If such a constraint is missing on one or very few program paths then it could be an indicator of a vulnerability (assuming the constraint is security-related) or those program paths are exceptional conditions where the constraints do not need to hold.
- Program constraints can also be verified using a symbolic checker which generates counter-examples or witnesses along execution paths where such constraints do not hold.

### 96. Understanding dependencies:

Dependencies exist when the correct compilation or functioning of program code relies on code/data from other smart contracts that were not necessarily developed by the project team.
- Explicit program dependencies are captured in the import statements and the inheritance hierarchy. For e.g., many projects use the community-developed, audited and time-tested smart contracts from OpenZeppelin for tokens, access control, proxy, security etc.
- Composability is expected and encouraged via smart contracts interfacing with other protocols and vice-versa, which results in emergent or implicit dependencies on the state/logic of external smart contracts via oracles for example.
- This is especially of interest/concern in DeFi protocols that rely on other related protocols for stablecoins, yield generation, borrowing/lending, derivatives, oracles etc.

### 97. Evaluating assumptions:

Many security vulnerabilities result from faulty assumptions e.g. who can access what and when, under what conditions, for what reasons etc. Identifying the assumptions made by the program code and evaluating if they are indeed correct can be the source of many audit findings. Some common examples of faulty assumptions are:
- Only admins can call these functions
- Initialization functions will only be called once by the contract deployer (e.g. for upgradeable contracts)
- Functions will always be called in a certain order (as expected by the specification)
- Parameters can only have non-zero values or values within a certain threshold e.g. addresses will never be zero valued
- Certain addresses or data values can never be attacker controlled. They can never reach program locations where they can be misused. (In program analysis literature, this is known as taint analysis)
- Function calls will always be successful and so checking for return values is not required

### 98. Evaluating security checklists:

Checklists are lists of itemized points that can be quickly and methodically followed (and referenced later by their list number) to make sure all listed items have been processed according to the domain of relevance.
- This checklist-based approach was made popular in the book “The Checklist Manifesto. How to Get Things Right” by Atul Gawande who is a noted surgeon, writer and public health leader.
- Given the mind-boggling complexities of the fast-evolving Ethereum infrastructure (new platforms, new languages, new tools and new protocols) and the risks associated with deploying smart contracts managing millions of dollars, there are so many things to get right with smart contracts that it is easy to miss a few checks, make incorrect assumptions or fail to consider potential situations. Smart contract experts therefore need checklists too.
- Smart contract security checklists (such as the articles in this series) help in navigating the vast number of key aspects to be remembered and applied. They help in going over the itemized features, concepts, pitfalls, best-practices and examples in a methodical manner without missing any items. Checklists are known to increase retention and have a faster recall. They also help in referencing specific items of interest e.g. #42 in Security Pitfalls & Best Practices 101 or #98 in Audit Techniques & Tools 101.

> “Gawande begins by making a distinction between errors of ignorance (mistakes we make because we don’t know enough), and errors of ineptitude (mistakes we made because we don’t make proper use of what we know). Failure in the modern world, he writes, is really about the second of these errors, and he walks us through a series of examples from medicine showing how the routine tasks of surgeons have now become so incredibly complicated that mistakes of one kind or another are virtually inevitable: it’s just too easy for an otherwise competent doctor to miss a step, or forget to ask a key question or, in the stress and pressure of the moment, to fail to plan properly for every eventuality. Gawande then visits with pilots and the people who build skyscrapers and comes back with a solution. Experts need checklists–literally–written guides that walk them through the key steps in any complex procedure. In the last section of the book, Gawande shows how his research team has taken this idea, developed a safe surgery checklist, and applied it around the world, with staggering success.” - Malcolm Gladwell's review of "The Checklist Manifesto"

### 99. Presenting proof-of-concept exploits:

Exploits are incidents where vulnerabilities are triggered by malicious actors to misuse smart contracts resulting, for example, in stolen/frozen assets
- Presenting proof-of-concepts of such exploits either in code or written descriptions of hypothetical scenarios make audit findings more realistic and relatable by illustrating specific exploit paths and justifying severity of findings
- Codified exploits should always be on a testnet, kept private and responsibly disclosed to project teams without any risk of being actually executed on live systems resulting in real loss of funds or access
- Descriptive exploit scenarios should make realistic assumptions on roles/powers of actors, practical reasons for their actions and sequencing of events that trigger vulnerabilities and illustrate the paths to exploitation

### 100. Estimating the likelihood and impact:

Likelihood indicates the probability of a vulnerability being discovered by malicious actors and triggered to successfully exploit the underlying weakness. Impact indicates the magnitude of implications on the technical and business aspects of the system if the vulnerability were to be exploited. Estimating if likelihood/impact are low/medium/high is non-trivial in many cases.
- If the exploit can be triggered by a few transactions manually without requiring much resources/access (e.g. not admin) and without assuming many conditions to hold true then the likelihood is evaluated as High. Exploits that require deep knowledge of the system workings, privileged roles, large resources or multiple edge conditions to hold true are evaluated as Medium likelihood. Others that require even harder assumptions to hold true, miner collusion, chain forks or insider collusion for e.g., are considered as Low likelihood.
- If there is any loss or locking up of funds then the impact is evaluated as High. Exploits that do not affect funds but disrupt the normal functioning of the system are typically evaluated as Medium. Anything else is of Low impact.
- Many likelihood and impact evaluations are contentious and debatable between the audit and project teams, typically with security-conscious audit teams pressing for higher likelihood and impact and project teams downplaying the risks.

Estimating the severity:

Severity, per OWASP, is a combination of likelihood and impact. With reasonable evaluations of those two, severity estimates from the OWASP matrix should be straightforward.

### 101. Summary:

Audits are a time, resource and expertise bound effort where trained experts evaluate smart contracts using a combination of automated and manual techniques to find as many vulnerabilities as possible. Audits can show the presence of vulnerabilities but not their absence.
