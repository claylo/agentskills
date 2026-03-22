# Pikchr Object Reference

Complete reference for all Pikchr object types and their properties.

## Block Objects

Block objects are filled shapes that can contain text.

### box

Rectangle. Corners can be rounded with `radius`.

```pikchr
box                                # default size
box "Label"                        # with text
box "Multi" "Line"                 # multiple text lines
box width 2in height 0.5in         # explicit size
box "Auto" fit                     # size to fit text
box rad 10px                       # rounded corners
```

Default size: `boxwid` × `boxht` (0.75in × 0.5in)

### circle

```pikchr
circle                             # default
circle "Label"
circle radius 0.3in
circle diameter 0.6in
```

Default size: `circlerad` (0.25in)

### ellipse

```pikchr
ellipse
ellipse "Label" width 1in height 0.4in
```

Default size: `ellipsewid` × `ellipseht`

### oval

Pill shape (rectangle with semicircular ends).

```pikchr
oval "Terminal" fit
oval width 1.5in height 0.4in
```

### cylinder

Database icon.

```pikchr
cylinder "DB" fit
cylinder radius 0.3in
```

### file

Document icon (rectangle with folded corner).

```pikchr
file "Report" fit
```

### diamond

Decision node for flowcharts.

```pikchr
diamond "Yes/No?" fit
diamond width 1in height 0.8in
```

Note: Anchor points are rotated (`.n` is at top vertex).

### dot

Small filled circle, typically for connection points.

```pikchr
dot
dot radius 3px
dot color red
```

Default size: `dotrad` (0.05in)

### text

Standalone text (no shape).

```pikchr
text "Just text"
text "Bold" bold
text "Multi" "Lines"
```

## Line Objects

Line objects connect points.

### line

Simple line segment.

```pikchr
line                               # default direction, length
line right 2in                     # direction + length
line from A to B                   # between points
line -> right 1in                  # with arrowhead at end
line <-> right 1in                 # arrows both ends
```

Default length: `linewid` (0.5in)

### arrow

Line with arrowhead at end.

```pikchr
arrow                              # same as: line ->
arrow right 1in
arrow from A.e to B.w
```

### spline

Curved line through multiple points.

```pikchr
spline from A.e then right 0.5in then down 0.5in then to B.n
spline -> from A go right 0.3in then heading 45 then to B
```

### arc

Circular arc (90° by default).

```pikchr
arc                                # default arc
arc cw                             # clockwise
arc ccw                            # counter-clockwise
arc from A.e to B.n
```

### move

Invisible movement (advances position without drawing).

```pikchr
box "A"; move right 1in; box "B"
```

## Common Properties

### Size Properties

```pikchr
width 2in                          # or: wid
height 0.5in                       # or: ht
radius 0.3in                       # or: rad
diameter 0.6in
thickness 2px                      # line thickness
fit                                # auto-size to text
```

### Line Styles

```pikchr
thin                               # thinner line
thick                              # thicker line (stackable: thick thick)
solid                              # reset to solid
dashed                             # dashed line
dashed 0.1in                       # custom dash length
dotted                             # dotted line
dotted 0.05in                      # custom dot spacing
invis                              # invisible (for positioning)
```

### Colors

```pikchr
color red                          # stroke color
fill lightblue                     # fill color
color 0x336699                     # hex color
fill none                          # transparent fill
```

140 HTML color names supported. Special: `none`/`off` for transparent.

### Arrows

```pikchr
->                                 # arrow at end
<-                                 # arrow at start
<->                                # arrows both ends
```

### Other Attributes

```pikchr
same                               # copy size from previous
same as ObjectLabel                # copy from named object
behind OtherObject                 # render behind another object
chop                               # stop at object boundary
close                              # close polygon path
cw                                 # clockwise (arcs)
ccw                                # counter-clockwise (arcs)
```

## Text Properties

Applied to string literals:

```pikchr
"text" above                       # above center
"text" below                       # below center
"text" ljust                       # left-justified
"text" rjust                       # right-justified
"text" center                      # centered (default)
"text" bold                        # bold font
"text" italic                      # italic font
"text" mono                        # monospace font
"text" big                         # larger (stackable)
"text" small                       # smaller (stackable)
"text" aligned                     # rotate to match line angle
```

## Default Variables

Size defaults (modify to change all subsequent objects):

```pikchr
boxwid = 0.75in                    # default box width
boxht = 0.5in                      # default box height
boxrad = 0                         # default corner radius
circlerad = 0.25in
ellipsewid = 0.75in
ellipseht = 0.5in
arcrad = 0.25in
linewid = 0.5in                    # default line length
lineht = 0.5in
arrowwid = 0.05in                  # arrowhead width
arrowht = 0.08in                   # arrowhead height
dotrad = 0.05in
```

Global settings:

```pikchr
scale = 0.8                        # scale entire diagram
fill = white                       # default fill color
color = black                      # default stroke color
thickness = 1px                    # default line thickness
fontscale = 1.0                    # text size multiplier
charht = 0.14in                    # character height
charwid = 0.08in                   # character width
linerad = 0                        # radius for line corners
```

## Subdiagrams

Group objects into a compound object:

```pikchr
A: [
    box "Header"
    line
    box "Body" ht 1in
]
# A is now a single object with .n, .s, .e, .w, etc.
box "External" with .w at 0.5in right of A.e
```

Access internal objects: `A.1st box`, `A.2nd box`, etc.

## Macros

Define reusable patterns:

```pikchr
define mybox { box $1 fit fill $2 rad 5px }
mybox("Label", lightblue)
mybox("Other", lightyellow)
```

Parameters: `$1` through `$9`. Unused parameters are omitted.
