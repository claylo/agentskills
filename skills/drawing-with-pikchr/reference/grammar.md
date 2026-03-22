# Pikchr Grammar Reference

Formal grammar specification for Pikchr.

## Token Classes

- **NEWLINE**: Un-escaped newline (U+000A). Backslash-escaped newlines are whitespace.
- **LABEL**: Object label. Starts with uppercase letter, followed by letters/digits/underscores.
- **VARIABLE**: Variable name. Starts with lowercase letter or `$`/`@`, followed by letters/digits/underscores.
- **NUMBER**: Numeric literal. Decimal, float, or hex (`0x`). Optional unit suffix: `in`, `cm`, `px`, `pt`, `pc`, `mm`.
- **ORDINAL**: Integer with suffix: `1st`, `2nd`, `3rd`, `4th`... Also: `first` = `1st`.
- **STRING**: Double-quoted text. Escape `\"` and `\\`. Newlines allowed.
- **COLORNAME**: One of 140 HTML color names. Plus `None`/`Off` = -1 (transparent).
- **CODEBLOCK**: Content within nested `{...}` (macro bodies).

## Statement List

```
statement-list:
    statement?
    statement-list NEWLINE statement?
    statement-list ; statement?
```

## Statements

```
statement:
    object-definition
    LABEL : object-definition
    LABEL : place
    direction
    VARIABLE assignment-op expr
    define VARIABLE CODEBLOCK
    print print-argument (, print-argument)*
    assert ( expr == expr )
    assert ( position == position )

direction:
    right | down | left | up

assignment-op:
    = | += | -= | *= | /=
```

## Object Definition

```
object-definition:
    object-class attribute*
    STRING text-attribute* attribute*
    [ statement-list ] attribute*

object-class:
    arc | arrow | box | circle | cylinder | diamond | dot |
    ellipse | file | line | move | oval | spline | text
```

## Attributes

```
attribute:
    path-attribute
    location-attribute
    STRING text-attribute*
    same
    same as object
    numeric-property new-property-value
    dashed expr?
    dotted expr?
    color color-expr
    fill color-expr
    behind object
    cw | ccw
    <- | -> | <->
    invis | invisible
    thick | thin | solid
    chop
    fit

numeric-property:
    diameter | ht | height | rad | radius | thickness | width | wid

new-property-value:
    expr
    expr %

text-attribute:
    above | aligned | below | big | bold | mono | monospace |
    center | italic | ljust | rjust | small
```

## Path Attributes

```
path-attribute:
    from position
    then? to position
    then? go? direction line-length?
    then? go? direction until? even with position
    (then | go) line-length? heading compass-angle
    (then | go) line-length? compass-direction
    close

line-length:
    expr
    expr %

compass-direction:
    n | north | ne | e | east | se | s | south | sw | w | west | nw
```

## Location Attributes

```
location-attribute:
    at position
    with edgename at position
    with dot-edgename at position
```

## Positions and Places

```
position:
    expr , expr
    place
    place + expr , expr
    place - expr , expr
    place + ( expr , expr )
    place - ( expr , expr )
    ( position , position )
    ( position )
    fraction of the way between position and position
    fraction way between position and position
    fraction between position and position
    fraction < position , position >
    distance which-way-from position

place:
    object
    object dot-edgename
    edgename of object
    ORDINAL vertex of object

which-way-from:
    above | below | right of | left of |
    n of | north of | ne of | e of | east of | se of |
    s of | south of | sw of | w of | west of | nw of |
    heading compass-angle from
```

## Object References

```
object:
    LABEL
    object . LABEL
    nth-object of object
    nth-object in object

nth-object:
    ORDINAL object-class
    ORDINAL last object-class
    ORDINAL previous object-class
    last object-class
    previous object-class
    last | previous
    ORDINAL []
    ORDINAL last []
    ORDINAL previous []
    last [] | previous []
```

## Edge Names

```
dot-edgename:
    .n | .north | .t | .top | .ne | .e | .east | .right | .se |
    .s | .south | .bot | .bottom | .sw | .w | .west | .left | .nw |
    .c | .center | .start | .end

edgename:
    n | north | ne | e | east | se | s | south | sw | w | west | nw |
    t | top | bot | bottom | left | right | c | center | start | end
```

## Expressions

```
expr:
    NUMBER
    VARIABLE
    COLORNAME
    place .x
    place .y
    object dot-property
    ( expr )
    expr + expr
    expr - expr
    expr * expr
    expr / expr
    - expr
    + expr
    abs ( expr )
    cos ( expr )
    dist ( position , position )
    int ( expr )
    max ( expr , expr )
    min ( expr , expr )
    sin ( expr )
    sqrt ( expr )

dot-property:
    .color | .dashed | .diameter | .dotted | .fill |
    .ht | .height | .rad | .radius | .thickness | .wid | .width
```

## Comments

```
# comment to end of line
// comment to end of line
/* block comment */
```

## Line Continuation

Backslash before newline continues the statement:

```pikchr
box "This is a very long label" \
    width 2in \
    height 0.5in
```
