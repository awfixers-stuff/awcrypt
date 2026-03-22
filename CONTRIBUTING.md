```md
# CONTRIBUTING.md - Awcrypt Project

Thank you for your interest in contributing to **awcrypt**, an Elixir reimplementation of core Ethereum execution-layer logic.

## Development Environment

We use **Nix** to provide a fully reproducible development environment with consistent tooling, Elixir version, dependencies, and formatting rules across all contributors.

### 1. Install Nix

If you do not already have Nix installed, follow the official single-user installation instructions (recommended for most developers):

```bash
# macOS / Linux (recommended method)
sh <(curl -L https://nixos.org/nix/install) --daemon
```

After installation, restart your terminal or source the profile:

```bash
. ~/.nix-profile/etc/profile.d/nix.sh
```

Verify:

```bash
nix --version
```

We require a recent version of Nix (2.20+ recommended).

### 2. Enter the Development Shell

Once Nix is installed, enter the project shell:

```bash
nix develop
```

This command does the following automatically:
- Installs the exact Elixir version pinned in `flake.nix`
- Installs Erlang/OTP
- Installs `mix`, `hex`, `rebar3`
- Sets up `mix format`, Dialyzer, and ExUnit
- Provides `git`, `direnv` (if enabled), and other utilities

You should now see `(awcrypt)` or similar in your shell prompt.

All subsequent commands (`mix test`, `mix format`, `mix deps.get`, etc.) should be run **inside** this shell.

Do **not** install Elixir globally or via asdf/brew/etc. — use only the version provided by `nix develop`.

## Code Writing & AI Assistance Policy

We maintain strict rules around code authorship and transparency:

1. **Only approved AI tool**: All AI-assisted code generation **must** use **Opencode** (our designated interface/tool).
   - No other LLMs, code completion tools, or AI assistants (including GitHub Copilot, Cursor, Claude, ChatGPT, etc.) are permitted.
   - This ensures consistent reasoning style, source referencing, and auditability.

2. **Session export requirement**:
   - After every significant coding session or contribution block using Opencode, **export the full conversation**.
   - Format the exported file exactly as:
     ```
     <your-username>-<sequential-chat-number>.md
     ```
     Examples:
     - `aw-001.md`
     - `aw-002.md`
     - `contributorxyz-015.md`

3. **Storage location**:
   - Place all exported session files in:
     ```
     .ai/sessions/
     ```
   - Create the directory if it does not exist:
     ```bash
     mkdir -p .ai/sessions
     ```

4. **Commit policy**:
   - Include the relevant session file(s) in the same commit or pull request that contains the AI-generated code.
   - Use clear commit messages referencing the session file:
     ```
     feat: implement EVM ADD opcode

     Implemented via Opencode session: .ai/sessions/aw-007.md
     ```

This policy helps maintain:
- Traceability of how logic was derived from Ethereum specs
- Consistency in implementation style
- Ability to review reasoning when debugging or auditing

## Pull Request Guidelines

- Target the `main` branch
- Include tests for new functionality (ExUnit + doctests where appropriate)
- Run `mix format --check-formatted` and `mix compile --warnings-as-errors` successfully
- Reference relevant Ethereum specs (Yellow Paper section, execution-specs file/line, go-ethereum file) in comments or PR description
- Attach or link the corresponding `.ai/sessions/*.md` file(s)

## Questions?

Open a discussion or draft PR. All contributors are expected to follow the Nix + Opencode workflow described above.

Happy hacking.
```
