# Squeek Wheel Dashboard

A single-file, community-built dashboard for the [Squeekchain](https://docs.squeekchain.com) hamster wheel (Sepolia testnet).

**Live:** https://jojay64.github.io/squeek-dashboard/

> Unofficial. Not affiliated with or endorsed by the Squeekchain team. Testnet only; SQK and Squeeks have no monetary value.

## What it shows

- **Hamster status:** an animated hamster in a wheel that spins at the real RPM, and sleeps when the wheel is idle.
- **Live numbers:** speed, cumulative work, settled Squeek block (with progress to the next one), settlement queue, oracle state and the optimistic projection.
- **Hamster forecast:** the hamster's usual wake-up and bedtime (UTC), computed from the observed typical day, plus a local-only guessing game.
- **Hamster Olympics:** the lifetime distance in fun units (marathons, track laps, Eiffel Towers, Paris to Lyon). Comparisons are approximate.
- **Activity charts:** observed work per day over 7 days and the typical-day profile, with partial days greyed out.
- **Checkpoint helper:** a beginner-friendly 4-step flow to read `checkpointStatus()` and, when a round is settled, test and send `checkpoint()` from your own wallet.
- **Extras:** an opt-in browser notification when the wheel starts spinning, and a squeak button.

## Data sources

Read-only, public endpoints:

- `https://api.squeekchain.com/v1/status`
- `https://api.squeekchain.com/v1/measurement/stats?range=7d`
- Sepolia RPC `https://ethereum-sepolia-rpc.publicnode.com` for `checkpointStatus()`

"Unknown" means the data is unavailable, not zero.

## Checkpoint helper: safety notes

- It talks only to the SqueekMining proxy `0x3beE588fC20dE788Bac17d33b41Da7346cb27EC4` on Sepolia (chain ID `11155111`), copied from the [official deployment record](https://docs.squeekchain.com/assets/deployments/sepolia.json). Verify it before signing.
- The only write function is `checkpoint()`, which takes no input and is permissionless.
- The page never asks for, stores or sees a private key or seed phrase. Your wallet signs.
- It re-reads `checkpointStatus()` before testing and before sending, simulates the call first, and never retries automatically.
- Please send one checkpoint per settled round. Repeating transactions gives no advantage.

## Run it locally

No build step. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

## Tech

Plain HTML, CSS and JavaScript in one file. [ethers.js](https://github.com/ethers-io/ethers.js) v6.13.4 is loaded from cdnjs with a Subresource Integrity hash for the wallet features. Fonts: Barlow Condensed, IBM Plex Mono and VT323 from Google Fonts.
