# Pikchr Positioning Reference

Complete guide to positioning objects in Pikchr diagrams.

## Anchor Points (Places)

Every object has named anchor points. For block objects:

```
         .n / .north / .t / .top
                  |
    .nw ----+-----+-----+---- .ne
            |           |
  .w/.left -+    .c     +- .e/.right  
            |  .center  |
    .sw ----+-----+-----+---- .se
                  |
        .s / .south / .bot / .bottom
```

Line objects also have `.start` and `.end` at their endpoints.

Lines have numbered vertices: `1st vertex of L`, `2nd vertex of L`, etc.

## Position Expressions

### Absolute Coordinates (avoid if possible)

```pikchr
box at 2in, 1in                    # x=2in, y=1in from origin
```

### Relative to Object

```pikchr
A: box "A"
box at A                           # at A's center
box at A.ne                        # at A's northeast corner
box at A + (0.5in, -0.3in)         # offset from A's center
box at A.e + (0.5in, 0)            # offset from A's east
```

### Directional Offset

```pikchr
A: box "A"
box at 1in right of A              # 1 inch to the right
box at 0.5in above A.n             # above north anchor
box at 2cm east of A               # compass direction
box at 1in heading 45 from A       # angle from north (clockwise)
```

### Anchor Alignment with "with"

```pikchr
A: box "A"
box "B" with .nw at A.se           # B's northwest at A's southeast
box "C" with .w at 0.5in right of A.e
```

### Coordinate Extraction

```pikchr
A: box "A" at 1in, 2in
B: box "B" at 3in, 0.5in
box at (A, B)                      # x from A, y from B → (1in, 0.5in)
box at (B.x, A.y)                  # explicit coordinate access
```

### Fractional Position

```pikchr
A: box "A"
B: box "B" at 2in right of A
dot at 1/2 way between A and B     # midpoint
dot at 1/3 <A, B>                  # shorthand: 1/3 from A toward B
dot at 0.75 between A.ne and B.sw  # between specific anchors
```

## The "until even with" Pattern

Pikchr's most powerful positioning feature for Manhattan-style routing:

```pikchr
A: box "Source"
B: box "Target" at 2in right of A and 1in below A

# Extend right until x-coordinate matches B
arrow from A.e right until even with B then to B.n

# Extend down until y-coordinate matches B  
arrow from A.s down until even with B then to B.w
```

Use with `then` for multi-segment paths:

```pikchr
A: box "A"
B: box "B" at 2in right of A and 1.5in below A

arrow from A.s \
    down 0.3in \
    then right until even with B \
    then down until even with B \
    then to B.n
```

## Path Attributes

### From and To

```pikchr
A: box "A"
B: box "B" at 2in right of A
arrow from A.e to B.w              # explicit start and end
arrow from A to B chop             # chop = stop at boundaries
```

### Direction and Distance

```pikchr
arrow right 2in                    # direction + distance
arrow down 1in then right 1in      # multi-segment
arrow go 1in heading 45            # angle from north
```

### Combined Directions

```pikchr
arrow right 2in up 1in             # diagonal (combined)
arrow right 2in then up 1in        # L-shape (separate segments)
```

## Layout Direction

Default layout direction is `right`. Objects stack in layout direction:

```pikchr
right                              # default
box "1"; box "2"; box "3"          # stacks left-to-right
```

```pikchr
down
box "A"; arrow; box "B"; arrow; box "C"
```

Layout direction affects `.start` and `.end` on block objects:

| Direction | .start | .end |
|-----------|--------|------|
| right     | .w     | .e   |
| down      | .n     | .s   |
| left      | .e     | .w   |
| up        | .s     | .n   |

## Object References

### By Label

```pikchr
Start: box "Begin"
arrow from Start.e                 # uppercase label
```

### By Ordinal

```pikchr
box "A"; box "B"; box "C"
arrow from 1st box to 2nd box      # 1st, 2nd, 3rd...
arrow from 3rd box.s down 0.5in
```

### Relative

```pikchr
box "A"
arrow from previous.e right 1in    # "previous" = last object
box "B"
arrow from last box.e              # "last" = most recent of type
```

### Scoped (in subdiagrams)

```pikchr
A: [
    box "inner1"
    box "inner2"
]
arrow from A.1st box.e to A.2nd box.w
```

## Distance Functions

```pikchr
A: box "A"
B: box "B" at 2in right of A
C: circle at dist(A,B) heading 60 from A   # same distance, different angle
line from A to C "dist = " & dist(A,B) above
```

## Expressions in Positions

```pikchr
$gap = 0.3in
$cols = 3

A: box "A"
B: box "B" at ($cols * 0.8in) right of A
C: box "C" at 1/2 way between A and B and $gap below A
```
