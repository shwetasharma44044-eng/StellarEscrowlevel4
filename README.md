# StellarEscrow - Milestone-Based Freelance Payment Escrow

StellarEscrow is a production-ready milestone payment escrow protocol built on **Stellar Soroban Smart Contracts**. It protects both clients and freelancers by locking payment funds in escrow and automatically releasing them upon milestone approvals, with built-in dispute resolution and automated expiry-based refunds.

---

## 🚀 Deployed Contracts & Assets
- **Testnet Contract Address**: `CABRYRJWNR5WVI34LSA667LTXG7NHIRJOAZASX5MTFJK5JHCAD7ILETJ`
- **Native SAC Token Address**: `CDLZFC3SYJYDTIW6A2NDSLZDGGF45Q7JPA7V6M45A57KGS65MAUZ74AF` (XLM)
- **RPC URL**: `https://soroban-testnet.stellar.org`
- **Network Passphrase**: `Test SDF Network ; September 2015`

---

## 🛠️ Tech Stack & Architecture

### 1. Smart Contract (Soroban Rust)
- **Folder**: `contracts/escrow_contract/`
- Implements decentralized trust logic using Rust and the Soroban SDK.
- Restricts critical functions (funding, completion, release, dispute resolution) to authorized callers via Stellar signature checks.

### 2. Frontend React Application
- **Folder**: `frontend/`
- Built with React, TypeScript, Vite, and Tailwind CSS.
- Integrates `@stellar/stellar-sdk` and `@creit.tech/stellar-wallets-kit` to connect wallets, simulate contract calls, sign envelopes, and submit transactions.

### 3. Feedback Server (Express + SQLite)
- **Folder**: `backend/`
- Stores aggregated rating telemetry and sentiment reviews in a local SQLite database (`feedback.db`).

---

## 🏃 Getting Started & Running Locally

### 1. Prerequisites
- **Node.js**: v18+
- **Rust**: with `wasm32-unknown-unknown` target.
- **Stellar CLI**: installed and configured.

### 2. Start the Backend Server
Navigate to the root and install backend packages:
```bash
cd backend
npm install
node server.js
```
The server will start on port `5000` and create `feedback.db` automatically.

### 3. Run the Frontend
Navigate to the frontend folder, configure variables, and launch:
```bash
cd ../frontend
npm install
npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## 📋 Milestone Lifecycle Flow
1. **Created**: Milestone is initialized by the client but is unfunded.
2. **Funded**: Client locks XLM into the contract. It is now safe for the freelancer to write code.
3. **Submitted**: Freelancer marks the work as complete. The review window starts.
4. **Released**: Client reviews work, approves it, and releases funds directly to the freelancer's wallet.
5. **Disputed**: If work doesn't meet requirements, client flags it. Locked funds enter dispute pending Arbiter resolution.
6. **Refunded**: Funds are returned to the client. This happens if a milestone expires unfunded, arbiter resolves in favor of client, or freelancer consents to voluntary cancel.

---

## 🧪 Running Smart Contract Tests
Execute the Cargo test suite in the contract folder:
```bash
cargo test --manifest-path contracts/escrow_contract/Cargo.toml --target-dir target_test -j 1
```
All 6 tests verify happy path flows, edge cases, dispute arbitrations, and authorization checks.
