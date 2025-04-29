# NEAR Agent Challenge System: A Blockchain-Powered Autonomous Competitive Intelligence Platform

## Project Overview

This project is a NEAR blockchain smart contract designed for a dynamic, interactive agent-based challenge system. The contract manages a competitive game-like environment where participants can submit challenges and attempt to dethrone the current "champion" by proposing stronger solutions or strategies.

### Key Features

- **Champion Management**: Tracks a current champion and allows participants to challenge the existing champion
- **Request-Response Mechanism**: Implements a structured system for submitting challenges and processing responses
- **Flexible Agent Interaction**: Supports dynamic system prompts and agent-based decision making
- **Blockchain-Powered Governance**: Leverages NEAR blockchain for secure, transparent challenge management

### Core Functionality

The contract enables users to:
- Submit messages/challenges to the current champion
- Track and manage a list of all historical champions
- Validate and process challenge responses through an operator
- Maintain a stateful system that evolves based on challenge outcomes

### Technical Highlights

- Built with Rust and NEAR SDK
- Uses advanced Rust collections like `UnorderedMap` and `UnorderedSet`
- Implements custom event logging for transparency
- Supports gas-efficient promise-based asynchronous interactions
- Provides robust input validation and error handling

## Getting Started, Installation, and Setup

### Prerequisites

