# Draft

`draft` tilts faces by `angle` so that the part comes out of a mold. The faces turn about the `neutral` plane, which is XY when you do not give it. The pull direction is the normal of the neutral plane, or `pull`.

```ocl
ocl 1.0

part cast {
  sketch base on XY {
    a = point(0 mm, 0 mm) fixed
    b = point(20 mm, 0 mm) fixed
    c = point(20 mm, 20 mm) fixed
    d = point(0 mm, 20 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  body = extrude(base.region, blind(10 mm))
  sides = draft(body, faces = [body.faces.lateral(base.ab), body.faces.lateral(base.bc), body.faces.lateral(base.cd), body.faces.lateral(base.da)], angle = 5 deg)
  cup = shell(sides, remove = body.faces.end_cap, thickness = 1 mm)
}
```

- Name each face by the sketch line that made it. The faces stay the same when a size changes.
- Draft before the shell and before the fillets. A shell of a body with draft gives walls with draft on the two sides.
- The faces at the neutral plane keep their size. Give `neutral = plane(XY, offset = 10 mm)` to keep the top size and make the bottom smaller.
- For a mold, 1 deg to 3 deg is usual. A draft of 0 deg is not a draft, so use a `param` with a `@range` that starts above 0 deg.
