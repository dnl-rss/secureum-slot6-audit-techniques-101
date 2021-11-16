### 48. Eth security toolbox

`Eth-security-toolbox` is a Docker container preinstalled and preconfigured with all of Trail of Bits’ Ethereum security tools.

This includes:
- Echidna property-based fuzz tester
- Etheno integration tool and differential tester
- Manticore symbolic analyzer and formal contract verifier
- Slither static analysis tool
- Rattle EVM lifter
- Not So Smart Contracts repository

### 49. Ethersplay

*Ethersplay* is a Binary Ninja plugin which enables an *EVM disassembler* and related analysis tools.
- Takes input of evm bytecode in raw binary format
- Renders control flow graph of all functions
- Shows Manticore coverage

### 50. Pyevmasm

*Pyevmasm* is an *assembler and disassembler library* for the EVM.

It includes a command line utility and a Python API.

### 51. Rattle

*Rattle* is an *EVM binary static analysis framework* designed to work on deployed smart contracts (not actively developed anymore).
- Takes EVM byte strings and uses a flow-sensitive analysis to recover the original control flow graph
- Lifts the control flow graph into an SSA/infinite register form, and optimizes the SSA – removing DUPs, SWAPs, PUSHs, and POPs
- The conversion from a stack machine to SSA form removes 60%+ of all EVM instructions and presents a much friendlier interface to those who wish to read the smart contracts they’re interacting with

### 52. EVM cfg builder

`Evm_cfg_builder` is a tool used to extract a control flow graph (CFG) from EVM bytecode and used by Ethersplay, Manticore, and other tools from Trail of Bits.
- Reliably recovers a Control Flow Graph (CFG) from EVM bytecode using a dedicated Value Set Analysis
- Recovers functions names
- Recovers attributes (e.g., payable, view, pure)
- Outputs the CFG to a dot file
- Library API

### 53. Crytic-compile

`crytic-compile` is a smart contract compilation library which is used in Trail of Bits’ security tools and supports Truffle, Embark, Etherscan, Brownie, Waffle, Hardhat and other development environments.

The plugin is used in Crytic tools, including:
- Slither
- Echidna
- Manticore
- evm-cfg-builder

### 54. Solc select

`solc-select` is a script to quickly switch between Solidity compiler versions.
- `solc-select`: manages installing and setting different solc compiler versions
- `solc`: wrapper around `solc` which picks the right version according to what was set via `solc-select`
- solc binaries are downloaded from https://binaries.soliditylang.org/ which contains official artifacts for many historical and modern solc versions for Linux and macOS

### 55. Etheno

Etheno is the Ethereum testing Swiss Army knife. It’s a JSON RPC multiplexer, analysis tool wrapper, and test integration tool.
- **JSON RPC Multiplexing**: Etheno runs a JSON RPC server that can multiplex calls to one or more clients:
    1. API for filtering and modifying JSON RPC calls
    2. Enables differential testing by sending JSON RPC sequences to multiple Ethereum clients
    3. Deploy to and interact with multiple networks at the same time
- **Analysis Tool Wrapper**: Etheno provides a JSON RPC client for advanced analysis tools like Manticore
    1. Lowers barrier to entry for using advanced analysis tools
    2. No need for custom scripts to set up account and contract state
    3. Analyze arbitrary transactions without Solidity source code
- **Integration with Test Frameworks**: i.e. Ganache and Truffle 
    1. Run a local test network with a single command
    2. Use Truffle migrations to bootstrap Manticore analyses
    3. Symbolic semantic annotations within unit tests
