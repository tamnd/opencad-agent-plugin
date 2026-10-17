# Wall bracket

An L bracket of 6061 aluminium that holds a shelf on a wall. Two M5 screws go through the riser into the wall, and two M5 screws hold the shelf on the base. The inner corner has a fillet of 6 mm to take the load.

## Parts

| Part | Process | Material | File |
| --- | --- | --- | --- |
| bracket | CNC from an L extrusion | 6061 aluminium | bracket.ocl |
| 4 screws | Bought, ISO 4762 M5 | Steel | none |

## Log

Phase 1, intake: requirements.toml has R1 to R4, and assumption A1 for the screws.
Check: the user confirmed R1 to R3. A1 is an assumption.

Phase 2, architecture: one machined part and four bought screws.
Check: the part has a process, and the screws have their standard.

Phase 5, parts: bracket.ocl has the L profile, the fillet, two holes in the base and two holes in the riser.
Check: model_check profile=standard passed. expected-checks.json has the result of each check.
