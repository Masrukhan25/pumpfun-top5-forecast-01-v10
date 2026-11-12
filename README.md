# Solana pump.fun Top-5 Market Cap Forecast

A deterministic Python tool that forecasts the **top 5 new Solana coins by market cap 24 hours out**, using house snapshots of pump.fun coins. It reads historical snapshots and a task file from an input directory, runs a Monte Carlo simulation of candidate market-cap paths, and writes a forecast JSON to an output directory.

## What it does

Given a set of recent pump.fun snapshots and a task specifying the forecast horizon, the program:

- Simulates candidate market-cap paths for each coin over the forecast horizons.
- Ranks coins by simulated market cap at the horizon and estimates per-place probabilities (1st, 2nd, 3rd, 4th, 5th).
- Produces an ordered top-5 guess.
- Estimates per-place newcomer market-cap size probabilities.
- Shrinks the unnamed share by applying a zero-probability naming trick to coins ranked 6–10.
- Uses a horizon-mixture size model with per-place newcomer size distributions.
- Steps the number of simulated newcomers by forecast horizon.
- Keeps only named coins above a minimum probability as order rows.

The output is a single JSON file containing per-place coin probabilities, the ordered top-5 guess, and per-place market-cap size probabilities.

## Requirements

- Python 3 (standard library only)
- No external packages, AI models, providers, or APIs are used.

## How to run

Set the input and output directories as environment variables and run:

```bash
export MAKEFUN_INPUT_DIR=/path/to/input
export MAKEFUN_OUTPUT_DIR=/path/to/output
python3 main.py
```

The program reads history snapshots and the task file from `MAKEFUN_INPUT_DIR` and writes the forecast JSON to `MAKEFUN_OUTPUT_DIR`.

## Input

- **History snapshots**: house snapshots of pump.fun coins over time.
- **Task file**: specifies the forecast horizon and related parameters.

## Output

A forecast JSON with:

- Per-place coin probabilities (1st through 5th).
- An ordered top-5 guess.
- Per-place market-cap size probabilities.

## Method in brief

The core of the tool is a Monte Carlo simulation of candidate market-cap paths over the forecast horizons. For each path, the number of simulated newcomers is stepped by forecast horizon, and a horizon-mixture size model with per-place newcomer size distributions is applied. A zero-probability naming trick is used to shrink the unnamed share by assigning zero probability to coins ranked 6–10. Only named coins above a minimum probability are kept as order rows in the final forecast.

The simulation is fully deterministic given the same inputs, so repeated runs produce identical results.

Measured pooled faithful-backtest score: -32.788 overall (order component -18.979, size component -13.809) over ten reads in four splits; every split is at least as good as the previous best version.

## Notes

- This entry is standard-library only and does not depend on any external services.
- Backtest scores and performance numbers will be added later and are intentionally omitted from this version.
