# Complex Flowchart Example

This example demonstrates a notification routing flowchart similar to Slack's notification flow diagram.

## Full Example

```pikchr
# Notification Flow Diagram
# Demonstrates: color coding, legend, decisions, orthogonal routing, notes

scale = 0.85

# Color definitions by category
$pref = 0x4C78A8        # blue - preference checks
$prefval = 0xE8C547     # yellow - preference values  
$state = 0x9B59B6       # purple - user state/actions
$parse = 0xFFFFFF       # white - parsing/processing
$yes_color = 0x27AE60   # green
$no_color = 0xE74C3C    # red

# Default styling
boxht = 0.35in
boxwid = 1.4in
linewid = 0.3in

# === ROW 1: Entry ===
down
Start: oval "Message\nReceived" fit fill $parse

arrow down 0.4in
Parse: box "Parse Message" fit fill $parse

arrow
D1: diamond "Has\n@mention?" fit fill $pref

# === MENTION BRANCH (right) ===
arrow from D1.e right 1in "No" above
NoMention: box "Check Channel\nPrefs" fit fill $pref

arrow
D2: diamond "Channel\nMuted?" fit fill $pref

arrow from D2.e right 1in "Yes" above
Muted: box "Skip\nNotification" fit fill $prefval
arrow
MutedEnd: circle rad 0.1in fill $no_color

# D2 No path (down)
arrow from D2.s down 0.5in "No" ljust
CheckDND: box "Check DND\nStatus" fit fill $state

arrow
D3: diamond "DND\nActive?" fit fill $state

arrow from D3.e right 1in "Yes" above
DNDActive: box "Queue for\nLater" fit fill $prefval
arrow right 0.4in then down until even with MutedEnd then to MutedEnd.n chop

# D3 No path (down)
arrow from D3.s down 0.5in "No" ljust
DeviceCheck: box "Check Device\nPreference" fit fill $pref

arrow
D4: diamond "Mobile\nEnabled?" fit fill $pref

# Mobile enabled - send push
arrow from D4.s down 0.5in "Yes" ljust
SendPush: box "Send Push\nNotification" fit fill $state
arrow
PushEnd: circle rad 0.1in fill $yes_color

# Mobile disabled - desktop only
arrow from D4.e right 1in "No" above
DesktopOnly: box "Desktop\nOnly" fit fill $prefval
arrow right 0.4in then down until even with PushEnd then to PushEnd.e chop

# === MENTION BRANCH (left from D1) ===
arrow from D1.w left 1in "Yes" above
HasMention: box "Check User\nPrefs" fit fill $pref

arrow
D5: diamond "Notify on\nMention?" fit fill $pref

# Not notify on mention
arrow from D5.w left 0.8in "No" above
NoNotify: box "Log Only" fit fill $prefval
arrow down 0.4in then right until even with D1 \
    then down until even with MutedEnd \
    then right until even with MutedEnd \
    then to MutedEnd.w chop

# Notify on mention - check urgency
arrow from D5.s down 0.5in "Yes" ljust  
CheckUrgent: box "Check\nKeywords" fit fill $parse

arrow
D6: diamond "Contains\nUrgent?" fit fill $parse

# Urgent - immediate
arrow from D6.w left 0.8in "Yes" above
Urgent: box "Priority\nAlert" fit fill $state
arrow down 0.4in then right until even with D5 \
    then down until even with PushEnd.s \
    then right until even with PushEnd \
    then to PushEnd.w chop

# Not urgent - normal flow
arrow from D6.s down 0.5in "No" ljust
NormalFlow: box "Standard\nNotification" fit fill $state
arrow down until even with PushEnd then right until even with PushEnd then to PushEnd.n chop

# === LEGEND ===
Legend: box width 1.6in height 1.1in thin \
    with .nw at Start.ne + (0.3in, 0.2in)

text "Legend" bold small with .n at 0.12in below Legend.n

# Legend entries
box width 0.35in height 0.18in fill $pref \
    at Legend.c + (-0.45in, 0.15in)
text "Preference" small with .w at 0.08in right of previous.e

box same fill $prefval \
    at Legend.c + (-0.45in, -0.05in)
text "Pref Value" small with .w at 0.08in right of previous.e

box same fill $state \
    at Legend.c + (-0.45in, -0.25in)
text "State/Action" small with .w at 0.08in right of previous.e

box same fill $parse \
    at Legend.c + (-0.45in, -0.45in)
text "Parsing" small with .w at 0.08in right of previous.e

# === INLINE NOTE ===
Note: box "Note: DND respects\nscheduled quiet hours\nand focus modes" \
    fit thin dashed \
    with .nw at 0.2in right of D3.e and 0.3in below D3.e

line thin dashed from Note.nw to D3.se
```

## Key Techniques Used

### 1. Color Variables for Categories
```pikchr
$pref = 0x4C78A8        # consistent category colors
box "Example" fill $pref
```

### 2. Orthogonal Routing with "until even with"
```pikchr
arrow from D4.e right 1in then down until even with PushEnd then to PushEnd.e chop
```

### 3. Decision Branching Pattern
```pikchr
D: diamond "Condition?" fit
arrow from D.s "Yes" ljust    # down = yes
arrow from D.e "No" above     # right = no
```

### 4. Terminal Markers
```pikchr
circle rad 0.1in fill $yes_color   # success endpoint
circle rad 0.1in fill $no_color    # failure endpoint
```

### 5. Legend Box
```pikchr
Legend: box width 1.6in height 1.1in thin with .nw at ...
text "Legend" bold small with .n at 0.12in below Legend.n
# Color swatches with labels
```

### 6. Inline Notes
```pikchr
Note: box "Note text..." fit thin dashed with .nw at ...
line thin dashed from Note.nw to Target.se
```
