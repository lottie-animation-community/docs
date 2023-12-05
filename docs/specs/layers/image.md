# Image Layer Type

Image layers are a container for an image.

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 2
[Reference Property](#reference-property) | refId | String | ✅
{!specs/layers/common-properties.md!}

## Type Property

??? Details
    **Property name:** *ty*

    **Property type**: [Layer Types](../../properties/layer-types)

    **Property value**: 2

## Reference Property

??? Details
    **Property name:** refId

    **Property type**: String

    **Required**

A reference to the asset id that should be painted within the container. The
asset information is located in the assets property and can be retrieved in
different ways.
