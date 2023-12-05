# Text Layer Type

A text layer defines a container for text data. It has a single specific
property (text) and others shared with other layer types (transform, masks,
effects, layer styles).

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Type](#type-property) | ty | [Layer Types](../../properties/layer-types) | ✅ | 5
[Text](#text) | t | Object | ✅
{!specs/layers/common-properties.md!}

## Type Property

??? Details
    **Property name:** *ty*

    **Property type**: [Layer Types](../../properties/layer-types)

    **Property value**: 5

## Text

??? Details
    **Property name:** *t*

    **Property type**: object

    **Required**

A text object is composed of four different objects: document data, animators,
text path, more options.

Definition | Name | Type | Required | Default
-- | :--: | :--: | :--: | :--:
[Document Data](#document-data) | d | [Text Document](../../properties/prop-types#text-document) \| [Text Document Animated Property](../../properties/animatable-properties#text-document-animated-property) | ✅
[Text Path Data](#text-path-data) | p | Object | ✅
[Other Options](#other-options) | m | Object | ✅
[Animators](#animators) | a | Array[Object] | ✅

### Document Data

??? Details
    **Property name:** *d*

    **Property type**: [Text Document](../../properties/prop-types/#text-document) | [Text Document Animated Property](../../properties/animatable-properties#text-document-animated-property)       

    **Required**

This property contains a single animatable property, k, that is a set of all the
paragraph data of the text.

### Text Path Data

??? Details
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

#### Mask

??? Details
    **Property name:** *m*

    **Property type**: [Number](../../properties/prop-types/#number)

    **Optional**

The index of the mask defined in the *masksProperties *attribute of the layer
that will be used as baseline of the text.

#### First Margin

??? Details
    **Property name:** *f*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

A margin to offset the drawing of the text from the first vertex of the shape

#### Last Margin

??? Details
    **Property name:** *l*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A margin to offset the drawing of the text from the last vertex of the shape

#### Force Alignment

??? Details
    **Property name:** *a*

    **Property type**: [Boolean](../../properties/prop-types/#boolean)

    **Required**

If active, each line of text should be rendered contained within the shape
defined by the mask by adjusting the tracking.

#### Perpendicular to Path

??? Details
    **Property name:** *p*

    **Property type**: [Boolean](../../properties/prop-types/#boolean)

    **Required**

If active, text should be rendered perpendicular to the direction of the
baseline.

#### Reversed

??? Details
    **Property name:** *r*

    **Property type**: [Boolean](../../properties/prop-types/#boolean)

    **Required**

If active, text should be rendered starting from the last vertex of the mask.

### Other Options

??? Details
    **Property name:** *m*

    **Property type**: object

    **Required**

This object contains other properties affecting the rendering of the text.

#### Anchor Point Grouping

??? Details
    **Property name:** *g*

    **Property type**: [Anchor Point Grouping types](../../properties/anchor-point-grouping-types)

    **Required**

This property defines how each character anchor point should be grouped relative
to the defined value to apply animators and drawing along a path.

#### Grouping Alignment

??? Details
    **Property name:** *a*

    **Property type**: [Array](../../properties/prop-types/#array) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

Controls the alignment of the anchor point relative to the anchor point group.
The tuple defines a pair of coordinates based on a percentage of the group
anchor point.

### Animators

??? Details
    **Property name:** *a*

    **Property type**: list

    **Required**

Animators are a collection of transformations that can be applied to a text
layer. They consist of a range selector and a set of optional properties.

#### Range Selector

??? Details
    **Property name:** *s*

    **Property type**: object

    **Required**

The range selector property has a set of properties that define how
transformations are applied to the text. This range allows for animations on
more granular parts of the text, like characters, words, lines and the text box.

##### Type

??? Details
    **Property name:** *t*

    **Property type**: [Text Selector types](../../properties/text-selector-types)

    **Required**

It specifies the type of selector, it can be expression based or parametric.

##### Range Units

??? Details
    **Property name:** *r*

    **Property type**: [Range Unit types](../../properties/range-unit-types)

    **Required**

It specifies the units that are used to calculate ranges.

##### Range Start

??? Details
    **Property name:** *s*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

It specifies the start of the range that transformations will be applied to. If
range units are percentage based, the values range from 0 to 100, if they are
index based, valid values are any positive number.

##### Range End

??? Details
    **Property name:** *e*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

It specifies the end of the range that transformations will be applied to. If
range units are percentage based, the values range from 0 to 100. If they are
index based, valid values are any positive number.

##### Range Offset

??? Details
    **Property name:** *o*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

It specifies an offset of the range that transformations will be applied to. If
range units are percentage based, the values range from -100 to 100. If they are
index based, valid values are any positive number.

##### range base mode

??? Details
    **Property name:** *b*

    **Property type**: [Range Base Mode types](../../properties/range-base-mode-types)

    **Required**

Specifies how ranges should affect text. It has four values: Characters,
Characters excluding Spaces, Words and Lines. Depending on this option, all the
other properties will operate on the block specified by it.

##### Range Shape

??? Details
    **Property name:** *sh*

    **Property type**: [Range Shape types](../../properties/range-shape-types)

    **Required**

The shape indicates how the range will operate over the selected blocks within
the range. The default value is square. You can think of this property as a
factor that should be multiplied to all transformations before applying the
final value.

##### Amount

??? Details
    **Property name:** *a*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

A multiplier expressed in percentage applied to the result of the factor of
transformation

##### Max Ease

??? Details
    **Property name:** *xe*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

An easing value that affects the speed of change as selection values change from
fully included (high) to fully excluded (low)

##### Min Ease

??? Details
    **Property name:** *ne*

    **Property type**: [Number](../../properties/prop-types/#number) | [Multi Dimensional Animated Property](../../properties/animatable-properties/#multi-dimensional-animated-property)

    **Required**

An easing value that affects the speed of change as selection values change from
fully included (high) to fully excluded (low)

##### Random

??? Details
    **Property name:** *rn*

    **Property type**: [Text Random types](../../properties/text-random-types)

    **Required**

If active, transformations should be applied randomly to each selected block.
Random seed is not enforced.

##### Transform Properties

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
