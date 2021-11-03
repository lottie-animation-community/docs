# Lottie Animation Format

### Version 1.0.0

### Status Draft

# Contents

[Abstract](#abstract)

[Rendering Model](#rendering-model)

[Animation](#animation)

[Property Types](#property-types)

[Animatable properties](#animatable-properties)

[Assets](#assets)

[Text Data](#text-data)

[Layers](#layers)

# Abstract

The Lottie Animation Format is a vector format focused on animation,
interaction, real-time updates and multi-platform support. This document
describes the specifications of the format and its extensibility. It will also
describe how to specify supported layer types and properties.

Given its objective of being a global animation vector format, the Lottie
Animation Format does not focus on performance improvements for a single
platform. Instead it tries to be as simple and extendable as possible.

The format is influenced by the Adobe After Effects Render Model, from which it
draws the layer, keyframes, and properties structure.

This specification defines a model for drawing and animating a set of layers on
a canvas.

# Rendering Model

TBD

# Animation

## General Considerations

### Floating-point coordinates

All values (both time and spatial) are defined as floating point numbers unless
otherwise specified. Values do not have units to accommodate for different
programming language capabilities, format preferences, and varying pixel density
devices.

### Required properties

It is recommended that any required property that is not defined in the format
should throw an exception and the animation should refuse to play. Although in
some cases a default value could fill the missing property, or an element could
be ejected from the animation, to preserve consistency and fidelity to the
original animation it is better to reject the animation altogether.

## Width and Height

**Property names:** *w / h*

**Property type:** Number

**Required**

An animation has a defined width and height. These two values will define the
visible drawing area. The (0,0) coordinate is positioned at the top, left corner
of the resulting rectangle.

All coordinates both positive and negative are valid. The width and height will
work as a visible frame of all the drawn scene.

## Duration

An animation must have a duration set in the number of frames. Its value is
expressed as a pair of properties: In point and Out point.

By definition the Out point should be larger or equal to the In point.

If this condition is not met, the player should refuse to play and throw an
exception.

Since these properties indicate a segment of an implicit timeline that goes from
*-Infinity* to *Infinity*, any keyframe that is out of bounds won’t be rendered
and only the parts that are within the segment will. Segments can nonetheless
have keyframes defined outside the time segment; but only the overlapping part
of the segment with the animation time boundaries will be rendered.

Exposing an API to adjust the segment at runtime could be helpful, for example
to animate various states responding to an interaction, or simply to allow for
multiple animations to be in a single file.

### In point

**Property name:** *ip*

**Property type:** Number

**Required**

The In point can be any number, positive or negative, and indicates the initial
frame of the animation that should be rendered.

### Out point

**Property name:** *op*

**Property type:** Number

The Out point can be any number, positive or negative, and indicates the final
frame (not included) of the animation that should be rendered.

## Frame Rate

**Property name:** *fr*

**Property type:** Number

**Required**

An animation has a defined framerate. It represents the number of frames per
second that should be used to calculate time interpolations. Its value can range
from 0 (not included) to any positive number.

The larger the frame rate, the more time-based resolution the animation will
have. This allows for more fine grained interpolations, specially useful for
heavy based discrete effects.

On the other hand, a large frame rate will need an equally large refresh rate,
which will be covered in the next section.

## Refresh Rate

The refresh rate of an animation is not explicitly defined in the animation
definition. It can depend on platform capabilities, engine preferences, and
could change during the course of an animation. 

It describes the number of times per second the engine should compute new values
and redraw the canvas. The larger its value, the smoother the animation will
look, but it will also require more computational power to calculate values and
redraw the result.

In general, devices have a refresh rate of 60Hz or 120Hz which means that
animations have a 16ms or 8ms budget, respectively, to redraw. A 60Hz default
refresh rate should be enough to render a smooth looking animation.

It is encouraged to develop an API to modify this value. This API can be used
for different scenarios:

* When performance issues can be detected and signaled, modifying the refresh
  rate should reduce per frame calculations and hence free computing cycles for
  other requirements.

* When the animation is not the most relevant element in the view, a lower
  refresh rate should be enough and it can be modified when the animation is
  front and center.

* When a user has reduced motion settings enabled.

* When a specific refresh rate reflects a particular design decision to convey a
  message, effect or experience.

Note: If the refresh rate differs from the defined original frame rate, it can
have unexpected values when interpolating between keyframes. Particularly when
two keyframes are positioned on consecutive frames, with different values, and
its easing expects an interpolation between them.

Relying on how the animation looks in the authoring tool is not enough, since it
can have a fixed refresh rate that doesn’t match the default one used inthe
runtime environment.

This issue can be solved in two ways. The first is to make sure that when
designing the animation, frames that should not interpolate are correctly set to
behave that way by using the hold type interpolation.

The second is to match refresh rate and frame rate. This should guarantee parity
between authoring tools and runtimes, but it can be a limitation to take
advantage of the full display rate of the device. The animation might look
degraded compared to other transitions. Nevertheless, this feature can be
especially useful for testing purposes where one set of stills has to match
another set.

## Playback speed

Playback speed is a value that describes the ratio between the total duration of
the original animation and the duration at which the animation is playing. 

This value is not part of the animation format but it should be an implicit or
explicit value of the runtime engine. Default playback speed is 1 which means
that the animation duration and playback duration are equal.

Negative values are supported, and they describe an animation that is played in
a reverse direction. (See direction as another option to reverse animation
playback.)

It is encouraged to expose an API to modify this value, but there is no
specification on how that API should be implemented.

This capability, combined with the previous refresh rate, allows for effects
like slow motion or high rate animations.

## Direction

Playback direction describes the direction at which the animation is
progressing. This value is not part of the animation format, but it is part of
the runtime engine. Its value can be either 1 or -1. Implicitly it has a default
value of 1, which means that the animation is running in its original direction.

It is encouraged to expose an API to modify this value, but there is no
specification on how that API should be implemented.

## Loop

Loop describes if the animation should restart once it reaches the end. This
value is not part of the animation format, but it is part of the runtime engine.
Its value can be either a boolean or a natural number including the 0 value. If
the value is 1, the animation should loop one time after it reaches the end,
totalling a number of 2 full cycles.

The true value should mean infinite loops, and false should mean 0 loops.

# Property types

Lottie has multiple properties depending on what type of layer, content and
attribute they are targeting. Below is the list of different types.

## Number

**Type**: number

A number property is a single floating point value. Depending on the attribute,
they can have valid ranges that will be specified when necessary.

## Array

**Type: **array[Number]

Some properties can have a single dimensional array containing the corresponding
value.

## String

**Type: **string

## Boolean

**Type: **boolean

## Shape

**Type: **object

Shapes are defined by four properties that describe the collection of bezier
curves that represent each path segment of the shape.

### Vertices

**Property Name**: *v*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

Vertices describe the collection of vertices needed to draw the bezier curves.

### In Points

**Property Name**: *i*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

In Points describe the collection of incoming control points needed to draw the
bezier curve. The number of in points should match the number of vertices of the
shape.

### Out Points

**Property Name**: *o*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

Out Points describe the collection of outgoing control points needed to draw the
bezier curve.  The number of out points should match the number of vertices of
the shape.

### Is Closed

**Property Name**: *c*

**Property Type**: Boolean

If true, the last vertex should connect to the first vertex of the list using
the out points of the last vertex and in points of the first  to draw a bezier
curve that closes the shape.

If false, the shape stroke between the first and last vertex should not be drawn
and the shape fill should describe a straight line between them.

## Color

**Type: **Tuples of length 3 and type Number

A Color is expressed as a 3-dimensional tuple where the first index is the red
component, the second is blue, and the third is green. All three values range
from 0 to 1.

Color is a special case of an Array.

## Gradient

**Type: **object

In order to express gradients in a concise way, gradients are described by two
properties.

### Points

**Property Name**: *p*

**Property Type**: Number

**Required**

Points describe the number of stops the gradient has.

### Colors

**Required**

**Property Name**: *k*

**Property Type**: Array[Number]

Each color stop has 4 values (color stop location, red, green, blue), and they
are all appended to the same array sequentially.

If the gradient has opacity, since opacity stops can differ from color stops,
they will be appended to the same array. All opacity values will be grouped at
the end of the array expressed as a pair of floating point numbers (opacity
color stop, opacity value).

Although opacity can have different color stop values, the total number of
opacity stops should be equal to the number of color stops.

In order to identify whether the array is representing only color values or also
opacity values, the Points property must be used.

## Text Document

### Font Family

**Property name:** *f*

**Property type**: string

**Required**

The font family of the rendered text

### Font Size

**Property name:** *s*

**Property type**: Number

**Required**

The font size of the text

### Font Caps

**Property name:** *ca*

**Property type**: Number

**Required**

Caps accepts 3 values. 

* 0 for regular, 

* 1 for All Caps, 

* 2 for Small Caps

### Paragraph Justification

**Property name:** *j*

**Property type**: Number

**Required**

Justification accepts 6 values.

* 0 for left justification

* 1 for right justification

* 2 for center justification

* 3 for full justification with last line aligned left

* 4 for full justification with last line aligned right

* 5 for full justification with last line aligned center

* 6 for full justification

### Text Tracking

**Property name:** *tr*

**Property type**: Number

**Required**

Tracking of the text

### Paragraph Line Height

**Property name:** *lh*

**Property type**: Number

**Required**

Line height of each text line

### Baseline Shift

**Property name:** *ls*

**Property type**: Number

**Required**

Distance from the text to its baseline. It can be a positive or negative value.

### Fill Color

**Property name:** *fc*

**Property type**: Color

**Optional**

Fill color of the text

### Stroke Color

**Property name:** *sc*

**Property type**: Color

**Optional**

Stroke color of the text

### Stroke Width

**Property name:** *sw*

**Property type**: Number

**Optional**

Stroke width of the text

### Box size

**Property name:** *sz*

**Property type**: Number

**Optional**

Text layers can have a text box that defines the boundaries of the container
where text would be rendered. Text lines should wrap around the box. If text
exceeds the box, it should clip it.

### Box position

**Property name:** *ps*

**Property type**: Coordinate

**Optional**

If the text layer has a box size defined, this property defines the position of
the box relative to the layer.

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

# Assets

Assets are a collection of source objects needed to fill layer information that
could be shared between multiple layer instances.

They range from preComps, audios, images, font binaries.

These objects do not always have a specified type, when they don’t, content can
be inferred from the requester.

## Precomps

Precomps have three properties: id, name and layers.

### Precomp id

**Property name:** *id*

**Property type:** Number

**Required**

The id of the precomp source

### Precomp name

**Property name:** *nm*

**Property type:** string

**Required**

The name of the source of the comp. This property is useful for expressions that
reference another composition.

### Precomp layers

**Property name:** *layers*

**Property type:** Array[Layers]

**Required**

The list of layers that compose a precomposition

## Images

Images contain the information to obtain an image source. It can be expressed in
different ways: inline or pointing to an external path.

### Asset type

**Property name:** *t*

**Property type:** Enum[Number]

**Property value:** 1

**Optional**

The type of the asset

### Image id

**Property name:** *id*

**Property type:** Number

**Required**

The id of the image source

### Image mode

**Property name:** *e*

**Property type:** Enum[Number]

**Required**

The way information is stored to retrieve the asset. Its values are

* 0 when the asset is loaded externally

* 1 when the asset is embedded inline in the json file

### Image width

**Property name:** *w*

**Property type:** Number

**Required**

The width of the image source

### Image height

**Property name:** *h*

**Property type:** Number

**Required**

The height of the image source

### Image path

**Property name:** *u*

**Property type:** String

**Required**

The path of the image excluding the file name

### Image name

**Property name:** *p*

**Property type:** String

**Required**

If asset mode is 0, it indicates the file name of the image.

If asset mode is 1, it contains the embedded image encoded as base 64.

## Audio

Audio contains the information to obtain an audio source. It can be expressed in
different ways: inline or pointing to an external path.

### Asset type

**Property name:** *t*

**Property type:** Enum[Number]

**Property value:** 2

**Required**

The type of the asset

### Audio id

**Property name:** *id*

**Property type:** Number

**Required**

The id of the audio source

### Audio mode

**Property name:** *e*

**Property type:** Enum[Number]

**Required**

The way information is stored to retrieve the asset. Its values are

* 0 when the asset is loaded externally

* 1 when the asset is embedded inline in the json file

### Audio path

**Property name:** *u*

**Property type:** String

**Required**

The path of the audio excluding the file name

### Audio name

**Property name:** *p*

**Property type:** String

**Required**

If asset mode is 0, it indicates the file name of the audio.

If asset mode is 1, it contains the embedded audio encoded as base 64.

# Text Data

In order to render text, two different solutions are provided by the format.
Text can be exported as glyphs or as regular text data.

## Glyphs

Glyphs allows Lottie to detach itself from any external font file to render
text. By including every character as a shape described by bezier curves, it can
render text independently from the original font.

### Chars

**Property name:** *chars*

**Property type:** Array[Char]

The chars array contains the list of all characters available to be rendered.

#### Char Object

**Property type:** Object

A character is described by a set of properties to identify its font family,
shape and other necessary properties to be rendered.

##### Character

**Property name:** *ch*

**Property type:** String

**Required**

The character string. It should be used to map the character requested by a text
layer.

##### Style

**Property name:** *style*

**Property type:** String

**Required**

The font style. It should be used to map the character requested by a text
layer.

##### Font Family

**Property name:** *fFamily*

**Property type:** String

**Required**

The font family. It should be used to map the character requested by a text
layer.

##### Advance Width

**Property name:** *w*

**Property type:** Number

**Required**

The distance at which the following character should be drawn, expressed
relative to a font size of 100px.

##### Data

**Property name:** *data*

**Property type:** object

**Required**

The data object contains a set of extra properties relative to the character.

###### Shapes

**Property name:** *shapes*

**Property type:** Array[Shape]

**Required**

The list of shapes that represent the character

## Fonts

**Property name:** *fonts*

**Property type:** object

**Required**

Fonts contain a set of properties related to font information needed to render
text layers.

### Fonts List

**Property name:** *list*

**Property type:** Array[Font]

The list property contains the array of fonts needed to render all text layers.

#### Font

**Property type:** Object

Each font describes the font information needed to render a specific text layer
type.

##### Font Origin

**Property name:** *origin*

**Property type:** Enum[Number]

**Required**

The font origin indicates how to interpret the other properties in order to load
the font

* 0 for None

* 1 for Google

* 2 for Typekit

* 3 for URL

##### Font Path

**Property name:** *fPath*

**Property type:** String

**Required**

The path that should be used to load the font

##### Font Class

**Property name:** *fClass*

**Property type:** String

**Optional**

The class that should be assigned to the text element for it to get the font
assigned via a css selector

##### Font Name

**Property name:** *fName*

**Property type:** String

**Required**

The name of the font to identify which font should be used to render the text

##### Font Family

**Property name:** *fFamily*

**Property type:** String

**Required**

The value of the font family

##### Font Weight

**Property name:** *fWeight*

**Property type:** String

**Required**

The value of the font weight

##### Font Style

**Property name:** *fStyle*

**Property type:** String

**Required**

The value of the font style

##### Ascent

**Property name:** *ascent*

**Property type:** Number

**Required**

The value of the font ascent expressed relative to a font size of 100px. The
ascent references the yOffset by which to draw the character.

# Layers

## Introduction

A collection of layers describes an animation or a composition (see compositions
for more information). 

They are the building blocks of an animation. The order of layers represents (in
most cases) the order in which they should be rendered. The first element in the
list should be rendered above all the other elements on the stack.

Some exceptions to this rule are camera layers that don’t have a visual
representation on the canvas. Others, that are not yet part of the
specification, are Light layers, and Adjustment layers.

Layers are categorized by the type of content, and each should be rendered
according to their unique specification.

## Layer Properties

Layers have common properties and unique properties related to their content. In
this section common properties will be covered. 

### Transform

**Property names**: *ks *

**Property type**: object

**Required**

The transform property is a container for a set of properties that are
traditionally related to GPU operations on textures. They represent affine
operations and opacity.

#### Anchor Point

**Property name**: *a *

**Property type**: Non-Animated Array | Spatial Animated Property

**Required**

The Anchor point is a two-dimensional (or three if layer is 3d) array that
represents the origin from which other transformations should be applied.

#### Position

**Property name**: *p*

**Property type**: object | Non-Animated Array | Spatial Animated Property

**Required**

The position can be expressed in two different ways, and it will be indicated by
a property called "s". If s is true, dimensions are separate. If s is false, it
behaves as an Array.

##### Position Separate Dimensions

**Property names**: *x, y, z *

**Property types**: Non-Animated Number | Multi Dimensional Animated Property

**Required**

If dimensions are separate, the position will be a container for 2 (or 3 if the
layer is 3d) one-dimensional independent properties. Those properties are "x",
“y”, and “z” and they can be animated separately.

#### Scale

**Property name**: *s *

**Property type**: Non-Animated Array | Multi Dimensional Animated Property

**Required**

The scale is a two dimensional (or three if layer is 3d) array that represents
the scaling percentage of the layer. If its value is 100%, no scaling is
applied.

#### Rotation for 2d layers

**Property name**: *r*

**Property type**: Non-Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the rotation expressed in degrees applied to the layer.

#### X Rotation for 3d layers

**Property names**: *rx *

**Property type**: Non-Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the x rotation expressed in degrees applied to the layer.

#### Y Rotation for 3d layers

**Property names**: *ry *

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the y rotation expressed in degrees applied to the layer.

#### Z Rotation for 3d layers

**Property names**: *rz *

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the z rotation expressed in degrees applied to the layer.

#### Orientation for 3d layers

**Property name**: *or*

**Property type**: Non Animated Array | Spatial Animated Property

**Optional**

3d layers have an extra property called orientation represented by an array of 3
floating point values expressed in degrees. It indicates the pitch, roll and yaw
of the layer.

#### Opacity

**Property name**: *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**REquired**

Opacity is a percentage based value. It defaults to 100 which means the layer is
fully opaque. Value 0 means the layer is fully transparent.

This property ranges from 0 to 100.

#### Skew

**Property name**: *sk*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Skew indicates the slant of the element.

#### Skew Axis

**Property name**: *sa*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Skew axis specifies the axis along which the character is skewed.

### Index

**Property name**: *ind*

**Property type**: Number

The index refers to a unique id that each layer has within a stack. It is used
for parenting and also for certain expressions.

### Parenting

**Property name**: *parent*

**Property type**: Number

**Optional**

If present, it points to the *ind* property of the target layer whose transform
data should be included in the transform operations that affect the layer. When
a layer has this attribute set, in order to draw the content, first all the
parenting hierarchy needs to be looked up iteratively. Once the parenting chain
is complete (the top layer doesn’t have a parent property set to look up),
transforms have to be applied from the top layer down to the transforms of the
current layer.

Note: Parenting should never target the original layer as part of the chain or
it would create an endless loop.

### Time Stretch

**Property name**: *sr*

**Property type**: Number

**Optional**

**Default: 1**

This is a factor by which time should be stretched on keyframe information. A
value of 1 means that no stretching is needed.

## Masks

**Property name:** *masksProperties*

**Property type**: Array[Mask Elements]

Masks define a list of bezier curve shapes that act as clipping paths or
composite operations on layers. Multiple masks with different properties can be
applied to the same layer where, depending on the mask mode, different rules of
stacking apply.

### Matte Masks

Matte masks are pairs of layers where one acts as a mask for the other. Those
layers are indicated by a tt property on the masked layer and a td on the
masking one. They are always stacked one after to the other on the stacking
order.

#### Masked Layer

**Property name:** *tt*

**Property type**: Enum[Number]

**Optional (Required if previous layer has a td property)**

This property only accepts four values and describes the type of mask that
should be applied.

Its values are

* 1 for Alpha Mask

* 2 for Inverted Alpha Mask

* 3 for Luminance Mask

* 4 for Inverted Luminance Mask

#### Masking Layer

**Property name:** *td*

**Property type**: Number

**Property value**: 1

**Optional (Required if next layer has a tt property)** 

This property only accepts one value that indicates it should be used as a
masking layer

### Mask elements

A mask can be considered as a clipping shape or a composite operation depending
on their specific properties and types.

#### Mask modes

**Property name:** *mode*

**Property type**: String

**Required**

Mask types describe how the shape should affect the underlying layer.

* **None**: Mask has no effect on the layer. This mode is solely used for
  attaching shapes to a layer that can be used by other properties, effects or
  expressions.

* **Add**: This mode will always take the original layer as input and add the
  opacity values to the final output.

* **Subtract: **This mode will take the output of masks higher in the stack as
  input. If there are no masks, it will take the layer as input. As output it
  will subtract the opacity values from the input.

* **Intersect:** This mode outputs the areas of opacity that overlap with masks
  higher in the stack.

* **Difference:** This mode outputs the subtracted areas of opacity that overlap
  with masks higher in the stack.

#### Inverted property

**Property name:** *inv*

**Property type**: Boolean

**Required**

The inverted property will take the opacity values of the output of the mask
(with its corresponding higher stack of masks if it applies) and perform a
boolean filter to invert the result.

#### Path property

**Property name:** *pt*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

The path property describes the bezier path that the mask will use to apply the
mask

#### Opacity property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity property defines how clipped pixels will transfer to the final
output. When opacity is set to 0.5, the resulting output will multiply the
alpha.

#### Expansion property

**Property name:** *x*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Expansion affects the path property of the mask by shrinking or growing it by
the specified number of pixels. The path should be transformed by the center of
the shape itself.

Since offsetting bezier curves is not a trivial task, this effect might be hard
to copy.

Lottie-web makes use of the feMorphology filter and applies an erode operator to
shrink the mask. For growing it, it adds a stroke to the mask which is used as
part of the masking itself.

## Layer Types

Each layer has a type attribute (‘ty’) which indicates the type of content.

### Solid Layer Type

A solid layer is the simplest type of drawable layer. It has three properties
specific to the layer type (color, width, height) and others shared with other
layer types (transform, masks, effects, layer styles).

#### Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 1

### Shape Layer Type

Shape layers define a set of shapes and shape modifiers grouped together. It has
a single specific property (shapes) and others shared with other layer types
(transform, masks, effects, layer styles).

#### Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 2

#### Shapes Property

**Property name:** *shapes*

**Property type**: Array[Shape Properties]

Shapes is a collection of paths and path modifiers like fills, colors, groups,
trim paths. Each property has a type defined by the "ty" prop.

#### Group Element

**type**: *gr*

A group contains a list of shape properties that are rendered as part of that
group. Any modifiers defined within the group will only apply to the stack
preceding the modifier but only scoped inside the group.

##### Items Property

**Property name:** *it*

**Property type**: Array[Shape Properties]

**Required**

The list of items defined within the group

#### Transform Element

**Property name**: *tr*

**Property type:** Transform

**Required**

A transform property that is applied to the stack of elements preceding it

#### Rectangle Element

**type**: *rc*

A rectangle is a parametric shape defined by its size, position and roundness.

##### Size Property

**Property name:** *s*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

Size is a two dimensional array that represents the width and height of the
rectangle.

##### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

##### Roundness Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Roundness is a one-dimensional value that represents the roundness of the
rectangle corners.

#### Ellipse Element

**type**: *el*

An ellipse is a parametric shape defined by its size and position

##### Size Property

**Property name:** *s*

**Property type**:Non Animated Array | Multi Dimensional Animated Property

Size is a two-dimensional array that represents the width and height of the
rectangle.

##### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group.

#### Polystar Element

**type**: *sr*

This element has two different subtypes that define a Star or a Polygon.

Its points, position, rotation, outer radius and outer roundness define a
polygon.

A star is defined by all the same properties as a polygon, and it includes an
inner roundness and inner radius.

##### Subtype Property

**Property name:** *sy*

**Property type**: Enum[Number]

* 1 for Star

* 2 for Polygon

**Required**

##### Points Property

**Property name:** *pt*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

Points is a one-dimensional number that defines the number of outer corners the
parametric shape has.

For a polygon, it also equals the total number of corners.

For a star, the number of corners is double, since every segment is subdivided
by an outer circle and an inner circle.

##### Position Property

**Property name:** *p*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

Position is a two-dimensional array that represents the coordinates of the
rectangle relative to its containing group. 

##### Rotation Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Position is a one-dimensional number that represents the rotation angle in
degrees of the shape.

##### Outer Radius Property

**Property name:** *or*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Outer radius is a one-dimensional number that represents the radius of a circle
where the outer points of the shape should be inscribed.

##### Outer Roundness Property

**Property name:** *os*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

Outer roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the shape should be
drawn. Those control points should be tangent to the circle defined by the outer
radius property.

##### Inner Radius Property

**Property name:** *ir*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

Inner radius is a one-dimensional number that represents the radius of a circle
where the inner points of the star should be inscribed.

##### Inner Roundness Property

**Property name:** *is*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

Inner roundness is a one-dimensional number that represents the length of the
control points of a bezier curve by which the corners of the inner points of the
star should be drawn. Those control points should be tangent to the circle
defined by the inner radius property.

#### Shape Element

**type**: *sh*

This element has a single property defining the path of the shape

##### Path Property

**Property name:** *ks*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

The shape described by the collection of bezier curves.

#### Fill Property

**type**: *fl*

**Required**

This property defines the fill color that should be applied to the stack of
elements preceding it. It has two inner properties, color and opacity

##### Color Property

**Property name:** *c*

**Property type**: Color | Multi Dimensional Animated Property

**Required**

The color of the fill

##### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

#### Gradient Fill Property

**type**: *gf*

This property defines the gradient fill color that should be applied to the
stack of elements preceding it. It has seven properties: Type, Start Point, End
Point, Highlight Angle, Highlight Length, Colors, Opacity.

##### Type Property

**Property name:** *t*

**Property type**: Enum[Number]

**Required**

This property only accepts two values and describes the type of gradient that
should be applied.

* 1 for Linear

* 2 for Radial

##### Start Point Property

**Property name:** *s*

**Property type**: Non Animated Array | Spatial Animated Property

**Required**

A two-dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

##### End Point Property

**Property name:** *e*

**Property type**: Non Animated Array | Spatial Animated Property

**Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

##### Highlight Length Property

**Property name:** *h*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

A value that offsets the starting point of the gradient.

##### Highlight Angle Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

A value that defines the angle by which the starting point of the gradient will
be offset.

##### Color Property

**Property name:** *g*

**Property type**: Gradient | Gradient Animated Property

**Required**

The set of colors that define the gradient

##### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the gradient fill

#### Stroke Property

**type**: *st*

This property defines the stroke color and width that should be applied to the
stack of elements preceding it. It has six inner properties, color, opacity,
stroke width, line cap, line join, miter limit.

##### Color Property

**Property name:** *c*

**Property type**: Color | Multi Dimensional Animated Property

**Required**

The color of the fill

##### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

##### Stroke Width Property

**Property name:** *w*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The width of the stroke applied to the shapes. It does not accept negative
numbers.

##### Line Cap Property

**Property name:** *lc*

**Property type**: Enum[Number]

**Required**

The line cap of the stroke

Its values are 

* 1 for Butt Cap

* 2 for Round Cap

* 3 for Projecting Cap

##### Line Join Property

**Property name:** *lj*

**Property type**: Enum[Number]

**Required**

The line join of the stroke

Its values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

##### Miter Limit Property

**Property name:** *ml*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

The miter limit is a one-dimensional value that applies only if the line join
value of the stroke is Miter Join.

#### Gradient Stroke Property

**type**: *st*

Defines the gradient stroke color and width that should be applied to the stack
of elements preceding it. It has eleven inner properties, color, opacity, stroke
width, line cap, line join, miter limit.

##### Color Property

**Property name:** *g*

**Property type**: Gradient | Gradient Animated Property 

**Required**

Describes the set of colors that define the gradient

##### Opacity Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity of the fill

##### Stroke Width Property

**Property name:** *w*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The width of the stroke applied to the shapes

##### Line Cap Property

**Property name:** *lc*

**Property type**: Number

**Required**

The line cap of the stroke

Its values are 

* 1 for Butt Cap

* 2 for Round Cap

* 3 for Projecting Cap

##### Line Join Property

**Property name:** *lj*

**Property type**: Number

**Required**

The line join of the stroke

Its values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

##### Miter Limit Property

**Property name:** *ml*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The miter limit is a value that applies only if the line join value of the
stroke is Miter Join.

##### Type Property

**Property name:** *t*

**Property type**: Enum[Number]

**Required**

This property only accepts two values and describes the type of gradient that
should be applied.

Its values are

* 1 for Linear

* 2 for Radial

##### Start Point Property

**Property name:** *s*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

A two dimensional array that represents the coordinates of the starting point of
the gradient interpolation.

##### End Point Property

**Property name:** *e*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

A two-dimensional array that represents the coordinates of the end point of the
gradient interpolation.

##### Highlight Length Property

**Property name:** *h*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

##### Highlight Angle Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

#### Merge Paths Property

**type**: *mm*

A merge path acts as a shape boolean operation applied to the set of shapes
preceding it. It contains a single property, the merge mode.

##### Merge Mode Property

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

#### Offset Paths Property

**type**: *op*

The offset path modifier should grow or shrink the set of shapes preceding it.
It is defined by three properties: amount, line join and miter limit.

##### Amount Property

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

A one-dimensional value that defines how much the shape should grow or shrink

##### Line Join Property

**Property name:** *lj*

**Property type**: Enum[Number]

The line join of the stroke

Its Values are 

* 1 for Miter Join

* 2 for Round Join

* 3 for Bevel Join

##### Miter Limit Property

**Property name:** *ml*

**Property type**: object

A one-dimensional value that applies only if the line join value of the stroke
is Miter Join

#### Repeater Property

**type**: *rp*

The repeater modifier creates copies of the set of shapes preceding it. It is
defined by three properties: copies, offset, transform.

##### Copies Property

**Property name:** *c*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines how many copies (including the original
shape) of the shapes should be drawn 

##### Offset Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines how many copies should be skipped before
starting the count of shapes to be drawn.

This value should be accounted for in order to calculate the following
*transform* property.

##### Transform Property

**Property name:** *tr*

**Property type**: Transform

**Required**

For each copy of a repeater, this transform operation is applied before the
drawing operation is applied. This allows to generate copies of the original
drawing with a combination of regular transform operations between them.

When the offset property is different from 0, these transform operations should
accumulate by the amount that the offset value defines before being applied to
the first copy.

#### Round Corners Property

**type**: *rd*

This modifier affects any vertex of a shape whose bezier control points are 0.
It has a single property that defines the radius of the effect.

##### Copies Property

**Property name:** *r*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

A one-dimensional value that defines the length of the control points of the
bezier curve that should be applied to the vertex. The direction of the control
point should be tangent to the vertex itself.

#### Trim Paths Property

**type**: *tm*

This modifier changes the shape of the set of elements preceding it. Although in
general, trim paths are expected to only affect the stroke of a shape, this
modifier should affect both how strokes and fills are applied.

The way the new shape is calculated should measure the full perimeter of the
shape individually (or the full set of shapes if Trim Multiple Shapes property
is set to Individually) and regenerate the new set of paths as a percentage of
the original set.

It is defined by four properties: Start, End, Offset and Trim Multiple Shapes.

##### Start Property

**Property name:** *s*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines the initial point of the shape that should
be drawn relative to the initial point of the original shape. It is expressed as
a percentage of the initial value.

##### End Property

**Property name:** *e*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines the end point of the shape that should be
drawn relative to the end point of the original shape. It is expressed as a
percentage of the end value.

##### Offset Property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A one-dimensional value that defines an offset by which the start and end points
should be calculated. A whole cycle is expressed as a 360 offset.

##### Trim Multiple Shapes Property

**Property name:** *m*

**Property type**: Enum[Number]

**Required**

This property accepts only two values:

* 1 for Simultaneous

* 2 for Individual

Simultaneous means that all shapes should be trimmed applying the calculations
separately. Individual means that a single trim effect should be applied to all
of them by calculating the full length of all shapes together.

### Text Layer Type

A text layer defines a container for text data. It has a single specific
property (text) and others shared with other layer types (transform, masks,
effects, layer styles).

#### Type Property

**Property name:** *ty*

**Property type**: Enum (Number)

**Property value**: 5

#### Text

**Property name:** *t*

**Property type**: object

**Required**

A text object is composed of four different objects: document data, animators,
text path, more options.

#### Document Data

**Property name:** *d*

**Property type**: Text Document | Text Document Animated Property

**Required**

This property contains a single animatable property, k, that is a set of all the
paragraph data of the text.

#### Text Path Data

**Property name:** *p*

**Property type**: object

**Required**

When enabled, a text layer will use a mask defined on the layer to describe an
irregular baseline that the text should follow when being rendered. If the text
has multiple lines, each line will use the same path described by the mask
offsetted by the line height of the text.

If a line of text exceeds the path described by the mask, both ends of the mask
should be extended infinitely, parallel to the the first and last vertex of the
path.

##### Mask

**Property name:** *m*

**Property type**: Number

**Optional**

The index of the mask defined in the *masksProperties *attribute of the layer
that will be used as baseline of the text.

##### First Margin

**Property name:** *f*

**Property type**: Non-Animated Number | Multi Dimensional Animated Property

A margin to offset the drawing of the text from the first vertex of the shape

##### Last Margin

**Property name:** *l*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A margin to offset the drawing of the text from the last vertex of the shape

##### Force Alignment

**Property name:** *a*

**Property type**: Boolean

**Required**

If active, each line of text should be rendered contained within the shape
defined by the mask by adjusting the tracking.

##### Perpendicular to Path

**Property name:** *p*

**Property type**: Boolean

**Required**

If active, text should be rendered perpendicular to the direction of the
baseline.

##### Reversed

**Property name:** *r*

**Property type**: Boolean

**Required**

If active, text should be rendered starting from the last vertex of the mask.

#### Other Options

**Property name:** *m*

**Property type**: object

**Required**

This object contains other properties affecting the rendering of the text.

##### Anchor Point Grouping

**Property name:** *g*

**Property type**: Enum(Number)

**Required**

This property defines how each character anchor point should be grouped relative
to the defined value to apply animators and drawing along a path.

Its values are:

* 1 aligned to character

* 2 aligned to word

* 3 aligned to line

* 4 aligned to all textbox

##### Grouping Alignment

**Property name:** *a*

**Property type**: Non Animated Array | Multi Dimensional Animated Property

**Required**

Controls the alignment of the anchor point relative to the anchor point group.
The tuple defines a pair of coordinates based on a percentage of the group
anchor point.

#### Animators

**Property name:** *a*

**Property type**: list

**Required**

Animators are a collection of transformations that can be applied to a text
layer. They consist of a range selector and a set of optional properties.

##### Range Selector

**Property name:** *s*

**Property type**: object

**Required**

The range selector property has a set of properties that define how
transformations are applied to the text. This range allows for animations on
more granular parts of the text, like characters, words, lines and the text box.

###### Type

**Property name:** *t*

**Property type**: Enum[Number]

**Required**

It specifies the type of selector, it can be expression based or parametric.

* 0 for parametric

* 1 for expression

###### Range Units

**Property name:** *r*

**Property type**: Enum[Number]

**Required**

It specifies the units that are used to calculate ranges.

* 1 for percentage based

* 2 for index based

###### Range Start

**Property name:** *s*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

It specifies the start of the range that transformations will be applied to. If
range units are percentage based, the values range from 0 to 100, if they are
index based, valid values are any positive number.

###### Range End

**Property name:** *e*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

It specifies the end of the range that transformations will be applied to. If
range units are percentage based, the values range from 0 to 100. If they are
index based, valid values are any positive number.

###### Range Offset

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

It specifies an offset of the range that transformations will be applied to. If
range units are percentage based, the values range from -100 to 100. If they are
index based, valid values are any positive number.

###### Range Base Mode

**Property name:** *b*

**Property type**: Enum[Number]

**Required**

Specifies how ranges should affect text. It has four values: Characters,
Characters excluding Spaces, Words and Lines. Depending on this option, all the
other properties will operate on the block specified by it.

* 1 for Characters

* 2 for Characters excluding Spaces

* 3 for Words

* 4 for Lines

###### Range Shape

**Property name:** *sh*

**Property type**: Enum[Number]

**Required**

The shape indicates how the range will operate over the selected blocks within
the range. The default value is square. You can think of this property as a
factor that should be multiplied to all transformations before applying the
final value.

* 1 for Square. All blocks within the range will have their transformations
  applied equally. Factor is always 1.

* 2 for Ramp Up. Transformations will be applied linearly increasing within the
  range. Factor grows from 0 to 1.

* 3 for Ramp Down. Transformations will be applied linearly decreasing within
  the range. Factor grows from 1 to 0.

* 4 for Triangle. Transformations will be applied linearly up to the middle of
  the range. First half increasingly, second half decreasing. Factor grows
  linearly from 0 to 1 and then from 1 to 0.

* 4 for Round. Similar to Triangle but instead of progressing linearly, it
  progresses describing half a circle.

* 5 for Smooth. Similar to Triangle but it describes two segments with easing
  values to progress from 0 to 1 and 1 to 0.

###### Amount

**Property name:** *a*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

A multiplier expressed in percentage applied to the result of the factor of
transformation

###### Max Ease

**Property name:** *xe*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

An easing value that affects the speed of change as selection values change from
fully included (high) to fully excluded (low)

###### Min Ease

**Property name:** *ne*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

An easing value that affects the speed of change as selection values change from
fully included (high) to fully excluded (low)

###### Random

**Property name:** *rn*

**Property type**: Enum[Number]

**Required**

If active, transformations should be applied randomly to each selected block.
Random seed is not enforced.

###### Transform Properties

This is the list of all supported properties that can modify a text within the
ranges described above:

* Anchor Point (**a**)

* Position (**p**)

* Scale (**s**)

* Rotation (**r**)

* Rotation X (**rx**)

* Rotation Y (**ry**)

* Opacity (**o**)

* Fill Color (**fc**)

* Fill Hue (**fh**)

* Fill Saturation (**fs**)

* Fill Brightness (**fb**)

* Fill Opacity (**fo**)

* Stroke Color (**sc**)

* Stroke Hue (**sh**)

* Stroke Saturation (**ss**)

* Stroke Brightness (**sb**)

* Stroke Width (**sw**)

* Stroke Opacity (**so**)

* Tracking (**t**)

* Text Skew (**sk**)

* Text Skew Axis (**sa**)

* Text Blur (**bl**)

* Text Line Spacing (**ls**)

All properties are animatable and optional.

### Image Layer Type

Image layers are a container for an image.

#### Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 2

#### Reference Property

**Property name:** refId

**Property type**: Number

**Required**

A reference to the asset id that should be painted within the container. The
asset information is located in the assets property and can be retrieved in
different ways.

### Null Layer Type

Null layers don’t have a visual representation. But they share the same
transform properties as any other drawable layer.

Null layers are often used to parent other layers to them so they can be used as
a detached hierarchy of transformations. Parenting can be recursive and null
layers can be parented to other null layers as well.

Another usage of null layers is for holding expressions properties that don’t
belong to a specific layer but can be shared among multiple ones.

They don’t have any specific properties but they share others with other layer
types (transform, masks, effects, layer styles).

#### Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 3

### Precomp Layer Type

A Precomps is a container for a set of layers that are grouped and are adjusted
by the properties that govern the precomp. It has four properties specific to
the layer type (width, height, time remap, refId) and others shared with other
layer types (transform, masks, effects, layer styles).

#### Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 0

#### Width

**Property name:** *w*

**Property type**: Number

**Required**

Width defines the width of the surface that has to be rendered from the precomp.
Whatever is outside that area won’t be visible even if it is within the visible
area of the precomp container. It basically works as the width of a clipping
mask of the inner layers.

#### Height

**Property name:** *h*

**Property type**: Number

**Required**

Height defines the height of the surface that has to be rendered from the
precomp. Whatever is outside that area will not be visible even if it is within
the visible area of the precomp container. It basically works as the height of a
clipping mask of the inner layers.

#### Time Remap

**Property name:** *tm*

**Property type:** Multi Dimensional Animated Property

**Required**

Time remap allows control of the timeline of a precomp independently from the
main timeline. It is expressed in seconds and since it is animatable, it can
support any amount of keyframes, easing and expressions.

#### Asset Reference

**Property name:** *refId*

**Property type:** Number

**Required**

The refId property points to an object on the assets list that completes the
composition information.

