# DefiShield
AI-powered DeFi risk intelligence for tokens, wallets, and on-chain exploits — powered by a GenLayer Intelligent Contract.

Live app: https://defishield.lovable.app/
Contract (GenLayer Studio): 0x409D56AEfaED39C4A6a3760C85143718623e7446
Network: GenLayer Studionet
What is DefiShield?
DefiShield is a defensive toolkit for DeFi users. It combines structured on-chain signals (liquidity locks, tax %, whale concentration, ownership state) with LLM-driven reasoning inside a GenLayer Intelligent Contract to produce explainable risk scores and alerts.

GenLayer contracts can call LLMs and the open web as part of consensus, so DefiShield's verdicts (e.g. "likely rug", "audited and clean", "suspicious social hype") are produced and validated on-chain rather than by an off-chain oracle.

Features
Module	What it does
Token Scan	Risk scoring for an ERC-20 / SPL token across Ethereum, Base, BNB Chain, and Solana using liquidity lock, tax, ownership, and social signals.
Wallet Forensics	Flags wallets linked to prior rugs, mixers, or exploit clusters.
Exploit Detector	Pattern-matches contracts against known exploit primitives (reentrancy, hidden mint, proxy upgrade traps).
Portfolio	Continuously re-scores tokens you hold.
Alerts	Subscribe to risk-state changes for a token, wallet, or contract.
Lookup	Free read-only queries — no wallet required.
Reads are free. Writes (submitting a scan, subscribing to alerts) require MetaMask connected to GenLayer Studionet.

Architecture
 ┌──────────────┐    reads     ┌───────────────────────────┐
 │  React app   │ ───────────► │  GenLayer Intelligent     │
 │ (Lovable)    │              │  Contract (Studionet)     │
 │              │ ◄─ writes ── │  0x409D56AE…23e7446       │
 └──────┬───────┘   MetaMask   └─────────────┬─────────────┘
        │                                    │ LLM + web
        │                                    ▼
        │                          consensus-validated
        │                          risk verdicts
        ▼
   user dashboard
Frontend: React + TanStack Start + Tailwind, deployed via Lovable.
Contract runtime: GenLayer Studionet (EVM-compatible, LLM-enabled execution).
Wallet: MetaMask, configured for the GenLayer Studionet RPC.
Risk Signals
DefiShield's token scan accepts:

Chain, symbol, project name, contract address
Liquidity lock duration (days)
Whale wallet %, developer wallet %
Buy tax %, sell tax %
Flags: LiquidityLocked, OwnerCanMint, OwnershipRenounced, SuspiciousSocialHype, PriorRugAssociation, AuditCompleted, CentralizedGovernance, HiddenBlacklistFn, ProxyContract, ExploitHistory, SuspiciousVolume, FakeCommunityGrowth
These are fed to the contract, which combines deterministic scoring with LLM reasoning to return a verdict + rationale.

Getting Started
Use the hosted app
Visit https://defishield.lovable.app/
Browse Lookup and Token Scan reads with no wallet.
To submit a scan or set alerts:
Install MetaMask
Add GenLayer Studionet as a custom network
Open the contract in GenLayer Studio
Run the frontend locally
git clone https://github.com/<your-org>/defishield.git
cd defishield
bun install
bun dev
Then open http://localhost:8080.

Environment
Create .env.local:

VITE_GENLAYER_RPC=https://studio.genlayer.com/api
VITE_DEFISHIELD_CONTRACT=0x409D56AEfaED39C4A6a3760C85143718623e7446
Repository Layout
.
├── README.md
├── LICENSE
├── .gitignore
├── CONTRIBUTING.md
└── contract/
    └── defishield.py        # GenLayer Intelligent Contract (source)
The deployed contract address is canonical. The contract/ directory tracks the source you can re-deploy or fork via GenLayer Studio.

Roadmap
 Multi-chain wallet forensics graph
 Webhook + Telegram alert delivery
 Public risk-score API
 Community-submitted exploit signatures
Disclaimer
DefiShield outputs are heuristic and probabilistic. They are not financial advice and not a guarantee of safety. Always do your own research before interacting with any token or contract.

License
MIT
