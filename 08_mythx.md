### 56. MythX

MythX is a powerful security analysis service that finds Solidity vulnerabilities in your Ethereum smart contract code during your development life cycle. It is a paid API-based service which uses several tools on the backend including a static analyzer (Maru), symbolic analyzer (Mythril) and a greybox fuzzer (Harvey) to implement a total of 46 detectors. Mythril is the open-source component of MythX.

### 57. MythX process:

1. Submit your code: The analysis requests are encrypted with TLS and the code you submit is accessed only by you. Submit both the source code and the compiled bytecode of your smart contracts for best results.
2. Activate a full suite of analysis techniques: The longer MythX runs, the more it can detect more security weaknesses.
3. Receive a detailed analysis report: MythX detects a majority of vulnerabilities listed in the SWC Registry. The report will return a listing of all the weaknesses found in your code, including the exact position of the issue and its SWC ID. Reports generated can be only accessed by you. MythX offers 3 scan modes, quick, standard and deep. You can see the differences here.

### 58. MythX tools:

When you submit your code to the API it gets analyzed by multiple microservices in parallel where these tools cooperate to return the more comprehensive results in the execution time provided.

A static analyzer that parses the Solidity AST

a symbolic analyzer that detects possible vulnerable states, and

a greybox fuzzer that detects vulnerable execution paths

### 59. MythX coverage:

*MythX* extends to most SWCs found in the SWC Registry with the 46 detectors listed here along with the type of analysis used.

### 60. MythX SaaS

MythX is based on a **SaaS** (Security as a Service) platform based on the premise that:
- Higher performance compared to running security tools locally
- Higher vulnerability coverage than any standalone tool
- Benefit from continuous improvements to our security analysis technology with new and improved security tests as the smart contract security landscape evolves.

### 61. MythX Privacy Guarantee

MythX privacy guarantee for the smart contract code submitted using their SaaS APIs:
- Code analysis requests are encrypted with TLS
- To provide comprehensive reports and improve performance, it stores some of the contract data in our database, including parts of the source code and bytecode but that data never leaves their secure server and is not shared with any outside parties.
- It keeps the results of your analysis so you can retrieve them later, but the report can be accessed by you only.

### 62. MythX running time:

- Quick scan: 5 minutes
- Standard scan: 30 minutes
- Deep scan: 90 minutes

### 63. MythX integrations, tools, and libraries

MythX official integrations, tools, and libraries include:
- **MythX CLI**: Unified tool to use MythX as a Command Line Interface (CLI) now with full Truffle Projects support.
- **MythX-JS**: Typescript library to integrate MythX in your JS or TS projects.
- **PythX**: Python library to integrate MythX in your Python projects.
- **MythX VSCode**: VSCode extension which allows you to scan smart-contracts and view results directly from your code editor.

### 64. MythX pricing:

| Package | Price | Scans | Description |
| - | - | - |
| On Demand | US$9.99/3 scans | On Demand | All scan modes and Prepaid scan packs |
| Developer | US$49/mo | 500 scans/month | Quick and Standard scan modes |
| Professional | US$249/mo | 10000 scans/month | All scan modes |
| Enterprise | Custom pricing | Custom | Custom plans for your team's specific needs; Custom Verification Service; Retainer for Custom Support |
