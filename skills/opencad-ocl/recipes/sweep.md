# Sweep

`sweep` moves the region of a sketch along a path. The path is a second sketch of lines and arcs, joined end to end. The profile must be at the start of the path and across it.

```ocl
ocl 1.0

part pipe {
  sketch p on XY {
    a = point(-2 mm, -2 mm) fixed
    b = point(2 mm, -2 mm) fixed
    c = point(2 mm, 2 mm) fixed
    d = point(-2 mm, 2 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  sketch run on XZ {
    o = point(0 mm, 0 mm) fixed
    a = point(0 mm, 30 mm) fixed
    c = point(10 mm, 30 mm) fixed
    b = point(10 mm, 40 mm) fixed
    up = line(o, a)
    bend = arc(center = c, start = b, end = a)
  }
  pipe = sweep(p.region, path = run)
}
```

- Start the path where the profile is, here at the origin. A profile away from the start of the path gives a shape that you did not intend.
- Make the path smooth. A bend radius smaller than the profile makes the sweep cut into itself and fail.
- At a sharp corner of the path, `transition = right` runs each leg on until they meet, and `transition = round` turns the profile about the corner.
- `sections = [q.region]` changes the profile on the way, as a taper.
- `frenet = true` turns the profile with the path. Use it for a path that is not in one plane.
