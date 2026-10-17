# Loft

`loft` makes a solid through the regions of two or more sketches, in order. It is smooth unless `ruled = true`. Draw each section on its own plane.

```ocl
ocl 1.0

part tower {
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
  sketch top on plane(XY, offset = 30 mm) {
    a = point(5 mm, 5 mm) fixed
    b = point(15 mm, 5 mm) fixed
    c = point(15 mm, 15 mm) fixed
    d = point(5 mm, 15 mm) fixed
    ab = line(a, b)
    bc = line(b, c)
    cd = line(c, d)
    da = line(d, a)
  }
  tower = loft(sections = [base, top], ruled = true)
  round = fillet(tower.edges.lateral(base.a), r = 1 mm)
}
```

- Give each section the same count of vertices, in the same order. A different count or order twists the loft.
- `ruled = true` gives flat sides between the sections. With more than two sections, a smooth loft can bulge out past them.
- The first or the last section can be a point, such as a `wire_point`, for a loft that ends in a tip.
- To add the loft to a body, give `into = body` and `op = add`, and start the first section on a face of the body.
- `solid = false` gives the sides only, as a sheet.
