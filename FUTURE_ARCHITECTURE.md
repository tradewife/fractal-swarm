# Future Architecture Branch — Starting Point

## Context

This branch is for designing and building the next layer of the autonomous trading system. The Binance system (on `master`) is working: night shift runs overnight, paper trader trades live, fast/full sim calibration is solid.

The goal is to move from "human reviews night shift results and deploys configs" to "system detects, proposes, and deploys autonomously."

## Current System State (master branch)

- Night shift: 12-phase pipeline (data → WFA → grid search → Darwinian → BB → experiments → regime → report → validation → discrepancy detection)
- Self-correction: calibration + discrepancy detector (Phase 8)
- Paper trader: per-symbol configs, ADX filter, state persistence
- All 4 symbols validated profitable through full simulator

## What to Build Here

This is a design + implementation branch. Explore, prototype, and iterate. Key modules to consider:

1. **`auto_config_updater.py`** — read night results, if candidate passes confidence threshold, propose paper_trader.py config change
2. **Per-regime config switching** — paper trader detects regime change and picks appropriate config set
3. **Performance degradation detection** — if paper trader PnL drops over N days, trigger ad-hoc night shift
4. **Multi-strategy portfolio** — BB Mean Reversion + MultiTF confluence running simultaneously
5. **Data refresh automation** — periodic OHLCV refresh without manual intervention

## Constraints

- Keep `per_symbol_optimizer.py` calibration invariants intact (ATR = vol×price, MR = daily only, Sharpe = actual freq)
- Night shift pipeline must remain deterministic and testable locally
- Any new module should be standalone (importable independently)
- Full-sim validation is the ground truth — fast sim is only a filter

## Files

- `CLAUDE.md` — full project documentation, calibration invariants, architecture
- `HANDOVER.md` — system state and pending tasks from Apr 6 session
- `scripts/night_shift.py` — main pipeline (reference for how modules integrate)
- `scripts/evaluator_calibration.py` — example of a standalone self-correction module
- `scripts/discrepancy_detector.py` — example of a post-night-shift check module
- `scripts/validate_night_shift.py` — how fast sim bridges to full sim
