# The ten phases

Each phase has an output in the project and an exit check. Do not start a phase before the check of the one before it passes. At the end of each phase, add two lines to DESIGN.md, as in this example:

```text
Phase 5, parts: bracket.ocl has the L profile, the fillet and four M5 holes.
Check: model_check profile=quick passed, build b3:fda08997f6e0d6aacc038302.
```

## 1. Intake

Write requirements.toml. Give each requirement an id, a text, a number with its unit and a source. The source is "user", a standard such as "ISO 273", or an assumption such as "assumption A1".

```toml
[R1]
text = "Mass in 6061 aluminium"
max = "60 g"
source = "user"

[assumptions]
A1 = "The screws are M5, as the user did not give a size."
```

Ask the user about the two largest unknowns, if the host lets you ask. If not, write each assumption in requirements.toml and in DESIGN.md.

Check: the user confirms the requirements, or DESIGN.md lists each assumption.

## 2. Architecture

Write the part list in DESIGN.md. Give each part its process, such as CNC, sheet metal, FDM or molding, and its material. Give each bought part its standard and size, such as `ISO4017 M6x20`. Write the concept in one paragraph.

Check: each part has a process, and each bought part has a catalog name. Call docs_search with `kind = standard_part` to find the standards that `fastener` makes.

## 3. Skeleton

Write skeleton.ocl. It has the frames, the axes, the envelopes and the key dimensions as params. Put the key relations in `requires` lines, such as `requires shaft_gap > 2 * bearing_width, "R4 room for the bearings"`.

Check: model_build passes, and each `requires` line of the skeleton passes.

## 4. Interfaces

Write one file in interfaces/ for each interface between parts, such as a bolt pattern, a bearing seat or a connector cut-out. Both parts read their numbers from the same file, so they cannot drift.

Check: each interface has a part on both sides in phase 5.

## 5. Parts

Write one file in parts/ for each part. Build them in the order of their dependencies. Start each part from docs_examples when one is close. Build after each feature, not after the whole part. Put the requirements of the part in `requires` lines in the part.

Check: model_check profile=quick passes for each part. Before you give a part to the user, model_check profile=standard passes too.

## 6. Assembly

Write assembly.ocl with the parts, the mates and the standard parts.

Check: no mate is unsolved, the degrees of freedom are the expected ones, and no two bodies overlap. model_check profile=standard has the stage `interference`, which checks the overlap. asm_solve checks the solve, the overlap, the clearance and the fasteners, and asm_motion checks the motion.

## 7. Function

Bind a calculation or a simulation to each functional requirement, such as a stress or a bearing life.

Check: each functional requirement has its evidence. This needs calc_run or sim_run from a later release. Until then, write the hand calculation in DESIGN.md with its numbers and its source.

## 8. Make

Give each part its process and fix the DFM findings, one verified fix at a time.

Check: dfm_check gives no major or blocker finding for each part, or DESIGN.md has a waiver with its reason. This needs dfm_check from a later release.

## 9. Document

Make the drawings, the PMI and the BOM.

Check: the completeness check of drawing_make passes. This needs drawing_make from a later release.

## 10. Release

Make the release pack: STEP AP242, the drawings as PDF, 3MF, DXF, the BOM as CSV and the verdict.

Check: model_check profile=release passes. A quote or an order always needs a person. Never ask for payment details or vendor credentials.
