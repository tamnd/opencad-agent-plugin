# From CadQuery, build123d and OpenSCAD to OCL

OCL is not a chain of calls. Each feature has a name, each sketch keeps its dimensions, and a feature changes a body with `into` and `op`. So a translation is not line by line. Find the design intent first, then write it as sketches and features.

## Map of the common calls

| CadQuery | build123d | OpenSCAD | OCL |
| --- | --- | --- | --- |
| `cq.Workplane("XY").box(60, 40, 10)` | `Box(60, 40, 10)` | `cube([60, 40, 10], center = true)` | `box(x = 60 mm, y = 40 mm, z = 10 mm)`. The box starts at the origin, so give `at = pt3(-30 mm, -20 mm, -5 mm)` for a box at the center |
| `.circle(5).extrude(20)` | `extrude(Circle(5), 20)` | `cylinder(r = 5, h = 20)` | `cylinder(r = 5 mm, h = 20 mm)` |
| `.sphere(10)` | `Sphere(10)` | `sphere(10)` | `sphere(r = 10 mm)` |
| `.rect(60, 40).extrude(10)` | `extrude(Rectangle(60, 40), 10)` | `linear_extrude(10) square([60, 40])` | A sketch with four lines and their dims, then `extrude(s.region, blind(10 mm))` |
| `.extrude(10, both = True)` | `extrude(..., 5, both = True)` | `linear_extrude(10, center = true)` | `extrude(s.region, symmetric(10 mm))` |
| `.revolve(360, (0, 0, 0), (0, 1, 0))` | `revolve(..., Axis.Y)` | `rotate_extrude()` | `revolve(s.region, axis = Y)` |
| `.cutThruAll()` | `extrude(..., mode = Mode.SUBTRACT)` | `difference() { ... }` | `extrude(s.region, through_all, into = body, op = cut)` |
| `.union(other)` | `a + b` | `union() { ... }` | `op = add` with `into = body` on the second feature |
| `.cut(other)` | `a - b` | `difference() { ... }` | `op = cut` with `into = body` |
| `.intersect(other)` | `a & b` | `intersection() { ... }` | `op = intersect` with `into = body` |
| `.edges("\|Z").fillet(2)` | `fillet(part.edges().filter_by(Axis.Z), 2)` | `minkowski()` or `hull()` | `fillet(body.edges.lateral(s.a), r = 2 mm)`, one edge for each sketch corner, or a list of them |
| `.chamfer(1)` | `chamfer(..., 1)` | none | `chamfer(edges, mode = equal(1 mm))`. The modes `distances(a, b)` and `angled(d, angle)` give the other forms |
| `.faces(">Z").shell(-2)` | `offset(..., -2, openings = top)` | none | `shell(body, remove = body.faces.end_cap, thickness = 2 mm)` |
| `.faces(">Z").workplane().hole(5)` | `Hole(2.5)` | `difference()` with a cylinder | A sketch of circles on the face plane, then `hole(profile = s, into = body, dia = 5 mm, depth = through_all)` |
| `.cboreHole(5, 9, 4)` | `CounterBoreHole(2.5, 4.5, 4)` | none | `hole(..., cut = counterbore, cut_dia = 9 mm, cut_depth = 4 mm)` |
| `.rarray(10, 0, 4, 1)` | `GridLocations(10, 0, 4, 1)` | `for (i = [0:3]) translate([i * 10, 0, 0])` | `linear_pattern(seeds = [boss], dir1 = X, count1 = 4, spacing1 = 10 mm)` |
| `.polarArray(20, 0, 360, 6)` | `PolarLocations(20, 6)` | `for (a = [0:60:300]) rotate(a)` | `circular_pattern(seeds = [boss], axis = Z, count = 6)` |
| `.mirror("YZ")` | `mirror(..., Plane.YZ)` | `mirror([1, 0, 0])` | `mirror(seeds = [feature], plane = YZ)` |
| `.loft()` | `loft()` | none | `loft(sections = [a, b])` |
| `.sweep(path)` | `sweep(path = p)` | none | `sweep(s.region, path = run)` |
| `.translate((x, y, z))` | `Pos(x, y, z) * part` | `translate([x, y, z])` | `at = pt3(x, y, z)` on a primitive, or draw the sketch on `plane(XY, offset = z)` |
| Variables in Python | Variables in Python | Variables | `param` for a value the user can change, `let` for a value you work out |
| `assert` in Python | `assert` in Python | `assert()` | `requires cond, "R1 message"`, which each build checks |

## What does not translate line by line

- A selector string, such as `">Z"` or `"|Z"`, picks faces by their place now. OCL names a face by the sketch entity that made it, so the name stays the same when a size changes. Find the sketch entity that makes each face that you select.
- A loop in Python or OpenSCAD that makes copies is a `linear_pattern` or a `circular_pattern`. A loop that makes different parts is one part for each in OCL.
- `hull()` and `minkowski()` of OpenSCAD have no direct form. Draw the outline in a sketch, or use a fillet on the edges.
- A thread made by a helix in CadQuery is `helix` with `op = cut` in OCL. See recipes/thread.md.
- A gear from a gear library is `gear` in OCL. See recipes/gear.md.
