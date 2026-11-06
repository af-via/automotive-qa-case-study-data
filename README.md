# Automotive QA case study — candidate data pack

All records in this pack are deterministic synthetic examples. They do not contain real vehicles, people, routes, or customer data. Do not interpret the sample as production safety evidence.

## Files and join keys

| File | Contents | Join key |
|---|---|---|
| `scenario_catalog.csv` | Ground truth, scene conditions, road type, speed, distance, and annotation quality | `scenario_id` |
| `fleet_runs.csv` | Capture time, trip, synthetic vehicle, vehicle variant, and ECU application versions | `fleet_run_id`; links to `scenario_id` |
| `model_predictions.csv` | Predictions, confidence, model version, inference latency, and prediction timestamp | `fleet_run_id` + `model_version` |
| `network_events.csv` | Time-aligned CAN and Ethernet event intervals, counters, bus/error/link details, and service state | `fleet_run_id` |
| `ecu_manifest.csv` | ECU hardware, bootloader, application, calibration, build, and network per variant | `vehicle_variant` + `ecu` |
| `diagnostic_snapshots.csv` | Synthetic diagnostic snapshot for each ECU/run | `fleet_run_id` + `ecu` |

`scenario_catalog.csv` uses `actual_pedestrian` values 1 (present) and 0 (absent). `object_distance_m` is blank when no pedestrian is present. `annotation_status=review` marks labels that should be treated cautiously. Model versions 2.4.1 and 2.5.0 are paired against the same captures where records are present.

The pack includes a small number of deliberate data-quality edge cases, including repeated or missing model-prediction records. Identify and report them; do not silently discard or impute records. An exact duplicate may be excluded from exploratory metrics only after it is called out. A conflicting duplicate should make the affected result invalid.

## Exercise rules

- A risk-priority case has a pedestrian present and either low visibility or object distance at most 20 m. Flag missed detections in those cases.
- The exercise p95 inference-latency limit is 150 ms.
- Each network row contains its own `expected_max_gap_ms`; flag an observation that exceeds it.
- These are interview exercise limits, not production safety criteria. State assumptions and do not infer causation from correlation.

The pack requires no proprietary diagnostic software or vehicle hardware. It is intended to be shared as input data only; do not add interviewer notes, expected answers, or reference-solution files to the candidate repository.
