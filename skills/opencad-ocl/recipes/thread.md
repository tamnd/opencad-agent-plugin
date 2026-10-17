# Thread and spring

`helix` sweeps the region of a sketch along a helix round `axis`. Give two of `pitch`, `height` and `turns`. Draw the profile on a plane through the axis, at the radius of the thread.

```ocl
ocl 1.0

part threaded {
  sketch tooth on XZ {
    a = point(9 mm, -1 mm) fixed
    b = point(11 mm, -1 mm) fixed
    c = point(11 mm, 1 mm) fixed
    d = point(9 mm, 1 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  coil = helix(tooth.region, axis = Z, pitch = 5 mm, height = 15 mm)
  rod = cylinder(r = 10 mm, h = 15 mm)
  thread = helix(tooth.region, axis = Z, pitch = 5 mm, height = 15 mm, into = rod, op = cut)
}
```

- `coil` is a spring: the helix alone is the body.
- `thread` cuts the groove of a thread into the rod with `op = cut`. Use `op = add` for a thread that stands out of the rod.
- Make the profile less tall than the pitch, or the turns touch and the build fails.
- `left = true` gives a left hand thread. `angle` gives a thread on a cone.
- For a standard metric thread on a bolt, use `fastener` with `thread = true`. For a tapped hole, use `hole` with `type = tapped`. Call docs_search for the sizes.
