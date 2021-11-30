# Animatable properties

Lottie supports animating different types of properties. Some of them are
multidimensional, some are unidimensional, and some have a spatial component.
Depending on the type of property, some attributes may vary. When these
properties are not animated, their signature will be different.

## Easing types

Lottie has different easing types depending on the type of property that is
being interpolated.

### One Dimensional Easing

Properties with this easing type have a single easing function for all parts of
the interpolating object. For example, shape vertices, outpoint and inpoints
share a single easing function.

#### Out Point

**Property Name**: *o*

**Property Type**: object

**Required**

The outpoint is the outgoing bezier control point that describes the easing
function.

#### Coord x

**Property Name**: *x*

**Property Type**: Number

**Required**

The x component of the control point

#### Coord y

**Property Name**: *y*

**Property Type**: Number

**Required**

The y component of the control point

#### In Point

**Property Name**: *i*

**Property Type**: object

**Required**

The outpoint is the outgoing bezier control point that describes the easing
function.

#### Coord x

**Property Name**: *x*

**Property Type**: Number

**Required**

The x component of the control point

#### Coord y

**Property Name**: *y*

**Property Type**: Number

**Required**

The y component of the control point

### Multi Dimensional Easing

Properties with this easing type have a different easing function for each of
their dimensions.

#### Out Point

**Property Name**: *o*

**Property Type**: object

**Required**

The outpoint is the outgoing bezier control point that describes the easing
function.

##### Coord x

**Property Name**: *x*

**Property Type**: Array[Number]

**Required**

The list of x components of the control point

##### Coord y

**Property Name**: *y*

**Property Type**: Array[Number]

**Required**

The list of y components of the control point

#### In Point

**Property Name**: *i*

**Property Type**: object

**Required**

The outpoint is the outgoing bezier control point that describes the easing
function.

##### Coord x

**Property Type**: Array[Number]

**Required**

The list of x components of the control point

The x component of the control point

##### Coord y

**Property Type**: Array[Number]

**Required**

The list of y components of the control point

The y component of the control point

## Non animated

When an animatable property is not animated, it will consist of a single prop
called "k", and its value will depend on the type of property.

## Animated

Animation objects are mostly described by five attributes: time, easing in,
easing out, hold and value

### Time

**Property name**: *t*

**Property type**: Number

**Required**

Time describes, in frames, the time at which a new value is specified.

### Easing Out

**Property name**: *o*

**Property type**: One Dimensional Easing | Multi Dimensional Easing

**Required**

Easing out describes the outgoing interpolation values used to create the easing
function between two keyframes.

### Easing In

**Property name**: *i*

**Property type**: One Dimensional Easing | Multi Dimensional Easing

**Required**

Easing in describes the incoming interpolation values used to create the easing
function between two keyframes.

### Hold

**Property name**: *h*

**Property type**: Boolean

**Optional**

If true, the hold property indicates that the specific keyframe should not
interpolate to the next value and instead should stay on its own value until it
reaches the next keyframe.

### Value

**Property name**: *s*

**Property type**: Number | One Dimensional Array | Multi Dimensional Array |
Shape | Gradient | Text Document | Color

**Required**

The value of the property at the specific keyframe that should be rendered at
time t

## One Dimensional Animated Property

One Dimensional Animated Properties include the same properties as animated
properties. Their easing type is One Dimensional Easing. 

Their value type is a Number or an Array.

## Multi Dimensional Animated Property

Multi Dimensional Animated Properties include the same properties as animated
properties. Their easing type is Multi Dimensional Easing.

Their value type is an Array.

## Shape Animated Property

Shape Animated Properties include the same properties as animated properties.
Their easing type is One Dimensional Easing and their value type is Shape.

## Gradient Animated Property

Shape Animated Properties include the same properties as animated properties.
Their easing type is One Dimensional Easing and their value type is Gradient.

## Spatial Animated Property

Spatial properties include the same properties as animated properties but they
add two extra properties that describe how the spatial interpolation should be
applied. These two properties describe the bezier curve control points needed to
draw the path between the spatial coordinates. Values are relative to the
coordinates of each keyframe.

Their easing type is One Dimensional Easing.

### Spatial Out

**Property name**: *to*

**Property type**: Array[Number]

**Required**

The coordinates of the first control point of the bezier curve relative to the
initial value of the interpolated segment

### Spatial In

**Property name**: *ti*

**Property type**: Array[Number]

**Required**

The coordinates of the second control point of the bezier curve relative to the
end value of the interpolated segment

## Text Document Animated Property

Text Document Animated Properties include the same properties as animated
properties. They don’t have an easing value and their Hold property is set to
true.
