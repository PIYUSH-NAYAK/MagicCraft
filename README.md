# MagicCraft

> A Minecraft-inspired 3D multiplayer voxel game powered by **React Three Fiber**, **Socket.IO**, and **Solana** blockchain — featuring real-time PvP, boss fights, NFT rewards, and on-chain game state via **MagicBlock Ephemeral Rollups**.

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [Solana / Anchor Smart Contract](#solana--anchor-smart-contract)
- [Multiplayer Architecture](#multiplayer-architecture)
- [Realms & Boss System](#realms--boss-system)
- [Wallet Integration](#wallet-integration)
- [NFT Minting](#nft-minting)
- [Scripts Reference](#scripts-reference)

---

## Overview

MagicCraft is a full-stack 3D multiplayer game built entirely in the browser. Players explore procedurally structured voxel worlds, fight bosses in themed realms, battle other players in real-time PvP, collect and use tools, and earn NFT rewards — all secured by the Solana blockchain.

---

## Tech Stack

| Layer | Technology |
|---|---|
| 3D Rendering | Three.js + React Three Fiber |
| Physics | `@react-three/rapier` (Rapier3D WASM) |
| UI Framework | React 18 |
| State Management | Zustand |
| Real-time Networking | Socket.IO (client + server) |
| Backend Server | Node.js + Express |
| Blockchain | Solana (Devnet) |
| Smart Contracts | Anchor Framework |
| On-chain Speed | MagicBlock Ephemeral Rollups |
| Wallet Support | Phantom, Solflare |
| AI NPCs | Groq API |
| Package Manager | Bun (contracts/app) + npm (web/server) |

---

## Project Structure

```
MagicCraft/
├── web/                    # Main React game client
│   ├── public/             # Static assets (models, textures, icons)
│   │   ├── models/         # GLTF 3D models (animals, blocks, enemies)
│   │   ├── assets/         # Realm textures (desert, jungle, snow)
│   │   └── nfts/           # NFT images
│   ├── src/                # Game source code
│   │   ├── App.js          # Root component & game canvas
│   │   ├── Player.js       # First-person player controller
│   │   ├── OtherPlayers.js # Remote player rendering
│   │   ├── Cube.js         # Voxel block system
│   │   ├── Ground.js       # Terrain generation
│   │   ├── Animals.js      # Animal entities
│   │   ├── Enemies.js      # Enemy AI
│   │   ├── Boss3D.js       # 3D boss rendering
│   │   ├── UI.js           # HUD, inventory, health bar
│   │   ├── SocketContext.js# Socket.IO client context
│   │   ├── SolanaProvider.js # Wallet adapter provider
│   │   ├── useStore.js     # Global Zustand state
│   │   ├── config/         # Realm & entity configs
│   │   └── idl/            # Anchor IDL for on-chain calls
│   └── .env.example        # Environment variable template
│
├── server/                 # Multiplayer game server
│   ├── index.js            # Express + Socket.IO server
│   └── package.json
│
├── programs/               # Solana smart contracts (Anchor)
│   └── counter/
│       └── src/lib.rs      # MagicCraft on-chain program
│
├── app/                    # Lightweight Anchor counter demo (Bun)
│   └── src/
│       ├── App.tsx
│       └── components/
│
├── migrations/             # Anchor deployment scripts
├── scripts/                # Anchor build/deploy/test helpers
├── tests/                  # Anchor integration tests
├── Anchor.toml             # Anchor project config
└── Cargo.toml              # Rust workspace config
```

---

## Features

- 🌍 **Voxel World** — Place and break blocks in a procedurally structured world
- 🧑‍🤝‍🧑 **Real-time Multiplayer** — See and fight other players live via Socket.IO
- 🏰 **3 Realms** — Desert (Giant), Jungle (Demon), Snow (Yeti) — each with a unique boss
- ⚔️ **PvP Combat** — Ranged and melee attacks between players (5-unit range)
- 👾 **Boss Fights** — 500 HP bosses that auto-attack, respawn every 30s
- 🐾 **Animals & NPCs** — Interactive animals with AI-driven dialogue (Groq)
- 🎒 **Inventory System** — Hotbar, item switching, in-hand 3D item rendering
- 🏃 **Movement** — Sprint (Shift), 3rd-person (F5), pointer-lock controls
- 💬 **Chat / Whisper** — In-game chat and private whisper system
- 🪙 **Solana Integration** — On-chain player stats (blocks placed, kills, score)
- ⚡ **MagicBlock Rollups** — Near-instant on-chain transactions for game actions
- 🎨 **NFT Minting** — Earn NFTs for animals caught/discovered during gameplay
- 🔑 **Session Keys** — Gasless transactions using delegated session authority

---

## Prerequisites

Make sure you have the following installed:

```bash
# Node.js (v18+)
node --version

# npm
npm --version

# Bun (for contracts and app/)
curl -fsSL https://bun.sh/install | bash
bun --version

# Rust (for Anchor/Solana programs)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup --version

# Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
solana --version

# Anchor CLI
cargo install --git https://github.com/coral-xyz/anchor anchor-cli --locked
anchor --version
```

---

## Environment Setup

### 1. Solana Wallet

```bash
# Generate a new local keypair (if you don't have one)
solana-keygen new --outfile ~/.config/solana/id.json

# Set cluster to Devnet
solana config set --url devnet

# Get some Devnet SOL for transactions
solana airdrop 2
```

### 2. Web Client Environment

```bash
# Copy the example env file
cp web/.env.example web/.env
```

Edit `web/.env`:

```env
# Groq API key for AI NPC dialogue
# Get yours at: https://console.groq.com
REACT_APP_GROQ_API_KEY=your_groq_api_key_here
```

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/PIYUSH-NAYAK/MagicCraft.git
cd MagicCraft
```

### 2. Install web client dependencies

```bash
cd web
npm install
cd ..
```

### 3. Install server dependencies

```bash
cd server
npm install
cd ..
```

### 4. Install contract/app dependencies (Bun)

```bash
bun install
```

---

## Running the Project

You need **3 terminals** running simultaneously:

### Terminal 1 — Game Server (Multiplayer)

```bash
cd server
npm start
# Runs on http://localhost:3001
```

### Terminal 2 — Web Game Client

```bash
cd web
npm start
# Runs on http://localhost:3000
```

### Terminal 3 — Anchor App (optional, for on-chain counter demo)

```bash
cd app
bun run dev
# Runs on http://localhost:5173
```

Open your browser at **http://localhost:3000**, connect a Solana wallet (Phantom or Solflare), and start playing.

---

## Solana / Anchor Smart Contract

The on-chain program is located in `programs/counter/src/lib.rs`.

**Program ID (Devnet):** `72WYk7b352n3SFb3k1qGmPnKDY2dUvr5rgK55RNa14Yt`

### Build the program

```bash
anchor build
```

### Deploy to Devnet

```bash
anchor deploy --provider.cluster devnet
```

### Run tests

```bash
anchor test
```

### On-chain Instructions

| Instruction | Description |
|---|---|
| `initialize` | Create a new player account on-chain |
| `enter_game` | Mark player as active in a specific realm |
| `end_game` | End the session, save final stats |
| `update_stats` | Update blocks placed, kills, attacks, score |
| `delegate` | Delegate account to MagicBlock Ephemeral Rollup |
| `commit` | Commit ephemeral state back to mainchain |

### MagicBlock Ephemeral Rollups

Game actions (attacks, block placements) are routed through MagicBlock's ephemeral rollup layer for near-instant finality without paying full Solana fees on every action. The account is delegated at game start and committed at game end.

---

## Multiplayer Architecture

The game server (`server/index.js`) manages all authoritative game state:

```
Client ──Socket.IO──▶ Server
                        │
                 ┌──────┴──────┐
                 │  Room State  │
                 │  Player HP   │
                 │  Boss State  │
                 │  Block Sync  │
                 │  Chat / Whisper│
                 └──────────────┘
```

- **Rooms:** Players join one of 3 rooms, each with its own boss and player list
- **Player HP:** Server-authoritative — clients cannot spoof health
- **Boss Logic:** Server controls boss AI, attack targeting, respawn timer
- **Block Sync:** All block place/break events are broadcast to all room members
- **Respawn:** Players respawn after 3 seconds with full HP

---

## Realms & Boss System

| Room | Realm Theme | Boss | Boss HP |
|------|-------------|------|---------|
| Room 1 | Desert | Giant | 500 |
| Room 2 | Jungle | Demon | 500 |
| Room 3 | Snow | Yeti | 500 |

- Bosses attack the nearest player every **2 seconds**
- Boss attack range: **20 units**
- Players deal **6 HP per hit** (3 hearts)
- Boss deals **4 HP per hit** (2 hearts)
- Player max HP: **40** (20 hearts)
- Boss respawns **30 seconds** after death

---

## Wallet Integration

MagicCraft supports:

- **Phantom** — `@solana/wallet-adapter-phantom`
- **Solflare** — `@solana/wallet-adapter-solflare`

Wallet connection is required to:
- Initialize your on-chain player account
- Sign game transactions (enter/exit game, stats update)
- Mint NFT rewards

The wallet button appears in the top-right corner of the game UI.

---

## NFT Minting

When players discover or capture animals, they can mint an NFT representing that animal.

- Mint addresses are stored on Devnet
- NFT metadata includes the animal type and player wallet
- Minting is triggered in-game and signed via the connected wallet

---

## Scripts Reference

| Command | Description |
|---|---|
| `cd web && npm start` | Start the React game client |
| `cd server && npm start` | Start the Socket.IO multiplayer server |
| `cd app && bun run dev` | Start the Anchor counter demo app |
| `anchor build` | Compile the Solana program |
| `anchor deploy` | Deploy program to configured cluster |
| `anchor test` | Run on-chain integration tests |
| `bun run scripts/setup-mints.ts` | Set up animal NFT mints on-chain |

---

## Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feat/your-feature`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to the branch: `git push origin feat/your-feature`
5. Open a Pull Request

---

## License

MIT © [PIYUSH-NAYAK](https://github.com/PIYUSH-NAYAK)
