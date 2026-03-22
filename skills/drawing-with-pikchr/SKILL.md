---
name: generating-pikchr
description: Generates Pikchr diagram code from descriptions. Pikchr is a text-based diagram language (PIC descendant) that renders to SVG. Use when asked to create flowcharts, architecture diagrams, syntax diagrams, or any technical illustration using Pikchr syntax. Covers object primitives, positioning, styling, and layout patterns.
---

# Generating Pikchr Diagrams

Pikchr is a diagram scripting language that compiles to SVG. Scripts are sequences of statements separated by newlines or semicolons.

## Quick Reference

### Object Primitives

```pikchr
box "label"           # rectangle
circle "label"        # circle  
ellipse "label"       # ellipse
oval "label"          # pill shape (horizontal)
cylinder "label"      # database icon
file "label"          # document icon
diamond "label"       # decision node
dot                   # small filled circle
line                  # line segment
arrow                 # line with arrowhead
spline                # curved line through points
arc                   # circular arc
text "label"          # standalone text
```

### Layout Direction

Default is `right`. Change with: `down`, `left`, `up`, `right`

```pikchr
down
box "A"; arrow; box "B"; arrow; box "C"
```

### Positioning

Objects auto-stack in layout direction. Override with:

```pikchr
box "A"
box "B" at 1in right of A          # absolute offset
box "C" with .nw at A.se           # anchor alignment
box "D" at (A, B)                  # X from A, Y from B
```

### Anchor Points

Every object has anchors: `.n`, `.ne`, `.e`, `.se`, `.s`, `.sw`, `.w`, `.nw`, `.c` (center), `.start`, `.end`

```pikchr
A: box "Source"
B: box "Target" at 2in right of A
arrow from A.e to B.w
```

### The "until even with" Pattern

Extend lines to align with another object (Pikchr's killer feature):

```pikchr
A: box "Start"
B: box "End" at 2in right of A and 1in below A
arrow from A.s down 0.3in then right until even with B then to B.n
```

### Sizing

```pikchr
box width 2in height 0.5in         # explicit dimensions
box "Long Label" fit               # auto-size to text
circle radius 0.3in
```

### Styling

```pikchr
box "styled" color blue fill lightyellow thick dashed
arrow -> thick                     # arrow at end
arrow <->                          # arrows both ends
line dotted thin
box invis "floating text"          # invisible box for positioning
```

### Colors

Use 140 HTML color names or hex: `color red`, `fill 0xc6e2ff`, `color 0x336699`

### Text Attributes

```pikchr
box "above" above "below" below    # vertical position
box "left" ljust "right" rjust     # horizontal justify
text "bold" bold "italic" italic
text "small" small "big" big
line "aligned" aligned             # text follows line angle
```

### Variables and Expressions

```pikchr
$gap = 0.5in
boxwid = 1.5in                     # default box width
boxht = 0.4in                      # default box height
linewid *= 0.5                     # shorten default line length
scale = 0.8                        # scale entire diagram
```

### Labels and References

```pikchr
Start: box "Begin"                 # label with uppercase first letter
arrow from Start.e                 # reference by label
arrow from previous.end            # reference previous object
arrow from 2nd box.s               # ordinal reference
arrow from last circle.n           # "last" keyword
```

### Macros

```pikchr
define component { box $1 fit rad 5px fill $2 }
component("API", lightyellow)
component("DB", lightblue)
```

## Common Patterns

### Flowchart Decision

```pikchr
D: diamond "condition?" fit
box "yes path" with .n at 0.5in below D.s
box "no path" with .w at 0.5in right of D.e
arrow from D.s to first box.n "Yes" aligned
arrow from D.e to last box.w "No" above
```

### Parallel Branches (Fanout)

```pikchr
Start: box "Process"
A: box "Task A" at 1in below Start and 0.8in left of Start
B: box "Task B" at 1in below Start
C: box "Task C" at 1in below Start and 0.8in right of Start
arrow from Start.s down 0.3in then left until even with A then to A.n
arrow from Start.s to B.n
arrow from Start.s down 0.3in then right until even with C then to C.n
```

### Labeled Connection

```pikchr
A: box "Source"
B: box "Target" at 2in right of A
arrow from A.e to B.w "data flow" above
```

### Legend Box

```pikchr
box "Diagram content..." width 3in height 2in

# Legend in corner
L: box with .nw at 0.2in below previous.sw width 1.2in height 0.8in thin
text "Legend" bold with .n at 0.1in below L.n
box width 0.3in height 0.15in fill lightblue at 0.15in below last text and 0.2in left of L.c
text "Type A" with .w at 0.05in right of previous.e
```

### Background Regions

```pikchr
# Draw content first, then background behind
A: box "Frontend"
B: box "Backend" at 1.5in right of A
box width 2.5in height 1in fill 0xe8e8e8 \
    with .c at 0.5 way between A and B \
    behind A
```

For detailed reference, see:
- `reference/grammar.md` - complete grammar specification
- `reference/objects.md` - all object types and properties
- `reference/positioning.md` - positioning, places, and anchors
- `reference/flowcharts.md` - flowchart-specific patterns and examples
- `examples/notification-flow.md` - complex multi-category flowchart with legend
