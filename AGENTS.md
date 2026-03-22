# AGENTS.md - Awcrypt Project Guidelines

## Project Overview

This is an Elixir library project named **awcrypt**. It uses:
- **Build tool**: Mix (Elixir's built-in build tool)
- **Test framework**: ExUnit
- **Formatter**: mix format (via .formatter.exs)

## Build/Lint/Test Commands

### Running Tests
```bash
# Run all tests
mix test

# Run a single test file
mix test test/awcrypt_test.exs

# Run a specific test by line number
mix test test/awcrypt_test.exs:5

# Run tests with coverage
mix test --cover
```

### Code Quality
```bash
# Format all code (runs automatically on save in most editors)
mix format

# Check formatting without modifying files
mix format --check-formatted

# Run all compile-time warnings
mix compile --warnings-as-errors
```

### Dependency Management
```bash
# Install dependencies
mix deps.get

# Update dependencies
mix deps.update --all

# Clean build artifacts
mix clean
```

### Documentation
```bash
# Generate documentation (requires ExDoc)
mix docs
```

### Full Build
```bash
# Compile the project
mix compile

# Run the project (if it has a runnable main)
mix run
```

## Code Style Guidelines

### General Principles
- Follow the official Elixir style guide: https://github.com/christopheradams/elixir_style_guide
- Use `mix format` for automatic formatting (configured in `.formatter.exs`)
- Keep lines under 98 characters when reasonable
- Use 2-space indentation

### Module Organization
```elixir
defmodule MyModule do
  @moduledoc """
  Module-level documentation using @moduledoc.
  Include examples with `iex>` code blocks.
  """

  @doc """
  Function documentation. Use @doc for public APIs.
  Include @spec for type specifications.
  """
  @spec my_function(String.t(), integer()) :: :ok | {:error, String.t()}
  def my_function(arg1, arg2) do
    # implementation
  end
end
```

### Naming Conventions
- **Modules**: `PascalCase` (e.g., `Awcrypt`, `MyModule`)
- **Functions/variables**: `snake_case` (e.g., `my_function`, `user_data`)
- **Constants**: `SCREAMING_SNAKE_CASE` (e.g., `@DEFAULT_TIMEOUT`)
- **Private functions**: prefixed with `p_` or suffixed with `_p` if needed for disambiguation
- **Test functions**: prefixed with `test_` (e.g., `test "does something" do`)

### Imports and Aliases
- List imports at the top of modules, grouped:
  1. `import` statements
  2. `alias` statements
  3. `require` statements
- Prefer `alias` over `import` when only a few functions are needed
- Avoid wildcard imports (`import Something.*`) except for test files

### Types and Specs
- Always include `@spec` for public functions
- Use typespecs for complex parameter/return types
- Use `@type` for custom types that are used multiple times
- Example:
  ```elixir
  @type result :: {:ok, term()} | {:error, String.t()}
  
  @spec my_func(integer(), String.t()) :: result()
  ```

### Error Handling
- Use pattern matching in function heads for control flow
- Use `with` for sequential operations that may fail
- Return `{:ok, value}` or `{:error, reason}` tuples for expected failures
- Raise exceptions only for unexpected/programming errors
- Use `defguard` or `defguardp` for complex boolean conditions

### Documentation
- Always document modules with `@moduledoc`
- Document public functions with `@doc`
- Use Markdown formatting in docstrings
- Include usage examples for complex functions

### Pipe Operator
- Use `|>` for transforming data through multiple functions
- Avoid nesting pipes; break into multiple lines if needed
- Keep the pipeline readable by using descriptive variable names

### Pattern Matching
- Use pattern matching in function heads when appropriate
- Use `case` for more complex conditionals
- Leverage guards for type/value constraints

### Testing Best Practices
- Use `describe` blocks to group related tests
- Use clear, descriptive test names (e.g., `test "returns error when input is invalid"`)
- Use `setup` callbacks for common test setup
- Mock sparingly; prefer integration tests where possible
- Use `doctest` to test examples in documentation

### File Structure
```
lib/
  awcrypt.ex          # Main module
  awcrypt/
    some_module.ex    # Sub-modules in directory

test/
  awcrypt_test.exs    # Tests for main module
  awcrypt/
    some_module_test.exs  # Tests in matching directory
  test_helper.exs     # Test configuration
```

### Strings and Binaries
- Use double-quoted strings `""` for regular strings
- Use `~w()` sigils for word lists
- Use `~c()` sigils for charlists
- Use `~S` for raw strings (no escape sequences)

### Collections
- Use `Enum` for iterating and transforming collections
- Use `Kernel` functions for simple operations
- Use `Stream` for lazy enumerations
- Pattern match on empty vs non-empty lists when needed

## Editor Configuration

Recommended settings for VS Code (`.vscode/settings.json`):
```json
{
  "[elixir]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "JakeBecker.elixir-ls"
  }
}
```

## Dependencies

This project has no external dependencies. Add any new dependencies to `mix.exs`:
```elixir
defp deps do
  [
    {:some_package, "~> 1.0"}
  ]
end
```

## Reference Documents

This project reimplements core Ethereum execution-layer logic in Elixir. Reference the following primary sources (oldest foundational documents first):

- **Ethereum Yellow Paper** (original formal specification, 2014 onward, Shanghai version maintained)  
  https://ethereum.github.io/yellowpaper/paper.pdf  
  Defines the core EVM, state transition function, gas model, and transaction semantics that awcrypt must faithfully reproduce.

- **Ethereum Execution Layer Specifications** (current executable reference spec in Python)  
  https://github.com/ethereum/execution-specs  
  Tracks network upgrades (hard forks). Use as the ground-truth for EVM behavior, block processing, and fork rules. Test vectors: https://github.com/ethereum/execution-spec-tests (historical; newer tests integrate into execution-specs).

- **go-ethereum Repository** (reference Go implementation)  
  https://github.com/ethereum/go-ethereum  
  - Main README: project overview, executables (geth, clef, evm, abigen), build instructions  
  - Source packages:  
    - EVM: https://github.com/ethereum/go-ethereum/tree/master/core/vm  
    - Chain/state processing: https://github.com/ethereum/go-ethereum/tree/master/core  
    - Trie/world state: https://github.com/ethereum/go-ethereum/tree/master/trie  
    - Networking (devp2p): https://github.com/ethereum/go-ethereum/tree/master/p2p  
    - Chain configs/forks: https://github.com/ethereum/go-ethereum/blob/master/params/config.go  
  - Latest release (as of March 2026): v1.17.1

- **Geth Official Documentation**  
  https://geth.ethereum.org/docs/  
  - Command-line options: https://geth.ethereum.org/docs/fundamentals/command-line-options  
  - JSON-RPC / Engine API: https://geth.ethereum.org/docs/interacting-with-geth/rpc (see also canonical spec at https://github.com/ethereum/execution-apis)

- **Security & Audits**  
  Security policy: https://github.com/ethereum/go-ethereum/blob/master/SECURITY.md  
  (Report via https://bounty.ethereum.org or bounty@ethereum.org; always use latest release)  
  Audit reports: https://github.com/ethereum/go-ethereum/tree/master/docs/audits  
  - 2017-04-25 Geth (Truesec)  
  - 2018-09-14 Clef (NCC)  
  - 2019-10-15 Discv5 (LeastAuthority)  
  - 2020-01-24 Discv5 (Cure53)

Include any deviations from these specs in module-level `@moduledoc` or a dedicated `DEVIATIONS.md` file.
```
