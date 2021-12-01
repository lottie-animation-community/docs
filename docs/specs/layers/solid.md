# Solid Layer Type

A solid layer is the simplest type of drawable layer. It has three properties
specific to the layer type (color, width, height) and others shared with other
layer types (transform, masks, effects, layer styles).

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 1
[Solid Width](#solid-width-property) | sw | Number | ✅
[Solid Height](#solid-height-property) | sh | Number | ✅
[Solid Color](#solid-color-property) | sc | String | ✅
{!specs/layers/common-properties.md!}

## Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 1

## Solid Width Property

**Property name:** *sw*

**Property type**: Number

The width of the solid

## Solid Height Property

**Property name:** *sh*

**Property type**: Number

The height of the solid

## Solid Color Property

**Property name:** *sc*

**Property type**: String

An hexadecimal RGB color

