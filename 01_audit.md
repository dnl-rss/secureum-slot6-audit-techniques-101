### 1. Audit:

An *audit* is an *external security assessment* of a project codebase, typically requested and paid-for by the project team.
- detects and describes (in a report) security issues with underlying vulnerabilities, severity/difficulty, potential exploit scenarios and recommended fixes.
- provides subjective insights into code quality, documentation and testing.
- scope/depth/format of audit reports varies across auditing teams but they generally cover similar aspects.

### 2. Audit Scope:

For Ethereum-based smart-contract projects, the *scope* is typically the *on-chain smart contract code*
- sometimes includes the *off-chain components* that interact with the smart contracts.

### 3. Audit Goal:

The *goal* of audits is to *assess project code* (with any associated specification, documentation) and alert project team, typically before launch, of potential security-related issues that need to be addressed to *improve security posture*, decrease attack surface and mitigate risk.

### 4. Audit Non-goal:

An audit is **not** a *security guarantee* of “bug-free” code
- rather, it is a best-effort endeavour by trained security experts operating within reasonable constraints of time, understanding, expertise and of course, decidability.

### 5. Audit Target:

Security companies execute audits for *clients who pay for their services*.
- engagements are geared towards priorities of project owners

Audits are **not intended** to alert potential *project users of any inherent risk*.

### 6. Audit Need:

Smart contract projects do not have enough *in-house security expertise* and/or *time to perform internal security assessments*
- therefore, they rely on *external experts* who have domain expertise in those areas.

Even if projects have some expertise in-house, they would still benefit from an unbiased external team with supplementary/complementary skill sets that can review the assumptions, design, specification and implementation of the project codebase.

### 7. Audit Types:

The *type of audit* generally falls into the following categories:
1. **New audit**: for a *new project* that is being launched
2. **Repeat audit**: for a *new version* of an existing project
3. **Fix audit**: for *reviewing the fixes* made to the findings from a current/prior audit
4. **Retainer audit**: for *constantly reviewing* project updates
5. **Incident audit**: for *reviewing an exploit incident*, root cause of the incident, identifying the underlying vulnerabilities, and proposing fixes.

### 8. Audit Timeline:

The *audit timeline* may vary from a *few days* for a fix/retainer audit to *several weeks* for a new/repeat/incident audit.

### 9. Audit Effort:

The *audit effort* typically involves *more than one auditor* simultaneously for getting independent, redundant or supplementary/complementary assessment expertise on the project.

### 10. Audit Costs:

The *cost of an audit* typically exceeds *USD $10K/week* depending on:
- the complexity of the project
- market demand/supply for audits
- the strength/reputation of the auditing firm.

### 11. Audit Prerequisites:

*Clear definition of the scope* of the project to be assessed, typically in the form of a specific commit hash of project files/folders on a github repository
- Public/private repository
- Public/anonymous team
- Specification of the project’s design and architecture
- Documentation of the project’s implementation and business logic
- Threat models and specific areas of concern
- Prior testing, tools used, other audits
- Timeline, effort and costs/payments
- Engagement dynamics/channels for questions/clarifications, findings communication and reports
- Points of contact on both sides

### 12. Audit Limitations:

Audits are necessary (for now at least) but not sufficient:

- There is *risk reduction*, but *residual risk* exists because of several factors
    - limited amount of audit time/effort,
    - limited insights into project specification/implementation,
    - limited security expertise in the new and fast evolving technologies
    - limited audit scope,
    - significant project complexity
    - limitations of automated/manual analysis
- Not all audits are equal
    - expertise/experience of auditors
    - effort invested vis-a-vis project complexity/quality
    - tools/processes used
- Audits provide a project’s security snapshot over a brief (typically few weeks) period
    - however, smart contracts need to evolve over time to add new features, fix bugs or optimize
    - relying on external audits after every change is impractical.

### 13. Audit Reports:

*Audit reports* include details of:
- scope
- goals
- effort
- timeline
- approach
- tools/techniques used
- findings summary
- vulnerability details
- vulnerability classification
- vulnerability severity/difficulty/likelihood
- vulnerability exploit scenarios
- vulnerability fixes
- informational recommendations/suggestions on programming best-practices.

### 14. Audit Findings Classification:

The *vulnerabilities found* during the audit are typically classified into different categories which helps to understand the nature of the vulnerability, potential impact/severity, impacted project components/functionality and exploit scenarios.

