# OCL style

The stage `style` of model_check passes when each file is in the format of `ocl fmt`. Run `ocl fmt FILE` to format a file in place, or `ocl fmt --check FILE` to only check it. The other rules here are for a person who reads the file.

## Layout

- Indent with two spaces. Put one statement on each line.
- Put a blank line after `ocl 1.0`, after the params and between parts.
- Put the params and the `let` values of the file at the top, before the first part.
- Put the `requires` lines of a part next to the geometry that they limit. A check that measures the last body goes at the end of the part.

## Names

- Use lower case words with `_`, such as `floor_holes` and `inner_corner`.
- Name a feature by what it is in the design, not by the function: `corner = fillet(...)`, not `fillet1 = fillet(...)`.
- Name a sketch by its place or its role: `profile`, `floor`, `wall`.
- In a sketch, name the points with one letter and the lines by their two points, as `ab = line(a, b)`, or by their role, as `base` and `riser`. A reference such as `profile.c` then reads well.
- Start the message of a `requires` with the id of the requirement, such as `"R2 width"`.

## Values

- Make a value a `param` when the user can change it, and give it a `@range`.
- Make a value a `let` when you work it out from other values.
- Give each dimension in the sketch with `dim`, not in the points alone. The dimension is the design intent, and the point is a start for the solver.

## Comments

- Use `//` comments to give the reason for a value, such as `// ISO 273 medium clearance for M5`.
- Do not write a comment that says what the code already says.

## Size

- Keep a file short enough to read in one go. Split a large part into files in parts/ and interfaces/.
