# Text Data

In order to render text, two different solutions are provided by the format.
Text can be exported as glyphs or as regular text data.

## Glyphs

Glyphs allows Lottie to detach itself from any external font file to render
text. By including every character as a shape described by bezier curves, it can
render text independently from the original font.

### Chars

??? Details
    **Property name:** *chars*

    **Property type:** Array{[Char](#char-object)}

The chars array contains the list of all characters available to be rendered.

#### Char Object

??? Details
    **Property type:** Object

A character is described by a set of properties to identify its font family,
shape and other necessary properties to be rendered.

##### Character

??? Details
    **Property name:** *ch*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The character string. It should be used to map the character requested by a text
layer.

##### Style

??? Details
    **Property name:** *style*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The font style. It should be used to map the character requested by a text
layer.

##### Font Family

??? Details
    **Property name:** *fFamily*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The font family. It should be used to map the character requested by a text
layer.

##### Advance Width

??? Details
    **Property name:** *w*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The distance at which the following character should be drawn, expressed
relative to a font size of 100px.

##### Data

??? Details
    **Property name:** *data*

    **Property type:** object

    **Required**

The data object contains a set of extra properties relative to the character.

###### Shapes

??? Details
    **Property name:** *shapes*

    **Property type:** Array{[Shape](../properties/prop-types/#shape)}

    **Required**

The list of shapes that represent the character

## Fonts

??? Details
    **Property name:** *fonts*

    **Property type:** object

    **Required**

Fonts contain a set of properties related to font information needed to render
text layers.

### Fonts List

??? Details
    **Property name:** *list*

    **Property type:** Array{[Font](#font)}

The list property contains the array of fonts needed to render all text layers.

#### Font

??? Details
    **Property type:** Object

Each font describes the font information needed to render a specific text layer
type.

##### Font Origin

??? Details
    **Property name:** *origin*

    **Property type:** [Font Origin types](../properties/font-origin-types)

    **Required**

The font origin indicates how to interpret the other properties in order to load
the font

##### Font Path

??? Details
    **Property name:** *fPath*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The path that should be used to load the font

##### Font Class

??? Details
    **Property name:** *fClass*

    **Property type:** [String](../properties/prop-types/#string)

    **Optional**

The class that should be assigned to the text element for it to get the font
assigned via a css selector

##### Font Name

??? Details
    **Property name:** *fName*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The name of the font to identify which font should be used to render the text

##### Font Family

??? Details
    **Property name:** *fFamily*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The value of the font family

##### Font Weight

??? Details
    **Property name:** *fWeight*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The value of the font weight

##### Font Style

??? Details
    **Property name:** *fStyle*

    **Property type:** [String](../properties/prop-types/#string)

    **Required**

The value of the font style

##### Ascent

??? Details
    **Property name:** *ascent*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The value of the font ascent expressed relative to a font size of 100px. The
ascent references the yOffset by which to draw the character.
