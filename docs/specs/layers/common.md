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

??? Details
    **Property names**: *ks*

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

??? Details
    **Property name**: *a*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

The Anchor point is a two-dimensional (or three if layer is 3d) array that
represents the origin from which other transformations should be applied.

### Position

??? Details
    **Property name**: *p*

    **Property type**: object | [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Required**

The position can be expressed in two different ways, and it will be indicated by
a property called "s". If s is true, dimensions are separate. If s is false, it
behaves as an Array.

#### Position Separate Dimensions

??? Details
    **Property names**: *x, y, z *

    **Property types**: Non-Animated Number | Multi Dimensional Animated Property

    **Required**

If dimensions are separate, the position will be a container for 2 (or 3 if the
layer is 3d) one-dimensional independent properties. Those properties are "x",
“y”, and “z” and they can be animated separately.

### Scale

??? Details
    **Property name**: *s*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The scale is a two dimensional (or three if layer is 3d) array that represents
the scaling percentage of the layer. If its value is 100%, no scaling is
applied.

### Rotation for 2d layers

??? Details
    **Property name**: *r*

    **Property type**:[Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

This property is the rotation expressed in degrees applied to the layer.

### X Rotation for 3d layers

??? Details
    **Property names**: *rx*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

This property is the x rotation expressed in degrees applied to the layer.

### Y Rotation for 3d layers

??? Details
    **Property names**: *ry*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

This property is the y rotation expressed in degrees applied to the layer.

### Z Rotation for 3d layers

??? Details
    **Property names**: *rz*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Conditional**

This property is the z rotation expressed in degrees applied to the layer.

### Orientation for 3d layers

??? Details
    **Property name**: *or*

    **Property type**: [Array](../../properties/prop-types/#array) | [Spatial Animated Property](../../properties/animatable-properties/#spatial-animated-property)

    **Optional**

3d layers have an extra property called orientation represented by an array of 3
floating point values expressed in degrees. It indicates the pitch, roll and yaw
of the layer.

### Opacity

??? Details
    **Property name**: *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **REquired**

Opacity is a percentage based value. It defaults to 100 which means the layer is
fully opaque. Value 0 means the layer is fully transparent.

This property ranges from 0 to 100.

### Skew

??? Details
    **Property name**: *sk*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Skew indicates the slant of the element.

### Skew Axis

??? Details
    **Property name**: *sa*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

Skew axis specifies the axis along which the character is skewed.

## Index

??? Details
    **Property name**: *ind*

    **Property type**: [Number](../../properties/prop-types/#number)

The index refers to a unique id that each layer has within a stack. It is used
for parenting and also for certain expressions.

## Parenting

??? Details
    **Property name**: *parent*

    **Property type**: [Number](../../properties/prop-types/#number)

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

??? Details
    **Property name**: *ip*

    **Property type**: [Number](../../properties/prop-types/#number)

The in point is a frame based Number that indicates the inital frame of the
layer

## Out Point

??? Details
    **Property name**: *op*

    **Property type**: [Number](../../properties/prop-types/#number)

The out point is a frame based Number that indicates the final frame of the
layer


## Start Point

??? Details
    **Property name**: *st*

    **Property type**: [Number](../../properties/prop-types/#number)

The start point is a frame based Number that indicates when the layer should be
visible on screen


## Time Stretch

??? Details
    **Property name**: *sr*

    **Property type**: [Number](../../properties/prop-types/#number)

    **Optional**

    **Default: 1**

This is a factor by which time should be stretched on keyframe information. A
value of 1 means that no stretching is needed.

# Masks

??? Details
    ## Clipping Masks

    **Property name:** *masksProperties*

    **Property type**: Array[[Mask Elements](#mask-elements)]

Masks define a list of bezier curve shapes that act as clipping paths or
composite operations on layers. Multiple masks with different properties can be
applied to the same layer where, depending on the mask mode, different rules of
stacking apply.

## Mask elements

A mask can be considered as a clipping shape or a composite operation depending
on their specific properties and types.

### Mask modes

??? Details
    **Property name:** *mode*

    **Property type**: [Mask Mode types](../../properties/mask-mode-types)

    **Required**

Mask types describe how the shape should affect the underlying layer.

### Inverted property

??? Details
    **Property name:** *inv*

    **Property type**: [Boolean](../../properties/prop-types/#boolean)

    **Required**

The inverted property will take the opacity values of the output of the mask
(with its corresponding higher stack of masks if it applies) and perform a
boolean filter to invert the result.

### Path property

??? Details
    **Property name:** *pt*

    **Property type**: [Shape](../../properties/prop-types/#shape) | [Shape Animated Property](../../properties/animatable-properties/#shape-animated-property)

    **Required**

The path property describes the bezier path that the mask will use to apply the
mask

### Opacity property

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

The opacity property defines how clipped pixels will transfer to the final
output. When opacity is set to 0.5, the resulting output will multiply the
alpha.

### Expansion property

??? Details
    **Property name:** *x*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

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

??? Details
    **Property name:** *tt*

    **Property type**: [Matte Mask types](../../properties/matte-mask-types)

    **Optional (Required if previous layer has a td property)**

This property only accepts four values and describes the type of mask that
should be applied.

### Masking Layer

??? Details
    **Property name:** *td*

    **Property type**: Number

    **Property value**: 1

    **Optional (Required if next layer has a tt property)** 

This property only accepts one value that indicates it should be used as a
masking layer