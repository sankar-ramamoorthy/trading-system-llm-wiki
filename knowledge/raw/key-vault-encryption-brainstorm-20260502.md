---
title: Key Vault and Encryption Brainstorm
type: raw
status: brainstorm
tags: [trading-system, security, api-keys, key-vault, encryption, milestone-9]
created: 2026-05-02
---

# Key Vault and Encryption Brainstorm

Raw brainstorm for local encrypted secret storage. Source material for a future ADR. Not yet accepted implementation scope.

## Why This Matters Now

The system currently has three active API keys:
- `GROQ_API_KEY` — LLM provider (trade capture parsing)
- `MASSIVE_API_KEY` — market data provider (daily OHLCV)
- Future: Alpaca API key/secret (paper trading, Milestone 10)

These live in `.env` (Docker) and are manually set as environment variables for local CLI use. This is workable for a single developer but brittle — easy to accidentally commit, hard to rotate, and invisible to the application.

The goal: an encrypted local vault that stores secrets by logical name, with environment-variable fallback for Docker and CI contexts.

---

## Encryption Approach Options

### Option A: Fernet symmetric encryption (`cryptography` library)

```python
from cryptography.fernet import Fernet
key = Fernet.generate_key()
f = Fernet(key)
token = f.encrypt(b"my-secret")
plaintext = f.decrypt(token)
```

**Pros:** Simple API, well-audited, AES-128-CBC + HMAC-SHA256, built-in expiry support.
**Cons:** Fixed 128-bit key size, key must be stored separately.
**Verdict:** Best first-pass choice. Already a transitive dependency via `cryptography`.

### Option B: AES-256-GCM via `cryptography` primitives

More control over key size and nonce. Authenticated encryption (GCM) provides integrity.

**Pros:** 256-bit keys, authenticated ciphertext, industry standard.
**Cons:** More boilerplate than Fernet, easy to misuse nonce.
**Verdict:** Better if we later need to match external key standards.

### Option C: OS keychain (`keyring` library)

```python
import keyring
keyring.set_password("trading-system", "GROQ_API_KEY", "sk-...")
keyring.get_password("trading-system", "GROQ_API_KEY")
```

**Pros:** No master key to manage — OS handles it. Windows Credential Store, macOS Keychain, Linux Secret Service.
**Cons:** Platform behavior differs. Docker containers have no keychain. Harder to back up or move.
**Verdict:** Good complement to file-based vault, not a replacement. Could be used for the master key itself.

### Option D: Age encryption (`pyrage`)

Modern format designed by the Filippo Valsorda group. No password stretching by default.

**Pros:** Modern, audited, passphrase or public-key modes.
**Cons:** Less familiar, smaller ecosystem.
**Verdict:** Interesting for the future, not the first-pass choice.

### Recommended: Start with Fernet (Option A), keep Option C in mind for master-key storage on Windows.

---

## Master-Key Management Options

The master key is the real security boundary. How it's stored determines the threat model.

### Option 1: Environment variable (`TRADING_SYSTEM_MASTER_KEY`)

```text
TRADING_SYSTEM_MASTER_KEY=<base64-fernet-key>
```

**Pros:** Consistent with existing env-var pattern. Works in Docker, CI, and local CLI.
**Cons:** Key still lives in a file (`.env`) or shell config. Same exposure risk as raw secrets.
**Verdict:** Simplest start. Acceptable if `.env` is never committed.

### Option 2: OS Credential Store for the master key

Store the master key in Windows Credential Manager (or macOS Keychain) using `keyring`. Application reads it at startup.

**Pros:** Master key never in any file. OS access controls apply.
**Cons:** Does not work in Docker without extra setup. Platform-specific.
**Verdict:** Best for local CLI use. Need a Docker fallback to env-var.

### Option 3: Derived key (OS username + machine ID)

Derive the master key from stable local identifiers using PBKDF2.

```python
import hashlib, platform, getpass
material = f"{getpass.getuser()}-{platform.node()}"
key = hashlib.pbkdf2_hmac("sha256", material.encode(), salt, 100_000)
```

**Pros:** No explicit secret to manage at all.
**Cons:** Key changes if username or hostname changes. Weaker than a random key.
**Verdict:** Usable as a fallback, not as the primary approach.

### Option 4: Interactive passphrase at startup

Prompt the user for a passphrase and derive the key via Argon2 or PBKDF2.

**Pros:** Strongest security — passphrase never stored anywhere.
**Cons:** Terrible UX for a local daily-use CLI tool. Breaks automation.
**Verdict:** Not suitable for this system.

### Recommended: Option 2 for local CLI (keyring → Windows Credential Manager), with Option 1 (env-var) as the Docker and fallback path. Hybrid resolution: keyring first, then `TRADING_SYSTEM_MASTER_KEY`, then error.

---

## Secret Resolution Precedence

```text
1. Encrypted vault file (.trading-system/keys.enc) — decrypted using master key
2. Environment variable fallback (GROQ_API_KEY, MASSIVE_API_KEY, etc.)
3. Clear error with redacted hint — never leak values
```

This preserves Docker compatibility: Docker containers read from env vars, not the vault file.

---

## Vault File

```text
.trading-system/keys.enc      # encrypted secrets
.trading-system/store.json    # existing trade data
```

- Never commit `keys.enc` (add to `.gitignore`)
- Backup alongside `store.json`
- Human-unreadable without the master key

---

## CLI Commands (future)

```powershell
uv run trading-system set-secret GROQ_API_KEY
uv run trading-system set-secret MASSIVE_API_KEY
uv run trading-system list-secrets          # names only, never values
uv run trading-system delete-secret GROQ_API_KEY
uv run trading-system rotate-master-key     # re-encrypt all secrets with new key
```

---

## Docker Behavior

Docker containers should NOT use the vault file. Continue using `.env` / `env_file` in docker-compose for all container secrets. The vault is for local `uv run` CLI use only.

The secret resolution layer should detect when no vault file exists and fall directly to env-var fallback without error — this is the Docker path.

---

## Redaction and Testing

- Never log, print, or include secret values in error messages
- Tests use fake secrets via environment variables — no vault file in test contexts
- Vault operations should be tested with a temp directory and a test master key

---

## What This Is NOT

- Not browser-based secret entry (frontend never touches secrets)
- Not a multi-user or team credential system
- Not cloud secret management
- Not audit logging of secret access
- Not production auth or authorization

---

## Milestone Assignment

**Milestone 9A: Local Key Vault ADR + Implementation**

Proposed sequence for Milestone 9:
- 9A: Write ADR for key vault boundary → implement `local_secret_vault` library → wire into CLI secret resolution
- 9B: Massive.com options chain data as a new snapshot type
- 9C: Milestone 9 closeout

Rationale: key vault before Alpaca (Milestone 10) because broker credentials are higher-stakes than market data API keys. Validate the vault on Groq + Massive keys first, then use it for Alpaca.

---

## Related Pages

- [[reusable-local-secret-vault-library]]
- [[api-key-vault-discussion-20260502]]
- [[milestone-7-api-first-trade-capture-issue-map]]
