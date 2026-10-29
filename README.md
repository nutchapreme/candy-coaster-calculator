# 🎢 Candy Coaster Engineering Calculator

A single-page web calculator for estimating speed, g-forces, dispatch timing, and throughput of a small multi-train "candy coaster" (trains carry candies instead of riders). Includes a read-only VelociCoaster reference preset as a sanity check.

**🔗 [Try it live](https://nutchapreme.github.io/candy-coaster-calculator/)**

No install, no dependencies. Runs in any browser.

## Features

- **Two presets**
  - **Custom Coaster**: editable inputs for your own design
  - **VelociCoaster Reference 🔒**: read-only, uses published ride data + illustrative assumptions
- **Imperial / Metric toggle**: values are stored internally in SI, so switching units never compounds rounding
- **Per-block clearance times**: block inputs auto-generate from the block count
- **Input validation**: out-of-range or non-integer values are highlighted red
- **Design warnings**: flags issues like height < 600 mm, too few blocks (N+1 rule), high lateral g
- **Export**: download results as CSV, or print / save as PDF

## Inputs

| Input | Unit | Notes |
|---|---|---|
| Highest point | ft / m | Requirement ≥ 0.600 m |
| Actual vertical drop | ft / m | Crest to bottom |
| Speed at top of drop | mph / m/s | 0 for release from rest |
| Ride track length | ft / m | Excludes station and lift |
| Average ride speed | mph / m/s | Separate from peak speed |
| Number of trains | — | ≥ 2 for this project |
| Number of controlled blocks | — | Includes station; should be ≥ trains + 1 |
| Candies per train | — | Full capacity |
| Lift/return time | s | Time not already in ride time |
| Station occupancy | s | Load, unload, clearance |
| Drop energy retained | % | Applies to gravitational energy only |
| Turn radius | ft / m | Representative flat turn |
| Braking deceleration | m/s² | Assumed constant |
| Eight-hour uptime | % | Assumed operating fraction |
| Block clearance (per block) | s | Min time between trains entering that block |

## Outputs

- Ideal and energy-adjusted bottom speed
- Ride travel time and full train cycle time
- Dispatch interval (scheduling lower bound)
- Theoretical throughput (candy rides/hour) and 8-hour estimate
- Unbanked turn lateral g and total felt g
- Ideal braking distance

## Equations

| Quantity | Model |
|---|---|
| Ideal bottom speed | `sqrt(v₀² + 2gh)` |
| Energy-adjusted speed | `sqrt(v₀² + 2ghη)` |
| Ride travel time | `ride length / average speed` |
| Full train cycle | `ride time + lift/return time + station time` |
| Dispatch interval | `max(cycle / trains, station time, max block clearance)` |
| Throughput | `3600 × candies per train / dispatch interval` |
| 8-hour throughput | `throughput × 8 × uptime` |
| Lateral acceleration | `v² / (r × 9.81)` in g |
| Total felt g (unbanked) | `sqrt(1 + lateral g²)` |
| Braking distance | `v² / (2 × deceleration)` |

## Limitations

This is a **planning tool, not a safety validation.**

- Dispatch interval is a theoretical lower bound. Real operation is slower due to block interactions, sensors, and control logic.
- It does **not** determine a minimum safe track length. Train length, block lengths, stopping distance, and sensor latency must be tested separately.
- Bottom speed assumes a vertical drop; energy loss applies to the drop only.
- Turn g assumes an unbanked flat turn at bottom speed.
- Braking distance excludes reaction/clearance distance.
- Powered launches break gravity-only speed comparisons.

## VelociCoaster Reference Preset

Published values: 4,700 ft length, 155 ft height, 70 mph peak, 4 trains × 24 riders.

All other values (crest speed, average speed, blocks, station time, energy retention, turn radius, braking, uptime) are **illustrative assumptions**, not real ride data.

Its 70 mph peak comes from the second LSM launch, not gravity. The calculator only models the top-hat drop, and the crest speed was chosen so the math lands near 70 mph, so treat the match as a consistency check, not independent validation.

## Usage

1. Open the [live calculator](https://nutchapreme.github.io/candy-coaster-calculator/)
   (or download `index.html` and open it locally)
2. Pick a preset, choose units, and edit inputs
3. Export CSV or print to PDF
