# Pikchr Flowchart Patterns

Patterns for creating professional flowcharts with proper styling, routing, and layout.

## Node Types

```pikchr
# Standard flowchart shapes
oval "Start/End" fit                           # terminal
box "Process" fit                              # action
diamond "Decision?" fit                        # branch
cylinder "Database" fit                        # data store
file "Document" fit                            # file/report
box "Subprocess" fit rad 10px                  # rounded = subprocess
```

## Decision Node with Branches

```pikchr
down
D: diamond "Is valid?" fit

# Yes path (down)
arrow from D.s "Yes" ljust
box "Process" fit

# No path (right)  
arrow from D.e right 0.8in "No" above
box "Handle Error" fit
```

## Multi-way Decision

```pikchr
D: diamond "Status?" fit

# Branch down-left
A: box "Active" fit with .n at 0.6in below D.s and 1in left of D.s
arrow from D.sw to A.n "active" aligned

# Branch down
B: box "Pending" fit with .n at 0.6in below D.s  
arrow from D.s to B.n "pending" aligned

# Branch down-right
C: box "Complete" fit with .n at 0.6in below D.s and 1in right of D.s
arrow from D.se to C.n "complete" aligned
```

## Orthogonal Routing (Manhattan Style)

The key is `until even with` for clean right-angle connectors:

```pikchr
A: box "Source"
B: box "Target" at 2in right of A and 1in below A

# Route: down, right, down
arrow from A.s \
    down 0.3in \
    then right until even with B \
    then to B.n
```

## Parallel Branches with Join

```pikchr
Start: box "Begin" fit
down

# Fan out to parallel tasks
T1: box "Task 1" fit at 1in below Start and 1in left of Start  
T2: box "Task 2" fit at 1in below Start
T3: box "Task 3" fit at 1in below Start and 1in right of Start

# Fan-out arrows
arrow from Start.s down 0.25in then left until even with T1 then to T1.n
arrow from Start.s to T2.n
arrow from Start.s down 0.25in then right until even with T3 then to T3.n

# Join point
Join: box "Merge" fit at 1in below T2

# Join arrows
arrow from T1.s down until even with Join then right until even with Join then to Join.w
arrow from T2.s to Join.n
arrow from T3.s down until even with Join then left until even with Join then to Join.e
```

## Swimlanes

```pikchr
$lanew = 2in
$laneh = 3in

# Lane backgrounds
L1: box width $lanew height $laneh fill 0xd8ecd0
L2: box same fill 0xd0ece8 with .nw at L1.ne
L3: box same fill 0xe8d8d0 with .nw at L2.ne

# Lane labels
text "User" bold with .s at 0.15in below L1.n
text "System" bold with .s at 0.15in below L2.n  
text "Database" bold with .s at 0.15in below L3.n

# Content in lanes
A: box "Request" fit with .n at 0.5in below L1.n
B: box "Validate" fit with .n at (L2.n, A.s) + (0, -0.3in)
C: box "Query" fit with .n at (L3.n, B.s) + (0, -0.3in)
D: box "Response" fit with .n at (L2.n, C.s) + (0, -0.3in)

# Arrows across lanes
arrow from A.e to B.w
arrow from B.e to C.w
arrow from C.w to D.e
```

## Legend Box

```pikchr
# Main diagram area
Main: box width 4in height 2.5in color gray thin

# Legend positioned at bottom-right
Legend: box width 1.5in height 0.9in thin \
    with .se at 0.1in above Main.se and 0.1in left of Main.se

text "Legend" bold small with .n at 0.08in below Legend.n

# Legend entries
box width 0.25in height 0.15in fill lightgreen \
    at Legend.c + (-0.4in, 0.05in)
text "Success" small with .w at 0.08in right of previous.e

box width 0.25in height 0.15in fill lightcoral \
    at Legend.c + (-0.4in, -0.15in)  
text "Error" small with .w at 0.08in right of previous.e
```

## Color-Coded Categories

```pikchr
# Define colors as variables
$user = 0x4C78A8      # blue - user actions
$system = 0x54A24B    # green - system actions
$data = 0xE4A844      # yellow - data operations

box "User Request" fit fill $user color white
arrow
box "Validate" fit fill $system color white
arrow
box "Save Record" fit fill $data
```

## Inline Notes / Callouts

```pikchr
D: diamond "Complex?" fit

# Note attached to decision
Note: box "This condition checks\nmultiple factors" \
    fit thin dashed \
    with .nw at 0.2in right of D.ne

line dashed thin from Note.w to D.ne
```

## Terminal Nodes (Start/End)

```pikchr
# Filled circle for start
Start: dot rad 0.15in fill black
arrow from Start.e

# Double circle for end  
End: circle rad 0.12in at 3in right of Start
circle rad 0.08in fill black at End.c
arrow from 2in right of Start to End.w
```

## Conditional Styling

Use variables to create consistent, semantic styling:

```pikchr
# Style definitions
$nodew = 1.2in
$nodeh = 0.4in
$nodecolor = 0x336699
$yescolor = 0x228B22
$nocolor = 0xDC143C

boxwid = $nodew
boxht = $nodeh

box "Input" fit fill $nodecolor color white
arrow
D: diamond "Valid?" fit
arrow "Yes" above color $yescolor
box "Process" fit fill $nodecolor color white
arrow from D.e right 1in "No" above color $nocolor
box "Reject" fit fill $nocolor color white
```

## Complex Flow Example

Demonstrates multiple patterns together:

```pikchr
scale = 0.9
down

# Start
Start: oval "Start" fit fill lightgray

arrow
Input: box "Receive Input" fit

arrow  
Validate: diamond "Valid?" fit

# Valid path
arrow "Yes" ljust
Process: box "Process Data" fit

arrow
D2: diamond "Complete?" fit

arrow "Yes" ljust
End: oval "End" fit fill lightgray

# Invalid path (right from Validate)
arrow from Validate.e right 1.2in "No" above
Error: box "Log Error" fit fill lightyellow

arrow down 0.5in then left until even with Input then to Input.e

# Incomplete path (right from D2)
arrow from D2.e right 1.2in "No" above
Retry: box "Retry" fit

arrow down 0.5in then left until even with Process then to Process.e
```
