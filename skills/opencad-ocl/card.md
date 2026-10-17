# OCL card

This card is the short form of OCL for an agent. The MCP server `opencadd mcp` gives it as the resource `ocad://docs/card`. It is made from [the OCL guide](https://github.com/tamnd/opencad/blob/main/docs/ocl/language.md) and the stdlib index, so do not edit it. Run the tests of ocad-ocl with `OC_BLESS=1` to make it again.

Before you use a function that is not on this card, call `docs_search` for it. After OCL0108, OCL0110 or OCL0111, call `docs_search` for the function of the call.

## Five rules

1. Write the unit on each length and each angle, as in `6 mm` and `30 deg`. A bare number is read as millimetres or radians, and it gives the warning OCL0102.
2. Name a face or an edge by its role, as in `body.faces.end_cap`, and not by a number. The name stays on the same face when the sizes change.
3. Write a `requires` line for each requirement of the design, so that each build checks it.
4. Put one part in a file.
5. Build the model after each feature, and correct the first error before you add the next feature.

## A file

```ocl
ocl 1.0

// A plate with a pocket. Everything after `//` to the end of the line is a comment.
param depth: Length = 4 mm

part plate {
  sketch base on XY {
    a = point(0 mm, 0 mm) fixed
    b = point(100 mm, 0 mm) fixed
    c = point(100 mm, 60 mm) fixed
    d = point(0 mm, 60 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  sketch slot on XY {
    a = point(50 mm, 20 mm) fixed
    b = point(80 mm, 20 mm) fixed
    c = point(80 mm, 40 mm) fixed
    d = point(50 mm, 40 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  /* The plate is 10 mm thick and the pocket is cut into its top. */
  body = extrude(base.region, blind(10 mm))
  pocket = extrude(slot.region, blind(depth), into = body, op = cut)
}
```

This builds two bodies: `plate.body`, a 100 × 60 × 10 block of 60000 mm3, and `plate.pocket`, the same with a 30 × 20 × 4 pocket cut out of it, 57600 mm3.

The parts of a file:

- `ocl 1.0` is the first line and names the version of the language. Write it in every file. A file without it, or with a version other than `1.0`, is error OCL0003, and the file does not build.
- `param` and `let` lines give names to values, at the top of the file or inside a part.
- `part name { ... }` holds one part. A file can hold more than one.
- Statements end at the end of the line, or at `;` to put two on one line. Inside `( )` and `[ ]` a statement can run over several lines, so a long call can be split after a comma.

Comments come in three kinds. `//` runs to the end of the line. `/* ... */` can span lines, and one can sit inside another. `///` is a doc comment, kept with the statement under it so that editors can show it.

A `use std...` line at the top is accepted, but the standard functions are always in scope, so no file needs one.

A `use` line can also take values from another file of the project or from a package. See [Files and packages](https://github.com/tamnd/opencad/blob/main/docs/ocl/language.md#files-and-packages).

## Sketches in brief

```text
sketch base on XY {
  a = point(0 mm, 0 mm) fixed
  b = point(40 mm, 0 mm)
  ab = line(a, b)
  horizontal(ab)
  dim distance(a, b) = 40 mm
}
```

A sketch is `sketch name on support { ... }`. The support is a plane (see [Planes](https://github.com/tamnd/opencad/blob/main/docs/ocl/language.md#planes)) or a flat face of a body. Inside it are the curves, each with a name, the constraints, and `dim` lines that set a distance, radius or angle. After a point or curve, `fixed` pins it, `construction` keeps it out of the profile, and `defining` marks it as the geometry the sketch is built around. Any other word there is `OCL0208`. A sketch cannot hold another sketch (`OCL0207`).

From outside the sketch, `base.region` is the area its curves bound, which is what `extrude` and `revolve` take, and `base.a` or `base.ab` is one of its points or curves. Inside a sketch a number with no unit is in millimetres, though writing `mm` is clearer. [Sketches](https://github.com/tamnd/opencad/blob/main/docs/sketches/sketches.md) covers the curves and regions, [Constraints and dimensions](https://github.com/tamnd/opencad/blob/main/docs/sketches/constraints.md) the constraints, and [External geometry](https://github.com/tamnd/opencad/blob/main/docs/sketches/external-geometry.md) bringing body edges into a sketch.

## Numbers and units

A number is written with its unit: `10 mm`, `2.5 in`, `30 deg`. The unit can touch the number (`10mm`) or follow a space. Long numbers can use `_` between digits (`12_000 mm`), and exponents work (`1.5e3 mm`, `1e-3 m`). Inside OpenCAD lengths are in millimetres and angles in radians, so `1 in` is 25.4 mm and `90 deg` is π/2.

| Kind | Units |
| --- | --- |
| Length | `nm`, `um`, `mm`, `cm`, `m`, `km`, `in`, `ft`, `yd`, `mil` |
| Angle | `deg`, `rad`, `grad`, `arcmin`, `arcsec`, `turn` |
| Mass | `g`, `kg`, `t`, `lb` |
| Force | `N`, `kN`, `lbf` |
| Pressure | `Pa`, `kPa`, `MPa`, `GPa`, `bar`, `psi` |
| Time | `s`, `ms`, `min`, `h` |
| Temperature | `degC`, `degF`, `K` |

Units combine with `*`, `/` and `^`, as in `7.85 g/cm^3`, `20 N*m` or `9.81 m/s^2`. A word that is not a unit is an error: `3 fur` gives `` `fur` is not a unit OCL knows ``. `degC` and `degF` cannot be combined with other units, since they do not start at zero.

Arithmetic keeps track of units. `10 mm + 2 mm` is `12 mm`, `10 mm * 2` is `20 mm`, `10 mm * 2 mm` is an area, and `10 mm + 2 kg` is an error (`OCL0103`). A value of the wrong kind for a parameter, such as an angle where a length goes, is `OCL0101`.

A number with no unit outside a sketch, where a length or angle is expected, is the warning `OCL0102`, with the fix in its help text: ``write the unit, as in `6 mm` ``. The number is still read as millimetres or radians, but the warning is there because a bare `6` is easy to misread.

These functions are built in: `sqrt`, `abs`, `round`, `floor`, `ceil`, `min`, `max`, `clamp`, `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2` and `lerp`, and the constant `PI`. They keep units where that makes sense: `sqrt(9 mm^2)` is `3 mm`, and `max(1 mm, 2 mm, 3 mm)` is `3 mm`. `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2` and `^` give the double nearest the true answer, so a file evaluates to the same bits on macOS, Linux, Windows and in the browser, and sketches solve the same way on each. The one known exception is `tan` of a number around 1e193, far past any angle a model has. Comparisons (`<`, `<=`, `==`, `!=`, `>=`, `>`) and `and`, `or` and `not` give `true` or `false`.

## let and param

```text
let name = value
let name: Type = value
param name: Type = default
param name: Type = default @range(low, high)
```

`let` names a value, so it can be used in several places and changed in one. `param` does the same and also marks the value as a parameter of the model: one a person or a program is meant to change, such as a width or a wall thickness. A parameter must have a type. `@range(low, high)` after the default limits the values it accepts, and a value outside the range is `OCL0308`.

```ocl
ocl 1.0

param width: Length = 60 mm @range(30 mm, 120 mm)
param wall: Length = 2 mm
let depth = 20 mm

part cup {
  sketch base on XY {
    a = point(0 mm, 0 mm) fixed
    b = point(40 mm, 0 mm)
    c = point(40 mm, 30 mm)
    d = point(0 mm, 30 mm)
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
    horizontal(ab, cd)
    vertical(bc, da)
    dim distance(a, b) = width
    dim distance(a, d) = 30 mm
  }
  body = extrude(base.region, blind(depth))
  hollow = shell(body, remove = body.faces.end_cap, thickness = wall)
}
```

With the defaults the block is 60 × 30 × 20, 36000 mm3, and the shell leaves 36000 - 56 × 26 × 18 = 9792 mm3.

The types are the names of kinds of quantity and a few others:

| Type | What it holds |
| --- | --- |
| `Length`, `Area`, `Volume`, `Angle` | Geometry: `10 mm`, `5 mm^2`, `30 deg` |
| `Mass`, `Time`, `Temperature`, `Density`, `Force`, `Torque`, `Pressure`, `Stress`, `Velocity`, `Acceleration` | Other physical quantities |
| `Scalar` | A number with no unit |
| `Int`, `Bool`, `Str` | A whole number, `true` or `false`, a string in double quotes |
| `[T]` | A list of `T`, such as `[Length]` |
| `T?` | A `T` that may be left out |
| `Enum(a, b, c)` | One of the listed names, such as `Enum(thin, thick)` |
| `Any` | Anything |

`opencadd build` builds with the defaults. Names are looked up in the part first and then in the file, so a part can use a parameter from the top of the file, as `cup` uses `width` above.

## References to faces and edges

Fillets, chamfers, shells, drafts, measurements and datums act on particular faces or edges of a body. OCL names them by where they came from, not by a number, so the name still points at the same face when the sketch changes size. A reference is a feature, then `faces` or `edges`, then a selector:

| Reference | What it picks |
| --- | --- |
| `body.faces.end_cap` | The face where the sweep ended: the top of an extrude on `XY`. |
| `body.faces.start_cap` | The face where the sweep started, on the sketch plane. |
| `body.faces.lateral(s.ab)` | The side face swept from the sketch curve `ab`. More than one curve picks all their faces. |
| `body.faces.side` | Each side face of the sweep. `body.faces.side(s.ab)` is the same as `body.faces.lateral(s.ab)`. |
| `round.faces.blend` | Each face that the fillet or chamfer `round` made. `round.faces.blend(e)` picks only the faces along the edges that `e` picks, such as `round.faces.blend(body.edges.lateral(s.a))`. |
| `housing.faces.inner` | Each face of the cavity that the shell `housing` made, offset from a face of its body. `housing.faces.inner(f)` picks only the faces offset from the faces that `f` picks, such as `housing.faces.inner(body.faces.start_cap)` for the floor. |
| `body.edges.lateral(s.a)` | The edge swept from the sketch point `a`, where two sides meet. More than one point picks all their edges. |
| `body.edges.between(f1, f2)` | The edges where a face that `f1` picks meets a face that `f2` picks, such as `body.edges.between(body.faces.end_cap, body.faces.lateral(s.ab))`. The two faces can come from different features. |
| `body.edges.at(x, y, z)` | The edge that passes through the point (x, y, z) in model space. It picks an edge of an imported body, which has no sketch to name its edges by. See [External geometry](https://github.com/tamnd/opencad/blob/main/docs/sketches/external-geometry.md). |
| `body.faces.at(x, y, z)` | The face that the point (x, y, z) in model space is on. It picks a face of an imported body, such as the face that a sketch goes on, as in `sketch lid on windsor_0.faces.at(48.881 mm, 66.696 mm, 343.417 mm) { ... }`. |
| `body` | The whole body, for measurements and datums. |

These are all the selectors there are today. The faces and edges carry the name of the feature that swept them, so `body` in a reference must be the extrude or revolve that made them, and `s` must be the sketch it swept. A blend face carries the name of its fillet or chamfer and the edge it rounds, and an inner face carries the name of its shell and the face it is offset from, so `blend` and `inner` pick the same faces when a radius, a wall thickness or a sketch dimension changes. A later feature that changes the body keeps the names: after `cut = extrude(..., into = body, op = cut)`, the top is still `body.faces.end_cap`, and a face the cut split in two gives both pieces.

## Ten features

- `body = extrude(profile.region, blind(10 mm))`: Pads the region of a sketch along its normal. With `into = body, op = cut` it cuts a pocket.
- `body = revolve(profile.region, axis = Y)`: Turns the region of a sketch round an axis, all the way round or by `extent = angle(90 deg)`.
- `round = fillet(body.edges.lateral(profile.c), r = 8 mm)`: Rounds edges with the radius `r`.
- `edged = chamfer(body.edges.lateral(profile.a), mode = equal(3 mm))`: Cuts edges back by a distance, two distances or a distance and an angle.
- `cup = shell(body, remove = body.faces.end_cap, thickness = 2 mm)`: Hollows a body to walls of `thickness`, open at the faces in `remove`.
- `holes = hole(profile = bolts, into = body, dia = 6.6 mm, depth = through_all)`: Drills a hole at each circle and point of a sketch, with an optional counterbore, countersink or thread.
- `row = linear_pattern(seeds = [boss], dir1 = X, count1 = 4, length1 = 60 mm)`: Copies features along one or two directions.
- `ring = circular_pattern(seeds = [boss], axis = Z, count = 6, angle = 360 deg)`: Copies features round an axis.
- `flip = mirror(seeds = [boss], plane = plane(YZ, offset = 50 mm))`: Copies features to the other side of a plane.
- `block = box(x = 40 mm, y = 30 mm, z = 20 mm)`: A block with its lowest corner at the origin, or at `at`. `cylinder`, `sphere`, `cone` and `torus` are the other primitives.

For the other features, call `docs_search`, or read the [examples](https://github.com/tamnd/opencad/blob/main/docs/ocl/examples.md).