Trail of Bits uses the below classification of vulnerabilities:
- **Access Controls**: Related to *authorization* of users and assessment of rights
- **Auditing and Logging**: Related to *auditing of actions* or *logging of problems*
- **Authentication**: Related to the *identification* of users
- **Configuration**: Related to *security configurations* of servers, devices, or software
- **Cryptography**: Related to *protecting data* for its privacy or integrity
- **Data Exposure**: Related to unintended *exposure of sensitive data*
- **Data Validation**: Related to improper *reliance on data structure or values*
- **Denial of Service**: Related to causing *system failure*
- **Error Reporting**: Related to the secure *reporting of error conditions*
- **Patching**: Related to *updating software*
- **Session Management**: Related to the *identification of authenticated user*
- **Timing**: Related to *race conditions, locking, or order of operations*
- **Undefined Behavior**: Related to *undefined behavior* triggered by the program

### 15. Audit Findings Likelihood/Difficulty:

OWASP = Open Web Application Security Project
- **likelihood**, or difficulty, is a rough measure of how likely this particular vulnerability is to be uncovered and exploited by an attacker.
- proposes three Likelihood levels: Low, Medium, and High.

Trail of Bits classifies every finding into four difficulty levels:
- **Undetermined**: difficulty of exploit was not determined during this engagement
- **Low**: commonly exploited, public tools exist or can be scripted that exploit this flaw
- **Medium**: attackers must write an exploit, or need an in-depth knowledge of a complex system
- **High**: attacker must have privileged insider access to the system, may need to know extremely complex technical details, or must discover other weaknesses in order to exploit this issue

### 16. Audit Findings Impact:

Per OWASP
- **Impact** estimates the magnitude of the technical and business impact on the system if the vulnerability were to be exploited.
- proposes three Impact levels: Low, Medium, and High.

### 17. Audit Findings Severity:

Per OWASP
- **Severity** considers both the *Likelihood* estimate and the *Impact* estimate.
- proposes a *3x3 Severity Matrix* which combines the three Likelihood levels with the three Impact levels

| Severity Matrix       | High Impact | Medium Impact | Low Impact |
|--------|----------|--------|--------|
| **High Likelihood**   | Critical | High   | Medium |
| **Medium Likelihood** | High     | Medium | Low    |
| **Low Likelihood**    | Medium   | Low    | Note   |

Trail of Bits uses:
- **Informational**: issue does not pose an immediate risk, but is relevant to security best practices or Defense in Depth
- **Undetermined**: extent of the risk was not determined during this engagement
- **Low**: risk is relatively small or is not a risk the customer has indicated is important
- **Medium**: individual user’s information is at risk, exploitation would be bad for client’s reputation, moderate financial impact, possible legal implications for client
- **High**: large numbers of users, very bad for client’s reputation, or serious legal or financial implications

ConsenSys uses:
- **Minor**: issues are subjective in nature.
    - typically suggestions around best practices or readability.
    - code maintainers should use their own judgment as to whether to address such issues.
- **Medium**: issues are objective in nature but are not security vulnerabilities.
    - should be addressed unless there is a clear reason not to.
- **Major**: issues are security vulnerabilities that may not be directly exploitable or may require certain conditions in order to be exploited.
    - all major issues should be addressed.
- **Critical**: issues are directly exploitable security vulnerabilities that need to be fixed.

### 18. Audit Checklist For Projects

(See here for Trail of Bits recommendations):

- [ ] Resolve the easy issues:
    1. Enable and address every last compiler warning
    2. Increase unit and feature test coverage
    3. Remove dead code, stale branches, unused libraries, and other extraneous weight.
- [ ] Document:
    1. Describe what your product does, who uses it, why, and how it delivers.
    2. Add comments about intended behavior in-line with the code.
    3. Label and describe your tests and results, both positive and negative.
    4. Include past reviews and bugs.
- [ ] Deliver the code, batteries included:
    1. Document the steps to create a build environment from scratch on a computer that is fully disconnected from your internal network
    2. Include external dependencies
    3. Document the build process, including debugging and the test environment
    4. Document the deployment process and environment, including all the specific versions of external tools and libraries for this process.

### 19. Audit Techniques:

involve a *combination of different methods* that are applied to the project codebase with accompanying specification and documentation.

Many are automated analyses performed with tools and some require manual assistance.
- Specification analysis (manual)
- Documentation analysis (manual)
- Testing (automated)
- Static analysis (automated)
- Fuzzing (automated)
- Symbolic checking (automated)
- Formal verification (automated)
- Manual analysis (manual)

One may also think of these as manual/semi-automated/fully-automated, where the distinction between semi-automated and fully-automated is the difference between a tool that requires a user to define properties versus a tool that requires (almost) no user configuration except to triage results.

Fully-automated tools tend to be straightforward to use, while semi-automated tools require some human assistance and are therefore more resource-expensive.
