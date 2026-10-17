# Gear and standard parts

`gear` makes a spur gear with involute teeth. It stands `h` up from XY, round Z. The pitch circle is `module` times `teeth` across. `fastener` makes a bolt, a nut or a washer to a standard.

```ocl
ocl 1.0

param teeth: Int = 24 @range(12, 80)

part drive {
  requires teeth >= 12, "R1 no undercut at 20 deg"
  wheel = gear(teeth = teeth, module = 2 mm, h = 8 mm)
  bore = cylinder(r = 4 mm, h = 8 mm, into = wheel, op = cut)
}

part hardware {
  bolt = fastener(standard = "ISO4017", size = "M6", length = 20 mm)
}
```

- Two gears mesh when they have the same `module` and the same `pressure`. Put the centres `module * (teeth1 + teeth2) / 2` apart.
- A gear with few teeth has an undercut. Use 12 teeth or more, or give a `shift`.
- Cut the bore with a cylinder at the origin and `op = cut`. The gear is centred on Z.
- Call docs_search with the kind `standard_part` for the standards and the sizes of `fastener`. Never invent a size.
