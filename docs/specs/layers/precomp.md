# Precomp Layer Type

A Precomps is a container for a set of layers that are grouped and are adjusted
by the properties that govern the precomp. It has four properties specific to
the layer type (width, height, time remap, refId) and others shared with other
layer types (transform, masks, effects, layer styles).

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 0
[Width](#width) | w | Number | ✅
[Height](#height) | h | Number | ✅
[Time Remap](#time-remap) | tm | [Multi Dimensional Animated Property](/specs/properties/animatable-properties/#multi-dimensional-animated-property) | ✅
[Asset Reference](#asset-reference) | refId | String | ✅
{!specs/layers/common-properties.md!}

## Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 0

## Width

**Property name:** *w*

**Property type**: Number

**Required**

Width defines the width of the surface that has to be rendered from the precomp.
Whatever is outside that area won’t be visible even if it is within the visible
area of the precomp container. It basically works as the width of a clipping
mask of the inner layers.

## Height

**Property name:** *h*

**Property type**: Number

**Required**

Height defines the height of the surface that has to be rendered from the
precomp. Whatever is outside that area will not be visible even if it is within
the visible area of the precomp container. It basically works as the height of a
clipping mask of the inner layers.

## Time Remap

**Property name:** *tm*

**Property type:** Multi Dimensional Animated Property

**Required**

Time remap allows control of the timeline of a precomp independently from the
main timeline. It is expressed in seconds and since it is animatable, it can
support any amount of keyframes, easing and expressions.

## Asset Reference

**Property name:** *refId*

**Property type:** String

**Required**

The refId property points to an object on the assets list that completes the
composition information.

