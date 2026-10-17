# Shell

`shell` hollows a body to a wall of `thickness`. The faces in `remove` stay open. With no `remove`, the body is hollow and closed.

```ocl
ocl 1.0

param wall: Length = 2 mm @range(1 mm, 4 mm)

part tray {
  sketch base on XY {
    a = point(0 mm, 0 mm) fixed
    b = point(40 mm, 0 mm) fixed
    c = point(40 mm, 30 mm) fixed
    d = point(0 mm, 30 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  body = extrude(base.region, blind(20 mm))
  cup = shell(body, remove = body.faces.end_cap, thickness = wall)
  scoop = shell(body, remove = [body.faces.end_cap, body.faces.lateral(base.ab)], thickness = wall)
}
```

- `remove` takes one face or a list. `body.faces.end_cap` is the top of an extrude, and `body.faces.lateral(base.ab)` is the side that the line `ab` made.
- The wall goes into the body. Give `outward = true` to keep the inner size and grow the outer size.
- Give `join = intersection` for sharp outer corners. The default `arc` rounds them.
- Shell before small fillets. A wall that is thicker than the smallest concave radius makes the shell fail.
- Put the wall in a `param` and check it with `requires`, so that a change to the wall cannot make the part too weak.
