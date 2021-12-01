# Null Layer Type

Null layers don’t have a visual representation. But they share the same
transform properties as any other drawable layer.

Null layers are often used to parent other layers to them so they can be used as
a detached hierarchy of transformations. Parenting can be recursive and null
layers can be parented to other null layers as well.

Another usage of null layers is for holding expressions properties that don’t
belong to a specific layer but can be shared among multiple ones.

They don’t have any specific properties but they share others with other layer
types (transform, masks, effects, layer styles).

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 3
{!specs/layers/common-properties.md!}

## Type Property

**Property name:** *ty*

**Property type**: Enum[Number]

**Property value**: 3
