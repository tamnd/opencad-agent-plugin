---
name: opencad-ocl
description: Write, edit or fix OCL, the text language of OpenCAD models (.ocl files). Use for any change to an .ocl file, for an OCL error code such as OCL0110, and to translate CadQuery, build123d or OpenSCAD code to OCL.
---

# Writing OCL

Read card.md before your first edit in a session. It has the rules, the shape of a file, sketches, units, params, the references to faces and edges and the ten features that most models use. The MCP server gives the same text as the resource `ocad://docs/card`.

## Before you write a call

- Call docs_search for each function that is not on the card. It gives the signature, each parameter with its unit and default, and whether a call must give it. A default of `...` means the kernel picks the value.
- Never invent a function or a parameter. If docs_search does not find it, OCL does not have it. Tell the user, or build the shape from features that exist.
- Call docs_examples for the start of a new part. Each example builds with no error.

## The rules that most errors break

1. Write `ocl 1.0` as the first line.
2. Write the unit on each length and each angle: `6 mm`, `30 deg`, `2.7 g/cm^3`.
3. A feature is `name = function(...)`. Each name is new in its part. A later feature takes an earlier one with `into = name`.
4. To cut or add to a body, give `into = body` and `op = cut` or `op = add`. With no `op`, the feature is a new body.
5. Give each sketch all its dimensions and constraints, or mark its points `fixed`. A sketch with degrees of freedom left fails the stage `dof` of model_check.
6. Name a face or an edge by the sketch entity that made it, as `body.faces.lateral(profile.ab)` or `body.edges.lateral(profile.c)`, or by its role, as `body.faces.end_cap`. Do not use a number. Use ref_resolve to check what a reference picks.
7. Put each requirement in a `requires` line, with an id in its message: `requires mass(body, density = 2.7 g/cm^3) < 60 g, "R1 mass"`. A check that measures the model runs after the build.
8. Put a value that the user can change in a `param` with a type, a default and a `@range`. Put a value that you work out in a `let`.
9. Do not give a `param` or a `let` the name of a function, such as `diameter`. The name hides the function in the file, and a sketch dimension such as `dim diameter(c)` then fails.

## After an error

Read the code and the place in the diagnostic. Fix the first error only, then build again.

| Code | What to do |
| --- | --- |
| `OCL0102` | Add the unit to the number |
| `OCL0108` | Give the extra arguments by name. Call docs_search for the function |
| `OCL0110` | Use a parameter name from the signature. The message gives the nearest name |
| `OCL0111` | Give each parameter that has no default |
| `OCL0202` | The name is not defined. Check the spelling, or call docs_search |
| `OCL0203` | Give the feature a new name |
| A fillet, a shell or a boolean fails | The kernel cannot make the shape. Use the recovery in failures.md of the opencad-design skill |

For a code that is not in this table, call docs_search with the code, such as `OCL0305`. It gives the meaning and the fix.

## Style

Write each file in the format of `ocl fmt`, or the stage `style` of model_check fails. Read style.md for the rules of names, comments and layout.

## Hard features

Each recipe has a model that builds, the parameters that matter and the errors to expect:

- recipes/shell.md: hollow a body and keep a face open.
- recipes/draft.md: tilt faces for a mold.
- recipes/loft.md: a solid between two or more sections.
- recipes/sweep.md: a profile along a path.
- recipes/thread.md: a thread or a spring with `helix`.
- recipes/gear.md: a spur gear with a bore, and a bolt from `fastener`.

## From other CAD code

translate.md maps CadQuery, build123d and OpenSCAD to OCL. The main differences: OCL names each feature, keeps each dimension in the sketch, and adds a feature to a body with `into` and `op` in place of a chain of calls.