- Rust (stable version, recommended via [rustup](https://rustup.rs/))
- Cargo (included with Rust)
- NEAR CLI or NEAR Wallet for contract deployment

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Install Rust dependencies:
   ```bash
   rustup default stable
   rustup target add wasm32-unknown-unknown
   ```

### Building the Contract

To compile the smart contract to WebAssembly:

```bash
cargo build --target wasm32-unknown-unknown --release
```

### Deployment

1. Ensure you have a NEAR account with sufficient funds for deployment
2. Deploy the contract using NEAR CLI:
   ```bash
   near deploy <YOUR_ACCOUNT_ID> ./target/wasm32-unknown-unknown/release/contract.wasm
   ```

### Quick Start

#### Viewing Current State

To view the current champion and game state:
```bash
near view <CONTRACT_ID> get_champion
near view <CONTRACT_ID> get_champion_owner
```

#### Submitting a Challenge

Submit a challenge to the current champion:
```bash
near call <CONTRACT_ID> request '{"message": "your_challenge"}' --accountId your.near
```

### Development

For local development and testing:
```bash
cargo test     # Run unit tests
cargo clippy   # Run linter
```

### Notes

- This contract is designed for the NEAR blockchain
- Requires NEAR SDK version 5.7.0
- Contract is written in Rust, edition 2018
- Compiled as a WebAssembly library (cdylib)

## API Reference

### Public Types

#### `CryptoHash`
- Type alias for a 32-byte array representing a cryptographic hash
- Defined as: `pub type CryptoHash = [u8; 32]`

### Structs

#### `Request`
Represents a request made to the agent
- Fields:
  - `data_id`: `CryptoHash` - Unique identifier for the request
  - `originator_id`: `AccountId` - The account that initiated the request
  - `message`: `String` - The request message

#### `Response`
Represents a response from the agent
- Fields:
  - `ok`: `bool` - Indicates if the response was successful
  - `data`: `Option<String>` - Optional response data
  - `signature`: `Option<String>` - Optional response signature

#### `ResponseMsg`
Internal message structure for parsing agent responses
- Fields:
  - `current_champion`: `String` - Current champion name
  - `guess_wins`: `bool` - Whether the guess was successful
  - `reason`: `String` - Explanation of the result

#### `AgentData`
Contextual data for agent processing
- Fields:
  - `request`: `Request` - The original request
  - `champions`: `Vec<String>` - List of all champions
  - `prompt`: `String` - System prompt for the agent

### Contract Methods

#### Initialization
```rust
pub fn new(
    owner_id: AccountId,
    operator_id: AccountId,
    initial_champion: String,
    agent_name: String,
    agent_system_prompt: String,
    agent_public_key: String
) -> Self
```
Creates a new contract instance
- Parameters:
  - `owner_id`: Account of the contract owner
  - `operator_id`: Account with operational permissions
  - `initial_champion`: Starting champion name
  - `agent_name`: Name of the agent
  - `agent_system_prompt`: Initial system prompt
  - `agent_public_key`: Public key of the agent

#### Query Methods
```rust
pub fn get_all_champions() -> Vec<String>
```
Retrieves a list of all champions

```rust
pub fn get_champion() -> String
```
Returns the current champion

```rust
pub fn get_champion_owner() -> AccountId
```
Returns the owner of the current champion

```rust
pub fn get_request(request_id: RequestId) -> Request
```
Retrieves a specific request by its ID

```rust
pub fn get_question() -> String
```
Generates a question about beating the current champion

```rust
pub fn get_requests() -> Vec<(RequestId, Request)>
```
Lists all existing requests

```rust
pub fn agent_data(request_id: RequestId) -> AgentData
```
Retrieves agent-specific data for a given request

#### Operational Methods
```rust
pub fn set_system_prompt(prompt: String)
```
Updates the agent's system prompt (operator-only)

```rust
pub fn request(message: String)
```
Submits a new request to the agent
- Validates input message
- Generates a unique request ID
- Emits a run_agent event

```rust
pub fn respond(
    data_id: CryptoHash, 
    request_id: RequestId, 
    response: Response
)
```
Processes a response for a specific request (operator-only)
- Validates and stores the response
- Resumes the promise yield

```rust
pub fn remove_request(request_id: RequestId)
```
Removes a specific request and its response (operator-only)

### Utility Functions
```rust
fn remaining_gas() -> Gas
```
Calculates the remaining gas for a transaction

```rust
fn is_valid_string(input: &str) -> bool
```
Validates that a string contains only lowercase ASCII characters

## Project Structure

The project is structured as a Rust-based smart contract using the NEAR SDK, with the following key directories and files:

### Source Code Structure
- `src/`: Main source code directory
  - `lib.rs`: Core contract implementation containing the primary contract logic, data structures, and methods
  - `events.rs`: Likely handles event-related functionality 
  - `utils.rs`: Utility functions and helper methods

### Build Configuration
- `Cargo.toml`: Rust project configuration file defining:
  - Project metadata
  - Dependencies
  - Build profiles
  - Compilation settings for NEAR smart contract development

### Development Utilities
- `build_local.sh`: Shell script potentially used for local build or deployment processes

### Key Architectural Components
- Smart contract defines a game-like mechanism with concepts like:
  - Requests and responses
  - Champions
  - Agent-based interactions
  - Operator and owner roles

### Contract Features
- Uses NEAR SDK collections like `UnorderedMap` and `UnorderedSet`
- Implements request-response pattern
- Supports champion management
- Includes system for tracking and processing requests

## Technologies Used

#### Programming Language
- Rust (Edition 2018)

#### Blockchain and Smart Contract Framework
- NEAR SDK (v5.7.0)
- NEAR Contract Standards (v5.7.0)

#### Serialization and Parsing
- Schemars (v0.8) - JSON schema generation
- Serde JSON (v1.0.133) - JSON serialization/deserialization
- Borsh - Binary Object Representation Serializer for Hashing

#### Data Structures
- near-sdk Collections:
  - UnorderedSet
  - UnorderedMap
  - LookupMap

#### Development and Testing Tools
- Cargo - Rust package manager and build tool
- Insta (v1.31.0) - Snapshot testing
- Regex (v1) - Regular expression support
- Near Units (v0.2.0) - Testing utilities

#### Compilation Profile
- Optimized for WebAssembly (WASM) deployment
- Focused on minimal binary size and performance
  - Single codegen unit
  - Zero-level optimization
  - Link-time optimization (LTO)
  - Panic abort strategy
  - Overflow checks enabled

#### Additional Libraries
- Uint (v0.9.0) - Unsigned integer type handling
- Anyhow (v1.0) - Error handling

## Additional Notes

### Performance Considerations

The contract is designed with gas efficiency in mind, with predefined minimum gas requirements for requests and responses:
- Minimum Request Gas: 40 TGas
- Minimum Response Gas: 40 TGas

### Contract State Management

The contract uses NEAR SDK's collections for efficient on-chain state management:
- `UnorderedMap` for tracking requests and responses
- `UnorderedSet` for maintaining a history of champions
- Persistent storage is managed through Borsh serialization

### Error Handling and Validation

The contract includes several built-in validation mechanisms:
- Input string validation to prevent illegal inputs
- Gas availability checks before processing requests
- Strict validation of AI agent responses
- Detailed logging for successful and unsuccessful challenge attempts

### Extensibility

The architecture supports future enhancements through:
- Configurable system prompts
- Flexible champion and request tracking
- Modular design with separate modules for events and utilities

### Security Mechanisms

- Operator-only methods for critical contract functions
- Immutable data storage for transparency
- Public key verification for autonomous AI agent
- Request and response tracking with unique identifiers

## Contributing

We welcome contributions to this autonomous AI smart contract project! To ensure the quality and integrity of the project, please follow these guidelines:

### Code Contributions

- Ensure all contributions align with the project's autonomous AI and on-chain data storage principles
- All new features must maintain the core architecture of on-chain data storage and off-chain AI inference
- Contributions should enhance the transparency, security, and autonomy of the system

### Development Requirements

- Use Rust programming language (edition 2018)
- Follow Rust best practices and idiomatic code standards
- Ensure compatibility with NEAR SDK version 5.7.0
- Use `near-contract-standards` for standard implementations

### Testing

- All contributions must include comprehensive unit tests
- Use `near-units` for testing NEAR-specific functionality
- Utilize `insta` for snapshot testing, particularly for JSON and complex data structures
- Ensure all existing tests pass before submitting a pull request

### Code Style

- Format code using standard Rust formatting tools
- Write clear, concise comments explaining complex logic
- Maintain the existing architectural patterns of on-chain state management
- Keep the contract methods clean and focused on their specific responsibilities

### Submitting Contributions

1. Fork the repository
2. Create a feature branch
3. Implement your changes
4. Write and run tests
5. Submit a pull request with a clear description of your changes

### Security Considerations

- All contributions will be reviewed for potential security implications
- Ensure that any new features maintain the contract's existing security model
- Be prepared to discuss and validate the security of proposed changes

### Documentation

- Update README and inline documentation to reflect any significant changes
- Provide clear examples of how new features interact with the existing system

By contributing, you agree that your code will be licensed under the project's existing license terms.

## License

The project is currently unlicensed, which means that no permissions are granted for reproduction, distribution, or modification of the source code. By default, this means that the copyright remains with the original authors, and others do not have the right to use, copy, modify, or distribute the software without explicit permission.

### Implications
- No one can legally use, copy, modify, or distribute the code
- All rights are reserved by the copyright holders
- Potential users should contact the project owners for any usage permissions

It is strongly recommended that the project maintainers choose and add an appropriate open-source license to clarify usage rights and encourage collaboration.