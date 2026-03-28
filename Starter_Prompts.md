# Starter Prompts

Ready-to-use prompts and context blocks for starting a Stellar project with Claude Code. Copy, fill in the brackets, and paste.


## The wallet vs. dApp distinction

This is the single most expensive ambiguity in a Stellar build session. If you say "build me a Stellar wallet," Claude defaults to a DeFi dApp pattern: connect Freighter, build a UI on top, sign transactions with an injected browser extension. That is not a self-custodial wallet.

**If you want a self-custodial wallet, say so explicitly:**

Bad:
> Build me a Stellar wallet with CETES integration.

Good:
> Build a self-custodial browser wallet that generates and stores its own BIP-39 mnemonic. No browser extension required. Users create an account with a password. The password runs through PBKDF2 to derive an AES-GCM key, which encrypts the mnemonic and stores it in localStorage. On login, the user's password decrypts the mnemonic in memory. All transaction signing happens locally; no external signer, no Freighter, no xBull.

The good version gives Claude the architecture. The bad version gives Claude a problem it will solve with whatever pattern is most common in its training data.


## Protocol context block

Paste this at the top of your first message in any Stellar session. Fill in the blanks for your project.

```
Context for this project:
- Stellar SDK: @stellar/stellar-sdk v14. The RPC namespace is `rpc`, not `SorobanRpc`.
  Use `new rpc.Server(url)` and `rpc.assembleTransaction()`.
- Network: [testnet | mainnet]. RPC: [https://soroban-testnet.stellar.org | https://mainnet.stellar.validationcloud.io/v1/YOUR_KEY].
- Stellar Wallets Kit: v2. Use the static API: `StellarWalletsKit.build(config)`.
  Do not use the v1 constructor pattern.
- USDC issuer (testnet): GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5
  (Use this one for DeFindex compatibility. The Circle testnet issuer is different and won't share liquidity.)
- USDC issuer (mainnet): GA5ZSEJYB37JRC5AVCIA5MOP4RHTM335X2KGX3IHOJAPP5RE34K4KZVN
- [Add any project-specific notes: Etherfuse customer_id, DeFindex vault address, etc.]
```


## Parallel agent prompt pattern

Use this after you have a working scaffold and want to parallelize the remaining build.

```
The scaffold is in place. Now use plan mode to divide the remaining work into three independent tracks:

Track 1: Core logic and tests
- [list the pure-logic modules: crypto layer, Stellar operations, protocol integrations]
- Write Vitest unit tests for each module as you go

Track 2: State management and routing
- [list the stores, state singletons, and routing logic]
- No UI components; just state and navigation logic

Track 3: UI components
- [list the pages and components]
- Use the stores from Track 2; do not duplicate state

After the plan is approved, spawn one agent per track and run them in parallel.
Once all three are done, spawn a fourth agent to do integration: wire the UI to the stores,
the stores to the core logic, and run the full test suite.
```

This pattern (from the DevRel experiment) ran three agents in parallel and cut wall-clock time by roughly 40% on a 145-minute total build.


## CLAUDE.md template for a Stellar project

Create a `CLAUDE.md` at your repo root with this content. Every Claude Code session reads it automatically.

```markdown
# Project context

## Tech stack
- Framework: [SvelteKit + Svelte 5 runes | React 19 | Next.js | other]
- Stellar SDK: @stellar/stellar-sdk v14 (rpc namespace, not SorobanRpc)
- Network: [testnet | mainnet]
- Package manager: [npm | pnpm | bun]

## Stellar configuration
- Network passphrase: [Networks.TESTNET | Networks.PUBLIC]
- RPC URL: [your RPC endpoint]
- USDC issuer: [issuer address]

## Protocol-specific notes
### Etherfuse
- customer_id: [your customer_id] — permanent, never generate a new one
- Auth header: `Authorization: your-api-key` (no Bearer prefix)
- Sandbox simulation: POST to /ramp/order/fiat_received to advance order state
- Indexing delay: wait 3-10s after order creation before querying status

### DeFindex
- Auth header: `Authorization: Bearer your-api-key`
- XLM vault address: [address]
- USDC vault address: [address]
- Classic Stellar assets must be SAC-deployed before depositing into vaults (all common ones already are)
- Endpoint is /vault/ not /vaults/; amounts are always arrays; success is HTTP 201

### Trustless Work
- Auth header: `Authorization: x-api-key: your-api-key`
- API URL: [mainnet: https://api.trustlesswork.com | dev: https://dev.api.trustlesswork.com]

## Wallet architecture
[Delete this section if not building a self-custodial wallet]
- Self-custodial: generates BIP-39 mnemonic in-browser, no browser extension
- Key derivation: SEP-0005 (m/44'/148'/0')
- Encryption: AES-GCM with PBKDF2 key derivation (256-bit key, 250k iterations)
- Storage: encrypted mnemonic in localStorage

## Testing
- Framework: Vitest
- Crypto tests: run in node environment (noble/curves rejects jsdom Buffer polyfill)
  Add to vitest.config: environmentMatchGlobs: [["src/lib/crypto/**", "node"]]
```


## Quick reference: common corrective prompts

**Wrong wallet type:**
> Stop. I don't want a dApp that connects to Freighter. I want a self-custodial wallet that generates its own keys. Start over with that architecture.

**Wrong SDK version:**
> You're using the old SorobanRpc namespace. In v14, it's `rpc`. Replace all instances of `SorobanRpc.Server` with `rpc.Server` and `SorobanRpc.assembleTransaction` with `rpc.assembleTransaction`.

**Wrong USDC issuer:**
> Use the DeFindex-compatible USDC issuer on testnet: GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5. The Circle issuer won't share liquidity with DeFindex pools.

**Stuck on DeFindex API key:**
> DeFindex has self-service signup at https://docs.defindex.io/api-integration-guide/api#generate-your-api-key. If you'd rather skip the key entirely, call the Soroban contracts directly using rpc.Server.simulateTransaction(), then assembleTransaction() and sendTransaction(). Use the paltalabs registry addresses for testnet vault addresses.

**Stuck polling after sendTransaction:**
> sendTransaction returns PENDING, not a final status. You need to poll rpc.getTransaction(hash) until status is SUCCESS or FAILED. Add a polling loop with 1s intervals and a 30s timeout.
