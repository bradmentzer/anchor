# GM Anchor

This is a simple program built on the Solana network using Anchor. It takes in a string parameter, like a name, and returns GM to that string.

## Dependencies:

- [Rust](https://www.rust-lang.org/tools/install)
- [Solana Tool Suite](https://docs.solana.com/cli/install-solana-cli-tools)
- [Yarn](https://yarnpkg.com/getting-started/install)

- [Anchor version manager](https://book.anchor-lang.com/getting_started/installation.html)

## Solana

[Solana](https://medium.com/solana-labs/7-innovations-that-make-solana-the-first-web-scale-blockchain-ddc50b1defda) is an open source permissionless blockchain with 1500 ndes capable of 50k TPS.

Comprised of multiple clustors or networks

- Devnet
- Testnet
- Mainnet beta
- Localnet/Test validator

Uses a BFT PoS consensus mechanism and Proof of History
Program is an on-chain smart contract

- stateless, no global variables
  Program ID is the address/account the program is stored in
  Data for program is stroed in accounts
- Accoutn is a buffer
- Like a file in an O/S
- Included matadata that tells the runtime who is allowed to access the data and how
- Held in validator emory and pay 'rent' to stay

ERC-20 example: one contract stores token details and wallet balances. Solona has one deployed program that performs functions and multiple accounts that has balances. Logic with state is not stored in smart contract

lamports are how much Solana is in the account

Basic operation unit is an instruction, like a function call

Instructions:

- Program ID (logic)
- Accounts (location data is being modified)
- Instructional Data (array of input data)

  Transaction is list of instructions. Clients submit transactions and fetch data via RPC API.

Validators get paid transaction fees + inflationary rewards. Stakers are rewarded for helping to valiidate the ledger by delegating their stake to validator nodes. Inflation is currently 8%. Each transaction submitted to the ledger imposes costs. TX fees cover processing the transaction, not storing it. Rent is the cost to store data over time.

2 methods of storage rent:

- set and forget (prepay 2 years worth of rent)
- pay per byte

Sending data to a program via JSON RPC requires serialization and deserialization. If you need to send input parameter to solana, all information needs to be serialized into a byte array (no sending strings)

Serialization: converting an object in memory into a stream of bytes
Deserialization: converting a stream of bytes into a readable object in memory

# Anchor

[Anchor](https://github.com/coral-xyz/anchor) is a framework for quickly building secure Solana programs. It is a tool to increase productivity by abstracting away low level details.

Check out [The Anchor Book](https://book.anchor-lang.com/) for in depth documentation on the framework.

Framework built by project serum (DEX)

- Gererates boilerplate code
- Handles security checks

## Tools

- Rust smart contracts
- Interface Description Language (like ABI file in solidity)
- Client generators (off-chain program using JS)
- CLI (interacting and deploying programs)

Smart contracts (in rust) are built and compiled into byte code. Anchor generates IDL file which which says how to interact with the program. From IDL we generate clients and with clients we can interact with the program and test.

Anchor program consists of three parts:

- Program module
  - where program logic is written
- Account structs
  - where accounts are validated
- Declare ID marcro
  - stores the address of the program

### Accounts

Where you define which accounts your instruction expects and which constrains these accounts should follow

- Types
  - Various account types
- Constraints
  - Handles security and access control for accounts

### Program Module

- where buisness logic is defined
- write functions which can be called by clients or other programs
- each defines endpoint takes a Contect type as an argument

  - provides access to the accounts of the executing program and the program ID
  - achor can take in input parameters directly on functin definityions without serialization

### Instructin Data

- can be added by adding arguments to the function after the contect argument
- Anchor will automatically deserialize the instruction data into the arguments

### IDL

- Generates an IDL specification for programs
  - similar to EVM ABI
- Used by clients to be able to interact with depolyed Anchor-based programs
  - contains function definitions, what kind of accounts they are expecting, what kind of arguments they are expecting, ect...
- Greatly simplifies calling functions, sending and recieving data
- Acts as the connecting glue between the on-chain program and off chain clients
