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

??? Details
    **Property name:** *ty*

    **Property type**: [Layer Types](../../properties/layer-types)

    **Property value**: 4

## Shapes Property

??? Details
    **Property name:** *shapes*

    **Property type**: Array {
      [Group Element](#group-element) 
      | [Transform Element](#transform-element)
      | [Rectangle Element](#rectangle-element)
      | [Ellipse Element](#ellipse-element)
      | [Polystar Element](#polystar-element)
      | [Shape Element](#shape-element)
      | [Fill Property](#fill-property)
      | [Gradient Fill Property](#gradient-fill-property)
      | [Stroke Property](#stroke-property)
      | [Merge Paths Property](#merge-paths-property)
      | [Offset Paths Property](#offset-paths-property)
      | [Repeater Property](#repeater-property)
      | [Trim Paths Property](#trim-paths-property)
    }

Shapes is a collection of paths and path modifiers like fills, colors, groups,
trim paths. Each property has a type defined by the "ty" prop.

## Group Element

### Type Property
??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *gr*

A group contains a list of shape properties that are rendered as part of that
group. Any modifiers defined within the group will only apply to the stack
preceding the modifier but only scoped inside the group.

### Items Property

??? Details
    **Property name:** *it*

    **Property type**: Array {
      [Group Element](#group-element) 
      | [Transform Element](#transform-element)
      | [Rectangle Element](#rectangle-element)
      | [Ellipse Element](#ellipse-element)
      | [Polystar Element](#polystar-element)
      | [Shape Element](#shape-element)
      | [Fill Property](#fill-property)
      | [Gradient Fill Property](#gradient-fill-property)
      | [Stroke Property](#stroke-property)
      | [Merge Paths Property](#merge-paths-property)
      | [Offset Paths Property](#offset-paths-property)
      | [Repeater Property](#repeater-property)
      | [Trim Paths Property](#trim-paths-property)
    }

    **Required**

The list of items defined within the group

## Transform Element

??? Details
    **Property name**: *tr*

    **Property type:** [Transform](../../layers/common/#transform)

    **Required**

A transform property that is applied to the stack of elements preceding it

## Rectangle Element

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *rc*

    **Required**

A rectangle is a parametric shape defined by its size, position and roundness.

### Size Property

??? Details
    **Property name:** *s*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Size is a two dimensional array that represents the width and height of the
rectangle.

??? Details
    ### Position Property

    **Property name:** *p*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

### Roundness Property

??? Details
    **Property name:** *r*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Roundness is a one-dimensional value that represents the roundness of the
rectangle corners.

## Ellipse Element

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *el*

An ellipse is a parametric shape defined by its size and position

### Size Property

??? Details
    **Property name:** *s*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Size is a two-dimensional array that represents the width and height of the
rectangle.

### Position Property

??? Details
    **Property name:** *p*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group.

## Polystar Element

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *sr*

This element has two different subtypes that define a Star or a Polygon.

Its points, position, rotation, outer radius and outer roundness define a
polygon.

A star is defined by all the same properties as a polygon, and it includes an
inner roundness and inner radius.

### Subtype Property

??? Details

    **Property name:** *sy*

    **Property type**: [Polystar Types](../../properties/polystar-types)

    **Required**

### Points Property

??? Details
    **Property name:** *pt*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Points is a one-dimensional number that defines the number of outer corners the
parametric shape has.

For a polygon, it also equals the total number of corners.

For a star, the number of corners is double, since every segment is subdivided
by an outer circle and an inner circle.

### Position Property

??? Details
    **Property name:** *p*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

### Rotation Property

??? Details
    **Property name:** *r*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Position is a one-dimensional number that represents the rotation angle in
degrees of the shape.

### Outer Radius Property

??? Details
    **Property name:** *or*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Outer radius is a one-dimensional number that represents the radius of a circle
where the outer points of the shape should be inscribed.

### Outer Roundness Property

??? Details
    **Property name:** *os*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Outer roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the shape should be
drawn. Those control points should be tangent to the circle defined by the outer
radius property.

### Inner Radius Property

??? Details
    **Property name:** *ir*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

Inner radius is a one-dimensional number that represents the radius of a circle
where the inner points of the star should be inscribed.

### Inner Roundness Property

??? Details
    **Property name:** *is*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

Inner roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the inner points of the
star should be drawn. Those control points should be tangent to the circle
defined by the inner radius property.

## Shape Element

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *sh*

This element has a single property defining the path of the shape

### Path Property

??? Details
  **Property name:** *ks*

  **Property type**: [Shape](../properties/prop-types/#shape) | [Shape Animated Property](../../properties/animatable-properties/#shape-animated-property)

  **Required**

The shape described by the collection of bezier curves.

## Fill Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *fl*

    **Required**

This property defines the fill color that should be applied to the stack of
elements preceding it. It has two inner properties, color and opacity

### Color Property

??? Details
    **Property name:** *c*

    **Property type**: [Color](../../properties/prop-types/#color) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The color of the fill

### Opacity Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The opacity of the fill

## Gradient Fill Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *gf*

    **Required**

This property defines the gradient fill color that should be applied to the
stack of elements preceding it. It has seven properties: Type, Start Point, End
Point, Highlight Angle, Highlight Length, Colors, Opacity.

### Gradient Type Property

??? Details
    **Property name:** *t*

    **Property type**: [Gradient Types](../../properties/gradient-types)

    **Required**

This property only accepts two values and describes the type of gradient that
should be applied.

### Start Point Property

??? Details
    **Property name:** *s*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

A two-dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

### End Point Property

??? Details
    **Property name:** *e*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

### Highlight Length Property

??? Details
    **Property name:** *h*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

A value that offsets the starting point of the gradient.

### Highlight Angle Property

??? Details
    **Property name:** *a*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

A value that defines the angle by which the starting point of the gradient will
be offset.

### Color Property

??? Details
    **Property name:** *g*

    **Property type**: [Gradient](../../properties/prop-types/#gradient) | [Gradient Animated Property](../../properties/animatable-properties/#gradient-animated-property)

    **Required**

The set of colors that define the gradient

### Opacity Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The opacity of the gradient fill

## Stroke Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *st*

This property defines the stroke color and width that should be applied to the
stack of elements preceding it. It has six inner properties, color, opacity,
stroke width, line cap, line join, miter limit.

### Color Property

??? Details
    **Property name:** *c*

    **Property type**: [Color](../../properties/prop-types/#color) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The color of the fill

### Opacity Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The opacity of the fill

### Stroke Width Property

??? Details
    **Property name:** *w*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The width of the stroke applied to the shapes. It does not accept negative
numbers.

### Line Cap Property

??? Details
    **Property name:** *lc*

    **Property type**: [Line Cap types](../../properties/line-cap-types)

    **Required**

The line cap of the stroke

### Line Join Property

??? Details
    **Property name:** *lj*

    **Property type**: [Line Join types](../../properties/line-join-types)

    **Required**

The line join of the stroke

### Miter Limit Property

??? Details
    **Property name:** *ml*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

The miter limit is a one-dimensional value that applies only if the line join
value of the stroke is Miter Join.

## Gradient Stroke Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *gs*

    **Required**

Defines the gradient stroke color and width that should be applied to the stack
of elements preceding it. It has eleven inner properties, color, opacity, stroke
width, line cap, line join, miter limit.

### Color Property

??? Details
    **Property name:** *g*

    **Property type**: [Gradient](../../properties/prop-types/#gradient) | [Gradient Animated Property](../../properties/animatable-properties/#gradient-animated-property)

    **Required**

Describes the set of colors that define the gradient

### Opacity Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The opacity of the fill

### Stroke Width Property

??? Details
    **Property name:** *w*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The width of the stroke applied to the shapes

### Line Cap Property

??? Details
    **Property name:** *lc*

    **Property type**: [Line Cap types](../../properties/line-cap-types)

    **Required**

The line cap of the stroke

### Line Join Property

??? Details
    **Property name:** *lj*

    **Property type**: [Line Join types](../../properties/line-join-types)

    **Required**

The line join of the stroke

### Miter Limit Property

??? Details
    **Property name:** *ml*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The miter limit is a value that applies only if the line join value of the
stroke is Miter Join.

### Gradient Type Property

??? Details
    **Property name:** *t*

    **Property type**: [Gradient Types](../../properties/gradient-types)

    **Required**

This property only accepts two values and describes the type of gradient that
should be applied.

Its values are

* 1 for Linear

* 2 for Radial

### Start Point Property

??? Details
    **Property name:** *s*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

A two dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

### End Point Property

??? Details
    **Property name:** *e*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

### Highlight Length Property

??? Details
    **Property name:** *h*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

### Highlight Angle Property

??? Details
    **Property name:** *a*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

## Merge Paths Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *mm*

    **Required**

A merge path acts as a shape boolean operation applied to the set of shapes
preceding it. It contains a single property, the merge mode.

### Merge Mode Property

??? Details
    **Property name:** *mm*

    **Property type**: [Merge mode types](../../properties/merge-mode-types)

    **Required**

The merge mode defines the type of operation that should be applied to shapes.

## Offset Paths Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *op*

    **Required**

The offset path modifier should grow or shrink the set of shapes preceding it.
It is defined by three properties: amount, line join and miter limit.

### Amount Property

??? Details
    **Property name:** *a*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

A one-dimensional value that defines how much the shape should grow or shrink

### Line Join Property

??? Details
    **Property name:** *lj*

    **Property type**: [Line Join types](../../properties/line-join-types)

The line join of the stroke

### Miter Limit Property

??? Details
    **Property name:** *ml*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

A one-dimensional value that applies only if the line join value of the stroke
is Miter Join

## Repeater Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *rp*

The repeater modifier creates copies of the set of shapes preceding it. It is
defined by three properties: copies, offset, transform.

### Copies Property

??? Details
    **Property name:** *c*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A one-dimensional value that defines how many copies (including the original
shape) of the shapes should be drawn 

### Offset Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A one-dimensional value that defines how many copies should be skipped before
starting the count of shapes to be drawn.

This value should be accounted for in order to calculate the following
*transform* property.

### Transform Property

??? Details
    **Property name:** *tr*

    **Property type**: [Transform](../../layers/common/#transform)

    **Required**

For each copy of a repeater, this transform operation is applied before the
drawing operation is applied. This allows to generate copies of the original
drawing with a combination of regular transform operations between them.

When the offset property is different from 0, these transform operations should
accumulate by the amount that the offset value defines before being applied to
the first copy.

## Round Corners Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *rd*

    **Required**

This modifier affects any vertex of a shape whose bezier control points are 0.
It has a single property that defines the radius of the effect.

### Roundness Property

??? Details
    **Property name:** *r*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

A one-dimensional value that defines the length of the control points of the
bezier curve that should be applied to the vertex. The direction of the control
point should be tangent to the vertex itself.

## Trim Paths Property

### Type Property

??? Details
    **Property name:** *ty*
    
    **Property type**: [Shape types](../../properties/shape-types)
    
    **value**: *tm*

    **Required**

This modifier changes the shape of the set of elements preceding it. Although in
general, trim paths are expected to only affect the stroke of a shape, this
modifier should affect both how strokes and fills are applied.

The way the new shape is calculated should measure the full perimeter of the
shape individually (or the full set of shapes if Trim Multiple Shapes property
is set to Individually) and regenerate the new set of paths as a percentage of
the original set.

It is defined by four properties: Start, End, Offset and Trim Multiple Shapes.

### Start Property

??? Details
    **Property name:** *s*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A one-dimensional value that defines the initial point of the shape that should
be drawn relative to the initial point of the original shape. It is expressed as
a percentage of the initial value.

### End Property

??? Details
    **Property name:** *e*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A one-dimensional value that defines the end point of the shape that should be
drawn relative to the end point of the original shape. It is expressed as a
percentage of the end value.

### Offset Property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A one-dimensional value that defines an offset by which the start and end points
should be calculated. A whole cycle is expressed as a 360 offset.

### Trim Multiple Shapes Property

??? Details
    **Property name:** *m*

    **Property type**: [Trim Paths modes](../../properties/trim-paths-modes)

    **Required**

Simultaneous means that all shapes should be trimmed applying the calculations
separately. Individual means that a single trim effect should be applied to all
of them by calculating the full length of all shapes together.