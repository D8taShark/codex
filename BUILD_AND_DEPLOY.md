# Build & Deployment Guide

This guide walks you (or your team) through building and deploying both the **TypeScript CLI** (`codex-cli`) and the **Rust engine** (`codex-rs`). It’s designed for users of all experience levels.

---

## 1) Prerequisites

- **Git**: Install from https://git-scm.com/downloads.
- **Node.js** (v16+) & **pnpm**:
  ```bash
  # Install pnpm on macOS/Linux
  curl -fsSL https://get.pnpm.io/install.sh | sh -
  # On Windows, use the installer: https://pnpm.io
  ```
- **Rust & Cargo**:
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  rustup default stable
  ```
- _(Optional)_ **just** for shortcuts:
  ```bash
  cargo install just
  ```

---

## 2) Clone the Repository

```bash
git clone https://github.com/your-org/codex.git
cd codex
```

---

## 3) Build & Test the TypeScript CLI

```bash
cd codex-cli
pnpm install         # install dependencies
pnpm test            # run all tests
pnpm run build       # compile TypeScript → JavaScript (dist/)
```

To try it locally:

```bash
export OPENAI_API_KEY=sk-...
node dist/bin/codex.js --help
```

---

## 4) Publish the CLI to npm

1. Bump version in `codex-cli/package.json` (e.g. `1.2.3` → `1.2.4`).
2. Commit and tag:
   ```bash
   git add codex-cli/package.json
   git commit -m "chore(release): v1.2.4"
   git tag v1.2.4
   git push --follow-tags
   ```
3. Publish:
   ```bash
   cd codex-cli
   pnpm publish --access public
   ```
   Team members can now install globally:

```bash
pnpm add -g @openai/codex
codex --help
```

---

## 5) Build & Test the Rust Engine

```bash
cd codex-rs
just fmt            # optional formatting shortcut
cargo test          # run Rust tests
cargo build --release  # compile optimized binaries
```

Binaries live in `codex-rs/target/release/` (e.g. `codex`, `codex exec`, `codex tui`).

---

## 6) Package & Distribute Rust Binaries

- **GitHub Releases**: Zip `target/release/` and attach to a Release.
- **Shared Drive/S3**: Copy binaries to your internal storage.
- **Cargo Install** (if devs have Rust):
  ```bash
  cargo install --path codex-rs
  ```

---

## 7) Environment Variables & Config

- **OpenAI**:
  ```bash
  export OPENAI_API_KEY=sk-yourkey
  ```
- **Azure** (if using Azure)
  ```bash
  export AZURE_OPENAI_API_KEY=your-azure-key
  ```
- Other flags:
  ```bash
  # Skip loading AGENTS.md
  export CODEX_DISABLE_PROJECT_DOC=1
  ```

Config files (`config.json`, `instructions.md`, etc.) live under `~/.codex/` by default.

---

## 8) Team Rollout

1. Publish CLI to npm.
2. Distribute Rust binaries or Docker image.
3. Document the install commands in your internal wiki:

   ```bash
   # CLI only
   pnpm add -g @openai/codex

   # Rust engine (optional)
   cargo install --path codex-rs
   ```

4. Encourage team to set required env vars before use.

**Enjoy using Codex!**
