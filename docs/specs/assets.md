# Assets

Assets are a collection of source objects needed to fill layer information that
could be shared between multiple layer instances.

They range from preComps, audios, images, font binaries.

These objects do not always have a specified type, when they don’t, content can
be inferred from the requester.

## Precomps

Precomps have three properties: id, name and layers.

### Precomp id

??? Details
    **Property name:** *id*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The id of the precomp source

### Precomp name

??? Details
    **Property name:** *nm*

    **Property type:** string

    **Required**

The name of the source of the comp. This property is useful for expressions that
reference another composition.

### Precomp layers

??? Details
    **Property name:** *layers*

    **Property type:** Array{[Layer Types](../properties/layer-types/)}

    **Required**

The list of layers that compose a precomposition

## Images

Images contain the information to obtain an image source. It can be expressed in
different ways: inline or pointing to an external path.

### Asset type

??? Details
    **Property name:** *t*

    **Property type:** [Asset Types](../properties/asset-types)

    **Property value:** 1

    **Optional**

The type of the asset

### Image id

??? Details
    **Property name:** *id*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The id of the image source

### Image mode

??? Details
    **Property name:** *e*

    **Property type:** [Load modes](../properties/load-modes)

    **Required**

The way information is stored to retrieve the asset.

### Image width

??? Details
    **Property name:** *w*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The width of the image source

### Image height

??? Details
    **Property name:** *h*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The height of the image source

### Image path

??? Details
    **Property name:** *u*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The path of the image excluding the file name

### Image name

??? Details
    **Property name:** *p*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

If asset mode is 0, it indicates the file name of the image.

If asset mode is 1, it contains the embedded image encoded as base 64.

## Audio

Audio contains the information to obtain an audio source. It can be expressed in
different ways: inline or pointing to an external path.

### Asset type

??? Details
    **Property name:** *t*

    **Property type:** [Asset Types](../properties/asset-types)

    **Property value:** 2

    **Required**

The type of the asset

### Audio id

??? Details
    **Property name:** *id*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The id of the audio source

### Audio mode

??? Details
    **Property name:** *e*

    **Property type:** [Load modes](../properties/load-modes)

    **Required**

The way information is stored to retrieve the asset.

### Audio path

??? Details
    **Property name:** *u*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The path of the audio excluding the file name

### Audio name

??? Details
    **Property name:** *p*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

If asset mode is 0, it indicates the file name of the audio.

If asset mode is 1, it contains the embedded audio encoded as base 64.
