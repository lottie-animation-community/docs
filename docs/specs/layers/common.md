# Introduction

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

{!specs/properties/layer-types.md!}

# Common Layer Properties

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
{!specs/layers/common-properties.md!}

Layers have common properties and unique properties related to their content. In
this section common properties will be covered. 

## Transform

**Property names**: *ks *

**Property type**: object

**Required**

The transform property is a container for a set of properties that are
traditionally related to GPU operations on textures. They represent affine
operations and opacity.

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Anchor Point](#anchor-point) | a | [Array](../../properties/prop-types/#array) \| [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property) | ✅ 
[Position](#position) | p | [Array](../../properties/prop-types/#array) \| [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property) | ✅ 
[Scale](#scale) | s | [Array](../../properties/prop-types/#array) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ✅ 
[2d Rotation](#rotation-for-2d-layers) | r | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 
[3d X Rotation](#x-rotation-for-3d-layers) | rx | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 
[3d Y Rotation](#y-rotation-for-3d-layers) | ry | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 
[3d Z Rotation](#z-rotation-for-3d-layers) | rz | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 
[3d Orientation](#orientation-for-3d-layers) | or | [Array](../../properties/prop-types/#array) \|  [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property) | ➖ 
[Opacity](#opacity) | o | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ✅ 
[Skew](#skew) | sk | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 
[Skew Axis](#skew-axis) | sa | [Number](../../properties/prop-types/#number) \| [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property) | ➖ 

### Anchor Point

**Property name**: *a*

**Property type**: Non-Animated Array | Spatial Animated Property

**Required**

The Anchor point is a two-dimensional (or three if layer is 3d) array that
represents the origin from which other transformations should be applied.

### Position

**Property name**: *p*

**Property type**: object | Non-Animated Array | Spatial Animated Property

**Required**

The position can be expressed in two different ways, and it will be indicated by
a property called "s". If s is true, dimensions are separate. If s is false, it
behaves as an Array.

#### Position Separate Dimensions

**Property names**: *x, y, z *

**Property types**: Non-Animated Number | Multi Dimensional Animated Property

**Required**

If dimensions are separate, the position will be a container for 2 (or 3 if the
layer is 3d) one-dimensional independent properties. Those properties are "x",
“y”, and “z” and they can be animated separately.

### Scale

**Property name**: *s*

**Property type**: Non-Animated Array | Multi Dimensional Animated Property

**Required**

The scale is a two dimensional (or three if layer is 3d) array that represents
the scaling percentage of the layer. If its value is 100%, no scaling is
applied.

### Rotation for 2d layers

**Property name**: *r*

**Property type**: Non-Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the rotation expressed in degrees applied to the layer.

### X Rotation for 3d layers

**Property names**: *rx *

**Property type**: Non-Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the x rotation expressed in degrees applied to the layer.

### Y Rotation for 3d layers

**Property names**: *ry *

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the y rotation expressed in degrees applied to the layer.

### Z Rotation for 3d layers

**Property names**: *rz *

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Conditional**

This property is the z rotation expressed in degrees applied to the layer.

### Orientation for 3d layers

**Property name**: *or*

**Property type**: Non Animated Array | Spatial Animated Property

**Optional**

3d layers have an extra property called orientation represented by an array of 3
floating point values expressed in degrees. It indicates the pitch, roll and yaw
of the layer.

### Opacity

**Property name**: *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**REquired**

Opacity is a percentage based value. It defaults to 100 which means the layer is
fully opaque. Value 0 means the layer is fully transparent.

This property ranges from 0 to 100.

### Skew

**Property name**: *sk*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Skew indicates the slant of the element.

### Skew Axis

**Property name**: *sa*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

Skew axis specifies the axis along which the character is skewed.

## Index

**Property name**: *ind*

**Property type**: Number

The index refers to a unique id that each layer has within a stack. It is used
for parenting and also for certain expressions.

## Parenting

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

## In Point

**Property name**: *ip*

**Property type**: Number

The in point is a frame based Number that indicates the inital frame of the
layer

## Out Point

**Property name**: *op*

**Property type**: Number

The out point is a frame based Number that indicates the final frame of the
layer


## Start Point

**Property name**: *st*

**Property type**: Number

The start point is a frame based Number that indicates when the layer should be
visible on screen


## Time Stretch

**Property name**: *sr*

**Property type**: Number

**Optional**

**Default: 1**

This is a factor by which time should be stretched on keyframe information. A
value of 1 means that no stretching is needed.

# Masks

## Clipping Masks

**Property name:** *masksProperties*

**Property type**: Array[Mask Elements]

Masks define a list of bezier curve shapes that act as clipping paths or
composite operations on layers. Multiple masks with different properties can be
applied to the same layer where, depending on the mask mode, different rules of
stacking apply.

## Mask elements

A mask can be considered as a clipping shape or a composite operation depending
on their specific properties and types.

### Mask modes

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

### Inverted property

**Property name:** *inv*

**Property type**: Boolean

**Required**

The inverted property will take the opacity values of the output of the mask
(with its corresponding higher stack of masks if it applies) and perform a
boolean filter to invert the result.

### Path property

**Property name:** *pt*

**Property type**: Non Animated Shape | Shape Animated Property

**Required**

The path property describes the bezier path that the mask will use to apply the
mask

### Opacity property

**Property name:** *o*

**Property type**: Non Animated Number | Multi Dimensional Animated Property

**Required**

The opacity property defines how clipped pixels will transfer to the final
output. When opacity is set to 0.5, the resulting output will multiply the
alpha.

### Expansion property

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



## Matte Masks

Matte masks are pairs of layers where one acts as a mask for the other. Those
layers are indicated by a tt property on the masked layer and a td on the
masking one. They are always stacked one after to the other on the stacking
order.

### Masked Layer

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

### Masking Layer

**Property name:** *td*

**Property type**: Number

**Property value**: 1

**Optional (Required if next layer has a tt property)** 

This property only accepts one value that indicates it should be used as a
masking layer