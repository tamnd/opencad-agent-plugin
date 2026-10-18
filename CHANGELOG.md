# Changelog

Every release gets a section here, written by hand, and the release workflow publishes that section as the release notes. The version is the workspace version in `Cargo.toml`, and a tag that has no section here does not become a release.

## 0.8.6

This patch includes six PRs merged since v0.8.5 across the release, the agent plugin, the MCP server, the bench and Part Design.

- The release archives of `opencadd` for Linux and macOS have the OCCT kernel, so `opencadd build` and `model_build` make solids with a binary from the release page. The OCCT libraries are in `lib` next to the binary. The Linux builds are for glibc 2.35 or later, with the targets `x86_64-unknown-linux-gnu` and `aarch64-unknown-linux-gnu`. Windows keeps its binary without the kernel (#2703, #2717).
- The launcher of the agent plugin downloads the release archive, checks its SHA-256 before it unpacks it, and checks the binary in the cache each time it runs. The MCPB bundles for Linux and macOS have the OCCT libraries in `server/lib` (#2703, #2719).
- The interference check of `asm_solve` and of `model_check` finds the pairs that touch with the distance of OCCT, so the checks of the bolted joint of the tests take 82 ms in a debug build, where they took more than 50 s. A screw in a hex nut of its size on its axis is the verdict `thread` in `asm_solve` and the info finding `interference.thread` in `model_check`, not an overlap (#2696, #2722).
- The bench has a weekly run of the MCP Bench in Claude Code and in Codex, with a report of the pass rate of each family for each host. `bench/run.py` has the new options `--family`, `--shard` and `--report` (MW29, #2716).
- The list of Multi-Transform has the context menu of FreeCAD, with the Scale step and the modes Extent and Spacing. Multi-Transform on a selected pattern or mirror turns it into a multi-transform in its place (PD-W10, #2720).
- A Shape Binder or a Sub-Shape Binder goes into the active body, as in FreeCAD. The Part Design steps of the journeys are in a body, and a Part Design command that makes the body active opens its panel in the same frame (#1448, #2718).

The MCP server gives 14 tools, about 5,266 tokens for the model, as in 0.8.5. The hash of the default tool list is `b3:41ea31ef52bee4004024f274dc22556baf1ce46f358ad949d8013469981427a9`, because the output schema of `asm_solve` lists the verdict `thread`.

## 0.8.5

This patch includes 14 PRs merged since v0.8.4 across the MCP server, the agent plugin, the release, the bench, the documents and Part Design.

- The new command `opencadd hook` runs the hooks of the agent plugin. `guard-requirements` asks the person before an edit removes or loosens a requirement, `build-reminder` asks the agent to build after an edit of an OCL file, and `guard-external` asks before a quote, an order or a dispatch to a shop (MW26, #2700).
- The agent plugin for Claude Code and Codex is in `plugin/`. It gives the MCP server, the two skills and the three hooks in one install. Its launcher finds `opencadd`, or downloads the release binary and checks its SHA-256. `cargo xtask plugin` puts the tree of `tamnd/opencad-agent-plugin` together (MW26, #2702).
- A release now has one MCPB bundle of the MCP server for each platform, for Claude Desktop. `opencadd mcp --project` with an empty value stops with an error, and does not serve the current folder of the host (MW27, #2704).
- A release now has the CycloneDX SBOMs of `opencadd` and of the desktop app. A new job builds the Linux x86-64 binary again and compares it byte for byte (MW27, #2705).
- `server.json` is the entry of the server in the official MCP registry, `io.github.tamnd/opencad`, and the release publishes it (MW28, #2706).
- The bench has 25 assembly tasks of family A, from a planetary gearbox to a scissor lift. The grader has the new checks `assembly`, `interference` and `motion`, and a `motion` check with `collides = true` passes only when a stop holds the part (MW29, #2707, #2709, #2710 and the tasks A-018 to A-025).
- `asm_motion` holds each free motion that the drive does not set, at its value where the assembly starts. A sweep of the elbow of an arm turns only the forearm (#2712).
- The tab of a modified document shows `*`, and Close, Close All and Exit ask to save it with the "Unsaved Document" box of FreeCAD 1.1.4 (DOC-W08, #2711).
- A Part Design feature opens its task panel again from the model tree. Double-click it, or select it and press Enter (PD-W15, #2699).
- The 16 Part Design primitive panels open with the sizes of FreeCAD 1.1.4. A subtractive primitive in a body with no solid and a wedge with a length of 0 give the texts of FreeCAD (PD-W09, #2701).
- The Hole panel has the 10 thread tables and the 11 head tables of FreeCAD 1.1.4, and the core refuses a bad hole with the texts of FreeCAD. The Draft panel opens with an angle of 1.5 deg (PD-W08, #2708).

The MCP server gives 14 tools, about 5,266 tokens for the model, as in 0.8.4. The hash of the default tool list is `b3:0d2e9ff87879ce86e034a7d1a6402d31fe60400175d99a046351894114701524`.

## 0.8.4

This patch includes five PRs merged since v0.8.3 across the MCP server, the test tools and the Forge kernel.

- The MCP server has the new tool `part_search` in the toolset `assembly`. It finds the standard screws, nuts, washers, pins and ball bearings of the bundled library and gives the OCL statement that puts each part in a model, with its key dimensions. A query such as `M6x20` or `bore 30 mm` limits the sizes, and `standard` limits the search to one family of standards (MW25, #2695).
- `asm_solve` has the new check `fasteners`. For each screw it finds the hole in each part, the length of thread that engages a threaded hole or a hex nut, and the room for a tool over the head. Each finding names the screw, the rule, the parts and the measured value (#2694, #2697).
- `cargo xtask desktop-journey` starts the app again for each journey, so each journey starts with empty storage. The new options `--journey ID` and `--binary PATH` run some journeys on a test build that you made before (#2692).
- The checks of the invariants in the test kit wait for 2 frames in WebKit too, as they do in Chrome (#2693).
- Forge has a behavior note for the OCCT approximation `AdvApp2Var`, with probes and OCCT data. Nothing in Forge uses it yet (SRF-W24, #2691).

The MCP server gives 14 tools, about 5,266 tokens for the model. The hash of the default tool list is `b3:0d2e9ff87879ce86e034a7d1a6402d31fe60400175d99a046351894114701524`.

## 0.8.3

This patch includes eight PRs merged since v0.8.2 across the MCP server, OCL, the documents, Part, Part Design and the Forge kernel.

- The MCP server has the new toolset `assembly` with the tools `asm_solve` and `asm_motion`. `asm_solve` solves the joints of an assembly and gives the free motions, the redundant and conflicting joints, the interference and the clearance of each pair of parts. `asm_motion` turns or slides one joint through a range and gives the smallest gap of each pair and the ranges where two parts collide. The new OCL checks `min_clearance_over` and `reach` are worked out by `asm_motion` (MW24, #2687).
- A `use` now takes the top-level parts of another file, and the bodies and sketches that a file publishes. The model that uses them builds only the features that they need. A feature of another file that does not build gives an error at the `use` that names the file (#2689).
- The app keeps more than one document open, each in a tab of its own, as FreeCAD 1.1.4 does. Each document keeps its model, its undo history and its selection (DOC-W07, #2682).
- File > New names a document Unnamed, Unnamed1, Unnamed2 and so on, by the rule of FreeCAD 1.1.4 (DOC-W09, #2685).
- The journey J-PT-06 builds the bottle of the Part tutorial. `part.shell` takes a point on each face to remove, and the datum commands take a point on a face (PT-W17, #2680).
- The K-CMD and K-TASK contracts run on web for the 24 command and task rows of PD-M1. New Body with a whole Body selected makes an empty body, and the Part Design panels have the box titles of FreeCAD (PD-W15, #2688).
- Forge has a clean room port of the OCCT plate solver `Plate_Plate`, from a new behavior note with probes and OCCT data. Nothing calls it yet, and the Filling still runs on OCCT (SRF-W24, #2683, #2684).

The MCP server gives 13 tools, about 4,957 tokens for the model. The hash of the default tool list is `b3:6466ecc7bb42b76393daf95b53783ee4e6b8e66293693cb624bbb54f7d3242bf`.

## 0.8.2

This patch includes four PRs merged since v0.8.1 across the MCP server, the Sketcher and the desktop build.

- A model can now be in more than one file. A file gives values to other files with `publish`, and a model takes them with `use`. A model can also use a package from `packages/`, which `ocl.lock` pins by its hash. A package that changed after its pin gives OCL0505 and does not build. Each file that a model uses goes into the build id, and a change to a value rebuilds only the features that use it (MW22, #2675).
- A reference can now pick a face by its role with `faces.side`, `faces.blend` and `faces.inner`, the faces of a sweep, of a fillet or chamfer and of the cavity of a shell. The legend of a render and `model_describe` give a reference by role for these faces, and each face in the legend and in `ref_resolve` has the new field `role` (MW23, #2677).
- Each Sketcher command names its undo step with the text that FreeCAD 1.1.4 gives its transaction, such as "Add sketch line" or "Trim edge" (SK-W14, #2676).
- The desktop CI build and the release build link OCCT, so a desktop app from a release builds solids, and CI tests the kernel code of the desktop app (PD-W14, #2674).

The MCP server gives 11 tools, about 4,050 tokens for the model. The hash of the default tool list is `b3:5d0315cc162ba235ea12a47b3c2ce31f605f361ee18f31dd22d80bc2d55c537b`.

## 0.8.1

This patch includes seven PRs merged since v0.8.0 across the MCP server, Part Design, Part, the documents and the Surface journeys.

- The new MCP tool `export` and the command `opencadd export` write a model or an assembly as one STEP AP242 file with the part names, the placements, the colors and the assembly structure. The inserts of one body share one product. The tool writes only inside the project root, does not replace a file outside `release/`, `drawings/` and `.opencad/` without `overwrite: true`, and gives each file with its sha256 (MW21, #2672).
- The face pick of New Sketch and the edge pick of Fillet and Chamfer now work on a Pad or any other modeled body, on web and on desktop (PD-W12, #2666).
- File > Open Recent lists the last 4 files as FreeCAD 1.1.4 does, with the full paths on desktop, File not found for a file that is gone, and Clear (DOC-W10, #2667).
- The journey J-PT-07 makes a solid from the Ahmed mesh as FreeCAD makes it, with the names that the FreeCAD GUI gives the bodies (PT-W17, #2668).
- The draggers of the Part Design features stay in the page on desktop, and the native `draggers` command that had no caller is gone (PD-W13, #2669).
- While a Part Design panel previews, the 3D view draws the ghost of the feature over the model, on web and on desktop, for each PD-M1 feature (PD-W11, #2670).
- The Surface journeys J-SF-01 to J-SF-04 pass in the macOS desktop test build, and the import line of a surface model gives its shells (#2671).

The MCP server gives 11 tools, about 4,050 tokens for the model. The hash of the default tool list is `b3:5d0315cc162ba235ea12a47b3c2ce31f605f361ee18f31dd22d80bc2d55c537b`.

## 0.8.0

This minor release ends milestone M0 of the Sketcher (#1442). Each of the 269 rows of the Sketcher topic now has a FreeCAD 1.1.4 oracle value from `fc-drive` in opencad-compat: 143 rows from a step that runs the command, 125 from the state of the command, the menu or the key in edit mode, and the Profiles menu with a reason, because FreeCAD 1.1.4 has no Profiles menu. The ledger has the 19 gap rows, the 19 Sketcher twins give their golden values on FreeCAD 1.1.4 (SK-W03, #2479), and open questions 1 to 4, 7, 9 and 12 have answers. It includes four PRs merged since v0.7.0.

- Each command of `ocad-core` has a JSON Schema that comes from the types of its arguments. The toolset `commands` of `opencadd mcp` gives the commands through `tools_search` and `command_run`, and the built-in agent and the server read one registry (MW19, #2662).
- File > Open shows the native dialog of FreeCAD with its title and filters, takes more than one file, opens the files of a drop anywhere on the window, and on desktop opens the files of the command line and of the Finder (DOC-W06, #2663).
- The stage `interference` of `model_check` measures each pair of bodies of an assembly and gives the volume, the depth and the witness points of each overlap. A joint can declare a fit, such as `fit = press(0.05 mm)`, and the OCL checks `no_interference()` and `min_clearance(a, b)` work on the placed parts (MW20, #2664).
- A line from `proximity_line`, an `inertia_axis` mode or `intersection_line` on a body slides to the place nearest the origin of the placement of the body, as the attacher of FreeCAD does (#2650).

## 0.7.0

This minor release ends milestone M0 of Part (#1452). Each Part journey from J-PT-01 to J-PT-10 now has a FreeCAD 1.1.4 twin on macOS and on Linux, and the GUI run of each one gives the values of its `golden.json`. It includes four PRs merged since v0.6.0.

- The journey J-PT-05 checks a supplier part. The user removes a hole of `Schenkel.stp` with Defeaturing, copies a face with Shape Element Copy, paints three faces with Appearance per Face, imports the NIST CTC-01 part, runs Check Geometry on both parts and exports STEP. `part.defeature` takes `at`, points on the faces of an imported body. The new command `part.element_copy` copies one face or one edge of a body. A copy now leaves its body in the view, as Simple Copy, Shape Element Copy and Clone do in FreeCAD (PT-W16 part 4, #2659).
- The journey J-PT-02 selects the body before Cross-Sections, as the FreeCAD rule of that command needs a selected shape (#2660).
- The regeneration cache no longer gives a feature the body of a feature with the same inputs and another id. Before, a second model in one `opencadd serve` session could get the face names of the first, and a pad up to its caps failed (#2649).
- The desktop app opens an FCStd file that has a feature but no shape that reads, in place of a panic (#2658).

## 0.6.0

This minor release ends phase P0 of the MCP roadmap. A coding agent such as Claude Code or Codex can now build, check, measure and render an OCL model through the OpenCAD MCP server, and each pull request proves that the server follows the protocol. It includes four PRs merged since v0.5.24.

- Each pull request runs the official MCP conformance suite on the server for protocol 2025-11-25 and for protocol 2026-07-28, and a failure blocks the merge. `scripts/conformance/run` runs it locally. The server now declares `prompts` with an empty list, and the error of an unknown resource has the URI in `data`. `opencadd mcp --list-tools` prints the tool list with its hash and its count of tokens, and the tests keep the names, the descriptions and the examples of each tool in their contract (MW18, #2656).
- The host smoke tests run Claude Code and Codex headless on the bracket, the enclosure and the gearbox each night, and check what reaches the model. `python3 bench/hosts.py` runs them locally (MW17, #2654).
- File > Save, Save As and Save Copy write a `.ocad` file in the web app and in the desktop app, and File > Open opens one. The first Save of a document from an FCStd or OCL file asks for a name, so OpenCAD never writes over a FreeCAD file (DOC-W05, #2655).
- The journey J-PT-04 builds a crank from `Crank.fcstd` with a fuse, primitives, a variable fillet, a chamfer, a tapered extrude, a revolution and a mirror. A second Union starts as `Fusion001`, as in FreeCAD (PT-W16 part 3, #2653).

The MCP server gives 10 tools, about 3,619 tokens for the model. The hash of the default tool list is `b3:e86e02a92d6e31685ceecb758395dd7f215f576fbcf41fefc1f0b1e323b8d38c`.

## 0.5.24

This patch includes five PRs merged since v0.5.23 across the MCP server, the agent skills, the bench, Part Design and the file formats.

- The skills `opencad-design` and `opencad-ocl` teach an agent in Claude Code or Codex the loop of build, check and measure. `opencad-design` has the ten phases from a brief to a checked part, 15 failures with their recoveries and a bracket example that the tests build. `opencad-ocl` has the rules of OCL, a table from CadQuery, build123d and OpenSCAD, and recipes for shell, draft, loft, sweep, thread and gear. `.claude/skills/` and `.agents/skills/` link to them (MW15, #2648).
- The MCP Bench has 10 part tasks, each with a brief, hidden requirements, a reference and a grader. `python3 bench/run.py --host claude` or `--host codex` runs the tasks headless and prints the build validity, the requirement pass rate and the false done rate. Claude Code 2.1.302 passes all 10 tasks with a false done rate of 0 (MW16, #2651).
- A datum mode that takes an edge can follow a line, an arc, a circle, an ellipse or a spline of a sketch (#2647).
- A saved `.ocad` bundle keeps each file that the OCL names with `blob("sha256:...")`, such as the BREP files of an FCStd import, so the document builds again after it opens (SRF-W25, #2510).
- The 3MF reader has a fuzz target and round trips the FT17 corpus, with three fixes that the new checks found (FW37 part 3, #2646).

## 0.5.23

This patch includes nine PRs merged since v0.5.22 across the MCP server, Part, Assembly, the shell and the Forge kernel.

- `opencadd tool NAME --args JSON` runs one MCP tool from the command line and prints the same JSON as the server (MW12, #2636).
- `opencadd mcp` sends kernel work to a pool of worker processes. A crash gives `kernel_crash`, a build past its deadline gives `timeout`, and the server stays up. On Linux and macOS a worker has no network and writes only to `.opencad` of the project (MW13, #2639).
- `opencadd mcp` has the tools `docs_search` and `docs_examples` and the resource `ocad://docs/card`. The new docs/ocl/diagnostics.md gives the fix of each OCL code, docs/ocl/examples.md has eight models that build, and docs/llms.txt indexes the docs in one file (MW14, #2644).
- Part_Fillet has a variable radius, and Part_Mirror can keep the mirror image alone (PT-W16 part 2, #2640).
- The joint task of Assembly shows the fields of FreeCAD for each joint type, with Reverse and the offsets, and a double-click on a joint edits it. The New Part panel makes a part with a body in the active assembly (ASM-W11, ASM-W09, #2637, #2643).
- The Windows menu lists the open windows and opens the Choose Window dialog, and a drag moves a document tab (SHL-W16, #2641, #2642).
- Forge matches the tangential deflection walk of OCCT, and the quasi uniform abscissa sweep has 1141 cases (GW20e, #2638).

## 0.5.22

This patch includes eleven PRs merged since v0.5.21 across the MCP server, Part Design, Part, Assembly, the desktop app and the Forge kernel.

- `opencadd mcp` has the tools `model_describe`, `ref_resolve`, `measure` and `render`. An agent gets its numbers from the B-rep with units, the method and the build id, and a numbered render names each face with a stable reference (MW10, #2625).
- `model_check` runs the profile `quick` or `standard` and gives a verdict with stages, findings and fix hints. Against a git ref or a build id, it marks each new finding and lists each `requires` that the change removed or made weaker (MW11, #2633).
- Part Design has the second side and Up to shape for Pad and Pocket, Up to face for Revolution and Groove, Recompute on change in its panels, and Transform body for the patterns and the mirror (PD-W07 parts 2 to 5, #2629, #2631, #2632, #2634).
- A plain `Part::Feature` of an FCStd file comes in as a feature (PT-W16, #2626).
- In the native view of the desktop app, a part drags with the mouse or with one finger, and the moving joint shows its frame (ASM-W13 parts 3 and 4, #2630).
- The shell contract rows run on the Linux desktop app (#2627).
- Forge makes the circle solvers on general curves and `Approx_CurveOnSurface` faster with the same bits, and fits `PointsToBSpline` by normal equations (GW23n, GW25c, #2624, #2628).

## 0.5.21

This patch includes four PRs merged since v0.5.20 across the MCP server and Part.

- `opencadd mcp` is the MCP server for coding agents such as Claude Code and Codex. It runs on stdio and has the tools `project_info`, `model_build` and `job_get`. Each path stays in the project root, and a build that takes longer than the inline limit goes on as a job that a new server can still report (MW8, #2621).
- Each MCP result is one JSON object with `summary` first, as text and as `structuredContent`, in a budget of about 6,000 tokens. A list over the budget is cut with a count, each error has a code, each tool declares an `outputSchema`, and `model_build` takes `detail` (MW9, #2622).
- Part has the Attachment editor panel of FreeCAD (PT-W20, #2619).
- A copy turns about a datum line, and a draft turns about a datum plane or a face (#2620).

## 0.5.20

This patch includes fourteen PRs merged since v0.5.19 across F-rep, the build tool, Part, Sketcher, Assembly, the web app, the UI checks and the OCCT patches.

- F-rep reads and writes 3MF Volumetric and Implicit level sets with Forge fields, and the import of a file that Forge wrote gives the field back with no loss (FW37 part 1, #2597).
- F-rep reads and writes 3MF Beam Lattice 1.2 with balls, and the mesh nodes of 3MF Implicit 1.0. The Forge beam list has flat beams, so a 3MF beam keeps its exact shape (FW37 parts 2a and 2b, #2603, #2617).
- `opencadd render` builds a model and draws it to a PNG with no display, with numbered faces and a legend (MW6, #2602). The new crate `ocad-mcp` is the place for the MCP protocol adapter (MW7, #2605).
- A 3D sketch solves, and a sweep can follow it (#2604).
- The web viewport draws the face colors of an appearance (#2607).
- In Assembly, a click in the 3D view picks the element and the frame of a joint (ASM-W10, #2598). A part drags with the drag mode of its joint, and Ctrl drags the selected parts (ASM-W13, #2616).
- The desktop app checks the Primitives panel (PT-W18, #2601). The UI sweep clicks the buttons in the body of each panel (#2599), closes a message box that shows after the app is idle (#2600), and starts again when a click leaves a task dialog open (#2608).
- An OCCT patch makes the edges of a filling same parameter in parallel (#2596).

## 0.5.19

This patch includes eight PRs merged since v0.5.18 across the build tool, Sketcher, Part, Assembly, F-rep, the Forge kernel and the specs.

- `opencadd build`, `opencadd serve` and the MCP server share one build path in `ocad_agent::build` (MW4, #2589). Each build gets an id, and the bodies of a build stay in the project cache, so a second build of a part that did not change evaluates no body again (MW5, #2591).
- A sketch has the `text()` call, which draws a string in DejaVu Sans as lines and degree 2 curves, as Draft ShapeString of FreeCAD does with its defaults. Six strings match FreeCAD 1.1.4 in area to 1e-8 (#2588).
- Check Geometry has the settings page and the results of Part_CheckGeometry of FreeCAD 1.1.4 (PT-W19, #2587).
- The assembly solves after a new placement of a part and while a joint is made (ASM-W12, #2592).
- F-rep slices a field into layer contours with no mesh and writes them as CLI and SVG (FW36, #2590).
- Forge classifies a point correctly on an almost degenerate B-spline face and reads mirrors as OCCT does (GW16e, #2585).
- Step 4 of J-SK-20 goes through the 4 Rectangle modes of FreeCAD 1.1.4, which has no Square mode (#2594).

## 0.5.18

This patch includes eight PRs merged since v0.5.17 across OCL, the build tool, the web app, the renderer, F-rep, the Forge kernel and the UI checks.

- OCL diagnostics come out as records with a file, a line, a column, a help and fixes. `opencadd build --json` prints them, and a fillet that fails points at its call and gives the largest radius that works (MW2, #2580).
- `opencadd build --param name=value` and the `params` field of a serve request set the parameters of a model without an edit to the file (MW3, #2582).
- The web app says that a STEP file with shells and no solid is a surface model, not a failed import (#2573).
- The renderer draws the meshes of a frame with multi-draws from an indirect buffer where the device has them (#2581).
- F-rep has the certified mesher with the Plantinga and Vegter test (FW35, #2579).
- The approximation facades of Forge are closer to OCCT at the C2 level (GW25b, #2578), and the bisector intersection and the point and curve bisector are faster (GW23m, #2583).
- The UI checks test the contrast of each panel in the light and the dark theme (#2584).

## 0.5.17

This patch includes eighteen PRs merged since v0.5.16 across the desktop app, Part, Part Design, F-rep, the Forge kernel, OCCT, the specs and the checks.

- The desktop app starts with an empty document, as FreeCAD does, so the window shows at once. Before, it loaded the V6 engine from the corpus before it made the window. `OPENCAD_SHOW_PART=engine` opens the engine (#2567).
- A body with no faces, as a circle or a helix is, shows in the 3D view as its lines. A click on a line picks the body, and View Fit goes round it (PT-W18, #2571). The Primitives panel makes the arc of an ellipse and the circle from 3 points, as FreeCAD does (PT-W18, #2508).
- Part has the appearance feature and `appearance.face` for Part_ColorPerFace (#2513), and the face colors of FCStd files open as appearances (#2570).
- Part Design has the body, the active body and Active Body Required (PD-W05, #2250), and Set Tip and Move Feature After with a Tip for each body (PD-W06, #2509).
- Geometric requires are evaluated after regeneration (#2569).
- F-rep has the implicit body, the mixed Booleans and the mesh caches (FW34, #2507). Forge has the intersection of two bisectors (GW23l, #2505).
- OCCT is patched so `BSplCLib::Eval` does not zero its basis buffer (#2514).
- The Shaft design wizard has its loads, supports and charts (#2503).
- The spec has the agentic workflow (#2511) and the MCP server (#2512) under `spec/ai`.
- J-A11Y-01 makes the plate of J-FC-02 with the keyboard only (#2568). The soak checks for detached DOM trees and the frame p95, with three leaks fixed (#2504), and records the listener count in each sample (#2506). The video of a run longer than 15 minutes is split by phase (#2502).

## 0.5.16

This patch includes eighteen PRs merged since v0.5.15 across F-rep, Part, Assembly, Surface, Fasteners, the Forge kernel and the UI checks.

- F-rep has the mesh and B-rep leaf opcodes with the mesh surface (#2486), the B-rep surface of the B-rep leaf with group 1 of the hybrid suite (#2490), and the narrow-band cache of the B-rep leaf with the FB24 and FB25 measurements (#2499) (FW33).
- The Primitives panel shows a ghost of the next object (PT-T432, #2487), keeps the focus on Create (#2491), and leans a cylinder by its two angles as FreeCAD skews it (#2496) (PT-W18).
- The assembly in edit gives the FreeCAD solve result code and shows the solver messages (ASM-W12 part 1, #2494).
- The ledger lists the FCStd feature types, so Sewing and Cut keep web C2 (#2492). The 2 Surface command gaps are closed as ignored rows (SRF-W12, #2484).
- The Fasteners workbench has the DIN 471 and DIN 472 retaining rings (#2489). The spec has the FreeCAD speed baselines of the add-ons (ADD-W05, #2488).
- Forge has the bisector of a point and a free-form curve (GW23j, #2481) and of two curves with a free-form guide (GW23k, #2493), the quasi uniform abscissa of a polygon and the span rule of the tangential deflection (GW20d, #2459), and `ocad_libm::hypot` in the radial band check of the face classifier (#2485).
- oc-drive compares a step with FreeCAD by model, tree, properties, silhouette and edges (#2495), and records the video of a proof bundle with step captions, click marks and key marks (#2497). The UI sweep cuts a clip of 10 s around each item that did nothing (#2498).

## 0.5.15

This patch includes nine PRs merged since v0.5.14 across Part, Sketcher, Surface, Fasteners and the host checks.

- The Part dialogs have the FreeCAD undo names, and Part has the Union and Convert to Solid texts of FreeCAD (PT-W13, #2475).
- A double-click on a Part primitive opens it again in the Primitives panel with its values and its Location. OK changes it in one undo step with the name Edit, and Cancel changes nothing, on web and on desktop (PT-W18, #2478).
- The view fits all after Part_Box, Part_Cylinder, Part_Sphere, Part_Cone and Part_Torus, and after Create of the Primitives panel, as FreeCAD sends ViewFit there, on web and on desktop (PT-W18, #2482).
- The Sketcher twins run on FreeCAD 1.1.4 (SK-W03, #2479).
- J-SF-04 opens each Surface type, flips a curve, saves and opens the file again (#2473). A pick on a Surface measures only the near edges and faces (#2474).
- The DIN 103 trapezoidal thread has its pitch and starts (#2476), and the ISO 14579 cap screws get their thread (#2480).
- J-SHL-01 compares the web and the desktop after each step (S-HOST, #2477).

## 0.5.14

This patch includes six PRs merged since v0.5.13 across Surface, Part, Fasteners, F-rep and the add-on topic.

- Surface Sewing and Surface Cut from FreeCAD files now open as real features, build with the FreeCAD options and stay in the file after a save. A patch keeps its reversed flags past its last curve, and Fill Boundary Curves can edit a patch from a file (#2454).
- A Surface feature has the tree menu item Show Control Points, which draws the poles and the net of its Bezier and B-spline faces and edges as `ViewProviderSpline` does, on web and on desktop (SRF-W21, #2466).
- The Part panels open with the start values of FreeCAD 1.1.4, Part Thickness offsets the walls out as FreeCAD does, and Explode and Edit attachment have the FreeCAD undo names (PT-W13, #2468).
- The EJOT PT screws of WN 14xx get the WN thread fused on when thread is true (#2467).
- F-rep adds property fields with declared units and the attribute rules for union, intersect, subtract, the smooth operations and the maps (FW32, #2469).
- The add-on topic records the FreeCAD 1.1.4 messages for a file with no add-on, the license box defaults and the SheetMetal two-key wait (ADD-W06, #2470).

## 0.5.13

This patch includes five PRs merged since v0.5.12 across Part, Geometry and the add-on topic.

- The 24 negative tests of Part, PT-T801 to PT-T825, pass in the desktop app. The desktop app now opens an FCStd file that has one shape it cannot read, and the tree marks that object, as the web app does. A failed fillet now shows its error mark and its box on the desktop (#2461).
- Check Geometry on the web commits its results in the next frame, so the panel no longer gives two React commits in one frame. PT-T394 passes again (#2462).
- With these two changes, the Part tests PT-T380 to PT-T396 pass on web and on desktop, which is exit check 2 of Part M1.
- `gp_Lin` gets the square distance between two lines, checked on the 225 line pairs and 15 near-parallel cases of OCCT 8.0.1 (#2463).
- A `gp_Mat2d::Powered(-2)` probe pins the negative-square boundary of OCCT 8.0.1 on 4,096 matrices with signed zeros, infinities and NaNs (#2460).
- The add-on topic records its M0 facts from FreeCAD 1.1.4 and the J-ADD-05 golden (#2464).

Part M1 still needs PT-W13 and each P0 row at C4 on all 4 targets.

## 0.5.12

This patch includes nine PRs merged since v0.5.11 across Geometry, F-rep, Fasteners, Part files and desktop commands.

- The `gp_Pln` facade adds equation-coefficient construction, signed point distance, line and plane distance forms, squared distances and line containment. The public OCCT 8.0.1 fixtures cover 12 valid constructors, 50 fixed pairs and 15 signed point calls on arm64, x86_64 and WASI SIMD128 (#2450).
- A paired curve projection heap row measures 4,096 points against OCCT 8.0.1. A busy Apple M4 diagnostic measured 77,680 bytes of OCCT peak heap and 4,440 bytes of Forge peak heap, while Forge retained more heap. The resource and 10x speed gates remain open (#2452).
- `gp_Mat2d` returned and in-place inverse now match 16,384 OCCT coefficient bits on a 4,096-matrix nonfinite grid across arm64, x86_64 and WASI SIMD128. The separate negative-square NaN sign gap remains open (#2456).
- F-rep adds field-driven TPMS grading, bounded axis warps, warnings for point-dependent cell sizes and a minimum wall check. The graded part passes the wall check at 5 mm cell size and fails it at 2.5 mm (#2455).
- Fasteners fuse modeled wood threads onto wood screws, then extend that path to tapping and ground screws (#2449, #2453).
- Part Import and Part Export show the five FreeCAD file filters in the desktop file dialog (#2451).
- Check Geometry runs against the in-process OCCT kernel in the desktop app and opens the results panel for Part shapes (#2457).
- Desktop view commands use the native camera for the checked Part command path (#2375).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.11

This patch includes five PRs merged since v0.5.10 across Geometry, Fasteners and shell journey tests.

- `gp_Lin::Normal(gp_Pnt)` matches 124 returned lines and 26 construction errors from a public OCCT 8.0.1 probe on arm64 macOS, x86_64 macOS and WASI SIMD128 (#2444).
- `gp_Lin` point distance, square distance and containment match 380 value bits and 1,330 boolean results across the same three targets. The fixture and the recent axis and line fixtures now run in the per-commit WASI test list (#2447).
- Plane sections match edge count, connected parts and ancestor faces on 111 checked corpus cuts. The exact length gate is `1e-9` relative or within OCCT vertex tolerances, and the reported median OCCT to Forge time ratio is 27.7 on those cuts. Section pcurves and the seven cuts below 10x remain open (#2445).
- Fasteners cuts modelled threads and two knurled bands into heat set inserts. Eight checked FreeCAD rows match face and edge counts and the stated volume and area bounds (#2443).
- The J-SHL journeys use the projected arc stored in PartDesignExample and count the Structure bar split button correctly. All seven checked shell journeys pass (#2446).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.10

This patch includes eleven PRs merged since v0.5.9 across Geometry, the Forge field, Fasteners, Part, the shell and add-on test data.

- `gp_Mat` power now matches every finite coefficient in the 126-record public OCCT fixture on arm64, x86_64 macOS and WASI SIMD128. NaN sign parity remains open (#2435).
- `gp_Ax1::IsCoaxial` matches 2,700 public OCCT boolean records on the same three targets, including a saved mini-vacuum-pump placement axis (#2439).
- `gp_Lin::Distance(gp_Lin)` adds line distance to the math type and facade. It matches 213 of 225 ordered public OCCT line pairs and all 15 near-parallel cases bit for bit on three targets. Twelve rounding cases remain open (#2440).
- A paired line distance benchmark has equal Forge and OCCT sums over the fixture lines. Five Apple M4 pairs measured diagnostic speed ratios of 1.89 on one thread and 1.65 on ten cores. CPU frequency evidence was unavailable, so the verified 10x target remains open (#2441).
- The Forge field adds graph lattices with a BVH for FW30b (#2438).
- Fasteners cuts 55 degree BSP threads into pipe taps and dies. The checked G1/16 through G1 cases match FreeCAD on topology and the stated measures (#2437).
- Part adds `part.primitive` at a placement and a Primitives dialog with 16 types and a Location box (#2431, #2436).
- The Part Design workspace now exposes Chamfer where the PartDesign_Chamfer palette entry searches (#2434).
- Web and desktop shell tests check panel memory against the stated 150 MB renderer and 64 MB GPU limits (#2433).
- The KiCad StepUp golden and its source record cover the J-ADD-11 add-on journey (#2432).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.9

This patch includes ten PRs merged since v0.5.8 across Geometry, Fasteners, Part and command contracts.

- The `gp_Mat` power fixture now limits known finite differences to two ULPs in the checked negative powers and caps the target-specific NaN sign counts. The focused test passes on arm64, x86_64 macOS and WASI (#2422).
- `gp_Mat` inverse now matches the two zero signs in both returned and in-place OCCT records for an infinity input. The 144-record fixture requires exact zero bits on three targets; NaN signs remain open (#2423).
- `gp_Mat` power matches four more finite OCCT coefficient bits on each checked target. The 126-record fixture has 1,048 exact coefficient bits on arm64 and WASI and 984 on x86_64 macOS. The remaining 28 finite differences and NaN sign gaps keep full parity open (#2426).
- B-spline and Bezier curve evaluation uses lazy Taylor forms of knot spans for `d0` through `d3`. Checked curve calls are 2.2 to 5.6 times faster than the OCCT geometry calls on the reported server, and inherent surface calls also use the forms. The 10x target remains open (#2424).
- Fasteners cuts modelled threads into set screws, threaded rods, taps and dies. Nineteen checked threaded parts match FreeCAD 1.1.4 on topology and the stated measures (#2421).
- Fasteners cuts straight knurls and modelled threads into DIN 464, DIN 465 and DIN 653 thumb screws. Ten checked cases match FreeCAD on topology and the stated measures (#2427).
- The Part Offset panel has FreeCAD's Self-intersection and Update view fields and a live preview (#2420).
- A double-click reopens an Offset panel with the feature's values, and OK edits that feature in place. The checked task row reaches C2 (#2429).
- The command contract drive moves the pointer off a closed menu, and the Part Cylinder fixture uses FreeCAD's default radius and height. The checked Box and Cylinder command rows pass (#2419).
- The command contracts find Part datum and selection filter commands in toolbar group menus. Four datum command rows reach C2 in the checked web run (#2425).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.8

This patch includes eight PRs merged since v0.5.7 across Geometry, the Forge field, Surface, Fasteners and the A2plus journey.

- `gp_Mat2d` singularity and powers have 138 public OCCT 8.0.1 records. Six singularity results and 128 power calls match bits on arm64, x86_64 macOS and WASI SIMD128. Four negative-square calls still differ on six NaN sign bits (#2409).
- `gp_Mat` singularity and powers have 126 public OCCT 8.0.1 records. All singularity and NaN propagation results match on three targets. Power has 1,044 of 1,080 coefficient bits exact on arm64 and WASI and 980 on x86_64 macOS. Finite rounding and NaN sign differences remain open (#2413, #2416).
- A current five-pair Apple M4 knot insertion diagnostic measured 254 ns per OCCT call and 157 ns per Forge call, with equal checksums and four Forge allocations per call. CPU frequency evidence was unavailable, so the 1.62 speed ratio is not a verified baseline (#2417).
- The Forge field adds seven strut and three plate lattice primitives for additive manufacturing. Graph lattice follow-up work remains open (#2411).
- Surface records loaded server3 baselines for Filling, Extend and Blend. The Filling row measured 326.89 ms in OpenCAD and 233.33 ms in FreeCAD 1.1.4, below its target (#2410).
- Fasteners cuts blind internal threads in four cap nut standards and two pin standards, with twelve checked FreeCAD 1.1.4 cases (#2414).
- The A2plus ballpoint assembly journey now uses the saved assembly and six part files and checks Update parts and Solve against FreeCAD 1.1.4 (#2415).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.7

This patch includes five PRs merged since v0.5.6 across Geometry and Fasteners.

- Positive-knot analysis skips detailed rounding work for clear spacing jumps. Eighteen focused tests pass on arm64, x86_64 and WASI SIMD128. A loaded Apple M4 diagnostic measured 12.6 to 13.6 ns before and 10.5 to 11.0 ns after with equal OCCT checksums; the verified 10x target remains open (#2402).
- `gp_Mat` addition, subtraction, left product and scalar operations match 153 public OCCT 8.0.1 records bit for bit on arm64, x86_64 and WASI SIMD128. The fixture includes division errors and unchanged matrices after failed in-place division (#2404).
- `gp_Mat2d` arithmetic matches 153 public OCCT 8.0.1 records bit for bit on arm64, x86_64 macOS and WASI SIMD128. Zero division returns IEEE values in the checked range, unlike `gp_Mat` division (#2405).
- `gp_Mat2d` row, column and setter operations match 72 valid-index public OCCT records across three targets. Nonrotation results match bit for bit and finite rotations match within `2e-15` per coefficient (#2406).
- Fasteners cuts modelled threads into T-slot nuts, self clinching nuts and standoffs (#2403).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.6

This patch includes five PRs merged since v0.5.5 across Geometry, Fasteners and shell benchmarks.

- `gp_Mat2d` returned and in-place inverse operations match 172 public OCCT 8.0.1 records bit for bit on arm64, x86_64 and WASI SIMD128, including checked singular matrices and NaN payloads (#2397).
- `gp_Mat` returned and in-place inverses match 64 finite-result OCCT records bit for bit across three targets. Another 80 records match finite nonzero values, infinity signs and NaN payloads. NaN signs and zero signs with nonfinite inputs remain open (#2399).
- `gp_Mat` row, column and setter operations match 40 valid-index public OCCT records bit for bit across three targets. Invalid indices return a defined range error and leave the matrix unchanged (#2400).
- Fasteners adds modelled threads for self clinching studs, threaded taper pins, PCB spacers and standoffs. Twenty-one checked cases match FreeCAD 1.1.4 on topology and the stated measures (#2398).
- Six shell benchmarks now include FreeCAD 1.1.4 baselines and paired OpenCAD results. The menu, workbench switch and report frame rows remain below their targets on the checked SwiftShader run (#2396).

Broader Geometry C1 coverage and verified 10x speed and resource targets remain open.

## 0.5.5

This patch includes six PRs merged since v0.5.4 across Geometry, the Forge field, Fasteners and add-on journeys.

- Surface D0 evaluates an owned B-spline point without derivative scratch while keeping the derivative grid's bits on arm64, x86_64 and WASI SIMD128. A failed-gate Apple M4 diagnostic moved rational degree-seven D0 from 260 ns to 192 ns per point. The verified 10x speed and resource targets remain open (#2389).
- The `gp_Mat2d` and `gp_Mat` facades match 980 public OCCT 8.0.1 value, product and error records bit for bit on arm64, x86_64 and WASI SIMD128. Indexed writes keep the matrix unchanged after an error, and both fixtures run in per-commit WASI CI (#2390, #2391).
- The Forge field adds builder and OCL functions for nine TPMS shapes, sheet and network forms, density lookup and lattice fill (#2392).
- Fasteners adds modelled threads for square head, shoulder, headless, round head and carriage bolts and screws. Twenty cases across ten standards match FreeCAD 1.1.4 on the checked topology and measures (#2393).
- The Fasteners and Curves add-on journeys now follow the checked FreeCAD 1.1.4 steps for hole matching and two-rail sweeps (#2394).

## 0.5.4

This patch includes nine PRs merged since v0.5.3 across Geometry, the Forge field, Fasteners, Part and Surface.

- Geometry matches 297 public OCCT 8.0.1 calls for `gp_XY` and `gp_XYZ` normalization, coordinate access and matrix products bit for bit on arm64, x86_64 and WASI SIMD128. The broader gp and math C1 review and verified 10x speed and resource targets remain open (#2381, #2383, #2385).
- Geometry adds analytic `Bisector_Bisec` results for point, line and circle sites, with 1,380 public OCCT records (#2382).
- The Forge field adds density tables for nine TPMS forms (#2380).
- Fasteners cuts the modelled thread into the remaining nut types and into flanged, countersunk, cheese, pan and button head screws (#2379, #2386).
- Part aligns Cut, Fillet and Fragments objects through the Attachment editor (#2358).
- Surface adds FreeCAD twin journeys for G1 Filling, Sections and Blend Curve (#2384).

## 0.5.3

This patch includes seven PRs merged since v0.5.2 across Geometry, Fasteners, Part and the shell.

- Geometry adds `gp_Vec`, `gp_Vec2d`, `gp_XY` and `gp_XYZ` arithmetic and linear form facade methods. The 1,578 checked public OCCT 8.0.1 records match bit for bit on arm64, x86_64 and WASI SIMD128. The broader gp and math C1 review and verified 10x speed and resource targets remain open (#2372, #2373, #2374, #2376).
- Fasteners cuts the modelled thread into hex head screws, cap screws and hex nuts (#2370).
- Part owns the palette items of its Boolean commands and checks EditAttachment against FreeCAD. It also adds a cone fixture and fixes an OCL name shadow (#2377).
- The shell adds the S-MENU, S-KEY, S-STATE and S-SIZE sweeps and lets F1 run nothing as in FreeCAD (#2371).

## 0.5.2

This patch includes eight PRs merged since v0.5.1 across Geometry, the Forge field, Fasteners, the shell and UI contracts.

- Geometry matches checked public OCCT 8.0.1 `gp_Pnt2d`, `gp_Pnt` and `gp_Vec` operations and `gp_Vec` angular predicates on arm64, x86_64 and WASI SIMD128. The broader gp and math C1 review and verified 10x speed and resource targets remain open (#2361, #2364, #2366, #2368).
- The Forge field adds TPMS fields with proved Lipschitz bounds (FW29, #2362).
- Fasteners makes carriage bolts as the FreeCAD workbench draws them (#2360).
- The shell keeps one million Report lines in a virtual list (#2367).
- UI contract checks reach toolbar group items and menu-only commands (#2357).

## 0.5.1

This patch includes eight PRs merged since v0.5.0 across Geometry, Surface, Part and Fasteners.

- Geometry matches public OCCT 8.0.1 calls for `gp_Dir2d` setters and operations, `gp_Vec2d` operations and errors, and `gp_Vec2d` angular predicates on arm64, x86_64 and WASI SIMD128. These are checked cases, not full OCCT compatibility. The broader parity and verified 10x targets remain open (#2348, #2351, #2354).
- Geometry evaluates B-spline surfaces from the Taylor form of each span piece (GW26c, #2350).
- Surface adds the per-edge Surface flow and checks it against FreeCAD 1.1.4 (J-SF-01, #2346).
- Part checks group memory, the current edge of a list row, Check Geometry row selection and the Part states on web and desktop (PT-T380 to PT-T388, #2347, #2353).
- Fasteners adds Countersunk Holes for countersunk screws (#2349).

## 0.5.0

This release ends the work items of milestone F-rep FP1: Fast core (#1609). It has five PRs merged since v0.4.13 across the Forge field, OCL, Geometry, Fasteners and web. The exit checks of FP1 stay open, because they compare Forge with Fidget on the reference machines, and the Fidget baselines need the probes of FW10 and FW11.

- In FP1, the Forge field has the batch, interval, Lipschitz and dual VMs, the tape simplifier and cache, the compiler passes, Boolean reification, the tile hierarchy, the smooth Booleans, offsets, patterns, profiles and lifts, the default mesher, the BCC mesher, the CPU viewport and the Rust builder `F`.
- OCL has fields. The operators of the field engine are functions in the namespace `field`, such as `field.sphere(5 mm)`. `a | b`, `a & b`, `a - b`, `a ^ b` and `-a` are the Booleans, and `a - smooth(1 mm) b` blends two fields. A `param` that is an argument of a field stays a parameter of the field (FW28, #2340).
- `ocl fmt --canonical` prints the canonical form of each field of an OCL file (FW28, #2343).
- Geometry: the angular predicates of `gp_Dir2d` now match 1,408 public OCCT calls at the boundaries of the tolerance (#2342).
- Fasteners adds Simplify, which copies each picked fastener as a plain body with the FreeCAD name of the copy, such as `bolt_Copy` (#2341).
- Web puts the Fit All status in the store flush, and K-MENU.04 expects the Assembly keys that FreeCAD drops (#2344).

## 0.4.13

This patch includes sixteen PRs merged since v0.4.12 across Geometry, the Forge field, Fasteners, Sketcher, Part, desktop and web.

- Geometry adds section of a shape by a plane (#2302). Plane distance and direction angular predicate calls now match the checked public OCCT boundary cases on three targets (#2321, #2326, #2338). The broader C1 and verified 10x targets remain open.
- The Forge field adds a CPU viewport with sphere tracing and an interval fallback, then adds the Rust builder F for field operators (#2327, #2334).
- Fasteners adds a document BOM, a Screw calculator and Change parameters for placed fasteners (#2323, #2330, #2337).
- Sketcher handles fillet and angle at coincident points and checks FreeCAD point and constraint order in real models (#2322, #2324).
- Part and desktop check picks and draggers in the desktop app, run the Surface scripts on macOS and check each Part dragger and element pick (#2331, #2332, #2335).
- Web updates the Optional group only after a user toggle and sends a dragger move in the store flush so a frame has one commit (#2329, #2333).

## 0.4.12

This patch includes eight PRs merged since v0.4.11 across Geometry, Part, Fasteners and the web UI.

- Geometry adds quasi uniform abscissa with pinned OCCT probe cases for GW20 (#2314). Shape distance uses a stack after certifying the minimum and a faster bound for horizontal circular edges. The 998-face plate still matches all 996 ordered OCCT solutions. The GW21 C2 differences and 1 ms large-body target remain open (#2318, #2319).
- Part names Undo and Redo steps as FreeCAD names its transactions, and the macOS tests finish the GUI twins of J-PT-01 and J-PT-03 (#2315, #2316).
- Fasteners chooses the size of a fastener on a round edge with size Auto as the FreeCAD workbench does (#2317).
- The web UI clears on-view values in the render when a sketch opens, and the surface task refuses a wrong Surface list on OK as FreeCAD does (#2311, #2312).

## 0.4.11

This patch includes ten PRs merged since v0.4.10 across Geometry, the Forge field mesher, Part, Assembly, Fasteners, the proof bundles, web and the UI sweep.

- Geometry finds the nearest pairs of a separated sphere and torus in less time (#2304).
- The Forge field adds the BCC marching tetrahedra mesher of 05 section 5 for differential tests (FW26). Its meshes and the dual contouring meshes agree with Manifold `LevelSet` in the tolerance on four cases (#2309).
- Part adds the FreeCAD enabled rules of the Part commands, and the kit selects the fixture objects first (PT-W13) (#2305).
- Assembly adds the Insert panel of Insert Component (ASM-W08) (#2299).
- Fasteners puts a fastener on a round edge, moves it to another round edge and flips it over, as Move of the Fasteners workbench does (#2303, #2308).
- The proof bundles write `report.html`, check `verdict.toml` and list the review queue (#2306).
- Web opens the palette and the first task panel in fewer ms under load (I14), and J-UI-06 runs each key with real input, with Ctrl+Tab that shows the next tab once (INP-W04) (#2300, #2307).
- The UI sweep sweeps each control of the OpenCAD profile (#2301).

## 0.4.10

This patch includes nine PRs merged since v0.4.9 across Geometry, the Forge field mesher, Fasteners and the UI sweep.

- Geometry resolves a full-circle distance continuum before subdivision and checks hyperbola-plane facade extrema with an analytic height derivative. The pinned pair and shape distance fixtures pass, while GW21 C2 parity and the 1 ms large-body target remain open (#2290, #2296).
- Geometry adds `GCPnts_UniformDeflection` and a faster discretization evaluator for GW20. The remaining OCCT parity and 10x checks stay open (#2295).
- The Forge field mesher adds manifold dual contouring on the tiles as its default path for FW25 (#2289).
- Fasteners adds PEM self clinching nuts, standoffs and studs (#2292, #2293, #2294).
- The UI sweep sees and closes a message box in the end-to-end test, and the notification button reads its place only while its box shows (#2291, #2298).

## 0.4.9

This patch includes nineteen PRs merged since v0.4.8 across Geometry, B-rep, Sketcher, Part, Surface, Assembly, Fasteners, the shell, web and tests.

- Geometry evaluates a B-spline surface a row of the span at a time (GW26b). The distance work of GW21 uses radial bounds to classify complete circles and extremal box corners for plane distance bounds, and pins the 998-face shape distance fixture (#2267, #2279, #2284, #2276).
- B-rep finds the minimum distance between two shapes, checked against OCCT (GW21) (#2248).
- Sketcher shows the B-spline helper submenu (#2272).
- Part passes the negative tests PT-T805, PT-T815 and PT-T824 (PT-W15) (#2281).
- Surface lets `part.fill` take a support face, as the Support Surface of the FreeCAD Filling does (SRF-B02) (#2277).
- Assembly adds the edit mode rules, the Joints group and Toggle Grounded on the selection (ASM-W07) (#2273).
- Fasteners adds the swivel T-slot nuts, the EJOT PT screws for plastics, the ISO 2936 hex keys, the ground screws and the IUT heat set inserts (#2271, #2278, #2285, #2286, #2287).
- The shell adds Save Views and Load Views of Freeze Display, the Task List dock, Dock Views, and journey J-SHL-08 for the Help menu, the Help page and About. OK on the Help page now writes the disabled GitHub button, as FreeCAD does (#2282, #2283).
- Web tells in the status bar why a menu item, a toolbar button or a field of a task panel is disabled. The UI sweep has a planted dead control that it must catch (#2274, #2275, #2280).

## 0.4.8

This patch includes twelve PRs merged since v0.4.7 across Geometry, Sketcher, Part, Fasteners, the shell, web and tests.

- FW24 adds the profile fields FO-P2-09 to FO-P2-11 (cubic Bezier, NURBS curve and closed profile) and the lifts FO-LF-01 to FO-LF-03 and FO-LF-06 (extrude, extrude range, revolve and elongate) to the field DAG. LBVH writes 4-wide nodes directly from two Morton split levels with the same leaf order, and the benchmark pairs it with the public OCCT `BVH_LinearBuilder` on one million boxes. Verified 10x targets remain open (#2263, #2266).
- Sketcher adds the General preferences page with the three FreeCAD on-view parameter modes, the advanced solver panel, redundant constraint removal, Escape behavior and the unified coincident tool. The pump drag test waits for sketch entry before it watches the camera (#2260, #2264).
- Part adds its 185 UI ledger rows to the contract runner, with a FreeCAD 1.1.4 probe as the oracle, and removes an inactive test placeholder (#2268, #2261).
- Fasteners adds the GN 505 quarter-turn T-slot nut and the GN 505.4 bolt (#2259).
- The shell adds journey J-SHL-04 with the Notification Area page, the Help page of Preferences with Help Website and About Qt, and fixes for journeys J-SHL-03 and J-UI-06. A task panel with no field takes the focus, so Esc closes it, and the overlay toggles tell why they do nothing (#2257, #2258, #2265, #2269, #2262).

## 0.4.7

This patch includes sixteen PRs merged since v0.4.6 across Geometry, Part Design, Surface, Sketcher, Fasteners, Assembly, the shell, web, desktop and build maintenance.

- GW8 pins 35 more B-spline curve points with infinite outer knots against public FreeCAD and OCCT calls. The BVH benchmark now pairs one million boxes against OCCT; a run on a busy host reduced Forge peak heap from 264 MB to 200 MB and retained heap from 132 MB to 68 MB. `gp_Pln` equation construction matches twelve public OCCT cases, including a FreeCAD model face. Broader gp and math C1 coverage and verified 10x targets remain open (#2239, #2245, #2252).
- A kernel edit joins a tool that splits a face alone and joins the other bosses and pockets in batches, while checking the same face and edge names. The B-REGEN-300-TAIL target remains open (#2238).
- Part Design adds To first and To last extents for Pad and Pocket and axes for Revolution. Part also gains negative tests PT-T801 to PT-T825 with the FreeCAD texts (#2244, #2247).
- Surface Filling picks inner edges and points from the list panel. Sketcher keeps the active keyboard field and locks visible values with Ctrl+Enter (#2243, #2240, #2251).
- Fasteners adds T-slot nuts. Assembly follows the FreeCAD chord rule for disabled commands. The shell adds the J-SHL-02 menu journey and the group command keys of INP-W06 (#2246, #2150, #2249, #2253).
- The web test times a DrivAerML orbit drag on the main thread. The desktop lockfile was synced to v0.4.6 before this version bump (#2241, #2242).
- The workspace format gate passes after line layout fixes in the Part Design extent code and an OCCT bracket test (#2254).

## 0.4.6

This patch includes eleven PRs merged since v0.4.5 across Geometry, Surface, Fasteners, Sketcher and the shell.

- GW8 checks finite limiting points and derivatives through order five on 180 plain and rational infinite-end B-spline curve cases. Public FreeCAD 1.1.4 and OCCT 8.0.1 probes also pin twenty B-spline surface points across both parameter directions. The surface results record the OCCT D0 and D1 point difference, and the focused suites run in per-commit wasm CI. Broader nonfinite behavior and verified 10x targets remain open (#2229, #2235).
- GW23h adds Bernstein root isolation for the `circle_tan_cen` and `circle_2tan_rad` solvers on Bezier and B-spline curves of degree six or less and their offsets (#2048).
- Surface adds eight negative tests in SRF-T804 to SRF-T814 and shows the Boundaries list in the Filling panel (#2225, #2234).
- Fasteners adds DIN 7996 pan head wood screws and ISO 7049 pan head tapping screws with three point forms, checked against FreeCAD shapes (#2228, #2231).
- Sketcher remembers the chosen group tool. The Validate Sketch command manifest and the App test now match the orientation check and advanced solver controls (#2226, #2230, #2232).
- The shell adds K-STAT and K-PANEL contracts and runs the shell ledger rows (#2227).

## 0.4.5

This patch includes six PRs merged since v0.4.4 across Geometry, Fasteners, Sketcher and the shell.

- GW8 curves with a `-Inf` first knot use the finite limiting point and tangent checked against 15 FreeCAD 1.1.4 calls. Raw curve and surface constructors reject a final NaN knot. Public OCCT 8.0.1 `D0` and FreeCAD `value` match bit for bit on 14 more calls, and both infinite domain ends now give a constructor error because two finite paths have different limits. Broader nonfinite behavior and the verified 10x targets remain open (#2218, #2221, #2223).
- Fasteners adds the four GOST 1144 half round head wood screws with a quadratic B-spline head and checks their volumes against FreeCAD (#2219).
- Sketcher keeps the 16 tangent orientation locks of the imported mini vacuum pump slots and displays saved projected edges when the web host cannot rebuild a native FreeCAD body (#2220).
- The shell adds journey J-SHL-03 for overlay panels and fixes menu focus and submenu pointer updates found by that journey (#2222).

## 0.4.4

This patch includes the Geometry, Surface, Part, Sketcher, Fasteners, renderer and shell changes merged since v0.4.3.

- GW8 `KnotForm` matches 7,533 five-knot public OCCT form and analysis calls and a 44,115-call six-knot form holdout. A 17-call ULP boundary, 177-call crossing grid and 339,735-call seeded crossing holdout now match. High-level B-spline curves with a `+Inf` last knot match 15 public OCCT 8.0.1 point and tangent calls on arm64, x86_64 and wasm32-wasip1 SIMD128. Broader nonfinite behavior and verified 10x targets remain open (#2204, #2207, #2214).
- Surface Filling adds edge and vertex constraints and gives each boundary curve its own flag. The Sections panel shows the list for its FreeCAD work item (#2200, #2201, #2208).
- Part makes toolbar primitives with the FreeCAD defaults and gives them a size dragger (#2205).
- Fasteners adds knurled thumb screws, pan head screws with a collar, wood screws, countersunk wood screws and round head screws (#2202, #2209, #2210, #2211, #2216).
- Sketcher applies advanced iteration and convergence controls (#2203).
- The renderer leaves out the back faces of solids (#2206).
- The shell adds the same-window restart journey, exposes the first task-panel command to the test channel and lets clicks under a tooltip reach the control (#2212, #2213, #2215).

## 0.4.3

This patch includes the Geometry, Surface, Sketcher, Part, Fasteners, web and desktop changes merged since v0.4.2.

- GW8 `KnotForm` and `KnotAnalysis` match 189 three-knot and 1,215 four-knot public OCCT nonfinite calls. The four-knot grid has 795 `Uniform` and 420 `NonUniform` forms. Longer ranges, safe curve construction and the verified 10x speed target remain open (#2183, #2192, #2195).
- GW14 point projection uses Bezier piece evaluation, conservative cone tests and reused patch subdivision storage. A busy Apple M4 diagnostic measured 6.36x, 8.38x and 8.16x all core speed against OCCT for the new surface, reused surface and curve rows, so the verified 10x target remains open (#2059).
- WebAssembly property queries use one worker. All 59 query library tests and eight property integration tests pass on wasm32-wasip1 SIMD128, while native thread counts stay unchanged (#2190).
- Surface fills across boundary edges and adds the Blend Curve panel fields, Sections menu and toolbar entry. The toolbar test now checks the live Sections form (#2181, #2182, #2197).
- Surface adds a shared pick list with reorder and removal controls. Fillet and Chamfer use it for their References field (#2193).
- Part shows picks, a ghost and draggers in the desktop app. Fasteners adds PCB spacers, bolts, square head bolts and headless screws (#2184, #2185, #2188, #2194).
- Sketcher saves FreeCAD grid preferences and matches FreeCAD grid line appearance (#2187, #2191).
- The web app finds `opencadd serve` without a kernel query parameter, and the desktop lockfile matches its package version (#2186, #2189).

## 0.4.2

This patch adds short knot classification parity, FreeCAD surface and Fasteners features, document work and shell changes merged since v0.4.1.

- GW8 `KnotForm` and clamped `KnotAnalysis` skip extra scans. Thirteen public OCCT probes match 1,308,475 calls, and nine focused tests pass on arm64, x86_64 and wasm32-wasip1 SIMD128. The 10x speed target remains open (#2170).
- Public OCCT and FreeCAD probes record nonfinite knot classification, curve construction and evaluation. Forge now matches 42 one and two knot `KnotForm` calls and 36 two-knot `KnotAnalysis` calls, including nonfinite, repeated and descending values. Longer nonfinite behavior remains open in #2172 (#2173, #2179).
- The documents topic fixes `.ocad` as the save format, records FreeCAD M0 answers and adds ten C0 gap rows to the UI ledger (#2168, #2177).
- The Fasteners workbench adds seven nail forms and nine threaded rod, tap and die forms from the FreeCAD tables (#2169, #2176).
- Sketcher drawing tools snap to the middle of a line or arc and keep the point-on-object constraint (#2171).
- Surface adds the Blend Curve edit panel and FreeCAD continuity limits, plus the Sections feature and command (#2164, #2174).
- A document keeps features in a hash map keyed by id for faster large edits, while ordered walks still use the stored ids (#2175).
- Preferences adds the Display UI page and applies supported tree and overlay settings after Apply (#2178).

## 0.4.1

This patch expands OCCT `KnotForm` checks and includes Part, Fasteners and large assembly changes merged since v0.4.0.

- `KnotForm` matches 448,388 finite overflow calls across seven public OCCT probes and 855,407 finite holdout calls across five schedules. Forty-eight overflow boundary cases and 52 wider finite cases pass on arm64, x86_64 and wasm32-wasip1 SIMD128. The quarter-gap probe excludes 771 nonfinite endpoint calls (#2158, #2160, #2163).
- A box selection and a one-feature edit on 20,000 parts read only the changed selection or features. The worker sends meshes and bodies only when they change (#2157).
- J-PT-01 checks `Schenkel.stp` against its FreeCAD 1.1.4 oracle. The 625 Part interaction cells are bound to test stubs that report `not built` until their tests are added (#2159, #2162).
- The Fasteners workbench adds DIN 6799 E-clips with the 58 edges that FreeCAD 1.1.4 has for each clip (#2161).
- The Surface toolbar, menu and context menu enable six commands from the FreeCAD 1.1.4 active-state rules and show why a command is disabled. Blend Curve and Extend Face now require the right selection (#2165).
- Journey J-SHL-05 checks the Part Design Tasks panel with a FreeCAD 1.1.4 twin. The second-dialog question uses the Part Design text, and Pad task fields are indexed once so the panel stays within its frame budget (#2166).

## 0.4.0

This release closes milestone PD-M0 Truth of the Part Design topic (#1447), and has the first change of PD-M1. Each Part Design P0 row has an oracle record from FreeCAD 1.1.4, the 10 Part Design reference files are pinned in the corpus, the UI ledger has a row for each of the 50 Part Design gaps, and the spec answers each M0 open question.

- The web app has the Select attachment panel of FreeCAD. New Sketch with nothing selected lists the base planes and the datum planes. Pad, Pocket and the other profile features take the one free sketch, or ask for the sketch in the panel with the FreeCAD status of each sketch. With no sketch, the "No sketch to work on" box shows. This is PD-W04 (#2153).
- In tamnd/opencad-compat, `fc-drive` records the oracles of the Part Design P0 rows, the `oc-ref-pd-*` fixtures are pinned, and `parity/freecad.toml` has rows for Duplicate Object and the two Part Design primitive groups.
- The macOS menu bar can be read and pressed through the new Swift tool `oc-ax`, so K-MENU.02 runs on macOS (#2155).

## 0.3.1

This patch improves `BSplCLib::KnotForm` agreement with OCCT on finite knot boundaries, records Part Design command gaps and restores the local T0 gate.

- `BSplCLib::KnotForm` now matches all 4,680 scale probe calls and 616,000 calls across four independently seeded finite holdouts. Fifty-three new boundary records pass on arm64, x86_64 and wasm32-wasip1 SIMD128. The 40,178-call overflow boundary probe still differs on 338 calls (#2148, #2151).
- The Part Design primitive groups and Duplicate Object command are tracked as gaps. The Part Design spec answers the M0 questions, and the UI ledger records 47 Part Design gap rows (#2145).
- The tree scope cache passes UI lint, and the Sketcher drawing test expects the radius and diameter fields that the runtime returns. Local T0 passes in 45 seconds (#2149).

## 0.3.0

This release closes milestone M0 Truth of the Surface topic (#1457). Each of the 5 Surface journeys now runs in the FreeCAD 1.1.4 GUI on Linux and macOS and agrees with its golden, and the Surface rows have FreeCAD oracles. The release also adds the Preferences dialog frame, the toolbar rows of FreeCAD, Circle on-view dimensions in Sketcher and faster tree updates on large assemblies.

- J-SF-05 runs on macOS. Each click in the 3D view divides the viewport point by the device pixel ratio, and `tests/fc-drive/run.py` reads `golden.<platform>.json` before `golden.json`. The macOS golden is necessary because the view has another size, so the picked lengths differ by 1 to 2%. In tamnd/opencad-compat, `fc-drive` waits for a menu command to become active, as FreeCAD sets the state from a timer after a selection change (#2144).
- The Part oracles of J-PT-03 and J-PT-10 are in `tests/oracles`, and `cargo xtask oracle ID --gui` checks each oracle in the FreeCAD GUI (#2139).
- The shell has the Preferences dialog of FreeCAD with its group tree, page stack, Reset menu and search. Other topics add their pages with the page API in `@opencad/ui/settings` (#2140).
- The toolbars are in rows as in FreeCAD, with the bar context menu, Lock Toolbars, a drag between rows and the toolbar area of the status bar (#2143).
- Sketcher shows on-view X and Y fields for the center of a circle, and Diameter or Radius for the rim. A locked value makes a constraint that you can edit (#2141).
- When a change touches only the marks of rows, the mirror, the tree model and the task watchers keep their tables. A mark change on the 20,000-part vehicle does not rebuild the feature list (#2142).
- `BSplCLib::KnotForm` accepts half a gap spacing at a boundary as OCCT does. A 20-row fixture pins the 8 corrected cases (#2146).

## 0.2.5

This patch adds Fasteners pins, FreeCAD task watchers, Shell overlays and Sketcher rectangle input. It reduces work during Sketcher drags and drawing view builds and expands Geometry and Surface checks.

- The Fasteners workbench adds internally threaded pins DIN 7979 and ISO 8736, grooved pins ISO 8739 to 8741 and ISO 8744 to 8747, split pins ISO 1234 and taper pins ISO 8737 (#2117, #2118, #2119, #2127, #2130).
- Sketcher reuses the connected constraint block across pointer frames during a point drag. The 500 entity sketch completed 360 frames without an unsolved frame or cursor gap (#2120).
- Sketcher accepts corner coordinates, width and height while drawing a rectangle. Locked values create editable dimensions, and the preview updates as the values change (#2129).
- The Tasks panel shows the Part Design and Assembly task watchers of FreeCAD 1.1.4 when no dialog is open (#2121).
- `cargo xtask contract` runs FreeCAD component checks and reports each result. The first 23 Surface rows have fixtures and a FreeCAD oracle for web runs (#2131, #2132).
- The Surface checks have FreeCAD 1.1.4 B-spline and G2 Filling oracles. All 221 interaction cells are bound to reportable test stubs; each remains open until its test is built (#2135, #2136).
- The Shell runs FreeCAD overlay dock commands and panel controls. A P0 sweep checks 113 menu items, 15 toolbar items and F1 for unknown actions and missing disabled reasons (#2123, #2134).
- Coarse drawing views on one page share a compound copy and the scaled meshes for each part, so the page does not copy and mesh the same parts for each view (#2125).
- A public OCCT KnotForm probe found that all 60,858 checked full ranges agree with their adjacent three-knot ranges. The remaining Forge classification gaps stay open (#2116).
- The FreeCAD J-SF-02 twin and GUI steps check G1 Filling across a four-edge body hole, including support rows and undo and redo behavior (#2133).
- All 90 `ocad-forge-math` test files are registered in per-commit wasm32-wasip1 SIMD128 CI. The added basis, derivative, Schoenberg and other math suites pass locally on arm64, x86_64 and wasm32-wasip1 SIMD128 (#2122, #2124, #2128).
- BVH builds use one thread on WebAssembly when more are requested, so WASI no longer aborts on thread creation. The 13 active BVH tests pass locally on three targets (#2126).
- The T0 gate passes after refreshing the Sketcher test, planted patches and geometry dashboard, fixing locked label contrast and contract runner formatting, and using deterministic math in the Ondsel comparison test (#2137).

## 0.2.4

This patch adds FreeCAD fraction controls to Extend Face, improves cubic B-spline point evaluation and quaternion parity, speeds large assembly selection, fixes Sketcher input focus and adds more Fasteners hardware.

- `part.extend_face` accepts the U and V fractions, symmetry, sample counts and tolerance of FreeCAD 1.1.4 `Surface::Extend`. The existing `by` length keeps its previous path (#2101).
- The Fasteners workbench adds spherical washers, conical seats, parallel pins, taper pins, clevis pins and slotted spring pins (#2102, #2103, #2104, #2105).
- The Part spec records the legacy FCStd file and the missing broken BREP file for PT-W06 (#2106).
- Box selection on the 20,000-part vehicle reuses one owner map and renders only its overlay on release. The checked A-SEL-BOX-10K runs measured 47.2 to 50.1 ms against a 50 ms budget (#2107).
- Cached cubic Bezier D0 evaluates from coordinate packs, and the direct planar and spatial cubic path uses fixed coordinate work. Public OCCT point bits match on arm64, x86_64 and wasm32-wasip1 SIMD128; paired timing rows keep equal checksums (#2108, #2111).
- Quaternion vector rotation returns NaN coordinates for a zero quaternion as OCCT does. A 265-call public probe has one Forge output digest on arm64, x86_64 and wasm32-wasip1 SIMD128 (#2109).
- Sketcher returns focus from an idle Point or Line on-view field to the sketch after two seconds. Escape first leaves the active field and keeps the drawing tool running; a second Escape stops it (#2110, #2114).
- Searched cubic 3D D0 uses 12 plain or 16 rational local work values. The 2,884 pinned OCCT point records stay bit equal on three targets, and paired checksums match (#2112).
- Geometry notes now state that 1748 is the NIST FTC-10 curve identifier. That curve has 10 distinct knots, 24 flat knots and 20 poles (#2113).

## 0.2.3

This patch adds the offsets, the shells and the patterns to Forge fields, the analysis data of surfaces for the renderer, the FreeCAD notification area, one settings store for the shell and more Fasteners nuts.

- Forge fields add the offsets and shells FO-OF-01 to FO-OF-08: `offset`, `shell`, `shell_inward`, `shell_outward`, `shell_variable`, `hollow`, `thicken_surface` and `onion`. A distance or a thickness can be a constant, a parameter or a field (#2084).
- Forge fields add the patterns FO-PA-01 to FO-PA-04, FO-PA-06 and FO-PA-07: `repeat`, `repeat_limited`, `repeat_mirrored`, `repeat_polar`, `pattern_on_points` and `pattern_on_curve`. A repeat reads one copy of the instance for each neighbour cell, and the new `pattern` node keeps the instance for the type checker (#2098).
- `ocad-forge-geom` adds `surface_props_batch`, `surface_props_grid` and `draft_angle`, the curvature, zebra and draft data of spec 05 section 8.3 for the renderer (#2085).
- The notification area of the status bar works as in FreeCAD 1.1.4, with the count of the unread items, the list and the Critical message box (#2096).
- The shell keeps all its settings in one store with the FreeCAD paths, names, types and defaults, and reads and writes `user.cfg` text. The old keys move into the store one time (#2070).
- An edit on the vehicle of 20,000 parts compares the features and the tree rows in place, so A-PROP-EDIT of J-PERF-05 is in its budget (#2095).
- The Properties panel shows a new selection in the frame after the one that shows it in the view, as FreeCAD does with its timer (#2090).
- `tests/fc-drive` runs J-SF-04 Open a Surface file in the FreeCAD GUI (#2092).
- The five heap gate tests that count allocations run without the test harness (#2097).
- The Part spec records the gap rows and the toolbar fix, and the ledger score leaves out TopoShapeListTest and 4 unused forms (#2088, #2089).
- The Fasteners workbench adds the pal nut DIN 7967, the wing nuts DIN 315 and ASME B18.6.9 form A, and the tee nut 4PWTI (#2091, #2093, #2094).

## 0.2.2

This patch adds the focus and Escape rules of the INP topic with real input, the first FreeCAD oracles of the Part journeys, a faster element pick layer, cached cubic Bezier points and more Fasteners nuts.

- Each FreeCAD key that a browser or the app keeps for itself now has a second key in the `freecad` profile, such as Alt+Shift+N for Std_New. `kept.ts` gives the kept keys of Chrome, Edge, Firefox, Safari and the apps, and a test checks each profile on each host (#2079).
- `focus.spec.ts` checks with real input that a key runs in the 3D view and the tree, types text in a field and goes to an open menu or dialog (#2082).
- `escape.spec.ts` checks that Escape closes one part at a time and that the focus goes back to the opener. A control such as a menu bar button now runs a key that it does not use itself, as in FreeCAD (#2086).
- `cargo xtask oracle` runs the FreeCAD twin of each folder in `tests/oracles` and compares it with its golden. J-PT-04 to J-PT-09 have their oracles (#2083).
- The element pick layer renders only the elements that a change of the selection or the preselection touches (#2080).
- `BezierD0CubicView` prepares the poles of a cubic Bezier curve one time for repeated D0 calls (#2051).
- The Sketcher guide shows the grid and the snap controls on the real pump sketch (#2078).
- The preferences spec says that the oracle reads the version migrator (#2077).
- The Fasteners workbench adds the weld nuts DIN 928, DIN 929 and ISO 21670, and the nut with a ball seat DIN 6330 (#2076, #2081).

## 0.2.1

This patch adds the tile hierarchy and the smooth Booleans to Forge fields, the tangential deflection to curve discretization, a faster box selection on large assemblies and more Fasteners nuts.

- Forge fields add the tile hierarchy. `sign_image`, `sign_volume` and `height_map` run T2 on tiles and cubes, prune the tape of each tile that has the surface and run T1 only at the last level. Rayon runs the top tiles, and each count of threads gives the same output. FB1 to FB4 are measured (#2065).
- Forge fields add the smooth Booleans. `smooth_union`, `smooth_intersect`, `smooth_subtract` and `smooth_union_n` take one of the eight kernels FO-SB-01 to FO-SB-08, `smooth_mix` gives the mix factor, and `lipschitz::smooth_choice` gives the pruning margin of the smooth min (#2073).
- The `disc` module adds `GCPnts_TangentialDeflection`, against an OCCT probe of 268 3D and 222 2D cases (#2063).
- J-PERF-05 times a hover, a click, an edit and a box selection on the vehicle of 20,000 parts. A box selection and a ray pick now find the bounds of each mesh one time, and the Properties panel finds the shared properties in one pass (#2068).
- The element pick layer renders in the store flush, so a frame has one React commit (invariant I16) (#2071).
- J-UI-06 checks the FreeCAD wait of 300 ms for a short key and presses real keys in sketch edit. `keys.spec.ts` checks the chord rules with real input (#2069).
- `tests/fc-drive` runs J-SF-05 Curve on Mesh in the FreeCAD GUI (#2067).
- The Projection Group test measures the lines of each view, not the hover area (#2072).
- The Fasteners workbench adds cap nuts, all metal lock nuts and lock nuts with a flange (#2062, #2064, #2066).

## 0.2.0

This release closes milestone M0 Truth of the assembly topic (#1462). Each of the 91 assembly rows and gaps now has a FreeCAD 1.1.4 oracle, and the open questions of the topic have answers. The release also adds curve projection, faster tangent cases of 2D curve intersection, the first Surface journeys in the FreeCAD GUI and more Fasteners nuts.

- The assembly oracle in tamnd/opencad-compat maps each of the 91 rows to a value in the GUI dump, a probe result or a golden JSON file, or to a reason why no probe opens it. The goldens of J-ASM-09 to J-ASM-12 cover the BOM, the assembly link, the simulation and the ASMT export (#1462).
- The assembly stand-ins `oc-ref-assembly-arm.FCStd` and `oc-ref-assembly-joints.FCStd` are in tamnd/opencad-corpus. The corpus also makes the 1k and the 20k synthetic vehicles as nested assemblies (#1462).
- The Fasteners workbench adds castle nuts and slotted nuts (#2056).
- ProjLib and GeomProjLib project a curve on a surface or a plane, at level C2 (#2053).
- The 2D curve intersection solves the tangent and the near tangent cases without tiny leaves (#2058).
- `tests/fc-drive` runs the GUI steps of J-SF-01 and J-SF-03 in FreeCAD 1.1.4 with `opencad-compat fc-drive`, and compares the result with the golden of each journey (#2061).

## 0.1.25

This patch adds compiler passes, axis hoisting and sign queries to Forge fields, plane sections of free-form surfaces, coarse drawing views and more Fasteners nuts.

- Forge fields add compiler passes and axis hoisting. `FieldGraph::optimize` merges equal nodes, folds constants and applies exact identities, and `BatchVm::eval_grid` runs each axis segment once for each chunk, plane or row. Hoisting makes the bracket of FB5 1.30 times as fast, and each output keeps the bits of T0 (#2040).
- Forge fields add the Boolean reification for sign queries. `Tape::sign_tape` and `SignVm` skip the parts of a field that do not change the sign, and each sign is the sign of T0. On a slice of the bracket the sign image takes less than one fifth of the time of T1, and FB1 is measured (#2055).
- `ss::plane_section` now gives the plane section of B-spline, Bezier, trimmed and offset surfaces, and of swept surfaces without a closed form, with a certified walk (#2049).
- TechDraw views take a `coarse` option, which finds the lines from a mesh of the body. The 500-part page of B-DRAWING-VIEWS meshes each distinct part one time (#2054).
- Knot-free planar and spatial `BSplCLib::D0` calls now run in Forge against a pinned OCCT 8.0.1 probe, with paired timing rows. Scalar and planar D0 spans are cached, and the cubic case has its own path (#2042, #2043, #2046, #2047).
- The Sketcher adds dimension modes for lines (#2045).
- The Fasteners workbench adds hex nuts with a flange, square nuts and nuts with a nylon insert (#2041, #2044, #2052).
- The preferences topic spec closes its M0 questions with the FreeCAD 1.1.4 dump (#2050).

## 0.1.24

This patch adds scalar and planar B-spline point evaluation, widens periodic derivative checks and extends FreeCAD parity records and Fasteners parts.

- Scalar and planar `BSplCLib::D0` calls now run in Forge. Each overload matches 2,884 public OCCT 8.0.1 results bit for bit on arm64, x86_64 and wasm32-wasip1 SIMD128 (#2038).
- Exact periodic D1, D2, D3 and DN checks now cover 72 high-degree calls, including period offsets and knot boundaries. The public OCCT differences remain recorded in the behavior note (#2033, #2035).
- The Fasteners workbench adds raised countersunk screws and flanged hex head bolts. The new shape tests cover 15 screws and 14 bolts against FreeCAD 1.1.4 (#2031, #2036).
- The assembly spec answers the ASM-W01 open questions using the pinned oracle and later work item reports (#2032).
- The UI parity ledger adds nine Preferences gap rows and ten Surface gap rows at C0 (#2034, #2037).

## 0.1.23

This patch fixes explicit periodic B-spline evaluation, adds exact coordinate entry in Sketcher and extends the Fasteners workbench.

- Explicit periodic `BSplCLib::D0`, D1, D2, D3 and DN spans now use the supplied parameter; searched calls keep period wrapping. Public OCCT probes cover 900 added D0 calls and 288 derivative calls. Twelve high-degree derivative calls also have exact rational checks that record eight OCCT point differences beyond the model box agreement rule (#2018, #2022, #2029).
- The Sketcher Point and Line tools take X and Y values beside the pointer and keep entered values as editable dimensions. The focused tests use the FreeCAD mini vacuum pump sketch (#2016, #2026).
- The Fasteners workbench adds pan and button head screws with checked FreeCAD 1.1.4 shapes (#2019, #2025).
- Face integration marks an element converged when its error estimate is within the element tolerance. The checked gap faces of `hammer.brep` now pass without changing the tolerance or integral values (#2023).
- The assembly ledger adds 24 missing gap rows and records the current levels of the assembly objects and task panels (#2021).

## 0.1.22

This patch adds T3 and tape pruning to Forge fields, more Fasteners parts and a wider OCCT knot check.

- Forge fields add T3, the dual VM, with the gradient, parameter derivatives and the Hessian. On the bracket of FB5 a gradient costs about 1.2 times a value (#2007).
- Forge fields prune a tape by the choices of T2 on a box, and keep the pruned tapes in a cache. On the Prospero shape a tile prunes 5,208 instructions to about 10, and every pruned tape gives the bits of the full tape (#2015).
- The Fasteners workbench adds the cheese head screws of ISO 1207, DIN 84, ISO 1580, ISO 7048 and ISO 14580 (#2012).
- Bulk knot insertion keeps up to eight requests in a fixed local array, and the `BSplCLib::D0` oracle adds 800 calls (#2010, #2013).
- An example lists what the FCStd reader does with each object of a file (#2006).

## 0.1.21

This patch adds more FreeCAD shell controls and Fasteners parts, and lowers Forge bulk knot insertion heap use.

- Forge bulk knot insertion now has paired OCCT time and heap rows. In-place updates cut the measured Forge allocations from eleven to five per owned cubic call and exact peak heap from 824 to 608 bytes, with the same checked output. Idle baselines and the 10x targets remain open (#1994, #1997, #1998, #2003).
- Large sketch validation uses a faster path and adds grouped actions. The joint solver is compared with OndselSolver for each joint kind (#1996, #2000, #2002).
- The FreeCAD shell shows menu access keys, a checkable message box and a shared input dialog. The Blend Curve limit follows FreeCAD's continuity sum rule (#1993, #2001, #1999).
- The Fasteners workbench adds countersunk screws and cross recess countersunk screws (#1995, #2004).
- The batch VM adds T2L, the Lipschitz VM and the far field rule with FPT7 (#1991).

## 0.1.20

This patch adds vector goldens of 200 drawings, fixes drawing views that build at the same time, and adds more Fasteners parts and OCCT checks.

- Drawing views that build at the same time on threads now get the same hidden lines as a view that builds alone. Before, two HLR updates at the same time could give wrong lines or no lines (#1967).
- A boolean whose result fails the samples of the fast check now goes to BRepCheck, so section views of parts such as the cylinder head of the V6 engine build (#1971).
- The test suite has vector goldens of 200 drawings of the corpus, and 36 views of the NIST parts match FreeCAD 1.1.4 (#1989).
- The Fasteners workbench adds hexalobular socket head cap screws and set screws (#1969, #1973).
- Forge matches more checked OCCT 8.0.1 periodic follow-up insertions, and the KnotForm probe shows every difference in both directions (#1965, #1966, #1970, #1976, #1988).
- Forge adds the Geom2dGcc solvers on general curves and T2, the interval VM (#1964, #1936).
- Large sketches stall less when they draw and when they delete (#1980).
- The desktop app opens when the engine corpus is not there, and journey J-SHL-01 covers the first hour of a FreeCAD user (#1963, #1968).

## 0.1.19

This patch extends OCCT geometry checks, protects imported sketches and adds more Fasteners parts.

- Forge matches checked OCCT 8.0.1 reversal and later insertion on plain periodic curves whose pole counts differ from their knot multiplicities. Rational probes expose zero-weight and NaN-pole outputs that Forge cannot represent with its finite curve data. Other periodic request groups and the verified 10x targets remain open (#1958, #1959, #1961).
- The intersection crate adds plane sections of quadrics, tori and swept surfaces with public OCCT probes (#1952).
- Imported FreeCAD sketches with invalid references no longer enter the editing path, and the Fasteners workbench adds socket head cap screws (#1957, #1960).

## 0.1.18

This patch adds more OCCT geometry coverage and FreeCAD workbench parts.

- Forge matches checked OCCT 8.0.1 periodic bulk insertion results with pole counts that differ from their knot multiplicities. Public high level probes cover seam pairs, a first-span triple, a mixed `Add` false pair and quadratic crossing pairs. Other repeated requests, later operations on these curves and the measured 10x targets remain open (#1954, #1955).
- Curve discretization adds arc length, points at equal arc lengths and deflection based points to `ocad-forge-query` (#1944).
- The 2D intersection crate builds planar arrangements and closed sketch regions from curves (#1950).
- The Fasteners workbench adds hex nuts and eleven washers. The desktop app opens its preloaded model on startup (#1951, #1953, #1949).

## 0.1.17

This patch pins more OCCT geometry behavior, adds pair extrema queries and updates the FreeCAD desktop shell.

- Forge matches checked OCCT 8.0.1 early middle periodic triples, repeated `Add` false unit requests and rational pair insertions on degrees 2 to 5. The pole-count anomalies and measured 10x targets remain open (#1939, #1942, #1947).
- Pair extrema queries cover two curves, a curve and a surface, and two surfaces, with public OCCT checks. Some OCCT parity cases remain open (#1929).
- Face mass integrals reuse B-spline basis values and adaptive panels on high degree faces. The measured `hammer.brep` CPU ratio is 4.0x on a busy shared machine, below the 10x target (#1935).
- The route checker records parity and performance evidence for nine F0 operations. No route has flipped to Forge (#1920).
- The FreeCAD shell adds the Start page, Report view controls, Help links, task dialog handling, document titles and test-channel file opening. The Fasteners workbench adds hex head screws, and the local T0 UI lint gate passes again (#1937, #1938, #1940, #1941, #1943, #1946, #1945).

## 0.1.16

This patch makes large assemblies faster to show and to orbit, adds the Sketcher toolset, and brings the desktop shell closer to FreeCAD.

- A large assembly draws its far parts with coarser meshes while the camera moves, so an orbit of the 20k vehicle keeps the display rate. An opened assembly starts its kernel build sooner, and its 20,000 parts show about 3.2 s after the drop, down from 4.8 to 5.8 s (#1927, #1933).
- The Sketcher has its toolset, five more Sketcher commands run, a real pump test measures point dragging against FreeCAD, and malformed constraints from the pinned FreeCAD corpus are tested (#1922, #1923, #1928).
- The desktop app keeps the window size, position, maximized state and hidden toolbars between runs, and builds the macOS menu bar from the menus of the page. The status bar is in the FreeCAD order, with status tips and a notification area (#1925, #1932, #1930).
- Forge pins periodic crossing knot pairs on degrees 3 to 5 and supports checked periodic triple knot insertions (#1924, #1931).
- The Gears workbench adds the T-shaped timing gear (#1926).

## 0.1.15

This patch checks more OCCT periodic knot insertion cases, speeds 2D curve intersections, and restores the local T0 gate.

- Forge matches checked OCCT 8.0.1 periodic duplicate pairs on degrees 2 to 5, including mixed first-span requests and quadratic triples. It also matches checked epsilon snaps, ties and near-equal pairs. Crossing-knot groups, nonpreserving repeated outputs and verified 10x targets remain open (#1909, #1912, #1917).
- The 2D curve intersection path adds prepared curves, and `ocad-libm::hypot` uses a faster path with the same checked result bits (#1915, #1913).
- Forge adds 49 `GCE2d` constructors that make no heap allocation, with differential OCCT 8.0.1 checks (#1911).
- The Gears workbench adds the crown gear (#1910).
- The Sketcher UI ledger records levels from the FreeCAD twin, and a real pump test checks point dragging after a constraint is made a reference (#1914, #1916).
- The pump drag test has typed sketch views, the Clipping panel has an icon, and the FreeCAD toolbar profile matches its generated toolset data. T0 passes again (#1918, #1919).

## 0.1.14

This patch adds bulk B-spline knot insertion, improves large assembly views, and adds more FreeCAD and OCCT behavior checks.

- Forge bulk knot insertion matches public OCCT 8.0.1 cases on nonperiodic curves and on periodic curves with single or sorted distinct requests. Periodic seam insertion preserves the curve on plain and rational degrees 2 to 5. Repeated periodic `Add` requests and verified 10x targets remain open (#1901, #1904).
- Knot form classification matches all 14,445 corrected overflow holdout calls and all 128 overflow grid calls. A separate boundary probe still differs on 338 of 40,178 calls (#1898).
- Fit All finds each shared part mesh box once. The desktop selects the visible engine body on startup, and it stops the kernel before OpenCascade shutdown (#1899, #1902, #1905).
- The Gears workbench adds the bevel gear. Point query finds the foot of a point close to a B-spline face (#1903, #1893).
- Pixel tests use the compositor on SwiftShader Vulkan, and the batch VM adds T1 with FT2 bit tests and an FB5 probe (#1900, #1906).
- The viewport reads its GPU name from the active device and can reopen a lost device without losing its scene. The 2 s recovery budget still needs a run on a machine GPU (#1907).

## 0.1.13

This source release builds drawing views together, adds two gears, refines FreeCAD key chords, and checks more OCCT knot cases.

- Drawing views and projection groups build together on the regen threads. A page of four views of 200 synthetic vehicle parts took at most 0.75 of its one-thread time on all cores (#1891).
- The Gears workbench adds the timing gear and the worm gear (#1888, #1895).
- A short key runs at once when its longer keys are disabled, and the spec records the hard keys of FreeCAD 1.1.4 (#1897, #1896).
- The UI ledger adds 445 gap rows for add-ons and Sketcher. Forge adds Bezier clipping for 2D curve intersections and analytic bisectors of points, lines and circles (#1892, #1890, #1887).
- B-spline knot form matches OCCT when the step between two knots overflows (#1889, #1894).

## 0.1.12

This patch makes a drawing view of many parts faster, adds the hypocycloid gear, uses the FreeCAD rule for key chords, and checks more knot cases against OCCT.

- A drawing view of the parts of an assembly finds the box of each different part one time and does not check the compound again. A front view of 200 parts of synthetic-vehicle-1k builds in about 2 s, where it took 7 s (#1884).
- The Gears workbench adds the hypocycloid gear (#1882).
- The chord engine uses the FreeCAD rule: only a full key that also starts a longer key has a timeout (#1878).
- The specs record the Python hosts, the trust rule for macros and add-ons, and the add-on twins (#1879).
- B-spline knot spacing and knot form match OCCT across zero, with adjacent steps and near negative exponent boundaries (#1880, #1881, #1883).

## 0.1.11

This patch makes large assemblies show with the kernel, adds the J-ASM-06 vehicle journey, adds gears and tangent circles, and checks more knot cases against OCCT.

- The parts of an assembly share the mesh of their leaf, in the kernel and on the GPU, as a FreeCAD App::Link does. 200 inserts of a NIST leaf build in 90 to 140 ms, where they took 6 to 9 s. The 20,000 parts of the J-ASM-06 vehicle show with the kernel about 8 s after the open on SwiftShader, where they took more than 5 minutes (#1872, #1876).
- The journey J-ASM-06 opens a vehicle of 20,000 parts, isolates the front left door and cuts the view with Clipping X. The door has the mass properties of FreeCAD 1.1.4. A selection on a model of 21,000 features no longer reads each feature, and the tree commits once in a frame (#1865).
- The Gears workbench adds the internal involute gear, the involute and cycloid racks, the cycloid gear and the lantern gear (#1861, #1864, #1870, #1875).
- The tangent circle solver adds the 12 constructors of `GccAna_Circ2d2TanOn`, with the center on a line or a circle (#1874). `ocad-forge-approx` adds the same parameter and the curve on surface approximation (#1869).
- B-spline knot form and knot analysis scan in one pass, with public OCCT 8.0.1 cases for periodic multiplicity and a holdout of 16,000 `KnotForm` calls (#1863, #1868, #1873).
- The Sketcher records the SK-W06 malformed sketch results and the SK-W07 key audit from FreeCAD 1.1.4. The Sketch On Sheet example reads back from OCL (#1862, #1867, #1871).

## 0.1.10

This patch checks more periodic surface isocurves against OCCT, reduces their allocation cost, extends solid classification and tangent circle construction, and improves sketch solving.

- Public OCCT 8.0.1 probes cover 64 writer calls and 64 owned isocurves on surfaces periodic in both directions. Plain and rational cases include nonuniform knots, doubled multiplicities and seam crossings. The output digests match on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1855).
- Owned isocurves share the surface's immutable periodic knot metadata. The periodic V output row falls from six to three Forge allocations per call and from 496 to 352 peak bytes with the same output sum. A diagnostic OCCT run measured 864 peak bytes and 1.92x Forge speed; the verified 10x targets remain open (#1859).
- The solid classifier uses box hierarchies, patch bounds and boundary strips to skip work that cannot affect a point. The plate and a 70 face part meet the 5 microsecond per point target in the reported run. The high face count corpus batch and near points on `occ/Ball.brep` remain open (#1858).
- The tangent circle solver adds all ten `GccAna_Circ2d3Tan` argument combinations with public OCCT probes and a behavior note. The Gears workbench involute gear adds spur, helical and double helical forms with a modeling guide and a bracket test (#1857, #1856).
- Sketch solving tries BFGS and SQP after DogLeg and Levenberg-Marquardt, as FreeCAD does. The Levenberg-Marquardt step uses the sparse Jacobian; a failed 500 bar solve fell from 77.8 seconds to 0.39 seconds in the reported release build (#1851, #1854).
- The command registry keeps id order and documents constraint name arguments. Sketch edit starts with its grid hidden, as FreeCAD does. Exact visual layer bit checks and valid tree icon kinds let T0 reach later checks; separate test failures remain open (#1847, #1848, #1850).

## 0.1.9

This patch adds knot removal and conic conversion, checks more surface isocurve cases against OCCT, and extends sheet metal unfolding.

- Forge can remove curve knots within a certified error bound and convert circles, ellipses, hyperbolas and parabolas to NURBS with the eight OCCT parameter forms. Public OCCT probes cover 368 conic cases and knot removal behavior. Periodic knot removal and surface knot removal remain open (#1838).
- Surface isocurve probes add 96 writer and 96 owned rows with nonuniform periodic knots and doubled multiplicities. A separate weight boundary probe found and fixed the rational flag and stored weight representation of direct curves, surfaces and owned isocurves at seven close weight pairs and two grids. The new weight tests pass on arm64, x86_64 and wasm32-wasip1 SIMD128. Verified 10x speed and resource targets remain open (#1842, #1845).
- Sheet unfold and bend lines can read the k-factor for the sheet thickness from a CSV material table. A new unfold sketch lays the flat sheet on XY at the origin and can separate its outline, bends, inner edges, holes, labels and cuts. The sketch was checked against 26 FreeCAD 1.1.4 cases (#1840, #1844).
- Popovers and tooltips read their side from the element. A submenu near the right edge now opens in one React commit, as checked by invariant I16 (#1843).
- The Sketcher plan records FreeCAD 1.1.4 answers for the remaining SK-W04 questions (#1841).

## 0.1.8

This patch reduces owned surface isocurve allocations, adds two OCCT compatible tangent circle solvers, extends sheet metal bend cuts, and adds assembly drawing, clipping and sub-assembly work.

- Owned surface isocurves use the surface's stored flat knots and pole grid. The bicubic nonperiodic U row fell from 17 to three Forge allocations per call. In paired periodic rows, fixed periodic U fell from 20 to three allocations and periodic V output fell from 25 to six. OCCT owned curve rows and a Windsor STEP face pass on arm64, x86_64 and wasm32-wasip1 SIMD128. The timing and heap runs do not establish a verified 10x baseline (#1829, #1832, #1834, #1837).
- `ocad-forge-construct` adds tangent circle solvers for a fixed radius with one center curve or two tangent inputs. Public OCCT probes check solution order, qualifiers, flags, tangent points and centers (#1830).
- Sheet metal bend lines can become spaced relief slots, and a sketch can set the open path of each cut (#1828, #1835).
- Journey J-DRW-05 makes an assembly drawing with a projection group, a section, a detail, balloons and a bill of materials. The section cuts each assembly part on its own (#1827).
- View > Clipping View opens axis and custom clipping controls, and New Assembly makes a sub-assembly when another assembly is active (#1833, #1836).
- The desktop test channel waits for the page ready signal. UI lint passes with the shared selection and native overlay fixes (#1831).

## 0.1.7

This patch adds more OCCT checks for surface isocurves, reduces allocations in owned isocurve calls, speeds up B-spline interpolation and point fitting, reads sheet metal bend factors from a material table, and adds Sketcher visual layers.

- Surface isocurves now match 80 more OCCT writer calls with mixed periodic directions and weight patterns. A direct 80-call `Geom_BSplineSurface::UIso` and `VIso` probe found the constant weight rule for owned nonrational curves, which Forge now follows (#1819, #1821).
- Four paired rows measure owned surface isocurves against OCCT. The bicubic U row found 40 Forge allocations per call. Reusing knot data reduced it to 17 allocations per call while direct OCCT and Windsor STEP checks stayed green. The timing runs exceeded the load gate, so the 10x target remains open (#1823, #1824).
- B-spline interpolation and point fitting use span cursors and less work in banded solves. The 1,000-point helix fit took 2,816 microseconds on the shared test server, down from 5,934 microseconds before this change. A smoothing fit now merges its penalty rows in band order (#1820).
- Sheet unfold and bend lines can read each bend's k-factor from a material table based on its radius and thickness (#1825).
- Sketcher can move geometry to Layer 0, Layer 1 or Hidden from the Elements menu. The choice survives FCStd import and OCL round trips, and an undo restores the prior layer (#1822).

## 0.1.6

This patch adds Export PDF for drawing pages, views of assemblies on a drawing page, and Activate Assembly. It also adds sheet metal bend perforation and bend lines, surface isocurves, and a corpus test of the solid classifier against OCCT.

- Export PDF in the File menu saves the drawing page in its tab as a vector PDF file at the paper size of its template. In the 3D view, it saves the image of the view on an A4 page (#1816).
- A TechDraw View, Broken View or Projection Group of an assembly shows the parts of the assembly, and `of` of `drawing_view` can be a list of bodies (#1812).
- Activate Assembly makes an assembly the active one, with the active assembly edit mode of FreeCAD (#1806).
- `sheet_wall` can perforate its bend, as the Perforation option of the SheetMetal workbench does. Nine walls pass against FreeCAD 1.1.4 (#1810).
- `sheet_bend_lines` gives the bend lines of an unfolded sheet, with the bend angle beside each line. 31 cases pass against FreeCAD 1.1.4 (#1817).
- `NurbsSurface::u_iso` and `v_iso` return isocurves as owned curves. The isocurve writer fills caller buffers and matches 80 OCCT `BSplSLib::Iso` calls. Paired timing rows compare it with OCCT (#1809, #1814, #1815).
- The solid classifier has a batch API on threads and a corpus test against `BRepClass3d_SolidClassifier` (#1808).
- The desktop test build opens a real FCStd file through File > Open (#1811). The desktop lock file names the release version (#1813).
- The Sketcher spec answers its questions on tool handlers and BFGS (#1807).

## 0.1.5

This patch adds surface D3 evaluation and its paired benchmarks. It also adds pointer drag for assemblies, settles six mass property corpus differences and adds Solid to Sheet Metal.

- A local basis path evaluates surface D3 on 50 lower-degree OCCT calls, 12 degree 25 exact rational references and nine rational Windsor STEP calls. Output digests match on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1802).
- Paired surface D3 timing rows have equal Forge and OCCT checksums. A failed-gate Apple M4 diagnostic measured 0.53x bicubic and 1.00x rational degree seven OCCT speed. The bicubic Forge path measured zero allocations and zero peak heap bytes (#1803).
- Six closed corpus shapes outside the mass property tolerance now have independent evidence for documented OCCT defects. The B-spline pcurve evaluator, knot crossing search and inner integral buffer are faster (#1800).
- Assembly parts can be dragged with the pointer in the 3D view. A Revolute joint turns the part about its hinge, and release makes one undo step (#1801).
- Solid to Sheet Metal makes walls and bends from the faces of a solid, with picked faces open and picked edges ripped. Seven FreeCAD 1.1.4 cases pass (#1804).

## 0.1.4

This patch adds surface D1 and D2 evaluation, point interpolation, curve fitting and desktop FCStd opening. It also adds SheetMetal Forming and restores the Sketcher toolbar in the FreeCAD profile.

- A local basis path evaluates surface D1 on 62 public OCCT cases and nine rational Windsor STEP calls. Twelve degree 25 calls use an exact rational reference. Output digests match on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1791).
- Paired surface D1 timing and heap rows have equal OCCT and Forge checksums. A failed-gate Apple M4 diagnostic measured 0.60x bicubic and 0.79x rational degree seven OCCT speed. The bicubic Forge path measured zero allocations during evaluation (#1792).
- A local basis path evaluates surface D2 on the same 62 OCCT cases, 12 degree 25 exact references and nine rational Windsor STEP calls. Output digests match on the three targets (#1795).
- Paired surface D2 rows have equal checksums. A failed-gate Apple M4 diagnostic measured 0.69x bicubic and 1.03x rational degree seven OCCT speed. The bicubic Forge path measured zero allocations during evaluation (#1797).
- Forge interpolates open and periodic B-spline curves and fits points within a tolerance. Differential tests check the OCCT API, and a behavior note records where OCCT gives a different result (#1790).
- The Sketcher toolbar returns to the FreeCAD profile with New Sketch, Attach Sketch, Reorient Sketch, Validate Sketch, Merge Sketches and Mirror Sketch (#1793).
- File > Open reads FCStd files in the desktop app and loads the document, viewport and kernel together. A real mini vacuum pump test checks seven visible bodies, 30 sketches and 532 constraints (#1798).
- The F-rep feature matrix records the last checked cells and keeps its TOML twin in step. Its check reports six done and 38 open rows at FP0 (#1794).
- `sheet_forming` presses a tool body into a sheet and matches eight FreeCAD 1.1.4 cases. The SheetMetal guide has an example (#1796).

## 0.1.3

This patch adds a part drag to assemblies, two SheetMetal commands and two shell menus. It also checks the facts of the F-rep spec and adds the check of the F-rep feature matrix.

- Drag Part (`asm.drag`) moves a part of an assembly to a placement, as a drag in the 3D view of FreeCAD does. The joints pull the part back to the nearest place where they hold, and the joined parts follow it. The host has a live drag, and the Newton step of the solver leaves out the products that are zero (#1780, #1783).
- `sheet_base_shape` makes a plate, an L, a U, a tub, a hat or a box of sheet, as the Base Shape of the SheetMetal workbench does. Twenty-one FreeCAD 1.1.4 cases pass (#1777).
- `sheet_sketch_on` cuts the loops of a sketch into a sheet metal part as if drawn on the flat pattern, over the bends. Ten FreeCAD 1.1.4 cases pass (#1787).
- File > Open Recent and View > Freeze Display are filled, and the Overlay Docked Panel submenu shows only when overlay is on (#1778).
- The Workbench bar lists the 15 FreeCAD workbenches. The 7 that run can be selected, and each other one is disabled with a reason (#1779).
- Imported FCStd sketch constraints keep the FreeCAD geometry and point IDs, and Sketcher shows them when Extended information is on (#1788).
- A surface point method forms the local U and V bases once, with a rational degree 7 timing row and nine pinned OCCT points from the Windsor STEP model (#1785).
- The facts of `spec/geometry/function/` that were marked "not verified" are checked against primary sources. The Lipschitz bound of the gyroid is now `√3 k`, and 07 has a table of the gyroid volume fraction (#1781).
- `cargo xtask frep-matrix` (FT14) checks the F-rep feature matrix against its TOML twin and reports each row as done, open or late. CI runs it on every merge (#1786).
- The ASMT journey comparison test is formatted (#1784).

## 0.1.2

This patch adds Sheet Metal Unfold and three Assembly journeys that check the GUI against FreeCAD 1.1.4.

- Sheet Metal Unfold lays a sheet metal part flat from one face, as the Unfold of the SheetMetal workbench does, with an ANSI or DIN k-factor. Twenty-five FreeCAD 1.1.4 cases pass (#1774).
- Component takes a turn about X, Y and Z. J-ASM-07 puts the 28 parts of the wheels of the synthetic vehicle in an assembly, explodes the wheel bolts in two steps and makes a bill of materials, as FreeCAD does (#1772).
- J-ASM-11 opens the door of the synthetic vehicle on its hinge in a simulation and plays it. Each frame matches FreeCAD to 1e-6 mm (#1773).
- J-ASM-12 writes the seat of the synthetic vehicle with Export ASMT File. The file has the lines that FreeCAD writes, with the numbers the same to a billionth (#1775).

## 0.1.1

This patch adds more periodic B-spline curve checks, a compact surface point path and paired timing rows. It also adds field VM determinism checks and an Assembly insert timing span.

- High-degree periodic curve derivatives pass 432 more OCCT calls with nonuniform knots and repeated interior knots. An exact rational reference found one degree 25 OCCT result that differs by `1.11e-8` (#1764).
- `SurfaceD0View` evaluates plain and rational surface points from compact knots and a U-major control grid. It passes 62 pinned OCCT calls with one matching output digest on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1765).
- Paired compact surface D0 rows have equal OCCT and Forge checksums. A failed-gate Apple M4 diagnostic measured 1.02x bicubic and 0.26x rational OCCT speed. The 10x target remains open (#1769).
- The field VM checks 163 fields across threads and targets. Three `ocad-libm` edge cases now keep the same output bits on the checked targets (#1766).
- Insert Component records A-ASM-INSERT on a 1,000-part synthetic vehicle. The element pick layer is deferred until after the result paint (#1768).
- The planar CDT uses an incircle filter during its build passes and an exact final pass. The 256-hole plate triangles stay the same, with about 3x faster CDT timing on that case (#1767).
- Sheet Metal Corner Relief cuts the meeting point of two bends with circle, square and sketch loop modes, or closes it with a weld. Nineteen FreeCAD 1.1.4 cases pass (#1770).

## 0.1.0

This release closes milestone M0 of the shell topic (SHL). The FreeCAD 1.1.4 shell is recorded row by row, and the UI ledger now computes a level for the shell panels, status bar items and dialogs. Forge adds field Booleans and transforms and more periodic derivative checks. Sketcher can show geometry references in the constraint list.

- The ledger finds the OpenCAD twin of a FreeCAD dock through the `freecadDock` value of a panel, and the twin of a status bar item or dialog through a `data-freecad` mark. Report view, Tasks, the status message, the progress bar, the input hints and the About dialog are at C2 (#1762).
- The shell spec closes each item that M0 had to confirm, from the selector menu to the critical message box, against the FreeCAD 1.1.4 dump in `opencad-compat` (#1687).
- `ocad-forge-field` adds union, intersect, subtract, xor, complement and clip, and the transforms of FO-TR-01 to FO-TR-06 (#1756).
- High-degree periodic 3D B-spline derivatives pass 360 pinned OCCT calls through order five and 72 calls at order six, at degrees 7, 12, 20 and 25 (#1758, #1761).
- The Sketcher Constraints menu has Extended information (in widget), which shows the geometry name and point position of each constraint. The setting persists across app starts (#1759).
- Journey J-DRW-04 makes a TechDraw page of NIST CTC-01 with a first angle projection group and three extent dimensions (#1760).
- Spec 03 names `part.sheet_fold` for SheetMetal_AddFoldWall, so the compat parity check passes again (#1757).

## 0.0.166

Forge adds paired order-six timing and an owned cache for repeated high-degree Bezier derivatives. Sketcher can remove confirmed redundant constraints after a command when the new setting is on.

- Degree 25 plain and rational order-six `DN` rows have equal Forge and OCCT checksums. An unverified Apple M4 diagnostic measured 0.84x plain and 0.90x rational OCCT speed before caching (#1751).
- An owned order-six Bezier cache uses at most 5,888 bytes and keeps direct output bits on checked in-span cases across arm64, x86_64 and wasm32-wasip1 SIMD128. An unverified diagnostic measured 28.87x plain and 2.11x rational OCCT speed for repeated order-only calls (#1752).
- The rational order-only cache keeps the same output bits. Five alternating local batches measured a median of 777 ns per old call and 473 ns per new call. An unverified paired diagnostic measured 4.20x rational OCCT speed (#1754).
- Sketcher can remove a redundant constraint after a command when the solver confirms that the sketch keeps the same degrees of freedom. The setting starts off, persists across app starts and shares the command's Undo step (#1753).

## 0.0.165

Forge adds a repeated-call cache for high-degree order-five B-spline derivatives and exact-reference checks for order six. Sketcher can create a dimensional constraint at its measured value.

- An owned order-five Bezier cache uses at most 5,056 bytes and keeps 160 direct and exact-reference cases bit equal on arm64, x86_64 and wasm32-wasip1 SIMD128. A failed-gate Apple M4 diagnostic measured 32.79x plain and 3.26x rational OCCT speed for repeated order-only calls (#1746).
- The cached rational order-five path keeps the same output bits. Five alternating local batches measured 330 to 354 ns per old call and 218 to 225 ns per new call. An unverified paired Apple M4 diagnostic measured 4.75x OCCT speed (#1747).
- High-degree order-six `BSplCLib::DN` passes 56 Bezier and 104 piecewise OCCT calls at degrees 7, 12, 20 and 25 with exact rational references on three targets. A direct Bezier path fixes a degree 12 knot-side error (#1749).
- Sketcher can use the measured value of a new dimensional constraint when Ask for value after creating a dimensional constraint is off. The setting starts on and persists across app starts (#1748).

## 0.0.164

Forge adds exact-reference checks for high-degree order-five B-spline derivatives and a direct Bezier path. Sketcher adds a setting for separate coincident constraint tools.

- A dense 1,064-call order-four Bezier grid checks cached and direct output against an exact rational reference on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1741).
- Order-five `BSplCLib::DN` passes 56 Bezier and 104 piecewise OCCT 8.0.1 calls at degrees 7, 12, 20 and 25 with exact rational references on three targets (#1742).
- Paired degree 25 order-five rows match OCCT checksums. An unverified Apple M4 diagnostic measured 0.21x plain and 0.20x rational OCCT speed before the direct path (#1743).
- The direct order-five Bezier path keeps the 160 exact-reference cases passing on three targets. An unverified Apple M4 diagnostic measured 0.98x plain and 0.99x rational OCCT speed. The 10x target remains open (#1744).
- Sketcher can show separate Coincident Constraint and Point on Object Constraint tools when Unified coincident constraint is off. The setting starts on, so existing projects keep the unified tool (#1740).

## 0.0.163

Forge checks high-degree 3D B-spline order-four derivatives and adds a cache for repeated Bezier calls. Sketcher defers dependent solid builds while editing and speeds up imported STEP face picks.

- Order-four `BSplCLib::DN` passes 56 clamped Bezier and 104 piecewise OCCT calls at degrees 7, 12, 20 and 25 with exact rational references on three targets. A linear Bezier path fixes a knot-side error (#1734).
- Paired degree 25 selected-span order-four rows match OCCT checksums. An unverified Apple M4 diagnostic measured 1.18x plain and 1.25x rational OCCT speed (#1735).
- An owned order-four Bezier cache uses at most 4,192 stored bytes and zero measured heap bytes during evaluation. An unverified Apple M4 diagnostic measured 27.47x plain and 3.87x rational OCCT speed for repeated order-only calls (#1738).
- Sketcher can defer dependent solid builds while editing and build the latest solid when Auto-update is enabled or the sketch closes (#1733).
- Sketcher indexes triangles from an imported STEP body for face picks. The WindsorML ray test measured 1.37 ms for 1,000 indexed picks against 106.52 ms for the prior linear scan, with the same face and distance (#1736).
- Sketcher skips STEP triangles with nonfinite coordinates so they cannot hide valid faces in the index (#1737).

## 0.0.162

Forge extends high-degree 3D B-spline second and third derivatives with public OCCT probes and exact rational checks. Plain cubic cached derivatives also take a shorter path.

- Plain cubic cached D1 through D3 keep pinned digests on three targets. A failed-gate Apple M4 diagnostic measured 11.72x OCCT speed for D1; D2 and D3 remain below 10x (#1729).
- High-degree `BSplCLib::D2` passes 56 Bezier and 104 piecewise cases at degrees 7, 12, 20 and 25 with exact rational references on three targets. A paired Apple M4 run measured 7.96x plain and 9.13x rational OCCT speed, with CPU frequency unverified (#1730).
- High-degree `BSplCLib::D3` passes the same case counts and degrees on three targets. A paired Apple M4 run measured 6.00x plain and 6.91x rational OCCT speed, with CPU frequency unverified (#1731).

## 0.0.161

Forge speeds up high-degree Bezier first derivatives while keeping the pinned OCCT and exact-reference results. Sketcher adds constraint tools to the Geometry list and remembers which task sections are open.

- Degree seven and higher Bezier `D1` uses a linear Bernstein recurrence. The pinned cases pass on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1723).
- An owned Bezier D1 span cache packs homogeneous poles once for repeated calls and uses at most 864 stored bytes without a heap allocation (#1725).
- Direct selected-span D1 reads source poles in one pass. A failed-gate Apple M4 diagnostic measured 10.41x plain and 12.03x rational OCCT speed at degree 25 with matching checksums. The idle speed baseline and 10x resource target remain open (#1727).
- The Sketcher Geometry list can apply dimension and constraint tools to selected geometry (#1724).
- Sketcher remembers the open state of Solver messages, Geometry and Constraints across edits and app restarts (#1726).

## 0.0.160

Forge pins high-degree 3D B-spline first derivatives against OCCT and an exact rational reference. The geometry ledger report now separates pinned packages from a future OCCT package. Sketcher adds an Auto constraints setting.

- Plain and rational degree 7, 12, 20 and 25 Bezier `BSplCLib::D1` calls match 56 pinned OCCT records with one output digest on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1717).
- A 104-call piecewise D1 fixture stores OCCT output and exact rational values. Forge passes the exact checks on three targets; eight degree 25 OCCT calls differ from the exact result by more than `1e-8` (#1718).
- Paired degree 25 selected-span D1 rows match OCCT checksums. A failed-gate Apple M4 diagnostic measured 0.53x to 0.54x OCCT speed, so the speed target remains open (#1721).
- The GW1 ledger report separates 183 pinned OCCT 8.0.1 packages from one 8.1 forecast package. The local scan passes; the provenance-bearing lock remains open (#1719).
- Sketcher can disable inferred constraints while drawing snapped geometry and keeps the setting when a sketch reopens (#1720).

## 0.0.159

Forge adds third and general 3D B-spline curve derivatives, paired OCCT timing rows and a compact cubic derivative cache. Sketcher keeps FreeCAD constraint names and resolves named dimensions in expressions.

- Forge checks 3D `BSplCLib::D3` against 348 public OCCT cases on arm64, x86_64 and wasm32-wasip1 (#1710).
- Forge checks order-four `BSplCLib::DN` against 348 public OCCT cases and rational derivatives through order six above the polynomial degree (#1711).
- Eight paired FTC-10 derivative rows match OCCT checksums. The direct path remains below the speed target (#1713).
- A 160-byte cubic derivative cache checks 70 OCCT cases per order on three targets. An unverified Apple M4 diagnostic measured 5.03x to 8.38x OCCT speed, so the 10x target remains open (#1714).
- Sketcher keeps FreeCAD constraint names in OCL and resolves named dimensions such as `Sketch.Constraints.width` in expressions and FCStd import (#1712, #1715).

## 0.0.158

Forge adds 3D B-spline curve first and second derivatives checked against OCCT 8.0.1. Sketcher can hide constraint labels and save their visibility in OCL.

- Forge evaluates 3D `BSplCLib::D1` against 348 public OCCT cases, including FTC-10, with one output digest on arm64, x86_64 and wasm32-wasip1 (#1706).
- Forge evaluates 3D `BSplCLib::D2` against 348 public OCCT cases with the same cross-target digest check (#1708).
- Sketcher can show or hide constraint labels from the constraint list and saves hidden labels in OCL. The command is undoable (#1705, #1707).

## 0.0.157

Forge adds packed B-spline span evaluation and 3D curve point evaluation against OCCT 8.0.1. SheetMetal adds Make Fold, the measure tools can pick vertices in the view, and the sketch tree opens Edit Sketch. The spec names the real SheetMetal commands.

- Forge evaluates packed `BSplCLib::Eval` spans with matching bits across 1,864 public OCCT cases and adds a paired FTC-10 timing row (#1701).
- Forge evaluates plain, rational and periodic 3D `BSplCLib::D0` curve points with matching bits across 1,184 public OCCT cases. Reused selected spans are faster than OCCT in the paired FTC-10 diagnostic (#1702).
- SheetMetal adds `sheet_fold`, the Make Fold command (#1700).
- Quick Measure and the Measure tool pick vertices directly in the view (#1699).
- The sketch tree calls the context menu action Edit Sketch and opens the selected sketch in the 2D editor (#1703).
- The SheetMetal section of spec 03 uses the real command names (#1698).

## 0.0.156

Forge adds Schoenberg interpolation points and speeds up cubic B-spline basis derivatives. The mesh kernel gets its data model and planar constrained triangulation. F-rep adds primitive nodes, Sketcher updates tool controls and Escape behavior, and the web test build keeps local storage at startup.

- F-rep adds the primitive nodes of its field graph (#1689).
- Sketcher adds continuous mode controls for its tools (#1690).
- Forge evaluates cubic B-spline basis derivatives with a shorter recurrence and keeps the public OCCT comparison green (#1692).
- The mesh kernel adds its data model, constrained Delaunay triangulation in UV and a plane path (#1691).
- The web test build keeps local storage when its page starts (#1693).
- Forge writes Schoenberg interpolation points with exact OCCT output bits across 203 cases (#1694).
- Sketcher honors the leave sketch with Escape preference (#1695).
- SheetMetal gives Extend Face its own command title (#1696).

## 0.0.155

Forge adds 2D analytic intersections and B-spline basis evaluation. Sketcher batches preselection, SheetMetal adds Make Hem, and the shell topic closes its M0 questions.

- Forge pairs BSplCLib knot analysis timing with OCCT and writes distinct knots and multiplicities from a flat knot sequence (#1681, #1683).
- Forge evaluates B-spline basis values and derivatives against 1,866 public OCCT records across degrees 1 through 25 (#1686).
- Forge adds 2D analytic intersections with certified results in `ocad-forge-int` (#1682).
- Sketcher batches geometry preselection and updates hover status (#1684).
- SheetMetal adds `part.sheet_hem`, the Make Hem command (#1685).
- The shell topic records its FreeCAD dump and closes the M0 questions (#1687).

## 0.0.154

TechDraw adds Weld Symbol and Update Template Fields, and SheetMetal adds Make Wall. Sketcher improves its geometry and constraint lists, and Forge adds more BSplCLib knot functions and the GccAna line solvers.

- TechDraw adds Weld Symbol, an AWS welding symbol on a leader line (#1669).
- TechDraw adds Update Template Fields. It fills the title block of a page and takes the scale of the first view on the page as a fraction, as FreeCAD does (#1679).
- SheetMetal adds `sheet_wall`, the Make Wall command (#1673).
- Sketcher shows internal geometry in the Geometry list and can filter the constraint marks in the view with the Constraints list (#1670, #1677).
- Forge adds BSplCLib knot reparameterization, knot reversal and knot classification (#1674, #1676, #1678).
- Forge adds the GccAna line solvers and Circ2dTanCen in `ocad-forge-construct` (#1671).
- The Hermite matrix path uses less work storage (#1672).
- The UI ledger has rows for the 13 shell gaps of M0 (#1675).

## 0.0.153

The F-rep work gets the tape of a field and T0, the point VM that is the reference for the other evaluators. SheetMetal adds Make Cutout, TechDraw adds Rich Text Annotation, and Forge computes more PLib Hermite cases.

- The F-rep field graph lowers to a tape of 8-byte instructions, with registers from a single reverse pass (#1661).
- T0 runs a tape on one point. Each opcode follows `SEMANTICS.md`, and constant folding gives the same bits. `ocad-libm` adds `sinh`, `cosh`, `tanh` and `exp2`, and `pow` now gives 1 for `pow(NaN, 0)` and `pow(1, NaN)`, as C Annex F says (#1667).
- SheetMetal adds `sheet_cutout`, the Make Cutout command. A cut that crosses a bend follows the bend (#1660, #1662).
- TechDraw adds Rich Text Annotation, with the HTML text of the TechDraw text editor, on a page, a view or a leader line (#1663).
- Forge computes low-order close-span PLib Hermite interpolation with the direct endpoint basis, and the bench times the Hermite matrix with OCCT (#1665, #1666).
- The desktop lockfile has the current Forge crates again, so the desktop build can use `--locked` (#1664).

## 0.0.152

Forge extends PLib Hermite interpolation through degree 25 and adds an OCCT facade for the F0 classes. Sketcher, SheetMetal, TechDraw and Curves add FreeCAD behavior. The F-rep field graph gets a canonical form and hash.

- Forge supports order seven on checked zero-start spans, then computes the exact endpoint basis for high-order and close-span Hermite cases. The direct output bits match on arm64, x86_64 and wasm32-wasip1 SIMD128 (#1647, #1657).
- Forge adds the `ocad_forge::occt` facade for F0 classes (#1656).
- Sketcher links geometry hover to its list and adds geometry list actions. The construction toggle now uses the selected geometry (#1649, #1651).
- SheetMetal adds `sheet_relief`, the Make Relief command, and `sheet_bend`, the Make Bend command (#1650, #1653).
- TechDraw adds Complex Section View, which cuts a view along a sketch (#1652).
- Curves uses the FreeCAD Curves workbench command names (#1655).
- The F-rep field graph adds a canonical form and hash (#1654).
- The web core lockfile includes the new Forge crates (#1648).

## 0.0.151

TechDraw adds Broken View, Print All Pages and Project Shape, each checked against FreeCAD on the SAE Notchback. SheetMetal adds two commands, and the F-rep work gets its field graph and its type checker.

- TechDraw adds Broken View, which takes parts out of a long body in a view, with the lines and the break lines of FreeCAD (#1634).
- TechDraw adds Print All Pages, which prints each page of the model on a sheet of paper of its own size (#1641).
- TechDraw adds Project Shape, the visible and hidden edges of a body from a direction as a flat body. The body stays in the 3D view (#1644).
- SheetMetal adds `sheet_extend`, the Extend Face command, and `sheet_junction`, the Make Junction command (#1637, #1642).
- Sketcher filters the geometry list by layer and by type (#1635).
- Forge pins the high-order PLib Hermite matrix behavior, and checks the face list counts of BREP files with exact bit checks (#1636, #1639).
- The F-rep work adds the field graph with hash-consing, parameters and node info, and the type checker for kind, unit, distance class, Lipschitz bound and box (#1640, #1643).
- The desktop lockfile has the Forge crates (#1638).

## 0.0.150

Forge matches more PLib conversions of OCCT 8.0.1 and classifies points in faces and solids. The FreeCAD pin moves to 1.1.4, and the test channel gets the UI calls of the topic specs.

- Forge matches the PLib Jacobi average error, degree reduction and low-degree conversion, and the low-degree Hermite-Jacobi conversion. It computes high-order Hermite interpolation from a stable basis (#1544, #1616, #1619, #1621, #1624).
- Forge classifies a point in a face or a solid (GW16), and computes the BRepGProp mass properties of `.brep` shapes, tested against OCCT on the corpus (GW15) (#1590, #1629).
- Sketcher adds the constraint list actions and filters the constraint list by FreeCAD type (#1554, #1630).
- TechDraw adds Spreadsheet View, a table of the cells of a sheet on a page (#1625).
- SheetMetal adds `sheet_base`, the Make Base Wall command (#1626).
- The test channel gets the UI calls of `spec/compat/freecad/05` section 2 and `idBuffer`, the entity of each pixel of the viewport. `oc-drive` gets the typed UI calls, and the answers have the same shape on web and desktop (#1543, #1620, #1622).
- The spec pins FreeCAD 1.1.4 for the source and the GUI, the hexagonal profile gets a parity row, and the F-rep spec is in `spec/geometry/function` (#1615, #1628, #1627).
- The F-rep work starts with the crates `ocad-forge-field` and `ocad-forge-implicit`, and the opcode semantics are in `ocad-forge-field/SEMANTICS.md` (#1631, #1632).

## 0.0.149

Forge checks more of the PLib Jacobi basis of OCCT 8.0.1. Curves, Sketcher and TechDraw add focused FreeCAD behavior, and the harness gets a proof bundle for each journey.

- Forge matches the PLib Jacobi Gauss points, weights, maximum basis values and maximum error bounds of OCCT (#1538, #1539, #1541).
- Curves sweeps profiles round the point where they meet, and carries a rotation sweep past its profiles, as Rotation Sweep does (#1468, #1536).
- Sketcher shows its commands in the menu of the sketch view, and the desktop app picks STEP faces to attach a sketch (#1537, #1535).
- TechDraw adds Surface Finish Symbol, and a symbol can now go on a view at a vertex, a line or a face and move with the view. A text field of a form now keeps a leading `=`, so a formula typed in a cell form stays a formula (#1540).
- The FreeCAD UI surface is split into 27 topic folders with the milestones M0 to M4, and `cargo xtask proof` writes the proof bundle of a journey (#1393, #1488).

## 0.0.148

Forge checks more of the OCCT 8.0.1 PLib basis behavior. Sketcher, Curves and TechDraw add focused FreeCAD behavior.

- Forge checks the PLib Hermite basis matrix, adds the Jacobi and Hermite-Jacobi basis through degree 30, and checks 2,624 OCCT records for evaluation and power coefficient conversion (#1383, #1392, #1434).
- Sketcher keeps constraints on rotated FreeCAD sketches and checks native editing of the Windsor roof sketch (#1390, #1385).
- Curves joins faces into one surface and lays an edge on a face (#1384, #1388).
- TechDraw repairs dimension references after geometry changes (#1387).
- The UI ledger uses the pinned FreeCAD source, and the workspace format and Clippy gates pass again (#1386, #1389, #1391).

## 0.0.147

Forge adds more OCCT compatible PLib conversions. Sketcher, Curves and TechDraw gain focused FreeCAD behavior.

- Forge converts PLib coefficients to poles, computes interleaved rational derivatives and checks the Hermite interpolation range as OCCT does (#1370, #1372, #1376).
- Sketcher keeps nearby points distinct in large models (#1374) and imports FreeCAD sketch B-splines as editable curves (#1380).
- Curves makes the curve where two pushed curves meet, as Mixed Curve does (#1373), and cuts faces into waterlines, as WaterLine does (#1379).
- TechDraw adds Export Page as DXF to save the lines, dimensions and annotations of a page as a DXF file (#1371).
- TechDraw adds Dimension, which makes the dimension that fits a pick of lines, vertices or a face, with the M key to the next type (#1381).

## 0.0.146

Forge adds OCCT compatible polynomial length and pole packing. Sketcher, Curves, and TechDraw gain focused FreeCAD behavior.

- Forge matches both PLib polynomial length overloads against 648 pinned OCCT cases, including the reported integration error and NIST FTC-10 control data (#1364, #1366).
- Forge packs and unpacks weighted and unweighted 2D and 3D PLib poles against 151 pinned OCCT records, including a zero weight (#1368).
- Sketcher preserves FreeCAD coincident constraints (#1362).
- Curves draws an edge curvature comb using the ParametricComb behavior (#1365).
- TechDraw adds Active View to place an image of the 3D view on a drawing page (#1367).

## 0.0.145

Sketcher keeps external references after a FreeCAD source edit. TechDraw adds Bitmap Image. Forge extends the PLib numerics needed by Geometry.

- Sketcher rebuilds projected geometry from the current solid after a source dimension changes, including the external edges of four sketches in the real mini vacuum pump (#1352).
- Bitmap Image embeds a PNG, JPEG, BMP or GIF in a TechDraw page and supports position, scale, rotation, cropping, stack order and clip groups (#1356).
- Forge matches OCCT 8.0.1 PLib bivariate polynomial derivatives, Lagrange interpolation, Jacobi setup, curve trimming, weighted trimming and surface U/V trimming with pinned public API fixtures (#1353, #1354, #1355, #1358, #1359).
- Point projection queries share the curve and surface hierarchy kept with the geometry record, and the three GW14 projection benchmark rows run faster (#1357).
- The remaining source formatting errors on main are fixed (#1348).

## 0.0.144

The Curves workbench adds twelve commands. Forge closes more OCCT 8.0.1 transform boundary gaps.

- MultiLoft, Parametric Solid, Segment Surface, Sketch On Surface, Sweep 2 Rails, Trim Face, Profile Support and Helical Sweep add surface and sweep workflows to Curves (#1333, #1337, #1335, #1338, #1339, #1340, #1342, #1343).
- CompressionSpring, ReflectLines, Flatten Face and Truncate Extend add spring construction, drawing lines, face flattening and body trimming to Curves (#1334, #1336, #1344, #1345).
- Forge matches the OCCT 8.0.1 Release behavior of 2D `SetValues`, scale factors, scale constructors, inverses and powers at their boundary inputs. The topology archive now rebuilds these transforms with the same stored bits (#1332, #1341, #1346, #1347, #1349, #1350).

## 0.0.143

TechDraw adds Clip Group and Insert SVG. The desktop Sketcher projects edges of bodies built in the document, and Forge matches more OCCT transform forms.

- Clip Group makes a frame on a drawing page that crops the views in it. Add View To Clip Group and Remove From Clip Group keep the place of the view on the page (#1322).
- Insert SVG puts an SVG symbol on a page from SVG text or from an SVG file that the form reads. The symbol has the size that TechDraw 1.1 gives it, with scale, rotation, stack and clip group, on the page and in the SVG file (#1329).
- The desktop Sketcher projects and intersects edges of bodies built in the document, and it refreshes them after an edit of the source feature (#1321, #1328).
- The FreeCAD reader keeps the pocket openings of an imported Shape Binder, so the dependent sketch gets its external references (#1323).
- Forge matches the OCCT 8.0.1 forms of 2D translations near zero, of scale factors in 2D and 3D, and of 2D mirrors and scales when the translation changes (#1326, #1327, #1330).
- Forge adds the level C3 check of its `.brep` writers, and the geometry ledger refreshes an old writer run id (#1324, #1325).

## 0.0.142

Sketcher can project imported STEP edges in the desktop app, and TechDraw adds Move View and Share View. The geometry ledger now records pinned header hashes and FreeCAD use counts.

- External Projection and External Intersection in Sketcher read edges from imported STEP files, including nested V6 engine parts and edges from an opened FreeCAD file. The viewport loads STEP edges when its mesh came from cache (#1315).
- Share View shows a drawing view on another page, and Move View puts it on a new page with one undo step. The commands also handle the details, sections and dimensions of the view (#1318).
- The GW1 ledger scan records SHA-256 hashes for 2,800 public and alias headers, counts FreeCAD 1.1.3 geometry uses, and fixes the item count to 3,727 unique keys. CI can generate and verify `ledger.lock` with a writer run id; the first generated lock is still pending (#1316, #1317, #1319).

## 0.0.141

TechDraw adds Geometric Hatch and Leader Line.

- Geometric Hatch puts the lines of a PAT pattern of FCPAT.pat on picked faces of a view, with its scale, rotation, colour and width. The kernel cuts the lines to the face as TechDraw cuts them, and the page tab and the SVG file draw them under the lines of the view (#1311).
- Leader Line draws a line from a point of a view through points on the page, with one of the 8 symbols of TechDraw at each end. A click in a view sets the start, and each next click on the page adds a point. The last part of the line can be horizontal, and the line turns with the view, as in TechDraw (#1313).

## 0.0.140

TechDraw adds New Page From Template and Image Hatch.

- New Page From Template makes a page with the sheet that you choose from the six sheets of OpenCAD, and the template of New Page is now the same list (#1308).
- Image Hatch puts one of the 8 line patterns of TechDraw on picked faces of a view, with its scale, rotation and colour. The page tab and the SVG file draw the pattern under the lines of the view (#1309).

## 0.0.139

TechDraw adds Select Line Attributes, Change Line Attributes and Remove Cosmetic Object.

- Select Line Attributes opens a panel with the line type, width and color of the session, and the cascade spacing and delta distance. Change Line Attributes puts that format on cosmetic lines and center lines, and the cascade commands, Extend Line and Shorten Line start with the spacing and the distance (#1305).
- Remove Cosmetic Object, the Cosmetic Eraser of TechDraw, removes cosmetic vertices, cosmetic lines and center lines of a view with clicks on them (#1306).

## 0.0.138

Part Design adds the Shaft Design Wizard, Part adds Persistent Section Cut, Surface adds Blend Curve, Surface Cut and Gordon surfaces, and the Curves workbench gets eight commands. TechDraw adds Edit Line Appearance and Toggle Edge Visibility.

- `part.shaft` makes a stepped shaft from round segments, each with its own length, diameter and bore, as the Shaft Design Wizard does (#1290).
- Persistent Section Cut cuts bodies open along a plane (#1291).
- Blend Curve joins two edges with a smooth curve, Surface Cut shows one sheet cut by another, and a Gordon surface goes through a network of crossing curves (#1292, #1293, #1294).
- The Curves commands extend an edge, draw a curve through points, fit a curve near points, draw the iso curves of a face, join edges into one curve, split an edge, spread points along an edge and sweep profiles along a spine (#1295 to #1302).
- A drawing view has `line_formats` with the line type of ISO 128-20, the weight, the colour and visible of each line. Edit Line Appearance sets them on picked lines, and Toggle Edge Visibility shows the lines that are not visible. The task form shows the line, face and vertex numbers of a view as a text field (#1303).

## 0.0.137

TechDraw adds Toggle View Lock, Position Section View, Toggle View Frames, Redraw Page, Extend Line and Shorten Line.

- A drawing view, a detail and a section have `locked`, as LockPosition of TechDraw. Toggle View Lock locks or unlocks picked views, and Position Section View moves a section the shorter way into line with its view (#1286).
- Each view on the page tab has a frame with its name, its vertices and its lock. The frame shows on a hover, or for all the views with Toggle View Frames. Redraw Page builds the page again on the kernel (#1287).
- Extend Line and Shorten Line move the two ends of a picked cosmetic line along it, or change the extend of a picked center line, by 2 mm or by the length in the form (#1288).

## 0.0.136

TechDraw adds Hole/Shaft Fit, view rotation with the two Align commands, and the four Stack commands, with the results of FreeCAD on NIST FTC 10 and the SAE Notchback.

- Hole/Shaft Fit puts an ISO 286 fit after the text of a length dimension, with the tolerances of the fit, and a dimension has its own over and under tolerances (#1280).
- A drawing view has a `rotation` and an `align` list. Align Vertices/Edge Horizontally and Vertically turn a view so that a picked line or the line between two picked vertices goes across or up the page, with the 1130 lines, 1210 vertices and 54 aligns of FreeCAD on NIST FTC 10 (#1283).
- The views and the text annotations of a page have a `stack`, as the StackOrder of TechDraw. Stack Top, Stack Bottom, Stack Up and Stack Down move picked views over or under the others, and the page tab and the SVG file paint them in that order (#1284).

## 0.0.135

Part can make faces and points from an imported STL mesh. The FreeCAD profile shows both commands in the Part menu. TechDraw adds Area Dimension, and the geometry test gates cover more of the pinned OCCT suite.

- `shape_from_mesh` makes planar faces from STL triangles and can sew them into shells. `points_from_mesh` makes vertices at the mesh corners and appears under FreeCAD's menu and palette names (#1268, #1277, #1278).
- TechDraw Area Dimension records the area of selected faces of a view (#1275).
- The sketch solver status is available through the OCL language server, and the OCL to kernel layer gate follows the crate order (#1269, #1271).
- The point transform benchmark compares Forge and OCCT checksums on one million seeded points, then times the Forge batch API (#1270, #1273).
- The pinned OCCT ledger, 1,000-surface differential test, DRAW script parser and 7,295-case GTest list run in pull request CI (#1274, #1276).

## 0.0.134

TechDraw adds Area Annotation and Centerline Between 2 Faces, with the faces of a view as TechDraw finds them. Part adds box selection of faces through the model, and geometry matches more OCCT 8.0.1 transform behavior.

- A drawing view finds its faces as TechDraw does. Area Annotation shows the area of picked faces in a balloon, with the 8 balloons of FreeCAD on NIST FTC 10 (#1260).
- Centerline Between 2 Faces puts a center line through the middle of picked faces, with the 14 lines of FreeCAD on two views of NIST FTC 10 (#1266).
- Box selection picks faces through the model, as Part Box Selection does (#1261).
- `gp_Trsf` SetValues matches OCCT 8.0.1, and the `gp_Trsf2d` SetValues behavior is recorded (#1264, #1265).

## 0.0.133

Geometry adds OCCT 8.0.1 linear-form cases and runs more of the pinned differential suite on wasm32 with SIMD128. The UI parity ledger adds usage weights. GW8 remains open while its full parity and ledger checks are unfinished.

- The `gp_XYZ` and `gp_XY` linear forms match 101 OCCT cases, including cancellation inputs and poles from NIST FTC-10 curve 1748 (#1255).
- The per-commit wasm32-wasip1 SIMD128 job runs exact-bit fixtures for Precision, gp linear forms and compact B-spline knots (#1256).
- The same wasm job runs all 29 gp differential tests and 16 gp value tests with embedded OCCT 8.0.1 fixtures (#1257).
- The UI parity ledger records usage weights for all 2,980 rows from hashed model and macro inputs (#1258).

## 0.0.132

Geometry matches more OCCT 8.0.1 B-spline and Precision behavior while reducing derivative scratch allocations. Sketcher and TechDraw gain more FreeCAD commands, and the geometry dashboard gains archive integrity checks.

- The B-spline knot lookup matches the OCCT span at negative knot predecessors in the full 1,776-case fixture (#1242).
- B-spline curve and surface derivatives use stack scratch through degree 25 and order 3. Local allocation tests show 20,000 to 2,000 allocations for the curve calls and 81,400 to 800 for the surface calls (#1245, #1246).
- A public OCCT 8.0.1 probe checks all Precision entry points on 76 boundary cases (#1249).
- The geometry dashboard hashes archive rows, shows V8 and V9, and searches location rows (#1244).
- TechDraw adds Oblique Chain Dimension and Oblique Coordinate Dimension, and valued sketch distance calls create editable driving dimensions (#1247, #1248).

## 0.0.131

Sketcher adds line distance and midpoint constraints. Geometry work improves B-spline knot lookup and removes heap scratch from curve D0 evaluation.

- Sketcher accepts line distance constraints in OCL and adds named midpoint and concentric constraints (#1235, #1238).
- The desktop smoke probe reads the pump dimension from the model (#1239).
- `ocad-forge-math` adds compact BSplCLib span lookup and matches OCCT 8.0.1 periodic parameter bits in 1,776 seeded probe cases (#1236, #1240).
- B-spline curve D0 evaluation uses stack scratch for degrees up to 25 (#1237).

## 0.0.130

Sketcher can edit more imported FreeCAD parts and constrain more curved geometry. Geometry work adds PLib evaluation and borrowed B-spline knot conversion, with OCCT probe cases and local benchmarks.

- Sketcher opens sketches on planar faces and in part frames from imported FreeCAD files. External geometry projects STEP ellipse and B-spline edges into the editor (#1211, #1212, #1213, #1231).
- Sketcher adds B-spline drawing, curve picking, tangency, Carbon Copy links, curved dimensions, parallel line dimensions and point-on-curve constraints (#1198, #1201, #1217, #1220, #1222, #1233).
- `ocad-forge-math` adds polynomial, Bezier, cubic Hermite and rational derivative evaluation, plus borrowed knot sequence conversion for B-splines (#1218, #1221, #1225, #1232).
- The geometry benchmark records validity checks and measures resident bytes per result face. The current Apple M4 result is 4033 bytes per face on B-CUT-256, above the 2870 byte target; CPU frequency could not be verified on macOS (#1226, #1228).

## 0.0.129

Drawing pages take extent dimensions, and the TechDraw commands that put dimensions in rows. The model tree can freeze a feature, and the Part workbench has Point Pattern.

- `drawing_dimension` takes `extent`, a list of lines. Horizontal Extent Dimension and Vertical Extent Dimension are in TechDraw > Dimensions and in the list of dimensions on the TechDraw Dimensions toolbar. Clicks on lines fill the form, and with no lines the dimension spans the whole view (#1100).
- `drawing_dimension` takes `middle`, which puts the text at the middle of its two points. Align Chain Dimensions Horizontally and Vertically and Cascade Horizontal and Vertical Dimensions are in TechDraw > Attributes/Modifications. Clicks on dimensions fill the form, and the first dimension gives the place of the row (#1156).
- `feature.freeze` freezes or thaws features, as Std_ToggleFreeze of FreeCAD 1.1 does. A frozen feature keeps its result when the features before it change. The row menu of the tree has Freeze and Unfreeze, and Save refuses while a feature is frozen (#1154).
- `point_pattern` puts a copy of the seeds at each point of a list or at each lone point of a sketch. The Point Pattern command is `part.point_pattern` (#1155).
- `spec/compat/freecad` has the plan to test and prove each FreeCAD UI component in the desktop app (#1099).

## 0.0.128

Drawing pages take chain, coordinate and chamfer dimensions, and the text of a dimension takes the FormatSpec of TechDraw. The Part workbench has the Shape builder, Import Curve Network and the Attachment editor, and Part Design has Move Feature After.

- `drawing_dimension` takes `format`, the FormatSpec of TechDraw, with the tags `f`, `e`, `g`, `w` and `r`. Insert '⌀' Prefix, Insert '□' Prefix, Insert 'n×' Prefix, Remove Prefix, Increase Decimal Places and Decrease Decimal Places are in TechDraw > Format/Organize Dimensions and on the TechDraw Extend Dimensions toolbar. Customize Format Label is in TechDraw > Attributes/Modifications. A click on a dimension fills the form (#1094).
- `drawing_dimension` can be between two `vertices`, or be one dimension of a `chain` or a `coordinate` list, and `chamfer` puts the angle after the text. Horizontal and Vertical Chain Dimension, Horizontal and Vertical Coordinate Dimension and Horizontal and Vertical Chamfer Dimension are in TechDraw > Format/Organize Dimensions and in the list of dimensions on the TechDraw Dimensions toolbar. Clicks on vertices fill the form (#1097).
- The Shape builder (Part_Builder) is in the Part menu and on the Solids toolbar, with the OCL feature `shape_builder`. It makes an edge, a wire, a face, a shell or a solid as the FreeCAD task does (#1092).
- Import Curve Network (Part_ImportCurveNet) runs Import and reads a STEP, IGES or BREP file as FreeCAD 1.1 reads it (#1093).
- Move Feature After (PartDesign_MoveFeatureInTree) is the command `feature.move`. It moves features in the body and links the solid features again as FreeCAD does (#1095).
- The Attachment editor (Part_EditAttachment) is the command `part.attachment` in Part > Attachment. It changes the mode, the references, the offset and the flip of a primitive, a coordinate system or a datum (#1096).

## 0.0.127

Drawing pages take cosmetic threads, circle center lines and bolt circles. The Part workbench has Set Tolerance.

- `cosmetic_lines` takes `thread_bottom(edge, factor)` and `thread_side(first, second, factor)`, the threads of TechDraw. Cosmetic Thread Hole Bottom View, Cosmetic Thread Bolt Bottom View, Cosmetic Thread Hole Side View and Cosmetic Thread Bolt Side View are in TechDraw > Centerlines/Threading and in the threads button of the TechDraw Centerlines toolbar. A click on a circle or on two lines fills the form (#1089).
- `cosmetic_lines` takes `circle_center_lines(edge)` and `bolt_circle(edges)`, the lines of Circle Centerlines and Bolt Circle Centerlines of TechDraw. The two commands are in TechDraw > Centerlines/Threading and in the circle centerlines button of the TechDraw Centerlines toolbar. A line of a view can now have the style of a center line, and the lines of Circle Centerlines have it (#1090).
- Set Tolerance (Part_ToleranceSet) is in Part > Compound, with the OCL feature `set_tolerance(parts, min, max, refine)`. It limits the tolerances of a copy of each part as FreeCAD 1.1 does (#1087).
- The test of the Scaled step of a multi-transform now checks the volumes of FreeCAD 1.1 (#1088).

## 0.0.126

Drawing pages take parallel and perpendicular lines, and cosmetic circles and arcs. The Part workbench has the three Join tools.

- `cosmetic_lines` takes `line_parallel(edge, vertex)` and `line_perpendicular(edge, vertex)`, the lines that Cosmetic Parallel Line and Cosmetic Perpendicular Line of TechDraw put through a vertex along or square to a line. The two commands (TechDraw_ExtensionLineParallel and TechDraw_ExtensionLinePerpendicular) are in TechDraw > Centerlines/Threading and in the parallel and perpendicular button of the TechDraw Centerlines toolbar. A click on a line and a click on a vertex pick them, in either order (#1083).
- `cosmetic_lines` takes `circle`, `circle_arc`, `circle_two_points`, `circle_three_points` and `arc_center_points`, the circles and arcs of TechDraw. Cosmetic 1 Point Circle, Cosmetic 2 Point Circle, Cosmetic 3 Point Circle and Cosmetic Arc are in TechDraw > Centerlines/Threading and in the circles button of the TechDraw Centerlines toolbar. Clicks on vertices fill the forms, and Cosmetic 1 Point Circle can make an arc (#1085).
- Connect Shapes, Embed Shapes and Cutout Shape are in Part > Join and in the Join button, with the new OCL features `connect`, `embed` and `cutout`. They follow JoinAPI.py of FreeCAD 1.1 (#1084).

## 0.0.125

Drawing pages take center lines, intersection vertices and offset vertices. The Measure tool saves a free distance between two picked points, and the OCCT kernel converts Forge shapes.

- A view has `center_lines`, which come after its cosmetic lines and add no vertex, as in FreeCAD. Centerline Between 2 Points (TechDraw_2PointCenterLine) and Centerline Between 2 Lines (TechDraw_2LineCenterLine) are in TechDraw > Add Lines and on the TechDraw Annotation toolbar, with the extension, the shifts, the rotation and the mode of the FreeCAD task. Two clicks on vertices or lines pick them. The page tab and the SVG file draw the lines with a long dash and two dots (#1079).
- `cosmetic_vertices` takes `line_intersection(first, second)` and `vertex_offset(vertex, x, y)`, the points that Cosmetic Intersection Vertices and Offset Vertex of TechDraw put on a view. The two commands (TechDraw_ExtensionVertexAtIntersection and TechDraw_CommandAddOffsetVertex) are in TechDraw > Centerlines/Threading and in the vertex button of the TechDraw Centerlines toolbar. Two clicks on lines or one click on a vertex pick them (#1080).
- `measurement(of, mode = distance_free)` keeps the two picked points and saves the distance between them with `dx`, `dy` and `dz`, as Distance Free of FreeCAD (#1077).
- `ocad-occt` converts a Forge shape to an OCCT shape and back through the BinTools bytes, and the harness compares the two kernels on moved shapes (#1031).
- The Not yet lists of the web app and Part Design docs no longer name things that work (#1078).

## 0.0.124

Drawing pages take midpoint and quadrant vertices and cosmetic lines. The Measure tool saves the deltas of a distance, a center of mass, and the area of a whole body as FreeCAD does.

- `cosmetic_vertices` takes `line_midpoint(edge)` and `line_quadrant(edge, quarter)`, the points that Midpoint Vertices and Quadrant Vertices of TechDraw put on a line. The two commands (TechDraw_Midpoints and TechDraw_Quadrants) are in TechDraw > Add Vertices and in the list of vertices on the TechDraw Annotation toolbar. A click on a line picks it (#1072).
- A view has `cosmetic_lines`, which come after its lines and add no vertex, as in FreeCAD. Cosmetic Line Through 2 Points (TechDraw_2PointCosmeticLine) is in TechDraw > Add Lines and on the TechDraw Annotation toolbar. Two clicks on the vertices of a view pick the ends. The page tab and the SVG file draw the lines dashed (#1075).
- A saved distance gives `dx`, `dy` and `dz`, as Show Delta of FreeCAD (#1071).
- `measurement(of, mode = center_of_mass)` saves the center of mass of a face, an edge or a body, as the Center of mass measurement of FreeCAD (#1073).
- `measurement(body, mode = area)` gives the area of all the faces of a whole body, as the Measure tool of FreeCAD (#1074).

## 0.0.123

Drawing pages take angles of three points and cosmetic vertices. Part Import and Part Export read and write STEP, IGES and BREP as FreeCAD does, and FreeCAD files open with their refines.

- `drawing_dimension` of type `angle3pt` gives the angle at a vertex of a view between two other vertices, as TechDraw calculates it. Angle Dimension From 3 Points (TechDraw_3PtAngleDimension) is in TechDraw > Dimensions and in the list of dimensions on the TechDraw Dimensions toolbar. Three clicks on the dots of the vertices pick them (#1067).
- A view has `cosmetic_vertices`, which come after its other vertices as in FreeCAD. Cosmetic Vertex (TechDraw_CosmeticVertex) is in TechDraw > Add Vertices and in the list of vertices on the TechDraw Annotation toolbar. A click in the area of a view picks the point (#1069).
- `import_iges` and `import_brep` read IGES and BREP files, and `POST /export` of `opencadd serve` writes STEP, IGES and BREP as `Part.export` of FreeCAD writes them. The web app imports and exports these files through the kernel (#1066).
- A FreeCAD file opens a Part Refine, the Refine Shape Feature of OpenSCAD and a Part boolean with Refine set as `refine` (#1068).

## 0.0.122

Drawing pages take projection groups and angle dimensions. Check Geometry runs the boolean operation check, and Boolean Fragments takes the modes and the tolerance of FreeCAD.

- `drawing_projection_group` puts a front view and the views that turn from it on a page, in first or third angle, as the Projection Group of TechDraw. Each view is a `drawing_projection`. Projection Group (TechDraw_ProjectionGroup) is in TechDraw > TechDraw Views and in the palette (#1062).
- `drawing_dimension` of type `angle` gives the angle between two straight lines of a view, as TechDraw calculates it. Angle Dimension (TechDraw_AngleDimension) is in TechDraw > Dimensions and in the list of dimensions on the TechDraw Dimensions toolbar. Two clicks on the page pick the two lines, and the page tab draws the arc (#1064).
- Check Geometry has the settings of the FreeCAD panel, with the boolean operation check and its nine checks. `POST /check` takes the checks to run (#1061).
- `boolean_fragments` takes the standard, split and compsolid modes and the tolerance of FreeCAD's Boolean Fragments, and Boolean XOR takes the tolerance (#1063).

## 0.0.121

Drawing pages take text and balloon annotations. Offset and Project on surface take the last options of FreeCAD. Forge projects a point on a curve with the curve ends as candidates, takes a start hint, and searches the boundary of a partial elementary surface.

- `drawing_annotation` puts a block of text on a drawing page, as the Text Annotation of TechDraw. Text Annotation (TechDraw_Annotation) is in TechDraw > Annotations and on a new TechDraw Annotation toolbar. The page tab and the SVG file draw it (#1057).
- `drawing_balloon` puts a balloon with a line and an arrow on a view, a detail or a section, as the Balloon Annotation of TechDraw. Balloon Annotation (TechDraw_Balloon) gives each new balloon the next number of its page. The page tab and the SVG file draw it (#1056).
- `offset` takes the skin, pipe and recto verso modes, the intersection switch and the fill of FreeCAD's Offset (#1055).
- `project_on` lands faces as well as edges, and takes the mode, the height and the offset of FreeCAD's Projection on surface. The default stays `edges`, so models do not change (#1058).
- `ocad-forge-query` has the endpoint form of point to curve extrema, from black box runs of OCCT, `ParamHint` and `nearest_from` that give the same result as `nearest` for any hint, and a search of the sides and corners for `nearest` on a partial plane, cylinder, cone, sphere or torus. Spec 08 has the spec problems that #985 found (#1059).

## 0.0.120

Drawing pages take section views, and the cut faces are hatched as in TechDraw. Sketches project B-spline edges and intersect faces of a body.

- `drawing_section` cuts the body of a view with a plane, as a view of its own, with the lines that TechDraw gives. Section View (TechDraw_SectionView) is in TechDraw > TechDraw Views, on the TechDraw Views toolbar and in the palette. The page tab and the SVG file show the section line on the view with an arrow and a letter at each end, and the caption under the section (#1051).
- A section hatches the faces that its plane cuts with the default hatch of TechDraw, in the page tab and in the SVG file. On the SAE Notchback, the faces and their areas are those of FreeCAD (#1053).
- External geometry projects and intersects B-spline edges (#1050), and intersects faces of a body with the sketch plane (#1052).

## 0.0.119

Drawing pages take detail views, and the TechDraw page of the SAE Notchback has its own journey. Sketches attach by every attachment mode of FreeCAD, and project slanted circles and ellipses.

- `drawing_detail` shows the part of a view in a circle bigger, as a view of its own, with the lines that TechDraw gives. Detail View (TechDraw_DetailView) is in TechDraw > TechDraw Views, on the TechDraw Views toolbar and in the palette. The page tab and the SVG file show the circle on the view with its letter, and the caption under the detail (#1048).
- Journey J-FC-10 makes a TechDraw page of the SAE Notchback with two views and three dimensions, and checks it against a FreeCAD twin (#1043). Its dimensions are timed as A-DRW-DIM, in the budget of 50 ms (#1044).
- A sketch attaches by any FreeCAD attachment mode (#1042), and a sketch that is attached to itself is refused (#1046).
- External geometry projects a circle or an arc at a slant as an ellipse, and an arc edge on as a line (#1045). It also projects and intersects ellipse edges (#1047).

## 0.0.118

Drawing pages take dimensions and export to SVG. Datums take the last attachment modes of FreeCAD and take datum lines and the origin axes as references.

- `drawing_dimension` puts a distance, a horizontal or vertical distance, a radius or a diameter on one line of a view, with the value and the text that TechDraw gives. The five TechDraw dimension commands are in TechDraw > Dimensions, on the TechDraw Dimensions toolbar and in the palette, with Shift+H and Shift+V. While the form is open, a click on a line of a view picks it (#1037).
- The page tab draws each dimension with its dimension line, its arrows and its text (#1037).
- Export Page as SVG (TechDraw_ExportPageSVG) saves the page in its tab as an SVG file in mm, with the frame, the title block, the views and the dimensions (#1040).
- Datums attach with Intersection line and Parallel plane (#1035), and with Midpoint and Folding (#1036).
- A datum line and the origin axes X, Y and Z can be references of an attachment (#1038). A datum line keeps the X axis of its frame, so the modes that read the axes of an object take it (#1039).

## 0.0.117

The web app shows the drawing pages. New Page and New View work from the TechDraw menu, the toolbars and the palette, and each page opens in a tab with its views. Datums take more attachment modes of FreeCAD: on a face, by inertia, by proximity and on conics.

- New Page (TechDraw_PageDefault) and New View (TechDraw_View) make a drawing page and a view of a body on it. The FreeCAD profile has the TechDraw toolset with the menus and toolbars of the FreeCAD TechDraw workbench (#1033).
- Each drawing page shows in a tab along the bottom of the window, with the sheet, the frame, the title block and the lines of its views. A hidden line is dashed. The kernel of `opencadd serve` draws the views (#1033).
- Datum planes and lines attach Tangent to surface and Normal to surface, and a datum on an edge takes the Path parameter of FreeCAD (#1029).
- Datums attach by inertia, with Inertial CS and the three inertia axes, and by proximity, with the proximity line and points (#1030).
- Datums attach to a focus, a directrix, an asymptote or the vertex of a conic edge (#1032).

Known issues: The desktop app does not show the drawing pages yet. A view has no dimensions yet, and a page does not export to SVG yet. The Measure tool gives no value for Distance Free, Position and Center of mass, and Quick Measure and the Measure tool do not pick a vertex yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.116

The first TechDraw views: a drawing page and the views of a body on it, with hidden lines, as FreeCAD 1.1 draws them. A saved measurement stays in the document. The Hole command takes all the thread types and screw heads of FreeCAD, and datums attach along an edge.

- New OCL features `drawing_page` and `drawing_view`. A view is a body seen along a direction, with the hidden lines removed, as a TechDraw view makes it. On four views of the SAE Notchback body, the lines are the same as in FreeCAD 1.1 to 1e-9 mm. The app does not show the pages yet (#1027).
- Save in the Measure panel keeps the measurement in the document as the new `measurement` feature, as FreeCAD keeps a measurement object. The command is `measure.tool` (#1024).
- A saved Distance of two elements that are not two circles is the shortest distance between them, as in FreeCAD (#1025).
- The Hole command takes the UTS, pipe, Whitworth and tyre valve threads of FreeCAD, with their clearance holes, tap drills and classes (#1021).
- The Hole command cuts the screw heads that the FreeCAD Hole lists, such as ISO 4762, DIN 7984 and ISO 2009 (#1022).
- A hole with no thread gets the head cut of the FreeCAD rule of thumb when no values are given (#1023).
- Datum planes, lines and points attach along an edge with the twelve path modes of FreeCAD, such as Normal to edge, Frenet NB and Center of curvature (#1026).

## 0.0.115

The Measure panel has the Measure tool of FreeCAD 1.1, and the Run Simulation of an assembly plays in the 3D view. The Hole command makes a modeled thread, also in a tapered hole, and a hole can drill at any mix of points, full circles and arcs. Forge computes mass properties with error control.

- The Measure tool, Std_Measure, opens in the Measure panel from Tools > Measure, the View toolbar and the palette. The Mode list shows the mode that Auto takes, and the Result shows the Length, Area, Radius, Angle or the Distance of two circles, the same as FreeCAD 1.1.4. Save keeps the measurement in a list in the panel (#1019).
- Run Simulation opens a player over the 3D view, with the controls of the FreeCAD Simulation panel. It moves the parts of the assembly frame by frame and does not change the document (#1016).
- The Hole command takes Modeled thread as the hole type, with the thread class, the clearance, the thread depth type and the hand of the FreeCAD Hole panel (#1017).
- A modeled thread in a tapered hole now builds, on a cone at the angle of the wall, as FreeCAD does. The FCStd reader brings it in (#1015).
- `hole` takes `full_circles`, `arcs`, `points_and_full_circles` and `points_and_arcs` for `on`, so the FCStd reader brings in each mix of the FreeCAD Hole (#1018).
- Forge computes the mass, the center of mass and the inertia of curves, faces and solids, with a bound on the error (#1013).

Known issues: Save of the Measure tool does not put the measurement in the document yet. The Measure tool gives no value for Distance Free, Position, Center of mass and the Distance of pairs that are not two circles. The edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure and the Measure tool do not pick a vertex yet. A joint frame comes from its placement field, and a pick on a face or an edge does not give it yet. Replace Component does not solve the assembly, so run Solve Assembly after it. An exploded view does not show the lines from where each part was to where it is. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.114

An assembly can have a simulation that moves its joints in time, with the frames of FreeCAD, and Export ASMT File writes an assembly as FreeCAD writes it. A loft can end in a point and go through sections with holes, and a Part Design hole can cut its modeled thread. The desktop app opens the DrivAerML car.

- The new command Simulation (`asm.simulation`, key V) makes a simulation of an assembly with a start, an end, a step and motions. A motion turns or slides a Revolute, Slider or Cylindrical joint by a formula in `time`, read as OndselSolver reads it. Run Simulation solves the assembly at each time and gives the placement of each part, the same as FreeCAD to 1e-6. Add Motion adds a motion (#1012).
- The new command Export ASMT File (`asm.export_asmt`) saves an assembly as the ASMT file of OndselSolver, with the same lines and numbers as FreeCAD (#1008).
- A loft can start or end in a point, as the FreeCAD Loft does with a vertex (#1005).
- A loft or a sweep through sections with holes now builds, so a ring profile gives a tube (#1007).
- `hole` of Part Design cuts a modeled thread into the bore, as FreeCAD does when Model thread is set. The FCStd reader brings in the thread values (#1011).
- The FCStd reader brings in Part Loft and the Additive and Subtractive Loft of Part Design (#1006).
- A `.brep` shape goes into a Forge shape and comes back with no loss, and Forge has periodic B-spline curves and surfaces (#1009).
- The desktop app opens DrivAerML run 1 as 49 named parts. A click selects a part and its row in the tree, and OCL code has syntax colors (#1010).

Known issues: the web app does not show the frames of a simulation in the 3D view yet. The edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. A joint frame comes from its placement field, and a pick on a face or an edge does not give it yet. Replace Component does not solve the assembly, so run Solve Assembly after it. An exploded view does not show the lines from where each part was to where it is. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.113

An assembly can make a bill of materials and an exploded view, as the FreeCAD Assembly workbench does. A sweep can end in a point and can hold its profile in each mode of the FreeCAD Pipe.

- The new command Bill of Materials (`asm.bom`, key O) makes a sheet that lists the bodies of the parts of an assembly, with the columns of FreeCAD. The sheet follows the assembly when you add, remove or replace a part, and the text that you type in it stays with the body of its row (#1001).
- The new command Exploded View (`asm.explode`, key E) moves parts of an assembly apart in steps, along X, Y and Z or away from the middle, with the formula of FreeCAD. Explosion Step adds a step. The view does not change the placements of the parts, and the parts show at their own places again when you hide the view (#1003).
- A section of `sweep` and `sweep_surface` can be a point at the start or the end, so the sweep closes in a tip, as Part Sweep of FreeCAD does with a vertex (#1000).
- `sweep` takes `fixed`, `binormal`, `auxiliary` and `curvilinear`, the Fixed, Binormal and Auxiliary modes of the FreeCAD Pipe. The FCStd reader brings in each Pipe mode (#1002).

Known issues: the edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. A joint frame comes from its placement field, and a pick on a face or an edge does not give it yet. Replace Component does not solve the assembly, so run Solve Assembly after it. An exploded view does not show the lines from where each part was to where it is. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.112

A part of an assembly can take another body, and a change of a joint parameter solves the assembly. Two new journeys drive a slider and crank and replace the body of a part, and each one equals FreeCAD. Part Sweep takes the sections, the Solid, Frenet and Transition options of FreeCAD and any edges as its path, and the FCStd reader brings in Part sweeps and Part Design pipes. The geometry archive holds each curve and surface kind.

- The new command Replace Component puts another body in the place of a part of an assembly. As in FreeCAD, you set the LinkedObject of the part in the Data tab, and the row is now a list of the bodies before the part. The part keeps its name, its placement and its joints. J-ASM-08 replaces the body of a part that a Revolute joint holds, and the joint still solves (#998).
- A change of a frame, an offset, a distance, an angle or the between of a joint solves the assembly again, as FreeCAD does when a property of a joint changes. J-ASM-05 drives a slider and crank of three WindsorML bodies from the Distance of a joint in the Data tab (#995).
- `sweep` takes `sections`, `solid`, `frenet` and `transition`, as Part Sweep of FreeCAD does, and `sweep_surface` takes `sections`, `frenet` and `transition` (#994). The path of a sweep can be a wire, such as a helix, or edges of a body (#997).
- The FCStd reader brings in Part Sweep with its sections, Frenet and Transition, and the Additive and Subtractive Pipe of Part Design (#996).
- The geometry archive of `ocad-forge-topo` holds each curve and surface kind, at geometry version 2, and `ShapeView` walks an archived shape in place with no copy (#993).

Known issues: the edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. A joint frame comes from its placement field, and a pick on a face or an edge does not give it yet. Replace Component does not solve the assembly, so run Solve Assembly after it. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.111

Every Part Design primitive takes all the values of its FreeCAD form, and the FCStd reader brings in each round primitive, the wedge and the prism. The joint form has a placement field for the frames of a joint, and two new journeys make assemblies: each joint type once on boxes, and three WindsorML bodies with three joints. After a command from a menu or a toolbar, the keys go to the model again. The geometry kernel reads and writes the OCCT `.brep` formats and projects points on curves and surfaces.

- The joint form has a placement field for `frame_a`, `frame_b`, `offset_a` and `offset_b`, with a box for X, Y and Z and for each turn. The FreeCAD profile gives I to Component, T to Gears Joint and L to Belt Joint in the Assembly toolset. J-ASM-01 makes each of the 13 joint types of FreeCAD once, and its FreeCAD twin gives the same placement for each part (#982).
- J-FC-09 puts three WindsorML bodies of the corpus in an assembly with a grounded joint, a Fixed joint and a Revolute joint, and compares the result with FreeCAD. After a command from a menu or a toolbar, the viewport gets the focus again, as the 3D view does in FreeCAD, so the keys of the toolset work. When A waits for A, C and no key comes, A runs New Assembly in one render (#991).
- Cylinder and Cone take the FreeCAD Angle, and Sphere, Torus and Ellipsoid take Angle1, Angle2 and Angle3, so a part of a round primitive builds as in FreeCAD (#984). Wedge takes all ten values of FreeCAD (#987). Prism leans by the X Skew and the Y Skew of FreeCAD (#989).
- The FCStd reader brings in the Part Design cone, each torus and each ellipsoid (#990), and the wedges and prisms of Part and Part Design (#987, #989).
- `ocad-forge-topo` reads and writes the `.brep` text and `BinTools` binary formats of OCCT, and a write in the form that the read found gives the same bytes (#988). It reads a shape of its native format with no copy when the bytes are aligned (#986).
- `ocad-forge-query` projects a point on a curve or a surface, inverts a point to its parameters, and gives the fast and the tight box of a curve or a surface (#985).

Known issues: the edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. A joint frame comes from its placement field, and a pick on a face or an edge does not give it yet. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.110

The Assembly toolset is in the FreeCAD profile, with the 17 commands of the FreeCAD Assembly workbench, its menu, toolbars and keys. The assembly solve splits an assembly into rigid groups and clusters, and it names the fewest joints in conflict. Part has more of the FreeCAD mirror, pattern and helix options and Convert to solid. The sketcher trims every sort of curve. The geometry kernel has the elementary forms and the conversions to B-splines, dual numbers and the B-rep data model.

- The FreeCAD profile has the Assembly toolset, with the Assembly menu, the Assembly and Assembly Joints toolbars and the keys of the FreeCAD 1.1.3 workbench. New icons show the joints (#976).
- The core has the Assembly commands: New Assembly, Component, Toggle Grounded, Solve Assembly and one command for each of the 13 joints. A joint command adds the joint, moves the part and solves the assembly in one undo step. When the joints cannot all hold, no part moves, as in FreeCAD (#973).
- `ocad-asm` puts the parts of a Fixed tree into one rigid group and solves each cluster of joints alone (#967). When a cluster cannot hold, the report gives the fewest joints that cannot hold together (#969).
- Mirrored and the Linear and Polar Patterns take a datum, an edge, a face or a line of a sketch, as FreeCAD lets you pick them (#970, #971). Part Mirror takes a base point and a normal, or a circle edge (#979).
- Helix has the four modes of FreeCAD, a cone angle, a growth, Reversed and an axis from a sketch, a datum line or an edge (#972).
- `make_solid` closes a set of sewn sheets into a solid, as Convert to solid of FreeCAD does (#975).
- Trim Edge takes ellipses, arcs of conics and B-splines, as Sketcher_Trimming of FreeCAD does (#968).
- `ocad-forge-math` has the elementary curves and surfaces of ElCLib, ElSLib and CSLib, and the conversions of conics, surfaces, joined Bezier curves and polynomials to B-splines, with tests against OCCT 8.0.1 (#958). It has dual numbers, and the B-spline kernel is generic over them (#974). The spec gives the rules that these tests found (#978).
- `ocad-forge-topo` has the B-rep data model: flat arrays, locations and orientations with the OCCT rules, explorers and maps that do not allocate, history, a content hash and a native format. The 262 face plate uses 251.8 bytes per face (#977).

Known issues: the edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT. The native format of `ocad-forge-topo` copies the data when it reads a shape (#827).

## 0.0.109

The assembly solver is in `ocad-asm`. It has all 13 joints of FreeCAD 1.1, drives, and the snap that puts a part on a new joint as FreeCAD does. The journey J-MS-01 measures each sort of selection that the web app can pick, and it compares the readings with FreeCAD. The sketcher splits every sort of curve and rounds where a line meets an arc.

- `ocad-asm` places parts with joints. It has the Fixed, Revolute, Cylindrical, Slider, Ball, Parallel, Perpendicular and Angle joints, and the four sorts of the Distance joint that FreeCAD makes from the geometry that you pick. A drive sets the angle of a Revolute joint or the distance of a Slider or a Cylindrical joint. After a solve, the report gives the free motions, the joints that repeat other joints and the joints that do not hold. A four bar linkage closes to 1e-9 mm at each crank angle of the test (#964).
- The solver has the Rack and Pinion, Screw, Gears and Belt couplings. A coupling counts the full turns of its parts, so a gear train turns past a half turn. A new joint moves the part that the other joints do not hold, with the parts joined to it, as `matchJCS` of FreeCAD does (#965).
- J-MS-01 opens a body of the corpus and measures two planes, two cylinders, a circle and a cylinder, and more pairs with Std_Measure. It compares each reading with the FreeCAD twin. When an element is not in the view, the journey turns the view with the standard view keys until it shows (#961).
- Split takes ellipses, arcs of conics and B-splines, as Sketcher_Split of FreeCAD does (#960). Fillet rounds where a line meets an arc or two arcs meet (#957). A point can be on an ellipse or on an arc of a conic (#963).
- `ocad-forge-geom` has the surface kinds, their views and properties, and the tolerant hashers of curves and surfaces (#956, #962). xtask has the GTest case list and the conformance dashboard of Forge (#959).

Known issues: the assembly has no commands, toolbar or panel in the web app yet. The edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.108

Quick Measure is in the status bar. A click on a face or an edge of an imported body selects it, and the status bar shows the length, the area, the radius, the distance or the angle, as FreeCAD shows them. The measure uses the exact curves and surfaces and not the mesh, and the journey J-FC-08 compares each reading with FreeCAD on the SAE notchback of the corpus. The sketcher has more of the FreeCAD tool options, and the Spreadsheet panel opens a sheet of 100,000 cells in its budgets.

- Quick Measure shows in the status bar within 50 ms of the selection. A click on a STEP body selects a face or an edge, and Ctrl+click adds one more in the FreeCAD profile. The status bar gives the length of an edge, the area of a face, the radius of a circle or a cylinder, and the distance and the angle of two edges (#952).
- J-FC-08 opens the SAE notchback, picks a cylinder face, two circles and two lines, and checks the text and the numbers of Quick Measure against the FreeCAD twin, to 1e-6. It checks the time of A-MEAS-QUICK. The harness gives `quickMeasure()`, and the I12 oracle names the face or the edge of a selection (#954).
- `ocad-io` measures the exact length of an edge and the exact area of a face. On a curved face the mesh gives up to 1.5 percent less area, so the measure goes on the surface (#942).
- The Spreadsheet panel reads a cell only when the grid draws it. A sheet of 100,000 cells opens in less than 100 ms and shows an edit in less than 16 ms (#940).
- A loft has the Closed property of FreeCAD, and the Loft command has Solid and Closed (#943). The 2D offset has the Fill, Mode, Join and Intersection of FreeCAD, and offsets the curves of a sketch (#944).
- The sketcher keeps external geometry when you delete all the geometry (#945). Rotate has copies and the equal option, and Scale has the keep option (#947). Symmetry has the keep and symmetric options (#948). Rectangle has the 3 corners mode and the frame (#949), and the oblong has the frame (#950). The M key changes the polyline between its line and arc modes (#953).
- A test checks that Align View to Sketch looks at the sketch plane as FreeCAD does (#946). The spec gives the sketch command ids of three FreeCAD rows (#951).
- `ocad-forge-math` has root isolation in the Bernstein basis, Bezier clipping and the Krawczyk test, which prove each root that they find (#941).

Known issues: the edge numbers of the web app are not the FreeCAD edge numbers on a body with seam or degenerate edges. Quick Measure does not pick a vertex yet. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.107

The Spreadsheet workbench is complete at the top level of the UI coverage: each of its 15 FreeCAD commands runs from its menu path, its toolbar button, its key and the palette, and the journey J-TBL-01 runs them on the vehicle table of the corpus. Export Spreadsheet is a core command and writes the file that FreeCAD writes, byte for byte. The panels, the status bar, the tabs and the context menus of the web app had a sweep. Forge has a differential harness and the box, tree and mesh modules of its math crate.

- J-TBL-01 opens a sheet whose aliases drive 50 boxes, edits a cell in the budget of A-TBL-EDIT, formats the sheet from the keys, the toolbar, the menu and the palette, and imports and exports the corpus table `synthetic-vehicle-1k.csv`. The text of a colored cell is black or white, by the contrast with its color (#934).
- `sheet.export` is the core command of Export Spreadsheet. It writes the value of each cell as FreeCAD writes it: a whole number with no unit as an integer, other numbers with 6 digits, a length in mm and an angle in degrees. A command that only reads the document is now a query, which makes no undo step (#936).
- The 15 Spreadsheet commands are at L4, and the palette finds Set Alias by its FreeCAD name (#937).
- A sweep clicks each rail button, panel menu, status bar menu, document tab, context menu, the palette and three dialogs in a real browser. The 3D view has a context menu, from a right click, the Menu key or Shift+F10. The tab of the model has no close button. The palette gives the focus back when it closes. The status bar says what a Navigation, Units or Keys item changed. Rail buttons of panels that are empty are gone (#933).
- `ocad-forge-diff` is the differential harness of Forge, with comparators for evaluation, extrema and constructions, verdicts and reproducers (#932).
- `ocad-forge-math` has the boxes, the oriented box, the 4-wide bounding volume tree with its builds and queries, the k-d tree and the mesh numerics (#935).

Known issues: the 3D view shows the faces and edges of an imported body only. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.106

The Spreadsheet workbench has all the commands of the FreeCAD Spreadsheet toolbar: the format commands and the import and export of CSV files, which run on the picked cells from the toolbar, the menu, the keys and the palette. A Part boolean takes the Refine of FreeCAD and more than two bodies. Forge has all the curve kinds in 3D and 2D, a reader for the OCCT DRAW test scripts and a benchmark harness.

- The 13 Spreadsheet format commands of FreeCAD are in the core: the six alignments, bold, italic and underline, the color of the text and of the cell, Merge Cells and Split Cell (#924).
- Shift+click or Shift+arrow picks a range of cells in the sheet panel. The format commands run on the picked cells from the Spreadsheet toolbar, the Spreadsheet menu, Ctrl+B, Ctrl+I and Ctrl+U, and the palette. The panel shows the format of each cell (#927).
- Import Spreadsheet (`sheet.import`) makes a sheet from a CSV file with commas, semicolons or tabs. Export Spreadsheet saves the sheet of the panel as a CSV file with the values that the cells show (#930).
- `boolean` in OCL takes `refine = true`, which merges the faces of the result that lie on one surface, as the Refine property of a FreeCAD boolean does. The command `part.boolean` takes `refine`, and `tools` for a fuse or a common of more than two bodies (#925).
- `ocad-forge-geom` has the line, the conics, the Bezier and B-spline curves, the trimmed curve and the offset curve in 3D and in 2D, with derivatives to any order and differential tests against OCCT 8.0.1 (#928). The AdjustPeriodic behavior note is fixed (#929).
- `ocad-forge-draw` parses the Tcl of the OCCT DRAW test scripts and reads the OCCT test tree, with the command table and the skip list of the `geometry` and `lowalgos` groups (#923).
- `ocad-forge-bench` runs Forge and OCCT in interleaved pairs of processes and reports the ratio of medians with a bootstrap interval, and `ocad-alloc-count` counts the heap (#926).

Known issues: the 3D view shows the faces and edges of an imported body only. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails (#917). The release Linux binaries do not include OCCT.

## 0.0.105

The FreeCAD profile has the FreeCAD menu bar with its submenus, and icons on the toolbars, the menus and the panel buttons. Part has Compound Filter and Explode Compound. Shell, draft and defeature can pick a face of an imported body by a point on it. Forge, the Rust geometry kernel, has the numerics of `ocad-forge-math`: the precision constants, the `gp` value types, the exact predicates, the solvers, the polynomial roots, the linear algebra and the quadrature. It also has the first behavior notes and the clean room check.

- Shell, draft and defeature read their faces through the same path as the sub-shape binder, so `body.faces.at(x, y, z)` picks a face of an imported body that no sketch names (#911).
- `compound_filter` (command `shape.filter`) is the Compound Filter of FreeCAD. It keeps parts of a compound by place, by volume, area, length or distance in a window, or by contact with a stencil, and `invert` keeps the other parts (#918).
- `shape.explode` is the Explode Compound of FreeCAD. It adds one `part_of` feature for each part of a compound in one undo step, with the FreeCAD names `<name>_child0`, `<name>_child1` and so on (#920).
- The menu bar of the FreeCAD profile has the FreeCAD menus in the FreeCAD order, with their submenus and separators, and it works with the keyboard. The standard commands of FreeCAD, such as Save a Copy, Print, the view rotations, Zoom, Fullscreen and About OpenCAD, now run. A harness clicks each menu and toolbar item in a real browser (#914).
- Each toolbar button, menu item and panel button shows the icon of its FreeCAD command. The name and the reason a command cannot run are in the tooltip and the accessible name. The toolset switch is at the start of the toolbar row, where FreeCAD has its workbench switch (#916).
- Rendering Order of the Sketcher has a test for each of its three orders (#913).
- `ocad-forge-math` has the exact predicates of Shewchuk, expansion sums and intervals (#908), the OCCT 8.0.1 precision constants and the `gp` value types (#910), and the solvers, polynomial roots, linear algebra, quadrature and seeded random streams (#915). Differential tests compare each function with OCCT 8.0.1. The tests found OCCT defects in `MathPoly::Laguerre`, `MathPoly::Cubic`, the trigonometric root finders and `MathLin::SVD`, and #824 lists them.
- `ocad-forge-geom` follows the OCCT 8.0.1 rules for closed and periodic curves and surfaces, which a probe measured (#912).
- The OCCT ledger accounts for the items that OCCT `master` removed or added after 8.0.1 (#909). It has 90 new rows for the OCCT 7.8.1 to 8.0.1 changes that a FreeCAD user can see, each with the behavior that OpenCAD gives (#919).
- `spec/geometry/behavior/` has the template and the first 10 behavior notes, for `gp`, `ElCLib` and `BSplCLib`. `cargo xtask clean-room` fails when a Forge file names an OCCT source file or has the OCCT license header, and CI runs it (#921).

Known issues: the 3D view shows the faces and edges of an imported body only. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The aarch64 Linux binary is not in the release, because the cross linker fails. The release Linux binaries do not include OCCT.

## 0.0.104

A Pad or Pocket can go up to a plane at an angle to the sketch, and up to the last or the first face that a line from the profile meets. The Sub-Shape Binder of FreeCAD is in Part Design, and journey J-PD-06 shows binders across bodies that follow a change in the source body. The FCStd reader brings in more of the FreeCAD corpus. Forge, the Rust geometry kernel, has its crate skeletons, its first NURBS crates and the OCCT 8 ledger gate.

- `up_to` takes a flat face or a datum plane at an angle to the sketch. The slanted face is the `end_cap` (#890). The FCStd reader writes a Pad or Pocket of Type Up to face to a datum plane as `up_to` that plane (#891).
- The new extents `up_to_last` and `up_to_first` go to the last or the first face of the body that a line from the middle of the profile crosses, as in FreeCAD. The FCStd reader writes them for a Pad of Type Up to last or Up to first and for a Pocket of Type Up to first. Pad002 of pad-test now has the FreeCAD volume (#897).
- The Sub-Shape Binder (`part.sub_shape_binder`, OCL `sub_shape_binder`) copies bodies, or faces and edges of one body, and follows them. It has Fuse, Make Face, Refine and a 2D offset with the joins of FreeCAD (#893).
- Journey J-PD-06 binds two faces of a block, pads a cap on the top binder, and changes the Length of the block. The binders and the cap follow the change, as in FreeCAD 1.1.4. New Sketch can go on a binder of one flat face, and `cargo xtask journey` compares each body in the `bodies` map of `golden.json` with the FreeCAD twin (#895).
- `project(...)` in a sketch takes curves and points of another sketch. The FCStd reader brings in external geometry on edges of a feature (#894) and on edges of another sketch, as Carbon Copy makes (#901).
- The FCStd reader brings in an Angle at given ends of two lines, an Angle of one line to the axis, and Horizontal and Vertical between two points. Nine sketches of the corpus go from partial to parametric (#892).
- A sketch line of no length stays in the sketch with its constraints, and the kernel leaves it out of the region, as FreeCAD does (#899).
- A FreeCAD Part Cut, Fuse or Common on a whole body takes the Tip of the body. The four cuts of test-geomop now come in with the FreeCAD volumes (#902).
- A FreeCAD Fillet or Chamfer edge between faces with no name is picked by its middle point, so the dress-up comes in and is not dropped (#903).
- An `App::InventorObject` gets a note that says it is a scene for display only (#905).
- Forge has 21 crate skeletons in the order of the geometry spec. `cargo xtask layers` holds them to the crate order and to the third party rule GD17, and `cargo xtask forge-wasm` builds them for wasm32 (#898). The NURBS code moves into `ocad-forge-math` and `ocad-forge-geom`, with no change in behavior (#900).
- `spec/geometry/ledger.toml` accounts for each of the 3735 public items of the 184 OCCT 8.0.1 packages in scope, and `cargo xtask geom-ledger` checks it (#904). `cargo xtask geom-ledger track` and the monthly workflow `geom-track` follow OCCT `master` and write `spec/geometry/ledger-8.1.md` (#906).

Known issues: the 3D view shows the faces and edges of an imported body only. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. A draft up to a plane at an angle to the sketch is not supported yet. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. The aarch64 Linux binary is not in the release, because the cross linker fails. The release Linux binaries do not include OCCT.

## 0.0.103

New Sketch picks a flat face of any body in the 3D view, and a sketch on a face now has the origin and axes of the FlatFace mode of FreeCAD. Journey J-PD-03 pads a square on the roof of an imported Windsor body, and journey J-PD-12 edits a dimension deep in a 300-feature history. The FCStd reader builds more of the FreeCAD files of the corpus.

- The new selector `body.faces.at(x, y, z)` picks the face of a body that the point is on. New Sketch on Face (`sketch.new_on`) takes the point in its new field At, so a sketch can go on a face of an imported STEP body (#880).
- While the New Sketch on Face panel is open, the faces of an imported body show over the 3D view. A click on a flat face fills At. A sketch on a flat face has its origin at the point of the face's plane nearest to the model origin, and its X axis comes from the model axis nearest to the normal of the face, as in FreeCAD. Before, the origin was the middle of the face, so a sketch on a face in an older OCL file can move. A hole on a face keeps its old frame. Journey J-PD-03 drops the Windsor body, clicks the roof, draws a square of 100 mm and pads it 20 mm. OCCT gives the volume, area, centroid and topology of the FreeCAD 1.1.4 twin (#888).
- Journey J-FC-06 picks two edges of the Windsor body for a fillet with preview, cancels, makes the fillet with Apply and cuts back an edge with a chamfer. OCCT gives the values of the FreeCAD 1.1.3 twin (#814).
- Journey J-PD-12 changes the Length of pocket9 in `synthetic-history-300.FCStd` in the property view. The ids and names of the features stay the same, the OCL changes on one line, and the body at the end matches the FreeCAD 1.1.3 twin (#883).
- Opening a sketch runs the generic rank only when a part of the sketch loses rank, and the Sketcher does not render the sketch again for a camera that the viewport rounded to 32-bit floats. Both make A-SK-ENTER on S1-2000 faster (#813, #815).
- `through_all` takes `reversed = true` or a `direction` to cut one way, as a FreeCAD Pocket of Type Through all does. Plain `through_all` still cuts both ways (#884).
- The FCStd reader writes a Tangent between two lines as two collinear lines (#881), joins the curves at the ends of a sketch line of no length (#886), and puts a sketch on a tilted plane on a datum plane of its own, so the features on it build (#887).
- An ellipse whose minor radius is a little past its major radius builds turned a quarter, as the same ellipse (#885).

Known issues: the 3D view shows the faces and edges of an imported body only. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body. Edit Feature on a pad does not open its task panel yet. The 300-feature edit of J-PD-12 takes about 13 s with OCCT in a debug build, against the bar of 1 s (#882). The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. The aarch64 Linux binary is not in the release, because the cross linker fails. The release Linux binaries do not include OCCT. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.102

A Body has a Tip, as in FreeCAD. Set Tip in the tree menu moves it, a new feature goes in after it, and the FCStd reader reads it. Journey J-FC-05 moves the Tip back, adds a feature and moves the Tip forward on a FreeCAD file with sixteen features. Fillet and Chamfer take edges that you pick on any body in the 3D view, and show a preview while the task panel is open.

- The document has one Tip. The new command Set Tip (`feature.set_tip`, FreeCAD `PartDesign_MoveTip`) is in the tree menu. A new feature goes in after the Tip and becomes the Tip. OCL writes `@tip` above the feature that is the Tip. The tree marks the Tip and dims each feature after it, and the kernel builds only the bodies up to the Tip (#803).
- The FCStd reader reads the Tip of the last body that has solids as `@tip`. The Tip of another body stays a note on the feature, because OpenCAD has one Tip (#804).
- The mesher cuts a face whose bound crosses itself into the loops it makes, as OCCT does. All 20 cars of DrivAerML now mesh with no failed face (#805).
- A revolve or a helix builds about a line of a sketch, such as the V axis that the FreeCAD import writes for a Revolution or a Groove (#806).
- Journey J-FC-05 sets the Tip on pocket9 of `synthetic-history-30.FCStd`, adds a Subtractive Box and sets the Tip on pocket15 again. The OCL is the golden OCL, and OCCT gives the volume of the FreeCAD 1.1.3 twin (#807).
- Fillet and Chamfer take the new argument `edges`. Each edge is a point on it, such as `pt3(477.498 mm, 142.294 mm, 141.171 mm)`, and the kernel uses the edge nearest to the point. The References list of FreeCAD maps to this field. On three edges of the Windsor body, OCCT gives the volume, area, centroid and topology of FreeCAD 1.1.3 (#808).
- While the Fillet or Chamfer panel is open, the edges of an imported body show over the 3D view. Click an edge to put it in References, and click it again to take it out (#809).
- The test kit asks the core for the OCL only when an invariant check needs it, after the step, so the write of the OCL is not in the span of each edit (#810).
- Fillet and Chamfer show a preview. The edges in References show as a ghost in the 3D view, and the core runs the command on a scratch copy of the document at each input, with the new `preview` request. An error shows under its field, and Cancel leaves no trace (#811).

Known issues: the 3D view shows the edges of an imported body only, and it also shows the edges at the back of the body. The ghost of Fillet and Chamfer marks the edges that the command changes, and not the rounded body, because the web core does not give the mesh of the result yet. The desktop app does not preview Fillet and Chamfer, does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. OpenCAD has one Tip, and FreeCAD has one in each body. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. The aarch64 Linux binary is not in the release, because the cross linker fails. The release Linux binaries do not include OCCT. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.101

The Part workbench has Check Geometry on the kernel, with the rows, the message and the shape content of FreeCAD 1.1. Journey J-PT-01 checks its compound and the AutoCFD chassis with it. The web app now reads the chassis as 10 solids, as FreeCAD does.

- Check Geometry checks each selected body with BRepCheck on the kernel, and shows the results tree and the shape content in the Tasks tab. It is in the Part menu, on the Boolean toolbar and in the palette. `opencadd serve` has `POST /check`. Journey J-PT-01 runs it from the toolbar and the menu, and compares the rows with the FreeCAD twin. OCCT 7.8.1 finds other bad faces in the chassis on arm64 and on x86_64, so golden.json keeps the rows of each CPU (#799).
- The STEP reader of the web app reads each part of a shell that shares no edge as a solid, in a compound, as FreeCAD does. The AutoCFD chassis imports as 10 solids and not 5 (#801).
- The search for a point on a B-spline surface goes past the narrow side of a patch that is almost a cone, and leaves a side whose first span is one curve. A face of DrivAerML run 19 now meshes, and edges of the ABC sample lie on their faces (#800).
- The frame check of the journeys counts the React commits of each frame by its number, not by its time (#797).

Known issues: the face numbers of a STEP file can differ from FreeCAD, because OCCT 8 and the web reader put the faces in another order than OCCT 7.8. Check Geometry needs the kernel, so it works in the web app only with `?kernel=`. FreeCAD 1.1 shows the wrong words for the BRepCheck statuses from 26 on, and OpenCAD shows the right words. The desktop app does not give the bytes of an imported file to the kernel yet, and does not import a STEP file yet. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. The aarch64 Linux binary is not in the release, because the cross linker fails. The release Linux binaries do not include OCCT. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.100

The web app gives the files of STEP imports to the kernel, so the features after an import build with `opencadd serve`. Journey J-SF-01 runs the Part and Surface commands for sheets through the GUI on reference points of the Ahmed body, to the values of FreeCAD 1.1.3. No command of the coverage gate is left without a GUI run.

- `opencadd serve` takes the bytes of a file at `POST /blob` and answers with its hash. The web worker sends the file of each STEP import before a build, and sends it again when the kernel restarts. A failed feature builds again when its file arrives (#792).
- Journey J-SF-01 reads the reference points of the rear of the Ahmed body from the STL of AhmedML run 1. It runs Ruled Surface, Filling, Make Face, 2D Offset, Extend Face, Projection on Surface, Fill Boundary Curves and Defeaturing from the toolbars, the menus and the palette. The sheets equal the FreeCAD 1.1.3 twin. Rule 8 of the coverage gate has no gaps now, and 111 commands are at L4 (#794).
- A unit test of the sketch view waits for the span of the open and two frames, and not for 200 ms (#793).

Known issues: the desktop app does not give the bytes of an imported file to the kernel yet. J-SF-01 does not check G1 continuity, because Filling cannot match the tangent of a support face yet, and OpenCAD has no command to scale a mesh. There are no draggers yet for the position inputs, for a hole, a thickness or an offset, and there is no push and pull. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. J-PT-01 does not run Check Geometry yet. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.99

The kernel builds imported STEP bodies, and journey J-PT-02 cuts the WindsorML body with Cross Sections, Slice, Slice apart and the new Part Section, to the values of FreeCAD 1.1.3. The kernel picks an edge of an imported body by a point, so all 121 OCL examples in the docs build. The menu bar has the FreeCAD Edit menu, and a 2,000-entity sketch opens about twice as fast.

- The kernel builds `import_step` features from the bytes of the file, which the host gives by the hash of the blob. Without the file, the import says that the file is not here, and the features after it wait (#786).
- Journey J-PT-02 drops the Windsor body of WindsorML run 0 on the viewport, makes cross sections across and along it, cuts it into three pieces with Slice and takes the roof off with Slice apart. The pieces, the halves and the sections equal the FreeCAD 1.1.3 twin (#787).
- Part Section (`part.section`) makes the edges where a body meets a tool body, as Part_Section of FreeCAD does. It is in Part > Section and on the Part Tools toolbar. J-PT-02 makes the section of the Windsor body, which has 10 edges and a length of 2642.52 mm, as in FreeCAD (#788).
- `body.edges.at(x, y, z)` builds on the kernel. The kernel and the web app pick the edge with the same rule. The External Projection and External Intersection examples on the Windsor body build, and the arch example in the sketch docs no longer crosses its base line (#790).
- Each toolset has the FreeCAD Edit menu first. Undo, Redo, Select All and Delete run, and the other items show that they are not in OpenCAD yet (#785).
- A sketch with 2,000 entities opens in about 210 ms, not 400 ms, and the open has an A-SK-ENTER span. The solver no longer builds the dense Jacobian to read the residuals, and the minimum degree ordering ends a clique at once (#789).

Known issues: the desktop app and the web worker do not give the bytes of an imported file to the kernel yet, so the features after an import build only in the tests. There are no draggers yet for the position inputs, for a hole, a thickness or an offset, and there is no push and pull. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. J-PT-01 does not run Check Geometry yet. Opening a 2,000-entity sketch is still over the 50 ms budget of A-SK-ENTER. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.98

Journeys J-PT-01, J-SK-17 and J-PD-02 run the Part primitives and Booleans, the Sketcher edit tools and the PartDesign dress-ups through the GUI, so 100 commands are at L4 in the coverage gate. The I16 flake of the sketch journeys is fixed. The mesher meshes more faces of the ABC files and refines a face faster.

- Journey J-PT-01 makes each Part primitive once and runs Boolean, XOR, Boolean Fragments, Refine Shape, Offset, Scale, Simple Copy, Reverse Shapes and Compound from the toolbar, the menu and the palette (#776).
- Journey J-SK-17 uses each edit tool of the Sketcher once. It found that a mirror or a half turn wrote `-0 mm` for a point on an axis. The edit tools now write `0 mm` (#778).
- Journey J-PD-02 runs Fillet, Chamfer, Draft and Thickness once, then adds an Involute Gear and fuses it by PartDesign Boolean. The result equals the FreeCAD 1.1.3 twin (#783).
- `oc.tool` in `oc-drive` starts a click tool from its key, toolbar, menu or the palette, and records the step. J-SK-02 uses it (#775).
- The command palette makes the row under the mouse active in the store flush, and the dragger of a length shows its handle in the commit that opens it. Before, invariant I16 sometimes found two commits in one frame in J-SK-02 and J-SK-17 (#782).
- The mesher lets the seam of a closed face wind between its holes (#774), splits an open triangle side of a solid at the corners on it (#777), and keeps the search on a spline surface from overshooting and from stopping past a side (#779).
- The mesher keeps the point and the normal at each corner while it refines a face (#780), and the first partials of a surface no longer allocate (#781). This makes the mesh of a DrivAerML slice faster.

Known issues: there are no draggers yet for the position inputs, for a hole, a thickness or an offset, and there is no push and pull. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. J-PD-02 uses a padded block and not WindsorML `windsor_0.stp`, and J-PT-01 does not run Check Geometry yet. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.97

The Fillet, Chamfer, Linear Pattern and Polar Pattern panels have draggers in the 3D view. 47 commands are at L4 in the coverage gate, because journeys ran them through the GUI. The STEP reader reads trimmed curves, and the mesher meshes more faces of the ABC files. The sketch journeys pass again.

- The Fillet panel has a Radius handle at the first corner, and the Chamfer panel has a Size handle, and a Size 2 handle with two distances. The Linear Pattern panel has a handle along each direction, and the Polar Pattern panel has one on the ring round the axis. They set the length, the angle or the spacing (#763).
- `cargo xtask journey-record` writes the commands that each journey ran through a key, a menu, a toolbar, the palette or a task panel. The coverage gate reads that record for L4. 47 commands are at L4 now (#764, #771, #772).
- With no tool in use, a press in a sketch takes a point before the mark of a constraint, as in FreeCAD. Before, a press on a corner with a mark near it did not drag the corner (#772).
- In a sketch, a dimension from the Dimension form clears the selection, as in FreeCAD, so the next Esc leaves the sketch (#770).
- The sketch and UI journeys select the geometry first and then start a dimension tool, as a FreeCAD user does. J-SK-03, J-SK-04, J-SK-05, J-SK-10, J-UI-02, J-UI-04 and J-UI-07 pass again (#770, #772).
- The STEP reader reads `TRIMMED_CURVE`, so the DrivAer cars and the chassis of the AutoCFD set open (#765).
- The mesher takes a circle between two vertices at one place as a whole turn (#767), finds the pole of a face of revolution inside the range of its curve (#768), and meshes a face bounded by two lines out and back as nothing (#769). A point on a B-spline surface no longer allocates, which makes the mesher faster (#766). The `mesh` example runs the STEP mesher over a set of files (#762).

Known issues: there are no draggers yet for the position inputs, for a hole, a thickness or an offset, and there is no push and pull. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. On a loaded machine, J-FC-01 and J-FC-04 can go a few ms over their time budgets. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.96

A FreeCAD user finds the Surface commands in a Surface toolset of the FreeCAD profile. Each field and property of the mapped FreeCAD commands now has an OpenCAD argument or a note. The mesher no longer bridges a hole through a corner of another hole.

- The toolset combo box of the FreeCAD profile has Surface, with the Surface menu and the Structure and Surface toolbars of FreeCAD. Filling, Fill Boundary Curves and Extend Face can run. Sections, Curve on Mesh and Blend Curve are greyed, because they are not in OpenCAD yet (#760).
- The FreeCAD profile maps the tool widget of the sketcher, the sketcher dialogs, and the Part Extrude and Revolve dialogs (#755), the properties of the datums, the Part primitives and the sketch object (#756), the Part tools such as Boolean, Loft, Sweep, Offset and Scale (#757), and Filling, Extend Face, Fill Boundary Curves and Projection on Surface (#759). A field that OpenCAD does not have gets a note that says why. The coverage gate now has 111 commands at L3.
- The mesher checks the corner that the bridge of a hole runs into, as it checks the other corners. Before, a bridge could run through a hole whose corners lie on the same line, which left open edges on a plate of the ABC files (#758).

Known issues: there are no draggers yet for the position inputs, for a fillet, a chamfer, a hole or a pattern, and there is no push and pull. No command is at L4 in the coverage gate yet, because L4 needs a journey record. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.95

The primitives have size draggers in the 3D view. A FreeCAD user who opens a PartDesign task panel in the FreeCAD profile sees each FreeCAD field, or a greyed field that says why OpenCAD does not have it. The mesher closes the gaps at flat ears.

- The Box, Cylinder, Sphere, Cone, Ellipsoid, Torus and Prism panels, additive and subtractive, have a size dragger for each size. The handles sit in the frame of the attachment of the primitive. A primitive attached to a feature, or with an attachment mode that is not an object mode on a standard plane, shows no handle yet, and a wedge has no handle yet (#747).
- The FreeCAD profile maps the properties that each feature has in FreeCAD, such as Label, Placement and Refine (#748).
- The FreeCAD profile maps each field and each visible property of the Pad, Pocket, Revolution and Groove (#749, #750), the Linear Pattern, Polar Pattern and Mirrored (#751), and the Fillet, Chamfer, Thickness and Draft (#753). A field that OpenCAD does not have is greyed, and its note says why. The coverage gate now has 43 commands at L3.
- The mesher splits the triangle across a flat ear at its middle point (#746), and goes round the flat ears again until none is left (#752), so a solid from the ABC files has no open edges there.

Known issues: there are no draggers yet for the position inputs, for a fillet, a chamfer, a hole or a pattern, and there is no push and pull. No command is at L4 in the coverage gate yet, because L4 needs a journey record. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.94

The Revolve, Groove and Pocket panels have draggers in the 3D view. The mesher meshes two more kinds of face from the ABC files.

- The Revolve and Groove panels have an angle dragger. The handle turns on an arc about the axis, the snap step changes with the zoom, and you can type a value with a unit, such as `0.5 turn`, during the drag (#741).
- The Pocket panel has a length dragger that goes into the body. A two sided pad or pocket has a second handle for the second length, and a revolve or a groove with two angles has a second handle for the second angle. A reversed blind pad puts its handle on the reversed side (#743).
- The mesher finds the seam of a closed B-spline that all but stops at its seam, so the edge lands in the right place on the face (#742).
- The mesher gives no triangles and no error for a face whose edges are all one point (#744).

Known issues: there are no draggers yet for the position inputs, for a fillet, a chamfer, a hole or a pattern, and there is no push and pull. The web viewport does not draw the features that the kernel builds unless the app uses `?kernel=`. A draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet. OpenCAD does not revolve about the vertical axis of a sketch yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.93

The FCStd import reads the Draft and the Thickness of FreeCAD. The mesher meshes a band with a slot that winds round it more than once.

- The FCStd import reads a Thickness as `shell`, with its faces, Value, Make thickness inwards, Mode, Join and Intersection. It reads a Draft as `draft`, with its faces, Angle, Reversed, neutral plane and pull direction. With no neutral plane, it finds the plane that FreeCAD finds from the edges of the first face. A twin test compares 7 thicknesses and 10 drafts with FreeCAD 1.1.3 (#739).
- The FCStd import keeps a Part object out of the solid that the PartDesign features of a file from before bodies build (#738).
- The mesher opens a band with a line that winds round it when a slot covers every place round the band, so no straight line misses the slot (#737).

Known issues: a draft whose neutral plane is not parallel to XY, YZ or ZX is not imported yet. A fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Sprocket or a Shape Binder yet, or a mirror plane or a direction from a face or an edge of a feature. OpenCAD does not revolve about the vertical axis of a sketch yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.92

The FCStd import reads the Hole, the Fillet and the Chamfer of FreeCAD. OCL can pick an edge by the two faces it lies between. The mesher meshes more faces.

- The FCStd import reads a Hole as `hole`, with its diameter, depth, drill point, taper and head cut, and with a screw standard as a counterbore or a countersink. A twin test compares 14 holes with FreeCAD 1.1.3 (#731).
- OCL picks an edge by the two faces it lies between, as `body.edges.between(A, B)` (#732).
- The FCStd import reads a Fillet and a Chamfer. It finds the two faces of each FreeCAD edge number in the BREP of the file and names the edge by those faces. The chamfer types and Flip direction come in too. A twin test compares 20 fillets and chamfers with FreeCAD 1.1.3 (#735).
- The mesher meshes a hole that the line across a band would cut (#728), meshes a band that goes the long way round a torus (#729), refines the longest edges of a face first so a face that stops at the triangle limit is as fine all over (#730), and finds an edge on a B-spline whose pole is along v (#733).

Known issues: a fillet or a chamfer on a face of a revolution, a pattern, a hole or another fillet is not imported yet. The FCStd import does not read a Thickness, a Draft, a Sprocket or a Shape Binder yet, or a mirror plane or a direction from a face or an edge of a feature. OpenCAD does not revolve about the vertical axis of a sketch yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.91

The FCStd import reads the patterns, the Mirrored, the MultiTransform, the side type, the custom direction and the taper of FreeCAD, and the mesher meshes more faces.

- The FCStd import reads the Type of a Pad or a Pocket with the correct values for Symmetric and Two sides, and the custom direction and Length along sketch normal. Before this change, a symmetric pad came in as a blind pad and a pad of two sides came in as a symmetric pad (#721).
- The FCStd import reads the Taper angle of a Pad or a Pocket with one side as the `draft` of the extrude (#722).
- The FCStd import reads a Linear Pattern, a Polar Pattern and a Mirrored with their seeds, directions, modes, spacings and counts. A twin test compares 16 patterns with FreeCAD 1.1.3 (#725).
- The FCStd import reads a MultiTransform with its steps. FreeCAD builds no copy for an instance that a Scaled step after other steps keeps at its size, so the import puts these instances in `skip`. A twin test compares 13 MultiTransforms with FreeCAD 1.1.3 (#726).
- The mesher meshes a cone that comes to its apex (#720), does not turn a thin triangle over (#723), and meshes a face whose only bounds are holes in a closed surface (#724).

Known issues: a taper with a custom direction, a taper of a two sided or symmetric feature, and TaperAngle2 are not imported yet. The FCStd import does not read a Thickness, a Draft, a Chamfer, a Fillet, a Hole, a Sprocket or a Shape Binder yet, or a mirror plane or a direction from a face or an edge of a feature. OpenCAD does not revolve about the vertical axis of a sketch yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.90

Pad and Pocket take a custom direction, Fillet and Chamfer take Use all edges, the patterns take the modes of FreeCAD, and the mesher reads more faces from Onshape.

- Linear Pattern and Polar Pattern take the Extent and Spacing modes, the custom spacings and Reverse of FreeCAD. A Linear Pattern takes a second direction for a grid. A twin test compares the patterns with FreeCAD 1.1.3 (#715).
- Fillet and Chamfer take Use all edges, and the Boolean Operation panel has the Bodies and the Type of FreeCAD. In the OCL, `edges` on its own is every edge of a body. A twin test compares 5 fillets and chamfers with FreeCAD 1.1.3 (#717).
- Pad and Pocket take a custom direction and Length along sketch normal. In the OCL, `extrude` takes `direction = vec3(x, y, z)` and `along_normal = true`. A custom direction works with blind, symmetric and two sided extents. A twin test compares 7 pads and 4 pockets with FreeCAD 1.1.3 (#718).
- The mesher meshes a band with a bound at each end and no seam (#713), finds points on surfaces of extrusion and of revolution with a search along the swept curve (#714), and meshes a dome bounded by one circle with no seam (#716). Onshape writes these faces.

Known issues: a direction from an edge, a taper with a custom direction, and up to face with a custom direction are not in OpenCAD yet. When Neutral Plane of a Draft is empty, OpenCAD takes XY, but FreeCAD guesses a plane from the first face. The FCStd import does not read a pattern, a Thickness, a Draft, a Chamfer, a Fillet, a Hole, a Sprocket or a Shape Binder yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.89

Chamfer, Thickness, Draft and Involute Gear take the fields of FreeCAD.

- Chamfer takes the Type of FreeCAD: Equal distance, Two distances, and Distance and angle. Flip direction takes the other face as the first face, and the first face is the face that FreeCAD takes (#708).
- Thickness takes the Mode, the Join type, Intersection and Make thickness inwards of FreeCAD. Make thickness inwards starts ticked, as in FreeCAD. A twin test compares 7 shells with FreeCAD 1.1.3 (#709).
- Draft takes the Neutral Plane, the Pull Direction and Reverse pull direction of FreeCAD. When Pull Direction is empty, it is the normal of the Neutral Plane. A twin test compares 9 drafts with FreeCAD 1.1.3 (#710).
- Involute Gear draws the outline that FreeCAD draws, with Bezier flanks and a root fillet. It takes the Addendum, Dedendum and Root fillet coefficients, External gear and High precision of FreeCAD. The default gear now has the root fillet of a new FreeCAD gear, so its volume changes. A twin test compares 8 gears with FreeCAD 1.1.3 (#711).

Known issues: when Neutral Plane of a Draft is empty, OpenCAD takes XY, but FreeCAD guesses a plane from the first face. A Thickness with no open face is not in OpenCAD yet. The FCStd import does not read a Thickness, a Draft, a Chamfer, a Fillet, a Hole, a Sprocket or a Shape Binder yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.88

Pad, Pocket, Revolution and Groove take the fields of FreeCAD, and the STEP reader reads offset surfaces.

- Pad and Pocket take the Type, the second side, Up to face, Offset to face, Taper angle and Reversed of FreeCAD. A Pad is `blind`, `symmetric`, `two_sided` or `up_to_face`, and a Pocket can also be `through_all`. A twin test compares 8 pads and 3 pockets with FreeCAD 1.1.3 (#704).
- Revolution and Groove take the Type, 2nd angle, Symmetric to plane and Reversed of FreeCAD, and a Groove can go through all. In the OCL, a revolve takes the extents `symmetric(a)` and `two_angles(a1, a2)` and the parameter `reversed`. The FCStd import reads these properties of a Revolution or a Groove. A twin test compares 5 revolves and 4 grooves with FreeCAD 1.1.3 (#706).
- The STEP reader reads `OFFSET_SURFACE`. A face on an offset surface is meshed, and a curve that refers to itself is an error, not a crash (#705).

Known issues: a Revolution or a Groove up to a face is not in OpenCAD yet. The second side of a Pad or a Pocket takes no taper or offset of its own yet. Sub-Shape Binder is not in OpenCAD yet. The FCStd import does not read a Hole, a Sprocket or a Shape Binder yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.87

Part Design has the Sprocket, Clone and the Shape Binder.

- Part Design has the Sprocket. It makes a sprocket for a roller chain with the teeth that FreeCAD draws. Select one of 30 ANSI and ISO 606 chains in Sprocket reference, and it gives the pitch, the roller diameter and the tooth width. A value that you type is used in place of the value of the chain. In the OCL, it is a `sprocket` (#700).
- Clone is on the Part Design Helper Features toolbar and in the Part Design menu. It opens the Copy Body panel with the title Clone, and the clone follows the body that it copies (#701).
- Part Design has the Shape Binder. It copies a body, or faces of one body, as a body that follows them. One face stays a face, and more faces make a compound, as in FreeCAD 1.1.3. The panel has Object and References, as the FreeCAD panel does. In the OCL, it is a `shape_binder` (#702).
- A box of a task panel that has no FreeCAD label takes the text of the button that picks its value, such as Object (#702).

Known issues: Sub-Shape Binder is not in OpenCAD yet, and a Shape Binder cannot take a sketch or edges. OpenCAD has no Body container yet, so a clone is a body in the tree. The FCStd import does not read a Hole, a Sprocket or a Shape Binder yet. The modeled thread of a Hole is not in OpenCAD yet. The viewport does not draw datums yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.86

Part Design has the Hole, a Part Design primitive takes the Attachment of FreeCAD, and the STEP import reports the gaps of a file at its vertices.

- Part Design has the Hole. It drills a hole into the body that you choose at each circle, arc and point of a sketch, with a flat or angled drill point, a taper and a counterbore, countersink or counterdrill head. With an ISO metric coarse or fine thread, it takes the clearance or tap drill diameter and the head sizes from the ISO tables, as FreeCAD does. Hole is in the Part Design menu and the Part Design Modeling Features toolbar. In the OCL, it is a `hole` with `profile`, `into`, `dia` and `depth` (#698).
- The task panels show the FreeCAD labels of a field in a grid, so the Hole panel, X, Y and Z of the pipes, and Side 1 and Side 2 of Pad and Pocket have their FreeCAD names (#698).
- Each of the 16 Part Design primitives takes an attachment mode, references, an Attachment Offset and Flip sides, as the Attachment panel of FreeCAD does. A primitive on a face, a datum or a coordinate system goes where FreeCAD 1.1.3 places it (#696).
- The STEP import reports a file whose vertices are more than 0.01 mm off the ends of their edges, in the status bar. The STEP reader also puts the end of an edge on a closed B-spline and on an ellipse in the right place (#697).

Known issues: the modeled thread of a Hole and the UNC, UNF, UNEF, NPT, BSP, BSW and BSF threads are not in OpenCAD yet. The viewport does not draw datums yet. The attachment modes that need an edge or a curve, such as Frenet, are not in OpenCAD yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.85

Datums of Part Design take the attachment modes of FreeCAD, OpenCAD has the Local Coordinate System, and journey J-PD-05 checks the place of each datum against FreeCAD 1.1.3.

- Datum Plane, Datum Line and Datum Point take an attachment mode, references, an Attachment Offset and Flip sides, as the Attachment panel of FreeCAD does. Each mode places the datum as FreeCAD 1.1.3 does: Translate origin, Object's XY, XZ and YZ, Plane face, Plane by 3 points and Normal to edge (3 points) for a plane, Object's X, Y and Z and Through two points for a line, and Object's origin and Center of mass for a point. In the OCL, the offset is a length or a `placement(...)` with a move and three turns (#692).
- Coordinate System makes a Local Coordinate System, as `Part_CoordinateSystem` and `PartDesign_CoordinateSystem` do in FreeCAD. It takes the plane modes and the six Align modes, such as O-X-Y. A sketch on a coordinate system or a datum plane takes its full placement, as in FreeCAD (#692).
- Journey J-PD-05 makes 19 datums from the toolbar and puts a boss, a notch and a post on them. Its FreeCAD twin gives the same volume, area, centroid, box and topology, and the same origin and axes for each datum (#692).
- A references field, such as Features of a pattern or Ref2 of a datum, names each feature under its ids (#694).
- The STEP reader of the web app reads a conversion-based length unit by its factor. Files from Onshape, such as the ABC dataset, no longer come in a thousand times too small (#693).
- A number field of a task panel hints "such as 40 mm or 90 deg", because it takes a length or an angle (#692).

Known issues: the viewport does not draw datums yet. The attachment modes that need an edge or a curve, such as Frenet, are not in OpenCAD yet. A Part Design primitive is at the origin, because OpenCAD does not have the Attachment of a primitive yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.84

Part Design has Multi-Transform, so each command of the Transformation Features toolbar can run, and two new journeys check Part Design against FreeCAD 1.1.3.

- Part Design has Multi-Transform. It takes features through a list of Mirror, Linear Pattern, Polar Pattern and Scale steps in turn, as in FreeCAD. A Scale step grows the copies so far in runs, from no change to its factor. The task panel has a Transformations list with a row for each step. In the OCL, it is a `multi_transform` with `steps`. The Mirror panel no longer needs an offset (#690).
- Journey J-PD-04 makes Mirror, Linear Pattern, Polar Pattern and Multi-Transform once each from the toolbar. Its FreeCAD twin gives the same volume, area, centroid, box and topology (#690).
- Journey J-PD-01 makes each additive and subtractive feature of Part Design once from its task panel. The wedge has an `axis` argument, and the helix and the torus have the same faces and seams as in FreeCAD, so the FreeCAD twin gives the same topology (#689).
- Pad, Revolution, Additive Loft, Additive Pipe and Additive Helix join the body that you choose, as a Part Design feature joins its body (#687).
- The Add list of a Features field no longer makes the task panel wider than the dock (#690).
- The reference dimension test follows the rule of #614 (#688).

Known issues: the Features field of a pattern shows the id of a feature, not its name. A Part Design primitive is at the origin, because OpenCAD does not have the Attachment of a primitive yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.83

The Part Design workbench has its primitives, loft, pipe and helix, and the Data tab shows the rows that FreeCAD gives to Python objects when it opens a file.

- Part Design has Additive Helix, Subtractive Loft, Subtractive Pipe and Subtractive Helix. Additive Loft and Additive Pipe take the fields of the FreeCAD panels. A subtractive feature cuts the body that you choose (#683).
- Part Design has the 16 primitives: Additive and Subtractive Box, Cylinder, Sphere, Cone, Ellipsoid, Torus, Prism and Wedge. Each one opens the page of its shape from the FreeCAD primitive panel. An additive primitive joins the body that you choose, or makes a new solid. A subtractive primitive cuts the body that you choose (#685).
- The Data tab of an FCStd object shows the rows of its extensions, such as the Attachment group, and applies the changes that FreeCAD makes with Python when it opens the file, for the Draft clone and 2D view, the involute gear and the CAM job, stock, profile, drilling, tool bit and tool controller. The corpus objects whose rows are different from FreeCAD went from 45 to 8 (#684).
- Journey J-IMP-07 also runs in the `timing` project and checks the budgets of A-FILE-OPEN-PART on the machine GPU (#682).
- The planted bug patches I5, I14 and I16 and the perf patch tree-32ms apply to main again (#681).

Known issues: 8 attached sketches of the corpus show a Placement that is different from FreeCAD, because FreeCAD calculates it again from the support. A Part Design primitive is at the origin, because OpenCAD does not have the Attachment of a primitive yet. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The aarch64 Linux binary is not in the release, because the cross linker fails. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.82

The Properties panel shows the Data tab of FreeCAD for the objects of a FreeCAD file, and journey J-FC-01 checks the tree and the properties of three FreeCAD files against FreeCAD 1.1.3.

- Journey J-FC-01 opens `freecad-1.1.3-crank`, `freecad-1.1.3-motor-mount-inch` and `freecad-1.1.3-part-design-example` and compares each row of the tree with FreeCAD. The tree shows the recompute mark on an object that FreeCAD marks for a recompute when it opens the file (#677).
- The app goes idle when you select a feature that the FreeCAD file hides. Before, the test channel waited for a selection that the viewport could not draw (#678).
- The Properties panel shows the groups, the names, the order and the values of the Data tab of FreeCAD for each object of a FreeCAD file. In the FreeCAD profile, you can set a property that OpenCAD can change, such as the Length of a Pad, and the other rows are locked. J-FC-01 compares the rows with FreeCAD (#679).

Known issues: 45 objects of the corpus show Data rows that are different from FreeCAD. These are Python objects that FreeCAD changes when it opens the file, and attached sketches whose Placement FreeCAD calculates again. Journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.81

The model tree shows the tree of a FreeCAD file as FreeCAD shows it, and each release now has single-file binaries for Linux, macOS and Windows.

- Each release has the `opencadd` command line tool as one file for Linux x86_64 and aarch64 (static), macOS x86_64 and aarch64, and Windows x86_64, with a `.tar.gz`, a `.zip` and `SHA256SUMS`. The script `scripts/release-assets` builds and attaches them when the release workflow cannot run (#671, #673).
- The model tree of a FreeCAD file has the FreeCAD tree: the Body, its closed Origin, the features with their sketches, the groups and the objects that are not features, with the FreeCAD label and icon of each (#672).
- The eye, Space, Hide and Show work on the objects of a FreeCAD file. A hidden Body, Part or Origin hides what it holds in the viewport, and a plain group gives its visibility to what it holds, as FreeCAD does (#675).
- The OCCT reader builds a band that it split in two faces again as one valid face, so more ABC files read as one closed shell (#674).

Known issues: journeys J-SK-04 and J-SK-05 fail with a wait in the sketcher. The gallery visual test fails with "the gallery changed size" on the pixel project. OpenCAD does not read the shape of an object that FreeCAD hid, so when you show it there is no shape to draw. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.80

The apps have a Spreadsheet panel, and journey J-FC-04 binds a pad length to a spreadsheet alias the FreeCAD way. The property editor milestone item #380 is done.

- The core has the commands `sheet.create`, `sheet.cell` and `sheet.alias`. Each edit of a cell or an alias is one undo step, and the features that read the cell change in the same step (#666).
- The Spreadsheet panel shows the cells of a sheet in a grid, with a Contents field and an Alias field. A typed key starts the edit of a cell, and Enter sends it (#667).
- The FreeCAD profile has the Spreadsheet toolset, with New Spreadsheet and Set Alias on its toolbar and Ctrl+Shift+A for Set Alias (#668).
- Journey J-FC-04 opens a FreeCAD file, makes a spreadsheet, gives a cell an alias and binds the length of the Pad to it with `=`. A click on a feature of an opened FCStd file now selects the right feature, and the sheet and property edits change the UI in one commit (#669).

Known issues: the gallery visual test fails with "the gallery changed size" on the pixel project. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.79

Pads and pockets take two lengths, and OCL has sheets, so FreeCAD files with spreadsheets come in with their cells.

- Pad and Pocket have the Two dimensions type of FreeCAD, as the `two_sided(d1, d2)` extent. The properties panel shows both lengths, and the FreeCAD reader keeps them (#662).
- OCL has `sheet` blocks, as the FreeCAD Spreadsheet workbench has them. A cell holds a number, a text or a formula, and `@alias(name)` gives it a name that the expressions of features can read (#663).
- The FreeCAD reader reads each spreadsheet as a sheet, with its cells and aliases. The expressions that read the cells come in as `@expr`. A formula that OCL cannot read is left out with a note (#664).

Known issues: there is no Spreadsheet panel in the apps yet. The gallery visual test fails with "the gallery changed size" on the pixel project. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.78

The Sketcher draws the marks of its constraints on the GPU, so UD6 is done.

- The viewport worker gives its canvas a new size in the next frame. Chromium sometimes lost the GPU device when the canvas changed size just after a frame (#658).
- The Sketcher draws the marks of the constraints in the overlay of the renderer. The marks move with the camera in the same frame as the shapes. Without WebGPU, a worker still draws them on a canvas (#659).

Known issues: the gallery visual test fails with "the gallery changed size" on the pixel project. The native viewport on Linux and the hardware-bound navigation and input items of M0 are still open.

## 0.0.77

The renderer draws labels and lines over the frame, as the first part of the Sketcher marks on the GPU.

- Clear the sketch selection in the store flush after a reply (#651).
- The renderer has an overlay pass for labels and lines. Each one is at a point of the model and an offset in pixels, so it follows the camera in the same frame. The text comes from a signed distance atlas of the code font, so it is sharp at all sizes (#652).
- The label atlas has bold and italic faces and the signs of the geometric constraints. The marks of a sketch can become overlay labels and lines on the sketch plane (#654).

Known issues: the Sketcher still draws its marks on a 2D canvas. The switch to the overlay comes in a later release.

## 0.0.76

The Sketcher tools now take their values from clicks on the sketch, with the hints of FreeCAD in the status bar.

- The dimensional constraint tools and Lock Position pick their geometry with clicks and show the FreeCAD hint of each step. The value field opens at the click that completes the picks (#647).
- Rotate, Scale, Offset and Mirror take their values from clicks. A dashed preview follows the pointer. Mirror goes across a line or an axis, or through a point or the origin (#648).
- Copy, Clone, Move and Rectangular Array take the destination of the reference point from one click, and dashed copies follow the pointer before it (#649).

Known issues: the sketcher does not show a sketch on the face of a body yet. A drag of a point does not snap to the grid yet (#382). The release builds do not have the OCCT kernel, so the operations that need it are off in them. Two dimensions with the same span draw their values on top of each other. The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.75

The sketcher now has each command of the FreeCAD Sketcher, with the weight of control points, the grid and snap tools, and the last five commands.

- A Radius Dimension on the control points of a B-spline now sets their weight, as a Weight constraint on a pole circle in FreeCAD. A double click on the weight opens it for edit. The journey J-SK-15 checks it against FreeCAD (#641).
- Toggle Grid, Toggle Snap and Rendering Order show the grid, snap a click to it or to a step of the snap angle, and set which kind of geometry is on top. They are in the status bar, the Edit controls and the palette, as the Sketcher Edit Tools of FreeCAD. The journey J-SK-16 checks them (#642).
- Edit Sketch (Sketcher_EditSketch) opens the selected sketch. Clone (Sketcher_Clone) opens the Copy form with Clone set. Creates a hexagonal profile (Sketcher_ProfilesHexagon1) adds the hexagon of FreeCAD's ProfileLib. Select Malformed Constraints and Select Partially Redundant Constraints select from the solver status. A sketch that is closed and opened again at the same revision now shows its geometry (#645).

Known issues: the sketcher does not show a sketch on the face of a body yet. A drag of a point does not snap to the grid yet (#382). The release builds do not have the OCCT kernel, so the operations that need it are off in them. Two dimensions with the same span draw their values on top of each other. The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.74

Each release now has apps and command line tools that are ready to use, and the sketcher has the rest of the B-Spline Tools that change the shape of a curve, as in FreeCAD.

- Each release now has a desktop app for Linux (an AppImage for x86_64 and aarch64), for macOS (a universal dmg and app) and for Windows (an exe), and a static `opencadd` command line tool for Linux, macOS and Windows. Each file has a SHA-256 sum and a build attestation. The install guide tells how to start each one for the first time (#638).
- Decrease B-Spline Degree (Sketcher_BSplineDecreaseDegree) and Decrease Knot Multiplicity (Sketcher_BSplineDecreaseKnotMultiplicity) give the control points and knots of FreeCAD. The journey J-SK-12 checks them on the Windsor body against FreeCAD (#636).
- Geometry to B-Spline (Sketcher_BSplineConvertToNURBS) changes a line, an arc, a circle, an ellipse or an arc of a conic into a B-spline with the same shape. The end points stay as the end control points. The journey J-SK-13 checks it on the side outline of the Windsor body against FreeCAD (#637).
- Join Curves (Sketcher_JoinCurves) makes two curves that meet at an end into one B-spline. It takes the joint point, two end points or two curves. A tangent constraint at the joint makes the joint smooth, as in FreeCAD. The journey J-SK-14 checks it on the side outline of the Windsor body against FreeCAD (#639).

Known issues: the sketcher does not show a sketch on the face of a body yet. The pole weight tool and the dimensions of the B-spline renderer are not in OpenCAD yet (#382). The release builds do not have the OCCT kernel, so the operations that need it are off in them. Two dimensions with the same span draw their values on top of each other. The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.73

The sketcher has the B-Spline Tools that keep the shape of a curve, as in FreeCAD.

- A B-spline now keeps its own knots, multiplicities and weights. A FreeCAD file keeps them when it opens, and the OCL writes them as `params`, `mults` and `weights` when they are not the default. Constrain Refraction now finds its item by the FreeCAD menu text (#632, #633).
- Increase B-Spline Degree (Sketcher_BSplineIncreaseDegree), Increase Knot Multiplicity (Sketcher_BSplineIncreaseKnotMultiplicity) and Insert Knot (Sketcher_BSplineInsertKnot) change a B-spline and keep its shape. Degree and Multiplicity run at once on the selected B-spline and clear the selection. Insert Knot puts a knot where the curve comes nearest to a click. A click now selects a B-spline, an ellipse or an arc of a conic. The journey J-SK-11 checks the three tools on the roof line of the Windsor body against FreeCAD (#634).

Known issues: the sketcher does not show a sketch on the face of a body yet. Decrease B-Spline Degree, Decrease Knot Multiplicity, Convert Geometry to B-Spline and Join Curves are not in OpenCAD yet (#382). Two dimensions with the same span draw their values on top of each other. The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.72

The sketcher has External Intersection and the Snell's law constraint, as in FreeCAD.

- External Intersection (Sketcher_Intersection, G, I) brings the points where the sketch plane cuts an edge of a body into the sketch as external geometry. Only the edges that cross or touch the plane show while the tool is on. In the OCL, it is `intersect(body.edges.at(x, y, z))`. The journey J-SK-09 checks it on the Windsor body against FreeCAD (#629).
- Constrain Refraction (Sketcher_ConstrainSnellsLaw, K, W) holds two rays and a boundary by Snell's law, with the ratio n2/n1. Like FreeCAD, it joins the ends of the rays and puts them on the boundary. In the OCL, it is `refraction(ray.end, bent.start, glass, ratio = 1.5)`. The journey J-SK-10 checks it on an edge of the Windsor body against FreeCAD (#630).

Known issues: the sketcher does not show a sketch on the face of a body yet, and the B-spline knot tools are not in OpenCAD yet (#382). A refraction does not take a B-spline or an ellipse, and its ratio cannot be edited in the apps yet. Two dimensions with the same span draw their values on top of each other. The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.71

The sketcher has the Visual Helpers, a section view, the internal geometry of conics and a clipboard, as in FreeCAD.

- Toggle Circular Helper for Arcs (Sketcher_ArcOverlay) shows the whole circle of each arc. The Toggle B-Spline Information Layer drop-down shows the degree, the control polygon, the curvature comb, the knot multiplicity and the control point weight of each B-spline. The helpers do not change the sketch, and they stay on from one sketch to the next. Z, Z now runs Switch Virtual Space (#624).
- Toggle Section View (Sketcher_ViewSection) cuts away the part of the model in front of the plane of the sketch in edit. Run it again, or leave the sketch, to show all of the model (#625).
- Toggle Internal Geometry (Sketcher_RestoreInternalAlignmentGeometry) shows the axes and the foci of an ellipse, an arc of an ellipse, a hyperbola or a parabola as construction geometry. When the conic has all of them, the command removes each part that no other constraint holds. In the OCL, the constraint is `internal(a, conic, role = major)` (#626).
- Copy Elements, Cut Elements and Paste Elements (Sketcher_Copy, Sketcher_Cut, Sketcher_Paste) carry the selected geometry and the constraints among it through a clipboard, in the same sketch or into another one. Ctrl+C, Ctrl+X and Ctrl+V run them in the sketch (#627).

Known issues: the sketcher does not show a sketch on the face of a body yet, and the B-spline knot tools are not in OpenCAD yet (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.70

You can check a sketch for problems and fix them, and hide constraints in the virtual space, as in FreeCAD.

- Validate Sketch (Sketcher_ValidateSketch) finds missing coincidences, invalid constraints, degenerate geometry, open and non-manifold vertices, and constraints on external geometry. Each check except the vertices has a fix. The status bar shows the FreeCAD message, and the sketcher selects what the check found. A choice field with long choices is now a dropdown in the task panel (#621).
- Switch Virtual Space (Sketcher_SwitchVirtualSpace) moves the selected constraints to the virtual space or back. With no selection, the sketcher shows the other space. The solver uses the constraints of both spaces. In the OCL, the word `virtual` comes after a constraint in the virtual space, and a FreeCAD file keeps the space of each constraint when it opens (#622).

Known issues: the sketcher does not show a sketch on the face of a body yet, and the B-spline knot tools are not in OpenCAD yet (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.69

You can move a sketch to another plane, a datum plane or a face, as in FreeCAD.

- Reorient Sketch puts the selected sketch on XY, XZ or YZ, from either side, with an offset. A sketch on a face or a datum plane comes off it. In the OCL, a plane looked at from the other side is written as `plane(XZ, reversed = true)` (#618).
- Attach Sketch (Sketcher_MapSketch) puts a sketch on a datum plane or on a flat face of an extruded body. When the support comes later in the tree, the sketch moves to just after it. The sketcher now shows a sketch on a datum plane that is a standard plane with an offset (#619).

Known issues: the sketcher does not show a sketch on the face of a body yet, and you cannot move a dimension yet (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.68

You can edit the value of a dimension in the sketch, and turn constraints off and on, as in FreeCAD.

- A double click on a dimension opens its value field, with the value selected. Enter sets the value, and Esc closes the field. Edit Value in the palette opens the same field for the selected dimension. A value typed in a reference dimension makes it a driving dimension (#614).
- A worker draws the marks of the constraints, so the page does not stop while they draw (#615).
- Toggle Constraints (K, Z in the FreeCAD profile) turns the selected constraints off and on. A constraint that is off stays in the sketch, but the solver does not use it. Its mark is grey and its text is in italics. In the OCL, the word `inactive` comes after it, and a FreeCAD file keeps the state when it opens (#616).

Known issues: you cannot move a dimension yet (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.67

The sketch in edit shows the marks of its constraints and dimensions, and the Sketcher has Carbon Copy, as in FreeCAD.

- Carbon Copy (G, W in the FreeCAD profile) copies the geometry and the constraints of another sketch into the sketch in edit. A click on a sketch in the tree picks it (#610).
- The palette finds 38 Sketcher commands under their FreeCAD menu text, such as "Coincident Constraint" and "Distance Dimension". G, V and G, 3, E are now keys of the FreeCAD profile (#611).
- The sketch in edit shows a mark for each constraint. A geometric constraint shows a sign by each edge that it holds. A dimension shows its lines and its value, and a reference dimension shows its value in parentheses. A click on a mark, a point or an edge selects it, and Delete deletes the selected constraints. A large sketch draws its marks over a few idle periods, so it opens and undoes in its budgets (#612).

Known issues: a double click on a dimension does not open its value yet, and you cannot move a dimension yet (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.66

The Sketcher has Move / Array Transform and Remove Axes Alignment, as in FreeCAD.

- Move / Array Transform (W in the FreeCAD profile) moves the selected geometry, or copies it along a step and in rows. The constraints among the selected geometry come with each copy, and with equal on, each copy is equal to its curve (#606).
- The steps of Move / Array Transform take clicks, with a dashed preview of the copies. U and J add and remove a copy, and R and F add and remove a row, as in FreeCAD (#607).
- Remove Axes Alignment (Z, R) changes the constraints that hold the selection to the axes. Horizontal and vertical lines become parallel and perpendicular, horizontal and vertical distances become distances, and symmetry about an axis and points on an axis are removed. The geometry keeps its shape and can turn as one (#608).

Known issues: the app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). Carbon Copy, the B-spline tools and some other Sketcher tools are to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.65

The constraint tools and the dimension tool of the sketcher take more forms, and they take the origin and the axes of the sketch, as in FreeCAD.

- `horizontal(p, q)` and `vertical(p, q)` take two points. `symmetric(p, q, about = m)` takes a point. Perpendicular and tangent take a point and two curves, and hold the curves at the point (#602).
- Constraints and dimensions take `origin`, `hline` and `vline`, the root point and the two axes of the sketch. Sketch > Select Origin (Z, O), Select Horizontal Axis (Z, H) and Select Vertical Axis (Z, V) select them (#603).
- The dimension tool picks the origin and the axes. A click picks sketch geometry first, and the axes only when no sketch geometry is at the click (#604).

## 0.0.64

The constraint tools of the sketcher pick their geometry with clicks, as in FreeCAD, and two new Sketcher tools are in.

- Ellipse From 3 Points (G, 3, E) and Centered Rectangle are new. The ellipse tools find the second axis by the construction of de la Hire, as FreeCAD 1.1.3 does (#597).
- Sketch > Stop Operation stops the tool in use and keeps the sketch in edit. Sketch > Leave Sketch and its toolbar button stop the tool and leave the edit (#598).
- Heal joins two free edges of a shell that lie on each other, and takes out a slit when the faces beside it run the wrong way. This makes ABC 00024353, 00029314 and 00028718 valid (#599).
- The constraint tools pick points and edges with clicks. The status bar shows the hint of the next pick, and the tool stays on after each constraint. A click on empty space removes the picks. A selection made before the tool starts gets the constraint at once (#600).

Known issues: horizontal or vertical on two points, perpendicular or tangent at a point, and symmetric about a point are not in the kernel yet, so the tools show a message for them (#382). The app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.63

The web app imports a STEP file as solids, and a sketch can take an edge of the imported body as external geometry.

- A `.stp` or `.step` file dropped on the viewport comes in as an `import_step` feature with its solids. The reader takes the curves and surfaces of the corpus car bodies, and the status bar tells the solids, faces and triangles (#593).
- External Projection (G, X in the FreeCAD profile) shows the edges of the imported bodies over the sketch in edit, and a click on an edge brings it into the sketch as external geometry. The OCL names the edge by a point on it, so the link stays when the file is read again. The journey J-SK-06 projects an edge of the Windsor body and its FreeCAD twin agrees. Fit All (V, F) and the isometric view (0) now work (#594).
- The Select commands of the Sketcher (Z, F / Z, K / Z, E / Z, P, R / Z, P, C) and Align View to Sketch (Q, P) run from the FreeCAD keys, the Sketch menu and the palette (#595).

Known issues: the app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). Std_Refresh and Fit Selection are not in OpenCAD yet. Many Sketcher tools and the renderer drawing of the dimensions are to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.62

Trace Section writes the section of an imported mesh into a sketch, so you can start a body from the shape of a scanned or simulated car.

- Trace Section (Shift+T in the OpenCAD profile) cuts each shown mesh by the plane of the sketch in edit, and writes the section into the sketch as points and lines in one undo step. Each polyline is simplified to within a ten-thousandth of the size of the section. The journey J-SK-07 traces the DrivAerML car on XZ into 4 closed polylines with 366 points and pads the section 1 mm, and its FreeCAD twin agrees with the golden mass properties. A large dropped file now goes to the app in pieces of 8 MB (#591).

Known issues: the app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). Std_ViewFitAll (V, F) and the other view commands are not in OpenCAD yet. J-SK-06 is to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.61

A reference dimension shows its measured value, and a sketch of 2,000 entities selects, deletes and undoes in budget.

- A reference dimension shows the measured value after each edit of the sketch, as in FreeCAD, and the OCL keeps it (#587).
- Select All (Mod+A) selects all of the geometry of a sketch in edit, and Delete removes it with the constraints on it, in one undo step. Esc clears the selection first. The journey J-SK-08 checks Select All, Delete and Undo on the sketch S1-2000 against A-SEL-ALL-SKETCH, A-SK-GEOM and A-UNDO. Undo of the 2,000 entities went from about 201 ms to 44 ms (#588).

Known issues: the app has no Edit menu, so Std_SelectAll and Std_Delete have keys and the palette only (#589). Std_ViewFitAll (V, F) and the other view commands are not in OpenCAD yet. J-SK-06 and J-SK-07 are to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.60

Large sketches solve fast enough to drag, and a pad of a sketch with many closed shapes makes one solid for each shape.

- A drag of a point stops at the limit of the constraints, as in FreeCAD, and does not break the sketch (#582).
- The solver uses sparse steps on a large sketch, so a sketch of 500 entities solves and diagnoses in a few milliseconds (#583).
- The journey J-SK-05 drags a point of the 500-entity sketch S1 and checks the solve and the frame against A-SK-DRAG-500. The press of the button starts the drag. Shapes side by side, and a shape in the hole of another, each make a face, and a pad of them makes a solid for each face (#585).

Known issues: a reference dimension keeps its last driving value and does not show the measured value (#584). Std_ViewFitAll (V, F) and the other view commands are not in OpenCAD yet. J-SK-06 to J-SK-08 are to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.59

Fillet and chamfer in the Sketcher work on two lines that do not meet, and a new journey checks each sketch tool by its FreeCAD key.

- Fillet and chamfer take two lines that do not meet. The fillet goes in the corner where the lines would meet, on the side of the clicks, with the radius of FreeCAD's `suggestFilletRadius`. U turns "preserve corner" off and on, as in FreeCAD (#579).
- The journey J-SK-02 uses each tool of the sketcher once, by its FreeCAD key and by clicks, and checks the FreeCAD hint of each step. A FreeCAD twin and an OCCT test check the pad at the end. A click of an edit tool on a point is now at the place of that point (#580).

Known issues: Std_ViewFitAll (V, F) and the other view commands are not in OpenCAD yet. J-SK-05 to J-SK-08 are to do (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.58

The Sketcher has the B-spline by knots of FreeCAD, and Extend Edge and Split Edge work on more curves.

- OCL, the kernel and the core have the B-spline by knots (`bspline(poles, knots = [...])`). The curve goes through each knot, open or periodic. The Sketcher draws it with G, B, I and G, B, O, the menu and the toolbar (#574).
- Extend Edge works on arcs. The end of an arc moves round its circle by an angle or by a length along the arc. Extend removes the constraints on the end that moves, as FreeCAD does (#576).
- Split Edge works on circles. A circle becomes one arc that starts and ends at the click and goes all the way round, as in FreeCAD. An arc whose ends meet counts as a whole turn in the kernel, the core and the web (#577).

Known issues: fillet works only on lines that meet at a point, and the fillet tool does not have "preserve corner" off (U) (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.57

The Sketcher has the arc slot and the periodic B-spline of FreeCAD, and it can hold a line perpendicular to a circle or an arc.

- The solver can hold a line perpendicular to a circle or an arc, so the line points at the center. The kernel, OCL and the core have the arc slot as four arcs, with round or flat ends (#570).
- The Sketcher draws an arc slot with G, S, 2, the menu and the toolbar. M switches between round and flat ends, as in FreeCAD. The palette form of `sketch.arc_slot` takes the center, the radius, the angles and the width (#571).
- OCL, the kernel and the core have the periodic B-spline (`bspline(poles, periodic = true)`). The B-spline closes on itself and bounds a region alone. The Sketcher draws it with G, B, P, the menu and the toolbar (#572).
- The lockfiles of the desktop app and the web core and viewport have the version of the workspace again (#570).

Known issues: the sketcher does not have the B-spline by knots yet, and extend and fillet work on lines only (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.56

The Sketcher has the FreeCAD tools for the arcs of an ellipse, a hyperbola and a parabola, with the FreeCAD steps and hints.

- The solver, the kernel, OCL and the core have the arc of an ellipse (`ellipse_arc`). The kernel makes its edge with OCCT, and a radius dimension on it sets the minor radius (#565).
- The Sketcher draws an arc of an ellipse with G, E, A, the menu and the toolbar. The arc follows the pointer round the ellipse, clockwise or counterclockwise, as in FreeCAD (#566).
- The solver, the kernel, OCL and the core have the arcs of a hyperbola (`hyperbola_arc`) and of a parabola (`parabola_arc`). The kernel makes each as an exact conic. The solver keeps all rows of one entity in one block, which fixes a panic for an arc of a parabola with a fixed focus and vertex (#567).
- The Sketcher draws an arc of a hyperbola with G, H and an arc of a parabola with G, J, as in FreeCAD (#568).

Known issues: the sketcher does not have the arc slot, the periodic B-spline or the B-spline by knots yet, and extend and fillet work on lines only (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.55

The Sketcher draws B-splines by their control points, and it has the FreeCAD edit tools fillet, chamfer, trim, extend and split, which you use with clicks.

- The Sketcher draws a B-spline by its control points with G, B, B, the menu and the toolbar. U and J change the degree, and a right click or Esc ends the B-spline, as in FreeCAD (#561).
- Fillet (G, F, F), chamfer, trim (G, T), extend and split work with clicks and show the FreeCAD 1.1 hints. A fillet of a corner point or of two lines that meet gets the radius that FreeCAD gives. M switches between fillet and chamfer (#562).
- Trim works on circles and arcs too. A circle that two curves cross becomes the arc that is left, and a piece from the middle of an arc leaves two arcs on one circle. The `sketch.trim` command now takes `curve` and not `line` (#563).

Known issues: extend and fillet work on lines only, and the sketcher does not have the arc of ellipse yet (#382). The WebGPU device is still lost with SwiftShader on Linux under high load (#359).

## 0.0.54

The Sketcher has the FreeCAD Lock, Block and Symmetric constraints and the root point snap. The solver messages name the same constraints as FreeCAD, and four new journeys compare the Sketcher with a FreeCAD twin.

- The Sketcher has Lock (K, L), Block (K, B) and Symmetric (S). A horizontal or vertical distance on one point measures from the sketch origin, as in FreeCAD. A move, rotate, scale or mirror of a locked point keeps the lock true (#554).
- A click near the root point snaps to it, and the Sketcher draws the H and V axes. A dimension writes the solved coordinates of the points that it moves to the text (J-FC-02) (#553).
- K, S and the other dimension keys of FreeCAD open the dimension tool with the measure of the key. This also applies to the menu and the toolbar (J-SK-03) (#556).
- The redundant message names the constraint that FreeCAD names: of each redundant group, the constraint in the most groups, and of those the newest (J-SK-04) (#557).
- The link of the degrees of freedom selects the curves that FreeCAD's Select Under-Constrained Elements selects. J-FC-03 checks this after each step (#559).
- The journey twins compare the constraint types, the degrees of freedom, the faults and the freedom after each step of a sketch with FreeCAD 1.1.3 (#556, #557, #559).
- A lost WebGPU device is written to the console with its cause, and the viewport does not trap on it (#555).
- Heal trims a periodic circle or ellipse pcurve to its edge. It does this only on a face that fails BRepCheck, and keeps the change only if the face then passes (#558).

Known issues: the WebGPU device is still lost with SwiftShader on Linux under high load (#359). J-UI-01 failed one time for I16 after a layout reset in a full run, and 8 repeats passed (#536).

## 0.0.53

The Sketcher now has the contextual dimension and the FreeCAD solver messages and colours, and it shows the Sketcher toolset during an edit. Many STL files import as one step, and Pocket and Groove are commands of their own.

- The Sketcher has the FreeCAD contextual dimension on D. It proposes a dimension from what you pick, M changes the proposal, and you type the value in the field at the dimension. During the edit of a sketch, the toolbar shows the Sketcher toolset (#546).
- The Sketcher task panel shows the FreeCAD solver messages. A link in a message selects the constraints or the elements that it names. The geometry has the FreeCAD colours of its solver state, and the theme has new `sketch.fc.*` colour tokens for them (#548).
- A drop of many STL files is one step in the history, with progress and Cancel, so one undo removes all of the files (J-IMP-06) (#541).
- Pocket and Groove are commands of their own, with the FreeCAD task panel fields. A blind pocket can go the other way with Reversed (#542).
- The palette uses the workspace of the active toolset, so a FreeCAD menu text finds the command of the workbench that you are in. OCL keeps the sketches in the file order (#545).
- A document tab without the core's document clears the selection (I1) (#547).
- These changes remove some of the double React commits in one frame (I16): at the first open, on a menu hover, and for a tooltip that opens in the store flush (#544, #549). A selection that draws the same image draws no new frame (#535).
- In a small window, the command search gets smaller first, so the FreeCAD top bar fits at 1024 px (#540).
- Heal bounds a face at the pole of its vertex loop (#537).
- The web build builds the design tokens first (#550). The gallery goldens, the J-IMP-07 golden and the I8 planted patch are up to date (#549, #551). Tests that do not measure the stand-in model open 100 parts (#539).

Known issues: J-UI-06 failed one time for I16 at the start of a sketch in a full run under load. In a second run it passed 16 of 16 times.

## 0.0.52

The viewport now shows the solids that you model, the core keeps expressions, and the FreeCAD profile has the Part Design and Part menus and toolbars. The Sketcher has the other FreeCAD geometry tools, and Pad has a length dragger.

- The web app and the desktop app build the bodies of the model with OCCT and draw them. The desktop app builds them in its own process. A feature that the kernel cannot build gets an error mark in the tree (#523, #525).
- An FCStd file opens with its shapes. The app reads the OCCT BREP shapes in the file and meshes the visible objects (#521, #526).
- The core keeps an expression on a feature parameter and computes it again in dependency order after each change. One undo puts back the expression and the values. A cycle is refused with an error that names the loop. OCL writes an expression as `@expr`, and the FCStd reader reads simple FreeCAD expressions (#524).
- The FreeCAD profile has the menus and the toolbars of Part Design and Part, in the FreeCAD order. A command that OpenCAD does not have yet stays in its place, disabled, with a tooltip that says so (#528).
- The Sketcher has arc and circle by 3 points, ellipse, centered and rounded rectangle, regular polygon and slot, with the FreeCAD steps and keys. Construction mode is on G, N. A point drawn near a curve goes on it, and the sketcher proposes tangent and perpendicular (#530).
- The Pad panel has a length dragger in the 3D view. You can type a value during the drag. Enter confirms it and Esc gives back the old value. The drag snaps to the grid, and Alt turns the snap off (#531).
- Heal builds a face again from its edges when its wires touch at a vertex, and it cuts a periodic pcurve down to the range of its edge (#522, #529).
- The address can set the parts of the stand-in model with `?parts=N`. J-UI-01 and J-UI-04 use 100 parts, so the viewport becomes idle in time on a software GPU (#532). The planted bugs apply again, and the oracle catches I1 (#527).

Known issues: invariant I16 (one React commit in a frame) fails now and then at the first open and in some J-UI-01 steps (#533). The props spec is slow on a software GPU with the 5,000 part stand-in.

## 0.0.51

The web app now opens FreeCAD FCStd files and has the first part of the Sketcher. Two features can no longer have the same name.

- Open a FreeCAD FCStd file with File, Open (Ctrl+O), from Open Recent or with a drop on the viewport. The app reads the FreeCAD tree, writes OCL for the sketches, Pad, Pocket, Revolution, Groove, the primitives and the Part booleans, and keeps the FreeCAD names. The Report view has one line for each FreeCAD object with what came of it. The shapes in the file do not load yet, because there is no BREP reader (#511). The FCStd reader has a fuzz target (#508).
- The Sketcher has the FreeCAD core drawing tools: point, line, polyline, arc, circle and rectangle, with the FreeCAD keys. While you draw, it proposes horizontal and vertical within 2 degrees, and coincident within 8 pixels of a point. The task panel shows the FreeCAD solver messages, and the geometry has the colour of its solver state (#517).
- A new feature that gets a name in use gets the next free name, as in FreeCAD, so a second Sketch is Sketch001. A rename to a name in use is refused. Before this change, a write and a read of the OCL lost one of the two features (#519).
- Heal removes degenerate edges and faces that do no work after the ShapeFix pass (#507).

Known issues: the viewport shows a stand-in scene and not the solids that you model (#518). The core does not keep expressions yet (#504).

## 0.0.50

The web app now has a property editor with the FreeCAD property names, and the desktop app sends its pointer input to the renderer in one binary batch per frame. This release also fixes two causes of double React commits in one frame, and it makes the planted bug harness and J-SK-01 work again.

- The property editor has Data and View tabs. In the FreeCAD profile it uses the FreeCAD groups and names, for example Length, Length2 and Midplane on a Pad. An edit is one command, also for many selected features, so one undo takes it back. Units work as in FreeCAD, and `=` opens the expression editor with name completion. The core does not keep expressions yet (#504). A sketch with 400 properties opens in 1.8 ms, and 1,000 selected features open in 12.2 ms (#502).
- The desktop app collects pointer and wheel events with `getCoalescedEvents` and sends one binary batch per frame to the renderer thread. The renderer sends back hover ids, dragger values and anchors once per frame, and the overlays use the anchor of the frame on the screen. The desktop CSP now lets the Tauri IPC protocol through, so commands no longer use the slow JSON fallback (#500).
- The box selection corner and the tooltip timer now change in the store flush of the frame, so React makes one commit per frame (I16) (#503).
- The oracle session fills the whole form, so the planted I4 bug shows a preview and the oracle detects it (#499). The selection spec clicks on points of the parts that the test channel gives (#496).
- The journey drive can read the value of a form field with `waitValue`. J-SK-01 uses it to read the depth in the property editor (#505).
- `cargo fmt --check` passes again with the pinned toolchain (#501).

Known issues: a second STL import moves in front of a sketch (#494). At a high machine load, specs that pick a lot can time out while they wait for the viewport (#480).

## 0.0.49

The web app now has the 12 FreeCAD navigation styles, a real model tree, pick and selection that follow FreeCAD, a command machine with generated task panels, docks with saved layouts, and a component gallery with visual tests. The test harness now runs the oracle in the UI fuzz and soak, and the perf gate is calibrated with four planted delays.

- The core sends the tree rows, the problems of each feature and the selection. A release build uses a binary form of the reply: for one change in a model of 1,000 features, the reply has 80 bytes against 248 bytes of JSON. The feature tree marks a suppressed feature and a feature with a warning (#469).
- The viewport has the 12 navigation styles of FreeCAD 1.1.3. A nav event that changes nothing draws no frame, and text keys do not go to the styles (#474, #478).
- The model tree nests each sketch under its feature, shows an icon and a state mark for each row, has a working context menu, and Space hides and shows the rows that you select (#476).
- The viewport highlights the preselection and the selection in the colours of FreeCAD. Selection follows FreeCAD: Ctrl+click in the FreeCAD profile, Shift+click in the OpenCAD profile, box selection, clarify selection, Tab over the parts under the pointer, and the selection filters. The app sends the selection to the core as one list (#481).
- A command now runs in a state machine, and its task panel is made from the arguments of the command (#482). The command palette resets its active row in the commit of the key, so it gives one React commit in a frame (#483).
- The docks have tabs and splits. A panel can float or go to a window on another screen. There is a FreeCAD layout and an OpenCAD layout, and the app keeps a layout for each profile and each screen setup (#490).
- The design tokens have the additions of spec ui/03 in the dark, light, dark-hc and light-hc themes. The `?gallery` page shows each component in each state, in 4 themes and 3 densities, and 12 FLIP goldens test it. A contrast test checks each pair of a text colour and its background in each theme. The gallery found that a pressed solid button and a hovered or pressed danger button had the wrong colours, and these are fixed (#487, #492).
- STEP: a new tight read keeps the fixes of the reader to 0.01 mm (#472).
- Harness: I14 does not count the frames of a test readback, and it checks each long frame once (#473, #477). The journeys, planted and the perf gate take a free port for each run (#470, #471). The timing tests run alone after the other journeys (#479). When `waitIdle` gives up, the error names the condition that is not idle (#488).
- Harness: the UI fuzz and the soak run the oracle after each action and each sample, with `cargo xtask ui-fuzz` and `ui-soak` (#493). The perf gate plants all four delays, and a calibration flagged each delay in 5 of 5 blocks (#491). The planted patches apply to the current tree again, and a test checks that each patch applies (#477, #485, #495).

Known issues: a second import of the same STL file moves in front of a sketch when the OCL is printed again (#494). Some journeys fail I16 with two commits in a frame (#486), and J-UI-01 does not pass after #481 and #482 (#489).

## 0.0.48

This release brings back the model store of 0.0.47. The merge of #464 removed it from main by mistake, so the 0.0.47 build did not have it.

- The model store of #463 is back, with the I16 check and its planted bug. The FreeCAD and OpenCAD key profiles of #464 stay (#467).
- The sketch in edit is now in the store, so a new sketch gives one React commit in its frame (#467).
- The import of a FreeCAD `user.cfg` now updates the app in one commit. Before, J-UI-05 failed I16 in 3 of 12 runs (#467).

## 0.0.47

Note: the build of this release does not have the model store. The merge of #464 removed it from main by mistake, and 0.0.48 brings it back.

The model of the web app is now in a store outside React, and a frame gives one React commit. The app has two key profiles, FreeCAD and OpenCAD, with a chord engine and the import of a FreeCAD `user.cfg`.

- `@opencad/ui/store` has a `Store` with no React import. It applies all the changes of a frame in one flush, so a frame gives one commit. The model is normalized, and a change keeps each feature object that did not change. The feature tree, the Properties panel, the code panel and the status bar read the store with selectors, so a reply that changes only the OCL does not render the tree. The new invariant I16 fails a journey when React commits more than once in a frame. In the full journey run, 41 tests made 1,488 commits in 1,054 frames, and no frame had more than 1. See docs/apps/web.md and docs/contributing/invariants.md (#463).
- The feature tree no longer commits a second time when it mounts and when the selection changes (#463).
- The first run asks "Which CAD tool do you know best?" and sets the FreeCAD or the OpenCAD profile. The FreeCAD profile has the 204 default keys of FreeCAD 1.1.3, in the global, viewport, sketch, assembly, TechDraw and spreadsheet scopes. A key of a command that OpenCAD does not have yet tells you so in the status bar. See docs/apps/keys.md (#464).
- The chord engine waits 800 ms for each next key, and the status bar shows the keys so far. New keys in both profiles: Mod+K and `/` open search, Mod+Shift+Space repeats the last command, and M, M measures a distance (#464).
- Keys > Import user.cfg reads the keys of a FreeCAD `user.cfg` and shows a report. You can also drop the file on the viewport. The keymap lint fails when one key runs two things in one scope (#464).

## 0.0.46

The core now sends a small numbered delta with each reply, not the whole document. CI caches the two corpora, and it keeps files that we may not pass on out of its artifacts. Heal repairs a face whose surface of revolution passes through its axis.

- `ocad-host` keeps a revision number per document. Each reply has `rev` and a `delta` with the features that were added, changed or removed and the undo state. `{"op": "snapshot"}` gives the whole document, and `{"op": "ocl"}` or `"with_ocl": true` gives the OCL. The old `state` op is gone. A request can carry `id` and `trace`, and the reply gives both back. One change on a model of 1,000 features gives a reply of 248 bytes, against 93,902 bytes for the snapshot. The web shell asks for the OCL only while the code panel is open. See docs/contributing/tracing.md (#461).
- `Mirror` in `@opencad/ui/store` applies the deltas in order. When a revision is missing, it drops its copy and asks for a snapshot (#461).
- CI checks out opencad-corpus and opencad-compat and caches their files, keyed by the hash of the lock files and the manifests. `cargo xtask corpus fetch --max-mb 50` gets the files that the journeys name, and tier T1 uses only files under 50 MB. See docs/contributing/ci-tiers.md (#460).
- A corpus file from a dataset with `pass_on = false`, such as ModelNet40, blurs the viewport during the test, and `cargo xtask corpus scrub` removes the results of that test before CI uploads its artifacts. See docs/contributing/journeys.md (#460).
- `heal` trims a surface of revolution whose profile crosses the axis inside its range, so ShapeFix closes the wire at the pole. ABC 00020489 is now valid, with 1 solid and 14 faces (#459).

## 0.0.45

A fillet or a chamfer can now go on the body after later features, as in FreeCAD. A sketch refuses two entities with the same name. The STEP reader keeps more files valid.

- `fillet` and `chamfer` take `into`, the same as the patterns. With `into`, the dress-up goes on that body, so the features after its edge stay in the result. Without `into`, nothing changes. See docs/modeling/dress-up.md (#455, #457).
- A sketch with two entities of the same name fails with OCL0203, "`b` is defined twice in the same scope". Before, the second entity hid the first, and the build reported degrees of freedom that the file seems to hold (#453).
- `read_step` turns on the OCCT reader's same parameter fix. A band with a circle at each end and no seam edge, as in ABC 00020224, no longer splits into two open faces. On 500 ABC files, two more files are valid (#454).
- `read_step` heals at 1e-7 mm instead of the precision the file declares. ABC 00020060 keeps 108 small solids that healing at 1e-3 mm threw away (#456).

## 0.0.44

The web app imports an STL file and shows its mesh in the viewport. A test can now open a file of the corpus by its id, and the drive checks the SHA-256 of the file first. Forge evaluates rational splines with short spans more exactly.

- Drop an STL file on the viewport, press Mod+I, or run Import from the palette. The model gets the feature `name = import_stl(blob("sha256:..."))`, which names the file by the hash of its bytes. The core worker reads the file and sends the mesh to the viewport worker directly, and the first mesh replaces the stand-in scene. The status bar gives the triangles and the open edges. Undo and redo take the mesh out and bring it back. See docs/apps/web.md (#449).
- `oc.open("corpus:<set>/<path>")` and `oc.open("compat:<id>")` check the size and the SHA-256 of the file against the lock file or the manifest before the app loads, then drop the file on the viewport. See docs/contributing/journeys.md (#449).
- The status bar counts the parts that the viewport draws after an import, such as "1 part" (#451).
- Forge keeps the rounding of the weighted control points out of the derivatives of a rational curve or surface. On surface 12519 of ABC file 00024359, the error went from 2.4e-8 to less than 1e-13 (#450).

## 0.0.43

Agents now have their own rules file, and CI checks the ways an agent can make a test pass with no fix. A pull request that changes the UI gets evidence that the harness writes. Large shapes heal faster.

- `AGENTS.md` at the root gives the rules for an AI agent that changes OpenCAD. `.github/CODEOWNERS` names the files with a budget or a threshold, and a change to them needs the review of a person. `cargo xtask agents` checks the two files (#441).
- `cargo xtask ui-lint` checks the TypeScript of the UI for a fixed wait, `any`, `@ts-ignore`, a dropped error, `.only`, `.skip`, a model edit outside `apps/web/src/commands/` and a literal where a token goes. See docs/contributing/ui-lint.md (#442).
- The standard heal pass skips the search for self-intersecting edges on a shape with 5,000 faces or more. On DrivAerML run 12, the pass took 60 s, against 1,626 s before (#443).
- `cargo xtask weakened` flags each change that makes a test weaker: a removed assertion, a tolerance that grows, a removed journey step, a new `#[ignore]`, a new allow comment, and a golden that changes in the same commit as its code. A flagged pull request needs the approval of a person. See docs/contributing/weakened-tests.md (#444, #445).
- `cargo xtask evidence` writes the UI evidence of a pull request: the screenshots of each journey, the test results, the test that fails on the base build, the corpus ids, the perf gate table and each changed budget and golden. The pull request template has a UI evidence section. See docs/contributing/ui-evidence.md (#447).

## 0.0.42

A failed test now saves what you need to find the cause, a flaky test gets an issue, and the CI tiers T0 to T3 run within their time limits.

- When a browser test fails, the kit saves the trace, a video, the viewport readback, a screenshot of the window, the store, the OCL, the machine states, the selection, the console log, the Rust log, the seed and the steps in the folder of the test. See docs/contributing/journeys.md (#437).
- `cargo xtask journey --rerun` runs each failed test again on the same build and writes an issue for each test that passes then. On main, CI opens that issue with the `flaky` label (#438).
- `tests/quarantine.toml` holds a flaky test with an issue and an owner for 14 days, and then the test blocks again. `cargo xtask quarantine` checks the file and that retries stay off. See docs/contributing/flaky-tests.md (#438).
- `cargo xtask tier T0` to `T3` runs the steps of a CI tier and fails when the tier goes over its limit. CI runs T0 on each push, T1 on each pull request, T2 on each merge to main, and T3 each night. See docs/contributing/ci-tiers.md (#439).

## 0.0.41

Each pull request now runs a perf gate that compares the base and the head, and the gate has a weekly calibration and a nightly trend.

- `cargo xtask perf-gate` builds the base and the head commit, runs the same workload on the two in blocks of A, B, B, A, and fails when an action is slower by 8 ms, or by 5% for a commit, in at least 9 of 10 blocks. The pull request gets the table. A `budget-change` label with a human approval lets a slower action merge. See docs/contributing/perf-gate.md (#433).
- `cargo xtask perf-calibrate` and a weekly workflow plant a 16 ms, a 32 ms and a 64 ms delay in main and check that the gate flags each one (#434).
- `cargo xtask perf-nightly` and a nightly job keep the time of each action for each night, and fail on a step or a slow drift over the last 14 nights (#435).

## 0.0.40

The web app has an invariant oracle that checks each step of a test, and each user action has a trace span that Perfetto opens.

- oc-drive checks 15 invariants after each step of a journey, such as that the tree is the order of the core, that undo gives back the state before, and that no frame of the page is over its budget. `cargo xtask planted` plants one bug for each invariant and checks that the oracle finds it. See docs/contributing/invariants.md (#428).
- Each user action of the spec ui/08 catalogue has a span with an action id and a trace id. The span closes at the frame that shows the result. The spans of the core join the trace of the action that sent the request (#430).
- A test build writes one Chrome trace for each test as `spans.json`, with the actions, the input events, the long frames, the core spans and the viewport frames. Perfetto opens it. See docs/contributing/tracing.md (#430).
- The trace has the GPU time of each render pass of the viewport as a counter (#431).
- Build the desktop app with the `tracy` feature to send the spans of the core and the renderer to the Tracy profiler 0.14.1 (#431).
- An imported shape that is valid but has tolerances larger than it needs now gets tighter tolerances when the healed shape still passes the check (#429).

## 0.0.39

The web viewport draws in a worker of its own, and you can turn and zoom the view. The black box tool for FreeCAD and OpenCAD has a guide.

- The web viewport runs in a worker on an OffscreenCanvas. The page sends the pointer and wheel input once a frame, so a slow frame does not stop the panels and a busy panel does not stop the view. In a traced 2 s drag on an M4 Mac, no main thread task is longer than 16 ms. See docs/apps/web.md (#426).
- Drag in the web viewport with the left button to turn the view, and turn the wheel to zoom (#426).
- On the web, the `pickAt`, `project` and `readback` calls of the test channel give a promise. oc-drive waits for each call, so the journeys do not change (#426).
- Each journey is a folder in `tests/journeys/<id>/` with its steps, its golden OCL and values, a FreeCAD twin in `freecad.py` and its time budgets. J-SK-01 is in this format. `cargo xtask journey` checks each folder, runs the FreeCAD twin, runs the browser tests and checks the budgets. See docs/contributing/journeys.md (#424).
- docs/contributing/blackbox.md tells how to time an action in FreeCAD and OpenCAD with `oc-blackbox` from opencad-bench, and how to write a session script for it (#425).

## 0.0.38

One journey file now runs in the browser and in the desktop app, through the new oc-drive package.

- `packages/oc-drive` gives a journey one API: `oc.command` runs a command by its FreeCAD name through the palette and fills its form, `oc.click` clicks an entity where the test channel projects it, and `oc.panel` gives the panels from the panel registry. See docs/contributing/journeys.md (#420).
- oc-drive has three adapters: Playwright for the browser, WebdriverIO for the desktop app, and CDP for a browser or WebView2 with a debug port (#420, #421, #422).
- J-SK-01 is now one file, `tests/journeys/j-sk-01/journey.ts`, and the browser test and the desktop runner both call it (#420).
- A test build of the desktop app has an embedded WebDriver server. `cargo xtask desktop-journey` runs the journey files in the app through it, and `--cdp` runs them over CDP on WebView2 on Windows (#421, #422).

## 0.0.37

Test builds start in deterministic mode, in the browser and in the desktop app, and the UI fonts come with the app.

- The app bundles the Inter and JetBrains Mono fonts, so the UI looks the same on each system and needs no network to load them (#417).
- A test build starts in deterministic mode: one seed for `Math.random`, a test clock that starts at 2026-01-01, no motion, no blinking text cursor, only the bundled fonts, the dark theme and en-US. The `settings`, `reset` and `advanceClock` calls of the test channel read and change it. See docs/contributing/test-channel.md (#417).
- Each journey gets its seed from its file and title, and the report shows it. Set `OC_SEED` to run all journeys with one seed (#417).
- The journeys run in three projects: `pixel` and `pixel-2x` on SwiftShader, and `timing` on the machine GPU. `cargo xtask journey --repeat N` runs one journey N times and checks that each run gives the same store, OCL and pixels (#417).
- The desktop app gets the seed from `OPENCAD_TEST_SEED`. `OPENCAD_GPU=software` makes it draw on lavapipe on Linux or WARP on Windows. `cargo xtask desktop-journey` checks the settings and then starts the app 20 more times to compare what each start gives (#418).

## 0.0.36

A test channel that lets a test see the 3D view, in the browser and in the desktop app, and preselection in the status bar.

- A test build of the web app has a test channel on `window.__ocTest`. A test reads the camera, the selection in the store, the viewport, the tree and the Properties panel, the pixels of the frame with their SHA-256, the OCL and the errors, and it can wait for the app to be idle. Build it with `VITE_TESTKIT=1`. See docs/contributing/test-channel.md (#412).
- A test build of the desktop app (`--features testkit`) has the same channel on a WebSocket on 127.0.0.1. The port goes in the file that `OPENCAD_TESTKIT_PORT_FILE` names. The native viewport answers the camera, pick, project, readback and frame calls. `cargo xtask desktop-journey` builds the app, starts it and checks these calls (#413).
- CI fails if a release desktop binary or the release web build has the test channel in it. Run the check with `cargo xtask testkit-check --binary PATH` (#413).
- The browser journeys run on SwiftShader when there is no GPU, or when `OC_GPU=swiftshader` is set, so they can run on any CI machine (#412).
- J-SK-01 checks the selection in all four places, the camera and the pixels through the channel (#412).
- The Properties panel shows the feature that is selected in the feature tree: its name, its command and each parameter (#412).
- The pointer over a part in the viewport preselects it, and the status bar says "Preselected: Part 4,297". The part is not highlighted yet (#415).
- The renderer can read back a region of the frame without a wait, project a point to the window, and count the GPU memory of its buffers and frame textures (#410, #413, #415).
- OCCT healing sews at 0.01 mm first, and when that makes no solid, it does not try the tighter tolerances. `Occt::sew` is now public, so a caller can sew without healing (#411, #414).

## 0.0.35

A UI coverage gate for each FreeCAD command, correctly rounded math, and the UI spec.

- `spec/ui/` says how the interface is built and how we prove that it works: FreeCAD continuity, the 10x speed program, the test harness and real data (#356).
- Each command in the manifest names the FreeCAD 1.1.3 commands that it does the work of, the FreeCAD task panel fields and object properties that it covers, and the journeys that run it. `commands.json` exports these in the `ui` block (#406, #408).
- The `ui coverage` CI job gives each FreeCAD command a level from L0 to L4 and fails a pull request that lowers the count at a level. Today 117 FreeCAD commands are at L1 or above. Run it with `cargo xtask ui-coverage`, and see docs/contributing/ui-coverage.md (#408).
- The new crate `ocad-libm` gives the sketch solver and OCL correctly rounded `sin`, `atan2`, `hypot`, `exp`, `pow` and the other functions they use (#407).
- `sew_loose` does not keep a sewing that grows the largest tolerance past 0.01 mm, or past the tolerance that the shape came in with (#354).
- A test pins how the STEP reader takes out a knot that is repeated past the degree, so an OCCT upgrade cannot change it without a test failure (#355).

## 0.0.34

Welding meshes, faster reads of large STEP files, and real car models in the docs.

- `opencadd mesh --weld` merges corners at the same point and takes out triangles with no area or repeated, so a mesh stored as loose triangles can be checked and harmonized. On a ModelNet40 car it takes open edges from 1,062,662 to 111,984 (#351).
- A STEP file of 50 MB or more is read without the OCCT reader's search for edges that cross themselves. A 226 MB DrivAerML car now reads in 67 s, where before it ran past 30 minutes (#352).
- docs/files/formats.md has what the readers make of the AhmedML, WindsorML and DrivAerML car models (#350).

## 0.0.33

Sketches that say how free they are, reference dimensions, and a stricter OCL checker.

- `opencadd build` prints, for each sketch, whether it is fully constrained or how many degrees of freedom are left and in which entities, which constraints say the same thing, and a warning when the sketch has collapsed (#344).
- A `ref dim` measures instead of driving. The build prints what it measures, and what it was written as when the two differ (#345).
- A fillet can pick the edge swept from a corner made of points brought in from another body, and an arc past a half turn comes into a sketch through `intersect`. Both already worked, and now have tests and docs (#346).
- A file has to start with `ocl 1.0`. Without it, or with another version, it is error OCL0003 (#347).
- A selector the kernel does not build, such as `body.faces.endcap`, is warning OCL0209 before the build, with a suggestion when it looks like a typo (#348).
- The desktop app picks a part when you click it and shows the renderer in the status bar (#343).
- The STL reader skips the color line that CFD tools write after each `solid`, so the DrivAer meshes read (#342).

## 0.0.32

Curvature continuous fillets, surface commands in the palette, and clearer errors.

- `fillet(..., profile = curvature_continuous)` rounds a straight edge between two flat faces with a blend whose curvature runs smoothly from zero on each face. It cuts a convex edge and fills a concave one, and leaves each face where a circular fillet of the same radius would (#333).
- The palette has `part.fill`, `part.extend_face`, `part.project_on`, `part.patch` and `part.cross_sections`. Each one writes the same feature as its OCL call (#339, #340).
- A sketch that cannot solve now names the constraints that cannot all hold, written as OCL, and a constraint given the wrong kind of entity names that entity (#337, #338).
- `opencadd build` prints each measurement and the result of each `requires` check, and exits 1 when a check fails (#336).
- `ocl fmt` keeps the worst exit status over several files and refuses flags it does not know. The checker lists the options when a bare name is not one of them (#334, #335).
- opencad-compat stands at 523 missing, 82 partial and 60 done, with the surface rows citing the new command test.

## 0.0.31

Sheets you can thicken, sweep and round.

- `thicken(sheet, t, side)` turns a sheet, or a sketch's region, into a solid wall `t` thick, on the inside, the outside or half on each side (#330).
- `sweep_surface(profile, path)` sweeps a sketch's curves, open or closed, along another sketch into a sheet with no caps. A line gives a strip and a closed outline gives a duct open at both ends (#331).
- `fillet_surface(sheet, edges, r)` rounds the creases of a sheet. `edges.lateral(s.a)` now finds the crease a sweep makes from corner `a` of its profile (#331).
- opencad-compat now stands at 523 missing, 82 partial and 60 done, with Part_Sweep citing the open profile sweep.

## 0.0.30

Sketch offsets and trims, and the hole feature.

- `offset2d(curves, by, cap)` copies a chain of lines and arcs, or a circle, at a distance in a sketch, held there as the curves move. Corners get a round cap, a flat one, or with `cap = intersection` the offsets carried on until they meet. `cap = arc` is FreeCAD's name for the round cap. `close_ends` now closes both ends of two open curves (#323, #324, #327).
- `trim(curve, by, keep)` cuts a line or arc back to where other curves cross it, keeping the start, the end or the part between, and holds each cut end on the curve that crosses there (#325).
- `hole(...)` drills into a flat face at each position: simple, clearance, counterbore, countersink or tapped, blind or through. `M3` to `M20` take their diameters from the ISO 273 clearance, tap drill, DIN 974-1 counterbore and ISO 10642 countersink tables, and a hole patterns like any cut (#328).
- opencad-compat now stands at 523 missing, 82 partial and 60 done.

## 0.0.29

Sketch helpers, sketch fillets, conics, and more chamfers.

- A loft can take a sketch's region as a section, as in `[a.region, b]` (#317).
- `chamfer` takes `distances(a, b)` for two setbacks and `angled(d, angle)` for a setback and an angle, as well as one distance (#318).
- `rect`, `polyline`, `polar` and `arc3` build in sketches. A rectangle or polyline names its corners `p1` on and its sides `e1` on, as in `box.r.p3`, and those names work in `edges.lateral` and `faces.lateral`. `hline` and `vline` are the sketch axes for angle dimensions (#319).
- `fillet2d(corner, r)` rounds the corner of two lines or arcs and keeps the corner point for dimensions, and `close_ends(a, b)` joins the open ends of two curves. Every curve now has its `start`, `end` and `center` points by name (#320).
- `conic(start, apex, end, rho)` draws an arc of an ellipse, parabola or hyperbola from its ends, the meeting point of its end tangents, and a rho (#321).
- opencad-compat now stands at 523 missing, 83 partial and 59 done.

## 0.0.28

Surface tools, user guides, and extrudes that find their own depth.

- A loft with `solid = false` is the sheet of its sides, with no caps (#303).
- `extend_face(face, by)` grows a face, or a sheet of one face, past each of its open sides (#304).
- `project_on(what, onto, along)` lays the outline of a sketch's region, or the edges of a body, onto a body or some of its faces, along a direction or along the normals of the faces (#305).
- `patch(edges, style)` spans two, three or four sides with a face, as a stretched, Coons or curved filling. Sides that meet smoothly, such as a curve the seam of a sphere splits in two, count as one (#307).
- User guides under `docs/`, from installing to the OCL language, sketches, every modeling feature, measurements, file formats, mesh repair, `opencadd` and the apps. Each OCL example in them is built on OCCT in the tests, so a guide that goes out of date fails the build. #306 tracks the guides, and the gaps the writers found are filed as #309 to #312 (#308).
- The `ZX` plane, the XZ plane seen from the front with its normal along +Y (#313).
- The `through_all` and `up_to(target, offset)` extents on extrudes. `through_all` goes right through the body in `into`, on both sides of the sketch, and `up_to` goes to a flat face or datum plane parallel to the sketch, or a datum point. `op = intersect` works on every feature that takes `into` (#314).
- `draft` and `direction` on extrudes. A drafted extrude narrows as it leaves the sketch, or widens below zero, and `direction` sends a blind extrude along another axis (#315).
- opencad-compat now stands at 525 missing, 81 partial and 59 done.

## 0.0.27

External geometry in sketches, mesh repair and decimation, and surface fill.

- A sketch can take in edges of bodies. `project(edges)` lays lines, circles and arcs flat on the sketch plane, and `intersect(body)` takes the curves where the plane cuts a body. They come in as fixed construction geometry, or as profile geometry when marked `defining`, and their points (`ext.start`, `ext.end`, `ext.center`) can be constrained and dimensioned against (#299).
- `Mesh::harmonize` turns stray triangles back and closed pieces outward, `Mesh::flip` turns them all over, and `Mesh::decimate` collapses edges by quadric error down to a triangle count or a tolerance, without folding or pinching the surface. `opencadd mesh` runs these on STL, OBJ, OFF or PLY files without needing OCCT. On an Apple M4 a 408,320-triangle sphere thins to 4,000 triangles in 1.14 s and stays closed (#300).
- `fill(boundary, through = [...])` spans the outline of a sketch's region, or each open loop of a sheet, with a smooth face through any points given (#301).

## 0.0.26

Booleans with several tools, defeaturing, datums and sketches on faces, and mesh export.

- `boolean` takes a `tools` list and folds each tool into the body in turn, and `body.boolean` does the same from a command (#294).
- `defeature(body, faces)` and `part.defeature` take a hole, a boss or a fillet out of a body and close the gap. A face that OCCT cannot remove is now an error instead of handing back the body unchanged (#295).
- `datum_plane`, `datum_line` and `datum_point` are features. A plane sits on a standard plane, a datum or a flat face with an offset, goes through three points, or is normal to a line. A line goes through two points or along an edge or the axis of a round face. A sketch can sit `on` a datum plane or a flat face of a body, and `sketch.new_on` starts one there (#296).
- `ocad_io::mesh` reads and writes OBJ, OFF and PLY next to STL, welds corners, and counts open and non-manifold edges. `opencadd build` (with the `occt` feature) builds an OCL model and writes a body as a mesh. The J-SK-01 fillet body meshes closed and comes within 0.02 mm³ of its exact volume at 0.0001 mm (#297).
- opencad-compat marks the datum rows, the attachment engine, Sketcher_MapSketch and four mesh rows partial. The table is now 538 missing, 68 partial and 59 done.

## 0.0.25

B-spline curves in sketches, suppressed features, and measurements that live in the model.

- `bspline(poles, degree)` and `sketch.bspline` add a clamped B-spline by its control points that closes regions and sweeps like any other edge. Volumes of bodies swept from a spline of several spans are now measured on a NURBS copy, since OCCT does not split the swept surface at the spline's knots (#289).
- Any feature takes `suppressed = true`, and `feature.suppress` sets or clears it. A suppressed feature hands on the body it would have changed, and one with no body to hand on fails and leaves what uses it stale (#290).
- The kernel measures a face or an edge (its size, center, direction, radius and axis), the shortest distance between shapes or their faces and edges, and the inertia of a body about its centroid with its principal moments and axes (#291).
- `measure_distance`, `measure_length`, `measure_area`, `measure_radius`, `measure_angle` and `measure_mass` keep measurements in the model as features, so they rebuild when it changes. Each has a `measure.` command (#292).
- opencad-compat marks Sketcher B-spline, PartDesign_InvoluteGear, feature suppression and the seven measure rows partial. The table is now 549 missing, 58 partial and 58 done.

## 0.0.24

Copying and arranging sketch geometry, whole-sketch edits, copies of bodies, and an involute spur gear.

- `sketch.copy` copies sketch entities with their constraints, moved by an offset, and can keep a clone tied to the original with equal constraints. `sketch.array` lays copies out in rows and columns (#282).
- `copy(body)` makes a body of its own from another, turned about a world axis and moved if asked. `reverse(body)` turns a body inside out. Both have `part.` commands (#283).
- `sketch.merge` puts two or more sketches on the same plane into one, and `sketch.mirror` makes a mirrored copy of a whole sketch about its horizontal axis, its vertical axis or its origin, with distances measured the other way (#284).
- The `block` constraint holds sketch entities where they are drawn and refuses a dimension that would move them (#285).
- `sketch.connect` joins lines and arcs end to end at their nearest ends and can close the chain into a region (#286).
- `gear(teeth, module, h)` and `part.gear` make an involute spur gear, with an optional pressure angle and profile shift. The kernel gains `plate()`, which extrudes an outline of lines, arcs and splines. Plate splines pass through their points at even steps, which keeps the involute exact to 2e-7 in area where it leaves the base circle (#287).
- opencad-compat marks Sketcher RectangularArray, Translate, Move, Copy, Clone, MergeSketches, MirrorSketch, ConstrainBlock and JoinCurves done, Part_ReverseShape done, and Part_SimpleCopy and PartDesign_InvoluteGear partial. Sketcher is now 29 of 73 done.

## 0.0.23

Faces and sheets from sketches, scaling, cutting bodies into pieces, and curves on their own. Volumes and lengths of splines are now measured adaptively.

- `make_face` turns a sketch's region into a flat sheet (#275). `offset_face` grows or shrinks it in its plane, with round or sharp corners (#276). `ruled_surface` joins two sketch outlines with straight lines (#277). A sheet now reports no volume, where an open shell used to give it a signed one.
- `scale` makes a body bigger or smaller about a point, by the same factor on every axis or a different one on each (#278).
- `fragments` cuts bodies into every piece they make of each other. `xor` keeps the pieces that lie in an odd number of them, and `slice` keeps the pieces of a body that other bodies or sheets cut it into (#279).
- `wire_point`, `wire_line`, `wire_circle`, `wire_ellipse`, `wire_polygon`, `wire_helix` and `wire_spiral` make curves on their own. The helix can lean out on a cone (#280).
- Every one of these has a `part.` command.
- Volumes are integrated adaptively. A stretched cylinder was 0.9% off before. The ellipsoid tests now hold to 1e-8, where they were at 1e-3. Wires report their length, measured edge by edge to a tolerance.
- opencad-compat marks Part_MakeFace, Part_Scale, Part_XOR, Part_Slice and the wire primitives done, and Offset2D, RuledSurface and BooleanFragments partial. Part is now 21 of 57 done with 19 partial.

## 0.0.22

Sketch offsets and ellipses, and more of the Part primitives: tubes, prisms, flat sheets and compounds.

- `sketch.offset` draws a closed loop of lines and arcs again a set distance out or in, with lines held parallel and arcs on the same centers. A single circle offsets to a circle (#267).
- `sketch.ellipse` draws an ellipse from its center, a point at the end of its major axis and its minor radius. It solves, it bounds a region, and it builds on OCCT as a hole or a pin whose volume matches π·a·b·h (#268).
- `tube(r, inner, h)` is a cylinder with a bore through it, and `prism(sides, r, h)` stands a regular polygon up its axis. Both take `axis`, `at`, `into` and `op` like the other primitives (#269, #270).
- `part.tube`, `part.prism`, `part.wedge` and `part.ellipsoid` add those primitives from the palette (#271).
- `plane_face(length, width)` and `part.plane_face` make a flat rectangular sheet (#272).
- `compound(parts)` holds bodies together without joining them, `part_of(compound, index)` takes one back out, and `part.compound` adds a compound from the palette (#273).

## 0.0.21

The sketch tools for cutting and rounding geometry already drawn, and patterns, revolves and helices about an axis off the origin.

- `revolve`, `helix` and `circular_pattern` take `axis(pt3(...), dir)`, so they can turn about any line, not just one through the origin. A pattern of four bosses about a point at (0, 20) lands each boss where it should, and the volumes match the exact figures (#262).
- `sketch.fillet` rounds the corner where two lines meet with an arc tangent to both, and `sketch.chamfer` cuts it with a line across. The corner point stays as construction held on both lines, so dimensions to it still hold (#263).
- `sketch.split` cuts a line or arc in two and the parts stay one curve. `sketch.extend` moves a free end of a line straight on or back (#264).
- `sketch.trim` takes out the piece of a line between the curves that cross it, or the whole line when nothing does. A cut end lands on a point already drawn there, so trimming two lines at a corner leaves them sharing it (#265).
- opencad-compat marks Sketcher_CreateChamfer done, and fillet, trim, split and extend partial. Sketcher is now 23 of 73 done with 15 partial.

## 0.0.20

The rest of the Part and PartDesign solid tools M1 parity counts, and the sketch tools for editing geometry already drawn. This is the first release with everything checked against an OCCT 8.0.1 rebuilt from source. That run found four bugs, and they are fixed here.

- `draft` tilts chosen faces about a neutral plane and keeps their names (#252). `offset` grows or shrinks a solid all round, with round or sharp joins, and its faces keep their names, so a shell after it can still open the top (#254). `refine` merges faces that lie on one surface. Two fused boxes go from 10 faces to 6, with one name each (#255). `part.draft`, `part.offset` and `part.refine` do the same from the GUI.
- Box, cylinder, cone, sphere and torus take `into` and `op`, so they add to or cut from a body like PartDesign's additive and subtractive primitives (#253). `wedge` and `ellipsoid` join them (#256).
- `helix` sweeps a region along a helix around an axis. A 2×2 square at radius 10 over 4 turns comes out at the volume Pappus gives, and `op = cut` makes threads (#257).
- `section` gives the edges where one or more parallel planes cut a body (#258). A pattern can take another pattern as a seed and copies every instance of it, which is how a MultiTransform chain builds (#259).
- New sketch commands: `sketch.polyline`, `sketch.oblong`, `sketch.clear`, `sketch.move`, `sketch.rotate`, `sketch.scale` and `sketch.symmetry`. A quarter turn swaps the horizontal and vertical constraints on what it turns. A scale grows the dimensions that only measure what it scales. Half a profile mirrored across its axis closes into the whole (#260).
- Fixes from the OCCT run (#259):
  - The helix shim had a most vexing parse.
  - Offset faces were named as new.
  - A section's `count` was read as a length.
- opencad-compat marks 16 more rows partial. PartDesign is now 7 of 35 done with 11 partial, and Sketcher 22 of 73 done with 11 partial.

## 0.0.19

Lofts and sweeps, the two P0 PartDesign features M1 was still missing for shapes beyond extrude and revolve.

- `loft(sections = [a, b, ...])` runs a solid through the regions of two or more sketches, smooth or `ruled`, and can be added to or cut from a body. `part.loft` does the same from the GUI, and `sketch.new` takes an `offset` so sketches can be stacked. A fillet on a loft's side edge finds that edge again after a section changes size (#249).
- `sweep(profile.region, path = sketch)` runs a region along the lines and arcs of another sketch, and `part.sweep` does the same from the GUI. Each side face is named after both the profile edge and the path segment it came from, so a bent path keeps every side apart (#250).
- OCL evaluation took the names a binding uses in reverse, so `[inlet, outlet]` evaluated the outlet first, and a save and reload changed the order of the sketches. Evaluation now follows the file's order (#249).
- opencad-compat's naming suite cuts a pattern from 3 instances to 2 in 498 models. Of 64,230 names on the patterned body, 55,764 stay where they were, and all 8,466 on the third instance are gone, with none moved. Across 20.3M references, 99.942% survive and none are rebound silently.
- opencad-compat marks Part_Loft, Part_Sweep and the PartDesign loft and pipe as partial.

## 0.0.18

More of the Part and PartDesign features M1 parity counts: cone and torus, shell, mirror and the linear and circular patterns, and rectangles, polygons and slots in sketches.

- `cone(r1, r2, h)` and `torus(r1, r2)` build on the kernel's primitives, and `part.cone` and `part.torus` make them from the GUI (#241). A revolve can cut a groove into a body or add a rib to it (#242).
- `sketch.rectangle` draws four lines held horizontal and vertical, optionally from the center. `sketch.polygon` draws 3 to 64 equal sides on a construction circle. `sketch.slot` draws two arcs and two lines held tangent (#243, #244).
- `shell(body, remove = ..., thickness = t)` hollows a body and leaves the picked faces open. This is the first selector for faces: `faces.start_cap`, `faces.end_cap` and `faces.lateral(curve)`. Each inner wall is named after the outer face it sits behind (#245).
- `linear_pattern`, `circular_pattern` and `mirror` copy bosses, pockets and bodies. Each copy is joined the way its seed was, and `skip` leaves instances out. The kernel's new `transform` names every face of a copy `Instance(n)` after the face it copies (#246).
- opencad-compat `regen-hash` builds 503 models on the M4 (macOS, arm64) and on an AMD EPYC server (Linux, x86_64). With every number rounded to 1e-6, the B-rep digests match for all 503. The exact-bit digests do not match, because the last bits differ between platforms (#16).
- opencad-compat now marks 40 of the 665 FreeCAD rows done and 14 partial, up from 35 done.

## 0.0.17

Persistent names that survive a reorder, a save, and an upstream cut that splits a face, measured by the new topological naming suite in opencad-compat over 500 generated models (#11).

- The kernel measures every face and edge: its kind of surface or curve, its area or length, its centroid, and its normal or axis. The naming suite uses this as the signature from spec 15 section 6.4. Moving every dimension of every model by 5% and 20% either way keeps 99.930% of 14.6M references, with none rebound silently.
- OCL evaluation gives each feature an id derived from its qualified name instead of from where it sits in the file. A model with its features reordered and then saved and opened again used to lose every persistent name. It now keeps all of them, 197,327 out of 197,327. Renaming a feature in the file changes its id.
- `edges.lateral(p)` finds its edge through a face that a later boolean cut in two, as long as only one edge matches. When none or several match, the error starts with "reference needs repair:". With a slot cut in upstream of 498 models, 149,189 of 158,199 names stay on the same plane, line or axis, the rest are gone, and none move to other geometry.

## 0.0.16

The Part and Sketcher commands that M1 parity counts: chamfer, revolve, circles and arcs, the construction and driving toggles, and the Part primitives with booleans. Each one is checked by building it with OCCT and comparing the volume against the exact value.

- `part.chamfer` cuts named edges back by an equal distance, written as `chamfer(edges, mode = equal(d))`, and finds them again after a resize.
- `part.revolve` turns a sketch region about X, Y or Z, either a full turn or `angle(a)`.
- Sketches take `circle(center, r)`, and a circle can be a loop of its own in a region, so it can make a cylinder or cut a hole. `dim radius(c)` and `dim diameter(c)` drive circles and arcs. `sketch.circle`, `sketch.arc`, `sketch.circle_through` and `sketch.arc_through` draw them. The three-point versions add a center point and keep the curve on the points it was drawn through.
- `sketch.construction` switches entities to and from construction geometry, and `sketch.driving` switches a dimension between driving the sketch and only reporting what it measures.
- `box`, `cylinder` and `sphere` build on the kernel's primitives, and `boolean(a, b, op = fuse | cut | common)` combines two bodies. `part.box`, `part.cylinder`, `part.sphere` and `part.boolean` make them from the GUI.
- The SketchGraphs validation sample converges on 99.91% of sketches from their recorded geometry and on 99.68% from geometry moved by 1% of the sketch size, against a 99.5% gate (#13).
- opencad-compat now marks 35 of the 165 Part, PartDesign and Sketcher rows done, up from 22.
- The lock files of the web and desktop apps carry the release version again.

## 0.0.15

A faster drag in a big sketch, and the FreeCAD parity rows for M1 counted against real tests.

- Each drag step is now solved with a sparse LDLᵀ factorisation instead of a dense least squares. The fill-reducing order is worked out once per drag and reused every frame. A step whose rows turn out to be dependent falls back to the dense solve, so the answer is the same as before. On B-SKETCH-DRAG-500, a 500-entity sketch from opencad-bench dragged for 120 frames, p50 is 0.67 ms on the M4, against a 4 ms limit. A first version that kept the factor in a map took 12.9 ms.
- The spec's import rows count only STEP files that declare a solid, since a file with none is meant as a sheet. opencad-compat reports both rates.
- opencad-compat marks 22 Part, PartDesign and Sketcher rows done and 20 partial, each naming the tests that prove it, and fails if a named test goes missing.

## 0.0.14

STEP import that turns more of ABC into solids, and a faster edit near the end of a long history. On ABC chunk 0002 (files 00020000 to 00029999) 9,815 of 10,000 files now read as valid solids, 98.15%, up from 9,801 before the OCCT patch below.

- Healing sews a shape that has faces and no solid, going up the ladder of spec 05 section 10.3 (1e-6, 1e-4, 1e-3 and 0.01 mm) and stopping at the first tolerance that closes every shell. Each closed shell becomes a solid with a positive volume. A shell that stays open stays a shell, so a file meant as a sheet comes in as one. In chunk 0002 this turned 12 files into solids, 9 of them valid.
- OCCT is now built with a patch queue, kept in `crates/ocad-occt/patches` and applied by `scripts/occt`. The first patch lets the STEP reader offset a basis surface OCCT calls C0. That's usually a spline with a knot of full multiplicity, smooth in space but not in its parameters, and the reader used to drop the face and open its shell. In chunk 0002 that gives 18 files a solid that had none, 14 of them valid. OCCT's marker and the CI cache key include the queue's hash, so a new patch rebuilds OCCT.
- An invalid import says which BRepCheck statuses it fails, so the causes can be counted. In chunk 0002 the largest classes are self-intersecting wires (45 of 83 files) and wires closed in space but not on the surface (25).
- `Occt::tolerances` returns the loosest vertex, edge and face tolerance of a shape and how many of each are above 0.01 mm. Spec 15 section 14 has a new "within tolerance" import row, reported only at M1.
- An edit in a run of joined extrudes keeps the body before it. The first edit builds the unchanged bodies as a run of their own, and later edits at the same place join only the new ones to that body. An edit at feature 290 of synth-history-300 no longer rebuilds the whole run, and B-REGEN TAIL p95 went from 115 ms to 90 ms on the M4, measured A/B.

## 0.0.13

Two fixes to STEP import that came out of real files. A solid made only of triangles now comes in the right way out, and a face OCCT would crash checking now comes back as invalid.

- OCCT's reader for tessellated STEP winds every strip and fan triangle the opposite way from ISO 10303-42 and keeps the file's normals, so a solid of triangles came in inside out with a negative volume. FreeCAD 1.1.3 on OCCT 7.8.1 does the same. After a read, a face that is only triangles is turned round when its triangles disagree with its normals. NIST FTC-08 in triangles now reads +503,280.82 mm3 where it read the same volume negative, and opencad-compat fails any import whose solid has a negative volume.
- BRepCheck crashed on a face whose curve on the surface OCCT cannot sample, which BRepTopAdaptor_FClass2d does not expect and then writes before the start of an array. ABC 00042004 has three such faces, on cones, where an edge runs into the apex and its curve swings out to millions of times the face's range. Every spline curve on a face is now sampled before BRepCheck runs, and one that cannot be makes the shape invalid. That costs 150 ms on the 38 s read of that file.

## 0.0.12

Booleans on a long history, fast enough to meet bar 5 on the M4. opencad-bench now runs B-REGEN on synth-history built through OCCT, and changing a dimension near the root of 300 features went from 21.1 s p50 at 366fab6 to 177 ms here, against a p95 target of 1 s. At 1,000 features it takes 666 ms against 3.5 s. Changing feature 290 takes 108 ms, still just over its 100 ms target.

- `Kernel::boolean_many` cuts or joins several tools in one call, and OCCT takes them in one BRepAlgoAPI operation. Faces keep the names they would get one boolean at a time. B-BOOL-CUT-BLOCK, 260 holes cut from synth-block in one boolean, takes 154 ms p50 on the M4.
- A boolean checks its result with the fast check of spec 05 section 10.2 instead of BRepCheck: every edge of a shell is used once each way, the vertices of the faces it made sit on their edges, and those edges agree with their curves on the faces. BRepCheck always classified every wire against every other, which was more than half the time of an edit near the end of a long history. The check is about 1% now.
- A boolean no longer looks for inside-out solids among its inputs, since each one passed a check when it was built. That saved about 13% of the instructions.
- A chain of extrudes with `into` waits until its last body is asked for, then joins the tools whose boxes keep clear of each other in one union and one difference. If that would name a face differently from joining them in turn, it joins them in turn. Spec 06 section 4 describes this.
- A sketch can be on `plane(XY, offset = 10 mm)`, a standard plane moved along its normal.
- Face history counts a boolean's inputs in 32 bits. It used a byte, so a join of more than 255 tools gave the later ones the names of the first ones, and a 1,000 feature history fell back to one boolean at a time and took 38 s.

## 0.0.11

Extrudes that add to or cut from a body, and the first work on how fast a long history rebuilds. opencad-bench now times B-REGEN on a 300 feature part, and on the M4 an edit near the end of it took 1.16 s where the target is 100 ms, so three changes go after where the time went.

- An extrude with `into` and `op = add` or `op = cut` now joins its extrusion to that body, or cuts it out, which is how spec 06 writes a boss and a pocket.
- A boolean runs the full BRepCheck only on the faces it made, and checks the topology of the whole result. The faces it passes through unchanged were checked when their input was built. Checking the whole body again was about 36% of every boolean on a long history.
- A built body no longer carries its volume and area. The kernel measures it when someone asks, since nothing asks during a rebuild, and measuring every body in the history was about 13% of each boolean.
- A persistent name shares the names it came from instead of copying them, so the naming that follows each boolean clones pointers rather than trees.
- The nightly host run has a `freecad` job, which checks the pinned FreeCAD 1.1.3 AppImage by hash and runs `opencad-compat freecad measure --check` on the FreeCAD and NIST corpus files. The compat job checks the parity list against the FreeCAD 1.1.3 source, so a command FreeCAD registers with no row and no ignored entry fails it.
- The focused gate, `cargo xtask ci --changed`, runs only what a change can break, and only the suites that read the spec when the spec changes.
- The renderer asks the device for its buffer size limit, so a 100,000 part scene fits.

## 0.0.10

- The renderer no longer opens wgpu's GL backend, which cannot sample the multisampled id target and drew every frame empty without an error, and the nightly host runs the render tests on Mesa's lavapipe with `OCAD_REQUIRE_GPU` set.
- The sketch solver and OCL now take sin, atan2, pow and the other transcendental functions from the pure Rust `libm` crate instead of the platform's, as spec 06 section 3.9 asks. The first nightly on Linux found `asin(0.5)` and `acos(0.5)` an ulp apart from macOS, where Apple's libm is the one that is off. The `libm` crate's outputs hash the same on aarch64 macOS, x86_64 Linux and wasm32 over 14 million inputs. A clippy lint in both crates now refuses the `f64` methods, and `mul_add` with them.
- `cargo xtask journey` builds the UI package's design tokens before the journeys, so it works on a fresh checkout. The nightly journeys job would have failed without it. `scripts/nightly-host` installs the pinned pnpm with npm, since Node 25 and later ship without corepack. J-SK-01 passes headless on server2.
- `scripts/nightly-host` runs the Linux jobs of this repo's nightly workflow and opencad-compat's on a machine of our own: the release suite with OCCT, the browser journeys, cargo deny, compat against main, the NIST import and the Forge and OCCT differential. It keeps a log per job, sets `nightly-host/*` commit statuses and comments a summary on #187. It is there while GitHub Actions cannot run.

## 0.0.9

Section planes and a steadier fuzz farm. The renderer can now cut the scene with up to six planes, and B-FRAME's camera path has the section sweep, which completes the path spec 15 asks for. On the M4 the cut car draws faster than the whole one, and a scene with no plane costs what it did before. The OCL formatter no longer changes its own output around trailing doc comments, and the fuzz farm now comes back to findings its daily cap held back. With that every fuzzing box of M0-3 is done.

- The fuzz farm now comes back to the findings its daily cap of five issues held back. Each round it reruns every finding that was never filed, marks the ones that no longer fail as fixed with the commit that fixed them, and files the rest. On server2 the three waiting since the cap was hit all turned out fixed, so none were filed.
- The renderer takes up to six section planes through `SceneDelta::SetSection`, and B-FRAME's camera path now has the section sweep spec 15 asks for: halfway round the orbit a plane sweeps in from the front of the car to three quarters of its length and back out. Parts wholly on the cut side are culled, and the rest are cut by the rasteriser's clip distances, or by discarding fragments on a device without them, such as DX12. A scene with no plane is drawn by pipelines that do not cut at all, since on the M4 either way of cutting costs every frame: clip distances 4 ms, and a shader that can discard 14 ms. During the sweep on the M4 at 2560x1600 with 5,000 parts, a frame takes 15.0 to 15.6 ms p50 with clip distances, against 19.1 to 19.3 ms for the whole car, and 26 to 27 ms by discarding. Caps come later.
- `ocl fmt` no longer changes its own output. A `///` comment after code was counted against the line when choosing whether to break a call or tuple, though it goes on a line of its own, and after a line starting with `.` it took that line's extra indent. Across the fuzz farm's 12,451 OCL inputs, 4,116 of which parse, formatting twice used to differ from formatting once on 76 of them, and now differs on none.
- The OCL parse timeout the fuzz farm reported in #163 is kept as a regression. The limits from #161 and #165 already fix it: the input now fails to parse in 2.6 ms, and 16 copies of it, 7.7 KB, fail in 6 ms.

## 0.0.8

The first bracket. J-SK-01's L bracket can now be built from an empty document in the browser through the palette alone, and the OCL it writes is the golden file byte for byte. The same OCL regenerates on OCCT 8.0.1 to 21,464.071 mm³, which is what the drawing works out to, in about 3.3 ms. Under that are the sketch and part commands, `ocad-host` with its JSON protocol, the core running as wasm in a web worker and in process on the desktop, a Playwright journey, and a kernel that solves sketches and builds extrudes and fillets. Picking works in the browser, and Metal now times every frame.

- The kernel builds sketches, extrudes and fillets, and the J-SK-01 bracket regenerates on OCCT 8.0.1. The extruded body is 20,640.000 mm³ and the filleted bracket 21,464.071 mm³, both equal to the drawing's own arithmetic, in about 3.3 ms on an M4. Changing the 40 mm dimension to 60 mm regenerates with the fillet still on its corner.
- J-SK-01 runs in the browser. A Playwright journey builds the bracket from an empty document through the palette and its forms alone, 21 commands against the real wasm core, and checks that the Code panel is `bracket.ocl` byte for byte. `cargo xtask journey` runs it, and the nightly workflow runs it too.
- The web shell and the desktop app now hold a real document. The browser runs `ocad-host` built to wasm in a web worker, and the desktop runs it in process behind a Tauri `core` command. The palette asks for a command's arguments in a form built from its manifest entry, shows an error under the field it names, and the feature tree, the Code panel and Mod+Z and Mod+Shift+Z all follow the core's replies. `?demo` keeps the old demo data.
- `ocad-host`, the document a shell holds. The desktop app, the web worker and `opencadd` will each keep one and send it JSON requests: run a command by id, undo, redo, open OCL, or ask for the state. Every reply carries the state after it, meaning the features in order, the OCL the document writes as, and what undo and redo would do, so a shell draws the last reply and cannot drift from the document. Arguments are read by the type the command declares, and a value with a unit is OCL text such as `"1 in"`, read the way an OCL file reads it. A test builds the J-SK-01 bracket through requests alone and gets the golden file back. The small JSON reader the language server used is now public in `ocad-ocl` so the host can share it.
- Commands for the sketcher and the first part tools: `sketch.new`, `sketch.point`, `sketch.line`, `sketch.constrain`, `sketch.dimension`, `sketch.set_dimension`, `part.extrude` and `part.fillet`. Each one lowers to adding a feature or setting a parameter, shaped the way OCL evaluates the same text, so what the GUI builds saves as the OCL someone would have typed. A new test builds the J-SK-01 bracket from these commands alone and writes out the golden file byte for byte. All eight pass the command conformance checks, round trip through OCL, and are in the palette.
- The J-SK-01 bracket from spec 15 is now a golden file, `tests/journeys/j-sk-01/bracket.ocl`: a 60 mm L bracket drawn on YZ with six lines, two orientation constraints and four dimensions, extruded symmetrically and filleted 8 mm on the inner corner. Tests in `ocad-ocl` check that it formats to itself, checks clean, evaluates to a sketch, an extrude and a fillet, and writes back byte for byte. It is the file the sketcher journey has to produce.
- Picking in the browser. `Renderer::pick_later` returns a future for the id under a pixel that holds nothing of the renderer, and `Renderer::pick` now waits on it. In the web shell a click on the viewport selects the part under it and the status bar names it. A drag of more than 4 px does not select. In Chrome on an Apple M4, 300 picks over a 1504 by 1656 canvas with 5,000 parts take 0.4 ms at p50 and 0.9 ms at p95. They agree with native Metal on every point, 72 hits and the same part in the middle. `window.__ocPicks()` runs the measurement.
- OCL: adding two temperatures on the `degC` or `degF` scale is an error, OCL0103, as spec 06 says. The checker tells a point on the scale from a difference, also through names, so `20 degC - 10 degC` and `20 degC + 10 K` are still fine. A word after a statement must be `fixed`, `construction` or `defining` on a sketch entity. Anything else is OCL0208, and after a bare number the message says the word is not a unit OCL knows. Before this, `3 fur` was 3 with the unit silently dropped.
- `ocad-render` times frames on Metal. The timestamps were resolved before Apple silicon had written the end of the last pass, so all but 1 of 3,300 B-FRAME frames came back untimed, and the bench fell back to wall time. They are now resolved once the frame is done. `gpu_ms` now runs from the first pass's start to the last pass's end, because a tile-based GPU overlaps the passes and their sum counted the overlap twice. That overlap is where the 5.6 ms outline pass came from. B-FRAME on an Apple M4 at 1504 by 1656 now times every frame that draws anything: 12.6 ms at p50, 17.6 ms at p95 and 20.3 ms at p99 natively, against 11.7 ms and 15.1 ms in Chrome on WebGPU.
## 0.0.7

The shell and the viewport. The web app now has the full spec 12 layout, with rails, resizable panels, tabs, a status bar, the feature tree, overlays, menus and the inline dimension field, all in `@opencad/ui`. The renderer runs in the browser on WebGPU and under the desktop UI on Metal, and in Chrome on an Apple M4 it draws the 5,000-part B-FRAME vehicle every 16.7 ms, the same as native. `apps/desktop` wraps the shell in Tauri. OCL fixes seven findings from the fuzz farm, among them exponential parsing and formatting, a stack overflow on long chains, and a formatter change that altered a file's meaning.

- The renderer runs on WebGPU in the browser. `apps/web/viewport` builds it to wasm, `cargo xtask wasm` writes it into the web app, and the web shell draws the 5,000-part B-FRAME vehicle behind its panels when the browser has WebGPU. GPU timestamps come back through a ring of buffers because a page cannot wait on the GPU, and the canvas gets an sRGB view since it has no sRGB format. In Chrome on an Apple M4 at 1504 by 1656, with 3.17M triangles, frames come every 16.7 ms at p50 and 18.4 ms at p95 with none dropped, the CPU spends 0.8 ms on each at p50, and the GPU 16.8 ms, which is where native Metal lands on the same machine (11.75 ms shaded plus 5.64 ms outline). `window.__ocBench()` in the page runs the measurement.
- OCL: a field or method on a number, a truth value or a string, such as `1.r` or `18 in .b`, is an error, OCL0205, from the checker and from the evaluator. It used to become a call for the kernel, which the writer wrote back in place of the number, so `param r: Length = 1.r` used in `point(r + 2, 0)` came out as `point((1.r + 2), 0 mm)`, where the `2` has no unit to take and the file no longer evaluated.
- `ocl fmt` no longer refuses a file with a doc comment just before a closing bracket, as in `(l, ///` then `)`. It wrote the trailing comma of a broken list after the comment, which moved the comma out of the tuple. It now puts the comma after the last item's code, and a doc comment after code now always starts its own line, as it did the second time round.
- The desktop smoke check finds the side panels by their titles. Since the rails carry the same names, it had been counting the rail buttons as the panels.
- `@opencad/ui`: `Rail`, `Panel`, `TopBar`, `StatusBar` and `TabStrip`, and the web shell is built from them. A rail button opens its panel and pressing it again closes the panel to the rail. Dragging a panel's edge moves only an outline, so the viewport changes size once, on release. `Mod+\` closes both panels and opens them again, and below 1280 px the right panel lies over the viewport. The document tabs take Ctrl+Tab and Mod+1 to Mod+9.
- `@opencad/ui`: `Tooltip`, `Popover`, `Menu`, `ContextMenu` and `InlineError`. Floating boxes open beside their anchor, flip when the other side has more room, and never leave the window. Tooltips follow WCAG 1.4.13. Menus follow the ARIA menu pattern with typeahead. The context menu opens on a secondary click, a 500 ms touch, Shift+F10 or the menu key, and opened from the keyboard on the tree, it sits under the active row. `apps/web` has the section 7.3 menu on the feature tree and shows Fillet 3's failure with its fixes in the properties panel.
- OCL: an expression's tree may be at most 128 levels tall, counting the links of chains like `a - b - c` as well as brackets, and the parser refuses a taller one with OCL0002. The parser built chains in a loop with no limit, but the checker, the evaluator and the formatter walk them by recursion, and a fuzz input with a long chain overflowed the checker's stack. A test grows fourteen shapes to the limit and puts the tallest through every stage on a 2 MB debug stack.
- `ocl fmt` no longer takes exponential time on nested brackets. It laid out each group on one line to see whether it fit and then again to write it, and each of those did the same for the groups inside; 20 levels took 73 s and now take 3 ms.
- `@opencad/ui`: `NumberField`, the inline dimension field of spec 12 section 10. It takes a number or an expression with units and parameters, shows the value as you type (`= 124 mm`, with `…` when the display rounds), and commits the expression as OCL on Enter, with a unit written after every bare number so the stored text says what you saw. Esc reverts, and the arrows and the wheel nudge by 1, 0.1 with Shift and 10 with Mod. Its evaluator agrees with OCL on all 131 expressions in a corpus that OCL's own test writes, and takes 17.7 µs per expression on an Apple M4. `apps/web` has a Depth field in the properties panel.
- `ocl fmt` no longer writes `2 in + 3 mm` as `2 in +3 mm`. It read `in` as the keyword from `for x in`, after which a `+` or `-` is a sign.
- OCL: the parser no longer takes exponential time on nested `name = {`. Each `{` was tried as a record and then as a block, and so was every `{` inside it, so 30 levels took 2^30 steps. It now remembers which braces were not records. The four fuzz inputs that timed out took 0.74 to 59 s and now take under 6 ms.
- OCL: a bare number in `min`, `max`, `clamp`, `atan2` and `lerp` now takes the default unit of the other arguments, as it already did in `+` and `-`. `max(30, 1 rad)` was 30 radians and is now 30 degrees.
- `apps/desktop`: the desktop app, Tauri 2.12 around the web shell, in a Cargo workspace of its own so the core checks do not build a webview. `OPENCAD_SMOKE=1` makes it check that the shell and the palette come up and quit; a debug build has the page loaded 1.2 to 1.9 s after start on an Apple M4. The gate and CI format and lint it, and the version check covers it.
- The viewport composites under the desktop UI: `Renderer::for_window` puts the renderer on a window surface and `Renderer::present` copies each frame into the viewport's rectangle, which the page reports in physical pixels and leaves transparent. The app builds the window before the webview so the webview sits on top, and shows the 5,000-part B-FRAME vehicle until documents open. A test copies a frame into a BGRA sRGB target at an offset and past the corner and checks every pixel. On macOS and Windows for now; Linux needs a child surface under WebKitGTK.
- `@opencad/ui`: `Tree`, `Dialog` and `Toast`. The tree draws only the rows in view plus 20 either side and has the section 11 keyboard model; in Chrome on an Apple M4, scrolling 10,000 rows takes 0.95 ms of JS per frame at p50 and 1.62 ms at p95, against a budget of 2 ms. `apps/web` shows the feature tree, and `?tree=N` fills it with N features.

## 0.0.6

The first UI. `@opencad/ui` holds the design tokens from spec 12, generated from OKLCH and checked for contrast and colour vision in four themes, and the command palette with its React primitives. `apps/web` is the first app, a shell with the palette on Mod+K over every command the core registers, read from a manifest the core writes. In Chrome on an Apple M4 the palette opens over 6,000 items in 16.9 ms at p50 and 25.8 ms at p95, and a keystroke is 2.0 to 3.2 ms at p95, against budgets of 50 ms and 8 ms.

- `apps/web`: the first app, a Vite and React shell with the section 4.1 layout built from the tokens and the command palette on Mod+K over every registered command. In Chrome on an Apple M4, opening the palette over 6,000 items takes 16.9 ms at p50 and 25.8 ms at p95, against a budget of 50 ms. The index is built while the app is idle, so the first open is 31 to 49 ms instead of 108 to 500.
- The command manifest. Every command now has a title, aliases, workspaces and an optional hotkey. `ocad_core::command::manifest` writes the registry as JSON, and `@opencad/ui` turns it into palette items with `commandItems`. A test in ocad-core fails when the checked-in `commands.json` is stale; `OC_BLESS=1` rewrites it.
- `@opencad/ui`: the first JS package, in a pnpm workspace. The design tokens come from an OKLCH theme generator (base, accent, contrast) and build to CSS custom properties and DTCG 2025.10 JSON, in dark, light and two high contrast themes. The build checks WCAG contrast for every text and control pair in every theme and keeps the six viewport overlay colours apart under protanopia, deuteranopia and tritanopia.
- `@opencad/ui`: the command palette. An fzf-style matcher and the section 8.2 ranker find a keystroke's results over 6,000 items in 2.0 to 3.2 ms at p95 on an Apple M4, against a budget of 8 ms, measured with `pnpm bench` on a quiet machine; the gate holds timing tests to four times their budget so a loaded runner does not fail them. `CommandPalette`, `Button`, `IconButton` and `Kbd` are the first React primitives.
- Spec 12 gains the sections it pointed at but did not have: tokens, accessibility, localization, UI budgets, the component inventory, the usability plan and open questions, with the generated values and measured numbers in them.
- `cargo xtask ci` and the ci workflow typecheck, test and build the JS packages, and the version check covers `package.json`.

## 0.0.5

The first renderer. `ocad-render` draws a scene headless on wgpu with 4x MSAA, metallic-roughness shading, reverse-Z depth and one instanced draw per mesh, with feature edges, silhouette outlines and GPU picking. It times every frame on the GPU where the backend can. B-FRAME, the 60 second camera path from spec 11 through a synthetic 5,000 part vehicle, runs at 2560x1600 with p50 17.34 ms and p95 25.79 ms on an Apple M4, up from p95 50.04 ms before the pick pass was folded into the shaded pass and culling was added.

- `ocad-render`: the renderer core. Scene deltas upsert and remove geometry, materials and instances, the camera is reverse-Z with an infinite far plane, and all geometry lives in one vertex buffer, one index buffer and one instance buffer. `capture` reads a frame back as RGBA, and `FrameStats` reports GPU time per pass, CPU time, draws, instances, triangles and edges.
- `ocad-render`: `Mesh::feature_edges` finds the boundary and the creases sharper than an angle, and the shaded pass draws them as lines pulled toward the camera by a share of their depth, so they stay on top of their faces at any distance and hidden ones stay hidden.
- `ocad-render`: GPU picking and outlines. The shaded pass writes each part's pick id to a second multisampled target, and a fullscreen pass darkens the pixels where the id changes and writes one id per pixel for `Renderer::pick` to read back.
- `ocad-render`: instances outside the view are culled on the CPU before each frame against the side and near planes. Inside the synthetic car this cuts 3.17M triangles to under 0.5M.
- `ocad-render`: `synth::vehicle` builds a deterministic car-shaped assembly of any size, half unique panels, rails, tubes and housings and half instanced fasteners, and `synth::camera_path` is the B-FRAME path. The `frame_bench` example flies it and reports p50, p95 and p99, and falls back to wall time where the GPU timestamps cover too few frames, as they do on Metal.
- `scripts/fuzz-farm`: a timeout is replayed with five times the limit and kept as slow if it finishes, each target keeps fuzzing until its budget is spent, and a run that prints no status line no longer stops the farm.
- `scripts/fuzz-farm`: an issue no longer shows an empty input. When tmin failed partway it could leave an empty file behind, and the farm filed that as the minimised input, as in #145, #148 and #149. The reduced input is now kept only if it is not empty and fails with the same signature, and the issue says whether its input was minimised.
- The governance charter and code of conduct, and how changes are reviewed.

## 0.0.4

Files in and out, and the first geometry kernel work. OCL gets a formatter, a writer that turns any document back into OCL, and a language server. `ocad-io` reads and writes the `.ocad` bundle and reads STEP Part 21 and STL, and all of it has fuzz targets. `ocad-occt` is the OCCT 8 backend: it implements the whole kernel trait, reads and writes STEP, heals imported shapes, and imports the NIST CTC, FTC and STC test parts to within their goldens. Forge, OpenCAD's own NURBS library, now evaluates rational curves and surfaces the same way OCCT does. Checked against OCCT and exact rational arithmetic on every B-spline in the NIST and DrivAerML files (28,613 curves and 8,181 surfaces) and in the first 2,000 ABC files (446,436 curves and 85,466 surfaces), it is never the kernel that is wrong. OCCT is wrong on 41 and 51 of them.

- `ocad-forge`: derivatives are taken relative to a control point of the span and the point is moved back at the end. A rational B-spline moves with its control points, so this is the same curve, but the quotient rule no longer subtracts terms the size of the coordinates from each other. A rational curve and a rational surface moved away from the origin by powers of two, which is exact, used to change their first derivatives by 6.4e-12 and 8.3e-12 relative and now keep them to 1e-12.
- `ocad-forge`: a coordinate is only taken relative to the span's control point when that subtraction is exact, which is when every value in the span is within a factor of two of it. On a DrivAerML surface whose z crosses zero inside one span, the rounded subtraction broke the exact differences of neighbouring points and put the second derivative 2e-9 off where it had been within 1e-12.
- `ocad-forge`: derivatives now come from differencing the control points and summing against the basis of lower degree, in both directions for a surface, instead of summing the points against basis derivatives. The two are the same exactly but not in floating point, and the differential harness found the old way losing up to six digits of the second derivative on real models: 1.1e-6 relative on a NIST FTC-10 curve with a span of 1.4e-4, and up to 5.4e-8 on DrivAerML surfaces, where OCCT was right to 1e-13. Checked in exact rational arithmetic, Forge is now within 5.3e-12 on every sample where the kernels disagree.
- `ocad-occt`: the OCCT 8 backend, behind the `occt` feature. It implements the whole `Kernel` trait through a cxx bridge: box, cylinder, sphere, planar profiles with holes, extrude, revolve, the booleans, fillet, chamfer, mass properties and tessellation. Every result is checked with `BRepCheck_Analyzer`, and OCCT exceptions come back as errors. Faces and edges get persistent names from OCCT's history. Rebuilding a bracket (block, pocket, hole, 4 fillets) at 500 random sizes kept every face and edge name, and the bracket takes 7.1 ms on the lab's i9-13900K. `scripts/occt` builds the pinned OCCT 8.0.1, and a new CI job runs the backend against it. STEP files read and write through OCCT's DataExchange module, with imported faces named `Imported(i)`: all 33 NIST PMI files read in 3.5 s, and a write and read back keeps volume and area to 1e-6. Mass properties measure only the solids of a shape that has any, so the loose support faces in an imported file do not count.
- `ocad-occt`: `nurbs::curve_derivatives` and `nurbs::surface_derivatives` evaluate a rational B-spline and its derivatives through OCCT's `Geom_BSplineCurve` and `Geom_BSplineSurface`, taking the data and returning the derivatives in the same shape `ocad-forge` does, for the differential harness. On 300 random curves and 100 random surfaces the two kernels agree to 3e-14 relative on the point and the first two derivatives.
- `ocad-occt`: `Occt::heal` for imported shapes that fail BRepCheck, following spec 05 section 10.3. It tries ShapeFix with its defaults, then a thorough pass that may change topology and geometry to fix crossing and self-intersecting wires, and keeps the first result that passes the check without growing a tolerance past 0.01 mm. `Occt::max_tolerance` reports the loosest vertex, edge or face. On the first 50,000 files of the ABC dataset, read through OCCT on the i9, 97.55% come in as valid solids, and 323 of those only after healing. Healing that got a shape through the check by growing a tolerance to 72 mm is refused, which is what the tolerance rule is for.
- `ocad-kernel`: the trait grows `make_cylinder`, `make_sphere`, `make_profile`, `fillet`, `chamfer`, `topology` and `properties`, each with a default that says it is not supported, and names grow the roles `SketchEntity`, `Fillet`, `Chamfer`, `Piece`, `Bounded` and `Imported`.
- `xtask`: `cargo xtask ci` no longer passes `--all-features`, and runs the OCCT backend when `OCCT_DIR` is set.
- `ocad-io`: a Part 21 reader for STEP, editions 2 and 3. `step::parse` checks the whole file and indexes its instances by id, and decodes a record only when asked, with the limits and escapes from spec 13 section 9.2. Anchor and reference sections are checked and skipped, and a signature is ignored. On one core of the lab server it indexes the 277 MB DrivAerML car body at 500 to 590 MB/s while other jobs run, against a target of 400, because most records are checked by one loop over their bytes and only the rest go token by token. All 33 NIST PMI files read with every reference resolved. The `scan` example measures any set of files.
- `ocad-io`: STL, binary and ASCII. `stl::read` tells them apart by size, reads every solid in an ASCII file as its own mesh, and refuses NaN, infinity and more than 200 M triangles. Binary writes back byte for byte and ASCII writes numbers that read back to the same bits. The DrivAerML body, 49 solids and 753,234 triangles in 142 MB of ASCII, reads at 189 MB/s, and the binary AhmedML body at about 1,000 MB/s.
- `fuzz/`: targets for Part 21, which also holds the fast path to the token by token answer, and for STL, with a dictionary and seeds from the NIST files.
- `ocad-ocl`: a sketch inside another sketch is refused with OCL0207 by the checker and the evaluator. Its points could name the outer sketch's points before that sketch was a feature, which stopped evaluation with a panic.
- `ocad-ocl`: a word after a sketch entity that names an argument of its call, as in `point(40, 0) x`, now stops evaluation with OCL0109. It used to become that argument given twice in the written OCL.
- `ocad-io`: the `.ocad` bundle, version 0. `bundle::write` and `bundle::read` handle the container from spec 13: the header, chunks addressed by BLAKE3, the index and the sealed trailer. A torn save still opens at the last complete trailer, and every flipped bit in a saved file is refused. `native::save` and `native::open` put a document in a bundle as a commit, its manifest and its OCL source, and reading never runs code. Every registered command and every example in the spec round-trips through a bundle byte for byte. On the lab server a save runs at 350 to 1,300 MB/s and an open at 320 to 1,400 MB/s depending on size, and opening costs about 1 us per chunk. Spec 13 section 2.6 has the version 0 layout.
- `fuzz/`: cargo-fuzz targets for the OCL parser and formatter, the checker and evaluator with the writer, and the bundle reader. The bundle target also reseals each input so mutations get past the hashes to the records. Inputs that found a bug are kept in `fuzz/regressions/` and run as ordinary tests.
- `ocad-ocl`: fixes from the first fuzzing runs. A doc comment ending in a space no longer formats to a different tree, a version written as a word, as in `schema part@r`, keeps touching its name when formatted, and a name that only a glob import from another file could define now stops evaluation with OCL0501 instead of evaluating to a model the writer could not say.
- `ocad-ocl`: `ocl lsp`, a language server over standard input and output. It publishes the parse error or the checker's diagnostics whenever a file changes, with secondary spans as related information, shows the type of a name or the signature of a function on hover, jumps to where a name is defined, and formats the file. `check::symbols` gives the same answers to other tools: every definition and every resolved use, with its type. It needs no new dependency, since the JSON it speaks is a small module of its own.
- `ocad-ocl`: `write` turns a document back into OCL. A sketch becomes a sketch block and any other feature a binding to the standard library call that makes it, inside `part` blocks when its name is qualified. Evaluating the result gives the same features, and a document OCL cannot say is refused with the feature and the reason. Every registered command now round-trips through it before and after the command, after undo and after redo, as do the five complete examples in the spec. This is the last check of M0-2.
- `ocad-ocl`: a lambda passed to the kernel, such as the predicate in `edges.where(|e| e.is_convex)`, is kept in the document as its body with the parameter standing for itself, and the features it mentions become inputs. Before this the document only said `fn`, so the selection was lost.
- `ocad-core`: the command examples make features shaped the way OCL makes them, so the round trip can check them.
- `ocad-ocl`: `format` and `ocl fmt`, the canonical layout for OCL. Two spaces of indent, one space around binary operators, compound units kept against their number, at most one blank line in a row, and every comment kept where it was. A call or list that does not fit in 100 columns puts one item on each line with a trailing comma, a list that is the only argument hugs the call, table rows line up by column, and comments after code on neighbouring lines line up. The formatter refuses to write anything whose syntax tree differs from the input's, and formatting its own output changes nothing, which the tests hold for every example in the spec. `ocl fmt --check` exits 1 on a file that is not formatted, for CI.

## 0.0.3

The sketch solver. `ocad-gcs` solves 2D sketches with DogLeg and Levenberg-Marquardt, reports redundant and conflicting constraints by name, keeps edits and drags on the branch the sketch was drawn on, and has been run against every sketch in the SketchGraphs validation split. Of 315,228 real Onshape sketches, 99.83% of those it can express solve from their recorded geometry and 99.75% from geometry moved by 1% of their size, and diagnosis finds no conflicts in any of them.

- `ocad-gcs`: the sketch model and the numeric solve. A sketch holds points, lines, circles and arcs, and the constraint set covers coincidence, horizontal and vertical, parallel and perpendicular, distances (point to point, point to line, horizontal and vertical), radius, angle, equal length and equal radius, point on line and on circle, midpoint, symmetry, and tangency between a line and a circle or between two circles. Every residual is written once over dual numbers, so the Jacobian is exact. The solve splits the sketch into independent parts, runs DogLeg with the shortest least squares step and falls back to Levenberg-Marquardt, and puts a part back where it was when it does not solve. Tangency at a shared endpoint uses its own equation, because the general one has a double root there and left a slot 1.7e-5 mm off.
- `ocad-gcs`: diagnosis. `Sketch::diagnose` reports the structural degrees of freedom, the numerical ones at the solved position, how far each entity can still move, groups of redundant constraints and minimal sets of conflicting ones, all sorted by id so an unchanged sketch always says the same thing. The structural count is the Jacobian's rank at made-up coordinates, because matching equations to parameters misses a distance repeated between the same two points. Redundancy groups come from fundamental circuits, so they do not depend on which independent rows the QR happens to pick, and conflicts are narrowed with a deletion filter.
- `ocad-gcs`: a part that fails from a start on a line of symmetry is tried once more from a small fixed nudge. A point midway between two fixed points had no gradient off the line joining them, so a solvable dimension edit was reported as a conflict.
- `ocad-gcs`: dimension edits stay on the branch the sketch was drawn on. `Sketch::edit` checks every triangle of pinned-down points before and after the solve, and when one turned over it walks the value there in 16 steps, and refuses the edit with GCS0107 if a triangle still turns over. Only triangles are checked, because the angle at a point between two ties can pass through straight as a chord moves, and checking those refused 199 of 202 edits that were fine. In 500 random edits to a truss, 10 of the 406 that could be solved flipped when solved in one go, the walk kept all 10, and none was refused. An edit that changes the sign of a signed dimension is meant to turn things over and is not checked.
- `ocad-gcs`: dragging. Each frame puts the point on the cursor and solves the rest for the shortest move from where the drag began, so dragging back puts everything back. When the cursor is out of reach, the point goes as close as the constraints let it, by steps along the constraints that each end solved. A frame costs 0.12 ms for a chain of 11 points on an M4 and 150 ms for 151, because every step is a dense QR, so dragging at the target size waits on the sparse solver.
- `ocad-gcs`: `Sketch::residual` says how far a sketch is from holding its constraints without solving it, so a harness can check that recorded geometry agrees with what the constraints say before it moves anything.
- `ocad-gcs`: when DogLeg does not converge, Levenberg-Marquardt starts from where the sketch was instead of where DogLeg stopped. A point that is both coincident with the end of a line and constrained onto it makes the linear model ask for a huge step that shrinks the line to nothing, and DogLeg followed it there. On 20,000 SketchGraphs sketches moved by 1% of their size, solves went from 99.31% to 99.71%.

## 0.0.2

The core can now regenerate a document. Commands, events and a parallel regeneration scheduler with a result cache sit on top of the document model, OCL evaluation no longer starts a thread per call, and Forge evaluates surfaces and derivatives and has the knot operations later geometry is built on. Every speedup listed here was found by a benchmark in opencad-bench and measured on the lab server.

- `ocad-core`: the command layer. Commands have an id, a version and typed arguments that describe themselves, validate before they change anything, and run inside a transaction that rolls back on failure and undoes as one step. A command can be called by id with its arguments as a record, which is how scripts and the agent will call them. The registry starts with `feature.add`, `feature.remove`, `feature.rename` and `feature.set`.
- `ocad-core`: the event bus. A session announces every commit, undo and redo with who made it and which features it touched, and regeneration will report progress and feature status on the same bus. Subscribers read from a 4096 event ring, and one that falls behind is told how many events it missed.
- `ocad-core`: the regeneration scheduler. Each feature gets a BLAKE3 fingerprint over its kind, the version of its evaluator, its parameters and its inputs' fingerprints, and results are cached by fingerprint, so an unchanged value, a rename or an undo costs no evaluation. What is left runs as a dynamic DAG on any number of threads with the same result as serial. A failed feature keeps its place in the error state and its dependents go stale with their last good result, and a panic in an evaluator is an error on that feature.
- `ocad-core`: the L1 command tests are generated from the registry. Every registered command is run by id on its own example and must round-trip its payloads, be one undo step, and undo and redo exactly.
- `ocad-core`: a regeneration worker keeps the next ready feature for itself and wakes other threads only for work it cannot take, so a chain of features stays on one core. A 300-feature chain regenerated on 10 threads now takes as long as on one thread, where before it was a third slower.
- `ocad-ocl`: the evaluator keeps its big-stack threads between evaluations instead of starting one per call. Evaluating a small part went from about 500 us to 50 us on the lab server, where starting a thread alone costs 650 us, and from 40 us to 19 us on an M4.
- `ocad-forge`: rational B-spline surfaces, and derivatives of any order for curves and surfaces, computed from the basis function derivatives and the quotient rule the way The NURBS Book and OCCT do it. Tests check them against finite differences on hundreds of random rational curves and surfaces, and against the exact tangent and curvature of a circle and a cylinder.
- `ocad-forge`: knot insertion, knot removal within a tolerance, and degree elevation, for curves and for surfaces in either direction. Property tests on hundreds of random rational curves and surfaces check that each one leaves the shape unchanged, that a knot that was inserted can be removed again to give back the original curve, and that elevation adds one copy of every interior knot per degree.

## 0.0.1

The first release. It does not build solids yet. What it has is the document model with undo, and OCL from source text to a document of features, with units checked on the way.

- The workspace, with one crate per entry in the crate map in `spec/01-foundations.md`, and the layer rule that keeps them in order.
- `ocad-core`: the document, the feature DAG, edits that return their own inverse, and undo and redo on top of that.
- `ocad-kernel`: the `Kernel` trait, persistent entity names and the D16 tolerances.
- `ocad-forge`: rational B-spline curve evaluation.
- `ocad-ocl`: the lexer, with units on numeric literals.
- `ocad-gcs`: sketch entities, constraints and the naive degree of freedom count.
- `opencadd`: `--version` and `--print-config`.
- The specification under `spec/`.
- `ocad-ocl`: `0..5` now lexes as a range, `..` is a punctuation token, and `1.2.3` is reported at its second `.` instead of failing as a bad number at the first digit (#83).
- `ocad-ocl`: the parser and the syntax tree. It covers the grammar in `spec/06-parametric-sketch-ocl.md` and the blocks other spec files add, and every OCL example in the spec parses. Errors carry a code, a span and a message that names what was expected. The lexer gains 36 units, `@attr` and `///` doc tokens, nestable block comments and string escapes (#53).
- `ocad-ocl`: lexing 1 MB of OCL takes about 7 ms instead of 13, and parsing it about 25 ms instead of 32, on an M-series laptop. The lexer no longer tries every punctuation string at every position, and the parser stops copying tokens and works out line breaks once.
- `ocad-ocl`: dimensions with rational exponents over the eight base dimensions, the quantity aliases from spec 06 such as `Length` and `Torque`, and the scale and offset of every unit into the internal mm, kg, s, rad and K system.
- `ocad-ocl`: the type checker. It resolves names in dependency order and reports cycles, checks units through arithmetic, comparisons, `sqrt`, trigonometry and powers, and checks calls against the standard library signatures in `prelude.ocl`. Diagnostics follow the style in spec 06 section 10.5, with a code, labelled spans and a hint. All 17 spec examples check without errors, and parsing and checking 5,000 lines takes about 6 ms on an M-series laptop (#53).
- `ocad-core`: features carry parameters as dimensioned values, and `Edit::Set` changes one and hands back the old value. Dimensions move from `ocad-ocl` into `ocad_core::quantity` so that both crates share them, and `ocad_ocl::dim` re-exports them (#53).
- `ocad-ocl`: the evaluator. It runs a checked file in dependency order and builds the document it describes: a call such as `extrude` or `fillet` becomes a feature named after its binding, a sketch becomes one feature holding its entities and constraints, and a reference such as `body.edges.lateral(profile.c)` becomes a path into a feature. Anything that needs geometry stays a call for the kernel, and a `requires` on it is reported as deferred. Parameters can be overridden within their `@range`, and fuel, call depth, overflow, division by zero and missing rows each stop evaluation with their own code. The five complete examples in the spec evaluate (#53).
