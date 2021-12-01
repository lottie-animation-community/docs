# Shape Layer Type

Shape layers define a set of shapes and shape modifiers grouped together. It has
a single specific property (shapes) and others shared with other layer types
(transform, masks, effects, layer styles).

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 4
[Shapes](#shapes-property) | shapes | Array[[Shape Properties](#shapes-property)] | ✅
{!specs/layers/common-properties.md!}

## Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 4

## Shapes Property

**Property name:** *shapes*

**Property type**: Array[Shape Properties]

Shapes is a collection of paths and path modifiers like fills, colors, groups,
trim paths. Each property has a type defined by the "ty" prop.

## Group Element

**type**: *gr*

A group contains a list of shape properties that are rendered as part of that
group. Any modifiers defined within the group will only apply to the stack
preceding the modifier but only scoped inside the group.

### Items Property

**Property name:** *it*

**Property type**: Array[Shape Properties]

**Required**

The list of items defined within the group

## Transform Element

**Property name**: *tr*

**Property type:** Transform

**Required**

A transform property that is applied to the stack of elements preceding it

## Rectangle Element

**type**: *rc*

A rectangle is a parametric shape defined by its size, position and roundness.

### Size Property

**Property name:** *s*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

Size is a two dimensional array that represents the width and height of the
rectangle.

### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

### Roundness Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Roundness is a one-dimensional value that represents the roundness of the
rectangle corners.

## Ellipse Element

**type**: *el*

An ellipse is a parametric shape defined by its size and position

### Size Property

**Property name:** *s*

**Property type**:Non Animated Array | Multi Dimensional Animated Property

Size is a two-dimensional array that represents the width and height of the
rectangle.

### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group.

## Polystar Element

**type**: *sr*

This element has two different subtypes that define a Star or a Polygon.

Its points, position, rotation, outer radius and outer roundness define a
polygon.

A star is defined by all the same properties as a polygon, and it includes an
inner roundness and inner radius.

### Subtype Property

**Property name:** *sy*

**Property type**: Enum[Number]

* 1 for Star

* 2 for Polygon

**Required**

### Points Property

**Property name:** *pt*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

Points is a one-dimensional number that defines the number of outer corners the
parametric shape has.

For a polygon, it also equals the total number of corners.

For a star, the number of corners is double, since every segment is subdivided
by an outer circle and an inner circle.

### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

### Rotation Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Position is a one-dimensional number that represents the rotation angle in
degrees of the shape.

### Outer Radius Property

**Property name:** *or*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Outer radius is a one-dimensional number that represents the radius of a circle
where the outer points of the shape should be inscribed.

### Outer Roundness Property

**Property name:** *os*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Outer roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the shape should be
drawn. Those control points should be tangent to the circle defined by the outer
radius property.

### Inner Radius Property

**Property name:** *ir*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

Inner radius is a one-dimensional number that represents the radius of a circle
where the inner points of the star should be inscribed.

### Inner Roundness Property

**Property name:** *is*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

Inner roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the inner points of the
star should be drawn. Those control points should be tangent to the circle
defined by the inner radius property.

## Shape Element

**type**: *sh*

This element has a single property defining the path of the shape

### Path Property

**Property name:** *ks*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

The shape described by the collection of bezier curves.

## Fill Property

**type**: *fl*

**Required**

This property defines the fill color that should be applied to the stack of
elements preceding it. It has two inner properties, color and opacity

### Color Property

**Property name:** *c*

**Property type**: Color | Multi Dimensional Animated Property

**Required**

The color of the fill

### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

## Gradient Fill Property

**type**: *gf*

This property defines the gradient fill color that should be applied to the
stack of elements preceding it. It has seven properties: Type, Start Point, End
Point, Highlight Angle, Highlight Length, Colors, Opacity.

### Type Property

**Property name:** *t*

**Property type**: Enum[Number]

**Required**

This property only accepts two values and describes the type of gradient that
should be applied.

* 1 for Linear

* 2 for Radial

### Start Point Property

**Property name:** *s*

**Property type**: Non Animated Array | Spatial Animated Property

**Required**

A two-dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

### End Point Property

**Property name:** *e*

**Property type**: Non Animated Array | Spatial Animated Property

**Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

### Highlight Length Property

**Property name:** *h*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

A value that offsets the starting point of the gradient.

### Highlight Angle Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

A value that defines the angle by which the starting point of the gradient will
be offset.

### Color Property

**Property name:** *g*

**Property type**: Gradient | Gradient Animated Property

**Required**

The set of colors that define the gradient

### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the gradient fill

## Stroke Property

**type**: *st*

This property defines the stroke color and width that should be applied to the
stack of elements preceding it. It has six inner properties, color, opacity,
stroke width, line cap, line join, miter limit.

### Color Property

**Property name:** *c*

**Property type**: Color | Multi Dimensional Animated Property

**Required**

The color of the fill

### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

### Stroke Width Property

**Property name:** *w*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The width of the stroke applied to the shapes. It does not accept negative
numbers.

### Line Cap Property

**Property name:** *lc*

**Property type**: Enum[Number]

**Required**

The line cap of the stroke

Its values are 

* 1 for Butt Cap

* 2 for Round Cap

* 3 for Projecting Cap

### Line Join Property

**Property name:** *lj*

**Property type**: Enum[Number]

**Required**

The line join of the stroke

Its values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

### Miter Limit Property

**Property name:** *ml*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

The miter limit is a one-dimensional value that applies only if the line join
value of the stroke is Miter Join.

## Gradient Stroke Property

**type**: *st*

Defines the gradient stroke color and width that should be applied to the stack
of elements preceding it. It has eleven inner properties, color, opacity, stroke
width, line cap, line join, miter limit.

### Color Property

**Property name:** *g*

**Property type**: Gradient | Gradient Animated Property 

**Required**

Describes the set of colors that define the gradient

### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

### Stroke Width Property

**Property name:** *w*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The width of the stroke applied to the shapes

### Line Cap Property

**Property name:** *lc*

**Property type**: Number

**Required**

The line cap of the stroke

Its values are 

* 1 for Butt Cap

* 2 for Round Cap

* 3 for Projecting Cap

### Line Join Property

**Property name:** *lj*

**Property type**: Number

**Required**

The line join of the stroke

Its values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

### Miter Limit Property

**Property name:** *ml*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The miter limit is a value that applies only if the line join value of the
stroke is Miter Join.

### Type Property

**Property name:** *t*

**Property type**: Enum[Number]

**Required**

This property only accepts two values and describes the type of gradient that
should be applied.

Its values are

* 1 for Linear

* 2 for Radial

### Start Point Property

**Property name:** *s*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

A two dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

### End Point Property

**Property name:** *e*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

### Highlight Length Property

**Property name:** *h*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

### Highlight Angle Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

## Merge Paths Property

**type**: *mm*

A merge path acts as a shape boolean operation applied to the set of shapes
preceding it. It contains a single property, the merge mode.

### Merge Mode Property

**Property name:** *mm*

**Property type**: Enum[Number]

**Required**

The merge mode defines the type of operation that should be applied to shapes.

Its values are:

* 1 for Merge

* 2 for Add

* 3 for Subtract

* 4 for Intersect

* 5 for Exclude Intersections

## Offset Paths Property

**type**: *op*

The offset path modifier should grow or shrink the set of shapes preceding it.
It is defined by three properties: amount, line join and miter limit.

### Amount Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

A one-dimensional value that defines how much the shape should grow or shrink

### Line Join Property

**Property name:** *lj*

**Property type**: Enum[Number]

The line join of the stroke

Its Values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

### Miter Limit Property

**Property name:** *ml*

**Property type**: object

A one-dimensional value that applies only if the line join value of the stroke
is Miter Join

## Repeater Property

**type**: *rp*

The repeater modifier creates copies of the set of shapes preceding it. It is
defined by three properties: copies, offset, transform.

### Copies Property

**Property name:** *c*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines how many copies (including the original
shape) of the shapes should be drawn 

### Offset Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines how many copies should be skipped before
starting the count of shapes to be drawn.

This value should be accounted for in order to calculate the following
*transform* property.

### Transform Property

**Property name:** *tr*

**Property type**: Transform

**Required**

For each copy of a repeater, this transform operation is applied before the
drawing operation is applied. This allows to generate copies of the original
drawing with a combination of regular transform operations between them.

When the offset property is different from 0, these transform operations should
accumulate by the amount that the offset value defines before being applied to
the first copy.

## Round Corners Property

**type**: *rd*

This modifier affects any vertex of a shape whose bezier control points are 0.
It has a single property that defines the radius of the effect.

### Copies Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

A one-dimensional value that defines the length of the control points of the
bezier curve that should be applied to the vertex. The direction of the control
point should be tangent to the vertex itself.

## Trim Paths Property

**type**: *tm*

This modifier changes the shape of the set of elements preceding it. Although in
general, trim paths are expected to only affect the stroke of a shape, this
modifier should affect both how strokes and fills are applied.

The way the new shape is calculated should measure the full perimeter of the
shape individually (or the full set of shapes if Trim Multiple Shapes property
is set to Individually) and regenerate the new set of paths as a percentage of
the original set.

It is defined by four properties: Start, End, Offset and Trim Multiple Shapes.

### Start Property

**Property name:** *s*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines the initial point of the shape that should
be drawn relative to the initial point of the original shape. It is expressed as
a percentage of the initial value.

### End Property

**Property name:** *e*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines the end point of the shape that should be
drawn relative to the end point of the original shape. It is expressed as a
percentage of the end value.

### Offset Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines an offset by which the start and end points
should be calculated. A whole cycle is expressed as a 360 offset.

### Trim Multiple Shapes Property

**Property name:** *m*

**Property type**: Enum[Number]

**Required**

This property accepts only two values:

* 1 for Simultaneous

* 2 for Individual

Simultaneous means that all shapes should be trimmed applying the calculations
separately. Individual means that a single trim effect should be applied to all
of them by calculating the full length of all shapes together.