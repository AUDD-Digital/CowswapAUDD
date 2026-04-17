# CowswapAUDD

Custom token list for [CoW Swap](https://swap.cow.fi/) featuring the Australian Digital Dollar (AUDD) alongside commonly traded ERC-20 tokens on Ethereum Mainnet.

## What This Repository Does

CoW Swap allows users to supply a custom token list URL so that tokens not on its default list appear in the swap interface. This repository hosts that list — a single JSON file (`tokenlist`) that CoW Swap fetches at runtime to populate its token picker.

By pointing CoW Swap at the raw URL of the `tokenlist` file in this repo, AUDD and 243 other curated tokens become available for trading.

## Repository Structure

```
.
├── tokenlist       # The token list (JSON with inline comments)
├── .gitignore
└── README.md
```

There is no build step, no dependencies, and no tests. The repository is a static data file served via GitHub.

## Token List Format

The `tokenlist` file follows the [Uniswap Token Lists](https://tokenlists.org/) specification with the following top-level fields:

| Field       | Value |
|-------------|-------|
| `name`      | `CowswapAUDD` |
| `timestamp` | `2024-02-27T16:30:00Z` |
| `version`   | `0.0.1` |
| `logoURI`   | AUDD logo from CoinGecko |
| `tokens`    | Array of 244 ERC-20 token entries |

Each token entry contains:

- `chainId` — always `1` (Ethereum Mainnet)
- `address` — the ERC-20 contract address
- `name` — human-readable token name
- `symbol` — ticker symbol
- `decimals` — token decimal places
- `logoURI` — CoinGecko-hosted logo thumbnail

## Token Coverage

- **244 tokens** across **1 chain** (Ethereum Mainnet only)
- **211 unique symbols** (some symbols appear multiple times for different contracts)
- AUDD is listed first, at contract address `0x4cce605ed955295432958d8951d0b176c10720d5`
- Other notable tokens include major stablecoins (USDT, USDC, DAI, BUSD), DeFi tokens (UNI, AAVE, COMP, MKR, SNX), and popular ERC-20s (SHIB, LINK, MATIC, APE)

## How to Use

1. Open [CoW Swap](https://swap.cow.fi/)
2. Navigate to token list settings
3. Add the raw GitHub URL for the `tokenlist` file in this repository
4. The tokens defined in the list will now appear in the CoW Swap token picker

## Known Issues

### Invalid JSON

The `tokenlist` file is **not valid JSON**:

1. **Inline comment on line 4** — A `#`-prefixed comment explains the version field. JSON does not support comments. Parsers that strictly validate JSON will reject this file.
2. **Duplicate trailing brackets on lines 1966–1967** — After the array and object are properly closed on lines 1964–1965, there is a redundant `]` and `}`. This causes a "Extra data" parse error in strict JSON parsers.

CoW Swap's token list parser may tolerate these issues, but the file will fail validation against the [Uniswap Token Lists JSON Schema](https://uniswap.org/tokenlist.schema.json).

### Duplicate Token Entries

Three tokens are listed twice with identical addresses:

| Symbol | Name             | Address |
|--------|------------------|---------|
| TON    | Toncoin          | `0x582d872a1b094fc48f5de31d3b73f2d9be47def1` |
| TON    | Tokamak Network  | `0x2be5e8c109e2197d077d13a82daead6a9b3433c5` |
| TON    | TON              | `0x6a6c2ada3ce053561c2fbc3ee211f23d9b8c520a` |

### Duplicate Symbols (Different Contracts)

25 symbols map to more than one contract address (e.g., BNB ×2, TON ×6, APE ×3, ALPHA ×3, RARE ×3). This is expected when multiple projects share a ticker, but may cause confusion in the swap UI.

### No Multi-Chain Support

All 244 tokens are on Ethereum Mainnet (`chainId: 1`). There are no entries for L2s (Arbitrum, Optimism, Base, Polygon) or other chains that CoW Swap supports.

### Stale Version

The list is at version `0.0.1` with a timestamp of February 2024. The version has not been incremented across the two "Update tokenlist" commits.

## Code Coverage

There are no tests, scripts, CI pipelines, or validation tooling in this repository. The token list is maintained by manual editing.
