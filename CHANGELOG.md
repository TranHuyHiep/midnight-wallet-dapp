# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2025-02-12

### Added

- Wallet connection via Midnight DApp Connector API (Lace wallet support)
- Contract deployment for the `unshielded-demo` Compact contract
- Token operations: mint, claim, deposit, and withdraw unshielded tokens
- NIGHT token deposit and withdraw support
- Built-in faucet for local "undeployed" testnet
- Faucet links for preview and qanet networks
- Real-time activity log with timestamped entries
- Docker and Docker Compose support for local blockchain environment
- CI pipeline with lint, format check, typecheck, build, and e2e smoke tests
- License header enforcement via GitHub Actions

### Dependencies

- MidnightJS SDK v3.0.0
- Wallet SDK Facade v1.0.0
- Ledger v7.0.0
- Compact JS v2.4.0 / Compact Runtime v0.14.0
- React 19, Vite 7, TypeScript 5.9
